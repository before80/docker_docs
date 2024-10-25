+++
title = "多容器应用"
date = 2024-10-23T14:54:35+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/get-started/docker-concepts/running-containers/multi-container-applications/](https://docs.docker.com/get-started/docker-concepts/running-containers/multi-container-applications/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# Multi-container applications - 多容器应用

{{< youtube "1jUwR6F9hvM">}}

## 说明 Explanation

Starting up a single-container application is easy. For example, a Python script that performs a specific data processing task runs within a container with all its dependencies. Similarly, a Node.js application serving a static website with a small API endpoint can be effectively containerized with all its necessary libraries and dependencies. However, as applications grow in size, managing them as individual containers becomes more difficult.

​	启动单容器应用程序非常简单。例如，一个执行特定数据处理任务的 Python 脚本可以在一个包含所有依赖项的容器中运行。同样，一个 Node.js 应用程序可以通过容器化提供一个静态网站和小型 API 端点，并包含所有必要的库和依赖项。然而，随着应用程序规模的增长，将它们作为单独的容器进行管理变得越来越困难。

Imagine the data processing Python script needs to connect to a database. Suddenly, you're now managing not just the script but also a database server within the same container. If the script requires user logins, you'll need an authentication mechanism, further bloating the container size.

​	想象一下，这个数据处理的 Python 脚本需要连接到一个数据库。突然之间，你不仅要管理脚本，还要在同一个容器中管理一个数据库服务器。如果脚本需要用户登录，你还需要一个认证机制，这会进一步增加容器的体积。

One best practice for containers is that each container should do one thing and do it well. While there are exceptions to this rule, avoid the tendency to have one container do multiple things.

​	容器的一条最佳实践是每个容器应该只做一件事，并且要做得很好。虽然有一些例外，但尽量避免让一个容器做多件事。

Now you might ask, "Do I need to run these containers separately? If I run them separately, how shall I connect them all together?"

​	你可能会问：“我需要单独运行这些容器吗？如果我分开运行它们，如何将它们连接在一起呢？”

While `docker run` is a convenient tool for launching containers, it becomes difficult to manage a growing application stack with it. Here's why:

​	虽然 `docker run` 是一个启动容器的方便工具，但随着应用栈的增长，使用它进行管理变得困难。以下是原因：

- Imagine running several `docker run` commands (frontend, backend, and database) with different configurations for development, testing, and production environments. It's error-prone and time-consuming. 想象一下要为开发、测试和生产环境运行多个 `docker run` 命令（前端、后端和数据库），这很容易出错且耗时。
- Applications often rely on each other. Manually starting containers in a specific order and managing network connections become difficult as the stack expands. 应用程序往往相互依赖。随着应用栈的扩展，手动启动容器并管理网络连接变得更加困难。
- Each application needs its `docker run` command, making it difficult to scale individual services. Scaling the entire application means potentially wasting resources on components that don't need a boost. 每个应用程序都需要自己的 `docker run` 命令，这使得单个服务的扩展变得困难。扩展整个应用程序意味着可能会在不需要扩展的组件上浪费资源。
- Persisting data for each application requires separate volume mounts or configurations within each `docker run` command. This creates a scattered data management approach. 为每个应用程序持久化数据需要分别在每个 `docker run` 命令中设置挂载卷或配置数据管理，这种分散的方式很麻烦。
- Setting environment variables for each application through separate `docker run` commands is tedious and error-prone. 通过单独的 `docker run` 命令设置环境变量繁琐且容易出错。

That's where Docker Compose comes to the rescue.

​	这就是 Docker Compose 派上用场的地方。

Docker Compose defines your entire multi-container application in a single YAML file called `compose.yml`. This file specifies configurations for all your containers, their dependencies, environment variables, and even volumes and networks. With Docker Compose:

​	Docker Compose 在一个名为 `compose.yml` 的 YAML 文件中定义了整个多容器应用程序。这个文件指定了所有容器的配置、它们的依赖关系、环境变量，甚至包括卷和网络配置。使用 Docker Compose，你可以：

- You don't need to run multiple `docker run` commands. All you need to do is define your entire multi-container application in a single YAML file. This centralizes configuration and simplifies management. 不需要运行多个 `docker run` 命令，只需在一个 YAML 文件中定义整个多容器应用程序。这简化了配置管理
- You can run containers in a specific order and manage network connections easily. 你可以轻松按特定顺序运行容器并管理网络连接。
- You can simply scale individual services up or down within the multi-container setup. This allows for efficient allocation based on real-time needs. 你可以轻松地在多容器设置中扩展或缩减单个服务，实现资源的高效分配。
- You can implement persistent volumes with ease. 可以轻松实现持久化卷。
- It's easy to set environment variables once in your Docker Compose file. 在 Docker Compose 文件中一次性设置环境变量也很方便。

By leveraging Docker Compose for running multi-container setups, you can build complex applications with modularity, scalability, and consistency at their core.

​	通过使用 Docker Compose 来运行多容器设置，你可以构建具有模块化、可扩展性和一致性的复杂应用程序。

## 试试看 Try it out

In this hands-on guide, you'll first see how to build and run a counter web application based on Node.js, an Nginx reverse proxy, and a Redis database using the `docker run` commands. You’ll also see how you can simplify the entire deployment process using Docker Compose.

​	在这个实践指南中，你将首先看到如何通过 `docker run` 命令构建和运行一个基于 Node.js 的计数器 Web 应用程序，并配合 Nginx 反向代理和 Redis 数据库。你还将看到如何通过 Docker Compose 简化整个部署过程。

### 开始 Set up

1. Get the sample application. If you have Git, you can clone the repository for the sample application. Otherwise, you can download the sample application. Choose one of the following options. 获取示例应用程序。如果你有 Git，可以克隆示例应用程序的存储库。否则，你可以下载示例应用程序。选择以下选项之一。

   {{< tabpane text=true persist=disabled >}}

   {{% tab header="Clone with git " %}}

   Use the following command in a terminal to clone the sample application repository.

   ​	在终端中使用以下命令克隆示例应用程序存储库：

   ```console
   $ git clone https://github.com/dockersamples/nginx-node-redis
   ```

   Navigate into the `nginx-node-redis` directory:

   ​	进入 `nginx-node-redis` 目录：

   ```console
   $ cd nginx-node-redis
   ```

   Inside this directory, you'll find two sub-directories - `nginx` and `web`.

   ​	该目录下你将看到两个子目录 - `nginx` 和 `web`。

   {{% /tab  %}}

   {{% tab header="Download" %}}

   Download the source and extract it.

   ​	下载源代码并解压。

   [Download the source](https://github.com/dockersamples/nginx-node-redis/archive/refs/heads/main.zip)

   Navigate into the `nginx-node-redis-main` directory:

   ​	进入 `nginx-node-redis-main` 目录：

   ```console
   $ cd nginx-node-redis-main
   ```

   Inside this directory, you'll find two sub-directories - `nginx` and `web`.

   ​	在这个目录中，你会发现两个子目录 - `nginx` 和 `web`。

   {{% /tab  %}}

   {{< /tabpane >}}

   

2. [Download and install]({{< ref "/get-started/GetDocker" >}}) Docker Desktop. [下载并安装 Docker Desktop](https://www.docker.com/products/docker-desktop)。

### 构建镜像 Build the images

1. Navigate into the `nginx` directory to build the image by running the following command: 进入 `nginx` 目录并运行以下命令构建 Nginx 镜像：

   

   ```console
   $ docker build -t nginx .
   ```

2. Navigate into the `web` directory and run the following command to build the first web image: 进入 `web` 目录并运行以下命令构建 Web 镜像：

   

   ```console
   $ docker build -t web .
   ```

### 运行容器 Run the containers

1. Before you can run a multi-container application, you need to create a network for them all to communicate through. You can do so using the `docker network create` command: 在运行多容器应用程序之前，需要创建一个网络让它们互相通信。使用以下命令创建网络：

   

   ```console
   $ docker network create sample-app
   ```

2. Start the Redis container by running the following command, which will attach it to the previously created network and create a network alias (useful for DNS lookups): 启动 Redis 容器并将其连接到之前创建的网络，并创建网络别名（用于 DNS 查找）：

   

   ```console
   $ docker run -d  --name redis --network sample-app --network-alias redis redis
   ```

3. Start the first web container by running the following command: 启动第一个 Web 容器：

   

   ```console
   $ docker run -d --name web1 -h web1 --network sample-app --network-alias web1 web
   ```

4. Start the second web container by running the following: 启动第二个 Web 容器：

   

   ```console
   $ docker run -d --name web2 -h web2 --network sample-app --network-alias web2 web
   ```

5. Start the Nginx container by running the following command: 启动 Nginx 容器：

   

   ```console
   $ docker run -d --name nginx --network sample-app  -p 80:80 nginx
   ```

   > **Note**
   >
   > 
   >
   > Nginx is typically used as a reverse proxy for web applications, routing traffic to backend servers. In this case, it routes to the Node.js backend containers (web1 or web2).
   >
   > ​	Nginx 通常用作 Web 应用程序的反向代理，将流量路由到后端服务器。在这种情况下，它将流量路由到 Node.js 后端容器（web1 或 web2）。

6. Verify the containers are up by running the following command:  运行以下命令，验证容器是否启动：

   

   ```console
   $ docker ps
   ```

   You will see output like the following:

   你将看到类似如下的输出：

   ```text
   CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS                NAMES
   2cf7c484c144   nginx     "/docker-entrypoint.…"   9 seconds ago        Up 8 seconds        0.0.0.0:80->80/tcp   nginx
   7a070c9ffeaa   web       "docker-entrypoint.s…"   19 seconds ago       Up 18 seconds                            web2
   6dc6d4e60aaf   web       "docker-entrypoint.s…"   34 seconds ago       Up 33 seconds                            web1
   008e0ecf4f36   redis     "docker-entrypoint.s…"   About a minute ago   Up About a minute   6379/tcp             redis
   ```

7. If you look at the Docker Dashboard, you can see the containers and dive deeper into their configuration. 如果你查看 Docker Dashboard，你可以看到容器并深入了解其配置。

   ![A screenshot of Docker Dashboard showing multi-container applications](Multi-containerapplications_img/multi-container-apps.webp)

8. With everything up and running, you can open [http://localhost](http://localhost/) in your browser to see the site. Refresh the page several times to see the host that’s handling the request and the total number of requests: 当所有容器都启动后，你可以在浏览器中打开 [http://localhost](http://localhost/) 来查看网站。多次刷新页面，查看处理请求的主机以及请求的总数：

   

   ```console
   web2: Number of visits is: 9
   web1: Number of visits is: 10
   web2: Number of visits is: 11
   web1: Number of visits is: 12
   ```

   > **Note**
   >
   > 
   >
   > You might have noticed that Nginx, acting as a reverse proxy, likely distributes incoming requests in a round-robin fashion between the two backend containers. This means each request might be directed to a different container (web1 and web2) on a rotating basis. The output shows consecutive increments for both the web1 and web2 containers and the actual counter value stored in Redis is updated only after the response is sent back to the client.
   >
   > ​	你可能注意到，Nginx 作为反向代理，可能以轮循方式分发传入的请求到两个后端容器。这意味着每个请求可能会轮流指向不同的容器（web1 和 web2）。输出显示 web1 和 web2 容器的连续递增，请求的实际计数存储在 Redis 中，并在响应返回给客户端后更新。

9. You can use the Docker Dashboard to remove the containers by selecting the containers and selecting the **Delete** button. 你可以使用 Docker Dashboard 选择容器并点击 **Delete** 按钮来删除容器。

   ![A screenshot of Docker Dashboard showing how to delete the multi-container applications](Multi-containerapplications_img/delete-multi-container-apps.webp)

## 使用 Docker Compose 简化部署 Simplify the deployment using Docker Compose

Docker Compose provides a structured and streamlined approach for managing multi-container deployments. As stated earlier, with Docker Compose, you don’t need to run multiple `docker run` commands. All you need to do is define your entire multi-container application in a single YAML file called `compose.yml`. Let’s see how it works.

​	Docker Compose 提供了一个结构化且简化的方法来管理多容器部署。如前所述，使用 Docker Compose，你无需运行多个 `docker run` 命令。你只需在一个名为 `compose.yml` 的 YAML 文件中定义整个多容器应用程序。让我们看看它是如何工作的。

Navigate to the root of the project directory. Inside this directory, you'll find a file named `compose.yml`. This YAML file is where all the magic happens. It defines all the services that make up your application, along with their configurations. Each service specifies its image, ports, volumes, networks, and any other settings necessary for its functionality.

​	导航到项目目录的根目录。在该目录下，你会找到一个名为 `compose.yml` 的文件。这个 YAML 文件定义了构成你的应用程序的所有服务，以及它们的配置。每个服务都指定了它的镜像、端口、卷、网络以及任何其他所需的设置。

1. Use the `docker compose up` command to start the application: 使用 `docker compose up` 命令启动应用程序：

   

   ```console
   $ docker compose up -d --build
   ```

   When you run this command, you should see output similar to the following:

   运行此命令后，你将看到类似如下的输出：

   ```console
   Running 5/5
   ✔ Network nginx-nodejs-redis_default    Created                                                0.0s
   ✔ Container nginx-nodejs-redis-web1-1   Started                                                0.1s
   ✔ Container nginx-nodejs-redis-redis-1  Started                                                0.1s
   ✔ Container nginx-nodejs-redis-web2-1   Started                                                0.1s
   ✔ Container nginx-nodejs-redis-nginx-1  Started
   ```

2. If you look at the Docker Dashboard, you can see the containers and dive deeper into their configuration. 如果你查看 Docker Dashboard，你可以看到容器并深入了解其配置。

   ![A screenshot of Docker Dashboard showing the containers of the application stack deployed using Docker Compose](Multi-containerapplications_img/list-containers.webp)

3. Alternatively, you can use the Docker Dashboard to remove the containers by selecting the application stack and selecting the **Delete** button. 你也可以使用 Docker Dashboard 通过选择应用程序堆栈并点击 **Delete** 按钮来删除容器。

   ![A screenshot of Docker Dashboard that shows how to remove the containers that you deployed using Docker Compose](Multi-containerapplications_img/delete-containers.webp)

In this guide, you learned how easy it is to use Docker Compose to start and stop a multi-container application compared to `docker run` which is error-prone and difficult to manage.

​	在本指南中，你学习了使用 Docker Compose 启动和停止多容器应用程序相比于 `docker run` 更加简便，避免了错误且更易于管理。

## 其他资源 Additional resources

- [`docker container run` CLI reference]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerrun" >}})  - [`docker container run` CLI 参考]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerrun" >}})
- [What is Docker Compose]({{< ref "/get-started/Dockerconcepts/Thebasics/WhatisDockerCompose" >}}) - [什么是 Docker Compose]({{< ref "/get-started/Dockerconcepts/Thebasics/WhatisDockerCompose" >}})
