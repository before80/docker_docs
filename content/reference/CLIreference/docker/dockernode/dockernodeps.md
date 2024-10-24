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

## Description

Lists all the tasks on a Node that Docker knows about. You can filter using the `-f` or `--filter` flag. Refer to the [filtering](https://docs.docker.com/reference/cli/docker/node/ps/#filter) section for more information about available filter options.

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.

## Options

| Option                                                       | Default | Description                                |
| ------------------------------------------------------------ | ------- | ------------------------------------------ |
| [`-f, --filter`](https://docs.docker.com/reference/cli/docker/node/ps/#filter) |         | Filter output based on conditions provided |
| [`--format`](https://docs.docker.com/reference/cli/docker/node/ps/#format) |         | Pretty-print tasks using a Go template     |
| `--no-resolve`                                               |         | Do not map IDs to Names                    |
| `--no-trunc`                                                 |         | Do not truncate output                     |
| `-q, --quiet`                                                |         | Only display task IDs                      |

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

### Filtering (--filter)

The filtering flag (`-f` or `--filter`) format is of "key=value". If there is more than one filter, then pass multiple flags (e.g., `--filter "foo=bar" --filter "bif=baz"`).

The currently supported filters are:

- [name](https://docs.docker.com/reference/cli/docker/node/ps/#name)
- [id](https://docs.docker.com/reference/cli/docker/node/ps/#id)
- [label](https://docs.docker.com/reference/cli/docker/node/ps/#label)
- [desired-state](https://docs.docker.com/reference/cli/docker/node/ps/#desired-state)

#### name

The `name` filter matches on all or part of a task's name.

The following filter matches all tasks with a name containing the `redis` string.



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



```console
$ docker node ps -f id=bg8c07zzg87di2mufeq51a2qp swarm-manager1

NAME                                IMAGE        NODE            DESIRED STATE  CURRENT STATE
redis.7.bg8c07zzg87di2mufeq51a2qp   redis:3.0.6  swarm-manager1  Running        Running 5 seconds
```

#### label

The `label` filter matches tasks based on the presence of a `label` alone or a `label` and a value.

The following filter matches tasks with the `usage` label regardless of its value.



```console
$ docker node ps -f "label=usage"

NAME                               IMAGE        NODE            DESIRED STATE  CURRENT STATE
redis.6.b465edgho06e318egmgjbqo4o  redis:3.0.6  swarm-manager1  Running        Running 10 minutes
redis.7.bg8c07zzg87di2mufeq51a2qp  redis:3.0.6  swarm-manager1  Running        Running 9 minutes
```

#### desired-state

The `desired-state` filter can take the values `running`, `shutdown`, or `accepted`.

### Format the output (--format)

The formatting options (`--format`) pretty-prints tasks output using a Go template.

Valid placeholders for the Go template are listed below:

| Placeholder     | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| `.ID`           | Task ID                                                      |
| `.Name`         | Task name                                                    |
| `.Image`        | Task image                                                   |
| `.Node`         | Node ID                                                      |
| `.DesiredState` | Desired state of the task (`running`, `shutdown`, or `accepted`) |
| `.CurrentState` | Current state of the task                                    |
| `.Error`        | Error                                                        |
| `.Ports`        | Task published ports                                         |

When using the `--format` option, the `node ps` command will either output the data exactly as the template declares or, when using the `table` directive, includes column headers as well.

The following example uses a template without headers and outputs the `Name` and `Image` entries separated by a colon (`:`) for all tasks:



```console
$ docker node ps --format "{{.Name}}: {{.Image}}"

top.1: busybox
top.2: busybox
top.3: busybox
```
