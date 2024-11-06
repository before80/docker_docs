+++
title = "在 Ubuntu 上安装 Docker Engine"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install Docker Engine on Ubuntu - 在 Ubuntu 上安装 Docker Engine

To get started with Docker Engine on Ubuntu, make sure you [meet the prerequisites](https://docs.docker.com/engine/install/ubuntu/#prerequisites), and then follow the [installation steps](https://docs.docker.com/engine/install/ubuntu/#installation-methods).

​	要在 Ubuntu 上开始使用 Docker 引擎，请确保您[满足前提条件](https://docs.docker.com/engine/install/ubuntu/#prerequisites)，然后按照[安装步骤](https://docs.docker.com/engine/install/ubuntu/#installation-methods)进行操作。

## 前置条件 Prerequisites

### 防火墙限制 Firewall limitations

> **Warning**
>
> 
>
> Before you install Docker, make sure you consider the following security implications and firewall incompatibilities.
>
> ​	在安装 Docker 之前，请确保您了解以下安全影响和防火墙不兼容性。

- If you use ufw or firewalld to manage firewall settings, be aware that when you expose container ports using Docker, these ports bypass your firewall rules. For more information, refer to [Docker and ufw]({{< ref "/manuals/DockerEngine/Networking/Packetfilteringandfirewalls#docker-and-ufw">}}).
  - 如果您使用 ufw 或 firewalld 来管理防火墙设置，请注意，当您使用 Docker 暴露容器端口时，这些端口会绕过您的防火墙规则。有关更多信息，请参阅 [Docker 和 ufw]({{< ref "/manuals/DockerEngine/Networking/Packetfilteringandfirewalls#docker-and-ufw">}})。

- Docker is only compatible with `iptables-nft` and `iptables-legacy`. Firewall rules created with `nft` are not supported on a system with Docker installed. Make sure that any firewall rulesets you use are created with `iptables` or `ip6tables`, and that you add them to the `DOCKER-USER` chain, see [Packet filtering and firewalls]({{< ref "/manuals/DockerEngine/Networking/Packetfilteringandfirewalls" >}}).
  - Docker 仅兼容 `iptables-nft` 和 `iptables-legacy`。系统中使用 Docker 时，不支持 `nft` 创建的防火墙规则。请确保您使用的任何防火墙规则集是通过 `iptables` 或 `ip6tables` 创建的，并将它们添加到 `DOCKER-USER` 链中，详见 [数据包过滤和防火墙]({{< ref "/manuals/DockerEngine/Networking/Packetfilteringandfirewalls" >}})。

### 操作系统要求 OS requirements

To install Docker Engine, you need the 64-bit version of one of these Ubuntu versions:

​	要安装 Docker 引擎，您需要以下 Ubuntu 版本的 64 位版本：

- Ubuntu Noble 24.04 (LTS)
- Ubuntu Jammy 22.04 (LTS)
- Ubuntu Focal 20.04 (LTS)

Docker Engine for Ubuntu is compatible with x86_64 (or amd64), armhf, arm64, s390x, and ppc64le (ppc64el) architectures.

​	Docker 引擎适用于 x86_64 (或 amd64)、armhf、arm64、s390x 和 ppc64le (ppc64el) 架构。

### 卸载旧版本 Uninstall old versions

Before you can install Docker Engine, you need to uninstall any conflicting packages.

​	在安装 Docker 引擎之前，您需要卸载任何冲突的包。

Distro maintainers provide unofficial distributions of Docker packages in APT. You must uninstall these packages before you can install the official version of Docker Engine.

​	发行版维护者在 APT 中提供了非官方的 Docker 包，您必须先卸载这些包，才能安装 Docker 引擎的官方版本。

The unofficial packages to uninstall are:

​	需要卸载的非官方包有：

- `docker.io`
- `docker-compose`
- `docker-compose-v2`
- `docker-doc`
- `podman-docker`

Moreover, Docker Engine depends on `containerd` and `runc`. Docker Engine bundles these dependencies as one bundle: `containerd.io`. If you have installed the `containerd` or `runc` previously, uninstall them to avoid conflicts with the versions bundled with Docker Engine.

​	此外，Docker 引擎依赖于 `containerd` 和 `runc`。Docker 引擎将这些依赖项捆绑为一个包：`containerd.io`。如果您之前安装了 `containerd` 或 `runc`，请卸载它们以避免与 Docker 引擎捆绑的版本冲突。

Run the following command to uninstall all conflicting packages:

​	运行以下命令卸载所有冲突的包：



```console
$ for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

`apt-get` might report that you have none of these packages installed.

​	`apt-get` 可能会报告您没有安装这些包。

Images, containers, volumes, and networks stored in `/var/lib/docker/` aren't automatically removed when you uninstall Docker. If you want to start with a clean installation, and prefer to clean up any existing data, read the [uninstall Docker Engine](https://docs.docker.com/engine/install/ubuntu/#uninstall-docker-engine) section.

​	存储在 `/var/lib/docker/` 中的镜像、容器、卷和网络在卸载 Docker 时不会自动删除。如果您想从头开始安装并清除所有现有数据，请阅读 [卸载 Docker 引擎](https://docs.docker.com/engine/install/ubuntu/#uninstall-docker-engine)部分。

## 安装方法 Installation methods

You can install Docker Engine in different ways, depending on your needs:

​	您可以根据需要通过不同方式安装 Docker 引擎：

- Docker Engine comes bundled with [Docker Desktop for Linux]({{< ref "/manuals/DockerDesktop/Install/Linux" >}}). This is the easiest and quickest way to get started.
  - Docker 引擎随 [Linux 的 Docker 桌面版]({{< ref "/manuals/DockerDesktop/Install/Linux" >}}) 一起捆绑。这是最简单、最快速的开始方法。

- Set up and install Docker Engine from [Docker's `apt` repository](#使用-apt-仓库安装-install-using-the-apt-repository).
  - 从 [Docker 的 `apt` 仓库](#使用-apt-仓库安装-install-using-the-apt-repository)设置并安装 Docker 引擎。

- [Install it manually](#从包安装-install-from-a-package) and manage upgrades manually.
  - [手动安装](#从包安装-install-from-a-package)，并手动管理升级。

- Use a [convenience script](#使用便捷脚本安装-install-using-the-convenience-script). Only recommended for testing and development environments.
  - 使用[便捷脚本](#使用便捷脚本安装-install-using-the-convenience-script)。仅推荐用于测试和开发环境。


### 使用 `apt` 仓库安装 Install using the `apt` repository

Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

​	在新的主机上首次安装 Docker 引擎之前，您需要设置 Docker 仓库。之后，您可以从仓库安装和更新 Docker。

1. Set up Docker's `apt` repository. 设置 Docker 的 `apt` 仓库。

   

   ```bash
   # Add Docker's official GPG key:
   sudo apt-get update
   sudo apt-get install ca-certificates curl
   sudo install -m 0755 -d /etc/apt/keyrings
   sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
   sudo chmod a+r /etc/apt/keyrings/docker.asc
   
   # Add the repository to Apt sources:
   echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
     $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
     sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   sudo apt-get update
   ```

   > **Note**
   >
   > 
   >
   > If you use an Ubuntu derivative distro, such as Linux Mint, you may need to use `UBUNTU_CODENAME` instead of `VERSION_CODENAME`.
   >
   > ​	如果您使用 Ubuntu 派生发行版，如 Linux Mint，可能需要使用 `UBUNTU_CODENAME` 而不是 `VERSION_CODENAME`。

2. Install the Docker packages. 安装 Docker 包。

   {{< tabpane text=true persist=disabled >}}

   {{% tab header="Latest" %}}

   To install the latest version, run:

   ​	要安装最新版本，运行以下命令：

   ```console
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

   {{% /tab  %}}

   {{% tab header="Specific version" %}}

   To install a specific version of Docker Engine, start by listing the available versions in the repository:

   ​	要安装特定版本的 Docker 引擎，首先在仓库中列出可用版本：

   ```console
   # List the available versions:
   $ apt-cache madison docker-ce | awk '{ print $3 }'
   
   5:27.1.1-1~ubuntu.24.04~noble
   5:27.1.0-1~ubuntu.24.04~noble
   ...
   ```

   Select the desired version and install:

   ​	选择所需版本并安装：

   ```console
   $ VERSION_STRING=5:27.1.1-1~ubuntu.24.04~noble
   $ sudo apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin
   ```

   {{% /tab  %}}

   {{< /tabpane >}}

   

   ------

3. Verify that the Docker Engine installation is successful by running the `hello-world` image.

   ​	通过运行 `hello-world` 镜像验证 Docker 引擎安装是否成功。

   ```console
   $ sudo docker run hello-world
   ```

   This command downloads a test image and runs it in a container. When the container runs, it prints a confirmation message and exits.
   
   ​	此命令下载一个测试镜像并在容器中运行。容器运行时会输出确认消息并退出。

You have now successfully installed and started Docker Engine.

​	您已成功安装并启动了 Docker 引擎。

> **Tip**
>
> 
>
> Receiving errors when trying to run without root?
>
> ​	尝试在非 root 下运行时遇到错误？
>
> The `docker` user group exists but contains no users, which is why you’re required to use `sudo` to run Docker commands. Continue to [Linux postinstall]({{< ref "/manuals/DockerEngine/Install/Post-installationsteps" >}}) to allow non-privileged users to run Docker commands and for other optional configuration steps.
>
> ​	`docker` 用户组存在但没有包含任何用户，因此需要使用 `sudo` 来运行 Docker 命令。继续参阅 [Linux 后续步骤]({{< ref "/manuals/DockerEngine/Install/Post-installationsteps" >}}) 以允许非特权用户运行 Docker 命令和其他可选配置步骤。

#### 使用阿里云镜像（个人添加）

```sh
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu \
$(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

​	再接着上面的第二个步骤进行安装。

​	最后执行：`sudo systemctl start docker` 来手动启动Docker 守护进程服务。

​	若需要开启启动，请参照：[使用 systemd 配置 Docker 开机启动]({{< ref "/manuals/DockerEngine/Install/Post-installationsteps#使用-systemd-配置-docker-开机启动-configure-docker-to-start-on-boot-with-systemd">}})。	

​	接着就是查看`docker`这个用户组，是否存在：

```sh
sudo getent group docker 
```

​	以及查看当前用户是否在`docker`这个用户组中：

```
groups $USER
```

​	若不在`docker`这个用户组中，可以通过以下命令进行添加：

```sh
sudo usermod -aG docker $USER
```



#### 升级 Docker 引擎 Upgrade Docker Engine

To upgrade Docker Engine, follow step 2 of the [installation instructions](#安装方法-installation-methods), choosing the new version you want to install.

​	要升级 Docker 引擎，请按照[安装说明](#安装方法-installation-methods)的第 2 步，选择要安装的新版本。



### 从包安装 Install from a package

If you can't use Docker's `apt` repository to install Docker Engine, you can download the `deb` file for your release and install it manually. You need to download a new file each time you want to upgrade Docker Engine.

​	如果您无法使用 Docker 的 `apt` 仓库安装 Docker 引擎，可以下载与您的发行版匹配的 `.deb` 文件并手动安装。每次升级 Docker 引擎时都需要下载新文件。

1. Go to [`https://download.docker.com/linux/ubuntu/dists/`](https://download.docker.com/linux/ubuntu/dists/).前往 [`https://download.docker.com/linux/ubuntu/dists/`](https://download.docker.com/linux/ubuntu/dists/)。

2. Select your Ubuntu version in the list. 在列表中选择您的 Ubuntu 版本。

3. Go to `pool/stable/` and select the applicable architecture (`amd64`, `armhf`, `arm64`, or `s390x`). 进入 `pool/stable/` 并选择适用的架构（`amd64`、`armhf`、`arm64` 或 `s390x`）。

4. Download the following `deb` files for the Docker Engine, CLI, containerd, and Docker Compose packages: 下载以下 Docker 引擎、CLI、containerd 和 Docker Compose 包的 `.deb` 文件：

   - `containerd.io_<version>_<arch>.deb`
   - `docker-ce_<version>_<arch>.deb`
   - `docker-ce-cli_<version>_<arch>.deb`
   - `docker-buildx-plugin_<version>_<arch>.deb`
   - `docker-compose-plugin_<version>_<arch>.deb`

5. Install the `.deb` packages. Update the paths in the following example to where you downloaded the Docker packages. 安装 `.deb` 包。将以下示例中的路径更新为下载 Docker 包的位置。

   

   ```console
   $ sudo dpkg -i ./containerd.io_<version>_<arch>.deb \
     ./docker-ce_<version>_<arch>.deb \
     ./docker-ce-cli_<version>_<arch>.deb \
     ./docker-buildx-plugin_<version>_<arch>.deb \
     ./docker-compose-plugin_<version>_<arch>.deb
   ```

   The Docker daemon starts automatically.

   ​	Docker 守护进程会自动启动。

6. Verify that the Docker Engine installation is successful by running the `hello-world` image. 通过运行 `hello-world` 镜像验证 Docker 引擎安装是否成功。

   

   ```console
   $ sudo service docker start
   $ sudo docker run hello-world
   ```

   This command downloads a test image and runs it in a container. When the container runs, it prints a confirmation message and exits.
   
   ​	此命令下载一个测试镜像并在容器中运行。容器运行时会输出确认消息并退出。

You have now successfully installed and started Docker Engine.

​	您已成功安装并启动了 Docker 引擎。

> **Tip**
>
> 
>
> Receiving errors when trying to run without root?
>
> ​	尝试在非 root 下运行时遇到错误？
>
> The `docker` user group exists but contains no users, which is why you’re required to use `sudo` to run Docker commands. Continue to [Linux postinstall]({{< ref "/manuals/DockerEngine/Install/Post-installationsteps" >}}) to allow non-privileged users to run Docker commands and for other optional configuration steps.
>
> ​	`docker` 用户组存在但没有包含任何用户，因此需要使用 `sudo` 来运行 Docker 命令。继续参阅 [Linux 后续步骤]({{< ref "/manuals/DockerEngine/Install/Post-installationsteps" >}}) 以允许非特权用户运行 Docker 命令和其他可选配置步骤。

#### 升级 Docker 引擎 Upgrade Docker Engine

To upgrade Docker Engine, download the newer package files and repeat the [installation procedure](#从包安装-install-from-a-package), pointing to the new files.

​	要升级 Docker 引擎，请下载更新的包文件并重复[安装过程](#从包安装-install-from-a-package)，指向新文件。

### 使用便捷脚本安装 Install using the convenience script

Docker provides a convenience script at https://get.docker.com/ to install Docker into development environments non-interactively. The convenience script isn't recommended for production environments, but it's useful for creating a provisioning script tailored to your needs. Also refer to the [install using the repository](#使用-apt-仓库安装-install-using-the-apt-repository) steps to learn about installation steps to install using the package repository. The source code for the script is open source, and you can find it in the [`docker-install` repository on GitHub](https://github.com/docker/docker-install).

​	Docker 提供了一个便捷脚本 https://get.docker.com/ 来非交互式地将 Docker 安装到开发环境中。便捷脚本不推荐用于生产环境，但在创建适合您需求的配置脚本时很有用。另请参阅 [使用仓库安装](#使用-apt-仓库安装-install-using-the-apt-repository)的步骤以了解如何通过包仓库安装。该脚本的源代码是开源的，可在 [GitHub 的 `docker-install` 仓库](https://github.com/docker/docker-install) 中找到。

Always examine scripts downloaded from the internet before running them locally. Before installing, make yourself familiar with potential risks and limitations of the convenience script:

​	在本地运行任何从互联网上下载的脚本之前，请始终仔细检查。安装前，请熟悉便捷脚本的潜在风险和限制：

- The script requires `root` or `sudo` privileges to run.
  - 该脚本需要 `root` 或 `sudo` 权限才能运行。

- The script attempts to detect your Linux distribution and version and configure your package management system for you.
  - 该脚本尝试检测您的 Linux 发行版和版本，并自动为您配置包管理系统。

- The script doesn't allow you to customize most installation parameters.
  - 该脚本不允许您自定义大多数安装参数。

- The script installs dependencies and recommendations without asking for confirmation. This may install a large number of packages, depending on the current configuration of your host machine.
  - 该脚本在未询问确认的情况下安装依赖项和推荐项。这可能会安装大量包，具体取决于主机的当前配置。

- By default, the script installs the latest stable release of Docker, containerd, and runc. When using this script to provision a machine, this may result in unexpected major version upgrades of Docker. Always test upgrades in a test environment before deploying to your production systems.
  - 默认情况下，该脚本安装 Docker、containerd 和 runc 的最新稳定版本。使用该脚本配置机器时，这可能导致 Docker 的意外主要版本升级。请在生产系统部署前在测试环境中测试升级。

- The script isn't designed to upgrade an existing Docker installation. When using the script to update an existing installation, dependencies may not be updated to the expected version, resulting in outdated versions.
  - 该脚本不适用于升级现有的 Docker 安装。使用该脚本更新现有安装时，依赖项可能不会更新到预期版本，导致版本过旧。


> **Tip: preview script steps before running 提示：在运行之前预览脚本步骤**
>
> You can run the script with the `--dry-run` option to learn what steps the script will run when invoked:
>
> ​	您可以使用 `--dry-run` 选项运行脚本，以了解脚本在调用时将执行的步骤：
>
> ```console
> $ curl -fsSL https://get.docker.com -o get-docker.sh
> $ sudo sh ./get-docker.sh --dry-run
> ```

This example downloads the script from https://get.docker.com/ and runs it to install the latest stable release of Docker on Linux:

​	此示例从 https://get.docker.com/ 下载脚本并运行它，以在 Linux 上安装 Docker 的最新稳定版本：

```console
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
Executing docker install script, commit: 7cae5f8b0decc17d6571f9f52eb840fbc13b2737
<...>
```

You have now successfully installed and started Docker Engine. The `docker` service starts automatically on Debian based distributions. On `RPM` based distributions, such as CentOS, Fedora, RHEL or SLES, you need to start it manually using the appropriate `systemctl` or `service` command. As the message indicates, non-root users can't run Docker commands by default.

​	您已成功安装并启动了 Docker 引擎。`docker` 服务在基于 Debian 的发行版中会自动启动。在基于 `RPM` 的发行版（如 CentOS、Fedora、RHEL 或 SLES）中，您需要使用适当的 `systemctl` 或 `service` 命令手动启动它。提示信息指出，默认情况下，非 root 用户无法运行 Docker 命令。

> **Use Docker as a non-privileged user, or install in rootless mode? 以非特权用户身份使用 Docker，或以无根模式安装？**
>
> The installation script requires `root` or `sudo` privileges to install and use Docker. If you want to grant non-root users access to Docker, refer to the [post-installation steps for Linux](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user). You can also install Docker without `root` privileges, or configured to run in rootless mode. For instructions on running Docker in rootless mode, refer to [run the Docker daemon as a non-root user (rootless mode)]({{< ref "/manuals/DockerEngine/Security/Rootlessmode" >}}).
>
> ​	安装脚本需要 `root` 或 `sudo` 权限才能安装和使用 Docker。如果您希望授予非 root 用户访问 Docker 的权限，请参考 [Linux 的后续步骤](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)。您还可以在没有 `root` 权限的情况下安装 Docker，或配置为无根模式运行。有关以无根模式运行 Docker 守护进程的说明，请参考 [以非 root 用户身份（无根模式）运行 Docker 守护进程]({{< ref "/manuals/DockerEngine/Security/Rootlessmode" >}})。

#### 安装预发布版本 Install pre-releases

Docker also provides a convenience script at https://test.docker.com/ to install pre-releases of Docker on Linux. This script is equal to the script at `get.docker.com`, but configures your package manager to use the test channel of the Docker package repository. The test channel includes both stable and pre-releases (beta versions, release-candidates) of Docker. Use this script to get early access to new releases, and to evaluate them in a testing environment before they're released as stable.

​	Docker 还提供了一个便捷脚本 https://test.docker.com/，用于在 Linux 上安装 Docker 的预发布版本。该脚本与 `get.docker.com` 上的脚本相同，但配置您的包管理器以使用 Docker 包仓库的测试通道。测试通道包括 Docker 的稳定版本和预发布版本（测试版、候选发布）。使用此脚本可以提前访问新版本，并在正式发布为稳定版之前在测试环境中评估它们。

To install the latest version of Docker on Linux from the test channel, run:

​	要从测试通道安装 Linux 上的 Docker 最新版本，请运行：

```console
$ curl -fsSL https://test.docker.com -o test-docker.sh
$ sudo sh test-docker.sh
```

#### 使用便捷脚本后升级 Docker - Upgrade Docker after using the convenience script

If you installed Docker using the convenience script, you should upgrade Docker using your package manager directly. There's no advantage to re-running the convenience script. Re-running it can cause issues if it attempts to re-install repositories which already exist on the host machine.

​	如果您使用便捷脚本安装了 Docker，您应直接使用包管理器升级 Docker。重新运行便捷脚本没有优势。如果脚本尝试重新安装主机上已存在的仓库，可能会引起问题。

## 卸载 Docker 引擎 Uninstall Docker Engine

1. Uninstall the Docker Engine, CLI, containerd, and Docker Compose packages: 卸载 Docker 引擎、CLI、containerd 和 Docker Compose 包：

   

   ```console
   $ sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
   ```

2. Images, containers, volumes, or custom configuration files on your host aren't automatically removed. To delete all images, containers, and volumes: 主机上的镜像、容器、卷或自定义配置文件不会自动删除。要删除所有镜像、容器和卷：

   

   ```console
   $ sudo rm -rf /var/lib/docker
   $ sudo rm -rf /var/lib/containerd
   ```

You have to delete any edited configuration files manually.

​	您需要手动删除任何已编辑的配置文件。

## 接下来 Next steps

- Continue to [Post-installation steps for Linux]({{< ref "/manuals/DockerEngine/Install/Post-installationsteps" >}}). 继续参阅 [Linux 的安装后步骤]({{< ref "/manuals/DockerEngine/Install/Post-installationsteps" >}})。
