+++
title = "Arch"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/desktop/install/linux/archlinux/](https://docs.docker.com/desktop/install/linux/archlinux/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install Docker Desktop on Arch-based distributions - 在基于 Arch 的发行版上安装 Docker Desktop

> **Docker Desktop terms**
>
> Commercial use of Docker Desktop in larger enterprises (more than 250 employees OR more than $10 million USD in annual revenue) requires a [paid subscription](https://www.docker.com/pricing/).
>
> ​	在大型企业中商业使用 Docker Desktop（超过 250 名员工或年收入超过 1000 万美元）需要[付费订阅](https://www.docker.com/pricing/)。

This page contains information on how to install, launch and upgrade Docker Desktop on an Arch-based distribution.

​	本页包含有关如何在基于 Arch 的发行版上安装、启动和升级 Docker Desktop 的信息。

> **Important**
>
> 
>
> This is an experimental installation package. Docker has not tested or verified the installation.
>
> ​	这是一个实验性安装包。Docker 尚未对其进行测试或验证。

## 前置条件 Prerequisites

To install Docker Desktop successfully, you must meet the [general system requirements]({{< ref "/manuals/DockerDesktop/Install/Linux#general-system-requirements">}}).

​	要成功安装 Docker Desktop，您必须满足[一般系统要求]({{< ref "/manuals/DockerDesktop/Install/Linux#general-system-requirements">}})。

## Install Docker Desktop

1. [Install the Docker client binary on Linux](https://docs.docker.com/engine/install/binaries/#install-daemon-and-client-binaries-on-linux). Static binaries for the Docker client are available for Linux as `docker`. You can use:

   [在 Linux 上安装 Docker 客户端二进制文件](https://docs.docker.com/engine/install/binaries/#install-daemon-and-client-binaries-on-linux)。Docker 客户端的静态二进制文件在 Linux 上作为 `docker` 提供。您可以使用：

   ```console
   $ wget https://download.docker.com/linux/static/stable/x86_64/docker-27.2.1.tgz -qO- | tar xvfz - docker/docker --strip-components=1
   $ mv ./docker /usr/local/bin
   ```

2. Download the latest Arch package from the [Release notes]({{< ref "/manuals/DockerDesktop/Releasenotes" >}}). 从[发行说明]({{< ref "/manuals/DockerDesktop/Releasenotes" >}})中下载最新的 Arch 包。

3. Install the package:

   安装包：

   ```console
   $ sudo pacman -U ./docker-desktop-x86_64.pkg.tar.zst
   ```

   By default, Docker Desktop is installed at `/opt/docker-desktop`.
   
   ​	默认情况下，Docker Desktop 安装在 `/opt/docker-desktop`。

## 启动 Docker Desktop - Launch Docker Desktop

To start Docker Desktop for Linux:

​	要启动 Linux 上的 Docker Desktop：

1. Open your **Applications** menu in Gnome/KDE Desktop and search for **Docker Desktop**. 在 Gnome/KDE 桌面的**应用程序**菜单中，搜索 **Docker Desktop**。

2. Select **Docker Desktop** to start Docker. 选择 **Docker Desktop** 启动 Docker。

   The Docker Subscription Service Agreement displays.

   ​	Docker 订阅服务协议会显示。

3. Select **Accept** to continue. Docker Desktop starts after you accept the terms. 选择 **接受** 继续。接受条款后，Docker Desktop 启动。

   Note that Docker Desktop won't run if you do not agree to the terms. You can choose to accept the terms at a later date by opening Docker Desktop.

   ​	请注意，如果您不同意条款，Docker Desktop 将无法运行。您可以选择稍后打开 Docker Desktop 接受条款。
   
   For more information, see [Docker Desktop Subscription Service Agreement](https://www.docker.com/legal/docker-subscription-service-agreement). It is recommended that you also read the [FAQs](https://www.docker.com/pricing/faq).
   
   ​	了解更多信息，请参阅 [Docker Desktop 订阅服务协议](https://www.docker.com/legal/docker-subscription-service-agreement)。建议您也阅读[常见问题](https://www.docker.com/pricing/faq)。

Alternatively, open a terminal and run:

​	或者，打开终端并运行：

```console
$ systemctl --user start docker-desktop
```

When Docker Desktop starts, it creates a dedicated [context](https://docs.docker.com/engine/context/working-with-contexts) that the Docker CLI can use as a target and sets it as the current context in use. This is to avoid a clash with a local Docker Engine that may be running on the Linux host and using the default context. On shutdown, Docker Desktop resets the current context to the previous one.

​	当 Docker Desktop 启动时，它会创建一个专用的 [上下文](https://docs.docker.com/engine/context/working-with-contexts)，Docker CLI 可以将其用作目标，并将其设置为当前使用的上下文。这样可以避免与可能在 Linux 主机上运行的本地 Docker 引擎发生冲突，后者使用默认上下文。在关闭时，Docker Desktop 会将当前上下文重置为上一个上下文。

The Docker Desktop installer updates Docker Compose and the Docker CLI binaries on the host. It installs Docker Compose V2 and gives users the choice to link it as docker-compose from the Settings panel. Docker Desktop installs the new Docker CLI binary that includes cloud-integration capabilities in `/usr/local/bin/com.docker.cli` and creates a symlink to the classic Docker CLI at `/usr/local/bin`.

​	Docker Desktop 安装程序会在主机上更新 Docker Compose 和 Docker CLI 二进制文件。它安装了 Docker Compose V2，并允许用户在设置面板中选择将其链接为 docker-compose。Docker Desktop 安装的新 Docker CLI 二进制文件（包括云集成功能）位于 `/usr/local/bin/com.docker.cli`，并在 `/usr/local/bin` 创建指向经典 Docker CLI 的符号链接。

After you’ve successfully installed Docker Desktop, you can check the versions of these binaries by running the following commands:

​	成功安装 Docker Desktop 后，可以通过运行以下命令检查这些二进制文件的版本：

```console
$ docker compose version
Docker Compose version v2.29.1

$ docker --version
Docker version 27.1.1, build 6312585

$ docker version
Client: 
 Version:           23.0.5
 API version:       1.42
 Go version:        go1.21.12
<...>
```

To enable Docker Desktop to start on sign in, from the Docker menu, select **Settings** > **General** > **Start Docker Desktop when you sign in to your computer**.

​	要启用 Docker Desktop 在登录时启动，请在 Docker 菜单中选择 **设置** > **常规** > **登录到计算机时启动 Docker Desktop**。

Alternatively, open a terminal and run:

​	或者，打开终端并运行：

```console
$ systemctl --user enable docker-desktop
```

To stop Docker Desktop, select the Docker menu icon to open the Docker menu and select **Quit Docker Desktop**.

​	要停止 Docker Desktop，选择 Docker 菜单图标以打开 Docker 菜单并选择 **退出 Docker Desktop**。

Alternatively, open a terminal and run:

​	或者，打开终端并运行：

```console
$ systemctl --user stop docker-desktop
```

## Next steps

- Explore [Docker's core subscriptions](https://www.docker.com/pricing/) to see what Docker can offer you.
  - 探索 [Docker 的核心订阅服务](https://www.docker.com/pricing/)，了解 Docker 能为您提供的服务。
- Take a look at the [Docker workshop]({{< ref "/get-started/Dockerworkshop" >}}) to learn how to build an image and run it as a containerized application.
  - 查看 [Docker workshop]({{< ref "/get-started/Dockerworkshop" >}}) 学习如何构建镜像并将其作为容器化应用程序运行。
- [Explore Docker Desktop]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop" >}}) and all its features.
  - [探索 Docker Desktop]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop" >}}) 及其所有功能。
- [Troubleshooting]({{< ref "/manuals/DockerDesktop/Troubleshootanddiagnose" >}}) describes common problems, workarounds, how to run and submit diagnostics, and submit issues.
  - [故障排查]({{< ref "/manuals/DockerDesktop/Troubleshootanddiagnose" >}}) 描述了常见问题、解决方法、如何运行和提交诊断以及提交问题。
- [FAQs]({{< ref "/manuals/DockerDesktop/FAQs/General" >}}) provide answers to frequently asked questions.
  - [常见问题]({{< ref "/manuals/DockerDesktop/FAQs/General" >}}) 提供了常见问题的解答。
- [Release notes]({{< ref "/manuals/DockerDesktop/Releasenotes" >}}) lists component updates, new features, and improvements associated with Docker Desktop releases.
  - [发行说明]({{< ref "/manuals/DockerDesktop/Releasenotes" >}}) 列出了与 Docker Desktop 发行相关的组件更新、新功能和改进。
- [Back up and restore data]({{< ref "/manuals/DockerDesktop/HowtobackupandrestoreyourDockerDesktopdata" >}}) provides instructions on backing up and restoring data related to Docker.
  - [备份和恢复数据]({{< ref "/manuals/DockerDesktop/HowtobackupandrestoreyourDockerDesktopdata" >}}) 提供了有关备份和恢复 Docker 相关数据的说明。
