+++
title = "docker system prune"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/system/prune/](https://docs.docker.com/reference/cli/docker/system/prune/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker system prune

| Description | Remove unused data              |
| :---------- | ------------------------------- |
| Usage       | `docker system prune [OPTIONS]` |

## Description

Remove all unused containers, networks, images (both dangling and unused), and optionally, volumes.

​	删除所有未使用的容器、网络、镜像（包括悬空和未使用的），以及可选地删除卷。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| `-a, --all`                                                  |         | 删除所有未使用的镜像，而不仅仅是悬空的镜像 Remove all unused images not just dangling ones |
| [`--filter`](https://docs.docker.com/reference/cli/docker/system/prune/#filter) |         | API 1.28+ 提供过滤值（例如 `label=<key>=<value>`） API 1.28+ Provide filter values (e.g. `label=<key>=<value>`) |
| `-f, --force`                                                |         | 不进行确认提示 Do not prompt for confirmation                |
| `--volumes`                                                  |         | 清理匿名卷 Prune anonymous volumes                           |

## Examples



```console
$ docker system prune

WARNING! This will remove:
        - all stopped containers
        - all networks not used by at least one container
        - all dangling images
        - unused build cache
Are you sure you want to continue? [y/N] y

Deleted Containers:
f44f9b81948b3919590d5f79a680d8378f1139b41952e219830a33027c80c867
792776e68ac9d75bce4092bc1b5cc17b779bc926ab04f4185aec9bf1c0d4641f

Deleted Networks:
network1
network2

Deleted Images:
untagged: hello-world@sha256:f3b3b28a45160805bb16542c9531888519430e9e6d6ffc09d72261b0d26ff74f
deleted: sha256:1815c82652c03bfd8644afda26fb184f2ed891d921b20a0703b46768f9755c57
deleted: sha256:45761469c965421a92a69cc50e92c01e0cfa94fe026cdd1233445ea00e96289a

Total reclaimed space: 1.84kB
```

By default, volumes aren't removed to prevent important data from being deleted if there is currently no container using the volume. Use the `--volumes` flag when running the command to prune anonymous volumes as well:

​	默认情况下，卷不会被删除，以防止在当前没有容器使用卷时误删重要数据。使用 `--volumes` 标志来清理匿名卷：



```console
$ docker system prune -a --volumes

WARNING! This will remove:
        - all stopped containers
        - all networks not used by at least one container
        - all anonymous volumes not used by at least one container
        - all images without at least one container associated to them
        - all build cache
Are you sure you want to continue? [y/N] y

Deleted Containers:
0998aa37185a1a7036b0e12cf1ac1b6442dcfa30a5c9650a42ed5010046f195b
73958bfb884fa81fa4cc6baf61055667e940ea2357b4036acbbe25a60f442a4d

Deleted Networks:
my-network-a
my-network-b

Deleted Volumes:
named-vol

Deleted Images:
untagged: my-curl:latest
deleted: sha256:7d88582121f2a29031d92017754d62a0d1a215c97e8f0106c586546e7404447d
deleted: sha256:dd14a93d83593d4024152f85d7c63f76aaa4e73e228377ba1d130ef5149f4d8b
untagged: alpine:3.3
deleted: sha256:695f3d04125db3266d4ab7bbb3c6b23aa4293923e762aa2562c54f49a28f009f
untagged: alpine:latest
deleted: sha256:ee4603260daafe1a8c2f3b78fd760922918ab2441cbb2853ed5c439e59c52f96
deleted: sha256:9007f5987db353ec398a223bc5a135c5a9601798ba20a1abba537ea2f8ac765f
deleted: sha256:71fa90c8f04769c9721459d5aa0936db640b92c8c91c9b589b54abd412d120ab
deleted: sha256:bb1c3357b3c30ece26e6604aea7d2ec0ace4166ff34c3616701279c22444c0f3
untagged: my-jq:latest
deleted: sha256:6e66d724542af9bc4c4abf4a909791d7260b6d0110d8e220708b09e4ee1322e1
deleted: sha256:07b3fa89d4b17009eb3988dfc592c7d30ab3ba52d2007832dffcf6d40e3eda7f
deleted: sha256:3a88a5c81eb5c283e72db2dbc6d65cbfd8e80b6c89bb6e714cfaaa0eed99c548

Total reclaimed space: 13.5 MB
```

### Filtering (`--filter`)

The filtering flag (`--filter`) format is of "key=value". If there is more than one filter, then pass multiple flags (e.g., `--filter "foo=bar" --filter "bif=baz"`)

​	过滤标志（`--filter`）的格式为 "key=value"。如果有多个过滤器，请传递多个标志（例如 `--filter "foo=bar" --filter "bif=baz"`）。

The currently supported filters are:

​	当前支持的过滤器包括：

- until (`<timestamp>`) - only remove containers, images, and networks created before given timestamp
  - until (`<timestamp>`) - 仅删除在指定时间戳之前创建的容器、镜像和网络

- label (`label=<key>`, `label=<key>=<value>`, `label!=<key>`, or `label!=<key>=<value>`) - only remove containers, images, networks, and volumes with (or without, in case `label!=...` is used) the specified labels.
  - label (`label=<key>`、`label=<key>=<value>`、`label!=<key>` 或 `label!=<key>=<value>`) - 仅删除带有（或不带有，使用 `label!=...` 时）指定标签的容器、镜像、网络和卷。


The `until` filter can be Unix timestamps, date formatted timestamps, or Go duration strings (e.g. `10m`, `1h30m`) computed relative to the daemon machine’s time. Supported formats for date formatted time stamps include RFC3339Nano, RFC3339, `2006-01-02T15:04:05`, `2006-01-02T15:04:05.999999999`, `2006-01-02T07:00`, and `2006-01-02`. The local timezone on the daemon will be used if you do not provide either a `Z` or a `+-00:00` timezone offset at the end of the timestamp. When providing Unix timestamps enter seconds[.nanoseconds], where seconds is the number of seconds that have elapsed since January 1, 1970 (midnight UTC/GMT), not counting leap seconds (aka Unix epoch or Unix time), and the optional .nanoseconds field is a fraction of a second no more than nine digits long.

​	`until` 过滤器可以是 Unix 时间戳、日期格式的时间戳或相对于守护进程机器时间计算的 Go 时长字符串（例如 `10m`、`1h30m`）。日期格式的时间戳支持的格式包括 RFC3339Nano、RFC3339、`2006-01-02T15:04:05`、`2006-01-02T15:04:05.999999999`、`2006-01-02T07:00` 和 `2006-01-02`。如果时间戳末尾未指定 `Z` 或 `+-00:00` 时区偏移量，将使用守护进程的本地时区。提供 Unix 时间戳时，请输入秒[.纳秒]，其中秒数是从 1970 年 1 月 1 日起的秒数（UTC/GMT 零点），可选的 .纳秒字段为不超过九位数的小数部分。

The `label` filter accepts two formats. One is the `label=...` (`label=<key>` or `label=<key>=<value>`), which removes containers, images, networks, and volumes with the specified labels. The other format is the `label!=...` (`label!=<key>` or `label!=<key>=<value>`), which removes containers, images, networks, and volumes without the specified labels.

​	`label` 过滤器接受两种格式。一种是 `label=...`（`label=<key>` 或 `label=<key>=<value>`），会删除带有指定标签的容器、镜像、网络和卷。另一种格式是 `label!=...`（`label!=<key>` 或 `label!=<key>=<value>`），会删除没有指定标签的容器、镜像、网络和卷。
