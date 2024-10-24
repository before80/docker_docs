+++
title = "docker compose wait"
date = 2024-10-23T14:54:43+08:00
weight = 260
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/compose/wait/](https://docs.docker.com/reference/cli/docker/compose/wait/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose wait

| Description | Block until the first service container stops        |
| :---------- | ---------------------------------------------------- |
| Usage       | `docker compose wait SERVICE [SERVICE...] [OPTIONS]` |

## [Description](https://docs.docker.com/reference/cli/docker/compose/wait/#description)

Block until the first service container stops

## [Options](https://docs.docker.com/reference/cli/docker/compose/wait/#options)

| Option           | Default | Description                                  |
| ---------------- | ------- | -------------------------------------------- |
| `--down-project` |         | Drops project when the first container stops |
