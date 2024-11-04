+++
title = "docker buildx use"
date = 2024-10-23T14:54:43+08:00
weight = 110
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/buildx/use/](https://docs.docker.com/reference/cli/docker/buildx/use/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx use

| Description | Set the current builder instance   |
| :---------- | ---------------------------------- |
| Usage       | `docker buildx use [OPTIONS] NAME` |

## Description

Switches the current builder instance. Build commands invoked after this command will run on a specified builder. Alternatively, a context name can be used to switch to the default builder of that context.

​	切换当前构建器实例。在此命令之后调用的构建命令将在指定的构建器上运行。或者，可以使用上下文名称切换到该上下文的默认构建器。

## Options

| Option      | Default | Description                                                  |
| ----------- | ------- | ------------------------------------------------------------ |
| `--default` |         | 将构建器设置为当前上下文的默认构建器 Set builder as default for current context |
| `--global`  |         | 构建器持久化上下文更改 Builder persists context changes      |

## Examples

### 覆盖配置的构建器实例（`--builder`） Override the configured builder instance (`--builder`)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).

​	与 [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder) 相同。
