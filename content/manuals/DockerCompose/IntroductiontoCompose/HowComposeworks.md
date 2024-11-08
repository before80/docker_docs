+++
title = "Docker Compose 的工作原理"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/intro/compose-application-model/](https://docs.docker.com/compose/intro/compose-application-model/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# How Compose works - Docker Compose 的工作原理

With Docker Compose you use a YAML configuration file, known as the [Compose file](https://docs.docker.com/compose/intro/compose-application-model/#the-compose-file), to configure your application’s services, and then you create and start all the services from your configuration with the [Compose CLI](https://docs.docker.com/compose/intro/compose-application-model/#cli).

​	使用 Docker Compose，可以通过 YAML 配置文件（即 [Compose 文件](https://docs.docker.com/compose/intro/compose-application-model/#the-compose-file)）配置应用程序的服务，然后使用 [Compose CLI](https://docs.docker.com/compose/intro/compose-application-model/#cli) 创建并启动所有服务。

The Compose file, or `compose.yaml` file, follows the rules provided by the [Compose Specification]({{< ref "/reference/Composefilereference" >}}) in how to define multi-container applications. This is the Docker Compose implementation of the formal [Compose Specification](https://github.com/compose-spec/compose-spec).

​	Compose 文件（或 `compose.yaml` 文件）遵循 [Compose 规范]({{< ref "/reference/Composefilereference" >}}) 提供的定义多容器应用程序的规则。这是 Docker Compose 对正式 [Compose 规范](https://github.com/compose-spec/compose-spec) 的实现。

## Compose 文件 The Compose file

The default path for a Compose file is `compose.yaml` (preferred) or `compose.yml` that is placed in the working directory. Compose also supports `docker-compose.yaml` and `docker-compose.yml` for backwards compatibility of earlier versions. If both files exist, Compose prefers the canonical `compose.yaml`.

​	Compose 文件的默认路径为当前工作目录下的 `compose.yaml`（推荐）或 `compose.yml`。为了兼容旧版本，Compose 还支持 `docker-compose.yaml` 和 `docker-compose.yml`。如果两种文件都存在，Compose 会优先使用标准的 `compose.yaml`。

You can use [fragments]({{< ref "/reference/Composefilereference/Fragments" >}}) and [extensions]({{< ref "/reference/Composefilereference/Extensions" >}}) to keep your Compose file efficient and easy to maintain.

​	您可以使用[片段]({{< ref "/reference/Composefilereference/Fragments" >}})和[扩展]({{< ref "/reference/Composefilereference/Extensions" >}})来保持 Compose 文件的高效和易于维护。

Multiple Compose files can be [merged]({{< ref "/reference/Composefilereference/Merge" >}}) together to define the application model. The combination of YAML files is implemented by appending or overriding YAML elements based on the Compose file order you set. Simple attributes and maps get overridden by the highest order Compose file, lists get merged by appending. Relative paths are resolved based on the first Compose file's parent folder, whenever complimentary files being merged are hosted in other folders. As some Compose file elements can both be expressed as single strings or complex objects, merges apply to the expanded form. For more information, see [Working with multiple Compose files]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles" >}}).

​	多个 Compose 文件可以[合并]({{< ref "/reference/Composefilereference/Merge" >}})在一起，以定义应用程序模型。YAML 文件组合通过基于指定的 Compose 文件顺序附加或覆盖 YAML 元素来实现。简单属性和映射会被最高优先级的 Compose 文件覆盖，列表则通过附加进行合并。相对路径基于第一个 Compose 文件的父文件夹解析。如果合并的辅助文件位于其他文件夹中，则相对路径按此规则处理。更多信息，请参阅[使用多个 Compose 文件]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles" >}})。

If you want to reuse other Compose files, or factor out parts of your application model into separate Compose files, you can also use [`include`]({{< ref "/reference/Composefilereference/Include" >}}). This is useful if your Compose application is dependent on another application which is managed by a different team, or needs to be shared with others.

​	如果您希望重用其他 Compose 文件，或将应用程序模型的部分功能分离到单独的 Compose 文件中，可以使用 [`include`]({{< ref "/reference/Composefilereference/Include" >}})。这在您的 Compose 应用程序依赖于其他团队管理的应用程序或需要与他人共享时非常有用。

## CLI

The Docker CLI lets you interact with your Docker Compose applications through the `docker compose` command, and its subcommands. Using the CLI, you can manage the lifecycle of your multi-container applications defined in the `compose.yaml` file. The CLI commands enable you to start, stop, and configure your applications effortlessly.

​	Docker CLI 允许您通过 `docker compose` 命令及其子命令与 Docker Compose 应用程序进行交互。使用 CLI，您可以轻松管理 `compose.yaml` 文件中定义的多容器应用程序的生命周期。CLI 命令使您可以轻松启动、停止和配置应用程序。

### 主要命令 Key commands

To start all the services defined in your `compose.yaml` file:

​	启动 `compose.yaml` 文件中定义的所有服务：



```console
$ docker compose up
```

To stop and remove the running services:

​	停止并移除运行中的服务：



```console
$ docker compose down 
```

If you want to monitor the output of your running containers and debug issues, you can view the logs with:

​	查看运行容器的输出日志，帮助监控和调试问题：

```console
$ docker compose logs
```

To lists all the services along with their current status:

​	列出所有服务及其当前状态：

```console
$ docker compose ps
```

For a full list of all the Compose CLI commands, see the [reference documentation]({{< ref "/reference/CLIreference/docker/dockercompose" >}}).

​	有关所有 Compose CLI 命令的完整列表，请参见[参考文档]({{< ref "/reference/CLIreference/docker/dockercompose" >}})。

## 示例说明 Illustrative example

The following example illustrates the Compose concepts outlined above. The example is non-normative.

​	以下示例说明了上述 Compose 概念。此示例为非规范性示例。

Consider an application split into a frontend web application and a backend service.

​	考虑一个包含前端 Web 应用程序和后端服务的应用程序。

The frontend is configured at runtime with an HTTP configuration file managed by infrastructure, providing an external domain name, and an HTTPS server certificate injected by the platform's secured secret store.

​	前端通过基础设施管理的 HTTP 配置文件进行运行时配置，提供外部域名，并通过平台的安全密钥存储注入 HTTPS 服务器证书。

The backend stores data in a persistent volume.

​	后端将数据存储在持久卷中。

Both services communicate with each other on an isolated back-tier network, while the frontend is also connected to a front-tier network and exposes port 443 for external usage.

​	这两个服务在隔离的 back-tier 网络上相互通信，前端还连接到 front-tier 网络并对外暴露端口 443。

![Compose application example](HowComposeworks_img/compose-application.webp)

The example application is composed of the following parts:

​	示例应用程序包含以下部分：

- 2 services, backed by Docker images: `webapp` and `database`
  - 2 个服务，由 Docker 镜像支持：`webapp` 和 `database`

- 1 secret (HTTPS certificate), injected into the frontend
  - 1 个密钥（HTTPS 证书），注入到前端

- 1 configuration (HTTP), injected into the frontend
  - 1 个配置（HTTP），注入到前端

- 1 persistent volume, attached to the backend
  - 1 个持久卷，附加到后端

- 2 networks
  - 2 个网络



```yml
services:
  frontend:
    image: example/webapp
    ports:
      - "443:8043"
    networks:
      - front-tier
      - back-tier
    configs:
      - httpd-config
    secrets:
      - server-certificate

  backend:
    image: example/database
    volumes:
      - db-data:/etc/data
    networks:
      - back-tier

volumes:
  db-data:
    driver: flocker
    driver_opts:
      size: "10GiB"

configs:
  httpd-config:
    external: true

secrets:
  server-certificate:
    external: true

networks:
  # The presence of these objects is sufficient to define them
  #	这些对象的存在本身就足以定义它们。
  front-tier: {}
  back-tier: {}
```

The `docker compose up` command starts the `frontend` and `backend` services, create the necessary networks and volumes, and injects the configuration and secret into the frontend service.

​	`docker compose up` 命令启动 `frontend` 和 `backend` 服务，创建所需的网络和卷，并将配置和密钥注入到前端服务中。

`docker compose ps` provides a snapshot of the current state of your services, making it easy to see which containers are running, their status, and the ports they are using:

​	`docker compose ps` 提供当前服务状态的快照，帮助您轻松查看哪些容器正在运行，它们的状态以及使用的端口：

```text
$ docker compose ps

NAME                IMAGE                COMMAND                  SERVICE             CREATED             STATUS              PORTS
example-frontend-1  example/webapp       "nginx -g 'daemon of…"   frontend            2 minutes ago       Up 2 minutes        0.0.0.0:443->8043/tcp
example-backend-1   example/database     "docker-entrypoint.s…"   backend             2 minutes ago       Up 2 minutes
```

## What's next

- [Quickstart 快速入门]({{< ref "/manuals/DockerCompose/Quickstart" >}})
- [Explore some sample applications 探索一些示例应用程序]({{< ref "/manuals/DockerCompose/Supportandfeedback/Sampleapps" >}})
- [Familiarize yourself with the Compose Specification 熟悉 Compose 规范]({{< ref "/reference/Composefilereference" >}})
