+++
title = "docker buildx use"
date = 2024-10-23T14:54:43+08:00
weight = 110
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/buildx/use/](https://docs.docker.com/reference/cli/docker/buildx/use/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx use

| Description | Set the current builder instance   |
| :---------- | ---------------------------------- |
| Usage       | `docker buildx use [OPTIONS] NAME` |

## [Description](https://docs.docker.com/reference/cli/docker/buildx/use/#description)

Switches the current builder instance. Build commands invoked after this command will run on a specified builder. Alternatively, a context name can be used to switch to the default builder of that context.

## [Options](https://docs.docker.com/reference/cli/docker/buildx/use/#options)

| Option      | Default | Description                                |
| ----------- | ------- | ------------------------------------------ |
| `--default` |         | Set builder as default for current context |
| `--global`  |         | Builder persists context changes           |

## [Examples](https://docs.docker.com/reference/cli/docker/buildx/use/#examples)

### [Override the configured builder instance (--builder)](https://docs.docker.com/reference/cli/docker/buildx/use/#builder)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).
