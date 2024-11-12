+++
title = "缓存"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/cache/](https://docs.docker.com/build/cache/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker build cache - Docker 构建缓存

When you build the same Docker image multiple times, knowing how to optimize the build cache is a great tool for making sure the builds run fast.

​	当多次构建相同的 Docker 镜像时，了解如何优化构建缓存是确保构建运行速度快的重要工具。

## 构建缓存的工作原理 How the build cache works

Understanding Docker's build cache helps you write better Dockerfiles that result in faster builds.

​	理解 Docker 的构建缓存有助于编写更高效的 Dockerfile，从而加速构建。

The following example shows a small Dockerfile for a program written in C.

​	以下示例展示了一个用于编写 C 程序的小型 Dockerfile。



```dockerfile
# syntax=docker/dockerfile:1
FROM ubuntu:latest

RUN apt-get update && apt-get install -y build-essentials
COPY main.c Makefile /src/
WORKDIR /src/
RUN make build
```

Each instruction in this Dockerfile translates to a layer in your final image. You can think of image layers as a stack, with each layer adding more content on top of the layers that came before it:

​	此 Dockerfile 中的每条指令都会转换为最终镜像中的一个层。可以将镜像层想象成一个堆叠，每一层都在前一层之上添加更多内容：

![Image layer diagram](_index_img/cache-stack.png)

Whenever a layer changes, that layer will need to be re-built. For example, suppose you make a change to your program in the `main.c` file. After this change, the `COPY` command will have to run again in order for those changes to appear in the image. In other words, Docker will invalidate the cache for this layer.

​	每当某一层发生更改，该层都需要重新构建。例如，假设在 `main.c` 文件中对程序进行了更改。此更改后，`COPY` 命令将需要重新执行，以使这些更改出现在镜像中。换句话说，Docker 会使该层的缓存失效。

If a layer changes, all other layers that come after it are also affected. When the layer with the `COPY` command gets invalidated, all layers that follow will need to run again, too:

​	如果一层发生更改，所有随后的层也会受到影响。当包含 `COPY` 命令的层被缓存失效时，后续的所有层都需要重新执行：

![Image layer diagram, showing cache invalidation](_index_img/cache-stack-invalidated.png)

And that's the Docker build cache in a nutshell. Once a layer changes, then all downstream layers need to be rebuilt as well. Even if they wouldn't build anything differently, they still need to re-run.

​	简而言之，这就是 Docker 构建缓存的基本原理。一旦某一层发生更改，则所有下游层也需要重新构建。即使它们不会产生不同的构建内容，它们也需要重新执行。

## 其他资源 Other resources

For more information on using cache to do efficient builds, see:

​	有关如何利用缓存进行高效构建的更多信息，请参阅：

- [Cache invalidation]({{< ref "/manuals/DockerBuild/Cache/Buildcacheinvalidation" >}})
  - [缓存失效]({{< ref "/manuals/DockerBuild/Cache/Buildcacheinvalidation" >}})

- [Optimize build cache]({{< ref "/manuals/DockerBuildCloud/Optimization" >}})
  - [优化构建缓存]({{< ref "/manuals/DockerBuildCloud/Optimization" >}})
- [Garbage collection]({{< ref "/manuals/DockerBuild/Cache/Buildgarbagecollection" >}})
  - [垃圾收集]({{< ref "/manuals/DockerBuild/Cache/Buildgarbagecollection" >}})
- [Cache storage backends]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends" >}})
  - [缓存存储后端]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends" >}})
