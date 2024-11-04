+++
title = "docker compose images"
date = 2024-10-23T14:54:43+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/images/](https://docs.docker.com/reference/cli/docker/compose/images/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose images

| Description | List images used by the created containers     |
| :---------- | ---------------------------------------------- |
| Usage       | `docker compose images [OPTIONS] [SERVICE...]` |

## Description

List images used by the created containers

​	列出已创建容器使用的镜像

## Options

| Option        | Default | Description                                                  |
| ------------- | ------- | ------------------------------------------------------------ |
| `--format`    | `table` | 格式化输出。可选值： [table \|json] -  Format the output. Values: [table \| json] |
| `-q, --quiet` |         | 仅显示 ID - Only display IDs                                 |
