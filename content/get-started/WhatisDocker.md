+++
title = "What is Docker?"
date = 2024-10-23T14:54:35+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/get-started/docker-overview/](https://docs.docker.com/get-started/docker-overview/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# 什么是 Docker? What is Docker?

Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker's methodologies for shipping, testing, and deploying code, you can significantly reduce the delay between writing code and running it in production.

​	Docker 是一个用于开发、交付和运行应用程序的开放平台。Docker 使您能够将应用程序与基础设施分离，从而可以快速交付软件。使用 Docker，您可以像管理应用程序一样管理基础设施。通过利用 Docker 的方法来交付、测试和部署代码，您可以显著减少从编写代码到在生产环境中运行的延迟。

## Docker 平台 The Docker platform

Docker provides the ability to package and run an application in a loosely isolated environment called a container. The isolation and security lets you run many containers simultaneously on a given host. Containers are lightweight and contain everything needed to run the application, so you don't need to rely on what's installed on the host. You can share containers while you work, and be sure that everyone you share with gets the same container that works in the same way.

​	Docker 提供了将应用程序打包并在一个称为容器的松散隔离环境中运行的能力。隔离性和安全性使您能够在给定主机上同时运行多个容器。容器轻量且包含运行应用程序所需的一切，因此您不必依赖主机上安装的内容。您可以在工作时共享容器，并确保与您共享的每个人都能获得相同的容器，并以相同的方式工作。

Docker provides tooling and a platform to manage the lifecycle of your containers:

​	Docker 提供了管理容器生命周期的工具和平台：

- Develop your application and its supporting components using containers.
- 使用容器开发您的应用程序及其支持组件。

- The container becomes the unit for distributing and testing your application.
- 容器成为分发和测试应用程序的单位。
- When you're ready, deploy your application into your production environment, as a container or an orchestrated service. This works the same whether your production environment is a local data center, a cloud provider, or a hybrid of the two.
- 准备就绪后，将应用程序作为容器或编排服务部署到生产环境中。无论您的生产环境是本地数据中心、云提供商还是两者的混合体，这种方式都适用。

## 我可以用 Docker 做什么？What can I use Docker for?

### 应用程序的快速、一致交付 Fast, consistent delivery of your applications 

Docker streamlines the development lifecycle by allowing developers to work in standardized environments using local containers which provide your applications and services. Containers are great for continuous integration and continuous delivery (CI/CD) workflows.

​	Docker 通过允许开发人员在标准化环境中使用提供应用程序和服务的本地容器，简化了开发生命周期。容器非常适合持续集成和持续交付（CI/CD）工作流程。

Consider the following example scenario:

​	请考虑以下示例场景：

- Your developers write code locally and share their work with their colleagues using Docker containers.
- 您的开发人员在本地编写代码，并使用 Docker 容器与同事共享他们的工作。

- They use Docker to push their applications into a test environment and run automated and manual tests.
- 他们使用 Docker 将应用程序推送到测试环境，并运行自动和手动测试。
- When developers find bugs, they can fix them in the development environment and redeploy them to the test environment for testing and validation.
- 当开发人员发现错误时，他们可以在开发环境中修复并重新部署到测试环境进行测试和验证。
- When testing is complete, getting the fix to the customer is as simple as pushing the updated image to the production environment.
- 测试完成后，将修复提交给客户的过程就像将更新的镜像推送到生产环境一样简单。

### 响应式部署和扩展 Responsive deployment and scaling

Docker's container-based platform allows for highly portable workloads. Docker containers can run on a developer's local laptop, on physical or virtual machines in a data center, on cloud providers, or in a mixture of environments.

​	Docker 的基于容器的平台允许高度可移植的工作负载。Docker 容器可以在开发人员的本地笔记本电脑、数据中心的物理或虚拟机、云提供商或混合环境中运行。

Docker's portability and lightweight nature also make it easy to dynamically manage workloads, scaling up or tearing down applications and services as business needs dictate, in near real time.

​	Docker 的可移植性和轻量特性也使得动态管理工作负载变得容易，可以根据业务需求，几乎实时地扩展或缩减应用程序和服务。

### 在相同硬件上运行更多工作负载 Running more workloads on the same hardware

Docker is lightweight and fast. It provides a viable, cost-effective alternative to hypervisor-based virtual machines, so you can use more of your server capacity to achieve your business goals. Docker is perfect for high density environments and for small and medium deployments where you need to do more with fewer resources.

Docker 轻量且快速。它为基于虚拟机管理程序的虚拟机提供了一种可行且具有成本效益的替代方案，因此您可以利用更多的服务器容量来实现业务目标。Docker 非常适合高密度环境以及需要用更少资源做更多事情的小型和中型部署。

## Docker 架构 Docker architecture

Docker uses a client-server architecture. The Docker client talks to the Docker daemon, which does the heavy lifting of building, running, and distributing your Docker containers. The Docker client and daemon can run on the same system, or you can connect a Docker client to a remote Docker daemon. The Docker client and daemon communicate using a REST API, over UNIX sockets or a network interface. Another Docker client is Docker Compose, that lets you work with applications consisting of a set of containers.

​	Docker 使用客户端-服务器架构。Docker 客户端与 Docker 守护进程通信，后者负责构建、运行和分发您的 Docker 容器的繁重工作。Docker 客户端和守护进程可以在同一个系统上运行，或者您可以将 Docker 客户端连接到远程的 Docker 守护进程。Docker 客户端和守护进程通过 REST API 通信，使用 UNIX 套接字或网络接口。另一个 Docker 客户端是 Docker Compose，它允许您处理由一组容器组成的应用程序。![Docker Architecture diagram](WhatisDocker_img/docker-architecture.webp)

### Docker 守护进程 The Docker daemon

The Docker daemon (`dockerd`) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.

​	Docker 守护进程（`dockerd`）监听 Docker API 请求并管理 Docker 对象，例如镜像、容器、网络和卷。守护进程还可以与其他守护进程通信，以管理 Docker 服务。

### Docker 客户端 The Docker client

The Docker client (`docker`) is the primary way that many Docker users interact with Docker. When you use commands such as `docker run`, the client sends these commands to `dockerd`, which carries them out. The `docker` command uses the Docker API. The Docker client can communicate with more than one daemon.

​	Docker 客户端（`docker`）是许多 Docker 用户与 Docker 交互的主要方式。当您使用诸如 `docker run` 的命令时，客户端会将这些命令发送给 `dockerd`，后者负责执行这些命令。`docker` 命令使用 Docker API。Docker 客户端可以与多个守护进程通信。

### Docker Desktop

Docker Desktop is an easy-to-install application for your Mac, Windows or Linux environment that enables you to build and share containerized applications and microservices. Docker Desktop includes the Docker daemon (`dockerd`), the Docker client (`docker`), Docker Compose, Docker Content Trust, Kubernetes, and Credential Helper. For more information, see [Docker Desktop]({{< ref "/manuals/DockerDesktop" >}}).

​	Docker Desktop 是一个易于安装的应用程序，适用于您的 Mac、Windows 或 Linux 环境，使您能够构建和共享容器化的应用程序和微服务。Docker Desktop 包含 Docker 守护进程（`dockerd`）、Docker 客户端（`docker`）、Docker Compose、Docker 内容信任、Kubernetes 和凭据助手。更多信息请参见 [Docker Desktop]({{< ref "/manuals/DockerDesktop" >}})。

### Docker 注册表 Docker registries

A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use, and Docker looks for images on Docker Hub by default. You can even run your own private registry.

​	Docker 注册表存储 Docker 镜像。Docker Hub 是一个任何人都可以使用的公共注册表，默认情况下 Docker 会在 Docker Hub 上查找镜像。您甚至可以运行自己的私有注册表。

When you use the `docker pull` or `docker run` commands, Docker pulls the required images from your configured registry. When you use the `docker push` command, Docker pushes your image to your configured registry.

​	当您使用 `docker pull` 或 `docker run` 命令时，Docker 会从您配置的注册表中拉取所需的镜像。当您使用 `docker push` 命令时，Docker 会将您的镜像推送到您配置的注册表中。

### Docker 对象 Docker objects

When you use Docker, you are creating and using images, containers, networks, volumes, plugins, and other objects. This section is a brief overview of some of those objects.

​	当您使用 Docker 时，您正在创建和使用镜像、容器、网络、卷、插件和其他对象。本节简要概述了其中的一些对象。

#### 镜像 Images

An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the `ubuntu` image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

​	镜像是一个只读模板，包含创建 Docker 容器的指令。通常，镜像是基于另一个镜像的，并带有一些额外的自定义。例如，您可能会构建一个基于 `ubuntu` 镜像的镜像，但安装了 Apache Web 服务器和您的应用程序，以及使您的应用程序运行所需的配置详细信息。

You might create your own images or you might only use those created by others and published in a registry. To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.

​	您可以创建自己的镜像，也可以只使用其他人创建并发布在注册表中的镜像。要构建自己的镜像，您需要创建一个 Dockerfile，使用简单的语法来定义创建镜像和运行它所需的步骤。Dockerfile 中的每个指令都会在镜像中创建一个层。当您更改 Dockerfile 并重新构建镜像时，只有发生更改的层会被重新构建。这也是与其他虚拟化技术相比，镜像如此轻量、小巧且快速的部分原因。

#### 容器 Containers

A container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI. You can connect a container to one or more networks, attach storage to it, or even create a new image based on its current state.

​	容器是镜像的可运行实例。您可以使用 Docker API 或 CLI 创建、启动、停止、移动或删除容器。您可以将容器连接到一个或多个网络，向其附加存储，甚至根据其当前状态创建一个新的镜像。

By default, a container is relatively well isolated from other containers and its host machine. You can control how isolated a container's network, storage, or other underlying subsystems are from other containers or from the host machine.

​	默认情况下，容器与其他容器及其主机相对隔离。您可以控制容器的网络、存储或其他底层子系统与其他容器或主机的隔离程度。

A container is defined by its image as well as any configuration options you provide to it when you create or start it. When a container is removed, any changes to its state that aren't stored in persistent storage disappear.

​	容器由其镜像以及您在创建或启动时提供的任何配置选项定义。当容器被删除时，任何没有存储在持久性存储中的状态更改都会消失。

##### 示例 `docker run` 命令 Example `docker run` command

The following command runs an `ubuntu` container, attaches interactively to your local command-line session, and runs `/bin/bash`.

​	以下命令运行一个 `ubuntu` 容器，交互式附加到您的本地命令行会话，并运行 `/bin/bash`。



```console
$ docker run -i -t ubuntu /bin/bash
```

When you run this command, the following happens (assuming you are using the default registry configuration):

​	当您运行此命令时，会发生以下情况（假设您使用默认的注册表配置）：

1. If you don't have the `ubuntu` image locally, Docker pulls it from your configured registry, as though you had run `docker pull ubuntu` manually. 如果您本地没有 `ubuntu` 镜像，Docker 会从您配置的注册表中拉取它，就像您手动运行 `docker pull ubuntu` 一样。
2. Docker creates a new container, as though you had run a `docker container create` command manually. Docker 创建了一个新容器，就像您手动运行了 `docker container create` 命令一样。
3. Docker allocates a read-write filesystem to the container, as its final layer. This allows a running container to create or modify files and directories in its local filesystem. Docker 为容器分配了一个读写文件系统，作为其最终层。这允许运行的容器在其本地文件系统中创建或修改文件和目录。
4. Docker creates a network interface to connect the container to the default network, since you didn't specify any networking options. This includes assigning an IP address to the container. By default, containers can connect to external networks using the host machine's network connection. Docker 创建了一个网络接口，将容器连接到默认网络，因为您没有指定任何网络选项。这包括为容器分配一个 IP 地址。默认情况下，容器可以使用主机的网络连接来连接到外部网络。
5. Docker starts the container and executes `/bin/bash`. Because the container is running interactively and attached to your terminal (due to the `-i` and `-t` flags), you can provide input using your keyboard while Docker logs the output to your terminal. Docker 启动容器并执行 `/bin/bash`。由于容器是交互式运行的并附加到您的终端（由于 `-i` 和 `-t` 标志），因此您可以使用键盘提供输入，同时 Docker 将输出记录到您的终端。
6. When you run `exit` to terminate the `/bin/bash` command, the container stops but isn't removed. You can start it again or remove it.  当您运行 `exit` 终止 `/bin/bash` 命令时，容器停止但不会被移除。您可以再次启动它或将其移除。

## 底层技术 The underlying technology

Docker is written in the [Go programming language](https://golang.org/) and takes advantage of several features of the Linux kernel to deliver its functionality. Docker uses a technology called `namespaces` to provide the isolated workspace called the container. When you run a container, Docker creates a set of namespaces for that container.

​	Docker 使用 [Go 编程语言](https://golang.org/) 编写，并利用 Linux 内核的多个功能来提供其功能。Docker 使用一种称为 `namespaces` 的技术来提供称为容器的隔离工作区。当您运行容器时，Docker 会为该容器创建一组命名空间。

These namespaces provide a layer of isolation. Each aspect of a container runs in a separate namespace and its access is limited to that namespace.

​	这些命名空间提供了一层隔离。容器的每个方面都在一个单独的命名空间中运行，其访问权限仅限于该命名空间。

## Next steps

- [Install Docker]({{< ref "/get-started/GetDocker" >}}) [安装 Docker]({{< ref "/get-started/GetDocker" >}})
- [Get started with Docker]({{< ref "/get-started/Introduction" >}}) [开始使用 Docker]({{< ref "/get-started/Introduction" >}})
