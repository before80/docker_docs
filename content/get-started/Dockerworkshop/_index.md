+++
title = "Docker 讲习班"
date = 2024-10-23T14:54:35+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/get-started/workshop/](https://docs.docker.com/get-started/workshop/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# Overview of the Docker workshop - Docker 讲习班概述

This 45-minute workshop contains step-by-step instructions on how to get started with Docker. This workshop shows you how to:

​	本次为期45分钟的讲习班包含如何入门使用 Docker 的逐步指导。通过本次讲习班，你将学习到以下内容：

- Build and run an image as a container. 构建并将镜像作为容器运行。

- Share images using Docker Hub. 使用 Docker Hub 分享镜像。
- Deploy Docker applications using multiple containers with a database. 使用带有数据库的多个容器部署 Docker 应用程序。
- Run applications using Docker Compose.  使用 Docker Compose 运行应用程序。

> **Note**
>
> 
>
> For a quick introduction to Docker and the benefits of containerizing your applications, see [Getting started]({{< ref "/get-started/Introduction" >}}).
>
> ​	如果你想快速了解 Docker 以及将应用程序容器化的好处，请参阅[入门指南]({{< ref "/get-started/Introduction" >}})。

## 什么是容器？ What is a container?

A container is a sandboxed process running on a host machine that is isolated from all other processes running on that host machine. That isolation leverages [kernel namespaces and cgroups](https://medium.com/@saschagrunert/demystifying-containers-part-i-kernel-space-2c53d6979504), features that have been in Linux for a long time. Docker makes these capabilities approachable and easy to use. To summarize, a container:

​	容器是运行在主机上的一个被隔离的进程，它与主机上运行的其他所有进程隔离。这个隔离机制依赖于[内核命名空间和控制组（cgroups）](https://medium.com/@saschagrunert/demystifying-containers-part-i-kernel-space-2c53d6979504)，这些特性在 Linux 系统中已经存在很长时间了。Docker 让这些功能变得更易于使用。简而言之，容器：

- Is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI. 是镜像的可运行实例。你可以通过 Docker API 或 CLI 创建、启动、停止、移动或删除容器。

- Can be run on local machines, virtual machines, or deployed to the cloud. 可以在本地机器、虚拟机或云端运行。
- Is portable (and can be run on any OS). 具有可移植性（可以在任何操作系统上运行）。
- Is isolated from other containers and runs its own software, binaries, configurations, etc. 与其他容器隔离，运行自己的软件、二进制文件、配置等。

If you're familiar with `chroot`, then think of a container as an extended version of `chroot`. The filesystem comes from the image. However, a container adds additional isolation not available when using chroot.

​	如果你熟悉 `chroot`，那么可以把容器看作是 `chroot` 的扩展版。容器的文件系统来自镜像，但容器还增加了 `chroot` 所不具备的其他隔离功能。

## 什么是镜像？ What is an image?

A running container uses an isolated filesystem. This isolated filesystem is provided by an image, and the image must contain everything needed to run an application - all dependencies, configurations, scripts, binaries, etc. The image also contains other configurations for the container, such as environment variables, a default command to run, and other metadata.

​	运行中的容器使用一个独立的文件系统。这个独立的文件系统是由镜像提供的，镜像包含运行应用程序所需的一切——所有依赖项、配置、脚本、二进制文件等。镜像还包含容器的其他配置，例如环境变量、默认运行的命令以及其他元数据。

## 接下来 Next steps

In this section, you learned about containers and images.

​	在本节中，你了解了容器和镜像的基础知识。

Next, you'll containerize a simple application and get hands-on with the concepts.

​	接下来，你将把一个简单的应用程序容器化，亲身实践这些概念。

[Containerize an application]({{< ref "/get-started/Dockerworkshop/Part1Containerizeanapplication" >}}) - [将应用程序容器化]({{< ref "/get-started/Dockerworkshop/Part1Containerizeanapplication" >}})
