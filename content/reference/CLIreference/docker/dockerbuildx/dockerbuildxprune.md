+++
title = "docker buildx prune"
date = 2024-10-23T14:54:43+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/buildx/prune/](https://docs.docker.com/reference/cli/docker/buildx/prune/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx prune

| Description | Remove build cache    |
| :---------- | --------------------- |
| Usage       | `docker buildx prune` |

## Description

Clears the build cache of the selected builder.

​	清除所选构建器的构建缓存。

You can finely control what cache data is kept using:

​	您可以通过以下方式精确控制保留哪些缓存数据：

- The `--filter=until=<duration>` flag to keep images that have been used in the last `<duration>` time.

  - 使用 `--filter=until=<duration>` 标志保留在过去 `<duration>` 时间内使用过的镜像。

    `<duration>` is a duration string, e.g. `24h` or `2h30m`, with allowable units of `(h)ours`, `(m)inutes` and `(s)econds`. `<duration>` 是一个持续时间字符串，例如 `24h` 或 `2h30m`，允许的单位有 `(h)小时`、`(m)分钟` 和 `(s)秒`。

- The `--keep-storage=<size>` flag to keep `<size>` bytes of data in the cache.

  - 使用 `--keep-storage=<size>` 标志保留 `<size>` 字节的数据在缓存中。


  `<size>` is a human-readable memory string, e.g. `128mb`, `2gb`, etc. Units are case-insensitive.

  `<size>` 是一个人类可读的内存字符串，例如 `128mb`、`2gb` 等。单位不区分大小写。

- The `--all` flag to allow clearing internal helper images and frontend images set using the `#syntax=` directive or the `BUILDKIT_SYNTAX` build argument.

  - 使用 `--all` 标志允许清除通过 `#syntax=` 指令或 `BUILDKIT_SYNTAX` 构建参数设置的内部辅助镜像和前端镜像。


## Options

| Option           | Default | Description                                                  |
| ---------------- | ------- | ------------------------------------------------------------ |
| `-a, --all`      |         | 包括内部/前端镜像 Include internal/frontend images           |
| `--filter`       |         | 提供过滤值（例如 `until=24h`）Provide filter values (e.g., `until=24h`) |
| `-f, --force`    |         | 不提示确认Do not prompt for confirmation                     |
| `--keep-storage` |         | 要为缓存保留的磁盘空间量Amount of disk space to keep for cache |
| `--verbose`      |         | 提供更详细的输出 Provide a more verbose output               |

## Examples

### 覆盖配置的构建器实例（`--builder`） Override the configured builder instance (`--builder`)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).

​	与 [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder) 相同。
