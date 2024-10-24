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

## Description

Lists the stacks.

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`--format`](https://docs.docker.com/reference/cli/docker/stack/ls/#format) |         | Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |

## Examples

The following command shows all stacks and some additional information:



```console
$ docker stack ls

ID                 SERVICES            ORCHESTRATOR
myapp              2                   Kubernetes
vossibility-stack  6                   Swarm
```

### Format the output (--format)

The formatting option (`--format`) pretty-prints stacks using a Go template.

Valid placeholders for the Go template are listed below:

| Placeholder     | Description        |
| --------------- | ------------------ |
| `.Name`         | Stack name         |
| `.Services`     | Number of services |
| `.Orchestrator` | Orchestrator name  |
| `.Namespace`    | Namespace          |

When using the `--format` option, the `stack ls` command either outputs the data exactly as the template declares or, when using the `table` directive, includes column headers as well.

The following example uses a template without headers and outputs the `Name` and `Services` entries separated by a colon (`:`) for all stacks:



```console
$ docker stack ls --format "{{.Name}}: {{.Services}}"
web-server: 1
web-cache: 4
```

To list all stacks in JSON format, use the `json` directive:



```console
$ docker stack ls --format json
{"Name":"myapp","Namespace":"","Orchestrator":"Swarm","Services":"3"}
```
