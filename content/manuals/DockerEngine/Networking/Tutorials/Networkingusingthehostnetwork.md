+++
title = "Networking using the host network 使用host网络的网络教程"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/network/tutorials/host/](https://docs.docker.com/engine/network/tutorials/host/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Networking using the host network - 使用host网络的网络教程

This series of tutorials deals with networking standalone containers which bind directly to the Docker host's network, with no network isolation. For other networking topics, see the [overview](https://docs.docker.com/engine/network/).

​	本系列教程介绍了如何在直接绑定到 Docker 主机网络的独立容器中进行网络设置，不进行网络隔离。有关其他网络主题，请参见[网络概述](https://docs.docker.com/engine/network/)。

## 目标 Goal

The goal of this tutorial is to start a `nginx` container which binds directly to port 80 on the Docker host. From a networking point of view, this is the same level of isolation as if the `nginx` process were running directly on the Docker host and not in a container. However, in all other ways, such as storage, process namespace, and user namespace, the `nginx` process is isolated from the host.

​	本教程的目标是启动一个直接绑定到 Docker 主机端口 80 的 `nginx` 容器。从网络的角度来看，这与 `nginx` 进程直接在 Docker 主机上运行而不是在容器中运行的隔离级别相同。但是，在存储、进程命名空间和用户命名空间等方面，`nginx` 进程仍然与主机隔离。

## 前置条件 Prerequisites

- This procedure requires port 80 to be available on the Docker host. To make Nginx listen on a different port, see the [documentation for the `nginx` image](https://hub.docker.com/_/nginx/)
  - 该过程需要 Docker 主机上的端口 80 可用。如需让 Nginx 监听其他端口，请参见 [`nginx` 镜像的文档](https://hub.docker.com/_/nginx/)。

- The `host` networking driver only works on Linux hosts, but is available as a [beta feature](https://docs.docker.com/release-lifecycle/#beta) on Docker Desktop version 4.29 and later for Mac, Windows, and Linux. To enable this feature, navigate to the **Resources** tab in **Settings**, and then under **Network** select **Enable host networking**.
  - `host` 网络驱动程序仅适用于 Linux 主机，但在 Docker Desktop 4.29 及更高版本的 Mac、Windows 和 Linux 上作为[测试功能](https://docs.docker.com/release-lifecycle/#beta)提供。启用该功能，请在**设置**的**资源**选项卡下，选择**网络**并勾选**启用主机网络**。

## 步骤 Procedure

1. Create and start the container as a detached process. The `--rm` option means to remove the container once it exits/stops. The `-d` flag means to start the container detached (in the background).

   以分离进程的方式创建并启动容器。`--rm` 选项表示在容器退出/停止时将其删除。`-d` 标志表示在后台启动容器。

   ```console
   $ docker run --rm -d --network host --name my_nginx nginx
   ```

2. Access Nginx by browsing to [http://localhost:80/](http://localhost/). 通过浏览器访问 Nginx，地址为 [http://localhost:80/](http://localhost/)。

3. Examine your network stack using the following commands: 使用以下命令检查您的网络栈：

   - Examine all network interfaces and verify that a new one was not created.

     查看所有网络接口，确认未创建新的接口。

     ```console
     $ ip addr show
     ```

   - Verify which process is bound to port 80, using the `netstat` command. You need to use `sudo` because the process is owned by the Docker daemon user and you otherwise won't be able to see its name or PID.

     使用 `netstat` 命令验证哪个进程绑定到了端口 80。需要使用 `sudo`，因为该进程由 Docker 守护进程用户拥有，否则无法看到其名称或 PID。

     ```console
     $ sudo netstat -tulpn | grep :80
     ```

4. Stop the container. It will be removed automatically as it was started using the `--rm` option.

   停止容器。由于启动时使用了 `--rm` 选项，它会自动被删除。

   ```console
   docker container stop my_nginx
   ```

## 其他网络教程 Other networking tutorials

- [Standalone networking tutorial 独立网络教程](https://docs.docker.com/engine/network/tutorials/standalone/)
- [Overlay networking tutorial - Overlay 网络教程](https://docs.docker.com/engine/network/tutorials/overlay/)
- [Macvlan networking tutorial - Macvlan 网络教程](https://docs.docker.com/engine/network/tutorials/macvlan/)
