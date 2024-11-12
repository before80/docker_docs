+++
title = "更新 Docker Hub 描述"
date = 2024-10-23T14:54:40+08:00
weight = 160
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/ci/github-actions/update-dockerhub-desc/](https://docs.docker.com/build/ci/github-actions/update-dockerhub-desc/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Update Docker Hub description with GitHub Actions - 使用 GitHub Actions 更新 Docker Hub 描述

You can update the Docker Hub repository description using a third party action called [Docker Hub Description](https://github.com/peter-evans/dockerhub-description) with this action:

​	您可以使用名为 [Docker Hub Description](https://github.com/peter-evans/dockerhub-description) 的第三方操作，配合此操作来更新 Docker Hub 仓库的描述：

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
      
      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          push: true
          tags: user/app:latest
      
      - name: Update repo description
        uses: peter-evans/dockerhub-description@e98e4d1628a5f3be2be7c231e50981aee98723ae # v4.0.0
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
          repository: user/app
```
