+++
title = "docker node rm"
date = 2024-10-23T14:54:43+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/node/rm/](https://docs.docker.com/reference/cli/docker/node/rm/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker node rm

| Description | Remove one or more nodes from the swarm   |
| :---------- | ----------------------------------------- |
| Usage       | `docker node rm [OPTIONS] NODE [NODE...]` |
| Aliases     | `docker node remove`                      |

Swarm This command works with the Swarm orchestrator.

​	Swarm 该命令适用于 Swarm 协调器。

## Description

Removes the specified nodes from a swarm.

​	将指定节点从 Swarm 中移除。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。有关管理节点和工作节点的更多信息，请参阅文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Options

| Option                                                       | Default | Description                                                |
| ------------------------------------------------------------ | ------- | ---------------------------------------------------------- |
| [`-f, --force`](https://docs.docker.com/reference/cli/docker/node/rm/#force) |         | 强制从 Swarm 中移除节点 Force remove a node from the swarm |

## Examples

### 从 Swarm 中移除一个已停止的节点 Remove a stopped node from the swarm



```console
$ docker node rm swarm-node-02

Node swarm-node-02 removed from swarm
```

### 尝试从 Swarm 中移除一个运行中的节点 Attempt to remove a running node from a swarm

Removes the specified nodes from the swarm, but only if the nodes are in the down state. If you attempt to remove an active node you will receive an error:

​	仅当节点处于关闭状态时才能将其从 Swarm 中移除。如果尝试移除一个活动的节点，会收到错误提示：

```console
$ docker node rm swarm-node-03

Error response from daemon: rpc error: code = 9 desc = node swarm-node-03 is not
down and can't be removed
```

### 强制从 Swarm 中移除一个不可访问的节点（`--force`） Forcibly remove an inaccessible node from a swarm (`--force`)

If you lose access to a worker node or need to shut it down because it has been compromised or is not behaving as expected, you can use the `--force` option. This may cause transient errors or interruptions, depending on the type of task being run on the node.

​	如果失去对某个工作节点的访问权限或需要关闭该节点，因为它已被入侵或未按预期运行，可以使用 `--force` 选项。这可能会导致瞬态错误或中断，具体取决于节点上正在运行的任务类型。



```console
$ docker node rm --force swarm-node-03

Node swarm-node-03 removed from swarm
```

A manager node must be demoted to a worker node (using `docker node demote`) before you can remove it from the swarm.

​	必须先将管理节点降级为工作节点（使用 `docker node demote`），然后才能将其从 Swarm 中移除。
