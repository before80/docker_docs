+++
title = "docker service ps"
date = 2024-10-23T14:54:43+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/service/ps/](https://docs.docker.com/reference/cli/docker/service/ps/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker service ps

| Description | List the tasks of one or more services             |
| :---------- | -------------------------------------------------- |
| Usage       | `docker service ps [OPTIONS] SERVICE [SERVICE...]` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 该命令适用于 Swarm 编排器。

## Description

Lists the tasks that are running as part of the specified services.

​	列出作为指定服务一部分运行的任务。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理节点和工作节点，请参阅文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --filter`](https://docs.docker.com/reference/cli/docker/service/ps/#filter) |         | 根据给定条件过滤输出 Filter output based on conditions provided |
| [`--format`](https://docs.docker.com/reference/cli/docker/service/ps/#format) |         | 使用 Go 模板美化打印任务输出 Pretty-print tasks using a Go template |
| `--no-resolve`                                               |         | 不在输出中将 ID 映射为名称 Do not map IDs to Names           |
| `--no-trunc`                                                 |         | 不截断输出 Do not truncate output                            |
| `-q, --quiet`                                                |         | 仅显示任务 ID Only display task IDs                          |

## Examples

### 列出服务的一部分任务 List the tasks that are part of a service

The following command shows all the tasks that are part of the `redis` service:

​	以下命令显示 `redis` 服务的所有任务：

```console
$ docker service ps redis

ID             NAME      IMAGE        NODE      DESIRED STATE  CURRENT STATE          ERROR  PORTS
0qihejybwf1x   redis.1   redis:3.0.5  manager1  Running        Running 8 seconds
bk658fpbex0d   redis.2   redis:3.0.5  worker2   Running        Running 9 seconds
5ls5s5fldaqg   redis.3   redis:3.0.5  worker1   Running        Running 9 seconds
8ryt076polmc   redis.4   redis:3.0.5  worker1   Running        Running 9 seconds
1x0v8yomsncd   redis.5   redis:3.0.5  manager1  Running        Running 8 seconds
71v7je3el7rr   redis.6   redis:3.0.5  worker2   Running        Running 9 seconds
4l3zm9b7tfr7   redis.7   redis:3.0.5  worker2   Running        Running 9 seconds
9tfpyixiy2i7   redis.8   redis:3.0.5  worker1   Running        Running 9 seconds
3w1wu13yupln   redis.9   redis:3.0.5  manager1  Running        Running 8 seconds
8eaxrb2fqpbn   redis.10  redis:3.0.5  manager1  Running        Running 8 seconds
```

In addition to running tasks, the output also shows the task history. For example, after updating the service to use the `redis:3.0.6` image, the output may look like this:

​	除了正在运行的任务外，输出还显示了任务历史。例如，在将服务更新为使用 `redis:3.0.6` 镜像后，输出可能如下所示：

```console
$ docker service ps redis

ID            NAME         IMAGE        NODE      DESIRED STATE  CURRENT STATE                   ERROR  PORTS
50qe8lfnxaxk  redis.1      redis:3.0.6  manager1  Running        Running 6 seconds ago
ky2re9oz86r9   \_ redis.1  redis:3.0.5  manager1  Shutdown       Shutdown 8 seconds ago
3s46te2nzl4i  redis.2      redis:3.0.6  worker2   Running        Running less than a second ago
nvjljf7rmor4   \_ redis.2  redis:3.0.6  worker2   Shutdown       Rejected 23 seconds ago        "No such image: redis@sha256:6…"
vtiuz2fpc0yb   \_ redis.2  redis:3.0.5  worker2   Shutdown       Shutdown 1 second ago
jnarweeha8x4  redis.3      redis:3.0.6  worker1   Running        Running 3 seconds ago
vs448yca2nz4   \_ redis.3  redis:3.0.5  worker1   Shutdown       Shutdown 4 seconds ago
jf1i992619ir  redis.4      redis:3.0.6  worker1   Running        Running 10 seconds ago
blkttv7zs8ee   \_ redis.4  redis:3.0.5  worker1   Shutdown       Shutdown 11 seconds ago
```

The number of items in the task history is determined by the `--task-history-limit` option that was set when initializing the swarm. You can change the task history retention limit using the [`docker swarm update`]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmupdate" >}}) command.

​	任务历史记录的条目数量由初始化 Swarm 时设置的 `--task-history-limit` 选项决定。您可以使用 [`docker swarm update`]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmupdate" >}}) 命令更改任务历史记录保留限制。

When deploying a service, docker resolves the digest for the service's image, and pins the service to that digest. The digest is not shown by default, but is printed if `--no-trunc` is used. The `--no-trunc` option also shows the non-truncated task ID, and error messages, as can be seen in the following example:

​	在部署服务时，Docker 解析服务镜像的摘要，并将服务固定到该摘要。默认情况下不会显示摘要，但使用 `--no-trunc` 选项时会显示。`--no-trunc` 选项还显示未截断的任务 ID 和错误信息，如以下示例所示：



```console
$ docker service ps --no-trunc redis

ID                          NAME         IMAGE                                                                                NODE      DESIRED STATE  CURRENT STATE            ERROR                                                                                           PORTS
50qe8lfnxaxksi9w2a704wkp7   redis.1      redis:3.0.6@sha256:6a692a76c2081888b589e26e6ec835743119fe453d67ecf03df7de5b73d69842  manager1  Running        Running 5 minutes ago
ky2re9oz86r9556i2szb8a8af   \_ redis.1   redis:3.0.5@sha256:f8829e00d95672c48c60f468329d6693c4bdd28d1f057e755f8ba8b40008682e  worker2   Shutdown       Shutdown 5 minutes ago
bk658fpbex0d57cqcwoe3jthu   redis.2      redis:3.0.6@sha256:6a692a76c2081888b589e26e6ec835743119fe453d67ecf03df7de5b73d69842  worker2   Running        Running 5 seconds
nvjljf7rmor4htv7l8rwcx7i7   \_ redis.2   redis:3.0.6@sha256:6a692a76c2081888b589e26e6ec835743119fe453d67ecf03df7de5b73d69842  worker2   Shutdown       Rejected 5 minutes ago   "No such image: redis@sha256:6a692a76c2081888b589e26e6ec835743119fe453d67ecf03df7de5b73d69842"
```

### Filtering (`--filter`)

The filtering flag (`-f` or `--filter`) format is a `key=value` pair. If there is more than one filter, then pass multiple flags (e.g. `--filter "foo=bar" --filter "bif=baz"`). Multiple filter flags are combined as an `OR` filter. For example, `-f name=redis.1 -f name=redis.7` returns both `redis.1` and `redis.7` tasks.

​	过滤标志（`-f` 或 `--filter`）的格式为 `key=value` 对。如果有多个过滤条件，请传递多个标志（例如 `--filter "foo=bar" --filter "bif=baz"`）。多个过滤标志将组合为 `OR` 过滤器。例如，`-f name=redis.1 -f name=redis.7` 将返回 `redis.1` 和 `redis.7` 任务。

The currently supported filters are:

​	当前支持的过滤器包括：

- [id](https://docs.docker.com/reference/cli/docker/service/ps/#id)
- [name](https://docs.docker.com/reference/cli/docker/service/ps/#name)
- [node](https://docs.docker.com/reference/cli/docker/service/ps/#node)
- [desired-state](https://docs.docker.com/reference/cli/docker/service/ps/#desired-state)

#### id

The `id` filter matches on all or a prefix of a task's ID.

​	`id` 过滤器匹配任务 ID 的全部或前缀。

```console
$ docker service ps -f "id=8" redis

ID             NAME      IMAGE        NODE      DESIRED STATE  CURRENT STATE      ERROR  PORTS
8ryt076polmc   redis.4   redis:3.0.6  worker1   Running        Running 9 seconds
8eaxrb2fqpbn   redis.10  redis:3.0.6  manager1  Running        Running 8 seconds
```

#### name

The `name` filter matches on task names.

​	`name` 过滤器匹配任务名称。

```console
$ docker service ps -f "name=redis.1" redis

ID            NAME     IMAGE        NODE      DESIRED STATE  CURRENT STATE      ERROR  PORTS
qihejybwf1x5  redis.1  redis:3.0.6  manager1  Running        Running 8 seconds
```

#### node

The `node` filter matches on a node name or a node ID.

​	`node` 过滤器匹配节点名称或节点 ID。

```console
$ docker service ps -f "node=manager1" redis

ID            NAME      IMAGE        NODE      DESIRED STATE  CURRENT STATE      ERROR  PORTS
0qihejybwf1x  redis.1   redis:3.0.6  manager1  Running        Running 8 seconds
1x0v8yomsncd  redis.5   redis:3.0.6  manager1  Running        Running 8 seconds
3w1wu13yupln  redis.9   redis:3.0.6  manager1  Running        Running 8 seconds
8eaxrb2fqpbn  redis.10  redis:3.0.6  manager1  Running        Running 8 seconds
```

#### desired-state

The `desired-state` filter can take the values `running`, `shutdown`, or `accepted`.

​	`desired-state` 过滤器可以取值 `running`、`shutdown` 或 `accepted`。

### Format the output (`--format`)

The formatting options (`--format`) pretty-prints tasks output using a Go template.

​	格式化选项（`--format`）使用 Go 模板以美观的方式打印任务输出。

Valid placeholders for the Go template are listed below:

​	Go 模板的有效占位符如下所示：

| Placeholder     | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| `.ID`           | 任务 ID  Task ID                                             |
| `.Name`         | 任务名称  Task name                                          |
| `.Image`        | 任务镜像 Task image                                          |
| `.Node`         | 节点 ID  Node ID                                             |
| `.DesiredState` | 任务的目标状态（`running`、`shutdown` 或 `accepted`）  Desired state of the task (`running`, `shutdown`, or `accepted`) |
| `.CurrentState` | 任务的当前状态  Current state of the task                    |
| `.Error`        | 错误信息  Error                                              |
| `.Ports`        | 任务发布的端口  Task published ports                         |

When using the `--format` option, the `service ps` command will either output the data exactly as the template declares or, when using the `table` directive, includes column headers as well.

​	使用 `--format` 选项时，`service ps` 命令将直接按模板输出数据；如果使用 `table` 指令，还会包含列标题。

The following example uses a template without headers and outputs the `Name` and `Image` entries separated by a colon (`:`) for all tasks:

​	以下示例使用不含标题的模板，并为所有任务输出 `Name` 和 `Image` 条目，以冒号（`:`）分隔：

```console
$ docker service ps --format "{{.Name}}: {{.Image}}" top

top.1: busybox
top.2: busybox
top.3: busybox
```
