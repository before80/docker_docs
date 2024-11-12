+++
title = "目标"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/bake/targets/](https://docs.docker.com/build/bake/targets/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Bake targets - Bake 目标

A target in a Bake file represents a build invocation. It holds all the information you would normally pass to a `docker build` command using flags.

​	Bake 文件中的目标代表一个构建调用。它包含了通常通过标志传递给 `docker build` 命令的所有信息。



```hcl
target "webapp" {
  dockerfile = "webapp.Dockerfile"
  tags = ["docker.io/username/webapp:latest"]
  context = "https://github.com/username/webapp"
}
```

To build a target with Bake, pass name of the target to the `bake` command.

​	要使用 Bake 构建目标，请将目标名称传递给 `bake` 命令。



```console
$ docker buildx bake webapp
```

You can build multiple targets at once by passing multiple target names to the `bake` command.

​	您可以通过传递多个目标名称给 `bake` 命令一次构建多个目标。



```console
$ docker buildx bake webapp api tests
```

## 默认目标 Default target

If you don't specify a target when running `docker buildx bake`, Bake will build the target named `default`.

​	如果在运行 `docker buildx bake` 时未指定目标，Bake 将构建名为 `default` 的目标。



```hcl
target "default" {
  dockerfile = "webapp.Dockerfile"
  tags = ["docker.io/username/webapp:latest"]
  context = "https://github.com/username/webapp"
}
```

To build this target, run `docker buildx bake` without any arguments:

​	要构建此目标，请直接运行 `docker buildx bake`：



```console
$ docker buildx bake
```

## 目标属性 Target properties

The properties you can set for a target closely resemble the CLI flags for `docker build`, with a few additional properties that are specific to Bake.

​	您可以为目标设置的属性与 `docker build` 的 CLI 标志非常相似，并包括一些 Bake 特有的属性。

For all the properties you can set for a target, see the [Bake reference]({{< ref "/manuals/DockerBuild/Bake/Bakefilereference#target" >}}).

​	有关可以为目标设置的所有属性，请参阅 [Bake 参考文档]({{< ref "/manuals/DockerBuild/Bake/Bakefilereference#target" >}})。

## 分组目标 Grouping targets

You can group targets together using the `group` block. This is useful when you want to build multiple targets at once.

​	您可以使用 `group` 块将目标分组，这在您需要同时构建多个目标时非常有用。



```hcl
group "all" {
  targets = ["webapp", "api", "tests"]
}

target "webapp" {
  dockerfile = "webapp.Dockerfile"
  tags = ["docker.io/username/webapp:latest"]
  context = "https://github.com/username/webapp"
}

target "api" {
  dockerfile = "api.Dockerfile"
  tags = ["docker.io/username/api:latest"]
  context = "https://github.com/username/api"
}

target "tests" {
  dockerfile = "tests.Dockerfile"
  contexts = {
    webapp = "target:webapp",
    api = "target:api",
  }
  output = ["type=local,dest=build/tests"]
  context = "."
}
```

To build all the targets in a group, pass the name of the group to the `bake` command.

​	要构建组中的所有目标，将组名称传递给 `bake` 命令。



```console
$ docker buildx bake all
```

## 其他资源 Additional resources

Refer to the following pages to learn more about Bake's features:

​	参阅以下页面了解更多 Bake 的功能：

- Learn how to use [variables]({{< ref "/manuals/DockerBuild/Bake/Variables" >}}) in Bake to make your build configuration more flexible.
  - 学习如何在 Bake 中使用 [变量]({{< ref "/manuals/DockerBuild/Bake/Variables" >}})，让构建配置更灵活。

- Learn how you can use matrices to build multiple images with different configurations in [Matrices]({{< ref "/manuals/DockerBuild/Bake/Matrixtargets" >}}).
  - 了解如何使用矩阵来构建具有不同配置的多个镜像，详见 [矩阵]({{< ref "/manuals/DockerBuild/Bake/Matrixtargets" >}})。
- Head to the [Bake file reference]({{< ref "/manuals/DockerBuild/Bake/Bakefilereference" >}}) to learn about all the properties you can set in a Bake file, and its syntax.
  - 查看 [Bake 文件参考]({{< ref "/manuals/DockerBuild/Bake/Bakefilereference" >}})，了解 Bake 文件中的所有属性及其语法。
