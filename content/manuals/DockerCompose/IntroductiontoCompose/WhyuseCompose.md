+++
title = "为什么使用 Compose ？"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/intro/features-uses/](https://docs.docker.com/compose/intro/features-uses/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Why use Compose? - 为什么使用 Compose ？

## Docker Compose 的主要优点 Key benefits of Docker Compose

Using Docker Compose offers several benefits that streamline the development, deployment, and management of containerized applications:

​	使用 Docker Compose 提供了多项优势，可简化容器化应用程序的开发、部署和管理：

- Simplified control: Docker Compose allows you to define and manage multi-container applications in a single YAML file. This simplifies the complex task of orchestrating and coordinating various services, making it easier to manage and replicate your application environment.
  - **简化管理**：Docker Compose 允许您在单个 YAML 文件中定义和管理多容器应用程序。这简化了管理和协调各种服务的复杂任务，使得管理和复制应用环境更加容易。

- Efficient collaboration: Docker Compose configuration files are easy to share, facilitating collaboration among developers, operations teams, and other stakeholders. This collaborative approach leads to smoother workflows, faster issue resolution, and increased overall efficiency.
  - **高效协作**：Docker Compose 配置文件易于共享，便于开发人员、运维团队和其他利益相关者之间的协作。此协作模式提高了工作流程效率，缩短了解决问题的时间，并提高了整体效率。

- Rapid application development: Compose caches the configuration used to create a container. When you restart a service that has not changed, Compose re-uses the existing containers. Re-using containers means that you can make changes to your environment very quickly.
  - **快速应用开发**：Compose 缓存用于创建容器的配置。如果重新启动未更改的服务，Compose 会重用现有的容器。重用容器意味着可以快速对环境进行更改。

- Portability across environments: Compose supports variables in the Compose file. You can use these variables to customize your composition for different environments, or different users.
  - **环境间的可移植性**：Compose 文件中支持变量。可以使用这些变量为不同环境或不同用户定制组合。

- Extensive community and support: Docker Compose benefits from a vibrant and active community, which means abundant resources, tutorials, and support. This community-driven ecosystem contributes to the continuous improvement of Docker Compose and helps users troubleshoot issues effectively.
  - **广泛的社区支持**：Docker Compose 得到了活跃的社区支持，资源丰富，教程众多，并提供支持。这个社区驱动的生态系统不断改进 Docker Compose，帮助用户有效地解决问题。


## Docker Compose 的常见用例 Common use cases of Docker Compose

Compose can be used in many different ways. Some common use cases are outlined below.

​	Compose 可用于多种场景。以下列出了一些常见用例。

### 开发环境 Development environments

When you're developing software, the ability to run an application in an isolated environment and interact with it is crucial. The Compose command line tool can be used to create the environment and interact with it.

​	在开发软件时，能够在隔离环境中运行应用程序并与之交互至关重要。Compose 命令行工具可用于创建环境并与之交互。

The [Compose file]({{< ref "/reference/Composefilereference" >}}) provides a way to document and configure all of the application's service dependencies (databases, queues, caches, web service APIs, etc). Using the Compose command line tool you can create and start one or more containers for each dependency with a single command (`docker compose up`).

​	[Compose 文件]({{< ref "/reference/Composefilereference" >}}) 提供了一种记录和配置所有应用程序服务依赖项（数据库、队列、缓存、Web 服务 API 等）的方法。使用 Compose 命令行工具，您可以通过单个命令 (`docker compose up`) 创建并启动每个依赖项的一个或多个容器。

Together, these features provide a convenient way for you to get started on a project. Compose can reduce a multi-page "developer getting started guide" to a single machine-readable Compose file and a few commands.

​	这些功能为项目入门提供了一种便捷方式。Compose 可以将多页的“开发人员入门指南”简化为一个可读的 Compose 文件和几个命令。

### 自动化测试环境 Automated testing environments

An important part of any Continuous Deployment or Continuous Integration process is the automated test suite. Automated end-to-end testing requires an environment in which to run tests. Compose provides a convenient way to create and destroy isolated testing environments for your test suite. By defining the full environment in a [Compose file]({{< ref "/reference/Composefilereference" >}}), you can create and destroy these environments in just a few commands:

​	持续部署或持续集成过程的重要组成部分是自动化测试套件。自动化端到端测试需要一个运行测试的环境。Compose 提供了一种方便的方式来为您的测试套件创建和销毁隔离的测试环境。通过在 [Compose 文件]({{< ref "/reference/Composefilereference" >}}) 中定义完整环境，您可以仅用几个命令创建和销毁这些环境：

```console
$ docker compose up -d
$ ./run_tests
$ docker compose down
```

### 单主机部署 Single host deployments

Compose has traditionally been focused on development and testing workflows, but with each release we're making progress on more production-oriented features.

​	虽然 Compose 传统上专注于开发和测试工作流程，但随着每次发布，我们正逐步引入更多面向生产的功能。

For details on using production-oriented features, see [Compose in production]({{< ref "/manuals/DockerCompose/How-tos/UseComposeinproduction" >}}).

​	有关使用面向生产的功能的详细信息，请参阅 [生产环境中的 Compose 使用]({{< ref "/manuals/DockerCompose/How-tos/UseComposeinproduction" >}})。

## What's next?

- [Learn about the history of Compose 了解 Compose 的历史]({{< ref "/manuals/DockerCompose/IntroductiontoCompose/Historyanddevelopment" >}})
- [Understand how Compose works 理解 Compose 的工作原理]({{< ref "/manuals/DockerCompose/IntroductiontoCompose/HowComposeworks" >}})
- [Quickstart 快速入门]({{< ref "/manuals/DockerCompose/Quickstart" >}})

