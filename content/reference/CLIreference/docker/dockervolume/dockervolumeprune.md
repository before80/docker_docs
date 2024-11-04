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

​	删除所有未使用的本地卷。未使用的本地卷是指没有被任何容器引用的卷。默认情况下，它只会删除匿名卷。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-a, --all`](https://docs.docker.com/reference/cli/docker/volume/prune/#all) |         | API 1.42+ 删除所有未使用的卷，而不仅仅是匿名卷 API 1.42+ Remove all unused volumes, not just anonymous ones |
| [`--filter`](https://docs.docker.com/reference/cli/docker/volume/prune/#filter) |         | 提供过滤器值（例如 `label=<label>`） Provide filter values (e.g. `label=<label>`) |
| `-f, --force`                                                |         | 不提示确认 Do not prompt for confirmation                    |

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

### Filtering (`--all, -a`)

Use the `--all` flag to prune both unused anonymous and named volumes.

​	使用 `--all` 标志来删除未使用的匿名卷和命名卷。

### Filtering (`--filter`)

The filtering flag (`--filter`) format is of "key=value". If there is more than one filter, then pass multiple flags (e.g., `--filter "foo=bar" --filter "bif=baz"`)

​	过滤标志（`--filter`）的格式为“键=值”。如果有多个过滤条件，可以传递多个标志（例如，`--filter "foo=bar" --filter "bif=baz"`）

The currently supported filters are:

​	当前支持的过滤器有：

- label (`label=<key>`, `label=<key>=<value>`, `label!=<key>`, or `label!=<key>=<value>`) - only remove volumes with (or without, in case `label!=...` is used) the specified labels.
  - label (`label=<key>`，`label=<key>=<value>`，`label!=<key>`，或 `label!=<key>=<value>`) - 仅删除具有（或在使用 `label!=...` 时不具有）指定标签的卷。

The `label` filter accepts two formats. One is the `label=...` (`label=<key>` or `label=<key>=<value>`), which removes volumes with the specified labels. The other format is the `label!=...` (`label!=<key>` or `label!=<key>=<value>`), which removes volumes without the specified labels.

​	`label` 过滤器接受两种格式。一种是 `label=...`（`label=<key>` 或 `label=<key>=<value>`），用于删除具有指定标签的卷。另一种格式是 `label!=...`（`label!=<key>` 或 `label!=<key>=<value>`），用于删除不具有指定标签的卷。
