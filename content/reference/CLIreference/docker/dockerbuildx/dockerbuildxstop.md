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

## Examples

### Override the configured builder instance (--builder)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).
