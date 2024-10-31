+++
title = "在生产环境中使用 Compose"
date = 2024-10-23T14:54:40+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/compose/how-tos/production/](https://docs.docker.com/compose/how-tos/production/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use Compose in production - 在生产环境中使用 Compose

When you define your app with Compose in development, you can use this definition to run your application in different environments such as CI, staging, and production.

​	当您在开发环境中使用 Compose 定义应用时，可以将此定义用于不同的环境，例如 CI、预发布和生产环境。

The easiest way to deploy an application is to run it on a single server, similar to how you would run your development environment. If you want to scale up your application, you can run Compose apps on a Swarm cluster.

​	部署应用的最简单方法是在单个服务器上运行它，这类似于运行开发环境的方式。如果您希望扩容应用，可以在 Swarm 集群上运行 Compose 应用。

### 为生产修改 Compose 文件 Modify your Compose file for production

You may need to make changes to your app configuration to make it ready for production. These changes might include:

​	您可能需要更改应用配置，使其适合生产环境。这些更改可能包括：

- Removing any volume bindings for application code, so that code stays inside the container and can't be changed from outside
  - 移除应用代码的卷绑定，使代码保留在容器内，防止从外部更改

- Binding to different ports on the host
  - 绑定到主机上的不同端口

- Setting environment variables differently, such as reducing the verbosity of logging, or to specify settings for external services such as an email server
  - 不同地设置环境变量，例如减少日志记录的详细程度，或指定外部服务（如邮件服务器）的设置

- Specifying a restart policy like [`restart: always`](https://docs.docker.com/reference/compose-file/services/#restart)to avoid downtime
  - 指定重启策略（如 [`restart: always`](https://docs.docker.com/reference/compose-file/services/#restart)）以避免停机

- Adding extra services such as a log aggregator
  - 添加额外服务，例如日志聚合器


For this reason, consider defining an additional Compose file, for example `production.yml`, which specifies production-appropriate configuration. This configuration file only needs to include the changes you want to make from the original Compose file. The additional Compose file is then applied over the original `compose.yml` to create a new configuration.

​	因此，建议定义一个额外的 Compose 文件，例如 `production.yml`，以指定适合生产环境的配置。该配置文件只需包含您希望与原始 Compose 文件不同的更改内容。然后，将该额外的 Compose 文件应用到原始的 `compose.yml` 上以创建新的配置。

Once you have a second configuration file, you can use it with the `-f` option:

​	拥有第二个配置文件后，可以使用 `-f` 选项来运行它：

```console
$ docker compose -f compose.yml -f production.yml up -d
```

See [Using multiple compose files]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles" >}}) for a more complete example, and other options.

​	有关更完整的示例和其他选项，请参阅[使用多个 Compose 文件]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles" >}})。

### 部署更改 Deploying changes

When you make changes to your app code, remember to rebuild your image and recreate your app's containers. To redeploy a service called `web`, use:

​	在更改应用代码时，请记得重新构建镜像并重新创建应用的容器。要重新部署名为 `web` 的服务，请使用以下命令：

```console
$ docker compose build web
$ docker compose up --no-deps -d web
```

This first command rebuilds the image for `web` and then stops, destroys, and recreates just the `web` service. The `--no-deps` flag prevents Compose from also recreating any services which `web` depends on.

​	第一个命令为 `web` 重建镜像，然后停止、销毁并重新创建 `web` 服务。`--no-deps` 标志可防止 Compose 同时重新创建 `web` 依赖的任何服务。

### 在单个服务器上运行 Compose - Running Compose on a single server

You can use Compose to deploy an app to a remote Docker host by setting the `DOCKER_HOST`, `DOCKER_TLS_VERIFY`, and `DOCKER_CERT_PATH` environment variables appropriately. For more information, see [pre-defined environment variables]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Pre-definedenvironmentvariables" >}}).

​	可以通过适当设置 `DOCKER_HOST`、`DOCKER_TLS_VERIFY` 和 `DOCKER_CERT_PATH` 环境变量，将应用部署到远程 Docker 主机上。有关更多信息，请参阅[预定义环境变量]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Pre-definedenvironmentvariables" >}})。

Once you've set up your environment variables, all the normal `docker compose` commands work with no further configuration.

​	设置环境变量后，所有常规的 `docker compose` 命令无需进一步配置即可正常使用。
