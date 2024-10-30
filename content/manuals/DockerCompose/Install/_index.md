+++
title = "安装"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Overview of installing Docker Compose - Docker Compose 安装概述

This page contains summary information about the available options for installing Docker Compose.

​	本页面概述了安装 Docker Compose 的可用选项。

## 安装场景 Installation scenarios

### 场景一：安装 Docker Desktop - Scenario one: Install Docker Desktop

The easiest and recommended way to get Docker Compose is to install Docker Desktop. Docker Desktop includes Docker Compose along with Docker Engine and Docker CLI which are Compose prerequisites.

​	获取 Docker Compose 最简单且推荐的方式是安装 Docker Desktop。Docker Desktop 包含 Docker Compose 以及 Docker Engine 和 Docker CLI，这些是 Compose 的先决条件。

Docker Desktop is available on:

​	Docker Desktop 可用于以下平台：

- [Linux]({{< ref "/manuals/DockerDesktop/Install/Linux" >}})
- [Mac]({{< ref "/manuals/DockerDesktop/Install/Mac" >}})
- [Windows]({{< ref "/manuals/DockerDesktop/Install/Windows" >}})

If you have already installed Docker Desktop, you can check which version of Compose you have by selecting **About Docker Desktop** from the Docker menu ![whale menu](_index_img/whale-x.svg) .

​	如果您已安装 Docker Desktop，可以通过在 Docker 菜单中选择 **关于 Docker Desktop** 来检查您拥有的 Compose 版本

> **Note**
>
> 
>
> After Docker Compose V1 was removed in Docker Desktop version [4.23.0](https://docs.docker.com/desktop/release-notes/#4230) as it had reached end-of-life, the `docker-compose` command now points directly to the Docker Compose V2 binary, running in standalone mode. If you rely on Docker Desktop auto-update, the symlink might be broken and command unavailable, as the update doesn't ask for administrator password.
>
> ​	Docker Compose V1 在 Docker Desktop 版本 [4.23.0](https://docs.docker.com/desktop/release-notes/#4230) 中被移除，因为它已达到生命周期结束。现在，`docker-compose` 命令直接指向以独立模式运行的 Docker Compose V2 二进制文件。如果您依赖 Docker Desktop 的自动更新，更新后可能导致符号链接损坏或命令不可用，因为更新不会请求管理员密码。
>
> This only affects Mac users. To fix this, either recreate the symlink:
>
> ​	这仅影响 Mac 用户。要解决此问题，可以重新创建符号链接：
>
> ```console
> $ sudo rm /usr/local/bin/docker-compose
> $ sudo ln -s /Applications/Docker.app/Contents/Resources/cli-plugins/docker-compose /usr/local/bin/docker-compose
> ```
>
> Or enable [Automatically check configuration]({{< ref "/manuals/DockerDesktop/Changesettings" >}}) which will detect and fix it for you.
>
> ​	或启用 [自动检查配置]({{< ref "/manuals/DockerDesktop/Changesettings" >}})，系统会自动检测并修复此问题。

### 场景二：安装 Compose 插件 Scenario two: Install the Compose plugin

If you already have Docker Engine and Docker CLI installed, you can install the Compose plugin from the command line, by either:

​	如果您已经安装了 Docker Engine 和 Docker CLI，可以通过以下方式从命令行安装 Compose 插件：

- [Using Docker's repository 使用 Docker 的存储库](https://docs.docker.com/compose/install/linux/#install-using-the-repository)
- [Downloading and installing manually 手动下载和安装](https://docs.docker.com/compose/install/linux/#install-the-plugin-manually)

> **Important**
>
> 
>
> This is only available on Linux
>
> ​	此安装方式仅适用于 Linux

### 场景三：安装 Compose 独立版本 Scenario three: Install the Compose standalone

You can [install the Compose standalone]({{< ref "/manuals/DockerCompose/Install/InstallComposestandalone" >}}) on Linux or on Windows Server.

​	您可以在 Linux 或 Windows Server 上[安装 Compose 独立版本]({{< ref "/manuals/DockerCompose/Install/InstallComposestandalone" >}})。

> **Warning**
>
> 
>
> This install scenario is not recommended and is only supported for backward compatibility purposes.
>
> ​	这种安装场景不推荐，仅用于向后兼容。
