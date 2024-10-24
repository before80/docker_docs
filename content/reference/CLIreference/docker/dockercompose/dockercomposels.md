+++
title = "docker compose ls"
date = 2024-10-23T14:54:43+08:00
weight = 110
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/compose/ls/](https://docs.docker.com/reference/cli/docker/compose/ls/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose ls

| Description | List running compose projects |
| :---------- | ----------------------------- |
| Usage       | `docker compose ls [OPTIONS]` |

## [Description](https://docs.docker.com/reference/cli/docker/compose/ls/#description)

Lists running Compose projects

## [Options](https://docs.docker.com/reference/cli/docker/compose/ls/#options)

| Option        | Default | Description                                |
| ------------- | ------- | ------------------------------------------ |
| `-a, --all`   |         | Show all stopped Compose projects          |
| `--filter`    |         | Filter output based on conditions provided |
| `--format`    | `table` | Format the output. Values: [table \| json] |
| `-q, --quiet` |         | Only display IDs                           |
