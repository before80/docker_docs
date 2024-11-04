+++
title = "docker compose ls"
date = 2024-10-23T14:54:43+08:00
weight = 110
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/ls/](https://docs.docker.com/reference/cli/docker/compose/ls/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose ls

| Description | List running compose projects |
| :---------- | ----------------------------- |
| Usage       | `docker compose ls [OPTIONS]` |

## Description

Lists running Compose projects

​	列出正在运行的 Compose 项目

## Options

| Option        | Default | Description                                                  |
| ------------- | ------- | ------------------------------------------------------------ |
| `-a, --all`   |         | 显示所有已停止的 Compose 项目 Show all stopped Compose projects |
| `--filter`    |         | 根据提供的条件过滤输出 Filter output based on conditions provided |
| `--format`    | `table` | 格式化输出。可选值：[table \|json] -   Format the output. Values: [table \| json] |
| `-q, --quiet` |         | 仅显示 ID - Only display IDs                                 |
