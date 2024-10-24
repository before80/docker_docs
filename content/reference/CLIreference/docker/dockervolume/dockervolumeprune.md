+++
title = "docker volume prune"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/volume/prune/](https://docs.docker.com/reference/cli/docker/volume/prune/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker volume prune

| Description | Remove unused local volumes     |
| :---------- | ------------------------------- |
| Usage       | `docker volume prune [OPTIONS]` |

## Description

Remove all unused local volumes. Unused local volumes are those which are not referenced by any containers. By default, it only removes anonymous volumes.

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-a, --all`](https://docs.docker.com/reference/cli/docker/volume/prune/#all) |         | API 1.42+ Remove all unused volumes, not just anonymous ones |
| [`--filter`](https://docs.docker.com/reference/cli/docker/volume/prune/#filter) |         | Provide filter values (e.g. `label=<label>`)                 |
| `-f, --force`                                                |         | Do not prompt for confirmation                               |

## Examples



```console
$ docker volume prune

WARNING! This will remove anonymous local volumes not used by at least one container.
Are you sure you want to continue? [y/N] y
Deleted Volumes:
07c7bdf3e34ab76d921894c2b834f073721fccfbbcba792aa7648e3a7a664c2e
my-named-vol

Total reclaimed space: 36 B
```

### Filtering (--all, -a)

Use the `--all` flag to prune both unused anonymous and named volumes.

### Filtering (--filter)

The filtering flag (`--filter`) format is of "key=value". If there is more than one filter, then pass multiple flags (e.g., `--filter "foo=bar" --filter "bif=baz"`)

The currently supported filters are:

- label (`label=<key>`, `label=<key>=<value>`, `label!=<key>`, or `label!=<key>=<value>`) - only remove volumes with (or without, in case `label!=...` is used) the specified labels.

The `label` filter accepts two formats. One is the `label=...` (`label=<key>` or `label=<key>=<value>`), which removes volumes with the specified labels. The other format is the `label!=...` (`label!=<key>` or `label!=<key>=<value>`), which removes volumes without the specified labels.
