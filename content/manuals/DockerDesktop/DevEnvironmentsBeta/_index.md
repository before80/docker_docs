+++
title = "开发环境 (Beta)"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/dev-environments/](https://docs.docker.com/desktop/dev-environments/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Overview of Dev Environments - 开发环境概述

> **Important**
>
> 
>
> Dev Environments is no longer under active development.
>
> ​	开发环境不再进行活跃开发。
>
> While the current functionality remains available, it may take us longer to respond to support requests.
>
> ​	尽管当前功能仍然可用，但我们可能会对支持请求的响应时间更长。

**Beta**

The Dev Environments feature is currently in [Beta](https://docs.docker.com/release-lifecycle/#beta).

​	开发环境功能目前处于 [Beta](https://docs.docker.com/release-lifecycle/#beta) 阶段。

Dev Environments let you create a configurable developer environment with all the code and tools you need to quickly get up and running.

​	开发环境允许您创建一个可配置的开发环境，包含所有代码和工具，使您可以快速启动和运行。

It uses tools built into code editors that allows Docker to access code mounted into a container rather than on your local host. This isolates the tools, files and running services on your machine allowing multiple versions of them to exist side by side.

​	它使用代码编辑器中的工具，使 Docker 可以访问挂载到容器中的代码，而不是在本地主机上。这样可以隔离工具、文件和运行服务，允许多个版本并行存在于同一系统中。

You can use Dev Environments through the intuitive GUI in Docker Dashboard or straight from your terminal with the new [`docker dev` CLI plugin]({{< ref "/manuals/DockerDesktop/DevEnvironmentsBeta/UsethedockerdevCLIplugin" >}}).

​	您可以通过 Docker Dashboard 中直观的 GUI 或直接通过新的 [`docker dev` CLI 插件]({{< ref "/manuals/DockerDesktop/DevEnvironmentsBeta/UsethedockerdevCLIplugin" >}}) 使用开发环境。

## 使用开发环境 Use Dev Environments

To use Dev Environments:

​	要使用开发环境：

1. Navigate to the **Features in Development** tab in **Settings**. 导航至 **设置** 中的 **Features in Development** 选项卡。
2. On the **Beta** tab, select **Turn on Dev Environments**. 在 **Beta** 选项卡中，选择 **开启开发环境**。
3. Select **Apply & restart**. 选择 **应用并重启**。

The Dev Environments tab is now visible in Docker Dashboard.

​	Docker Dashboard 中现在可以看到开发环境选项卡。

## 它是如何工作的？How does it work?

> **Changes to Dev Environments with Docker Desktop 4.13 - Docker Desktop 4.13 版本中的开发环境更改**
>
> Docker has simplified how you configure your dev environment project. All you need to get started is a `compose-dev.yaml` file. If you have an existing project with a `.docker/` folder this is automatically migrated the next time you launch.
>
> ​	Docker 简化了开发环境项目的配置。您只需一个 `compose-dev.yaml` 文件即可开始。如果您已有包含 `.docker/` 文件夹的项目，在下一次启动时会自动迁移。

Dev Environments is powered by [Docker Compose]({{< ref "/manuals/DockerCompose" >}}). This allows Dev Environments to take advantage of all the benefits and features of Compose whilst adding an intuitive GUI where you can launch environments with the click of a button.

​	开发环境由 [Docker Compose]({{< ref "/manuals/DockerCompose" >}}) 提供支持。这使开发环境可以利用 Compose 的所有优势和功能，并增加一个直观的 GUI，您只需单击按钮即可启动环境。

Every dev environment you want to run needs a `compose-dev.yaml` file which configures your application's services and lives in your project directory. You don't need to be an expert in Docker Compose or write a `compose-dev.yaml` file from scratch as Dev Environments creates a starter `compose-dev.yaml` files based on the main language in your project.

​	每个您想要运行的开发环境都需要一个 `compose-dev.yaml` 文件，用于配置应用程序的服务，并存放在您的项目目录中。您无需成为 Docker Compose 专家，也无需从头编写 `compose-dev.yaml` 文件，因为开发环境会根据您的项目主语言创建初始的 `compose-dev.yaml` 文件。

You can also use the many [sample dev environments](https://github.com/docker/awesome-compose) as a starting point for how to integrate different services. Alternatively, see [Set up a dev environment]({{< ref "/manuals/DockerDesktop/DevEnvironmentsBeta/Setupadevenvironment" >}}) for more information.

​	您还可以使用许多 [示例开发环境](https://github.com/docker/awesome-compose) 作为不同服务集成的起点。或者，参阅[设置开发环境]({{< ref "/manuals/DockerDesktop/DevEnvironmentsBeta/Setupadevenvironment" >}}) 以获取更多信息。

## What's next?

Learn how to:

​	了解如何：

- [Launch a dev environment 启动开发环境]({{< ref "/manuals/DockerDesktop/DevEnvironmentsBeta/Launchadevenvironment" >}})
- [Set up a dev environment 设置开发环境]({{< ref "/manuals/DockerDesktop/DevEnvironmentsBeta/Setupadevenvironment" >}})
- [Distribute your dev environment 分发您的开发环境]({{< ref "/manuals/DockerDesktop/DevEnvironmentsBeta/Distributeyourdevenvironment" >}})
