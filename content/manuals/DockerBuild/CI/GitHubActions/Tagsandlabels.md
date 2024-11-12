+++
title = "Tags and labels"
date = 2024-10-23T14:54:40+08:00
weight = 140
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/build/ci/github-actions/manage-tags-labels/](https://docs.docker.com/build/ci/github-actions/manage-tags-labels/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Manage tags and labels with GitHub Actions 

If you want an "automatic" tag management and [OCI Image Format Specification](https://github.com/opencontainers/image-spec/blob/master/annotations.md) for labels, you can do it in a dedicated setup step. The following workflow will use the [Docker Metadata Action](https://github.com/docker/metadata-action) to handle tags and labels based on GitHub Actions events and Git metadata:

​	如果您希望对标签进行“自动”管理，并为标签使用 [OCI 镜像格式规范](https://github.com/opencontainers/image-spec/blob/master/annotations.md)，可以在专门的设置步骤中实现。以下工作流将使用 [Docker Metadata Action](https://github.com/docker/metadata-action) 基于 GitHub Actions 事件和 Git 元数据来处理标签和标签：

```yaml
name: ci

on:
  schedule:
    - cron: "0 10 * * *"
  push:
    branches:
      - "**"
    tags:
      - "v*.*.*"
  pull_request:

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Docker meta
        id: meta
        uses: docker/metadata-action@v5
        with:
          # list of Docker images to use as base name for tags
          images: |
            name/app
            ghcr.io/username/app            
          # generate Docker tags based on the following events/attributes
          tags: |
            type=schedule
            type=ref,event=branch
            type=ref,event=pr
            type=semver,pattern={{version}}
            type=semver,pattern={{major}}.{{minor}}
            type=semver,pattern={{major}}
            type=sha            
      
      - name: Set up QEMU
        uses: docker/setup-qemu-action@v3
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      
      - name: Login to Docker Hub
        if: github.event_name != 'pull_request'
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      
      - name: Login to GHCR
        if: github.event_name != 'pull_request'
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.repository_owner }}
          password: ${{ secrets.GITHUB_TOKEN }}
      
      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          push: ${{ github.event_name != 'pull_request' }}
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```
