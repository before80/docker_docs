+++
title = "docker stack ls"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/stack/ls/](https://docs.docker.com/reference/cli/docker/stack/ls/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker stack ls

| Description | List stacks                 |
| :---------- | --------------------------- |
| Usage       | `docker stack ls [OPTIONS]` |
| Aliases     | `docker stack list`         |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 编排器。

## Description

Lists the stacks.

​	列出栈。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理器和工作节点，请参阅文档中的 Swarm 模式部分。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`--format`](https://docs.docker.com/reference/cli/docker/stack/ls/#format) |         | 使用自定义模板格式化输出：`table`：以表格式显示输出并带列标题（默认） `table TEMPLATE`：使用指定的 Go 模板以表格式显示输出 `json`：以 JSON 格式显示输出 `TEMPLATE`：使用指定的 Go 模板显示输出。详细信息请参阅 [模板格式化文档](https://docs.docker.com/go/formatting/)。   Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |

## Examples

The following command shows all stacks and some additional information:

​	以下命令显示所有栈及一些附加信息：

```console
$ docker stack ls

ID                 SERVICES            ORCHESTRATOR
myapp              2                   Kubernetes
vossibility-stack  6                   Swarm
```

### Format the output (`--format`)

The formatting option (`--format`) pretty-prints stacks using a Go template.

​	格式化选项 (`--format`) 使用 Go 模板对栈进行漂亮地打印。

Valid placeholders for the Go template are listed below:

​	Go 模板的有效占位符如下：

| Placeholder     | Description                  |
| --------------- | ---------------------------- |
| `.Name`         | 栈名称 Stack name            |
| `.Services`     | 服务数量 Number of services  |
| `.Orchestrator` | 编排器名称 Orchestrator name |
| `.Namespace`    | 命名空间 Namespace           |

When using the `--format` option, the `stack ls` command either outputs the data exactly as the template declares or, when using the `table` directive, includes column headers as well.

​	使用 `--format` 选项时，`stack ls` 命令将按模板声明的方式输出数据，或在使用 `table` 指令时包含列标题。

The following example uses a template without headers and outputs the `Name` and `Services` entries separated by a colon (`:`) for all stacks:

​	以下示例使用无标题模板，输出所有栈的 `Name` 和 `Services` 条目，以冒号 (`:`) 分隔：

```console
$ docker stack ls --format "{{.Name}}: {{.Services}}"
web-server: 1
web-cache: 4
```

To list all stacks in JSON format, use the `json` directive:

​	若要以 JSON 格式列出所有栈，请使用 `json` 指令：

```console
$ docker stack ls --format json
{"Name":"myapp","Namespace":"","Orchestrator":"Swarm","Services":"3"}
```
