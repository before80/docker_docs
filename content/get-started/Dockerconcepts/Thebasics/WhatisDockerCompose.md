+++
title = "什么是 Docker Compose？"
date = 2024-10-23T14:54:35+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-docker-compose/](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-docker-compose/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# What is Docker Compose? - 什么是 Docker Compose？

{{< youtube "xhcUIK4fGtY">}}

## 说明 Explanation

If you've been following the guides so far, you've been working with single container applications. But, now you're wanting to do something more complicated - run databases, message queues, caches, or a variety of other services. Do you install everything in a single container? Run multiple containers? If you run multiple, how do you connect them all together?

​	如果您一直在按照之前的指南进行操作，那么您已经在处理单容器应用程序。但现在您可能希望做一些更复杂的事情，例如运行数据库、消息队列、缓存或各种其他服务。您会将所有这些服务都安装在一个容器中吗？还是运行多个容器？如果运行多个容器，您如何将它们连接在一起呢？

One best practice for containers is that each container should do one thing and do it well. While there are exceptions to this rule, avoid the tendency to have one container do multiple things.

​	对于容器的一个最佳实践是，每个容器应该只做一件事，并且做好它。虽然这个规则有一些例外，但应尽量避免让一个容器做多件事。

You can use multiple `docker run` commands to start multiple containers. But, you'll soon realize you'll need to manage networks, all of the flags needed to connect containers to those networks, and more. And when you're done, cleanup is a little more complicated.

​	您可以使用多个 `docker run` 命令来启动多个容器，但您很快就会发现，您需要管理网络、所有用于连接容器的标志等。完成后，清理起来也会稍微复杂一些。

With Docker Compose, you can define all of your containers and their configurations in a single YAML file. If you include this file in your code repository, anyone that clones your repository can get up and running with a single command.

​	通过 Docker Compose，您可以在一个 YAML 文件中定义所有容器及其配置。如果将这个文件包含在代码仓库中，任何克隆您仓库的人只需一个命令即可开始运行。

It's important to understand that Compose is a declarative tool - you simply define it and go. You don't always need to recreate everything from scratch. If you make a change, run `docker compose up` again and Compose will reconcile the changes in your file and apply them intelligently.

​	重要的是要理解，Compose 是一个声明式工具——您只需定义它并执行。您不需要每次都从头开始重建所有内容。如果您做了更改，只需再次运行 `docker compose up`，Compose 会智能地将您的文件中的更改进行对比并应用。



> **Dockerfile versus Compose file - Dockerfile 与 Compose 文件**
>
> A Dockerfile provides instructions to build a container image while a Compose file defines your running containers. Quite often, a Compose file references a Dockerfile to build an image to use for a particular service.
>
> ​	Dockerfile 提供构建容器镜像的指令，而 Compose 文件定义您的运行容器。通常情况下，Compose 文件引用 Dockerfile 以构建用于特定服务的镜像。

## 试试看 Try it out

In this hands-on, you will learn how to use a Docker Compose to run a multi-container application. You'll use a simple to-do list app built with Node.js and MySQL as a database server.

​	在本次动手操作中，您将学习如何使用 Docker Compose 来运行一个多容器应用程序。您将使用一个由 Node.js 和 MySQL 数据库服务器构建的简单待办事项列表应用。

### 启动应用程序 Start the application

Follow the instructions to run the to-do list app on your system.

​	按照以下说明在您的系统上运行待办事项列表应用。

1. [Download and install](https://www.docker.com/products/docker-desktop/) Docker Desktop. [下载并安装](https://www.docker.com/products/docker-desktop) Docker Desktop。

2. Open a terminal and [clone this sample application](https://github.com/dockersamples/todo-list-app). 打开终端并[克隆此示例应用程序](https://github.com/dockersamples/todo-list-app)。

   

   ```console
   git clone https://github.com/dockersamples/todo-list-app 
   ```

3. Navigate into the `todo-list-app` directory: 进入 `todo-list-app` 目录：

   

   ```console
   cd todo-list-app
   ```

   Inside this directory, you'll find a file named `compose.yaml`. This YAML file is where all the magic happens! It defines all the services that make up your application, along with their configurations. Each service specifies its image, ports, volumes, networks, and any other settings necessary for its functionality. Take some time to explore the YAML file and familiarize yourself with its structure.

   在此目录中，您会找到一个名为 `compose.yaml` 的文件。这个 YAML 文件就是所有的关键！它定义了组成应用程序的所有服务及其配置。每个服务都指定了其镜像、端口、卷、网络以及其他必要的设置。花点时间探索一下这个 YAML 文件，并熟悉它的结构。

4. Use the [`docker compose up`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposeup" >}}) command to start the application: 使用 [`docker compose up`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposeup" >}}) 命令启动应用程序：

   

   ```console
   docker compose up -d --build
   ```

   When you run this command, you should see an output like this:

   运行此命令后，您应该会看到如下输出：

   ```console
   [+] Running 4/4
   ✔ app 3 layers [⣿⣿⣿]      0B/0B            Pulled           7.1s
     ✔ e6f4e57cc59e Download complete                          0.9s
     ✔ df998480d81d Download complete                          1.0s
     ✔ 31e174fedd23 Download complete                          2.5s
   [+] Running 2/4
     ⠸ Network todo-list-app_default           Created         0.3s
     ⠸ Volume "todo-list-app_todo-mysql-data"  Created         0.3s
     ✔ Container todo-list-app-app-1           Started         0.3s
     ✔ Container todo-list-app-mysql-1         Started         0.3s
   ```

   A lot happened here! A couple of things to call out:

   ​	这里发生了很多事情！几个重点如下：

   - Two container images were downloaded from Docker Hub - node and MySQL
   - 两个容器镜像从 Docker Hub 下载——node 和 MySQL
   - A network was created for your application
   - 为应用程序创建了一个网络
   - A volume was created to persist the database files between container restarts
   - 创建了一个卷以在容器重启时持久化数据库文件
   - Two containers were started with all of their necessary config
   - 启动了两个容器，并且它们的所有必要配置都已经就绪

   If this feels overwhelming, don't worry! You'll get there!

   如果感觉有点复杂，不用担心！您会逐步掌握的！

5. With everything now up and running, you can open [http://localhost:3000](http://localhost:3000/) in your browser to see the site. Feel free to add items to the list, check them off, and remove them. 现在所有内容都已启动并运行，您可以在浏览器中打开 [http://localhost:3000](http://localhost:3000/) 查看网站。您可以随意添加待办事项、勾选已完成的事项并删除它们。

   ![A screenshot of a webpage showing the todo-list application running on port 3000](WhatisDockerCompose_img/todo-list-app.webp)

6. If you look at the Docker Desktop GUI, you can see the containers and dive deeper into their configuration. 如果查看 Docker Desktop GUI，您可以看到容器并深入了解它们的配置。

   ![A screenshot of Docker Desktop dashboard showing the list of containers running todo-list app](WhatisDockerCompose_img/todo-list-containers.webp)

### 关闭应用程序 Tear it down

Since this application was started using Docker Compose, it's easy to tear it all down when you're done.

​	由于该应用程序是使用 Docker Compose 启动的，因此在完成后可以轻松关闭它。

1. In the CLI, use the [`docker compose down`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposedown" >}}) command to remove everything: 在终端中，使用 [`docker compose down`]({{< ref "/reference/CLIreference/docker/dockercompose/dockercomposedown" >}}) 命令删除所有内容：

   

   ```console
   docker compose down
   ```

   You'll see output similar to the following:

   您将看到类似以下的输出：

   ```console
   [+] Running 2/2
   ✔ Container todo-list-app-mysql-1  Removed        2.9s
   ✔ Container todo-list-app-app-1    Removed        0.1s
   ✔ Network todo-list-app_default    Removed        0.1s
   ```

   > **Volume persistence 卷的持久性**
   >
   > By default, volumes *aren't* automatically removed when you tear down a Compose stack. The idea is that you might want the data back if you start the stack again.
   >
   > ​	默认情况下，当您关闭 Compose 栈时，卷不会自动删除。这样设计是为了防止数据丢失，以便您再次启动堆栈时可以恢复数据。
   >
   > If you do want to remove the volumes, add the `--volumes` flag when running the `docker compose down` command:
   >
   > ​	如果您确实想删除卷，请在运行 `docker compose down` 命令时添加 `--volumes` 标志：
   >
   > 
   >
   > ```console
   > docker compose down --volumes
   > ```

2. Alternatively, you can use the Docker Desktop GUI to remove the containers by selecting the application stack and selecting the **Delete** button. 或者，您也可以使用 Docker Desktop GUI 来删除容器，选择应用程序栈并选择 **删除** 按钮。

   ![A screenshot of the Docker Desktop GUI showing the containers view with an arrow pointing to the "Delete" button](WhatisDockerCompose_img/todo-list-delete.webp)

   > **Using the GUI for Compose stacks 在 GUI 中使用 Compose 栈**
   >
   > Note that if you remove the containers for a Compose app in the GUI, it's removing only the containers. You'll have to manually remove the network and volumes if you want to do so.
   >
   > ​	请注意，如果您在 GUI 中删除 Compose 应用的容器，它仅删除容器。如果您想删除网络和卷，必须手动删除它们。

In this walkthrough, you learned how to use Docker Compose to start and stop a multi-container application.

​	在本次操作中，您学习了如何使用 Docker Compose 启动和停止多容器应用程序。

## 其他资源 Additional resources

This page was a brief introduction to Compose. In the following resources, you can dive deeper into Compose and how to write Compose files.

​	本页面简要介绍了 Compose。以下资源将帮助您深入了解 Compose 及其文件编写方法。

- [Overview of Docker Compose]({{< ref "/manuals/DockerCompose" >}}) - [Docker Compose 概述]({{< ref "/manuals/DockerCompose" >}})

- [Overview of Docker Compose CLI](https://docs.docker.com/compose/reference/) 
- [How Compose works]({{< ref "/manuals/DockerCompose/IntroductiontoCompose/HowComposeworks" >}}) - [Compose 如何工作]({{< ref "/manuals/DockerCompose/IntroductiontoCompose/HowComposeworks" >}})
