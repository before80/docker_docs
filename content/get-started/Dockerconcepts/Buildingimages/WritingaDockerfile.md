+++
title = "编写 Dockerfile"
date = 2024-10-23T14:54:35+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/get-started/docker-concepts/building-images/writing-a-dockerfile/](https://docs.docker.com/get-started/docker-concepts/building-images/writing-a-dockerfile/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# Writing a Dockerfile 编写 Dockerfile

{{< youtube "Jx8zoIhiP4c">}}

## 说明 Explanation

A Dockerfile is a text-based document that's used to create a container image. It provides instructions to the image builder on the commands to run, files to copy, startup command, and more.

​	Dockerfile 是一个基于文本的文档，用于创建容器镜像。它向镜像构建器提供指令，如要运行的命令、要复制的文件、启动命令等。

As an example, the following Dockerfile would produce a ready-to-run Python application:

​	以下是一个用于生成可运行的 Python 应用程序的 Dockerfile 示例：



```dockerfile
FROM python:3.12
WORKDIR /usr/local/app

# Install the application dependencies
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

# Copy in the source code
COPY src ./src
EXPOSE 5000

# Setup an app user so the container doesn't run as the root user
RUN useradd app
USER app

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8080"]
```

### 常用指令 Common instructions

Some of the most common instructions in a `Dockerfile` include:

​	`Dockerfile` 中一些常见的指令包括：

- `FROM <image>` - this specifies the base image that the build will extend. `FROM <image>` - 指定构建将基于的基础镜像。

- `WORKDIR <path>` - this instruction specifies the "working directory" or the path in the image where files will be copied and commands will be executed. `WORKDIR <path>` - 指定工作目录，即镜像中的文件复制和命令执行的路径。
- `COPY <host-path> <image-path>` - this instruction tells the builder to copy files from the host and put them into the container image. `COPY <host-path> <image-path>` - 指示构建器从主机复制文件到容器镜像中。
- `RUN <command>` - this instruction tells the builder to run the specified command. `RUN <command>` - 告诉构建器执行指定的命令。
- `ENV <name> <value>` - this instruction sets an environment variable that a running container will use. `ENV <name> <value>` - 设置一个环境变量，运行中的容器将使用该变量。
- `EXPOSE <port-number>` - this instruction sets configuration on the image that indicates a port the image would like to expose. `EXPOSE <port-number>` - 配置镜像希望暴露的端口。
- `USER <user-or-uid>` - this instruction sets the default user for all subsequent instructions. `USER <user-or-uid>` - 设置后续指令的默认用户。
- `CMD ["<command>", "<arg1>"]` - this instruction sets the default command a container using this image will run. `CMD ["<command>", "<arg1>"]` - 设置容器使用此镜像运行时的默认命令。

To read through all of the instructions or go into greater detail, check out the [Dockerfile reference](https://docs.docker.com/engine/reference/builder/).

​	若要了解所有指令或详细信息，请查看 [Dockerfile 参考文档](https://docs.docker.com/engine/reference/builder/)。

## 试试看 Try it out

Just as you saw with the previous example, a Dockerfile typically follows these steps:

​	正如之前的示例所示，Dockerfile 通常遵循以下步骤：

1. Determine your base image 确定基础镜像
2. Install application dependencies 安装应用程序依赖
3. Copy in any relevant source code and/or binaries 复制相关的源代码或二进制文件
4. Configure the final image 配置最终镜像

In this quick hands-on guide, you'll write a Dockerfile that builds a simple Node.js application. If you're not familiar with JavaScript-based applications, don't worry. It isn't necessary for following along with this guide.

​	在本快速实践指南中，你将编写一个构建简单 Node.js 应用程序的 Dockerfile。即使你不熟悉基于 JavaScript 的应用程序，也可以跟随本指南完成操作。

### 设置 Set up

[Download this ZIP file](https://github.com/docker/getting-started-todo-app/raw/build-image-from-scratch/app.zip) and extract the contents into a directory on your machine.

​	[下载这个 ZIP 文件](https://github.com/docker/getting-started-todo-app/raw/build-image-from-scratch/app.zip) 并将内容解压到你机器上的目录中。

### 创建 Dockerfile - Creating the Dockerfile

Now that you have the project, you’re ready to create the `Dockerfile`.

​	现在你已经有了项目，接下来就可以创建 `Dockerfile`。

1. [Download and install](https://www.docker.com/products/docker-desktop/) Docker Desktop. [下载并安装](https://www.docker.com/products/docker-desktop/) Docker Desktop。

2. Create a file named `Dockerfile` in the same folder as the file `package.json`. 在与 `package.json` 文件相同的文件夹中创建一个名为 `Dockerfile` 的文件。

   > **Dockerfile file extensions - Dockerfile 文件扩展名**
   >
   > It's important to note that the `Dockerfile` has *no* file extension. Some editors will automatically add an extension to the file (or complain it doesn't have one).
   >
   > ​	请注意，`Dockerfile` 没有文件扩展名。一些编辑器可能会自动添加扩展名（或提示它没有扩展名）。

3. In the `Dockerfile`, define your base image by adding the following line: 在 `Dockerfile` 中，通过添加以下行定义你的基础镜像：

   

   ```dockerfile
   FROM node:20-alpine
   ```

4. Now, define the working directory by using the `WORKDIR` instruction. This will specify where future commands will run and the directory files will be copied inside the container image. 现在，使用 `WORKDIR` 指令定义工作目录。这将指定后续命令运行的目录，以及将文件复制到容器镜像的目录。

   

   ```dockerfile
   WORKDIR /app
   ```

5. Copy all of the files from your project on your machine into the container image by using the `COPY` instruction: 使用 `COPY` 指令将你的项目中的所有文件复制到容器镜像中：

   

   ```dockerfile
   COPY . .
   ```

6. Install the app's dependencies by using the `yarn` CLI and package manager. To do so, run a command using the `RUN` instruction: 使用 `yarn` 命令行工具和包管理器安装应用程序的依赖项。为此，使用 `RUN` 指令运行以下命令：

   

   ```dockerfile
   RUN yarn install --production
   ```

7. Finally, specify the default command to run by using the `CMD` instruction: 最后，使用 `CMD` 指令指定默认的运行命令：

   

   ```dockerfile
   CMD ["node", "./src/index.js"]
   ```

   And with that, you should have the following Dockerfile:

   最终的 Dockerfile 如下：

   ```dockerfile
   FROM node:20-alpine
   WORKDIR /app
   COPY . .
   RUN yarn install --production
   CMD ["node", "./src/index.js"]
   ```

> **This Dockerfile isn't production-ready yet  此 Dockerfile 还未达到生产环境的最佳实践**
>
> It's important to note that this Dockerfile is *not* following all of the best practices yet (by design). It will build the app, but the builds won't be as fast, or the images as secure, as they could be.
>
> ​	需要注意的是，这个 Dockerfile 尚未遵循所有最佳实践（这是有意为之）。它可以构建应用程序，但构建速度和镜像的安全性还有待优化。
>
> Keep reading to learn more about how to make the image maximize the build cache, run as a non-root user, and multi-stage builds.
>
> ​	继续阅读，了解如何通过最大化构建缓存、以非 root 用户运行以及多阶段构建来优化镜像。

> **Containerize new projects quickly with `docker init`  使用 `docker init` 快速容器化新项目**
>
> The `docker init` command will analyze your project and quickly create a Dockerfile, a `compose.yaml`, and a `.dockerignore`, helping you get up and going. Since you're learning about Dockerfiles specifically here, you won't use it now. But, [learn more about it here](https://docs.docker.com/engine/reference/commandline/init/).
>
> ​	`docker init` 命令可以分析你的项目，并快速创建一个 Dockerfile、`compose.yaml` 和 `.dockerignore`，帮助你快速上手。由于本指南专注于 Dockerfile，所以现在你不需要使用它。但你可以 [在这里了解更多](https://docs.docker.com/engine/reference/commandline/init/)。

## 其他资源 Additional resources

To learn more about writing a Dockerfile, visit the following resources:

​	若想进一步了解编写 Dockerfile，请访问以下资源：

- [Dockerfile reference]({{< ref "/reference/Dockerfilereference" >}}) - [Dockerfile 参考]({{< ref "/reference/Dockerfilereference" >}})
- [Dockerfile best practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [Base images]({{< ref "/manuals/DockerBuild/Building/Baseimages" >}}) - [基础镜像]({{< ref "/manuals/DockerBuild/Building/Baseimages" >}})
- [Getting started with Docker Init]({{< ref "/reference/CLIreference/docker/dockerinit" >}}) - [Docker Init 入门]({{< ref "/reference/CLIreference/docker/dockerinit" >}})

## 接下来 Next steps

Now that you have created a Dockerfile and learned the basics, it's time to learn about building, tagging, and pushing the images.

​	现在你已经创建了一个 Dockerfile 并学习了基础知识，接下来是学习如何构建、标记和推送镜像。

[Build, tag and publish the Image]({{< ref "/get-started/Dockerconcepts/Buildingimages/Buildtagandpublishanimage" >}}) - [构建、标记并发布镜像]({{< ref "/get-started/Dockerconcepts/Buildingimages/Buildtagandpublishanimage" >}})
