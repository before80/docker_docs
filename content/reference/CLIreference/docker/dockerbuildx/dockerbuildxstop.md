+++
title = "docker buildx stop"
date = 2024-10-23T14:54:43+08:00
weight = 100
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/buildx/stop/](https://docs.docker.com/reference/cli/docker/buildx/stop/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx stop

| Description | Stop builder instance       |
| :---------- | --------------------------- |
| Usage       | `docker buildx stop [NAME]` |

## Description

Stops the specified or current builder. This does not prevent buildx build to restart the builder. The implementation of stop depends on the driver.

​	停止指定的或当前的构建器。这并不会阻止 buildx build 重新启动构建器。停止的实现取决于驱动程序。

## Examples

### 覆盖配置的构建器实例（`--builder`） Override the configured builder instance (`--builder`)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).

​	与 [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder) 相同。
