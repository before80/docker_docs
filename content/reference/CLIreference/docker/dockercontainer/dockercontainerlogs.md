+++
title = "docker container logs"
date = 2024-10-23T14:54:43+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/logs/](https://docs.docker.com/reference/cli/docker/container/logs/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container logs

| Description | Fetch the logs of a container               |
| :---------- | ------------------------------------------- |
| Usage       | `docker container logs [OPTIONS] CONTAINER` |
| Aliases     | `docker logs`                               |

## Description

The `docker logs` command batch-retrieves logs present at the time of execution.

​	`docker logs` 命令会批量检索执行时存在的日志。

For more information about selecting and configuring logging drivers, refer to [Configure logging drivers]({{< ref "/manuals/DockerEngine/Logsandmetrics/Configureloggingdrivers" >}}).

​	有关选择和配置日志驱动程序的更多信息，请参考 [配置日志驱动程序]({{< ref "/manuals/DockerEngine/Logsandmetrics/Configureloggingdrivers" >}})。

The `docker logs --follow` command will continue streaming the new output from the container's `STDOUT` and `STDERR`.

​	`docker logs --follow` 命令将继续从容器的 `STDOUT` 和 `STDERR` 流式传输新输出。

Passing a negative number or a non-integer to `--tail` is invalid and the value is set to `all` in that case.

​	传递负数或非整数值给 `--tail` 是无效的，在这种情况下值将设置为 `all`。

The `docker logs --timestamps` command will add an [RFC3339Nano timestamp](https://pkg.go.dev/time#RFC3339Nano) , for example `2014-09-16T06:17:46.000000000Z`, to each log entry. To ensure that the timestamps are aligned the nano-second part of the timestamp will be padded with zero when necessary.

​	`docker logs --timestamps` 命令将为每个日志条目添加 [RFC3339Nano 时间戳](https://pkg.go.dev/time#RFC3339Nano)，例如 `2014-09-16T06:17:46.000000000Z`。为了确保时间戳对齐，时间戳的纳秒部分将在必要时用零填充。

The `docker logs --details` command will add on extra attributes, such as environment variables and labels, provided to `--log-opt` when creating the container.

​	`docker logs --details` 命令将添加创建容器时提供给 `--log-opt` 的额外属性，例如环境变量和标签。

The `--since` option shows only the container logs generated after a given date. You can specify the date as an RFC 3339 date, a UNIX timestamp, or a Go duration string (e.g. `1m30s`, `3h`). Besides RFC3339 date format you may also use RFC3339Nano, `2006-01-02T15:04:05`, `2006-01-02T15:04:05.999999999`, `2006-01-02T07:00`, and `2006-01-02`. The local timezone on the client will be used if you do not provide either a `Z` or a `+-00:00` timezone offset at the end of the timestamp. When providing Unix timestamps enter seconds[.nanoseconds], where seconds is the number of seconds that have elapsed since January 1, 1970 (midnight UTC/GMT), not counting leap seconds (aka Unix epoch or Unix time), and the optional .nanoseconds field is a fraction of a second no more than nine digits long. You can combine the `--since` option with either or both of the `--follow` or `--tail` options.

​	`--since` 选项仅显示在给定日期之后生成的容器日志。您可以将日期指定为 RFC 3339 日期、UNIX 时间戳或 Go 持续时间字符串（例如 `1m30s`、`3h`）。除了 RFC3339 日期格式外，您还可以使用 RFC3339Nano、`2006-01-02T15:04:05`、`2006-01-02T15:04:05.999999999`、`2006-01-02T07:00` 和 `2006-01-02`。如果您未在时间戳末尾提供 `Z` 或 `+-00:00` 时区偏移，则将使用客户端的本地时区。当提供 UNIX 时间戳时，输入秒[.纳秒]，其中秒是自 1970 年 1 月 1 日（UTC/GMT 午夜）以来经过的秒数，不计算闰秒（即 UNIX 纪元或 UNIX 时间），可选的 .纳秒字段是最多九位数字的秒的小数部分。您可以将 `--since` 选项与 `--follow` 或 `--tail` 选项结合使用。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| `--details`                                                  |         | 显示提供给日志的额外细节 Show extra details provided to logs |
| `-f, --follow`                                               |         | 跟随日志输出 Follow log output                               |
| `--since`                                                    |         | 显示自时间戳以来的日志（例如 `2013-01-02T13:23:37Z` 或相对时间，例如 `42m` 表示 42 分钟） Show logs since timestamp (e.g. `2013-01-02T13:23:37Z`) or relative (e.g. `42m` for 42 minutes) |
| `-n, --tail`                                                 | `all`   | 从日志末尾显示的行数 Number of lines to show from the end of the logs |
| `-t, --timestamps`                                           |         | 显示时间戳 Show timestamps                                   |
| [`--until`](https://docs.docker.com/reference/cli/docker/container/logs/#until) |         | API 1.35+ 在时间戳之前显示日志（例如 `2013-01-02T13:23:37Z` 或相对时间，例如 `42m` 表示 42 分钟） API 1.35+ Show logs before a timestamp (e.g. `2013-01-02T13:23:37Z`) or relative (e.g. `42m` for 42 minutes) |

## Examples

### 检索到特定时间点的日志（`--until`） Retrieve logs until a specific point in time (`--until`)

In order to retrieve logs before a specific point in time, run:

​	为了检索在特定时间点之前的日志，运行：



```console
$ docker run --name test -d busybox sh -c "while true; do $(echo date); sleep 1; done"
$ date
Tue 14 Nov 2017 16:40:00 CET
$ docker logs -f --until=2s test
Tue 14 Nov 2017 16:40:00 CET
Tue 14 Nov 2017 16:40:01 CET
Tue 14 Nov 2017 16:40:02 CET
```
