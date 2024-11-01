+++
title = "探索 Docker Desktop"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/use-desktop/](https://docs.docker.com/desktop/use-desktop/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Explore Docker Desktop - 探索 Docker Desktop

When you open Docker Desktop, the Docker Dashboard displays.

​	打开 Docker Desktop 后，您将看到 Docker Dashboard。

![Docker Dashboard on Containers view](_index_img/dashboard.webp)

The **Containers** view provides a runtime view of all your containers and applications. It allows you to interact with containers and applications, and manage the lifecycle of your applications directly from your machine. This view also provides an intuitive interface to perform common actions to inspect, interact with, and manage your Docker objects including containers and Docker Compose-based applications. For more information, see [Explore running containers and applications]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/Containers" >}}).

​	**Containers** 视图提供了所有容器和应用程序的运行时视图。它允许您与容器和应用程序交互，并直接在机器上管理应用程序的生命周期。此视图还提供了直观的界面，用于执行常见操作以检查、交互和管理 Docker 对象（包括容器和基于 Docker Compose 的应用程序）。欲了解更多信息，请参阅 [探索运行中的容器和应用程序]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/Containers" >}})。

The **Images** view displays a list of your Docker images and allows you to run an image as a container, pull the latest version of an image from Docker Hub, and inspect images. It also displays a summary of image vulnerabilities. In addition, the **Images** view contains clean-up options to remove unwanted images from the disk to reclaim space. If you are logged in, you can also see the images you and your organization have shared on Docker Hub. For more information, see [Explore your images]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/Images" >}}).

​	**Images** 视图显示您的 Docker 镜像列表，允许您将镜像作为容器运行，从 Docker Hub 拉取镜像的最新版本，并检查镜像。此外，**Images** 视图还包含清理选项，可以从磁盘中删除不需要的镜像以释放空间。如果您已登录，您还可以看到您和您的组织在 Docker Hub 上共享的镜像。有关详细信息，请参阅 [探索您的镜像]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/Images" >}})。

The **Volumes** view displays a list of volumes and allows you to easily create and delete volumes and see which ones are being used. For more information, see [Explore volumes]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/Volumes" >}}).

​	**Volumes** 视图显示卷的列表，并允许您轻松创建和删除卷，还可以查看哪些卷正在使用中。有关更多信息，请参阅 [探索卷]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/Volumes" >}})。

The **Builds** view lets you inspect your build history and manage builders. By default, it displays a list of all your ongoing and completed builds. [Explore builds]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/Builds" >}}).

​	**Builds** 视图可让您检查构建历史记录并管理构建器。默认情况下，它会显示所有正在进行和已完成的构建的列表。[探索构建]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/Builds" >}})。

In addition, the Docker Dashboard allows you to:

​	此外，Docker Dashboard 还允许您：

- Navigate to the **Settings** menu to configure your Docker Desktop settings. Select the **Settings** icon in the Dashboard header.
  - 导航到 **Settings** 菜单以配置您的 Docker Desktop 设置。在 Dashboard 顶部选择 **Settings** 图标。

- Access the **Troubleshoot** menu to debug and perform restart operations. Select the **Troubleshoot** icon in the Dashboard header.

  - 访问 **Troubleshoot** 菜单进行调试和重启操作。在 Dashboard 顶部选择 **Troubleshoot** 图标。

- Be notified of new releases, installation progress updates, and more in the **Notifications center**. Select the bell icon in the bottom-right corner of the Docker Dashboard to access the notification center.

  - 在 **通知中心** 中接收有关新版本、安装进度更新等的通知。选择 Docker Dashboard 右下角的铃铛图标以访问通知中心。

- Access the **Learning center** from the Dashboard header. It helps you get started with quick in-app walkthroughs and provides other resources for learning about Docker. 从 Dashboard 顶部访问 **学习中心**，它可帮助您快速入门并提供有关 Docker 的其他学习资源。

  For a more detailed guide about getting started, see [Get started]({{< ref "/get-started/Introduction" >}}).

  ​	有关更详细的入门指南，请参阅 [开始使用]({{< ref "/get-started/Introduction" >}})。

- Get to the [Docker Scout]({{< ref "/manuals/DockerScout" >}}) dashboard.

  - 前往 [Docker Scout]({{< ref "/manuals/DockerScout" >}}) 仪表板。

- Check the status of Docker services.

  - 检查 Docker 服务的状态。


## 快速搜索 Quick search

From the Docker Dashboard you can use Quick Search, which is located in the Dashboard header, to search for:

​	您可以在 Docker Dashboard 顶部使用快速搜索，搜索以下内容：

- Any container or Compose application on your local system. You can see an overview of associated environment variables or perform quick actions, such as start, stop, or delete.
  - 本地系统上的任何容器或 Compose 应用程序。您可以查看相关环境变量概览或执行快速操作，如启动、停止或删除。

- Public Docker Hub images, local images, and images from remote repositories (private repositories from organizations you're a part of in Hub). Depending on the type of image you select, you can either pull the image by tag, view documentation, go to Docker Hub for more details, or run a new container using the image.
  - 公共 Docker Hub 镜像、本地镜像和远程存储库（您所属组织的私有存储库）中的镜像。根据选择的镜像类型，您可以按标签拉取镜像、查看文档、转到 Docker Hub 了解更多信息，或使用镜像运行新容器。

- Extensions. From here, you can learn more about the extension and install it with a single click. Or, if you already have an extension installed, you can open it straight from the search results.
  - 扩展。从这里，您可以了解更多有关扩展的信息并一键安装。如果您已安装了扩展，可以直接从搜索结果中打开它。

- Any volume. From here you can view the associated container.
  - 任何卷。您可以查看关联的容器。

- Docs. Find help from Docker's official documentation straight from Docker Desktop.
  - 文档。直接从 Docker Desktop 中查找 Docker 的官方文档。


## Docker 菜单 The Docker menu

Docker Desktop also provides an easy-access tray icon that appears in the taskbar and is referred to as the Docker menu ![whale menu](_index_img/whale-x.svg) .

​	Docker Desktop 还提供了易于访问的托盘图标，显示在任务栏中，称为 Docker 菜单 ![whale menu](_index_img/whale-x.svg)

To display the Docker menu, select the ![whale menu](_index_img/whale-x.svg) icon. It displays the following options:

​	选择图标![whale menu](_index_img/whale-x.svg)以显示 Docker 菜单，提供以下选项：

- **Dashboard**. This takes you to the Docker Dashboard. **Dashboard**：跳转到 Docker Dashboard。
- **Sign in/Sign up**
- **Settings**
- **Check for updates**
- **Troubleshoot**
- **Give feedback**
- **Switch to Windows containers** (if you're on Windows) （如果您在 Windows 上）
- **About Docker Desktop**. Contains information on the versions you are running, and links to the Subscription Service Agreement for example. 包含有关您当前运行的版本的信息，以及订阅服务协议的链接。
- **Docker Hub**
- **Documentation**
- **Extensions**
- **Kubernetes**
- **Restart**
- **Quit Docker Desktop**
