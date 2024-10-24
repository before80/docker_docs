+++
title = "docker compose watch"
date = 2024-10-23T14:54:43+08:00
weight = 270
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/compose/watch/](https://docs.docker.com/reference/cli/docker/compose/watch/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose watch

| Description | Watch build context for service and rebuild/refresh containers when files are updated |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker compose watch [SERVICE...]`                          |

## [Description](https://docs.docker.com/reference/cli/docker/compose/watch/#description)

Watch build context for service and rebuild/refresh containers when files are updated

## [Options](https://docs.docker.com/reference/cli/docker/compose/watch/#options)

| Option    | Default | Description                                   |
| --------- | ------- | --------------------------------------------- |
| `--no-up` |         | Do not build & start services before watching |
| `--prune` |         | Prune dangling images on rebuild              |
| `--quiet` |         | hide build output                             |
