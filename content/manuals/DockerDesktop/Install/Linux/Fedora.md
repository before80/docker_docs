+++
title = "Fedora"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/desktop/install/linux/fedora/](https://docs.docker.com/desktop/install/linux/fedora/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install Docker Desktop on Fedora - 在 Fedora 上安装 Docker Desktop

> **Docker Desktop terms**
>
> Commercial use of Docker Desktop in larger enterprises (more than 250 employees OR more than $10 million USD in annual revenue) requires a [paid subscription](https://www.docker.com/pricing/).
>
> ​	在大型企业中商业使用 Docker Desktop（超过 250 名员工或年收入超过 1000 万美元）需要[付费订阅](https://www.docker.com/pricing/)。

This page contains information on how to install, launch and upgrade Docker Desktop on a Fedora distribution.

​	本页包含有关如何在 Fedora 发行版上安装、启动和升级 Docker Desktop 的信息。

## 前置条件 Prerequisites

To install Docker Desktop successfully, you must:

​	要成功安装 Docker Desktop，您必须：

- Meet the [general system requirements]({{< ref "/manuals/DockerDesktop/Install/Linux#general-system-requirements">}}).
  - 满足[常规系统要求]({{< ref "/manuals/DockerDesktop/Install/Linux#general-system-requirements">}})。

- Have a 64-bit version of Fedora 39 or Fedora 40.
  - 使用 64 位版本的 Fedora 39 或 Fedora 40。


Additionally, for a GNOME desktop environment you must install AppIndicator and KStatusNotifierItem [GNOME extensions](https://extensions.gnome.org/extension/615/appindicator-support/).

​	此外，对于 GNOME 桌面环境，您必须安装 AppIndicator 和 KStatusNotifierItem [GNOME 扩展](https://extensions.gnome.org/extension/615/appindicator-support/)。

For non-GNOME desktop environments, `gnome-terminal` must be installed:

​	对于非 GNOME 桌面环境，必须安装 `gnome-terminal`：

```console
$ sudo dnf install gnome-terminal
```

## 安装 Docker Desktop - Install Docker Desktop

To install Docker Desktop on Fedora:

​	在 Fedora 上安装 Docker Desktop 的步骤：

1. Set up [Docker's package repository](https://docs.docker.com/engine/install/fedora/#set-up-the-repository). 设置 [Docker 的包仓库](https://docs.docker.com/engine/install/fedora/#set-up-the-repository)。

2. Download the latest [RPM package](https://desktop.docker.com/linux/main/amd64/docker-desktop-x86_64.rpm?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-linux-amd64). For checksums, see the [Release notes]({{< ref "/manuals/DockerDesktop/Releasenotes" >}}). 下载最新的 [RPM 包](https://desktop.docker.com/linux/main/amd64/docker-desktop-x86_64.rpm?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-linux-amd64)。检查文件校验和，请查看[发行说明]({{< ref "/manuals/DockerDesktop/Releasenotes" >}})。

3. Install the package with dnf as follows:

   使用 dnf 安装该包：

   ```console
   $ sudo dnf install ./docker-desktop-<arch>.rpm
   ```

   Don't forget to substitute `<arch>` with the architecture you want.

   ​	请将 `<arch>` 替换为您需要的架构。
   
   By default, Docker Desktop is installed at `/opt/docker-desktop`.
   
   ​	默认情况下，Docker Desktop 安装在 `/opt/docker-desktop`。

There are a few post-install configuration steps done through the post-install script contained in the RPM package.

​	安装后的一些配置步骤会通过 RPM 包中的后安装脚本完成。

The post-install script:

​	后安装脚本将会：

- Sets the capability on the Docker Desktop binary to map privileged ports and set resource limits.
  - 设置 Docker Desktop 二进制文件的特权端口映射能力和资源限制。

- Adds a DNS name for Kubernetes to `/etc/hosts`.
  - 将 Kubernetes 的 DNS 名称添加到 `/etc/hosts`。

- Creates a symlink from `/usr/local/bin/com.docker.cli` to `/usr/bin/docker`. This is because the classic Docker CLI is installed at `/usr/bin/docker`. The Docker Desktop installer also installs a Docker CLI binary that includes cloud-integration capabilities and is essentially a wrapper for the Compose CLI, at`/usr/local/bin/com.docker.cli`. The symlink ensures that the wrapper can access the classic Docker CLI.
  - 在 `/usr/local/bin/com.docker.cli` 和 `/usr/bin/docker` 之间创建符号链接。经典的 Docker CLI 安装在 `/usr/bin/docker`。Docker Desktop 安装器还安装了一个带有云集成功能的 Docker CLI 二进制文件，本质上是 Compose CLI 的包装器，位于 `/usr/local/bin/com.docker.cli`。该符号链接确保包装器可以访问经典的 Docker CLI。

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

## 升级 Docker Desktop - Upgrade Docker Desktop

Once a new version for Docker Desktop is released, the Docker UI shows a notification. You need to first remove the previous version and then download the new package each time you want to upgrade Docker Desktop. Run:

​	当 Docker Desktop 发布新版本后，Docker UI 会显示通知。要升级 Docker Desktop，您需要首先移除旧版本，然后下载新包。运行：



```console
$ sudo dnf remove docker-desktop
$ sudo dnf install ./docker-desktop-<arch>.rpm
```

Don't forget to substitute `<arch>` with the architecture you want.

​	请将 `<arch>` 替换为您需要的架构。

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
