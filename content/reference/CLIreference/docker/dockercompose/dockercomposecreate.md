+++
title = "docker compose create"
date = 2024-10-23T14:54:43+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/compose/create/](https://docs.docker.com/reference/cli/docker/compose/create/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose create

| Description | Creates containers for a service               |
| :---------- | ---------------------------------------------- |
| Usage       | `docker compose create [OPTIONS] [SERVICE...]` |

## [Description](https://docs.docker.com/reference/cli/docker/compose/create/#description)

Creates containers for a service

## [Options](https://docs.docker.com/reference/cli/docker/compose/create/#options)

| Option             | Default  | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
| `--build`          |          | Build images before starting containers                      |
| `--force-recreate` |          | Recreate containers even if their configuration and image haven't changed |
| `--no-build`       |          | Don't build an image, even if it's policy                    |
| `--no-recreate`    |          | If containers already exist, don't recreate them. Incompatible with --force-recreate. |
| `--pull`           | `policy` | Pull image before running ("always"\|"missing"\|"never"\|"build") |
| `--quiet-pull`     |          | Pull without printing progress information                   |
| `--remove-orphans` |          | Remove containers for services not defined in the Compose file |
| `--scale`          |          | Scale SERVICE to NUM instances. Overrides the `scale` setting in the Compose file if present. |
