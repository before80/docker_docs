+++
title = "docker stack services"
date = 2024-10-23T14:54:43+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/stack/services/](https://docs.docker.com/reference/cli/docker/stack/services/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker stack services

| Description | List the services in the stack          |
| :---------- | --------------------------------------- |
| Usage       | `docker stack services [OPTIONS] STACK` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 编排器。

## Description

Lists the services that are running as part of the specified stack.

​	列出指定栈中运行的服务。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理器和工作节点，请参阅 Swarm 模式部分。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --filter`](https://docs.docker.com/reference/cli/docker/stack/services/#filter) |         | 根据提供的条件过滤输出 Filter output based on conditions provided |
| [`--format`](https://docs.docker.com/reference/cli/docker/stack/services/#format) |         | 使用自定义模板格式化输出：'table': 表格格式输出并包含列标题（默认） 'table TEMPLATE': 使用指定的 Go 模板以表格格式输出 'json': 以 JSON 格式输出 'TEMPLATE': 使用指定的 Go 模板输出。有关模板格式化的更多信息，请参阅 https://docs.docker.com/go/formatting/  Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `-q, --quiet`                                                |         | 仅显示 ID Only display IDs                                   |

## Examples

The following command shows all services in the `myapp` stack:

​	以下命令显示 `myapp` 栈中的所有服务：

```console
$ docker stack services myapp

ID            NAME            REPLICAS  IMAGE                                                                          COMMAND
7be5ei6sqeye  myapp_web       1/1       nginx@sha256:23f809e7fd5952e7d5be065b4d3643fbbceccd349d537b62a123ef2201bc886f
dn7m7nhhfb9y  myapp_db        1/1       mysql@sha256:a9a5b559f8821fe73d58c3606c812d1c044868d42c63817fa5125fd9d8b7b539
```

### Filtering (`--filter`)

The filtering flag (`-f` or `--filter`) format is a `key=value` pair. If there is more than one filter, then pass multiple flags (e.g. `--filter "foo=bar" --filter "bif=baz"`). Multiple filter flags are combined as an `OR` filter.

​	过滤标志 (`-f` 或 `--filter`) 的格式为 `key=value`。如果有多个过滤条件，则传递多个标志（例如 `--filter "foo=bar" --filter "bif=baz"`）。多个过滤条件按 `OR` 逻辑组合。

The following command shows both the `web` and `db` services:

​	以下命令显示 `web` 和 `db` 服务：

```console
$ docker stack services --filter name=myapp_web --filter name=myapp_db myapp

ID            NAME            REPLICAS  IMAGE                                                                          COMMAND
7be5ei6sqeye  myapp_web       1/1       nginx@sha256:23f809e7fd5952e7d5be065b4d3643fbbceccd349d537b62a123ef2201bc886f
dn7m7nhhfb9y  myapp_db        1/1       mysql@sha256:a9a5b559f8821fe73d58c3606c812d1c044868d42c63817fa5125fd9d8b7b539
```

The currently supported filters are:

​	当前支持的过滤器包括：

- id / ID (`--filter id=7be5ei6sqeye`, or `--filter ID=7be5ei6sqeye`)

- label (`--filter label=key=value`)

- mode (`--filter mode=replicated`, or `--filter mode=global`)

  - Swarm: not supported
  
- name (`--filter name=myapp_web`)

- node (`--filter node=mynode`)

  - Swarm: not supported
  
- service (`--filter service=web`)

  - Swarm: not supported

### Format the output (`--format`)

The formatting options (`--format`) pretty-prints services output using a Go template.

Valid placeholders for the Go template are listed below:

| Placeholder | Description                                                  |
| ----------- | ------------------------------------------------------------ |
| `.ID`       | Service ID                                                   |
| `.Name`     | Service name                                                 |
| `.Mode`     | 服务模式（replicated, global） Service mode (replicated, global) |
| `.Replicas` | 服务副本数 Service replicas                                  |
| `.Image`    | Service image                                                |

When using the `--format` option, the `stack services` command will either output the data exactly as the template declares or, when using the `table` directive, includes column headers as well.

​	使用 `--format` 选项时，`stack services` 命令将按模板指定的方式输出数据，或者在使用 `table` 指令时包含列标题。

The following example uses a template without headers and outputs the `ID`, `Mode`, and `Replicas` entries separated by a colon (`:`) for all services:

​	以下示例使用无标题模板，输出所有服务的 `ID`、`Mode` 和 `Replicas` 条目，以冒号 (`:`) 分隔：

```console
$ docker stack services --format "{{.ID}}: {{.Mode}} {{.Replicas}}"

0zmvwuiu3vue: replicated 10/10
fm6uf97exkul: global 5/5
```

To list all services in JSON format, use the `json` directive:

​	若要以 JSON 格式列出所有服务，请使用 `json` 指令。



```console
$ docker stack services ls --format json
{"ID":"0axqbl293vwm","Image":"localstack/localstack:latest","Mode":"replicated","Name":"myapp_localstack","Ports":"*:4566-\u003e4566/tcp, *:8080-\u003e8080/tcp","Replicas":"0/1"}
{"ID":"384xvtzigz3p","Image":"redis:6.0.9-alpine3.12","Mode":"replicated","Name":"myapp_redis","Ports":"*:6379-\u003e6379/tcp","Replicas":"1/1"}
{"ID":"hyujct8cnjkk","Image":"postgres:13.2-alpine","Mode":"replicated","Name":"myapp_repos-db","Ports":"*:5432-\u003e5432/tcp","Replicas":"0/1"}
```

