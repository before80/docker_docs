+++
title = "Part 1: 将应用程序容器化"
date = 2024-10-23T14:54:35+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/get-started/workshop/02_our_app/](https://docs.docker.com/get-started/workshop/02_our_app/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# Containerize an application - 将应用程序容器化

For the rest of this guide, you'll be working with a simple todo list manager that runs on Node.js. If you're not familiar with Node.js, don't worry. This guide doesn't require any prior experience with JavaScript.

​	在本指南的剩余部分，你将使用一个简单的运行在 Node.js 上的待办事项列表管理器。如果你不熟悉 Node.js，也不用担心，本指南不需要你具备 JavaScript 的任何基础知识。

## 前置条件 Prerequisites

- You have installed the latest version of [Docker Desktop]({{< ref "/get-started/GetDocker" >}}). 你已经安装了最新版本的 [Docker Desktop]({{< ref "/get-started/GetDocker" >}})。
- You have installed a [Git client](https://git-scm.com/downloads). 你已经安装了 [Git 客户端](https://git-scm.com/downloads)。
- You have an IDE or a text editor to edit files. Docker recommends using [Visual Studio Code](https://code.visualstudio.com/).
- 你有一个 IDE 或文本编辑器来编辑文件。Docker 推荐使用 [Visual Studio Code](https://code.visualstudio.com/)。

## 获取应用程序 Get the app

Before you can run the application, you need to get the application source code onto your machine.

​	在运行应用程序之前，你需要将应用程序的源代码获取到你的机器上。

1. Clone the [getting-started-app repository](https://github.com/docker/getting-started-app/tree/main) using the following command: 使用以下命令克隆 [getting-started-app 仓库](https://github.com/docker/getting-started-app/tree/main)：

   

   ```console
   $ git clone https://github.com/docker/getting-started-app.git
   ```

2. View the contents of the cloned repository. You should see the following files and sub-directories.查看克隆的仓库内容。你应该看到以下文件和子目录。

   

   ```text
   ├── getting-started-app/
   │ ├── .dockerignore
   │ ├── package.json
   │ ├── README.md
   │ ├── spec/
   │ ├── src/
   │ └── yarn.lock
   ```

## 构建应用程序的镜像 Build the app's image

To build the image, you'll need to use a Dockerfile. A Dockerfile is simply a text-based file with no file extension that contains a script of instructions. Docker uses this script to build a container image.

​	要构建镜像，你需要使用 Dockerfile。Dockerfile 是一个没有文件扩展名的基于文本的文件，里面包含了一系列指令的脚本。Docker 使用这些脚本来构建容器镜像。

1. In the `getting-started-app` directory, the same location as the `package.json` file, create a file named `Dockerfile`. You can use the following commands to create a Dockerfile based on your operating system. 在 `getting-started-app` 目录中，与 `package.json` 文件相同的位置，创建一个名为 `Dockerfile` 的文件。你可以根据操作系统使用以下命令来创建 Dockerfile。

   {{< tabpane text=true persist=disabled >}}

   {{% tab header="Mac / Linux / Windows (Git Bash) " %}}

   In the terminal, run the following commands.

   ​	在终端中运行以下命令。

   Make sure you're in the `getting-started-app` directory. Replace `/path/to/getting-started-app` with the path to your `getting-started-app` directory.

   ​	确保你处于 `getting-started-app` 目录中。用你 `getting-started-app` 目录的路径替换 `/path/to/getting-started-app`。

   ```console
   $ cd /path/to/getting-started-app
   ```

   Create an empty file named `Dockerfile`.

   ​	创建一个空的 `Dockerfile` 文件。

   ```console
   $ touch Dockerfile
   ```

   {{% /tab  %}}

   {{% tab header="Windows (Command Prompt)" %}}

   In the Windows Command Prompt, run the following commands.

   ​	在 Windows 命令提示符中运行以下命令。

   Make sure you're in the `getting-started-app` directory. Replace `\path\to\getting-started-app` with the path to your `getting-started-app` directory.

   ​	确保你处于 `getting-started-app` 目录中。用你 `getting-started-app` 目录的路径替换 `\path\to\getting-started-app`。

   ```console
   $ cd \path\to\getting-started-app
   ```

   Create an empty file named `Dockerfile`.

   ​	创建一个空的 `Dockerfile` 文件。

   ```console
   $ type nul > Dockerfile
   ```

   {{% /tab  %}}

   {{% tab header=" Windows (PowerShell)" %}}

   In PowerShell, run the following commands.

   ​	在 PowerShell 中运行以下命令。

   Make sure you're in the `getting-started-app` directory. Replace `\path\to\getting-started-app` with the path to your `getting-started-app` directory.

   ​	确保你处于 `getting-started-app` 目录中。用你 `getting-started-app` 目录的路径替换 `\path\to\getting-started-app`。

   ```console
   $ cd \path\to\getting-started-app
   ```

   Create an empty file named `Dockerfile`.

   ​	创建一个空的 `Dockerfile` 文件。

   

   ```powershell
   $ New-Item -Path . -Name Dockerfile -ItemType File
   ```

   {{% /tab  %}}

   {{< /tabpane >}}

   

2. Using a text editor or code editor, add the following contents to the Dockerfile: 使用文本编辑器或代码编辑器，向 Dockerfile 中添加以下内容：

   

   ```dockerfile
   # syntax=docker/dockerfile:1
   
   FROM node:18-alpine
   WORKDIR /app
   COPY . .
   RUN yarn install --production
   CMD ["node", "src/index.js"]
   EXPOSE 3000
   ```

3. Build the image using the following commands: 使用以下命令构建镜像：

   In the terminal, make sure you're in the `getting-started-app` directory. Replace `/path/to/getting-started-app` with the path to your `getting-started-app` directory.

   在终端中，确保你处于 `getting-started-app` 目录中。用你 `getting-started-app` 目录的路径替换 `/path/to/getting-started-app`。

   ```console
   $ cd /path/to/getting-started-app
   ```

   Build the image.

   ​	构建镜像。

   ```console
   $ docker build -t getting-started .
   ```

   The `docker build` command uses the Dockerfile to build a new image. You might have noticed that Docker downloaded a lot of "layers". This is because you instructed the builder that you wanted to start from the `node:18-alpine` image. But, since you didn't have that on your machine, Docker needed to download the image.

   ​	`docker build` 命令使用 Dockerfile 来构建一个新的镜像。你可能注意到 Docker 下载了很多“层”。这是因为你指示构建器从 `node:18-alpine` 镜像开始。但由于你没有在本地拥有这个镜像，Docker 需要下载该镜像。

   After Docker downloaded the image, the instructions from the Dockerfile copied in your application and used `yarn` to install your application's dependencies. The `CMD` directive specifies the default command to run when starting a container from this image.

   ​	下载镜像后，Dockerfile 中的指令将你的应用程序复制进去，并使用 `yarn` 安装了应用程序的依赖项。`CMD` 指令指定了从该镜像启动容器时的默认运行命令。
   
   Finally, the `-t` flag tags your image. Think of this as a human-readable name for the final image. Since you named the image `getting-started`, you can refer to that image when you run a container.
   
   ​	最后，`-t` 标志为你的镜像打标签。可以把它看作是最终镜像的可读名称。由于你将镜像命名为 `getting-started`，你可以在运行容器时引用该镜像。
   
   The `.` at the end of the `docker build` command tells Docker that it should look for the `Dockerfile` in the current directory.
   
   ​	`docker build` 命令末尾的 `.` 表示 Docker 应该在当前目录中查找 Dockerfile。

## 启动应用容器 Start an app container

Now that you have an image, you can run the application in a container using the `docker run` command.

​	现在你已经有了一个镜像，你可以使用 `docker run` 命令在容器中运行应用程序。

1. Run your container using the `docker run` command and specify the name of the image you just created: 使用 `docker run` 命令并指定你刚刚创建的镜像名称来运行容器：

   

   ```console
   $ docker run -dp 127.0.0.1:3000:3000 getting-started
   ```

   The `-d` flag (short for `--detach`) runs the container in the background. This means that Docker starts your container and returns you to the terminal prompt. You can verify that a container is running by viewing it in Docker Dashboard under **Containers**, or by running `docker ps` in the terminal.

   ​	`-d` 标志（`--detach` 的缩写）让容器在后台运行。这意味着 Docker 启动了你的容器，并将你返回到终端提示符。你可以通过 Docker Dashboard 中的 **Containers** 选项卡或在终端运行 `docker ps` 来验证容器是否正在运行。

   The `-p` flag (short for `--publish`) creates a port mapping between the host and the container. The `-p` flag takes a string value in the format of `HOST:CONTAINER`, where `HOST` is the address on the host, and `CONTAINER` is the port on the container. The command publishes the container's port 3000 to `127.0.0.1:3000` (`localhost:3000`) on the host. Without the port mapping, you wouldn't be able to access the application from the host.

   ​	`-p` 标志（`--publish` 的缩写）在主机和容器之间创建了端口映射。`-p` 标志接受一个 `HOST:CONTAINER` 格式的字符串，其中 `HOST` 是主机上的地址，`CONTAINER` 是容器上的端口。该命令将容器的 3000 端口发布到主机上的 `127.0.0.1:3000`（`localhost:3000`）。如果没有端口映射，你将无法从主机访问应用程序。

2. After a few seconds, open your web browser to [http://localhost:3000](http://localhost:3000/). You should see your app. 几秒钟后，打开你的浏览器，访问 [http://localhost:3000](http://localhost:3000/)。你应该能看到你的应用。

   ![Empty todo list](Part1Containerizeanapplication_img/todo-list-empty.webp)

3. Add an item or two and see that it works as you expect. You can mark items as complete and remove them. Your frontend is successfully storing items in the backend. 添加一个或两个事项，并查看它是否按预期工作。你可以将事项标记为完成并删除它们。你的前端已成功地在后台存储了事项。

At this point, you have a running todo list manager with a few items.

​	此时，你已经有了一个运行中的待办事项列表管理器，其中有一些事项。

If you take a quick look at your containers, you should see at least one container running that's using the `getting-started` image and on port `3000`. To see your containers, you can use the CLI or Docker Desktop's graphical interface.

​	如果快速查看你的容器，你应该看到至少有一个使用 `getting-started` 镜像并运行在 `3000` 端口的容器。你可以使用 CLI 或 Docker Desktop 的图形界面来查看你的容器。

{{< tabpane text=true persist=disabled >}}

{{% tab header="CLI " %}}

Run the following `docker ps` command in a terminal to list your containers.

​	在终端中运行以下 `docker ps` 命令列出你的容器。

```console
$ docker ps
```

Output similar to the following should appear.

​	你应该看到类似于以下的输出。

```console
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                      NAMES
df784548666d        getting-started     "docker-entrypoint.s…"   2 minutes ago       Up 2 minutes        127.0.0.1:3000->3000/tcp   priceless_mcclintock
```

{{% /tab  %}}

{{% tab header="Docker Desktop" %}}

In Docker Desktop, select the **Containers** tab to see a list of your containers.

​	在 Docker Desktop 中，选择 **Containers** 选项卡查看你的容器列表。

![Docker Desktop with get-started container running](Part1Containerizeanapplication_img/dashboard-two-containers.webp)

{{% /tab  %}}

{{< /tabpane >}}



------

## 总结 Summary

In this section, you learned the basics about creating a Dockerfile to build an image. Once you built an image, you started a container and saw the running app.

​	在本节中，你学习了如何创建 Dockerfile 来构建镜像的基础知识。在构建镜像后，你启动了一个容器并查看了运行中的应用程序。

Related information:

​	相关信息：

- [Dockerfile reference]({{< ref "/reference/Dockerfilereference" >}}) -   [Dockerfile 参考]({{< ref "/reference/Dockerfilereference" >}})
- [docker CLI reference]({{< ref "/reference/CLIreference/docker" >}}) - [docker CLI 参考]({{< ref "/reference/CLIreference/docker" >}})

## 接下来 Next steps

Next, you're going to make a modification to your app and learn how to update your running application with a new image. Along the way, you'll learn a few other useful commands.

​	接下来，你将对应用程序进行修改，并学习如何通过新镜像更新正在运行的应用程序。在此过程中，你将学习一些其他有用的命令。

[Update the application]({{< ref "/get-started/Dockerworkshop/Part2Updatetheapplication" >}}) - [更新应用程序]({{< ref "/get-started/Dockerworkshop/Part2Updatetheapplication" >}})
