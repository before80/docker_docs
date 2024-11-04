+++
title = "docker context use"
date = 2024-10-23T14:54:43+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/context/use/](https://docs.docker.com/reference/cli/docker/context/use/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker context use

| Description | Set the current docker context |
| :---------- | ------------------------------ |
| Usage       | `docker context use CONTEXT`   |

## Description

Set the default context to use, when `DOCKER_HOST`, `DOCKER_CONTEXT` environment variables and `--host`, `--context` global options aren't set. To disable usage of contexts, you can use the special `default` context.

​	设置默认上下文，以在未设置 `DOCKER_HOST`、`DOCKER_CONTEXT` 环境变量和 `--host`、`--context` 全局选项时使用。要禁用上下文的使用，可以使用特殊的 `default` 上下文。
