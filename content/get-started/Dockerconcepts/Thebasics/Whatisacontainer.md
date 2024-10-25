+++
title = "什么是容器？"
date = 2024-10-23T14:54:35+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# What is a container? - 什么是容器？

{{< youtube "W1kWqFkiu7k">}}

## 说明 Explanation

Imagine you're developing a killer web app that has three main components - a React frontend, a Python API, and a PostgreSQL database. If you wanted to work on this project, you'd have to install Node, Python, and PostgreSQL.

​	想象一下，您正在开发一个出色的 web 应用，它有三个主要组件——一个 React 前端、一个 Python API 和一个 PostgreSQL 数据库。如果您想在这个项目上工作，您必须安装 Node、Python 和 PostgreSQL。

How do you make sure you have the same versions as the other developers on your team? Or your CI/CD system? Or what's used in production?

​	如何确保您和团队中的其他开发人员、CI/CD 系统以及生产环境使用的版本是相同的？

How do you ensure the version of Python (or Node or the database) your app needs isn't affected by what's already on your machine? How do you manage potential conflicts?

​	如何确保您的应用所需的 Python（或 Node 或数据库）版本不会受到您机器上已安装内容的影响？如何处理可能的冲突？

Enter containers!

​	容器来了！

What is a container? Simply put, containers are isolated processes for each of your app's components. Each component - the frontend React app, the Python API engine, and the database - runs in its own isolated environment, completely isolated from everything else on your machine.

​	什么是容器？简单来说，容器是您应用的每个组件的隔离进程。每个组件——前端 React 应用、Python API 引擎和数据库——都在其自己的隔离环境中运行，与机器上的其他内容完全隔离。

Here's what makes them awesome. Containers are:

​	容器的强大之处在于：

- Self-contained. Each container has everything it needs to function with no reliance on any pre-installed dependencies on the host machine.
- **自包含**。每个容器都包含其运行所需的一切，不依赖于主机机器上预先安装的任何依赖项。

- Isolated. Since containers are run in isolation, they have minimal influence on the host and other containers, increasing the security of your applications.
- **隔离**。由于容器在隔离环境中运行，它们对主机和其他容器的影响最小，增强了应用程序的安全性。
- Independent. Each container is independently managed. Deleting one container won't affect any others.
- **独立**。每个容器都是独立管理的。删除一个容器不会影响其他容器。
- Portable. Containers can run anywhere! The container that runs on your development machine will work the same way in a data center or anywhere in the cloud!
- **可移植**。容器可以在任何地方运行！在您的开发机器上运行的容器在数据中心或云端都能以相同方式运行。

### 容器 vs 虚拟机（VM） Containers versus virtual machines (VMs)

Without getting too deep, a VM is an entire operating system with its own kernel, hardware drivers, programs, and applications. Spinning up a VM only to isolate a single application is a lot of overhead.

​	简单来说，虚拟机是一个完整的操作系统，带有自己的内核、硬件驱动程序、程序和应用程序。启动一个虚拟机来隔离单个应用程序需要大量的开销。

A container is simply an isolated process with all of the files it needs to run. If you run multiple containers, they all share the same kernel, allowing you to run more applications on less infrastructure.

​	而容器只是一个隔离的进程，包含其运行所需的所有文件。多个容器共享相同的内核，因此可以在更少的基础设施上运行更多应用程序。

> **Using VMs and containers together** 一起使用虚拟机和容器
>
> Quite often, you will see containers and VMs used together. As an example, in a cloud environment, the provisioned machines are typically VMs. However, instead of provisioning one machine to run one application, a VM with a container runtime can run multiple containerized applications, increasing resource utilization and reducing costs.
>
> ​	通常，您会看到容器和虚拟机一起使用。例如，在云环境中，预配的机器通常是虚拟机。但是，与其预配一台机器运行一个应用程序，不如使用带有容器运行时的虚拟机来运行多个容器化应用程序，以提高资源利用率并降低成本。

## 试试看 Try it out

In this hands-on, you will see how to run a Docker container using the Docker Desktop GUI.

​	在本次动手操作中，您将学习如何使用 Docker Desktop 图形用户界面（GUI）运行 Docker 容器。

{{< tabpane text=true persist=disabled >}}

{{% tab header="Using the GUI " %}}

Use the following instructions to run a container.

​	按以下步骤运行容器。

1. Open Docker Desktop and select the **Search** field on the top navigation bar. 打开 Docker Desktop 并在顶部导航栏中选择 **搜索** 字段。

2. Specify `welcome-to-docker` in the search input and then select the **Pull** button. 在搜索输入框中指定 `welcome-to-docker`，然后选择 **Pull** 按钮。

   ![A screenshot of the Docker Dashboard showing the search result for welcome-to-docker Docker image ](Whatisacontainer_img/search-the-docker-image.webp)

3. Once the image is successfully pulled, select the **Run** button. 镜像成功拉取后，选择 **Run** 按钮。

4. Expand the **Optional settings**. 展开 **可选设置**。

