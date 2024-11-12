+++
title = "在作业之间共享构建的镜像"
date = 2024-10-23T14:54:40+08:00
weight = 130
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/ci/github-actions/share-image-jobs/](https://docs.docker.com/build/ci/github-actions/share-image-jobs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Share built image between jobs with GitHub Actions - 在 GitHub Actions 中在作业之间共享构建的镜像

As each job is isolated in its own runner, you can't use your built image between jobs, except if you're using [self-hosted runners](https://docs.github.com/en/actions/hosting-your-own-runners/about-self-hosted-runners) However, you can [pass data between jobs](https://docs.github.com/en/actions/using-workflows/storing-workflow-data-as-artifacts#passing-data-between-jobs-in-a-workflow) in a workflow using the [actions/upload-artifact](https://github.com/actions/upload-artifact) and [actions/download-artifact](https://github.com/actions/download-artifact) actions:

​	由于每个作业都在各自的运行器中隔离，因此除非使用[自托管运行器](https://docs.github.com/en/actions/hosting-your-own-runners/about-self-hosted-runners)，否则无法在作业之间共享构建的镜像。然而，您可以在工作流中通过 [actions/upload-artifact](https://github.com/actions/upload-artifact) 和 [actions/download-artifact](https://github.com/actions/download-artifact) 操作[在作业之间传递数据](https://docs.github.com/en/actions/using-workflows/storing-workflow-data-as-artifacts#passing-data-between-jobs-in-a-workflow)：

```yaml
name: ci

on:
  push:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      
      - name: Build and export
        uses: docker/build-push-action@v6
        with:
          tags: myimage:latest
          outputs: type=docker,dest=/tmp/myimage.tar
      
      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: myimage
          path: /tmp/myimage.tar

  use:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Download artifact
        uses: actions/download-artifact@v4
        with:
          name: myimage
          path: /tmp
      
      - name: Load image
        run: |
          docker load --input /tmp/myimage.tar
          docker image ls -a          
```
