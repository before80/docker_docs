+++
title = "Docker Desktop"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/desktop/](https://docs.docker.com/desktop/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Overview of Docker Desktop - Docker Desktop 概述

Docker Desktop is a one-click-install application for your Mac, Linux, or Windows environment that lets you build, share, and run containerized applications and microservices.

​	Docker Desktop 是一个可在 Mac、Linux 或 Windows 环境中一键安装的应用程序，允许您构建、共享和运行容器化的应用程序和微服务。

It provides a straightforward GUI (Graphical User Interface) that lets you manage your containers, applications, and images directly from your machine.

​	它提供了一个简洁的 GUI（图形用户界面），使您可以直接从计算机管理容器、应用程序和镜像。

Docker Desktop reduces the time spent on complex setups so you can focus on writing code. It takes care of port mappings, file system concerns, and other default settings, and is regularly updated with bug fixes and security updates.

​	Docker Desktop 减少了复杂设置的时间消耗，让您可以专注于编写代码。它处理端口映射、文件系统问题及其他默认设置，并定期提供错误修复和安全更新。

{{< tabpane text=true persist=disabled >}}

{{% tab header="Docker Desktop 包含哪些内容？ What's included in Docker Desktop? " %}}

- [Docker Engine]({{< ref "/manuals/DockerEngine" >}})
- Docker CLI client
- [Docker Scout]({{< ref "/manuals/DockerScout" >}}) (additional subscription may apply)
- [Docker Build]({{< ref "/manuals/DockerBuild" >}})
- [Docker Extensions]({{< ref "/manuals/DockerExtensions" >}})
- [Docker Compose]({{< ref "/manuals/DockerCompose" >}})
- [Docker Content Trust]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker" >}})
- [Kubernetes](https://github.com/kubernetes/kubernetes/)
- [Credential Helper 凭据助手](https://github.com/docker/docker-credential-helpers/)

{{% /tab  %}}

{{% tab header="Docker Desktop 的关键特性是什么？ What are the key features of Docker Desktop?" %}}

- Ability to containerize and share any application on any cloud platform, in multiple languages and frameworks.
  - 能够在任何云平台上以多种语言和框架对任何应用程序进行容器化并共享。

- Quick installation and setup of a complete Docker development environment.
  - 快速安装和设置完整的 Docker 开发环境。

- Includes the latest version of Kubernetes.
  - 包含最新版本的 Kubernetes。

- On Windows, the ability to toggle between Linux and Windows containers to build applications.
  - 在 Windows 上，可以在 Linux 和 Windows 容器之间切换来构建应用程序。

- Fast and reliable performance with native Windows Hyper-V virtualization.
  - 通过原生 Windows Hyper-V 虚拟化实现快速和可靠的性能。

- Ability to work natively on Linux through WSL 2 on Windows machines.
  - 在 Windows 机器上通过 WSL 2 实现本地 Linux 兼容。

- Volume mounting for code and data, including file change notifications and easy access to running containers on the localhost network.
  - 支持代码和数据的卷挂载，包括文件更改通知，并可轻松访问本地网络中的运行容器。


{{% /tab  %}}

{{< /tabpane >}}



------

Docker Desktop works with your choice of development tools and languages and gives you access to a vast library of certified images and templates in [Docker Hub](https://hub.docker.com/). This allows development teams to extend their environment to rapidly auto-build, continuously integrate, and collaborate using a secure repository.

​	Docker Desktop 支持您选择的开发工具和语言，并让您可以访问 [Docker Hub](https://hub.docker.com/) 中大量的认证镜像和模板。这样开发团队可以扩展环境，实现快速自动构建、持续集成，并通过安全的存储库进行协作。



Install Docker Desktop

Install Docker Desktop on [Mac]({{< ref "/manuals/DockerDesktop/Install/Mac" >}}), [Windows]({{< ref "/manuals/DockerDesktop/Install/Windows" >}}), or [Linux]({{< ref "/manuals/DockerDesktop/Install/Linux" >}}).

​	在 [Mac]({{< ref "/manuals/DockerDesktop/Install/Mac" >}})、[Windows]({{< ref "/manuals/DockerDesktop/Install/Windows" >}}) 或 [Linux]({{< ref "/manuals/DockerDesktop/Install/Linux" >}}) 上安装 Docker Desktop。

Explore Docker Desktop

Navigate Docker Desktop and learn about its key features.

​	浏览 Docker Desktop 并了解其关键功能。



View the release notes

Find out about new features, improvements, and bug fixes.

​	了解新特性、改进和错误修复。



Browse common FAQs

Explore general FAQs or FAQs for specific platforms.

​	探索一般常见问题解答或特定平台的常见问题解答。



Find additional resources

Find information on networking features, deploying on Kubernetes, and more.

​	查找有关网络功能、在 Kubernetes 上部署等信息。



Give feedback

Provide feedback on Docker Desktop or Docker Desktop features.

​	为 Docker Desktop 或 Docker Desktop 功能提供反馈。
