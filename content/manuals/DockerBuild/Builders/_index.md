+++
title = "Builders"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/build/builders/](https://docs.docker.com/build/builders/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Builders

A builder is a BuildKit daemon that you can use to run your builds. BuildKit is the build engine that solves the build steps in a Dockerfile to produce a container image or other artifacts.

​	构建器是一个可以用于运行构建的 BuildKit 守护进程。BuildKit 是构建引擎，用于解析 Dockerfile 中的构建步骤，以生成容器镜像或其他产物。

You can create and manage builders, inspect them, and even connect to builders running remotely. You interact with builders using the Docker CLI.

​	您可以创建和管理构建器，检查它们，甚至连接到远程运行的构建器。您可以使用 Docker CLI 与构建器交互。

## 默认构建器 Default builder

Docker Engine automatically creates a builder that becomes the default backend for your builds. This builder uses the BuildKit library bundled with the daemon. This builder requires no configuration.

​	Docker 引擎会自动创建一个构建器，它成为构建的默认后端。这个构建器使用守护进程中捆绑的 BuildKit 库，不需要任何配置。

The default builder is directly bound to the Docker daemon and its [context]({{< ref "/manuals/DockerEngine/Manageresources/Dockercontexts" >}}). If you change the Docker context, your `default` builder refers to the new Docker context.

​	默认构建器直接绑定到 Docker 守护进程及其[上下文]({{< ref "/manuals/DockerEngine/Manageresources/Dockercontexts" >}})。如果更改了 Docker 上下文，您的 `default` 构建器将指向新的 Docker 上下文。

## 构建驱动 Build drivers

Buildx implements a concept of [build drivers]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}}) to refer to different builder configurations. The default builder created by the daemon uses the [`docker` driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockerdriver" >}}).

​	Buildx 实现了[构建驱动]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}})的概念，以指代不同的构建器配置。由守护进程创建的默认构建器使用 [`docker` 驱动]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockerdriver" >}})。

Buildx supports the following build drivers:

​	Buildx 支持以下构建驱动：

- `docker`: uses the BuildKit library bundled into the Docker daemon.
  - `docker`：使用 Docker 守护进程中捆绑的 BuildKit 库。

- `docker-container`: creates a dedicated BuildKit container using Docker.
  - `docker-container`：使用 Docker 创建一个专用的 BuildKit 容器。

- `kubernetes`: creates BuildKit pods in a Kubernetes cluster.
  - `kubernetes`：在 Kubernetes 集群中创建 BuildKit pods。

- `remote`: connects directly to a manually managed BuildKit daemon.
  - `remote`：直接连接到手动管理的 BuildKit 守护进程。

## 选定的构建器 Selected builder

Selected builder refers to the builder that's used by default when you run build commands.

​	选定的构建器是指在您运行构建命令时默认使用的构建器。

When you run a build, or interact with builders in some way using the CLI, you can use the optional `--builder` flag, or the `BUILDX_BUILDER` [environment variable](https://docs.docker.com/build/building/variables/#buildx_builder), to specify a builder by name. If you don't specify a builder, the selected builder is used.

​	当您运行构建或通过 CLI 与构建器交互时，可以使用可选的 `--builder` 标志或 `BUILDX_BUILDER` [环境变量](https://docs.docker.com/build/building/variables/#buildx_builder)按名称指定构建器。如果未指定构建器，将使用选定的构建器。

Use the `docker buildx ls` command to see the available builder instances. The asterisk (`*`) next to a builder name indicates the selected builder.

​	使用 `docker buildx ls` 命令可以查看可用的构建器实例。构建器名称旁边的星号 (`*`) 表示选定的构建器。

```console
$ docker buildx ls
NAME/NODE       DRIVER/ENDPOINT      STATUS   BUILDKIT PLATFORMS
default *       docker
  default       default              running  v0.11.6  linux/amd64, linux/amd64/v2, linux/amd64/v3, linux/386
my_builder      docker-container
  my_builder0   default              running  v0.11.6  linux/amd64, linux/amd64/v2, linux/amd64/v3, linux/386
```

### 选择不同的构建器 Select a different builder

To switch between builders, use the `docker buildx use <name>` command.

​	要在构建器之间切换，请使用 `docker buildx use <name>` 命令。

After running this command, the builder you specify is automatically selected when you invoke builds.

​	运行此命令后，在您执行构建时，会自动选择指定的构建器。

## 其他信息 Additional information

- For information about how to interact with and manage builders, see [Manage builders]({{< ref "/manuals/DockerBuild/Builders/Managebuilders" >}})
  - 有关如何与构建器交互并进行管理的信息，请参阅[管理构建器]({{< ref "/manuals/DockerBuild/Builders/Managebuilders" >}})
- To learn about different types of builders, see [Build drivers]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}})
  - 要了解不同类型的构建器，请参阅[构建驱动]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}})
