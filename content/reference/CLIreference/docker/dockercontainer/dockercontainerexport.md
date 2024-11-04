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

​	`docker export` 命令不会导出与容器关联的卷的内容。如果卷挂载在容器中现有目录之上，`docker export` 将导出底层目录的内容，而不是卷的内容。

Refer to [Backup, restore, or migrate data volumes](https://docs.docker.com/engine/storage/volumes/#back-up-restore-or-migrate-data-volumes) in the user guide for examples on exporting data in a volume.

​	请参阅用户指南中的 [备份、恢复或迁移数据卷](https://docs.docker.com/engine/storage/volumes/#back-up-restore-or-migrate-data-volumes) 以获取有关导出卷中数据的示例。

## Options

| Option         | Default | Description                                                  |
| -------------- | ------- | ------------------------------------------------------------ |
| `-o, --output` |         | 写入文件，而不是 STDOUT - Write to a file, instead of STDOUT |

## Examples

The following commands produce the same result.

​	以下命令产生相同的结果。



```console
$ docker export red_panda > latest.tar
```



```console
$ docker export --output="latest.tar" red_panda
```
