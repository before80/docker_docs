+++
title = "docker container prune"
date = 2024-10-23T14:54:43+08:00
weight = 110
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/prune/](https://docs.docker.com/reference/cli/docker/container/prune/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container prune

| Description | Remove all stopped containers      |
| :---------- | ---------------------------------- |
| Usage       | `docker container prune [OPTIONS]` |

## Description

Removes all stopped containers.

​	移除所有已停止的容器。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`--filter`](https://docs.docker.com/reference/cli/docker/container/prune/#filter) |         | 提供过滤值（例如 `until=<timestamp>`）Provide filter values (e.g. `until=<timestamp>`) |
| `-f, --force`                                                |         | 不提示确认 Do not prompt for confirmation                    |

## Examples

### 清理容器 Prune containers



```console
$ docker container prune
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Deleted Containers:
4a7f7eebae0f63178aff7eb0aa39cd3f0627a203ab2df258c1a00b456cf20063
f98f9c2aa1eaf727e4ec9c0283bc7d4aa4762fbdba7f26191f26c97f64090360

Total reclaimed space: 212 B
```

### Filtering (`--filter`)

The filtering flag (`--filter`) format is of "key=value". If there is more than one filter, then pass multiple flags (e.g., `--filter "foo=bar" --filter "bif=baz"`)

​	过滤标志（`--filter`）的格式为 "key=value"。如果有多个过滤条件，则可以传递多个标志（例如，`--filter "foo=bar" --filter "bif=baz"`）。

The currently supported filters are:

​	当前支持的过滤器包括：

- until (`<timestamp>`) - only remove containers created before given timestamp
  - until (`<timestamp>`) - 仅移除在给定时间戳之前创建的容器

- label (`label=<key>`, `label=<key>=<value>`, `label!=<key>`, or `label!=<key>=<value>`) - only remove containers with (or without, in case `label!=...` is used) the specified labels.
  - label (`label=<key>`，`label=<key>=<value>`，`label!=<key>` 或 `label!=<key>=<value>`） - 仅移除具有（或没有，使用 `label!=...` 时）指定标签的容器。


The `until` filter can be Unix timestamps, date formatted timestamps, or Go duration strings (e.g. `10m`, `1h30m`) computed relative to the daemon machine’s time. Supported formats for date formatted time stamps include RFC3339Nano, RFC3339, `2006-01-02T15:04:05`, `2006-01-02T15:04:05.999999999`, `2006-01-02T07:00`, and `2006-01-02`. The local timezone on the daemon will be used if you do not provide either a `Z` or a `+-00:00` timezone offset at the end of the timestamp. When providing Unix timestamps enter seconds[.nanoseconds], where seconds is the number of seconds that have elapsed since January 1, 1970 (midnight UTC/GMT), not counting leap seconds (aka Unix epoch or Unix time), and the optional .nanoseconds field is a fraction of a second no more than nine digits long.

​	`until` 过滤器可以是 Unix 时间戳、日期格式的时间戳或相对于守护进程机器时间的 Go 持续时间字符串（例如 `10m`，`1h30m`）。支持的日期格式时间戳包括 RFC3339Nano、RFC3339、`2006-01-02T15:04:05`、`2006-01-02T15:04:05.999999999`、`2006-01-02T07:00` 和 `2006-01-02`。如果您未在时间戳末尾提供 `Z` 或 `+-00:00` 时区偏移，则将使用守护进程的本地时区。提供 UNIX 时间戳时，请输入秒[.纳秒]，其中秒是自 1970 年 1 月 1 日（UTC/GMT 午夜）以来经过的秒数，不计闰秒（即 UNIX 纪元或 UNIX 时间），可选的 .纳秒字段是最多九位数字的秒的小数部分。

The `label` filter accepts two formats. One is the `label=...` (`label=<key>` or `label=<key>=<value>`), which removes containers with the specified labels. The other format is the `label!=...` (`label!=<key>` or `label!=<key>=<value>`), which removes containers without the specified labels.

​	`label` 过滤器接受两种格式。一种是 `label=...`（`label=<key>` 或 `label=<key>=<value>`），用于移除具有指定标签的容器。另一种格式是 `label!=...`（`label!=<key>` 或 `label!=<key>=<value>`），用于移除没有指定标签的容器。

The following removes containers created more than 5 minutes ago:

​	以下命令移除创建超过 5 分钟的容器：



```console
$ docker ps -a --format 'table {{.ID}}\t{{.Image}}\t{{.Command}}\t{{.CreatedAt}}\t{{.Status}}'

CONTAINER ID        IMAGE               COMMAND             CREATED AT                      STATUS
61b9efa71024        busybox             "sh"                2017-01-04 13:23:33 -0800 PST   Exited (0) 41 seconds ago
53a9bc23a516        busybox             "sh"                2017-01-04 13:11:59 -0800 PST   Exited (0) 12 minutes ago

$ docker container prune --force --filter "until=5m"

Deleted Containers:
53a9bc23a5168b6caa2bfbefddf1b30f93c7ad57f3dec271fd32707497cb9369

Total reclaimed space: 25 B

$ docker ps -a --format 'table {{.ID}}\t{{.Image}}\t{{.Command}}\t{{.CreatedAt}}\t{{.Status}}'

CONTAINER ID        IMAGE               COMMAND             CREATED AT                      STATUS
61b9efa71024        busybox             "sh"                2017-01-04 13:23:33 -0800 PST   Exited (0) 44 seconds ago
```

The following removes containers created before `2017-01-04T13:10:00`:

​	以下命令移除在 `2017-01-04T13:10:00` 之前创建的容器：



```console
$ docker ps -a --format 'table {{.ID}}\t{{.Image}}\t{{.Command}}\t{{.CreatedAt}}\t{{.Status}}'

CONTAINER ID        IMAGE               COMMAND             CREATED AT                      STATUS
53a9bc23a516        busybox             "sh"                2017-01-04 13:11:59 -0800 PST   Exited (0) 7 minutes ago
4a75091a6d61        busybox             "sh"                2017-01-04 13:09:53 -0800 PST   Exited (0) 9 minutes ago

$ docker container prune --force --filter "until=2017-01-04T13:10:00"

Deleted Containers:
4a75091a6d618526fcd8b33ccd6e5928ca2a64415466f768a6180004b0c72c6c

Total reclaimed space: 27 B

$ docker ps -a --format 'table {{.ID}}\t{{.Image}}\t{{.Command}}\t{{.CreatedAt}}\t{{.Status}}'

CONTAINER ID        IMAGE               COMMAND             CREATED AT                      STATUS
53a9bc23a516        busybox             "sh"                2017-01-04 13:11:59 -0800 PST   Exited (0) 9 minutes ago
```

