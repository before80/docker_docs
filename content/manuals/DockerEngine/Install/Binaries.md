+++
title = "从二进制文件安装 Docker Engine"
date = 2024-10-23T14:54:40+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/install/binaries/](https://docs.docker.com/engine/install/binaries/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install Docker Engine from binaries - 从二进制文件安装 Docker 引擎

> **Important**
>
> 
>
> This page contains information on how to install Docker using binaries. These instructions are mostly suitable for testing purposes. We do not recommend installing Docker using binaries in production environments as they don't have automatic security updates. The Linux binaries described on this page are statically linked, which means that vulnerabilities in build-time dependencies are not automatically patched by security updates of your Linux distribution.
>
> ​	本页面介绍了如何使用二进制文件安装 Docker。此方法主要适用于测试目的。我们不建议在生产环境中使用二进制文件安装 Docker，因为这类安装不具备自动安全更新。本页面描述的 Linux 二进制文件是静态链接的，这意味着构建时依赖的漏洞不会通过 Linux 发行版的安全更新自动修补。
>
> Updating binaries is also slightly more involved when compared to Docker packages installed using a package manager or through Docker Desktop, as it requires (manually) updating the installed version whenever there is a new release of Docker.
>
> ​	与使用包管理器或 Docker Desktop 安装的 Docker 包相比，更新二进制文件稍显复杂，因为每次 Docker 发布新版本时需要手动更新已安装版本。
>
> Also, static binaries may not include all functionalities provided by the dynamic packages.
>
> ​	此外，静态二进制文件可能不包括动态包提供的所有功能。
>
> On Windows and Mac, we recommend that you install [Docker Desktop]({{< ref "/manuals/DockerDesktop" >}}) instead. For Linux, we recommend that you follow the instructions specific for your distribution.
>
> ​	对于 Windows 和 Mac，我们建议您安装 [Docker Desktop]({{< ref "/manuals/DockerDesktop" >}})。对于 Linux，我们建议按照适用于您发行版的说明进行安装。

If you want to try Docker or use it in a testing environment, but you're not on a supported platform, you can try installing from static binaries. If possible, you should use packages built for your operating system, and use your operating system's package management system to manage Docker installation and upgrades.

​	如果您希望尝试 Docker 或在测试环境中使用它，但您的平台不受支持，可以尝试从静态二进制文件安装。如果可能，您应使用为操作系统构建的包，并使用操作系统的包管理系统来管理 Docker 的安装和升级。

Static binaries for the Docker daemon binary are only available for Linux (as `dockerd`) and Windows (as `dockerd.exe`). Static binaries for the Docker client are available for Linux, Windows, and macOS (as `docker`).

​	Docker 守护进程二进制文件的静态版本仅适用于 Linux（称为 `dockerd`）和 Windows（称为 `dockerd.exe`）。Docker 客户端的静态二进制文件适用于 Linux、Windows 和 macOS（称为 `docker`）。

This topic discusses binary installation for Linux, Windows, and macOS:

​	本主题讨论 Linux、Windows 和 macOS 的二进制安装：

- [Install daemon and client binaries on Linux 在 Linux 上安装守护进程和客户端二进制文件](https://docs.docker.com/engine/install/binaries/#install-daemon-and-client-binaries-on-linux)
- [Install client binaries on macOS 在 macOS 上安装客户端二进制文件](https://docs.docker.com/engine/install/binaries/#install-client-binaries-on-macos)
- [Install server and client binaries on Windows 在 Windows 上安装服务器和客户端二进制文件](https://docs.docker.com/engine/install/binaries/#install-server-and-client-binaries-on-windows)

## 在 Linux 上安装守护进程和客户端二进制文件 Install daemon and client binaries on Linux

### 前置条件 Prerequisites

Before attempting to install Docker from binaries, be sure your host machine meets the prerequisites:

​	在尝试从二进制文件安装 Docker 之前，请确保您的主机满足以下前提条件：

- A 64-bit installation
  - 64 位操作系统

- Version 3.10 or higher of the Linux kernel. The latest version of the kernel available for your platform is recommended.

  - Linux 内核版本 3.10 或更高版本。建议使用平台支持的最新内核版本。

- `iptables` version 1.4 or higher

  - `iptables` 版本 1.4 或更高

- `git` version 1.7 or higher

  - `git` 版本 1.7 或更高

- A `ps` executable, usually provided by `procps` or a similar package.

  - `ps` 可执行文件，通常由 `procps` 或类似包提供

- [XZ Utils](https://tukaani.org/xz/) 4.9 or higher

  - [XZ Utils](https://tukaani.org/xz/) 4.9 或更高版本

- A [properly mounted](https://github.com/tianon/cgroupfs-mount/blob/master/cgroupfs-mount) `cgroupfs` hierarchy; a single, all-encompassing `cgroup` mount point is not sufficient. See Github issues [#2683](https://github.com/moby/moby/issues/2683), [#3485](https://github.com/moby/moby/issues/3485), [#4568](https://github.com/moby/moby/issues/4568)).

  - 正确挂载的 [`cgroupfs` 层级结构](https://github.com/tianon/cgroupfs-mount/blob/master/cgroupfs-mount)；一个单一的 `cgroup` 挂载点是不够的。参见 GitHub 问题 [#2683](https://github.com/moby/moby/issues/2683)、[#3485](https://github.com/moby/moby/issues/3485)、[#4568](https://github.com/moby/moby/issues/4568)。

  

#### 尽可能保护您的环境 Secure your environment as much as possible

##### 操作系统注意事项 OS considerations

Enable SELinux or AppArmor if possible.

​	尽可能启用 SELinux 或 AppArmor。

It is recommended to use AppArmor or SELinux if your Linux distribution supports either of the two. This helps improve security and blocks certain types of exploits. Review the documentation for your Linux distribution for instructions for enabling and configuring AppArmor or SELinux.

​	建议在您的 Linux 发行版支持的情况下使用 AppArmor 或 SELinux。这有助于提高安全性并阻止某些类型的漏洞攻击。请参阅 Linux 发行版的文档，了解启用和配置 AppArmor 或 SELinux 的说明。

> **Security warning**
>
> If either of the security mechanisms is enabled, do not disable it as a work-around to make Docker or its containers run. Instead, configure it correctly to fix any problems.
>
> ​	如果启用了其中的一个安全机制，请不要将其禁用来解决 Docker 或其容器的运行问题。相反，请正确配置它以解决问题。

##### Docker 守护进程注意事项 Docker daemon considerations

- Enable `seccomp` security profiles if possible. See [Enabling `seccomp` for Docker]({{< ref "/manuals/DockerEngine/Security/SeccompsecurityprofilesforDocker" >}}). 尽可能启用 `seccomp` 安全配置文件。请参阅 [为 Docker 启用 `seccomp`]({{< ref "/manuals/DockerEngine/Security/SeccompsecurityprofilesforDocker" >}})。 
- Enable user namespaces if possible. See the [Daemon user namespace options](https://docs.docker.com/reference/cli/dockerd/#daemon-user-namespace-options). 尽可能启用用户命名空间。请参阅 [守护进程用户命名空间选项](https://docs.docker.com/reference/cli/dockerd/#daemon-user-namespace-options)。

### 安装静态二进制文件 Install static binaries

1. Download the static binary archive. Go to https://download.docker.com/linux/static/stable/, choose your hardware platform, and download the `.tgz` file relating to the version of Docker Engine you want to install.  下载静态二进制文件压缩包。访问 https://download.docker.com/linux/static/stable/，选择您的硬件平台，并下载与要安装的 Docker 版本对应的 `.tgz` 文件。

2. Extract the archive using the `tar` utility. The `dockerd` and `docker` binaries are extracted. 使用 `tar` 工具提取文件，`dockerd` 和 `docker` 二进制文件会被提取出来。

   

   ```console
   $ tar xzvf /path/to/FILE.tar.gz
   ```

3. **Optional**: Move the binaries to a directory on your executable path, such as `/usr/bin/`. If you skip this step, you must provide the path to the executable when you invoke `docker` or `dockerd` commands. **可选**：将二进制文件移动到您的可执行文件路径上的目录，例如 `/usr/bin/`。如果跳过此步骤，运行 `docker` 或 `dockerd` 命令时需要提供二进制文件的路径。

   

   ```console
   $ sudo cp docker/* /usr/bin/
   ```

4. Start the Docker daemon: 启动 Docker 守护进程：

   

   ```console
   $ sudo dockerd &
   ```

   If you need to start the daemon with additional options, modify the above command accordingly or create and edit the file `/etc/docker/daemon.json` to add the custom configuration options.

   ​	如果需要以其他选项启动守护进程，请相应修改上述命令，或创建并编辑文件 `/etc/docker/daemon.json` 以添加自定义配置选项。

5. Verify that Docker is installed correctly by running the `hello-world` image. 通过运行 `hello-world` 镜像验证 Docker 是否正确安装。

   

   ```console
   $ sudo docker run hello-world
   ```

   This command downloads a test image and runs it in a container. When the container runs, it prints a message and exits.
   
   ​	此命令下载一个测试镜像并在容器中运行。容器运行时会打印一条消息并退出。

You have now successfully installed and started Docker Engine.

​	您已成功安装并启动了 Docker 引擎。

> **Tip**
>
> 
>
> Receiving errors when trying to run without root?
>
> ​	遇到运行时需要 `root` 权限的问题？
>
> The `docker` user group exists but contains no users, which is why you’re required to use `sudo` to run Docker commands. Continue to [Linux postinstall]({{< ref "/manuals/DockerEngine/Install/Post-installationsteps" >}}) to allow non-privileged users to run Docker commands and for other optional configuration steps.
>
> ​	`docker` 用户组存在但没有用户，因此需要使用 `sudo` 来运行 Docker 命令。继续参阅 [Linux 后续步骤]({{< ref "/manuals/DockerEngine/Install/Post-installationsteps" >}}) 以允许非特权用户运行 Docker 命令和其他可选配置步骤。

## 在 macOS 上安装客户端二进制文件 Install client binaries on macOS

> **Note**
>
> 
>
> The following instructions are mostly suitable for testing purposes. The macOS binary includes the Docker client only. It does not include the `dockerd` daemon which is required to run containers. Therefore, we recommend that you install [Docker Desktop]({{< ref "/manuals/DockerDesktop" >}}) instead.
>
> ​	以下说明主要适用于测试目的。macOS 二进制文件仅包含 Docker 客户端，不包含运行容器所需的 `dockerd` 守护进程。因此，我们建议您安装 [Docker Desktop]({{< ref "/manuals/DockerDesktop" >}})。

The binaries for Mac also do not contain:

​	Mac 的二进制文件还不包含：

- A runtime environment. You must set up a functional engine either in a Virtual Machine, or on a remote Linux machine.
  - 运行时环境。您必须在虚拟机或远程 Linux 机器上设置一个功能齐全的引擎。

- Docker components such as `buildx` and `docker compose`.
  - Docker 组件，例如 `buildx` 和 `docker compose`。


To install client binaries, perform the following steps:

​	要安装客户端二进制文件，请执行以下步骤：

1. Download the static binary archive. Go to https://download.docker.com/mac/static/stable/ and select `x86_64` (for Mac on Intel chip) or `aarch64` (for Mac on Apple silicon), and then download the `.tgz` file relating to the version of Docker Engine you want to install. 下载静态二进制文件压缩包。访问 https://download.docker.com/mac/static/stable/，选择 `x86_64`（适用于 Intel 芯片的 Mac）或 `aarch64`（适用于 Apple Silicon 的 Mac），然后下载与要安装的 Docker 版本对应的 `.tgz` 文件。

2. Extract the archive using the `tar` utility. The `docker` binary is extracted. 使用 `tar` 工具提取文件，`docker` 二进制文件会被提取出来。

   

   ```console
   $ tar xzvf /path/to/FILE.tar.gz
   ```

3. Clear the extended attributes to allow it run. 清除扩展属性以允许其运行。

   

   ```console
   $ sudo xattr -rc docker
   ```

   Now, when you run the following command, you can see the Docker CLI usage instructions:

   ​	现在，运行以下命令可以查看 Docker CLI 使用说明：

   ```console
   $ docker/docker
   ```

4. **Optional**: Move the binary to a directory on your executable path, such as `/usr/local/bin/`. If you skip this step, you must provide the path to the executable when you invoke `docker` or `dockerd` commands. **可选**：将二进制文件移动到您的可执行文件路径上的目录，例如 `/usr/local/bin/`。如果跳过此步骤，运行 `docker` 或 `dockerd` 命令时需要提供可执行文件的路径。

   

   ```console
   $ sudo cp docker/docker /usr/local/bin/
   ```

5. Verify that Docker is installed correctly by running the `hello-world` image. The value of `<hostname>` is a hostname or IP address running the Docker daemon and accessible to the client. 通过运行 `hello-world` 镜像验证 Docker 是否正确安装。`<hostname>` 的值为运行 Docker 守护进程且客户端可访问的主机名或 IP 地址。

   

   ```console
   $ sudo docker -H <hostname> run hello-world
   ```

   This command downloads a test image and runs it in a container. When the container runs, it prints a message and exits.
   
   ​	此命令下载一个测试镜像并在容器中运行。容器运行时会打印一条消息并退出。

## 在 Windows 上安装服务器和客户端二进制文件 Install server and client binaries on Windows

> **Note**
>
> 
>
> The following section describes how to install the Docker daemon on Windows Server which allows you to run Windows containers only. When you install the Docker daemon on Windows Server, the daemon doesn't contain Docker components such as `buildx` and `compose`. If you're running Windows 10 or 11, we recommend that you install [Docker Desktop]({{< ref "/manuals/DockerDesktop" >}}) instead.
>
> ​	以下部分描述了如何在 Windows Server 上安装 Docker 守护进程，它仅允许运行 Windows 容器。安装 Docker 守护进程时，不包含 `buildx` 和 `compose` 等 Docker 组件。如果您使用的是 Windows 10 或 11，我们建议您安装 [Docker Desktop]({{< ref "/manuals/DockerDesktop" >}})。

Binary packages on Windows include both `dockerd.exe` and `docker.exe`. On Windows, these binaries only provide the ability to run native Windows containers (not Linux containers).

​	Windows 上的二进制包包括 `dockerd.exe` 和 `docker.exe`。在 Windows 上，这些二进制文件仅支持运行本地 Windows 容器（不支持 Linux 容器）。

To install server and client binaries, perform the following steps:

​	要安装服务器和客户端二进制文件，请执行以下步骤：

1. Download the static binary archive. Go to https://download.docker.com/win/static/stable/x86_64 and select the latest version from the list. 下载静态二进制文件压缩包。访问 https://download.docker.com/win/static/stable/x86_64 并选择最新版本。

2. Run the following PowerShell commands to install and extract the archive to your program files: 运行以下 PowerShell 命令安装并解压缩到程序文件目录：

   

   ```powershell
   PS C:\> Expand-Archive /path/to/<FILE>.zip -DestinationPath $Env:ProgramFiles
   ```

3. Register the service and start the Docker Engine: 注册服务并启动 Docker 引擎：

   

   ```powershell
   PS C:\> &$Env:ProgramFiles\Docker\dockerd --register-service
   PS C:\> Start-Service docker
   ```

4. Verify that Docker is installed correctly by running the `hello-world` image. 通过运行 `hello-world` 镜像验证 Docker 是否正确安装。

   

   ```powershell
   PS C:\> &$Env:ProgramFiles\Docker\docker run hello-world:nanoserver
   ```

   This command downloads a test image and runs it in a container. When the container runs, it prints a message and exits.
   
   ​	此命令下载一个测试镜像并在容器中运行。容器运行时会打印一条消息并退出。

## 升级静态二进制文件 Upgrade static binaries

To upgrade your manual installation of Docker Engine, first stop any `dockerd` or `dockerd.exe` processes running locally, then follow the regular installation steps to install the new version on top of the existing version.

​	要升级手动安装的 Docker 引擎，首先停止本地运行的所有 `dockerd` 或 `dockerd.exe` 进程，然后按照常规安装步骤将新版本安装在现有版本之上。

## 接下来 Next steps

- Continue to [Post-installation steps for Linux]({{< ref "/manuals/DockerEngine/Install/Post-installationsteps" >}}).
  - 继续参阅 [在Linux 的后续安装步骤]({{< ref "/manuals/DockerEngine/Install/Post-installationsteps" >}})。
