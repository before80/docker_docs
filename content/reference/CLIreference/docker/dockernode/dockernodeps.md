+++
title = "docker node ps"
date = 2024-10-23T14:54:43+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/node/ps/](https://docs.docker.com/reference/cli/docker/node/ps/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker node ps

| Description | List tasks running on one or more nodes, defaults to current node |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker node ps [OPTIONS] [NODE...]`                         |

Swarm This command works with the Swarm orchestrator.

​	Swarm 该命令适用于 Swarm 协调器。

## Description

Lists all the tasks on a Node that Docker knows about. You can filter using the `-f` or `--filter` flag. Refer to the [filtering](https://docs.docker.com/reference/cli/docker/node/ps/#filter) section for more information about available filter options.

​	列出 Docker 已知的节点上的所有任务。您可以使用 `-f` 或 `--filter` 标志进行过滤。有关可用过滤器选项的更多信息，请参阅[过滤](https://docs.docker.com/reference/cli/docker/node/ps/#filter)部分。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。有关管理节点和工作节点的更多信息，请参阅文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --filter`](https://docs.docker.com/reference/cli/docker/node/ps/#filter) |         | 根据提供的条件过滤输出 Filter output based on conditions provided |
| [`--format`](https://docs.docker.com/reference/cli/docker/node/ps/#format) |         | 使用 Go 模板美化任务输出 Pretty-print tasks using a Go template |
| `--no-resolve`                                               |         | 不将 ID 映射为名称 Do not map IDs to Names                   |
| `--no-trunc`                                                 |         | 不截断输出 Do not truncate output                            |
| `-q, --quiet`                                                |         | 仅显示任务 ID Only display task IDs                          |

## Examples



```console
$ docker node ps swarm-manager1

NAME                                IMAGE        NODE            DESIRED STATE  CURRENT STATE
redis.1.7q92v0nr1hcgts2amcjyqg3pq   redis:3.0.6  swarm-manager1  Running        Running 5 hours
redis.6.b465edgho06e318egmgjbqo4o   redis:3.0.6  swarm-manager1  Running        Running 29 seconds
redis.7.bg8c07zzg87di2mufeq51a2qp   redis:3.0.6  swarm-manager1  Running        Running 5 seconds
redis.9.dkkual96p4bb3s6b10r7coxxt   redis:3.0.6  swarm-manager1  Running        Running 5 seconds
redis.10.0tgctg8h8cech4w0k0gwrmr23  redis:3.0.6  swarm-manager1  Running        Running 5 seconds
```

### Filtering (`--filter`)

The filtering flag (`-f` or `--filter`) format is of "key=value". If there is more than one filter, then pass multiple flags (e.g., `--filter "foo=bar" --filter "bif=baz"`).

​	过滤标志（`-f` 或 `--filter`）的格式为 "key=value"。如果有多个过滤条件，请传递多个标志（例如，`--filter "foo=bar" --filter "bif=baz"`）。

The currently supported filters are:

​	当前支持的过滤器有：

- [name](https://docs.docker.com/reference/cli/docker/node/ps/#name)
- [id](https://docs.docker.com/reference/cli/docker/node/ps/#id)
- [label](https://docs.docker.com/reference/cli/docker/node/ps/#label)
- [desired-state](https://docs.docker.com/reference/cli/docker/node/ps/#desired-state)

#### name

The `name` filter matches on all or part of a task's name.

​	`name` 过滤器匹配任务名称的全部或部分。

The following filter matches all tasks with a name containing the `redis` string.

​	以下过滤器匹配名称包含 `redis` 字符串的所有任务。



```console
$ docker node ps -f name=redis swarm-manager1

NAME                                IMAGE        NODE            DESIRED STATE  CURRENT STATE
redis.1.7q92v0nr1hcgts2amcjyqg3pq   redis:3.0.6  swarm-manager1  Running        Running 5 hours
redis.6.b465edgho06e318egmgjbqo4o   redis:3.0.6  swarm-manager1  Running        Running 29 seconds
redis.7.bg8c07zzg87di2mufeq51a2qp   redis:3.0.6  swarm-manager1  Running        Running 5 seconds
redis.9.dkkual96p4bb3s6b10r7coxxt   redis:3.0.6  swarm-manager1  Running        Running 5 seconds
redis.10.0tgctg8h8cech4w0k0gwrmr23  redis:3.0.6  swarm-manager1  Running        Running 5 seconds
```

#### id

The `id` filter matches a task's id.

​	`id` 过滤器匹配任务的 ID。



```console
$ docker node ps -f id=bg8c07zzg87di2mufeq51a2qp swarm-manager1

NAME                                IMAGE        NODE            DESIRED STATE  CURRENT STATE
redis.7.bg8c07zzg87di2mufeq51a2qp   redis:3.0.6  swarm-manager1  Running        Running 5 seconds
```

#### label

The `label` filter matches tasks based on the presence of a `label` alone or a `label` and a value.

​	`label` 过滤器基于标签的存在（或标签和值）匹配任务。

The following filter matches tasks with the `usage` label regardless of its value.

​	以下过滤器匹配具有 `usage` 标签的任务，无论其值为何。



```console
$ docker node ps -f "label=usage"

NAME                               IMAGE        NODE            DESIRED STATE  CURRENT STATE
redis.6.b465edgho06e318egmgjbqo4o  redis:3.0.6  swarm-manager1  Running        Running 10 minutes
redis.7.bg8c07zzg87di2mufeq51a2qp  redis:3.0.6  swarm-manager1  Running        Running 9 minutes
```

#### desired-state

The `desired-state` filter can take the values `running`, `shutdown`, or `accepted`.

​	`desired-state` 过滤器可以取值 `running`、`shutdown` 或 `accepted`。

### Format the output (`--format`)

The formatting options (`--format`) pretty-prints tasks output using a Go template.

​	格式化选项（`--format`）使用 Go 模板美化任务输出。

Valid placeholders for the Go template are listed below:

​	Go 模板的有效占位符如下所示：

| Placeholder     | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| `.ID`           | 任务 ID Task ID                                              |
| `.Name`         | 任务名称 Task name                                           |
| `.Image`        | 任务镜像 Task image                                          |
| `.Node`         | 节点 ID  Node ID                                             |
| `.DesiredState` | 任务的预期状态（`running`, `shutdown`, 或 `accepted`） Desired state of the task (`running`, `shutdown`, or `accepted`) |
| `.CurrentState` | 任务的当前状态 Current state of the task                     |
| `.Error`        | 错误信息 Error                                               |
| `.Ports`        | 任务发布的端口 Task published ports                          |

When using the `--format` option, the `node ps` command will either output the data exactly as the template declares or, when using the `table` directive, includes column headers as well.

​	使用 `--format` 选项时，`node ps` 命令将按模板声明的确切数据输出，或在使用 `table` 指令时包含列标题。

The following example uses a template without headers and outputs the `Name` and `Image` entri separated by a colon (`:`) for all tasks:

​	以下示例使用无标题的模板，输出所有任务的 `Name` 和 `Image` 条目，用冒号（`:`）分隔：



```console
$ docker node ps --format "{{.Name}}: {{.Image}}"

top.1: busybox
top.2: busybox
top.3: busybox
```
