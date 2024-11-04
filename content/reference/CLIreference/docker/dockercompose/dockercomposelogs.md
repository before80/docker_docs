+++
title = "docker compose logs"
date = 2024-10-23T14:54:43+08:00
weight = 100
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/logs/](https://docs.docker.com/reference/cli/docker/compose/logs/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose logs

| Description | View output from containers                  |
| :---------- | -------------------------------------------- |
| Usage       | `docker compose logs [OPTIONS] [SERVICE...]` |

## Description

Displays log output from services

​	显示服务的日志输出

## Options

| Option             | Default | Description                                                  |
| ------------------ | ------- | ------------------------------------------------------------ |
| `-f, --follow`     |         | 跟随日志输出 Follow log output                               |
| `--index`          |         | 如果服务有多个副本，则为容器的索引 index of the container if service has multiple replicas |
| `--no-color`       |         | 生成单色输出 Produce monochrome output                       |
| `--no-log-prefix`  |         | 日志中不打印前缀 Don't print prefix in logs                  |
| `--since`          |         | 显示自时间戳以来的日志（例如 2013-01-02T13:23:37Z）或相对时间（例如 42m 表示 42 分钟） Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes) |
| `-n, --tail`       | `all`   | 从每个容器的日志末尾显示的行数 Number of lines to show from the end of the logs for each container |
| `-t, --timestamps` |         | 显示时间戳 Show timestamps                                   |
| `--until`          |         | 在时间戳之前显示日志（例如 2013-01-02T13:23:37Z）或相对时间（例如 42m 表示 42 分钟） Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes) |
