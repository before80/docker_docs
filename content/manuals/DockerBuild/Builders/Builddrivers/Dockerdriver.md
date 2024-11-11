+++
title = "Docker 驱动"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/builders/drivers/docker/](https://docs.docker.com/build/builders/drivers/docker/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker driver - Docker 驱动

The Buildx Docker driver is the default driver. It uses the BuildKit server components built directly into the Docker Engine. The Docker driver requires no configuration.

​	Buildx 的 Docker 驱动是默认驱动。它使用直接内置在 Docker 引擎中的 BuildKit 服务器组件，且无需任何配置。

Unlike the other drivers, builders using the Docker driver can't be manually created. They're only created automatically from the Docker context.

​	与其他驱动不同，使用 Docker 驱动的构建器无法手动创建。它们仅能从 Docker 上下文自动创建。

Images built with the Docker driver are automatically loaded to the local image store.

​	使用 Docker 驱动构建的镜像会自动加载到本地镜像存储中。

## 概述 Synopsis



```console
# The Docker driver is used by buildx by default
# 默认情况下，buildx 使用 Docker 驱动
docker buildx build .
```

It's not possible to configure which BuildKit version to use, or to pass any additional BuildKit parameters to a builder using the Docker driver. The BuildKit version and parameters are preset by the Docker Engine internally.

​	无法配置要使用的 BuildKit 版本，也无法向使用 Docker 驱动的构建器传递任何额外的 BuildKit 参数。BuildKit 版本和参数由 Docker 引擎内部预设。

If you need additional configuration and flexibility, consider using the [Docker container driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockercontainerbuilddriver" >}}).

​	如果需要更多配置和灵活性，请考虑使用 [Docker 容器驱动]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockercontainerbuilddriver" >}})。

## 进一步阅读 Further reading

For more information on the Docker driver, see the [buildx reference](https://docs.docker.com/reference/cli/docker/buildx/create/#driver).

​	有关 Docker 驱动的更多信息，请参阅 [buildx 参考](https://docs.docker.com/reference/cli/docker/buildx/create/#driver)。
