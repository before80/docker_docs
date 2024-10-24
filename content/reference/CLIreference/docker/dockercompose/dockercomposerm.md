+++
title = "docker compose rm"
date = 2024-10-23T14:54:43+08:00
weight = 180
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/compose/rm/](https://docs.docker.com/reference/cli/docker/compose/rm/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose rm

| Description | Removes stopped service containers         |
| :---------- | ------------------------------------------ |
| Usage       | `docker compose rm [OPTIONS] [SERVICE...]` |

## [Description](https://docs.docker.com/reference/cli/docker/compose/rm/#description)

Removes stopped service containers.

By default, anonymous volumes attached to containers are not removed. You can override this with `-v`. To list all volumes, use `docker volume ls`.

Any data which is not in a volume is lost.

Running the command with no options also removes one-off containers created by `docker compose run`:



```console
$ docker compose rm
Going to remove djangoquickstart_web_run_1
Are you sure? [yN] y
Removing djangoquickstart_web_run_1 ... done
```

## [Options](https://docs.docker.com/reference/cli/docker/compose/rm/#options)

| Option          | Default | Description                                         |
| --------------- | ------- | --------------------------------------------------- |
| `-f, --force`   |         | Don't ask to confirm removal                        |
| `-s, --stop`    |         | Stop the containers, if required, before removing   |
| `-v, --volumes` |         | Remove any anonymous volumes attached to containers |
