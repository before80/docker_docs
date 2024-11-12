+++
title = "导出到 Docker"
date = 2024-10-23T14:54:40+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/ci/github-actions/export-docker/](https://docs.docker.com/build/ci/github-actions/export-docker/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Export to Docker with GitHub Actions - 使用 GitHub Actions 导出到 Docker

You may want your build result to be available in the Docker client through `docker images` to be able to use it in another step of your workflow:

​	您可能希望构建结果在 Docker 客户端中可用，通过 `docker images` 命令来查看，以便在工作流的其他步骤中使用该结果：

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
          load: true
          tags: myimage:latest
      
      - name: Inspect
        run: |
          docker image inspect myimage:latest       
```
