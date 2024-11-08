+++
title = "docker node ls"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/node/ls/](https://docs.docker.com/reference/cli/docker/node/ls/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker node ls

| Description | List nodes in the swarm    |
| :---------- | -------------------------- |
| Usage       | `docker node ls [OPTIONS]` |
| Aliases     | `docker node list`         |

Swarm This command works with the Swarm orchestrator.

​	Swarm 该命令适用于 Swarm 协调器。

## Description

Lists all the nodes that the Docker Swarm manager knows about. You can filter using the `-f` or `--filter` flag. Refer to the [filtering](https://docs.docker.com/reference/cli/docker/node/ls/#filter) section for more information about available filter options.

​	列出 Docker Swarm 管理器已知的所有节点。您可以使用 `-f` 或 `--filter` 标志进行过滤。有关可用过滤器选项的更多信息，请参阅[过滤](https://docs.docker.com/reference/cli/docker/node/ls/#filter)部分。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。有关管理节点和工作节点的更多信息，请参阅文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --filter`](https://docs.docker.com/reference/cli/docker/node/ls/#filter) |         | 根据提供的条件过滤输出  Filter output based on conditions provided |
| [`--format`](https://docs.docker.com/reference/cli/docker/node/ls/#format) |         | 使用自定义模板格式化输出：'table'：以表格格式显示输出（默认带列头） 'table TEMPLATE'：使用指定的 Go 模板以表格格式打印输出 'json'：以 JSON 格式打印 'TEMPLATE'：使用指定的 Go 模板打印输出。有关使用模板格式化输出的更多信息，请参阅 https://docs.docker.com/go/formatting/  Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `-q, --quiet`                                                |         | 仅显示 ID  Only display IDs                                  |

## Examples



```console
$ docker node ls

ID                           HOSTNAME        STATUS  AVAILABILITY  MANAGER STATUS
1bcef6utixb0l0ca7gxuivsj0    swarm-worker2   Ready   Active
38ciaotwjuritcdtn9npbnkuz    swarm-worker1   Ready   Active
e216jshn25ckzbvmwlnh5jr3g *  swarm-manager1  Ready   Active        Leader
```

> **Note**
>
> In the above example output, there is a hidden column of `.Self` that indicates if the node is the same node as the current docker daemon. A `*` (e.g., `e216jshn25ckzbvmwlnh5jr3g *`) means this node is the current docker daemon.
>
> ​	在上述示例输出中，有一个隐藏的 `.Self` 列，指示该节点是否与当前 Docker 守护进程相同。一个 `*`（例如，`e216jshn25ckzbvmwlnh5jr3g *`）表示该节点是当前 Docker 守护进程。

### Filtering (`--filter`)

The filtering flag (`-f` or `--filter`) format is of "key=value". If there is more than one filter, then pass multiple flags (e.g., `--filter "foo=bar" --filter "bif=baz"`)

​	过滤标志（`-f` 或 `--filter`）的格式为 "key=value"。如果有多个过滤条件，请传递多个标志（例如，`--filter "foo=bar" --filter "bif=baz"`）。

The currently supported filters are:

​	当前支持的过滤器有：

- [id](https://docs.docker.com/reference/cli/docker/node/ls/#id)
- [label](https://docs.docker.com/reference/cli/docker/node/ls/#label)
- [node.label](https://docs.docker.com/reference/cli/docker/node/ls/#nodelabel)
- [membership](https://docs.docker.com/reference/cli/docker/node/ls/#membership)
- [name](https://docs.docker.com/reference/cli/docker/node/ls/#name)
- [role](https://docs.docker.com/reference/cli/docker/node/ls/#role)

#### id

The `id` filter matches all or part of a node's id.

​	`id` 过滤器匹配节点的全部或部分 ID。

```console
$ docker node ls -f id=1

ID                         HOSTNAME       STATUS  AVAILABILITY  MANAGER STATUS
1bcef6utixb0l0ca7gxuivsj0  swarm-worker2  Ready   Active
```

#### label

The `label` filter matches nodes based on engine labels and on the presence of a `label` alone or a `label` and a value. Engine labels are configured in the [daemon configuration](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file). To filter on Swarm `node` labels, use [`node.label` instead](https://docs.docker.com/reference/cli/docker/node/ls/#nodelabel).

​	`label` 过滤器基于引擎标签和标签的存在（或标签和值）来匹配节点。引擎标签在[守护进程配置](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file)中配置。要过滤 Swarm `node` 标签，请使用 [`node.label`](https://docs.docker.com/reference/cli/docker/node/ls/#nodelabel) 代替。

The following filter matches nodes with the `foo` label regardless of its value.

​	以下过滤器匹配带有 `foo` 标签的节点，无论其值为何。

```console
$ docker node ls -f "label=foo"

ID                         HOSTNAME       STATUS  AVAILABILITY  MANAGER STATUS
1bcef6utixb0l0ca7gxuivsj0  swarm-worker2  Ready   Active
```

#### node.label

The `node.label` filter matches nodes based on node labels and on the presence of a `node.label` alone or a `node.label` and a value.

​	`node.label` 过滤器基于节点标签和标签的存在（或标签和值）来匹配节点。

The following filter updates nodes to have a `region` node label:

​	以下过滤器将节点更新为带有 `region` 标签：



```console
$ docker node update --label-add region=region-a swarm-test-01
$ docker node update --label-add region=region-a swarm-test-02
$ docker node update --label-add region=region-b swarm-test-03
$ docker node update --label-add region=region-b swarm-test-04
```

Show all nodes that have a `region` node label set:

​	显示所有带有 `region` 标签的节点：

```console
$ docker node ls --filter node.label=region

ID                            HOSTNAME        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
yg550ettvsjn6g6t840iaiwgb *   swarm-test-01   Ready     Active         Leader           23.0.3
2lm9w9kbepgvkzkkeyku40e65     swarm-test-02   Ready     Active         Reachable        23.0.3
hc0pu7ntc7s4uvj4pv7z7pz15     swarm-test-03   Ready     Active         Reachable        23.0.3
n41b2cijmhifxxvz56vwrs12q     swarm-test-04   Ready     Active                          23.0.3
```

Show all nodes that have a `region` node label, with value `region-a`:	

​	显示所有带有 `region` 标签且值为 `region-a` 的节点：

```console
$ docker node ls --filter node.label=region=region-a

ID                            HOSTNAME        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
yg550ettvsjn6g6t840iaiwgb *   swarm-test-01   Ready     Active         Leader           23.0.3
2lm9w9kbepgvkzkkeyku40e65     swarm-test-02   Ready     Active         Reachable        23.0.3
```

#### membership

The `membership` filter matches nodes based on the presence of a `membership` and a value `accepted` or `pending`.

​	`membership` 过滤器基于 `membership` 的存在和值 `accepted` 或 `pending` 匹配节点。

The following filter matches nodes with the `membership` of `accepted`.

​	以下过滤器匹配 `membership` 为 `accepted` 的节点。

```console
$ docker node ls -f "membership=accepted"

ID                           HOSTNAME        STATUS  AVAILABILITY  MANAGER STATUS
1bcef6utixb0l0ca7gxuivsj0    swarm-worker2   Ready   Active
38ciaotwjuritcdtn9npbnkuz    swarm-worker1   Ready   Active
```

#### name

The `name` filter matches on all or part of a node hostname.

​	`name` 过滤器匹配节点主机名的全部或部分。

The following filter matches the nodes with a name equal to `swarm-master` string.

​	以下过滤器匹配名称为 `swarm-manager1` 的节点。

```console
$ docker node ls -f name=swarm-manager1

ID                           HOSTNAME        STATUS  AVAILABILITY  MANAGER STATUS
e216jshn25ckzbvmwlnh5jr3g *  swarm-manager1  Ready   Active        Leader
```

#### role

The `role` filter matches nodes based on the presence of a `role` and a value `worker` or `manager`.

​	`role` 过滤器基于 `role` 的存在和 `worker` 或 `manager` 值来匹配节点。

The following filter matches nodes with the `manager` role.

​	以下过滤器匹配 `manager` 角色的节点。

```console
$ docker node ls -f "role=manager"

ID                           HOSTNAME        STATUS  AVAILABILITY  MANAGER STATUS
e216jshn25ckzbvmwlnh5jr3g *  swarm-manager1  Ready   Active        Leader
```

### Format the output (`--format`)

The formatting options (`--format`) pretty-prints nodes output using a Go template.

​	格式化选项（`--format`）使用 Go 模板美化节点输出。

Valid placeholders for the Go template are listed below:

​	Go 模板的有效占位符如下所示：

| Placeholder      | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| `.ID`            | 节点 ID Node ID                                              |
| `.Self`          | 守护进程节点 (`true/false`，`true` 表示是当前 Docker 守护进程) Node of the daemon (`true/false`, `true`indicates that the node is the same as current docker daemon) |
| `.Hostname`      | 节点主机名 Node hostname                                     |
| `.Status`        | 节点状态 Node status                                         |
| `.Availability`  | 节点可用性 ("active", "pause" 或 "drain") Node availability ("active", "pause", or "drain") |
| `.ManagerStatus` | 节点的管理器状态 Manager status of the node                  |
| `.TLSStatus`     | 节点的 TLS 状态 ("Ready" 或 "Needs Rotation") TLS status of the node ("Ready", or "Needs Rotation" has TLS certificate signed by an old CA) |
| `.EngineVersion` | 引擎版本 Engine version                                      |

When using the `--format` option, the `node ls` command will either output the data exactly as the template declares or, when using the `table` directive, includes column headers as well.

​	使用 `--format` 选项时，`node ls` 命令将按模板声明的确切数据输出，或在使用 `table` 指令时包含列标题。

The following example uses a template without headers and outputs the `ID`, `Hostname`, and `TLS Status` entries separated by a colon (`:`) for all nodes:

​	以下示例使用无标题的模板，输出所有节点的 `ID`、`Hostname` 和 `TLSStatus` 条目，用冒号（`:`）分隔：



```console
$ docker node ls --format "{{.ID}}: {{.Hostname}} {{.TLSStatus}}"

e216jshn25ckzbvmwlnh5jr3g: swarm-manager1 Ready
35o6tiywb700jesrt3dmllaza: swarm-worker1 Needs Rotation
```

To list all nodes in JSON format, use the `json` directive:

​	要以 JSON 格式列出所有节点，请使用 `json` 指令：



```console
$ docker node ls --format json
{"Availability":"Active","EngineVersion":"23.0.3","Hostname":"docker-desktop","ID":"k8f4w7qtzpj5sqzclcqafw35g","ManagerStatus":"Leader","Self":true,"Status":"Ready","TLSStatus":"Ready"}
```

