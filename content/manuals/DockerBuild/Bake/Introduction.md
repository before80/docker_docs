+++
title = "简介"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/bake/introduction/](https://docs.docker.com/build/bake/introduction/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Introduction to Bake - Bake 简介

Bake is an abstraction for the `docker build` command that lets you more easily manage your build configuration (CLI flags, environment variables, etc.) in a consistent way for everyone on your team.

​	Bake 是 `docker build` 命令的抽象，可以让您更轻松地以一致的方式管理构建配置（CLI 标志、环境变量等），适用于团队中的每个人。

Bake is a command built into the Buildx CLI, so as long as you have Buildx installed, you also have access to bake, via the `docker buildx bake` command.

​	Bake 是 Buildx CLI 中的一个命令，因此只要您安装了 Buildx，就可以通过 `docker buildx bake` 命令访问 Bake。

## 使用 Bake 构建项目 Building a project with Bake

Here's a simple example of a `docker build` command:

​	以下是一个简单的 `docker build` 命令示例：





```console
$ docker build -f Dockerfile -t myapp:latest .
```

This command builds the Dockerfile in the current directory and tags the resulting image as `myapp:latest`.

​	该命令构建当前目录中的 Dockerfile，并将生成的镜像标记为 `myapp:latest`。

To express the same build configuration using Bake:

​	使用 Bake 表达相同的构建配置：

docker-bake.hcl



```hcl
target "myapp" {
  context = "."
  dockerfile = "Dockerfile"
  tags = ["myapp:latest"]
}
```

Bake provides a structured way to manage your build configuration, and it saves you from having to remember all the CLI flags for `docker build` every time. With this file, building the image is as simple as running:

​	Bake 提供了一种结构化的方式来管理您的构建配置，并且可以让您不用每次都记住 `docker build` 的所有 CLI 标志。通过这个文件，构建镜像只需运行以下命令：



```console
$ docker buildx bake myapp
```

For simple builds, the difference between `docker build` and `docker buildx bake` is minimal. However, as your build configuration grows more complex, Bake provides a more structured way to manage that complexity, that would be difficult to manage with CLI flags for the `docker build`. It also provides a way to share build configurations across your team, so that everyone is building images in a consistent way, with the same configuration.

​	对于简单的构建，`docker build` 和 `docker buildx bake` 之间的区别很小。然而，当构建配置变得更加复杂时，Bake 提供了一种更结构化的方式来管理复杂性，而使用 `docker build` 的 CLI 标志来管理这些配置将变得困难。它还为团队提供了共享构建配置的方式，从而确保每个人以一致的配置构建镜像。

## Bake 文件格式 The Bake file format

You can write Bake files in HCL, YAML (Docker Compose files), or JSON. In general, HCL is the most expressive and flexible format, which is why you'll see it used in most of the examples in this documentation, and in projects that use Bake.

​	您可以使用 HCL、YAML（Docker Compose 文件）或 JSON 编写 Bake 文件。通常，HCL 是最具表现力和灵活性的格式，因此您将在本指南的大部分示例以及使用 Bake 的项目中看到它。

The properties that can be set for a target closely resemble the CLI flags for `docker build`. For instance, consider the following `docker build` command:

​	为目标设置的属性与 `docker build` 的 CLI 标志非常相似。例如，以下是一个 `docker build` 命令：



```console
$ docker build \
  -f Dockerfile \
  -t myapp:latest \
  --build-arg foo=bar \
  --no-cache \
  --platform linux/amd64,linux/arm64 \
  .
```

The Bake equivalent would be:

​	Bake 的等效配置如下：



```hcl
target "myapp" {
  context = "."
  dockerfile = "Dockerfile"
  tags = ["myapp:latest"]
  args = {
    foo = "bar"
  }
  no-cache = true
  platforms = ["linux/amd64", "linux/arm64"]
}
```

## Next steps

To learn more about using Bake, see the following topics:

​	要了解更多关于使用 Bake 的内容，请参阅以下主题：

- Learn how to define and use [targets]({{< ref "/manuals/DockerBuild/Bake/Targets" >}}) in Bake
  - 了解如何在 Bake 中定义和使用[目标]({{< ref "/manuals/DockerBuild/Bake/Targets" >}})

- To see all the properties that can be set for a target, refer to the [Bake file reference]({{< ref "/manuals/DockerBuild/Bake/Bakefilereference" >}}).
  - 查看所有可为目标设置的属性，请参考 [Bake 文件参考]({{< ref "/manuals/DockerBuild/Bake/Bakefilereference" >}})。
