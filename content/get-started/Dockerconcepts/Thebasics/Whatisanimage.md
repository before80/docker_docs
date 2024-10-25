+++
title = "什么是镜像？"
date = 2024-10-23T14:54:35+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-an-image/](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-an-image/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# What is an image? - 什么是镜像？

{{< youtube "NyvT9REqLe4">}}

## 说明 Explanation

Seeing a [container]({{< ref "/get-started/Dockerconcepts/Thebasics/Whatisacontainer" >}}) is an isolated process, where does it get its files and configuration? How do you share those environments?

​	既然[容器]({{< ref "/get-started/Dockerconcepts/Thebasics/Whatisacontainer" >}})是一个隔离的进程，那么它的文件和配置从哪里来？如何分享这些环境？

That's where container images come in!

​	这就是容器镜像的作用！

A container image is a standardized package that includes all of the files, binaries, libraries, and configurations to run a container.

​	容器镜像是一个标准化的包，包含运行容器所需的所有文件、二进制文件、库和配置。

For a [PostgreSQL](https://hub.docker.com/_/postgres) image, that image will package the database binaries, config files, and other dependencies. For a Python web app, it'll include the Python runtime, your app code, and all of its dependencies.

​	例如，[PostgreSQL](https://hub.docker.com/_/postgres) 镜像会打包数据库二进制文件、配置文件和其他依赖项。对于一个 Python Web 应用程序，它会包含 Python 运行时、应用代码及其所有依赖项。

There are two important principles of images:

​	镜像有两个重要原则：

1. Images are immutable. Once an image is created, it can't be modified. You can only make a new image or add changes on top of it. 镜像是不可变的。一旦创建镜像，它不能被修改。您只能创建新的镜像或在其基础上添加更改。
2. Container images are composed of layers. Each layer represented a set of file system changes that add, remove, or modify files. 容器镜像由多个层组成。每一层表示一组文件系统的更改，用于添加、删除或修改文件。

These two principles let you to extend or add to existing images. For example, if you are building a Python app, you can start from the [Python image](https://hub.docker.com/_/python) and add additional layers to install your app's dependencies and add your code. This lets you focus on your app, rather than Python itself.

​	这两个原则让您可以扩展或添加现有镜像。例如，如果您在构建一个 Python 应用程序，您可以从 [Python 镜像](https://hub.docker.com/_/python) 开始，并添加额外的层来安装应用程序的依赖项和添加代码。这让您可以专注于您的应用，而不是 Python 本身。

### 查找镜像 Finding images

[Docker Hub](https://hub.docker.com/) is the default global marketplace for storing and distributing images. It has over 100,000 images created by developers that you can run locally. You can search for Docker Hub images and run them directly from Docker Desktop.

​	[Docker Hub](https://hub.docker.com/) 是存储和分发镜像的默认全球市场。它拥有超过 100,000 个由开发者创建的镜像，您可以在本地运行它们。您可以直接从 Docker Desktop 搜索 Docker Hub 上的镜像并运行它们。

Docker Hub provides a variety of Docker-supported and endorsed images known as Docker Trusted Content. These provide fully managed services or great starters for your own images. These include:

​	Docker Hub 提供了多种受 Docker 支持和认证的镜像，称为 Docker Trusted Content。这些提供了完全托管的服务或是您自己镜像的优秀起点。包括：

- [Docker Official Images](https://hub.docker.com/search?q=&type=image&image_filter=official) - a curated set of Docker repositories, serve as the starting point for the majority of users, and are some of the most secure on Docker Hub [Docker 官方镜像](https://hub.docker.com/search?q=&type=image&image_filter=official) - 一个精心挑选的 Docker 仓库集，作为大多数用户的起点，并且是 Docker Hub 上最安全的镜像之一。

- [Docker Verified Publishers](https://hub.docker.com/search?q=&image_filter=store) - high-quality images from commercial publishers verified by Docker [Docker 认证发布者](https://hub.docker.com/search?q=&image_filter=store) - 经 Docker 验证的商业发布者提供的高质量镜像。
- [Docker-Sponsored Open Source](https://hub.docker.com/search?q=&image_filter=open_source) - images published and maintained by open-source projects sponsored by Docker through Docker's open source program [Docker 赞助的开源项目](https://hub.docker.com/search?q=&image_filter=open_source) - 由开源项目发布并维护，Docker 通过其开源计划赞助这些项目。

For example, [Redis](https://hub.docker.com/_/redis) and [Memcached](https://hub.docker.com/_/memcached) are a few popular ready-to-go Docker Official Images. You can download these images and have these services up and running in a matter of seconds. There are also base images, like the [Node.js](https://hub.docker.com/_/node) Docker image, that you can use as a starting point and add your own files and configurations.

​	例如，[Redis](https://hub.docker.com/_/redis) 和 [Memcached](https://hub.docker.com/_/memcached) 是几个非常受欢迎的 Docker 官方镜像。您可以下载这些镜像，并在几秒钟内启动这些服务。还有基础镜像，例如 [Node.js](https://hub.docker.com/_/node) Docker 镜像，您可以将其作为起点，并添加您自己的文件和配置。

## 试试看 Try it out

{{< tabpane text=true persist=disabled >}}

{{% tab header="Using the GUI" %}}

In this hands-on, you will learn how to search and pull a container image using the Docker Desktop GUI.

​	在本次动手操作中，您将学习如何使用 Docker Desktop GUI 搜索并拉取容器镜像。

### 搜索并下载镜像 Search for and download an image

1. Open the Docker Dashboard and select the **Images** view in the left-hand navigation menu. 打开 Docker 仪表盘，在左侧导航菜单中选择 **镜像** 视图。

   ![A screenshot of the Docker Dashboard showing the image view on the left sidebar](Whatisanimage_img/click-image.webp)

2. Select the **Search images to run** button. If you don't see it, select the *global search bar* at the top of the screen. 选择 **搜索镜像以运行** 按钮。如果看不到它，请选择屏幕顶部的 *全局搜索栏*。

   ![A screenshot of the Docker Dashboard showing the search ta](Whatisanimage_img/search-image.webp)

3. In the **Search** field, enter "welcome-to-docker". Once the search has completed, select the `docker/welcome-to-docker` image. 在 **搜索** 字段中输入 "welcome-to-docker"。搜索完成后，选择 `docker/welcome-to-docker` 镜像。

   ![A screenshot of the Docker Dashboard showing the search results for the docker/welcome-to-docker image](Whatisanimage_img/select-image.webp)

4. Select **Pull** to download the image. 选择 **Pull** 下载镜像。

{{% /tab  %}}

{{% tab header="Using the CLI" %}}

Follow the instructions to search and pull a Docker image using CLI to view its layers.

​	按照说明使用 CLI 搜索并拉取 Docker 镜像并查看其层。

### 搜索并下载镜像 Search for and download an image

1. Open a terminal and search for images using the [`docker search`]({{< ref "/reference/CLIreference/docker/dockersearch" >}}) command: 打开终端，使用 [`docker search`]({{< ref "/reference/CLIreference/docker/dockersearch" >}}) 命令搜索镜像：

   

   ```console
   docker search docker/welcome-to-docker
   ```

   You will see output like the following:

   您将看到类似以下的输出：

   ```console
   NAME                       DESCRIPTION                                     STARS     OFFICIAL
   docker/welcome-to-docker   Docker image for new users getting started w…   20
   ```

   This output shows you information about relevant images available on Docker Hub.

   该输出为您显示了 Docker Hub 上可用的相关镜像信息。

2. Pull the image using the [`docker pull`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerpull" >}}) command. 使用 [`docker pull`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerpull" >}}) 命令拉取镜像。

   

   ```console
   docker pull docker/welcome-to-docker
   ```

   You will see output like the following:

   您将看到类似以下的输出：

   ```console
   Using default tag: latest
   latest: Pulling from docker/welcome-to-docker
   579b34f0a95b: Download complete
   d11a451e6399: Download complete
   1c2214f9937c: Download complete
   b42a2f288f4d: Download complete
   54b19e12c655: Download complete
   1fb28e078240: Download complete
   94be7e780731: Download complete
   89578ce72c35: Download complete
   Digest: sha256:eedaff45e3c78538087bdd9dc7afafac7e110061bbdd836af4104b10f10ab693
   Status: Downloaded newer image for docker/welcome-to-docker:latest
   docker.io/docker/welcome-to-docker:latest
   ```

   Each of line represents a different downloaded layer of the image. Remember that each layer is a set of filesystem changes and provides functionality of the image.
   
   每一行代表镜像中下载的不同层。请记住，每一层都是一组文件系统更改，并提供镜像的功能。

{{% /tab  %}}

{{< /tabpane >}}

### 了解镜像 Learn about the image

Once you have an image downloaded, you can learn quite a few details about the image either through the GUI or the CLI.

​	下载镜像后，您可以通过 GUI 或 CLI 了解镜像的许多详细信息。

1. In the Docker Dashboard, select the **Images** view. 在 Docker 仪表盘中选择 **镜像** 视图。

2. Select the **docker/welcome-to-docker** image to open details about the image. 选择 **docker/welcome-to-docker** 镜像以打开镜像详情。

   ![A screenshot of the Docker Dashboard showing the images view with an arrow pointing to the docker/welcome-to-docker image](Whatisanimage_img/pulled-image.webp)

3. The image details page presents you with information regarding the layers of the image, the packages and libraries installed in the image, and any discovered vulnerabilities. 镜像详情页面向您展示有关镜像层、安装的软件包和库以及发现的任何漏洞的信息。

   ![A screenshot of the image details view for the docker/welcome-to-docker image](Whatisanimage_img/image-layers.webp)

------

In this walkthrough, you searched and pulled a Docker image. In addition to pulling a Docker image, you also learned about the layers of a Docker Image.

​	在本次操作中，您搜索并拉取了 Docker 镜像。除了拉取 Docker 镜像，您还学习了关于 Docker 镜像层的知识。

## 其他资源 Additional resources

The following resources will help you learn more about exploring, finding, and building images:

- Docker Trusted Content Docker 可信内容
  - [Docker Official Images docs]({{< ref "/manuals/Trustedcontent/DockerOfficialImages" >}}) [Docker 官方镜像文档]({{< ref "/manuals/Trustedcontent/DockerOfficialImages" >}})
  - [Docker Verified Publisher docs]({{< ref "/manuals/Trustedcontent/DockerVerifiedPublisherProgram" >}}) [Docker 认证发布者文档]({{< ref "/manuals/Trustedcontent/DockerVerifiedPublisherProgram" >}})
  - [Docker-Sponsored Open Source Program docs]({{< ref "/manuals/Trustedcontent/Docker-SponsoredOpenSourceProgram" >}}) [Docker 赞助的开源项目文档]({{< ref "/manuals/Trustedcontent/Docker-SponsoredOpenSourceProgram" >}})
- [Explore the Image view in Docker Desktop]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/Images" >}}) [探索 Docker Desktop 中的镜像视图]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/Images" >}})
- [Packaging your software 打包您的软件](https://docs.docker.com/build/building/packaging/)
- [Docker Hub](https://hub.docker.com/)

## 接下来 Next steps

Now that you have learned the basics of images, it's time to learn about distributing images through registries.

​	现在您已经了解了镜像的基础知识，是时候学习通过注册表分发镜像了。

[What is a registry?]({{< ref "/get-started/Dockerconcepts/Thebasics/Whatisaregistry" >}}) [什么是注册表？]({{< ref "/get-started/Dockerconcepts/Thebasics/Whatisaregistry" >}})

