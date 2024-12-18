+++
title = "docker service logs"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/service/logs/](https://docs.docker.com/reference/cli/docker/service/logs/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker service logs

| Description | Fetch the logs of a service or task          |
| :---------- | -------------------------------------------- |
| Usage       | `docker service logs [OPTIONS] SERVICE|TASK` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 该命令适用于 Swarm 编排器。

## Description

The `docker service logs` command batch-retrieves logs present at the time of execution.

​	`docker service logs` 命令批量检索执行时的日志。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理节点和工作节点，请参阅文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

The `docker service logs` command can be used with either the name or ID of a service, or with the ID of a task. If a service is passed, it will display logs for all of the containers in that service. If a task is passed, it will only display logs from that particular task.

​	`docker service logs` 命令可以使用服务的名称或 ID，或者任务的 ID。如果提供服务，则会显示该服务中所有容器的日志。如果提供任务，则仅显示该特定任务的日志。

> **Note**
>
> This command is only functional for services that are started with the `json-file` or `journald` logging driver.
>
> ​	此命令仅对使用 `json-file` 或 `journald` 日志驱动程序启动的服务有效。

For more information about selecting and configuring logging drivers, refer to [Configure logging drivers]({{< ref "/manuals/DockerEngine/Logsandmetrics/Configureloggingdrivers" >}}).

​	有关选择和配置日志驱动程序的更多信息，请参阅[配置日志驱动程序]({{< ref "/manuals/DockerEngine/Logsandmetrics/Configureloggingdrivers" >}})。

The `docker service logs --follow` command will continue streaming the new output from the service's `STDOUT` and `STDERR`.

​	`docker service logs --follow` 命令将持续流式传输服务的 `STDOUT` 和 `STDERR` 的新输出。

Passing a negative number or a non-integer to `--tail` is invalid and the value is set to `all` in that case.

​	将负数或非整数传递给 `--tail` 是无效的，此时该值将设为 `all`。

The `docker service logs --timestamps` command will add an [RFC3339Nano timestamp](https://pkg.go.dev/time#RFC3339Nano) , for example `2014-09-16T06:17:46.000000000Z`, to each log entry. To ensure that the timestamps are aligned the nano-second part of the timestamp will be padded with zero when necessary.

​	`docker service logs --timestamps` 命令将为每条日志条目添加 [RFC3339Nano 时间戳](https://pkg.go.dev/time#RFC3339Nano)，例如 `2014-09-16T06:17:46.000000000Z`，以确保时间戳对齐，必要时纳秒部分会用零填充。

The `docker service logs --details` command will add on extra attributes, such as environment variables and labels, provided to `--log-opt` when creating the service.

​	`docker service logs --details` 命令会添加额外属性，如在创建服务时通过 `--log-opt` 提供的环境变量和标签。

The `--since` option shows only the service logs generated after a given date. You can specify the date as an RFC 3339 date, a UNIX timestamp, or a Go duration string (e.g. `1m30s`, `3h`). Besides RFC3339 date format you may also use RFC3339Nano, `2006-01-02T15:04:05`, `2006-01-02T15:04:05.999999999`, `2006-01-02T07:00`, and `2006-01-02`. The local timezone on the client will be used if you do not provide either a `Z` or a `+-00:00` timezone offset at the end of the timestamp. When providing Unix timestamps enter seconds[.nanoseconds], where seconds is the number of seconds that have elapsed since January 1, 1970 (midnight UTC/GMT), not counting leap seconds (aka Unix epoch or Unix time), and the optional .nanoseconds field is a fraction of a second no more than nine digits long. You can combine the `--since` option with either or both of the `--follow` or `--tail` options.

​	`--since` 选项仅显示给定日期之后生成的服务日志。您可以将日期指定为 RFC 3339 日期、UNIX 时间戳或 Go 持续时间字符串（例如 `1m30s`、`3h`）。除 RFC3339 日期格式外，还可使用 RFC3339Nano、`2006-01-02T15:04:05`、`2006-01-02T15:04:05.999999999`、`2006-01-02T07:00` 和 `2006-01-02`。如果您未在时间戳末尾提供 `Z` 或 `+-00:00` 时区偏移量，则使用客户端的本地时区。Unix 时间戳格式为 seconds[.nanoseconds]，其中 seconds 表示自 1970 年 1 月 1 日以来经过的秒数（即 Unix 时间戳或 Unix 时间），可选的 .nanoseconds 字段表示不超过九位的小数秒。您可以将 `--since` 选项与 `--follow` 或 `--tail` 选项组合使用。

## Options

| Option             | Default | Description                                                  |
| ------------------ | ------- | ------------------------------------------------------------ |
| `--details`        |         | API 1.30+ 显示日志的附加详细信息 API 1.30+ Show extra details provided to logs |
| `-f, --follow`     |         | 跟随日志输出 Follow log output                               |
| `--no-resolve`     |         | 不在输出中将 ID 映射为名称 Do not map IDs to Names in output |
| `--no-task-ids`    |         | 输出中不包含任务 ID Do not include task IDs in output        |
| `--no-trunc`       |         | 不截断输出 Do not truncate output                            |
| `--raw`            |         | API 1.30+ 不以整洁的格式显示日志 API 1.30+ Do not neatly format logs |
| `--since`          |         | 显示自时间戳以来的日志（例如 `2013-01-02T13:23:37Z`）或相对时间（例如 `42m` 表示 42 分钟） Show logs since timestamp (e.g. `2013-01-02T13:23:37Z`) or relative (e.g. `42m` for 42 minutes) |
| `-n, --tail`       | `all`   | 显示日志末尾的行数 Number of lines to show from the end of the logs |
| `-t, --timestamps` |         | 显示时间戳 Show timestamps                                   |
