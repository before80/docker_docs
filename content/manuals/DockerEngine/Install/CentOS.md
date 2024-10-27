+++
title = "在 CentOS 上安装 Docker Engine"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/install/centos/](https://docs.docker.com/engine/install/centos/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install Docker Engine on CentOS - 在 CentOS 上安装 Docker 引擎

To get started with Docker Engine on CentOS, make sure you [meet the prerequisites](https://docs.docker.com/engine/install/centos/#prerequisites), and then follow the [installation steps](https://docs.docker.com/engine/install/centos/#installation-methods).

​	要在 CentOS 上开始使用 Docker 引擎，请确保您[满足前提条件](https://docs.docker.com/engine/install/centos/#prerequisites)，然后按照[安装步骤](https://docs.docker.com/engine/install/centos/#installation-methods)进行操作。

## 前置条件 Prerequisites

### 操作系统要求 OS requirements

To install Docker Engine, you need a maintained version of one of the following CentOS versions:

​	要安装 Docker 引擎，您需要以下 CentOS 版本的受维护版本：

- CentOS 9 (stream)

The `centos-extras` repository must be enabled. This repository is enabled by default. If you have disabled it, you need to re-enable it.

​	必须启用 `centos-extras` 仓库。该仓库默认启用。如果您已禁用它，则需要重新启用。

### 卸载旧版本 Uninstall old versions

Older versions of Docker went by `docker` or `docker-engine`. Uninstall any such older versions before attempting to install a new version, along with associated dependencies.

​	旧版本的 Docker 名为 `docker` 或 `docker-engine`。在尝试安装新版本之前，请卸载任何这些旧版本及相关依赖项。

```console
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

`yum` might report that you have none of these packages installed.

​	`yum` 可能会报告您没有安装这些包。

Images, containers, volumes, and networks stored in `/var/lib/docker/` aren't automatically removed when you uninstall Docker.

​	存储在 `/var/lib/docker/` 中的镜像、容器、卷和网络在卸载 Docker 时不会自动删除。

## 安装方法 Installation methods

You can install Docker Engine in different ways, depending on your needs:

​	您可以根据需要通过不同方式安装 Docker 引擎：

- You can [set up Docker's repositories](https://docs.docker.com/engine/install/centos/#install-using-the-repository) and install from them, for ease of installation and upgrade tasks. This is the recommended approach.
  - 您可以[设置 Docker 的仓库](https://docs.docker.com/engine/install/centos/#install-using-the-repository)并从中安装，以便于安装和升级任务。这是推荐的方法。

- You can download the RPM package, [install it manually](https://docs.docker.com/engine/install/centos/#install-from-a-package), and manage upgrades completely manually. This is useful in situations such as installing Docker on air-gapped systems with no access to the internet.
  - 您可以下载 RPM 包，手动[安装](https://docs.docker.com/engine/install/centos/#install-from-a-package)，并完全手动管理升级。这在没有网络访问的隔离系统上安装 Docker 时特别有用。

- In testing and development environments, you can use automated [convenience scripts](https://docs.docker.com/engine/install/centos/#install-using-the-convenience-script) to install Docker.
  - 在测试和开发环境中，您可以使用自动[便捷脚本](https://docs.docker.com/engine/install/centos/#install-using-the-convenience-script)安装 Docker。

### 使用 RPM 仓库安装 Install using the rpm repository

Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

​	在新的主机上首次安装 Docker 引擎之前，您需要设置 Docker 仓库。之后，您可以从仓库中安装和更新 Docker。

#### 设置仓库 Set up the repository

Install the `yum-utils` package (which provides the `yum-config-manager` utility) and set up the repository.

​	安装 `yum-utils` 包（提供 `yum-config-manager` 工具）并设置仓库。



```console
$ sudo yum install -y yum-utils
$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

#### 安装 Docker 引擎 Install Docker Engine

1. Install Docker Engine, containerd, and Docker Compose: 安装 Docker 引擎、containerd 和 Docker Compose：

   {{< tabpane text=true persist=disabled >}}

   {{% tab header="Latest" %}}

   To install the latest version, run:

   ​	要安装最新版本，运行：

   ```console
   $ sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

   If prompted to accept the GPG key, verify that the fingerprint matches `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35`, and if so, accept it.

   ​	如果提示接受 GPG 密钥，请验证指纹是否为 `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35`，如果匹配，请接受。

   This command installs Docker, but it doesn't start Docker. It also creates a `docker` group, however, it doesn't add any users to the group by default.

   ​	此命令安装 Docker，但不会启动 Docker。它还会创建一个 `docker` 组，但默认情况下不会向该组添加任何用户。

   {{% /tab  %}}

   {{% tab header="Specific version" %}}

   To install a specific version, start by listing the available versions in the repository:

   ​	要安装特定版本，请首先列出仓库中的可用版本：

   ```console
   $ yum list docker-ce --showduplicates | sort -r
   
   docker-ce.x86_64    3:27.1.1-1.el9    docker-ce-stable
   docker-ce.x86_64    3:27.1.0-1.el9    docker-ce-stable
   <...>
   ```

   The list returned depends on which repositories are enabled, and is specific to your version of CentOS (indicated by the `.el9` suffix in this example).

   ​	列表中的版本取决于已启用的仓库，并且特定于您使用的 CentOS 版本（如 `.el9` 后缀所示）。

   Install a specific version by its fully qualified package name, which is the package name (`docker-ce`) plus the version string (2nd column), separated by a hyphen (`-`). For example, `docker-ce-3:27.1.1-1.el9`.

   ​	通过完整的包名安装特定版本，该包名由包名 (`docker-ce`) 和版本字符串（第 2 列）组成，中间用连字符（`-`）分隔。例如，`docker-ce-3:27.1.1-1.el9`。

   Replace `<VERSION_STRING>` with the desired version and then run the following command to install:

   ​	将 `<VERSION_STRING>` 替换为所需版本，然后运行以下命令进行安装：

   ```console
   $ sudo yum install docker-ce-VERSION_STRING docker-ce-cli-VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin
   ```

   This command installs Docker, but it doesn't start Docker. It also creates a `docker` group, however, it doesn't add any users to the group by default.

   ​	该命令会安装 Docker，但不会启动 Docker。它还会创建一个 `docker` 组，但默认情况下不会向该组添加任何用户。

   {{% /tab  %}}

   {{< /tabpane >}}

   

   ------

2. Start Docker. 启动 Docker。

   

   ```console
   $ sudo systemctl start docker
   ```

3. Verify that the Docker Engine installation is successful by running the `hello-world` image. 通过运行 `hello-world` 镜像验证 Docker 引擎是否安装成功。

   

   ```console
   $ sudo docker run hello-world
   ```

   This command downloads a test image and runs it in a container. When the container runs, it prints a confirmation message and exits.
   
   ​	此命令会下载一个测试镜像并在容器中运行。容器运行时会打印一条确认消息并退出。

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

To upgrade Docker Engine, follow the [installation instructions](https://docs.docker.com/engine/install/centos/#install-using-the-repository), choosing the new version you want to install.

​	要升级 Docker 引擎，请按照[安装说明](https://docs.docker.com/engine/install/centos/#install-using-the-repository)，选择要安装的新版本。

### 从包安装 Install from a package

If you can't use Docker's `rpm` repository to install Docker Engine, you can download the `.rpm` file for your release and install it manually. You need to download a new file each time you want to upgrade Docker Engine.

​	如果无法使用 Docker 的 `rpm` 仓库安装 Docker 引擎，您可以下载与您的发行版匹配的 `.rpm` 文件并手动安装。每次升级 Docker 引擎时都需要下载新文件。

1. Go to https://download.docker.com/linux/centos/ and choose your version of CentOS. Then browse to `x86_64/stable/Packages/` and download the `.rpm` file for the Docker version you want to install. 前往 https://download.docker.com/linux/centos/，选择您的 CentOS 版本。然后浏览到 `x86_64/stable/Packages/` 并下载所需版本的 `.rpm` 文件。

2. Install Docker Engine, changing the following path to the path where you downloaded the Docker package. 安装 Docker 引擎，将以下路径更改为您下载 Docker 包的路径。

   

   ```console
   $ sudo yum install /path/to/package.rpm
   ```

   Docker is installed but not started. The `docker` group is created, but no users are added to the group.

   ​	Docker 已安装但未启动。`docker` 组已创建，但没有用户被添加到该组。

3. Start Docker. 启动 Docker。

   

   ```console
   $ sudo systemctl start docker
   ```

4. Verify that the Docker Engine installation is successful by running the `hello-world` image. 通过运行 `hello-world` 镜像验证 Docker 引擎是否安装成功。

   

   ```console
   $ sudo docker run hello-world
   ```

   This command downloads a test image and runs it in a container. When the container runs, it prints a confirmation message and exits.
   
   ​	此命令会下载一个测试镜像并在容器中运行。容器运行时会打印一条确认消息并退出。

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

To upgrade Docker Engine, download the newer package files and repeat the [installation procedure](https://docs.docker.com/engine/install/centos/#install-from-a-package), using `yum -y upgrade` instead of `yum -y install`, and point to the new files.

​	要升级 Docker 引擎，请下载更新的包文件并重复[安装过程](https://docs.docker.com/engine/install/centos/#install-from-a-package)，使用 `yum -y upgrade` 替代 `yum -y install`，并指向新文件。

### 使用便捷脚本安装 Install using the convenience script

Docker provides a convenience script at https://get.docker.com/ to install Docker into development environments non-interactively. The convenience script isn't recommended for production environments, but it's useful for creating a provisioning script tailored to your needs. Also refer to the [install using the repository](https://docs.docker.com/engine/install/centos/#install-using-the-repository) steps to learn about installation steps to install using the package repository. The source code for the script is open source, and you can find it in the [`docker-install` repository on GitHub](https://github.com/docker/docker-install).

​	Docker 提供了一个便捷脚本 https://get.docker.com/ 来非交互式地将 Docker 安装到开发环境中。便捷脚本不推荐用于生产环境，但在创建适合您需求的配置脚本时很有用。另请参阅 [使用仓库安装](https://docs.docker.com/engine/install/centos/#install-using-the-repository)的步骤以了解如何通过包仓库安装。该脚本的源代码是开源的，可在 [GitHub 的 `docker-install` 仓库](https://github.com/docker/docker-install) 中找到。

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


> **Tip: preview script steps before running  提示：在运行之前预览脚本步骤**
>
> You can run the script with the `--dry-run` option to learn what steps the script will run when invoked:
>
> ​	您可以使用 `--dry-run` 选项运行脚本，以了解脚本在调用时将执行的步骤：
>
> 
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

​	您已成功安装并启动了 Docker 引擎。在基于 Debian 的发行版中，`docker` 服务会自动启动。在基于 `RPM` 的发行版（如 CentOS、Fedora、RHEL 或 SLES）中，您需要使用适当的 `systemctl` 或 `service` 命令手动启动它。提示信息指出，默认情况下，非 root 用户无法运行 Docker 命令。

> **Use Docker as a non-privileged user, or install in rootless mode? 以非特权用户身份使用 Docker，或以无根模式安装？**
>
> The installation script requires `root` or `sudo` privileges to install and use Docker. If you want to grant non-root users access to Docker, refer to the [post-installation steps for Linux](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user). You can also install Docker without `root` privileges, or configured to run in rootless mode. For instructions on running Docker in rootless mode, refer to [run the Docker daemon as a non-root user (rootless mode)]({{< ref "/manuals/DockerEngine/Security/Rootlessmode" >}}).
>
> ​	安装脚本需要 `root` 或 `sudo` 权限才能安装和使用 Docker。如果您希望授予非 root 用户访问 Docker 的权限，请参考 [Linux 的后续步骤](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)。您还可以在没有 `root` 权限的情况下安装 Docker，或配置为无根模式运行。有关以无根模式运行 Docker 守护进程的说明，请参考 [以非 root 用户身份（无根模式）运行 Docker 守护进程]({{< ref "/manuals/DockerEngine/Security/Rootlessmode" >}})。

#### 安装预发布版本 Install pre-releases

Docker also provides a convenience script at https://test.docker.com/ to install pre-releases of Docker on Linux. This script is equal to the script at `get.docker.com`, but configures your package manager to use the test channel of the Docker package repository. The test channel includes both stable and pre-releases (beta versions, release-candidates) of Docker. Use this script to get early access to new releases, and to evaluate them in a testing environment before they're released as stable.

​	Docker 还提供了一个便捷脚本 https://test.docker.com/，用于在 Linux 上安装 Docker 的预发布版本。该脚本与 `get.docker.com` 上的脚本相同，但配置您的包管理器以使用 Docker 包仓库的测试通道。测试通道包括 Docker 的稳定版本和预发布版本（测试版、候选发布）。使用此脚本可以提前访问新版本，并在正式发布为稳定版之前在测试环境中评估它们。

To install the latest version of Docker on Linux from the test channel, run:

​	要从测试通道安装 Linux 上的 Docker 最新版本，运行：

```console
$ curl -fsSL https://test.docker.com -o test-docker.sh
$ sudo sh test-docker.sh
```

#### 使用便捷脚本之后升级 Docker - Upgrade Docker after using the convenience script

If you installed Docker using the convenience script, you should upgrade Docker using your package manager directly. There's no advantage to re-running the convenience script. Re-running it can cause issues if it attempts to re-install repositories which already exist on the host machine.

​	如果您使用便捷脚本安装了 Docker，您应直接使用包管理器升级 Docker。重新运行便捷脚本没有优势。如果脚本尝试重新安装主机上已存在的仓库，可能会引起问题。

## 卸载 Docker 引擎 - Uninstall Docker Engine

1. Uninstall the Docker Engine, CLI, containerd, and Docker Compose packages: 卸载 Docker 引擎、CLI、containerd 和 Docker Compose 包：

   

   ```console
   $ sudo yum remove docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
   ```

2. Images, containers, volumes, or custom configuration files on your host aren't automatically removed. To delete all images, containers, and volumes: 主机上的镜像、容器、卷或自定义配置文件不会自动删除。要删除所有镜像、容器和卷：

   

   ```console
   $ sudo rm -rf /var/lib/docker
   $ sudo rm -rf /var/lib/containerd
   ```

You have to delete any edited configuration files manually.

​	您需要手动删除任何已编辑的配置文件。

## 接下来 Next steps

- Continue to [Post-installation steps for Linux]({{< ref "/manuals/DockerEngine/Install/Post-installationsteps" >}}). 
  - 继续参阅 [在Linux 中安装的后续步骤]({{< ref "/manuals/DockerEngine/Install/Post-installationsteps" >}})。
