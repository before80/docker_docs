+++
title = "Install Compose standalone"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/compose/install/standalone/](https://docs.docker.com/compose/install/standalone/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install Compose standalone

On this page you can find instructions on how to install Compose standalone on Linux or Windows Server, from the command line.

### [On Linux](https://docs.docker.com/compose/install/standalone/#on-linux)

> **Warning**
>
> 
>
> Note that Compose standalone uses the `-compose` syntax instead of the current standard syntax `compose`.
> For example type `docker-compose up` when using Compose standalone, instead of `docker compose up`.

1. To download and install Compose standalone, run:

   

   ```console
   $ curl -SL https://github.com/docker/compose/releases/download/v2.29.6/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
   ```

2. Apply executable permissions to the standalone binary in the target path for the installation.

3. Test and execute compose commands using `docker-compose`.

   > **Tip**
   >
   > 
   >
   > If the command `docker-compose` fails after installation, check your path. You can also create a symbolic link to `/usr/bin` or any other directory in your path. For example:
   >
   > 
   >
   > ```console
   > $ sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
   > ```

### [On Windows Server](https://docs.docker.com/compose/install/standalone/#on-windows-server)

Follow these instructions if you are running the Docker daemon and client directly on Microsoft Windows Server and want to install Docker Compose.

1. Run PowerShell as an administrator. When asked if you want to allow this app to make changes to your device, select **Yes** in order to proceed with the installation.

2. GitHub now requires TLS1.2. In PowerShell, run the following:

   

   ```powershell
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
   ```

3. Run the following command to download the latest release of Compose (v2.29.6):

   

   ```powershell
    Start-BitsTransfer -Source "https://github.com/docker/compose/releases/download/v2.29.6/docker-compose-windows-x86_64.exe" -Destination $Env:ProgramFiles\Docker\docker-compose.exe
   ```

   To install a different version of Compose, substitute `v2.29.6` with the version of Compose you want to use.

   > **Note**
   >
   > 
   >
   > On Windows Server 2019 you can add the Compose executable to `$Env:ProgramFiles\Docker`. Because this directory is registered in the system `PATH`, you can run the `docker-compose --version` command on the subsequent step with no additional configuration.

4. Test the installation.

   

   ```console
   $ docker-compose.exe version
   Docker Compose version v2.29.6
   ```
