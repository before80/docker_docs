+++
title = "docker compose create"
date = 2024-10-23T14:54:43+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/create/](https://docs.docker.com/reference/cli/docker/compose/create/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose create

| Description | Creates containers for a service               |
| :---------- | ---------------------------------------------- |
| Usage       | `docker compose create [OPTIONS] [SERVICE...]` |

## Description

Creates containers for a service

​	为服务创建容器

## Options

| Option             | Default  | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
| `--build`          |          | 启动容器之前构建镜像 Build images before starting containers |
| `--force-recreate` |          | 即使配置和镜像没有改变，也重新创建容器 Recreate containers even if their configuration and image haven't changed |
| `--no-build`       |          | 不构建镜像，即使有构建策略 Don't build an image, even if it's policy |
| `--no-recreate`    |          | 如果容器已经存在，则不重新创建它们。与 `--force-recreate` 不兼容。 If containers already exist, don't recreate them. Incompatible with --force-recreate. |
| `--pull`           | `policy` | 在运行之前拉取镜像 Pull image before running ("always"\|"missing"\|"never"\|"build") |
| `--quiet-pull`     |          | 拉取时不打印进度信息 Pull without printing progress information |
| `--remove-orphans` |          | 删除在 Compose 文件中未定义的服务的容器 Remove containers for services not defined in the Compose file |
| `--scale`          |          | 将服务扩展到指定的实例数。如果 Compose 文件中存在 `scale` 设置，则覆盖该设置。 Scale SERVICE to NUM instances. Overrides the `scale` setting in the Compose file if present. |
