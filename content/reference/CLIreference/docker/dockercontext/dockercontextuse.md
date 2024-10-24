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
