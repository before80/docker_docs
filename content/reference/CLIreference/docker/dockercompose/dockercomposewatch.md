+++
title = "docker compose watch"
date = 2024-10-23T14:54:43+08:00
weight = 270
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/watch/](https://docs.docker.com/reference/cli/docker/compose/watch/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose watch

| Description | Watch build context for service and rebuild/refresh containers when files are updated |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker compose watch [SERVICE...]`                          |

## Description

Watch build context for service and rebuild/refresh containers when files are updated

​	监视服务的构建上下文，并在文件更新时重新构建/刷新容器

## Options

| Option    | Default | Description                                                  |
| --------- | ------- | ------------------------------------------------------------ |
| `--no-up` |         | 在监视之前不构建和启动服务 Do not build & start services before watching |
| `--prune` |         | 在重建时修剪悬空的镜像 Prune dangling images on rebuild      |
| `--quiet` |         | 隐藏构建输出 hide build output                               |
