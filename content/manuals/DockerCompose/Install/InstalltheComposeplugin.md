+++
title = "安装 Compose 插件"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/install/linux/](https://docs.docker.com/compose/install/linux/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install the Compose plugin - 安装 Compose 插件

On this page you can find instructions on how to install the Compose plugin on Linux from the command line.

​	本页面介绍如何在 Linux 上通过命令行安装 Compose 插件。

To install the Compose plugin on Linux, you can either:

​	在 Linux 上安装 Compose 插件的方式如下：

- [Set up Docker's repository on your Linux system [在 Linux 系统上设置 Docker 的存储库](https://docs.docker.com/compose/install/linux/#install-using-the-repository)](https://docs.docker.com/compose/install/linux/#install-using-the-repository).
- [Install Compose manually 手动安装 Compose](https://docs.docker.com/compose/install/linux/#install-the-plugin-manually).

> **Note**
>
> 
>
> These instructions assume you already have Docker Engine and Docker CLI installed and now want to install the Compose plugin.
> For Compose standalone, see [Install Compose Standalone]({{< ref "/manuals/DockerCompose/Install/InstallComposestandalone" >}}).
>
> ​	这些说明假设您已经安装了 Docker Engine 和 Docker CLI，并且现在想安装 Compose 插件。 如果需要安装 Compose 独立版，请参见 [安装 Compose 独立版]({{< ref "/manuals/DockerCompose/Install/InstallComposestandalone" >}})。

## Install using the repository

1. Set up the repository. Find distro-specific instructions in: 设置存储库。查找适用于特定发行版的说明：

   [Ubuntu](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository) | [CentOS](https://docs.docker.com/engine/install/centos/#set-up-the-repository) | [Debian](https://docs.docker.com/engine/install/debian/#install-using-the-repository) | [Raspberry Pi OS](https://docs.docker.com/engine/install/raspberry-pi-os/#install-using-the-repository) | [Fedora](https://docs.docker.com/engine/install/fedora/#set-up-the-repository) | [RHEL](https://docs.docker.com/engine/install/rhel/#set-up-the-repository) | [SLES](https://docs.docker.com/engine/install/sles/#set-up-the-repository).

2. Update the package index, and install the latest version of Docker Compose: 更新包索引，并安装最新版本的 Docker Compose：

   - For Ubuntu and Debian, run:

     对于 Ubuntu 和 Debian，运行：

     ```console
     $ sudo apt-get update
     $ sudo apt-get install docker-compose-plugin
     ```

   - For RPM-based distros, run:

     对于基于 RPM 的发行版，运行：

     ```console
     $ sudo yum update
     $ sudo yum install docker-compose-plugin
     ```

3. Verify that Docker Compose is installed correctly by checking the version.

   通过检查版本验证 Docker Compose 是否正确安装。

   ```console
   $ docker compose version
   ```

   Expected output:

   预期输出：

   ```text
   Docker Compose version vN.N.N
   ```

   Where `vN.N.N` is placeholder text standing in for the latest version.
   
   ​	其中 `vN.N.N` 是最新版本的占位符。

### Update Compose

To update the Compose plugin, run the following commands:

​	要更新 Compose 插件，请运行以下命令：

- For Ubuntu and Debian, run:

  对于 Ubuntu 和 Debian，运行：

  ```console
  $ sudo apt-get update
  $ sudo apt-get install docker-compose-plugin
  ```

- For RPM-based distros, run:

  对于基于 RPM 的发行版，运行：

  ```console
  $ sudo yum update
  $ sudo yum install docker-compose-plugin
  ```

## Install the plugin manually

> **Note**
>
> 
>
> This option requires you to manage upgrades manually. We recommend setting up Docker's repository for easier maintenance.
>
> ​	此选项要求手动管理升级。建议设置 Docker 的存储库以简化维护。

1. To download and install the Compose CLI plugin, run:

   下载并安装 Compose CLI 插件，运行：

   ```console
   $ DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}
   $ mkdir -p $DOCKER_CONFIG/cli-plugins
   $ curl -SL https://github.com/docker/compose/releases/download/v2.29.6/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
   ```

   This command downloads the latest release of Docker Compose (from the Compose releases repository) and installs Compose for the active user under `$HOME` directory.

   ​	此命令将从 Compose 发布库下载最新的 Docker Compose 版本，并在 `$HOME` 目录下为当前用户安装 Compose。

   To install:

   - Docker Compose for *all users* on your system, replace `~/.docker/cli-plugins` with `/usr/local/lib/docker/cli-plugins`.
     - 如需为*所有用户*安装 Docker Compose，请将 `~/.docker/cli-plugins` 替换为 `/usr/local/lib/docker/cli-plugins`。
   - A different version of Compose, substitute `v2.29.6` with the version of Compose you want to use.
     - 如需安装其他版本的 Compose，将 `v2.29.6` 替换为所需的版本。

   - For a different architecture, substitute `x86_64` with the [architecture you want](https://github.com/docker/compose/releases).
     - 如需不同架构，请将 `x86_64` 替换为所需的 [架构](https://github.com/docker/compose/releases)。

2. Apply executable permissions to the binary:

   为二进制文件设置可执行权限：

   ```console
   $ chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose
   ```

   or, if you chose to install Compose for all users:

   或者，如果为所有用户安装 Compose：

   ```console
   $ sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
   ```

3. Test the installation.

   测试安装。

   ```console
   $ docker compose version
   ```

   Expected output:

   预期输出：

   ```text
   Docker Compose version v2.29.6
   ```
