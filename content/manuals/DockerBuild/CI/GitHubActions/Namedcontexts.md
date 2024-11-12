+++
title = "命名上下文"
date = 2024-10-23T14:54:40+08:00
weight = 100
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/ci/github-actions/named-contexts/](https://docs.docker.com/build/ci/github-actions/named-contexts/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Named contexts with GitHub Actions - 在 GitHub Actions 中使用命名上下文

You can define [additional build contexts](https://docs.docker.com/reference/cli/docker/buildx/build/#build-context), and access them in your Dockerfile with `FROM name` or `--from=name`. When Dockerfile defines a stage with the same name it's overwritten.

​	您可以定义[其他构建上下文](https://docs.docker.com/reference/cli/docker/buildx/build/#build-context)，并在 Dockerfile 中通过 `FROM name` 或 `--from=name` 访问它们。当 Dockerfile 中定义的阶段与该名称相同时，它会被覆盖。

This can be useful with GitHub Actions to reuse results from other builds or pin an image to a specific tag in your workflow.

​	在使用 GitHub Actions 时，这对重用其他构建的结果或在工作流中将镜像固定到特定标签很有用。

## 将镜像固定到标签 Pin image to a tag

Replace `alpine:latest` with a pinned one:

​	将 `alpine:latest` 替换为一个固定版本：

```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
RUN echo "Hello World"
```



```yaml
name: ci

on:
  push:

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      
      - name: Build
        uses: docker/build-push-action@v6
        with:
          build-contexts: |
            alpine=docker-image://alpine:3.20            
          tags: myimage:latest
```

## 在后续步骤中使用镜像 Use image in subsequent steps

By default, the [Docker Setup Buildx](https://github.com/marketplace/actions/docker-setup-buildx) action uses `docker-container` as a build driver, so built Docker images aren't loaded automatically.

​	默认情况下，[Docker Setup Buildx](https://github.com/marketplace/actions/docker-setup-buildx) 操作使用 `docker-container` 作为构建驱动，因此构建的 Docker 镜像不会自动加载。

With named contexts you can reuse the built image:

​	使用命名上下文可以重用构建的镜像：

```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
RUN echo "Hello World"
```



```yaml
name: ci

on:
  push:

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
        with:
          driver: docker
      
      - name: Build base image
        uses: docker/build-push-action@v6
        with:
          context: "{{defaultContext}}:base"
          load: true
          tags: my-base-image:latest
      
      - name: Build
        uses: docker/build-push-action@v6
        with:
          build-contexts: |
            alpine=docker-image://my-base-image:latest            
          tags: myimage:latest
```

## 配合容器构建器使用 Using with a container builder

As shown in the previous section we are not using the default [`docker-container` driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockercontainerbuilddriver" >}}) for building with named contexts. That's because this driver can't load an image from the Docker store as it's isolated. To solve this problem you can use a [local registry]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Localregistry" >}}) to push your base image in your workflow:

​	如上一节所示，我们未使用构建命名上下文的默认[`docker-container`驱动]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockercontainerbuilddriver" >}})。这是因为该驱动无法从 Docker 存储加载镜像，因为它是隔离的。为了解决这个问题，您可以使用[本地注册表]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Localregistry" >}})在工作流中推送您的基础镜像。

```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
RUN echo "Hello World"
```



```yaml
name: ci

on:
  push:

jobs:
  docker:
    runs-on: ubuntu-latest
    services:
      registry:
        image: registry:2
        ports:
          - 5000:5000
    steps:
      - name: Set up QEMU
        uses: docker/setup-qemu-action@v3
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
        with:
          # network=host driver-opt needed to push to local registry
          driver-opts: network=host
      
      - name: Build base image
        uses: docker/build-push-action@v6
        with:
          context: "{{defaultContext}}:base"
          tags: localhost:5000/my-base-image:latest
          push: true
      
      - name: Build
        uses: docker/build-push-action@v6
        with:
          build-contexts: |
            alpine=docker-image://localhost:5000/my-base-image:latest            
          tags: myimage:latest
```
