+++
title = "Bake"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/build/bake/](https://docs.docker.com/build/bake/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Bake

**实验性功能 Experimental**

Bake is an experimental feature, and we are looking for [feedback from users](https://github.com/docker/buildx/issues).

​	Bake 是一个实验性功能，我们正在寻求[用户反馈](https://github.com/docker/buildx/issues)。

Bake is a feature of Docker Buildx that lets you define your build configuraton using a declarative file, as opposed to specifying a complex CLI expression. It also lets you run multiple builds concurrently with a single invocation.

​	Bake 是 Docker Buildx 的一项功能，允许您使用声明性文件定义构建配置，而不必指定复杂的 CLI 表达式。它还允许您通过单次调用并行运行多个构建。

A Bake file can be written in HCL, JSON, or YAML formats, where the YAML format is an extension of a Docker Compose file. Here's an example Bake file in HCL format:

​	Bake 文件可以用 HCL、JSON 或 YAML 格式编写，其中 YAML 格式是 Docker Compose 文件的扩展。以下是一个 HCL 格式的 Bake 文件示例：



```hcl
group "default" {
  targets = ["frontend", "backend"]
}

target "frontend" {
  context = "./frontend"
  dockerfile = "frontend.Dockerfile"
  args = {
    NODE_VERSION = "22"
  }
  tags = ["myapp/frontend:latest"]
}

target "backend" {
  context = "./backend"
  dockerfile = "backend.Dockerfile"
  args = {
    GO_VERSION = "1.23"
  }
  tags = ["myapp/backend:latest"]
}
```

The `group` block defines a group of targets that can be built concurrently. Each `target` block defines a build target with its own configuration, such as the build context, Dockerfile, and tags.

​	`group` 块定义了可以并行构建的一组目标。每个 `target` 块定义一个构建目标及其自身的配置，如构建上下文、Dockerfile 和标签。

To invoke a build using the above Bake file, you can run:

​	要使用上述 Bake 文件执行构建，可以运行：



```console
$ docker buildx bake
```

This executes the `default` group, which builds the `frontend` and `backend` targets concurrently.

​	这将执行 `default` 组，该组并行构建 `frontend` 和 `backend` 目标。

## Get started

To learn how to get started with Bake, head over to the [Bake introduction]({{< ref "/manuals/DockerBuild/Bake/Introduction" >}}).

​	要了解如何开始使用 Bake，请前往 [Bake 介绍]({{< ref "/manuals/DockerBuild/Bake/Introduction" >}})。
