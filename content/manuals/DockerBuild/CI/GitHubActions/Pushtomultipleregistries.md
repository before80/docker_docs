+++
title = "推送到多个注册表"
date = 2024-10-23T14:54:40+08:00
weight = 110
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/ci/github-actions/push-multi-registries/](https://docs.docker.com/build/ci/github-actions/push-multi-registries/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Push to multiple registries with GitHub Actions - 使用 GitHub Actions 推送到多个注册表

The following workflow will connect you to Docker Hub and GitHub Container Registry, and push the image to both registries:

​	以下工作流将连接到 Docker Hub 和 GitHub Container Registry，并将镜像推送到两个注册表：

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
            ghcr.io/user/app:latest
            ghcr.io/user/app:1.0.0            
```
