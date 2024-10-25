+++
title = "Part 7: 使用 Docker Compose"
date = 2024-10-23T14:54:35+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/get-started/workshop/08_using_compose/](https://docs.docker.com/get-started/workshop/08_using_compose/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# Use Docker Compose - 使用 Docker Compose

[Docker Compose]({{< ref "/manuals/DockerCompose" >}}) is a tool that helps you define and share multi-container applications. With Compose, you can create a YAML file to define the services and with a single command, you can spin everything up or tear it all down.

​	[Docker Compose]({{< ref "/manuals/DockerCompose" >}}) 是一个帮助你定义和共享多容器应用的工具。通过 Compose，你可以创建一个 YAML 文件来定义服务，并通过一个命令启动或停止所有服务。

The big advantage of using Compose is you can define your application stack in a file, keep it at the root of your project repository (it's now version controlled), and easily enable someone else to contribute to your project. Someone would only need to clone your repository and start the app using Compose. In fact, you might see quite a few projects on GitHub/GitLab doing exactly this now.

​	使用 Compose 的最大优点是可以将应用栈定义在文件中，将其保存在项目根目录（进行版本控制），并轻松让他人参与项目。其他人只需克隆你的代码库并使用 Compose 启动应用即可。事实上，你可能会看到 GitHub 或 GitLab 上有许多项目正是这么做的。

## 创建 Compose 文件 Create the Compose file

In the `getting-started-app` directory, create a file named `compose.yaml`.

​	在 `getting-started-app` 目录下，创建一个名为 `compose.yaml` 的文件。

```text
├── getting-started-app/
│ ├── Dockerfile
│ ├── compose.yaml
│ ├── node_modules/
│ ├── package.json
│ ├── spec/
│ ├── src/
│ └── yarn.lock
```

## 定义应用服务 Define the app service

In [part 6]({{< ref "/get-started/Dockerworkshop/Part6Multi-containerapps" >}}), you used the following command to start the application service.

​	在[第 6 部分]({{< ref "/get-started/Dockerworkshop/Part6Multi-containerapps" >}})中，你使用以下命令启动了应用服务。

```console
$ docker run -dp 127.0.0.1:3000:3000 \
  -w /app -v "$(pwd):/app" \
  --network todo-app \
  -e MYSQL_HOST=mysql \
  -e MYSQL_USER=root \
  -e MYSQL_PASSWORD=secret \
  -e MYSQL_DB=todos \
  node:18-alpine \
  sh -c "yarn install && yarn run dev"
```

You'll now define this service in the `compose.yaml` file.

​	现在，你将在 `compose.yaml` 文件中定义此服务。

1. Open `compose.yaml` in a text or code editor, and start by defining the name and image of the first service (or container) you want to run as part of your application. The name will automatically become a network alias, which will be useful when defining your MySQL service.  在文本或代码编辑器中打开 `compose.yaml` 文件，首先定义应用中第一个服务（或容器）的名称和镜像。名称将自动成为网络别名，在定义 MySQL 服务时会很有用。

   

   ```yaml
   services:
     app:
       image: node:18-alpine
   ```

2. Typically, you will see `command` close to the `image` definition, although there is no requirement on ordering. Add the `command` to your `compose.yaml` file. 通常，你会看到 `command` 紧跟在 `image` 定义之后，虽然顺序没有要求。将 `command` 添加到你的 `compose.yaml` 文件中。

   

   ```yaml
   services:
     app:
       image: node:18-alpine
       command: sh -c "yarn install && yarn run dev"
   ```

3. Now migrate the `-p 127.0.0.1:3000:3000` part of the command by defining the `ports` for the service. 现在，通过定义 `ports` 来迁移 `-p 127.0.0.1:3000:3000` 部分。

   

   ```yaml
   services:
     app:
       image: node:18-alpine
       command: sh -c "yarn install && yarn run dev"
       ports:
         - 127.0.0.1:3000:3000
   ```

4. Next, migrate both the working directory (`-w /app`) and the volume mapping (`-v "$(pwd):/app"`) by using the `working_dir` and `volumes` definitions. 接下来，通过使用 `working_dir` 和 `volumes` 定义迁移工作目录 (`-w /app`) 和卷映射 (`-v "$(pwd):/app"`)。

   One advantage of Docker Compose volume definitions is you can use relative paths from the current directory.

   ​	Docker Compose 的卷定义一个优势在于你可以使用当前目录的相对路径。

   ```yaml
   services:
     app:
       image: node:18-alpine
       command: sh -c "yarn install && yarn run dev"
       ports:
         - 127.0.0.1:3000:3000
       working_dir: /app
       volumes:
         - ./:/app
   ```

5. Finally, you need to migrate the environment variable definitions using the `environment` key. 最最后，你需要使用 `environment` 键来迁移环境变量的定义。

   

   ```yaml
   services:
     app:
       image: node:18-alpine
       command: sh -c "yarn install && yarn run dev"
       ports:
         - 127.0.0.1:3000:3000
       working_dir: /app
       volumes:
         - ./:/app
       environment:
         MYSQL_HOST: mysql
         MYSQL_USER: root
         MYSQL_PASSWORD: secret
         MYSQL_DB: todos
   ```

### 定义 MySQL 服务 Define the MySQL service

Now, it's time to define the MySQL service. The command that you used for that container was the following:

​	现在，定义 MySQL 服务。用于该容器的命令如下：

```console
$ docker run -d \
  --network todo-app --network-alias mysql \
  -v todo-mysql-data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=todos \
  mysql:8.0
```

1. First define the new service and name it `mysql` so it automatically gets the network alias. Also specify the image to use as well. 首先定义新的服务并命名为 `mysql`，使其自动获得网络别名，同时指定所需镜像。

   

   ```yaml
   services:
     app:
       # The app service definition
     mysql:
       image: mysql:8.0
   ```

2. Next, define the volume mapping. When you ran the container with `docker run`, Docker created the named volume automatically. However, that doesn't happen when running with Compose. You need to define the volume in the top-level `volumes:` section and then specify the mountpoint in the service config. By simply providing only the volume name, the default options are used. 定义卷映射。使用 `docker run` 运行容器时，Docker 会自动创建命名卷。但在使用 Compose 时不会自动创建。你需要在顶部 `volumes:` 部分定义卷，然后在服务配置中指定挂载点。只需提供卷名称，默认选项将自动使用。

   

   ```yaml
   services:
     app:
       # The app service definition
     mysql:
       image: mysql:8.0
       volumes:
         - todo-mysql-data:/var/lib/mysql
   
   volumes:
     todo-mysql-data:
   ```

3. Finally, you need to specify the environment variables. 最后，指定环境变量。

   

   ```yaml
   services:
     app:
       # The app service definition
     mysql:
       image: mysql:8.0
       volumes:
         - todo-mysql-data:/var/lib/mysql
       environment:
         MYSQL_ROOT_PASSWORD: secret
         MYSQL_DATABASE: todos
   
   volumes:
     todo-mysql-data:
   ```

At this point, your complete `compose.yaml` should look like this:

​	此时，你的完整 `compose.yaml` 应如下所示：

```yaml
services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 127.0.0.1:3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  mysql:
    image: mysql:8.0
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
```

## 运行应用程序栈 Run the application stack

Now that you have your `compose.yaml` file, you can start your application.

​	现在你已经有了 `compose.yaml` 文件，可以启动应用程序了。

1. Make sure no other copies of the containers are running first. Use `docker ps` to list the containers and `docker rm -f <ids>` to remove them. 确保没有其他容器实例在运行。使用 `docker ps` 列出容器，并使用 `docker rm -f <ids>` 删除它们。

2. Start up the application stack using the `docker compose up` command. Add the `-d` flag to run everything in the background. 使用 `docker compose up` 命令启动应用栈。添加 `-d` 标志在后台运行所有服务。

   

   ```console
   $ docker compose up -d
   ```

   When you run the previous command, you should see output like the following:

   ​	运行此命令后，你会看到如下输出：

   ```plaintext
   Creating network "app_default" with the default driver
   Creating volume "app_todo-mysql-data" with default driver
   Creating app_app_1   ... done
   Creating app_mysql_1 ... done
   ```

   You'll notice that Docker Compose created the volume as well as a network. By default, Docker Compose automatically creates a network specifically for the application stack (which is why you didn't define one in the Compose file).

   ​	注意，Docker Compose 创建了卷和网络。默认情况下，Docker Compose 会自动为应用程序栈创建特定的网络（因此你无需在 Compose 文件中定义网络）。

3. Look at the logs using the `docker compose logs -f` command. You'll see the logs from each of the services interleaved into a single stream. This is incredibly useful when you want to watch for timing-related issues. The `-f` flag follows the log, so will give you live output as it's generated. 使用 `docker compose logs -f` 命令查看日志。你会看到每个服务的日志交织在一个流中，这在监控与时间相关的问题时非常有用。`-f` 标志会跟随日志，实时输出生成的内容。

   If you have run the command already, you'll see output that looks like this:

   ​	如果你已经运行了该命令，你会看到如下输出：

   ```plaintext
   mysql_1  | 2019-10-03T03:07:16.083639Z 0 [Note] mysqld: ready for connections.
   mysql_1  | Version: '8.0.31'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server (GPL)
   app_1    | Connected to mysql db at host mysql
   app_1    | Listening on port 3000
   ```

   The service name is displayed at the beginning of the line (often colored) to help distinguish messages. If you want to view the logs for a specific service, you can add the service name to the end of the logs command (for example, `docker compose logs -f app`).

   ​	服务名称显示在行的开头（通常带有颜色）以帮助区分消息。如果你想查看特定服务的日志，可以在日志命令末尾添加服务名称（例如 `docker compose logs -f app`）。

4. At this point, you should be able to open your app in your browser on [http://localhost:3000](http://localhost:3000/) and see it running. 此时，你应该可以在浏览器中打开 [http://localhost:3000](http://localhost:3000/) 访问应用程序并看到它正在运行。

## 在 Docker Dashboard 中查看应用栈 See the app stack in Docker Dashboard

If you look at the Docker Dashboard, you'll see that there is a group named **getting-started-app**. This is the project name from Docker Compose and used to group the containers together. By default, the project name is simply the name of the directory that the `compose.yaml` was located in.

​	在 Docker Dashboard 中，你会看到一个名为 **getting-started-app** 的组。这是 Docker Compose 项目名称，用于将容器分组在一起。默认情况下，项目名称是 `compose.yaml` 所在目录的名称。

If you expand the stack, you'll see the two containers you defined in the Compose file. The names are also a little more descriptive, as they follow the pattern of `<service-name>-<replica-number>`. So, it's very easy to quickly see what container is your app and which container is the mysql database.

​	展开栈时，可以看到你在 Compose 文件中定义的两个容器。名称也更加描述性，因为它们遵循 `<service-name>-<replica-number>` 的模式。这样可以很方便地快速看到哪个容器是应用程序，哪个容器是 MySQL 数据库。

## 清除所有 Tear it all down

When you're ready to tear it all down, simply run `docker compose down` or hit the trash can on the Docker Dashboard for the entire app. The containers will stop and the network will be removed.

​	当你准备清除所有内容时，只需运行 `docker compose down`，或在 Docker Dashboard 中点击整个应用栈的垃圾桶图标。容器会停止，网络会被移除。

> **Warning**
>
> By default, named volumes in your compose file are not removed when you run `docker compose down`. If you want to remove the volumes, you need to add the `--volumes` flag.
>
> ​	默认情况下，在运行 `docker compose down` 时不会删除 Compose 文件中的命名卷。如果你想删除卷，需要添加 `--volumes` 标志。
>
> The Docker Dashboard does not remove volumes when you delete the app stack.
>
> ​	删除应用栈时，Docker Dashboard 不会删除卷。

## 总结 Summary

In this section, you learned about Docker Compose and how it helps you simplify the way you define and share multi-service applications.

​	在本节中，你学习了 Docker Compose 以及它如何帮助简化多服务应用程序的定义和共享。

Related information: 相关信息：

- [Compose overview]({{< ref "/manuals/DockerCompose" >}}) - [Compose 概述]({{< ref "/manuals/DockerCompose" >}})
- [Compose file reference]({{< ref "/reference/Composefilereference" >}}) - [Compose 文件参考]({{< ref "/reference/Composefilereference" >}})
- [Compose CLI reference]({{< ref "/reference/CLIreference/docker/dockercompose" >}}) - 
- [Compose CLI 参考]({{< ref "/reference/CLIreference/docker/dockercompose" >}})

## 接下来 Next steps

Next, you'll learn about a few best practices you can use to improve your Dockerfile.

​	接下来，你将学习一些可以用于改进 Dockerfile 的最佳实践。

[Image-building best practices]({{< ref "/get-started/Dockerworkshop/Part8Image-buildingbestpractices" >}}) - [镜像构建最佳实践]({{< ref "/get-started/Dockerworkshop/Part8Image-buildingbestpractices" >}})
