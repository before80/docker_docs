+++
title = "docker compose down"
date = 2024-10-23T14:54:43+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/down/](https://docs.docker.com/reference/cli/docker/compose/down/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose down

| Description | Stop and remove containers, networks       |
| :---------- | ------------------------------------------ |
| Usage       | `docker compose down [OPTIONS] [SERVICES]` |

## Description

Stops containers and removes containers, networks, volumes, and images created by `up`.

By default, the only things removed are:

- Containers for services defined in the Compose file.
- Networks defined in the networks section of the Compose file.
- The default network, if one is used.

Networks and volumes defined as external are never removed.

Anonymous volumes are not removed by default. However, as they don’t have a stable name, they are not automatically mounted by a subsequent `up`. For data that needs to persist between updates, use explicit paths as bind mounts or named volumes.

## Options

| Option             | Default | Description                                                  |
| ------------------ | ------- | ------------------------------------------------------------ |
| `--remove-orphans` |         | Remove containers for services not defined in the Compose file |
| `--rmi`            |         | Remove images used by services. "local" remove only images that don't have a custom tag ("local"\|"all") |
| `-t, --timeout`    |         | Specify a shutdown timeout in seconds                        |
| `-v, --volumes`    |         | Remove named volumes declared in the "volumes" section of the Compose file and anonymous volumes attached to containers |
