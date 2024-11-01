+++
title = "镜像"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/desktop/use-desktop/images/](https://docs.docker.com/desktop/use-desktop/images/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Explore the Images view in Docker Desktop - 探索 Docker Desktop 中的镜像视图

The **Images** view lets you manage Docker images without having to use the CLI. By default, it displays a list of all Docker images on your local disk.

​	**镜像** 视图允许您在不使用 CLI 的情况下管理 Docker 镜像。默认情况下，它会显示本地磁盘上的所有 Docker 镜像列表。

You can also view Hub images once you have signed in to Docker Hub. This allows you to collaborate with your team and manage your images directly through Docker Desktop.

​	登录 Docker Hub 后，您还可以查看 Hub 中的镜像。这使您可以与团队协作并直接通过 Docker Desktop 管理您的镜像。

The **Images** view lets you perform core operations such as running an image as a container, pulling the latest version of an image from Docker Hub, pushing the image to Docker Hub, and inspecting images.

​	**镜像** 视图让您可以执行核心操作，例如将镜像作为容器运行、从 Docker Hub 拉取镜像的最新版本、将镜像推送到 Docker Hub，以及检查镜像。

It also displays metadata about the image such as the:

​	它还显示有关镜像的元数据，例如：

- Tag
- Image ID
- Date created
- Size of the image.

An **In Use** tag displays next to images used by running and stopped containers. You can choose what information you want displayed by selecting the **More options** menu to the right of the search bar, and then use the toggle switches according to your preferences.

​	**使用中** 标签会显示在被运行或停止的容器使用的镜像旁。您可以通过选择搜索栏右侧的 **更多选项** 菜单，并根据您的偏好使用切换开关来选择显示的信息。

The **Images on disk** status bar displays the number of images and the total disk space used by the images and when this information was last refreshed.

​	**磁盘上的镜像** 状态栏显示镜像数量、镜像所用的总磁盘空间，以及上次刷新信息的时间。

## 管理您的镜像 Manage your images

Use the **Search** field to search for any specific image.

​	使用 **Search** 字段搜索特定镜像。

You can sort images by:

​	您可以按以下方式对镜像进行排序：

- In use 使用中
- Unused 未使用
- Dangling 悬挂状态

## 将镜像作为容器运行 Run an image as a container

From the **Images view**, hover over an image and select **Run**.

​	在 **镜像视图** 中，悬停在镜像上并选择 **运行**。

When prompted you can either:

​	在提示时，您可以：

- Select the **Optional settings** drop-down to specify a name, port, volumes, environment variables and select **Run**
  - 选择 **可选设置** 下拉菜单来指定名称、端口、卷、环境变量，然后选择 **运行**

- Select **Run** without specifying any optional settings.
  - 直接选择 **运行**，不指定任何可选设置。


## 检查镜像 Inspect an image

To inspect an image, select the image row. Inspecting an image displays detailed information about the image such as the:

​	要检查镜像，请选择镜像行。检查镜像时，会显示有关镜像的详细信息，例如：

- Image history
- Image ID
- Date the image was created
- Size of the image
- Layers making up the image
- Base images used
- Vulnerabilities found
- Packages inside the image

[Docker Scout]({{< ref "/manuals/DockerScout" >}}) powers this vulnerability information. For more information about this view, see [Image details view]({{< ref "/manuals/DockerScout/Explore/Imagedetailsview" >}})

​	[Docker Scout]({{< ref "/manuals/DockerScout" >}}) 提供了该漏洞信息。有关此视图的更多信息，请参见 [镜像详细信息视图]({{< ref "/manuals/DockerScout/Explore/Imagedetailsview" >}})。

## 从 Docker Hub 拉取最新镜像 Pull the latest image from Docker Hub

Select the image from the list, select the **More options** button and select **Pull**.

​	从列表中选择镜像，点击 **更多选项** 按钮，然后选择 **拉取**。

> **Note**
>
> 
>
> The repository must exist on Docker Hub in order to pull the latest version of an image. You must be signed in to pull private images.
>
> ​	要拉取镜像的最新版本，该镜像的仓库必须在 Docker Hub 上存在。您必须登录才能拉取私有镜像。

## 将镜像推送到 Docker Hub - Push an image to Docker Hub

Select the image from the list, select the **More options** button and select **Push to Hub**.

​	从列表中选择镜像，点击 **更多选项** 按钮，然后选择 **推送到 Hub**。

> **Note**
>
> 
>
> You can only push an image to Docker Hub if the image belongs to your Docker ID or your organization. That is, the image must contain the correct username/organization in its tag to be able to push it to Docker Hub.
>
> ​	只有在镜像属于您的 Docker ID 或组织时，您才能将其推送到 Docker Hub。也就是说，镜像必须在其标签中包含正确的用户名/组织名称，才能将其推送到 Docker Hub。

## 删除镜像 Remove an image

> **Note**
>
> 
>
> To remove an image used by a running or a stopped container, you must first remove the associated container.
>
> ​	要删除由正在运行或已停止的容器使用的镜像，必须先删除关联的容器。

An unused image is an image which is not used by any running or stopped containers. An image becomes dangling when you build a new version of the image with the same tag.

​	未使用的镜像是不被任何运行或停止的容器使用的镜像。当您使用相同的标签构建镜像的新版本时，镜像会变为悬挂状态。

To remove individual images, select the bin icon.

​	要删除单个镜像，请选择垃圾桶图标。

## Docker Hub 仓库 Docker Hub repositories

The **Images** view also allows you to manage and interact with images in Docker Hub repositories. By default, when you go to **Images** in Docker Desktop, you see a list of images that exist in your local image store. The **Local** and **Hub** tabs near the top toggles between viewing images in your local image store, and images in remote Docker Hub repositories that you have access to.

​	**镜像** 视图还允许您管理和与 Docker Hub 仓库中的镜像进行交互。默认情况下，当您进入 Docker Desktop 中的 **镜像** 时，会看到本地镜像存储中存在的镜像列表。顶部的 **本地** 和 **Hub** 选项卡在查看本地镜像存储中的镜像和您有访问权限的远程 Docker Hub 仓库中的镜像之间切换。

Switching to the **Hub** tab prompts you to sign in to your Docker Hub account, if you're not already signed in. When signed in, it shows you a list of images in Docker Hub organizations and repositories that you have access to.

​	切换到 **Hub** 选项卡会提示您登录 Docker Hub 账户（如果尚未登录）。登录后，它会显示您有访问权限的 Docker Hub 组织和仓库中的镜像列表。

Select an organization from the drop-down to view a list of repositories for that organization.

​	从下拉菜单中选择一个组织即可查看该组织的仓库列表。

If you have enabled [Docker Scout]({{< ref "/manuals/DockerScout" >}}) on the repositories, image analysis results appear next to the image tags.

​	如果您已在仓库上启用 [Docker Scout]({{< ref "/manuals/DockerScout" >}})，镜像分析结果将显示在镜像标签旁。

Hovering over an image tag reveals two options:

​	将鼠标悬停在镜像标签上会显示两个选项：

- **Pull**: Pull the latest version of the image from Docker Hub.
  - **拉取**：从 Docker Hub 拉取镜像的最新版本。

- **View in Hub**: Open the Docker Hub page and display detailed information about the image.
  - **在 Hub 中查看**：打开 Docker Hub 页面并显示有关该镜像的详细信息。


## 其他资源 Additional resources

- [What is an image? 什么是镜像？]({{< ref "/get-started/Dockerconcepts/Thebasics/Whatisanimage" >}})
