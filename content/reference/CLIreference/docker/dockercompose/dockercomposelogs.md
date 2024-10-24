+++
title = "docker compose logs"
date = 2024-10-23T14:54:43+08:00
weight = 100
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/compose/logs/](https://docs.docker.com/reference/cli/docker/compose/logs/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose logs

| Description | View output from containers                  |
| :---------- | -------------------------------------------- |
| Usage       | `docker compose logs [OPTIONS] [SERVICE...]` |

## [Description](https://docs.docker.com/reference/cli/docker/compose/logs/#description)

Displays log output from services

## [Options](https://docs.docker.com/reference/cli/docker/compose/logs/#options)

| Option             | Default | Description                                                  |
| ------------------ | ------- | ------------------------------------------------------------ |
| `-f, --follow`     |         | Follow log output                                            |
| `--index`          |         | index of the container if service has multiple replicas      |
| `--no-color`       |         | Produce monochrome output                                    |
| `--no-log-prefix`  |         | Don't print prefix in logs                                   |
| `--since`          |         | Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes) |
| `-n, --tail`       | `all`   | Number of lines to show from the end of the logs for each container |
| `-t, --timestamps` |         | Show timestamps                                              |
| `--until`          |         | Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes) |
