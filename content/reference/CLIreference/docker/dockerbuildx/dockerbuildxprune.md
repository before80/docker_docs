+++
title = "docker buildx prune"
date = 2024-10-23T14:54:43+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/buildx/prune/](https://docs.docker.com/reference/cli/docker/buildx/prune/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx prune

| Description | Remove build cache    |
| :---------- | --------------------- |
| Usage       | `docker buildx prune` |

## [Description](https://docs.docker.com/reference/cli/docker/buildx/prune/#description)

Clears the build cache of the selected builder.

You can finely control what cache data is kept using:

- The `--filter=until=<duration>` flag to keep images that have been used in the last `<duration>` time.

  `<duration>` is a duration string, e.g. `24h` or `2h30m`, with allowable units of `(h)ours`, `(m)inutes` and `(s)econds`.

- The `--keep-storage=<size>` flag to keep `<size>` bytes of data in the cache.

  `<size>` is a human-readable memory string, e.g. `128mb`, `2gb`, etc. Units are case-insensitive.

- The `--all` flag to allow clearing internal helper images and frontend images set using the `#syntax=` directive or the `BUILDKIT_SYNTAX` build argument.

## [Options](https://docs.docker.com/reference/cli/docker/buildx/prune/#options)

| Option           | Default | Description                               |
| ---------------- | ------- | ----------------------------------------- |
| `-a, --all`      |         | Include internal/frontend images          |
| `--filter`       |         | Provide filter values (e.g., `until=24h`) |
| `-f, --force`    |         | Do not prompt for confirmation            |
| `--keep-storage` |         | Amount of disk space to keep for cache    |
| `--verbose`      |         | Provide a more verbose output             |

## [Examples](https://docs.docker.com/reference/cli/docker/buildx/prune/#examples)

### [Override the configured builder instance (--builder)](https://docs.docker.com/reference/cli/docker/buildx/prune/#builder)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).
