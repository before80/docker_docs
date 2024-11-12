+++
title = "在注册表之间复制镜像"
date = 2024-10-23T14:54:40+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/ci/github-actions/copy-image-registries/](https://docs.docker.com/build/ci/github-actions/copy-image-registries/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Copy image between registries with GitHub Actions - 使用 GitHub Actions 在注册表之间复制镜像

[Multi-platform images]({{< ref "/manuals/DockerBuild/Building/Multi-platform" >}}) built using Buildx can be copied from one registry to another using the [`buildx imagetools create` command]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildximagetools/dockerbuildximagetoolscreate" >}}):

​	使用 Buildx 构建的[多平台镜像]({{< ref "/manuals/DockerBuild/Building/Multi-platform" >}})可以通过 [`buildx imagetools create` 命令]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildximagetools/dockerbuildximagetoolscreate" >}})从一个注册表复制到另一个注册表。

```yaml
name: ci

on:
  push:

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Set up QEMU
        uses: docker/setup-qemu-action@v3
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      
      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      
      - name: Login to GitHub Container Registry
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.repository_owner }}
          password: ${{ secrets.GITHUB_TOKEN }}
      
      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          platforms: linux/amd64,linux/arm64
          push: true
          tags: |
            user/app:latest
            user/app:1.0.0            
      
      - name: Push image to GHCR
        run: |
          docker buildx imagetools create \
            --tag ghcr.io/user/app:latest \
            --tag ghcr.io/user/app:1.0.0 \
            user/app:latest          
```
