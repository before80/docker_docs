+++
title = "docker container export"
date = 2024-10-23T14:54:43+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/export/](https://docs.docker.com/reference/cli/docker/container/export/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container export

| Description | Export a container's filesystem as a tar archive |
| :---------- | ------------------------------------------------ |
| Usage       | `docker container export [OPTIONS] CONTAINER`    |
| Aliases     | `docker export`                                  |

## Description

The `docker export` command doesn't export the contents of volumes associated with the container. If a volume is mounted on top of an existing directory in the container, `docker export` exports the contents of the underlying directory, not the contents of the volume.

Refer to [Backup, restore, or migrate data volumes](https://docs.docker.com/engine/storage/volumes/#back-up-restore-or-migrate-data-volumes) in the user guide for examples on exporting data in a volume.

## Options

| Option         | Default | Description                        |
| -------------- | ------- | ---------------------------------- |
| `-o, --output` |         | Write to a file, instead of STDOUT |

## Examples

The following commands produce the same result.



```console
$ docker export red_panda > latest.tar
```



```console
$ docker export --output="latest.tar" red_panda
```
