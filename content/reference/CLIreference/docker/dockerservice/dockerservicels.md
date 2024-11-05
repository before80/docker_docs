+++
title = "docker service ls"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/service/ls/](https://docs.docker.com/reference/cli/docker/service/ls/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker service ls

| Description | List services                 |
| :---------- | ----------------------------- |
| Usage       | `docker service ls [OPTIONS]` |
| Aliases     | `docker service list`         |

Swarm This command works with the Swarm orchestrator.

​	Swarm 该命令适用于 Swarm 编排器。

## Description

This command lists services that are running in the swarm.

​	此命令列出在 Swarm 中运行的服务。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理节点和工作节点，请参阅文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --filter`](https://docs.docker.com/reference/cli/docker/service/ls/#filter) |         | 基于给定条件过滤输出 Filter output based on conditions provided |
| [`--format`](https://docs.docker.com/reference/cli/docker/service/ls/#format) |         | 使用自定义模板格式化输出：'table': 表格式显示输出，带有列标题（默认） 'table TEMPLATE': 使用给定的 Go 模板以表格式显示输出 'json': 以 JSON 格式输出 'TEMPLATE': 使用给定的 Go 模板输出。有关模板格式化的更多信息，请参阅 https://docs.docker.com/go/formatting/  Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `-q, --quiet`                                                |         | 仅显示 ID  Only display IDs                                  |

## Examples

On a manager node:

​	在管理节点上：

```console
$ docker service ls

ID            NAME      MODE            REPLICAS             IMAGE
c8wgl7q4ndfd  frontend  replicated      5/5                  nginx:alpine
dmu1ept4cxcf  redis     replicated      3/3                  redis:3.0.6
iwe3278osahj  mongo     global          7/7                  mongo:3.3
hh08h9uu8uwr  job       replicated-job  1/1 (3/5 completed)  nginx:latest
```

The `REPLICAS` column shows both the actual and desired number of tasks for the service. If the service is in `replicated-job` or `global-job`, it will additionally show the completion status of the job as completed tasks over total tasks the job will execute.

​	`REPLICAS` 列显示服务的实际和期望任务数。如果服务是 `replicated-job` 或 `global-job` 模式，则还会显示作业的完成状态为已完成任务数与总任务数之比。

### Filtering (`--filter`)

The filtering flag (`-f` or `--filter`) format is of "key=value". If there is more than one filter, then pass multiple flags (e.g., `--filter "foo=bar" --filter "bif=baz"`).

​	过滤标志 (`-f` 或 `--filter`) 格式为 "key=value"。如果有多个过滤条件，则传递多个标志（例如，`--filter "foo=bar" --filter "bif=baz"`）。

The currently supported filters are:

​	当前支持的过滤器包括：

- [id](https://docs.docker.com/reference/cli/docker/service/ls/#id)
- [label](https://docs.docker.com/reference/cli/docker/service/ls/#label)
- [mode](https://docs.docker.com/reference/cli/docker/service/ls/#mode)
- [name](https://docs.docker.com/reference/cli/docker/service/ls/#name)

#### id

The `id` filter matches all or the prefix of a service's ID.

​	`id` 过滤器匹配服务 ID 的完整值或前缀。

The following filter matches services with an ID starting with `0bcjw`:

​	以下过滤器匹配 ID 以 `0bcjw` 开头的服务：



```console
$ docker service ls -f "id=0bcjw"
ID            NAME   MODE        REPLICAS  IMAGE
0bcjwfh8ychr  redis  replicated  1/1       redis:3.0.6
```

#### label

The `label` filter matches services based on the presence of a `label` alone or a `label` and a value.

​	`label` 过滤器根据 `label` 的存在或指定的 `label` 和其值来匹配服务。

The following filter matches all services with a `project` label regardless of its value:

​	以下过滤器匹配所有带有 `project` 标签的服务，不论其值为何：



```console
$ docker service ls --filter label=project
ID            NAME       MODE        REPLICAS  IMAGE
01sl1rp6nj5u  frontend2  replicated  1/1       nginx:alpine
36xvvwwauej0  frontend   replicated  5/5       nginx:alpine
74nzcxxjv6fq  backend    replicated  3/3       redis:3.0.6
```

The following filter matches only services with the `project` label with the `project-a` value.

​	以下过滤器仅匹配 `project` 标签的值为 `project-a` 的服务。



```console
$ docker service ls --filter label=project=project-a
ID            NAME      MODE        REPLICAS  IMAGE
36xvvwwauej0  frontend  replicated  5/5       nginx:alpine
74nzcxxjv6fq  backend   replicated  3/3       redis:3.0.6
```

#### mode

The `mode` filter matches on the mode (either `replicated` or `global`) of a service.

​	`mode` 过滤器匹配服务的模式（`replicated` 或 `global`）。

The following filter matches only `global` services.

​	以下过滤器仅匹配 `global` 服务。

```console
$ docker service ls --filter mode=global
ID                  NAME                MODE                REPLICAS            IMAGE
w7y0v2yrn620        top                 global              1/1                 busybox
```

#### name

The `name` filter matches on all or the prefix of a service's name.

​	`name` 过滤器匹配服务名称的完整值或前缀。

The following filter matches services with a name starting with `redis`.

​	以下过滤器匹配名称以 `redis` 开头的服务。

```console
$ docker service ls --filter name=redis
ID            NAME   MODE        REPLICAS  IMAGE
0bcjwfh8ychr  redis  replicated  1/1       redis:3.0.6
```

### Format the output (`--format`)

The formatting options (`--format`) pretty-prints services output using a Go template.

​	格式化选项 (`--format`) 使用 Go 模板美化打印服务输出。

Valid placeholders for the Go template are listed below:

​	以下是 Go 模板中有效的占位符：

| Placeholder | Description                                                  |
| ----------- | ------------------------------------------------------------ |
| `.ID`       | 服务 ID  Service ID                                          |
| `.Name`     | Service name                                                 |
| `.Mode`     | 服务模式（replicated, global）  Service mode (replicated, global) |
| `.Replicas` | 服务副本 Service replicas                                    |
| `.Image`    | Service image                                                |
| `.Ports`    | 服务在 ingress 模式下发布的端口  Service ports published in ingress mode |

When using the `--format` option, the `service ls` command will either output the data exactly as the template declares or, when using the `table` directive, includes column headers as well.

​	当使用 `--format` 选项时，`service ls` 命令会精确地按照模板声明的数据进行输出，或者在使用 `table` 指令时包含列标题。

The following example uses a template without headers and outputs the `ID`, `Mode`, and `Replicas` entries separated by a colon (`:`) for all services:

​	以下示例使用无标题模板输出所有服务的 `ID`、`Mode` 和 `Replicas` 项，以冒号 (`:`) 分隔：



```console
$ docker service ls --format "{{.ID}}: {{.Mode}} {{.Replicas}}"

0zmvwuiu3vue: replicated 10/10
fm6uf97exkul: global 5/5
```

To list all services in JSON format, use the `json` directive:

​	要以 JSON 格式列出所有服务，请使用 `json` 指令。

```console
$ docker service ls --format json
{"ID":"ssniordqolsi","Image":"hello-world:latest","Mode":"replicated","Name":"hello","Ports":"","Replicas":"0/1"}
```

