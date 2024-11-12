+++
title = "Contexts"
date = 2024-10-23T14:54:40+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/build/bake/contexts/](https://docs.docker.com/build/bake/contexts/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Using Bake with additional contexts - 使用 Bake 和附加上下文

In addition to the main `context` key that defines the build context, each target can also define additional named contexts with a map defined with key `contexts`. These values map to the `--build-context` flag in the [build command](https://docs.docker.com/reference/cli/docker/buildx/build/#build-context).

​	除了定义构建上下文的主要 `context` 键之外，每个目标还可以使用键 `contexts` 定义带有名称的附加上下文。这些值映射到 [构建命令](https://docs.docker.com/reference/cli/docker/buildx/build/#build-context) 中的 `--build-context` 参数。

Inside the Dockerfile these contexts can be used with the `FROM` instruction or `--from` flag.

​	在 Dockerfile 中，可以使用 `FROM` 指令或 `--from` 参数来引用这些上下文。

Supported context values are:

​	支持的上下文值包括：

- Local filesystem directories

  - 本地文件系统目录

- Container images

  - 容器镜像

- Git URLs
- HTTP URLs
- Name of another target in the Bake file

  - Bake 文件中的另一个目标名称

  

## 固定 Alpine 镜像 Pinning alpine image



```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
RUN echo "Hello world"
```



```hcl
# docker-bake.hcl
target "app" {
  contexts = {
    alpine = "docker-image://alpine:3.13"
  }
}
```

## 使用辅助源目录 Using a secondary source directory



```dockerfile
# syntax=docker/dockerfile:1
FROM scratch AS src

FROM golang
COPY --from=src . .
```



```hcl
# docker-bake.hcl
target "app" {
  contexts = {
    src = "../path/to/source"
  }
}
```

## 使用另一个目标作为构建上下文 Using a target as a build context

To use a result of one target as a build context of another, specify the target name with `target:` prefix.

​	要使用一个目标的结果作为另一个目标的构建上下文，请使用 `target:` 前缀指定目标名称。



```dockerfile
# syntax=docker/dockerfile:1
FROM baseapp
RUN echo "Hello world"
```



```hcl
# docker-bake.hcl
target "base" {
  dockerfile = "baseapp.Dockerfile"
}

target "app" {
  contexts = {
    baseapp = "target:base"
  }
}
```

In most cases you should just use a single multi-stage Dockerfile with multiple targets for similar behavior. This case is only recommended when you have multiple Dockerfiles that can't be easily merged into one.

​	在大多数情况下，应该使用包含多个目标的单个多阶段 Dockerfile 来实现类似的行为。仅当您有多个难以合并的 Dockerfile 时，才推荐这种方法。

## 去重上下文传输 Deduplicate context transfer

> **Note**
>
> As of Buildx version 0.17.0 and later, Bake automatically deduplicates context transfer for targets that share the same context. In addition to Buildx version 0.17.0, the builder must be running BuildKit version 0.16.0 or later, and the Dockerfile syntax must be `docker/dockerfile:1.10` or later.
>
> ​	从 Buildx 版本 0.17.0 起，对于共享相同上下文的目标，Bake 会自动去重上下文传输。除了 Buildx 版本 0.17.0，构建器还需要运行 BuildKit 版本 0.16.0 或更高版本，Dockerfile 语法必须为 `docker/dockerfile:1.10` 或更高版本。
>
> If you meet these requirements, you don't need to manually deduplicate context transfer as described in this section.
>
> ​	如果满足这些要求，则不需要手动去重上下文传输。
>
> - To check your Buildx version, run `docker buildx version`.
>   - 要检查 Buildx 版本，运行 `docker buildx version`。
> - To check your BuildKit version, run `docker buildx inspect --bootstrap` and look for the `BuildKit version` field.
>   - 要检查 BuildKit 版本，运行 `docker buildx inspect --bootstrap` 并查找 `BuildKit version` 字段。
> - To check your Dockerfile syntax version, check the `syntax` [parser directive](https://docs.docker.com/reference/dockerfile/#syntax) in your Dockerfile. If it's not present, the default version whatever comes bundled with your current version of BuildKit. To set the version explicitly, add `#syntax=docker/dockerfile:1.10` at the top of your Dockerfile.
>   - 要检查 Dockerfile 语法版本，请检查 Dockerfile 中的 `syntax` [解析指令](https://docs.docker.com/reference/dockerfile/#syntax)。如果未提供，默认使用当前 BuildKit 版本中的版本。要显式设置版本，请在 Dockerfile 顶部添加 `#syntax=docker/dockerfile:1.10`。

When you build targets concurrently, using groups, build contexts are loaded independently for each target. If the same context is used by multiple targets in a group, that context is transferred once for each time it's used. This can result in significant impact on build time, depending on your build configuration. For example, say you have a Bake file that defines the following group of targets:

​	当您使用组同时构建多个目标时，每个目标的构建上下文会独立加载。如果一个组中的多个目标使用相同的上下文，则每次使用该上下文时都会传输一次。根据您的构建配置，这可能会显著影响构建时间。例如，假设您有一个 Bake 文件，定义了以下目标组：

​	

```hcl
group "default" {
  targets = ["target1", "target2"]
}

target "target1" {
  target = "target1"
  context = "."
}

target "target2" {
  target = "target2"
  context = "."
}
```

In this case, the context `.` is transferred twice when you build the default group: once for `target1` and once for `target2`.

​	在此情况下，构建 `default` 组时，`context` 为 `.` 的上下文会被传输两次：一次用于 `target1`，一次用于 `target2`。

If your context is small, and if you are using a local builder, duplicate context transfers may not be a big deal. But if your build context is big, or you have a large number of targets, or you're transferring the context over a network to a remote builder, context transfer becomes a performance bottleneck.

​	如果上下文很小，并且使用的是本地构建器，那么重复的上下文传输可能不会造成太大影响。但如果构建上下文很大，或您有大量目标，或者正在通过网络将上下文传输到远程构建器，那么上下文传输会成为性能瓶颈。

To avoid transferring the same context multiple times, you can define a named context that only loads the context files, and have each target that needs those files reference that named context. For example, the following Bake file defines a named target `ctx`, which is used by both `target1` and `target2`:

​	为避免多次传输相同的上下文，您可以定义一个命名的上下文，该上下文仅加载一次上下文文件，并让每个需要这些文件的目标引用该命名上下文。例如，以下 Bake 文件定义了一个名为 `ctx` 的目标，供 `target1` 和 `target2` 使用：



```hcl
group "default" {
  targets = ["target1", "target2"]
}

target "ctx" {
  context = "."
  target = "ctx"
}

target "target1" {
  target = "target1"
  contexts = {
    ctx = "target:ctx"
  }
}

target "target2" {
  target = "target2"
  contexts = {
    ctx = "target:ctx"
  }
}
```

The named context `ctx` represents a Dockerfile stage, which copies the files from its context (`.`). Other stages in the Dockerfile can now reference the `ctx` named context and, for example, mount its files with `--mount=from=ctx`.

​	命名上下文 `ctx` 代表一个 Dockerfile 阶段，它从其上下文（`.`）中复制文件。Dockerfile 中的其他阶段现在可以引用 `ctx` 命名上下文，例如，可以使用 `--mount=from=ctx` 挂载其文件。



```dockerfile
FROM scratch AS ctx
COPY --link . .

FROM golang:alpine AS target1
WORKDIR /work
RUN --mount=from=ctx \
    go build -o /out/client ./cmd/client \

FROM golang:alpine AS target2
WORKDIR /work
RUN --mount=from=ctx \
    go build -o /out/server ./cmd/server
```