5. In the **Container name**, specify `welcome-to-docker`. 在 **容器名称** 中，指定 `welcome-to-docker`。

6. In the **Host port**, specify `8080`. 在 **主机端口** 中，指定 `8080`。

   ![A screenshot of Docker Dashboard showing the container run dialog with welcome-to-docker typed in as the container name and 8080 specified as the port number](Whatisacontainer_img/run-a-new-container.webp)

7. Select **Run** to start your container. 选择 **Run** 启动容器。

Congratulations! You just ran your first container! 🎉 

​	恭喜！您刚刚运行了您的第一个容器！🎉

{{% /tab  %}}

{{% tab header="Using the CLI" %}}

Follow the instructions to run a container using the CLI:

​	按以下步骤通过 CLI 运行容器：

1. Open your CLI terminal and start a container by using the [`docker run`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerrun" >}}) command: 打开终端并使用 [`docker run`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerrun" >}}) 命令启动容器：

   

   ```console
   $ docker run -d -p 8080:80 docker/welcome-to-docker
   ```

   The output from this command is the full container ID.
   
   此命令的输出为完整的容器 ID。

Congratulations! You just fired up your first container! 🎉

​	恭喜！您已经启动了您的第一个容器！🎉

{{% /tab  %}}

{{< /tabpane >}}





### 查看您的容器 View your container

You can view all of your containers by going to the **Containers** view of the Docker Dashboard.

​	您可以通过 Docker 仪表板的 **Containers** 视图查看所有容器。

![Screenshot of the container view of the Docker Desktop GUI showing the welcome-to-docker container running on the host port 8080](Whatisacontainer_img/view-your-containers.webp)

This container runs a web server that displays a simple website. When working with more complex projects, you'll run different parts in different containers. For example, you might run a different container for the frontend, backend, and database.

​	该容器运行一个 Web 服务器，显示一个简单的网站。在处理更复杂的项目时，您会在不同的容器中运行不同的部分。例如，您可能会为前端、后端和数据库分别运行不同的容器。

### 访问前端 Access the frontend

When you launched the container, you exposed one of the container's ports onto your machine. Think of this as creating configuration to let you to connect through the isolated environment of the container.

​	启动容器时，您将容器的一个端口暴露到您的机器上。这可以视为配置，允许您通过容器的隔离环境进行连接。

For this container, the frontend is accessible on port `8080`. To open the website, select the link in the **Port(s)** column of your container or visit [http://localhost:8080](https://localhost:8080/) in your browser.

​	对于此容器，前端可通过端口 `8080` 访问。要打开网站，请选择容器 **端口** 列中的链接，或在浏览器中访问 [http://localhost:8080](https://localhost:8080/)。

![Screenshot of the landing page coming from the running container](Whatisacontainer_img/access-the-frontend.webp)

### 探索您的容器 Explore your container

Docker Desktop lets you explore and interact with different aspects of your container. Try it out yourself.

​	Docker Desktop 允许您探索、与容器的不同方面进行交互。试试看。



1. Go to the **Containers** view in the Docker Dashboard. 在 Docker 仪表板中进入 **Containers** 视图。

2. Select your container. 选择您的容器。

3. Select the **Files** tab to explore your container's isolated file system. 选择 **Files** 选项卡以探索容器的隔离文件系统。

   ![Screenshot of the Docker Dashboard showing the files and directories inside a running container](Whatisacontainer_img/explore-your-container.webp)

### 停止您的容器 Stop your container

The `docker/welcome-to-docker` container continues to run until you stop it.

​	`docker/welcome-to-docker` 容器会持续运行，直到您将其停止。

1. Go to the **Containers** view in the Docker Dashboard. 进入 Docker 仪表板中的 **Containers** 视图。

2. Locate the container you'd like to stop. 找到您想停止的容器。

3. Select the **Stop** action in the **Actions** column. 在 **操作** 列中选择 **Stop** 操作。

   ![Screenshot of the Docker Dashboard with the welcome container selected and being prepared to stop](Whatisacontainer_img/stop-your-container.webp)

------

## 其他资源 Additional resources

The following links provide additional guidance into containers:

​	以下链接提供了关于容器的更多指导：

- [Running a container]({{< ref "/manuals/DockerEngine/Containers/Runningcontainers" >}}) [运行容器]({{< ref "/manuals/DockerEngine/Containers/Runningcontainers" >}})

- [Overview of container 容器概述](https://www.docker.com/resources/what-container/) 
- [Why Docker? 为什么选择 Docker？](https://www.docker.com/why-docker/)

## 接下来 Next steps

Now that you have learned the basics of a Docker container, it's time to learn about Docker images.

​	现在您已经了解了 Docker 容器的基础知识，是时候学习 Docker 镜像了。

[What is an image?]({{< ref "/get-started/Dockerconcepts/Thebasics/Whatisanimage" >}}) [什么是镜像？]({{< ref "/get-started/Dockerconcepts/Thebasics/Whatisanimage" >}})
