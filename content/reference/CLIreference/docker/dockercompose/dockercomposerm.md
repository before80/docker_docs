+++
title = "docker compose rm"
date = 2024-10-23T14:54:43+08:00
weight = 180
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/rm/](https://docs.docker.com/reference/cli/docker/compose/rm/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose rm

| Description | Removes stopped service containers         |
| :---------- | ------------------------------------------ |
| Usage       | `docker compose rm [OPTIONS] [SERVICE...]` |

## Description

Removes stopped service containers.

​	删除已停止的服务容器。

By default, anonymous volumes attached to containers are not removed. You can override this with `-v`. To list all volumes, use `docker volume ls`.

​	默认情况下，附加到容器的匿名卷不会被删除。你可以通过 `-v` 覆盖此选项。要列出所有卷，可以使用 `docker volume ls`。

Any data which is not in a volume is lost.

​	任何不在卷中的数据将丢失。

Running the command with no options also removes one-off containers created by `docker compose run`:

​	运行没有选项的命令也会删除通过 `docker compose run` 创建的一次性容器：



```console
$ docker compose rm
Going to remove djangoquickstart_web_run_1
Are you sure? [yN] y
Removing djangoquickstart_web_run_1 ... done
```

## Options

| Option          | Default | Description                                                  |
| --------------- | ------- | ------------------------------------------------------------ |
| `-f, --force`   |         | 无需确认即可删除 Don't ask to confirm removal                |
| `-s, --stop`    |         | 如果需要，先停止容器再删除 Stop the containers, if required, before removing |
| `-v, --volumes` |         | 删除附加到容器的任何匿名卷 Remove any anonymous volumes attached to containers |
