+++
title = "Docker Compose 独立版安装"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/install/standalone/](https://docs.docker.com/compose/install/standalone/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install Compose standalone - Docker Compose 独立版安装

On this page you can find instructions on how to install Compose standalone on Linux or Windows Server, from the command line.

​	本页面介绍如何在 Linux 或 Windows Server 上通过命令行安装 Compose 独立版。

### On Linux

> **Warning**
>
> 
>
> Note that Compose standalone uses the `-compose` syntax instead of the current standard syntax `compose`.
> For example type `docker-compose up` when using Compose standalone, instead of `docker compose up`.
>
> ​	Compose 独立版使用 `-compose` 语法而非当前的标准语法 `compose`。 例如，使用 Compose 独立版时，需键入 `docker-compose up`，而不是 `docker compose up`。

1. To download and install Compose standalone, run:

   下载并安装 Compose 独立版，运行以下命令：

   ```console
   $ curl -SL https://github.com/docker/compose/releases/download/v2.29.6/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
   ```

2. Apply executable permissions to the standalone binary in the target path for the installation. 为目标路径中的独立二进制文件设置可执行权限。

3. Test and execute compose commands using `docker-compose`. 使用 `docker-compose` 测试并执行 compose 命令。

   > **Tip**
   >
   > 
   >
   > If the command `docker-compose` fails after installation, check your path. You can also create a symbolic link to `/usr/bin` or any other directory in your path. For example:
   >
   > ​	如果安装后 `docker-compose` 命令失败，检查您的路径。也可以在路径中的任意目录（如 `/usr/bin`）创建符号链接。例如：
   >
   > ```console
   > $ sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
   > ```

### On Windows Server

Follow these instructions if you are running the Docker daemon and client directly on Microsoft Windows Server and want to install Docker Compose.

​	如果您在 Microsoft Windows Server 上直接运行 Docker 守护进程和客户端，并希望安装 Docker Compose，请按照以下说明操作：

1. Run PowerShell as an administrator. When asked if you want to allow this app to make changes to your device, select **Yes** in order to proceed with the installation. 以管理员身份运行 PowerShell。当系统提示是否允许此应用更改设备时，选择 **是** 以继续安装。

2. GitHub now requires TLS1.2. In PowerShell, run the following:

   GitHub 现在要求 TLS1.2。在 PowerShell 中运行以下命令：

   ```powershell
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
   ```

3. Run the following command to download the latest release of Compose (v2.29.6):

   运行以下命令以下载最新版本的 Compose (v2.29.6)：

   ```powershell
    Start-BitsTransfer -Source "https://github.com/docker/compose/releases/download/v2.29.6/docker-compose-windows-x86_64.exe" -Destination $Env:ProgramFiles\Docker\docker-compose.exe
   ```

   xxxxxxxxxx1 1$ docker info --format '{{range .ClientInfo.Plugins}}{{if eq .Name "compose"}}{{.Path}}{{end}}{{end}}'console

   ​	如需安装其他版本的 Compose，将 `v2.29.6` 替换为您要使用的 Compose 版本。

   > **Note**
   >
   > 
   >
   > On Windows Server 2019 you can add the Compose executable to `$Env:ProgramFiles\Docker`. Because this directory is registered in the system `PATH`, you can run the `docker-compose --version` command on the subsequent step with no additional configuration.
   >
   > ​	在 Windows Server 2019 上，可以将 Compose 可执行文件添加到 `$Env:ProgramFiles\Docker`。因为此目录已注册到系统 `PATH` 中，您可以直接运行 `docker-compose --version` 命令，无需额外配置。

4. Test the installation.

   测试安装。

   ```console
   $ docker-compose.exe version
   Docker Compose version v2.29.6
   ```
