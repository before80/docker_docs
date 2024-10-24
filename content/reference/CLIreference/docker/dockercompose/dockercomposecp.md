+++
title = "docker compose cp"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/compose/cp/](https://docs.docker.com/reference/cli/docker/compose/cp/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose cp

| Description | Copy files/folders between a service container and the local filesystem |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker compose cp [OPTIONS] SERVICE:SRC_PATH DEST_PATH|- docker compose cp [OPTIONS] SRC_PATH|- SERVICE:DEST_PATH` |

## [Description](https://docs.docker.com/reference/cli/docker/compose/cp/#description)

Copy files/folders between a service container and the local filesystem

## [Options](https://docs.docker.com/reference/cli/docker/compose/cp/#options)

| Option              | Default | Description                                             |
| ------------------- | ------- | ------------------------------------------------------- |
| `-a, --archive`     |         | Archive mode (copy all uid/gid information)             |
| `-L, --follow-link` |         | Always follow symbol link in SRC_PATH                   |
| `--index`           |         | Index of the container if service has multiple replicas |
