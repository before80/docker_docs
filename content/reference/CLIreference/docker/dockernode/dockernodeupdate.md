+++
title = "docker node update"
date = 2024-10-23T14:54:43+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/node/update/](https://docs.docker.com/reference/cli/docker/node/update/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker node update

| Description | Update a node                       |
| :---------- | ----------------------------------- |
| Usage       | `docker node update [OPTIONS] NODE` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 该命令适用于 Swarm 协调器。

## Description

Update metadata about a node, such as its availability, labels, or roles.

​	更新节点的元数据，例如其可用性、标签或角色。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。有关管理节点和工作节点的更多信息，请参阅文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| `--availability`                                             |         | 节点的可用性（`active`, `pause`, `drain`） Availability of the node (`active`, `pause`, `drain`) |
| [`--label-add`](https://docs.docker.com/reference/cli/docker/node/update/#label-add) |         | 添加或更新节点标签（`key=value`） Add or update a node label (`key=value`) |
| `--label-rm`                                                 |         | 移除节点标签（如果存在） Remove a node label if exists       |
| `--role`                                                     |         | 节点角色（`worker`, `manager`） Role of the node (`worker`, `manager`) |

## Examples

### 为节点添加标签元数据（`--label-add`） Add label metadata to a node (`--label-add`)

Add metadata to a swarm node using node labels. You can specify a node label as a key with an empty value:

​	使用节点标签为 Swarm 节点添加元数据。可以将节点标签指定为带有空值的键：



```bash
$ docker node update --label-add foo worker1
```

To add multiple labels to a node, pass the `--label-add` flag for each label:

​	要向节点添加多个标签，请为每个标签传递 `--label-add` 标志：



```console
$ docker node update --label-add foo --label-add bar worker1
```

When you [create a service]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}}), you can use node labels as a constraint. A constraint limits the nodes where the scheduler deploys tasks for a service.

​	在[创建服务]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}})时，可以使用节点标签作为约束条件。约束限制了调度器将服务任务部署的节点位置。

For example, to add a `type` label to identify nodes where the scheduler should deploy message queue service tasks:

​	例如，添加 `type` 标签以识别调度器应在其中部署消息队列服务任务的节点：



```bash
$ docker node update --label-add type=queue worker1
```

The labels you set for nodes using `docker node update` apply only to the node entity within the swarm. Do not confuse them with the docker daemon labels for [dockerd]({{< ref "/reference/CLIreference/dockerd" >}}).

​	使用 `docker node update` 设置的节点标签仅适用于 Swarm 中的节点实体。请勿将其与 [dockerd]({{< ref "/reference/CLIreference/dockerd" >}}) 的 Docker 守护进程标签混淆。

For more information about labels, refer to [apply custom metadata](https://docs.docker.com/engine/userguide/labels-custom-metadata/).

​	有关标签的更多信息，请参考[应用自定义元数据](https://docs.docker.com/engine/userguide/labels-custom-metadata/)。
