+++
title = "docker compose kill"
date = 2024-10-23T14:54:43+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/kill/](https://docs.docker.com/reference/cli/docker/compose/kill/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose kill

| Description | Force stop service containers                |
| :---------- | -------------------------------------------- |
| Usage       | `docker compose kill [OPTIONS] [SERVICE...]` |

## Description

Forces running containers to stop by sending a `SIGKILL` signal. Optionally the signal can be passed, for example:



```console
$ docker compose kill -s SIGINT
```

## Options

| Option             | Default   | Description                                                  |
| ------------------ | --------- | ------------------------------------------------------------ |
| `--remove-orphans` |           | Remove containers for services not defined in the Compose file |
| `-s, --signal`     | `SIGKILL` | SIGNAL to send to the container                              |
