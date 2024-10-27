+++
title = "Docker Engine 的 Linux 安装后步骤"
date = 2024-10-23T14:54:40+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/install/linux-postinstall/](https://docs.docker.com/engine/install/linux-postinstall/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Linux post-installation steps for Docker Engine - Docker 引擎的 Linux 后安装步骤

These optional post-installation procedures describe how to configure your Linux host machine to work better with Docker.

​	这些可选的安装后步骤描述了如何配置 Linux 主机以更好地使用 Docker。

## 以非 root 用户管理 Docker - Manage Docker as a non-root user

The Docker daemon binds to a Unix socket, not a TCP port. By default it's the `root` user that owns the Unix socket, and other users can only access it using `sudo`. The Docker daemon always runs as the `root` user.

​	Docker 守护进程绑定到 Unix 套接字，而不是 TCP 端口。默认情况下，该 Unix 套接字由 `root` 用户拥有，其他用户只能通过 `sudo` 访问它。Docker 守护进程始终以 `root` 用户身份运行。

If you don't want to preface the `docker` command with `sudo`, create a Unix group called `docker` and add users to it. When the Docker daemon starts, it creates a Unix socket accessible by members of the `docker` group. On some Linux distributions, the system automatically creates this group when installing Docker Engine using a package manager. In that case, there is no need for you to manually create the group.

​	如果您不想在 `docker` 命令前加上 `sudo`，可以创建一个名为 `docker` 的 Unix 组并将用户添加到该组。当 Docker 守护进程启动时，它会创建一个 Unix 套接字，供 `docker` 组的成员访问。在某些 Linux 发行版中，使用包管理器安装 Docker 引擎时系统会自动创建该组。在这种情况下，您无需手动创建该组。

> **Warning**
>
> 
>
> The `docker` group grants root-level privileges to the user. For details on how this impacts security in your system, see [Docker Daemon Attack Surface](https://docs.docker.com/engine/security/#docker-daemon-attack-surface).
>
> ​	`docker` 组赋予用户 root 级别的权限。有关这对系统安全性的影响，请参阅 [Docker 守护进程攻击面](https://docs.docker.com/engine/security/#docker-daemon-attack-surface)。

> **Note**
>
> 
>
> To run Docker without root privileges, see [Run the Docker daemon as a non-root user (Rootless mode)]({{< ref "/manuals/DockerEngine/Security/Rootlessmode" >}}).
>
> ​	要在没有 root 权限的情况下运行 Docker，请参阅 [以非 root 用户身份（无根模式）运行 Docker 守护进程]({{< ref "/manuals/DockerEngine/Security/Rootlessmode" >}})。

To create the `docker` group and add your user:

​	要创建 `docker` 组并添加您的用户：

1. Create the `docker` group. 创建 `docker` 组。

   

   ```console
   $ sudo groupadd docker
   ```

2. Add your user to the `docker` group. 将您的用户添加到 `docker` 组。

   

   ```console
   $ sudo usermod -aG docker $USER
   ```

3. Log out and log back in so that your group membership is re-evaluated. 注销并重新登录，以便重新评估您的组成员资格。

   > If you're running Linux in a virtual machine, it may be necessary to restart the virtual machine for changes to take effect.
   >
   > ​	如果您在虚拟机中运行 Linux，可能需要重新启动虚拟机才能使更改生效。

   You can also run the following command to activate the changes to groups:

   ​	您也可以运行以下命令来激活组更改：

   ```console
   $ newgrp docker
   ```

4. Verify that you can run `docker` commands without `sudo`. 验证是否可以在没有 `sudo` 的情况下运行 `docker` 命令。

   

   ```console
   $ docker run hello-world
   ```

   This command downloads a test image and runs it in a container. When the container runs, it prints a message and exits.

   ​	该命令会下载一个测试镜像并在容器中运行。当容器运行时，它会打印一条消息并退出。

   If you initially ran Docker CLI commands using `sudo` before adding your user to the `docker` group, you may see the following error:

   ​	如果您在将用户添加到 `docker` 组之前已使用 `sudo` 运行 Docker CLI 命令，可能会看到以下错误：
   
   
   
   ```none
   WARNING: Error loading config file: /home/user/.docker/config.json -
   stat /home/user/.docker/config.json: permission denied
   ```

   This error indicates that the permission settings for the `~/.docker/` directory are incorrect, due to having used the `sudo` command earlier.

   ​	该错误表示 `~/.docker/` 目录的权限设置不正确，这是由于之前使用了 `sudo` 命令导致的。
   
   To fix this problem, either remove the `~/.docker/` directory (it's recreated automatically, but any custom settings are lost), or change its ownership and permissions using the following commands:
   
   ​	要解决此问题，可以删除 `~/.docker/` 目录（它会自动重新创建，但任何自定义设置会丢失），或者使用以下命令更改其所有权和权限：
   
   
   
   ```console
   $ sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
   $ sudo chmod g+rwx "$HOME/.docker" -R
   ```

## 使用 systemd 配置 Docker 开机启动 Configure Docker to start on boot with systemd

Many modern Linux distributions use [systemd](https://systemd.io/) to manage which services start when the system boots. On Debian and Ubuntu, the Docker service starts on boot by default. To automatically start Docker and containerd on boot for other Linux distributions using systemd, run the following commands:

​	许多现代 Linux 发行版使用 [systemd](https://systemd.io/) 管理系统启动时启动的服务。在 Debian 和 Ubuntu 上，Docker 服务默认在启动时自动启动。要在使用 systemd 的其他 Linux 发行版上设置 Docker 和 containerd 开机自动启动，请运行以下命令：

```console
$ sudo systemctl enable docker.service
$ sudo systemctl enable containerd.service
```

To stop this behavior, use `disable` instead.

​	要取消此行为，请使用 `disable` 替代。



```console
$ sudo systemctl disable docker.service
$ sudo systemctl disable containerd.service
```

You can use systemd unit files to configure the Docker service on startup, for example to add an HTTP proxy, set a different directory or partition for the Docker runtime files, or other customizations. For an example, see [Configure the daemon to use a proxy](https://docs.docker.com/engine/daemon/proxy/#systemd-unit-file).

​	您可以使用 systemd 单元文件来配置 Docker 服务的启动，例如添加 HTTP 代理、更改 Docker 运行时文件的目录或分区，或进行其他自定义配置。有关示例，请参阅[配置守护进程使用代理](https://docs.docker.com/engine/daemon/proxy/#systemd-unit-file)。

## 配置默认日志驱动程序 Configure default logging driver

Docker provides [logging drivers]({{< ref "/manuals/DockerEngine/Logsandmetrics" >}}) for collecting and viewing log data from all containers running on a host. The default logging driver, `json-file`, writes log data to JSON-formatted files on the host filesystem. Over time, these log files expand in size, leading to potential exhaustion of disk resources.

​	Docker 提供了用于收集和查看主机上所有容器日志数据的[日志驱动程序]({{< ref "/manuals/DockerEngine/Logsandmetrics" >}})。默认日志驱动程序 `json-file` 会将日志数据写入主机文件系统中的 JSON 格式文件。随着时间推移，这些日志文件会扩展，可能导致磁盘资源耗尽。

To avoid issues with overusing disk for log data, consider one of the following options:

​	为避免日志数据导致的磁盘占用问题，考虑以下选项：

- Configure the `json-file` logging driver to turn on [log rotation]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/JSONFileloggingdriver" >}}).
  - 配置 `json-file` 日志驱动程序，开启[日志轮转]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/JSONFileloggingdriver" >}})。

- Use an [alternative logging driver](https://docs.docker.com/engine/logging/configure/#configure-the-default-logging-driver) such as the ["local" logging driver]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/Localfileloggingdriver" >}}) that performs log rotation by default.
  - 使用[替代日志驱动程序](https://docs.docker.com/engine/logging/configure/#configure-the-default-logging-driver)，例如默认支持日志轮转的[“本地”日志驱动程序]({{< ref "/manuals/DockerEngine/Logsandmetrics/Loggingdrivers/Localfileloggingdriver" >}})。

- Use a logging driver that sends logs to a remote logging aggregator.
  - 使用将日志发送到远程日志聚合器的日志驱动程序。

## 接下来 Next steps

- Take a look at the [Docker workshop]({{< ref "/get-started/Dockerworkshop" >}}) to learn how to build an image and run it as a containerized application.
  - 查看 [Docker 讲习班]({{< ref "/get-started/Dockerworkshop" >}})，了解如何构建镜像并将其作为容器化应用程序运行。
