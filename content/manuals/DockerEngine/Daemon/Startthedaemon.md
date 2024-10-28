+++
title = "启动守护进程"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/daemon/start/](https://docs.docker.com/engine/daemon/start/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Start the daemon - 启动守护进程

This page shows how to start the daemon, either manually or using OS utilities.

​	本页介绍了如何手动或使用操作系统工具启动 Docker 守护进程。

## 使用操作系统工具启动守护进程 Start the daemon using operating system utilities

On a typical installation the Docker daemon is started by a system utility, not manually by a user. This makes it easier to automatically start Docker when the machine reboots.

​	在典型的安装中，Docker 守护进程由系统工具启动，而不是手动启动。这使得在机器重启时自动启动 Docker 更加容易。

The command to start Docker depends on your operating system. Check the correct page under [Install Docker]({{< ref "/manuals/DockerEngine/Install" >}}).

​	启动 Docker 的命令取决于您的操作系统。请检查 [安装 Docker]({{< ref "/manuals/DockerEngine/Install" >}}) 页面下的正确页面。

### 使用 systemd 启动 Start with systemd

On some operating systems, like Ubuntu and Debian, the Docker daemon service starts automatically. Use the following command to start it manually:

​	在一些操作系统（例如 Ubuntu 和 Debian）上，Docker 守护进程服务会自动启动。使用以下命令手动启动它：



```console
$ sudo systemctl start docker
```

If you want Docker to start at boot, see [Configure Docker to start on boot](https://docs.docker.com/engine/install/linux-postinstall/#configure-docker-to-start-on-boot-with-systemd).

​	如果希望 Docker 在启动时自动启动，请参阅 [配置 Docker 开机启动](https://docs.docker.com/engine/install/linux-postinstall/#configure-docker-to-start-on-boot-with-systemd)。

## 手动启动守护进程 Start the daemon manually

If you don't want to use a system utility to manage the Docker daemon, or just want to test things out, you can manually run it using the `dockerd` command. You may need to use `sudo`, depending on your operating system configuration.

​	如果您不想使用系统工具来管理 Docker 守护进程，或者只是想进行测试，可以使用 `dockerd` 命令手动运行它。根据操作系统的配置，可能需要使用 `sudo`。

When you start Docker this way, it runs in the foreground and sends its logs directly to your terminal.

​	以这种方式启动 Docker 时，它会在前台运行，并将日志直接发送到您的终端。



```console
$ dockerd

INFO[0000] +job init_networkdriver()
INFO[0000] +job serveapi(unix:///var/run/docker.sock)
INFO[0000] Listening for HTTP on unix (/var/run/docker.sock)
```

To stop Docker when you have started it manually, issue a `Ctrl+C` in your terminal.

​	当手动启动 Docker 时，按下 `Ctrl+C` 可以停止 Docker。

