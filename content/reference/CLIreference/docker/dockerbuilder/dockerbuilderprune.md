+++
title = "docker builder prune"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/builder/prune/](https://docs.docker.com/reference/cli/docker/builder/prune/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker builder prune

| Description | Remove build cache     |
| :---------- | ---------------------- |
| Usage       | `docker builder prune` |

## Description

Remove build cache

​	删除构建缓存

## Options

| Option           | Default | Description                                                  |
| ---------------- | ------- | ------------------------------------------------------------ |
| `-a, --all`      |         | 删除所有未使用的构建缓存，而不仅是悬空的缓存Remove all unused build cache, not just dangling ones |
| `--filter`       |         | 提供过滤值（例如 `until=24h`）Provide filter values (e.g. `until=24h`) |
| `-f, --force`    |         | 不提示确认直接执行 Do not prompt for confirmation            |
| `--keep-storage` |         | 保留的缓存磁盘空间量 Amount of disk space to keep for cache  |

> 个人注释	
>
> ​	在 `docker builder prune` 命令中，`--keep-storage` 选项的作用是指定要保留的构建缓存的磁盘空间量，以防止删除所有缓存。这个选项允许用户设置一个缓存的磁盘空间上限，Docker 会在执行清理操作时根据此上限保留最近的缓存数据，而不是删除所有的未使用缓存。
>
> ​	例如，使用 `--keep-storage=1GB` 会保留大约 1GB 的缓存数据，其余超出部分将被清理。这样可以在优化磁盘空间的同时保留一部分构建缓存，加快后续构建速度。
