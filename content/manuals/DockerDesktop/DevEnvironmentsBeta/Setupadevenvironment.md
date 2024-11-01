+++
title = "设置开发环境"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/dev-environments/set-up/](https://docs.docker.com/desktop/dev-environments/set-up/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Set up a dev environment - 设置开发环境

> **Important**
>
> 
>
> Dev Environments is no longer under active development.
>
> ​	开发环境不再进行活跃开发。
>
> While the current functionality remains available, it may take us longer to respond to support requests.
>
> ​	尽管当前功能仍然可用，但我们可能会对支持请求的响应时间更长。

> **Changes to Dev Environments with Docker Desktop 4.13  - Docker Desktop 4.13 中开发环境的变更**
>
> Docker has simplified how you configure your dev environment project. All you need to get started is a `compose-dev.yaml` file. If you have an existing project with a `.docker/` folder this is automatically migrated the next time you launch.
>
> ​	Docker 已简化开发环境项目的配置。要开始，您只需要一个 `compose-dev.yaml` 文件。如果您有带 `.docker/` 文件夹的现有项目，系统会在您下一次启动时自动迁移。
>
> If you are using `.docker/docker-compose.yaml`, we move it to `../compose-dev.yaml`. If you are using `.docker/config.json`, we create a `../compose-dev.yaml` file with a single service named "app”. It is configured to use the image or Dockerfile referenced in the JSON as a starting point.
>
> ​	如果您使用的是 `.docker/docker-compose.yaml`，我们会将其移动到 `../compose-dev.yaml`。如果您使用的是 `.docker/config.json`，我们会创建一个包含单个服务“app”的 `../compose-dev.yaml` 文件，并以 JSON 中引用的镜像或 Dockerfile 作为起点。

To set up a dev environment, there are additional configuration steps to tell Docker Desktop how to build, start, and use the right image for your services.

​	要设置开发环境，您可以进行一些额外的配置步骤，以便 Docker Desktop 知道如何构建、启动和使用您的服务所需的正确镜像。

Dev Environments use an `compose-dev.yaml` file located at the root of your project. This file allows you to define the image required for a dedicated service, the ports you'd like to expose, along with additional configuration options.

​	开发环境使用位于项目根目录的 `compose-dev.yaml` 文件。此文件允许您定义所需的服务镜像、想要暴露的端口以及其他配置选项。

The following is an example `compose-dev.yaml` file.

​	以下是一个示例 `compose-dev.yaml` 文件：

```yaml
version: "3.7"
services:
  backend:
    build:
      context: backend
      target: development
    secrets:
      - db-password
    depends_on:
      - db
  db:
    image: mariadb
    restart: always
    healthcheck:
      test: [ "CMD", "mysqladmin", "ping", "-h", "127.0.0.1", "--silent" ]
      interval: 3s
      retries: 5
      start_period: 30s
    secrets:
      - db-password
    volumes:
      - db-data:/var/lib/mysql
    environment:
      - MYSQL_DATABASE=example
      - MYSQL_ROOT_PASSWORD_FILE=/run/secrets/db-password
    expose:
      - 3306
  proxy:
    build: proxy
    ports:
      - 8080:80
    depends_on:
      - backend
volumes:
  db-data:
secrets:
  db-password:
    file: db/password.txt
```

In the yaml file, the build context `backend` specifies that that the container should be built using the `development` stage (`target` attribute) of the Dockerfile located in the `backend` directory (`context` attribute)

​	在此 yaml 文件中，构建上下文 `backend` 指定容器应使用 `development` 阶段（`target` 属性）构建，该阶段位于 `backend` 目录中的 Dockerfile。

The `development` stage of the Dockerfile is defined as follows:

​	Dockerfile 的 `development` 阶段如下所示：



```dockerfile
# syntax=docker/dockerfile:1
FROM golang:1.16-alpine AS build
WORKDIR /go/src/github.com/org/repo
COPY . .
RUN go build -o server .
FROM build AS development
RUN apk update \
    && apk add git
CMD ["go", "run", "main.go"]
FROM alpine:3.12
EXPOSE 8000
COPY --from=build /go/src/github.com/org/repo/server /server
CMD ["/server"]
```

The `development` target uses a `golang:1.16-alpine` image with all dependencies you need for development. You can start your project directly from VS Code and interact with the others applications or services such as the database or the frontend.

​	`development` 目标使用 `golang:1.16-alpine` 镜像，包含开发所需的所有依赖项。您可以直接从 VS Code 启动项目并与数据库或前端等其他应用程序或服务交互。

In the example, the Docker Compose files are the same. However, they could be different and the services defined in the main Compose file may use other targets to build or directly reference other images.

​	在此示例中，Docker Compose 文件相同，但它们可以不同，主 Compose 文件中定义的服务可能使用其他目标进行构建或直接引用其他镜像。

## What's next?

Learn how to [distribute your dev environment]({{< ref "/manuals/DockerDesktop/DevEnvironmentsBeta/Distributeyourdevenvironment" >}})

​	了解如何 [分发您的开发环境]({{< ref "/manuals/DockerDesktop/DevEnvironmentsBeta/Distributeyourdevenvironment" >}})
