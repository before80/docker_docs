+++
title = "docker network prune"
date = 2024-10-23T14:54:43+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/network/prune/](https://docs.docker.com/reference/cli/docker/network/prune/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker network prune

| Description | Remove all unused networks       |
| :---------- | -------------------------------- |
| Usage       | `docker network prune [OPTIONS]` |

## Description

Remove all unused networks. Unused networks are those which are not referenced by any containers.

​	删除所有未使用的网络。未使用的网络是指没有被任何容器引用的网络。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`--filter`](https://docs.docker.com/reference/cli/docker/network/prune/#filter) |         | 提供过滤器值（例如 `until=<时间戳>`） Provide filter values (e.g. `until=<timestamp>`) |
| `-f, --force`                                                |         | 不提示确认 Do not prompt for confirmation                    |

## Examples



```console
$ docker network prune

WARNING! This will remove all custom networks not used by at least one container.
Are you sure you want to continue? [y/N] y
Deleted Networks:
n1
n2
```

### Filtering (`--filter`)

The filtering flag (`--filter`) format is of "key=value". If there is more than one filter, then pass multiple flags (e.g., `--filter "foo=bar" --filter "bif=baz"`)

​	过滤标志（`--filter`）的格式为“键=值”。如果有多个过滤条件，可以传递多个标志（例如，`--filter "foo=bar" --filter "bif=baz"`）。

The currently supported filters are:

​	当前支持的过滤器有：

- until (`<timestamp>`) - only remove networks created before given timestamp
  - until (`<时间戳>`) - 仅删除在给定时间戳之前创建的网络

- label (`label=<key>`, `label=<key>=<value>`, `label!=<key>`, or `label!=<key>=<value>`) - only remove networks with (or without, in case `label!=...` is used) the specified labels.
  - label (`label=<key>`，`label=<key>=<value>`，`label!=<key>`，或 `label!=<key>=<value>`) - 仅删除具有（或在使用 `label!=...` 时不具有）指定标签的网络


The `until` filter can be Unix timestamps, date formatted timestamps, or Go duration strings (e.g. `10m`, `1h30m`) computed relative to the daemon machine’s time. Supported formats for date formatted time stamps include RFC3339Nano, RFC3339, `2006-01-02T15:04:05`, `2006-01-02T15:04:05.999999999`, `2006-01-02T07:00`, and `2006-01-02`. The local timezone on the daemon will be used if you do not provide either a `Z` or a `+-00:00` timezone offset at the end of the timestamp. When providing Unix timestamps enter seconds[.nanoseconds], where seconds is the number of seconds that have elapsed since January 1, 1970 (midnight UTC/GMT), not counting leap seconds (aka Unix epoch or Unix time), and the optional .nanoseconds field is a fraction of a second no more than nine digits long.

​	`until` 过滤器可以是 Unix 时间戳、日期格式的时间戳，或相对于守护进程机器时间的 Go 时间持续字符串（例如 `10m`，`1h30m`）。支持的日期格式包括 RFC3339Nano、RFC3339、`2006-01-02T15:04:05`、`2006-01-02T15:04:05.999999999`、`2006-01-02T07:00` 和 `2006-01-02`。如果不在时间戳末尾提供 `Z` 或 `+-00:00` 时区偏移，将使用守护进程的本地时区。Unix 时间戳应以秒[.纳秒]表示，其中秒数是从1970年1月1日（UTC/GMT午夜）起经过的秒数，不包括闰秒，且可选的 .纳秒字段为不超过九位的秒的小数部分。

The `label` filter accepts two formats. One is the `label=...` (`label=<key>` or `label=<key>=<value>`), which removes networks with the specified labels. The other format is the `label!=...` (`label!=<key>` or `label!=<key>=<value>`), which removes networks without the specified labels.

​	`label` 过滤器接受两种格式。一种是 `label=...`（`label=<key>` 或 `label=<key>=<value>`），用于删除具有指定标签的网络。另一种格式是 `label!=...`（`label!=<key>` 或 `label!=<key>=<value>`），用于删除不具有指定标签的网络。

The following removes networks created more than 5 minutes ago. Note that system networks such as `bridge`, `host`, and `none` will never be pruned:

​	以下命令删除创建时间超过5分钟的网络。请注意，系统网络如 `bridge`、`host` 和 `none` 永远不会被删除：



```console
$ docker network ls

NETWORK ID          NAME                DRIVER              SCOPE
7430df902d7a        bridge              bridge              local
ea92373fd499        foo-1-day-ago       bridge              local
ab53663ed3c7        foo-1-min-ago       bridge              local
97b91972bc3b        host                host                local
f949d337b1f5        none                null                local

$ docker network prune --force --filter until=5m

Deleted Networks:
foo-1-day-ago

$ docker network ls

NETWORK ID          NAME                DRIVER              SCOPE
7430df902d7a        bridge              bridge              local
ab53663ed3c7        foo-1-min-ago       bridge              local
97b91972bc3b        host                host                local
f949d337b1f5        none                null                local
```
