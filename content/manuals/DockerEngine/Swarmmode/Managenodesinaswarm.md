+++
title = "管理 Swarm 中的节点"
date = 2024-10-23T14:54:40+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/manage-nodes/](https://docs.docker.com/engine/swarm/manage-nodes/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Manage nodes in a swarm - 管理 Swarm 中的节点

As part of the swarm management lifecycle, you may need to:

​	在 Swarm 管理生命周期中，您可能需要：

- [List nodes in the swarm](https://docs.docker.com/engine/swarm/manage-nodes/#list-nodes)

- [Inspect an individual node](https://docs.docker.com/engine/swarm/manage-nodes/#inspect-an-individual-node)
- [Update a node](https://docs.docker.com/engine/swarm/manage-nodes/#update-a-node)
- [Leave the swarm](https://docs.docker.com/engine/swarm/manage-nodes/#leave-the-swarm)

## 列出节点 List nodes

To view a list of nodes in the swarm run `docker node ls` from a manager node:

​	在管理节点上运行 `docker node ls` 可查看 Swarm 中的节点列表：

```console
$ docker node ls

ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
46aqrk4e473hjbt745z53cr3t    node-5    Ready   Active        Reachable
61pi3d91s0w3b90ijw3deeb2q    node-4    Ready   Active        Reachable
a5b2m3oghd48m8eu391pefq5u    node-3    Ready   Active
e7p8btxeu3ioshyuj6lxiv6g0    node-2    Ready   Active
ehkv3bcimagdese79dn78otj5 *  node-1    Ready   Active        Leader
```

The `AVAILABILITY` column shows whether or not the scheduler can assign tasks to the node:

​	`AVAILABILITY` 列显示调度器是否可以将任务分配给节点：

- `Active` means that the scheduler can assign tasks to the node.
  - `Active` 表示调度器可以将任务分配给该节点。

- `Pause` means the scheduler doesn't assign new tasks to the node, but existing tasks remain running.
  - `Pause` 表示调度器不会向该节点分配新任务，但现有任务继续运行。

- `Drain` means the scheduler doesn't assign new tasks to the node. The scheduler shuts down any existing tasks and schedules them on an available node.
  - `Drain` 表示调度器不会向该节点分配新任务。调度器关闭现有任务并将其安排在可用节点上。


The `MANAGER STATUS` column shows node participation in the Raft consensus:

​	`MANAGER STATUS` 列显示节点在 Raft 共识中的参与情况：

- No value indicates a worker node that does not participate in swarm management.
  - 没有值表示该节点为工作节点，不参与 Swarm 管理。

- `Leader` means the node is the primary manager node that makes all swarm management and orchestration decisions for the swarm.
  - `Leader` 表示节点是主管理节点，负责 Swarm 的所有管理和编排决策。

- `Reachable` means the node is a manager node participating in the Raft consensus quorum. If the leader node becomes unavailable, the node is eligible for election as the new leader.
  - `Reachable` 表示节点是参与 Raft 共识的管理节点。如果主节点不可用，该节点有资格选举为新主节点。

- `Unavailable` means the node is a manager that can't communicate with other managers. If a manager node becomes unavailable, you should either join a new manager node to the swarm or promote a worker node to be a manager.
  - `Unavailable` 表示节点是无法与其他管理节点通信的管理节点。如果管理节点不可用，应加入新管理节点或将工作节点提升为管理节点。


For more information on swarm administration refer to the [Swarm administration guide]({{< ref "/manuals/DockerEngine/Swarmmode/AdministerandmaintainaswarmofDockerEngines" >}}).

​	有关 Swarm 管理的更多信息，请参阅 [Swarm 管理指南]({{< ref "/manuals/DockerEngine/Swarmmode/AdministerandmaintainaswarmofDockerEngines" >}})。

## 检查单个节点 Inspect an individual node

You can run `docker node inspect <NODE-ID>` on a manager node to view the details for an individual node. The output defaults to JSON format, but you can pass the `--pretty` flag to print the results in human-readable format. For example:

​	在管理节点上运行 `docker node inspect <NODE-ID>` 可查看单个节点的详细信息。输出默认以 JSON 格式显示，但您可以使用 `--pretty` 标志以可读格式输出。例如：



```console
$ docker node inspect self --pretty

ID:                     ehkv3bcimagdese79dn78otj5
Hostname:               node-1
Joined at:              2016-06-16 22:52:44.9910662 +0000 utc
Status:
 State:                 Ready
 Availability:          Active
Manager Status:
 Address:               172.17.0.2:2377
 Raft Status:           Reachable
 Leader:                Yes
Platform:
 Operating System:      linux
 Architecture:          x86_64
Resources:
 CPUs:                  2
 Memory:                1.954 GiB
Plugins:
  Network:              overlay, host, bridge, overlay, null
  Volume:               local
Engine Version:         1.12.0-dev
```

## 更新节点 Update a node

You can modify node attributes to:

​	您可以修改节点属性以：

- [Change node availability](https://docs.docker.com/engine/swarm/manage-nodes/#change-node-availability)

- [Add or remove label metadata](https://docs.docker.com/engine/swarm/manage-nodes/#add-or-remove-label-metadata)
- [Change a node role](https://docs.docker.com/engine/swarm/manage-nodes/#promote-or-demote-a-node)

### 更改节点的可用性 Change node availability

Changing node availability lets you:

​	更改节点可用性可以让您：

- Drain a manager node so that it only performs swarm management tasks and is unavailable for task assignment.
  - 将管理节点置为 `Drain` 状态，使其仅执行 Swarm 管理任务，不参与任务分配。

- Drain a node so you can take it down for maintenance.
  - 将节点置为 `Drain` 状态，以便维护时将其关闭。

- Pause a node so it can't receive new tasks.
  - 暂停节点，使其无法接收新任务。

- Restore unavailable or paused nodes availability status.
  - 恢复不可用或暂停的节点的可用状态。


For example, to change a manager node to `Drain` availability:

​	例如，将管理节点更改为 `Drain` 可用性：



```console
$ docker node update --availability drain node-1

node-1
```

See [list nodes](https://docs.docker.com/engine/swarm/manage-nodes/#list-nodes) for descriptions of the different availability options.

​	查看 [列出节点](https://docs.docker.com/engine/swarm/manage-nodes/#list-nodes) 了解不同的可用性选项。

### 添加或移除标签元数据 Add or remove label metadata

Node labels provide a flexible method of node organization. You can also use node labels in service constraints. Apply constraints when you create a service to limit the nodes where the scheduler assigns tasks for the service.

​	节点标签提供了一种灵活的节点组织方法。您还可以在服务约束中使用节点标签。在创建服务时应用约束，以限制调度器分配服务任务的节点。

Run `docker node update --label-add` on a manager node to add label metadata to a node. The `--label-add` flag supports either a `<key>` or a `<key>=<value>` pair.

​	在管理节点上运行 `docker node update --label-add` 为节点添加标签元数据。`--label-add` 标志支持 `<key>` 或 `<key>=<value>` 格式。

Pass the `--label-add` flag once for each node label you want to add:

​	每个要添加的标签需单独传递 `--label-add` 标志：



```console
$ docker node update --label-add foo --label-add bar=baz node-1

node-1
```

The labels you set for nodes using `docker node update` apply only to the node entity within the swarm. Do not confuse them with the Docker daemon labels for [dockerd]({{< ref "/manuals/DockerEngine/Manageresources/Dockerobjectlabels" >}}).

​	通过 `docker node update` 设置的标签仅适用于 Swarm 中的节点实体，不要将其与 [dockerd]({{< ref "/manuals/DockerEngine/Manageresources/Dockerobjectlabels" >}}) 的 Docker 守护进程标签混淆。

Therefore, node labels can be used to limit critical tasks to nodes that meet certain requirements. For example, schedule only on machines where special workloads should be run, such as machines that meet [PCI-SS compliance](https://www.pcisecuritystandards.org/).

​	因此，节点标签可以用来限制特定任务到符合要求的节点上。例如，仅在符合 [PCI-SS 合规](https://www.pcisecuritystandards.org/) 的机器上运行特殊工作负载。

A compromised worker could not compromise these special workloads because it cannot change node labels.

​	被入侵的工作节点无法更改节点标签，因此无法妥协这些特殊的工作负载。

Engine labels, however, are still useful because some features that do not affect secure orchestration of containers might be better off set in a decentralized manner. For instance, an engine could have a label to indicate that it has a certain type of disk device, which may not be relevant to security directly. These labels are more easily "trusted" by the swarm orchestrator.

​	引擎标签在某些不直接影响容器安全编排的特性中依然有用。例如，引擎可使用标签表示其拥有特定类型的磁盘设备。这些标签更易于被 Swarm 编排器“信任”。

Refer to the `docker service create` [CLI reference]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}}) for more information about service constraints.

​	有关服务约束的更多信息，请参阅 `docker service create` [CLI 参考]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}})。

### 提升或降级节点 Promote or demote a node

You can promote a worker node to the manager role. This is useful when a manager node becomes unavailable or if you want to take a manager offline for maintenance. Similarly, you can demote a manager node to the worker role.

​	您可以将工作节点提升为管理角色。这在管理节点不可用或您需要维护某个管理节点时非常有用。同样，您可以将管理节点降级为工作角色。

> **Note**
>
> 
>
> Regardless of your reason to promote or demote a node, you must always maintain a quorum of manager nodes in the swarm. For more information refer to the [Swarm administration guide]({{< ref "/manuals/DockerEngine/Swarmmode/AdministerandmaintainaswarmofDockerEngines" >}}).
>
> ​	无论提升或降级节点的原因是什么，始终需要在 Swarm 中保持管理节点的共识。有关更多信息，请参阅 [Swarm 管理指南]({{< ref "/manuals/DockerEngine/Swarmmode/AdministerandmaintainaswarmofDockerEngines" >}})。

To promote a node or set of nodes, run `docker node promote` from a manager node:

​	在管理节点上运行 `docker node promote` 以提升一个或多个节点：

```console
$ docker node promote node-3 node-2

Node node-3 promoted to a manager in the swarm.
Node node-2 promoted to a manager in the swarm.
```

To demote a node or set of nodes, run `docker node demote` from a manager node:

​	在管理节点上运行 `docker node demote` 以降级一个或多个节点：

```console
$ docker node demote node-3 node-2

Manager node-3 demoted in the swarm.
Manager node-2 demoted in the swarm.
```

`docker node promote` and `docker node demote` are convenience commands for `docker node update --role manager` and `docker node update --role worker` respectively.

​	`docker node promote` 和 `docker node demote` 是 `docker node update --role manager` 和 `docker node update --role worker` 的快捷命令。

## 在 Swarm 节点上安装插件 Install plugins on swarm nodes

If your swarm service relies on one or more [plugins]({{< ref "/manuals/DockerEngine/DockerEngineplugins/DockerPluginAPI" >}}), these plugins need to be available on every node where the service could potentially be deployed. You can manually install the plugin on each node or script the installation. You can also deploy the plugin in a similar way as a global service using the Docker API, by specifying a `PluginSpec` instead of a `ContainerSpec`.

​	如果您的 Swarm 服务依赖于一个或多个 [插件]({{< ref "/manuals/DockerEngine/DockerEngineplugins/DockerPluginAPI" >}})，这些插件需在可能部署该服务的每个节点上可用。您可以手动在每个节点上安装插件或脚本化安装。您还可以使用 Docker API，以类似全局服务的方式部署插件，指定 `PluginSpec` 而不是 `ContainerSpec`。

> **Note**
>
> 
>
> There is currently no way to deploy a plugin to a swarm using the Docker CLI or Docker Compose. In addition, it is not possible to install plugins from a private repository.
>
> ​	目前无法使用 Docker CLI 或 Docker Compose 部署插件，也无法从私有仓库安装插件。

The [`PluginSpec`](https://docs.docker.com/engine/extend/plugin_api/#json-specification) is defined by the plugin developer. To add the plugin to all Docker nodes, use the [`service/create`](https://docs.docker.com/reference/api/engine/v1.31/#operation/ServiceCreate) API, passing the `PluginSpec` JSON defined in the `TaskTemplate`.

​	[`PluginSpec`](https://docs.docker.com/engine/extend/plugin_api/#json-specification) 由插件开发者定义。要将插件添加到所有 Docker 节点，请使用 [`service/create`](https://docs.docker.com/reference/api/engine/v1.31/#operation/ServiceCreate) API，并在 `TaskTemplate` 中传递 `PluginSpec` JSON。

## 离开 Swarm Leave the swarm

Run the `docker swarm leave` command on a node to remove it from the swarm.

​	在节点上运行 `docker swarm leave` 命令以退出 Swarm。

For example to leave the swarm on a worker node:

​	例如，在工作节点上退出 Swarm：

```console
$ docker swarm leave

Node left the swarm.
```

When a node leaves the swarm, Docker Engine stops running in Swarm mode. The orchestrator no longer schedules tasks to the node.

​	当节点离开 Swarm 后，Docker 引擎停止运行 Swarm 模式。调度器不再为该节点分配任务。

If the node is a manager node, you receive a warning about maintaining the quorum. To override the warning, pass the `--force` flag. If the last manager node leaves the swarm, the swarm becomes unavailable requiring you to take disaster recovery measures.

​	如果该节点是管理节点，您会收到关于保持共识的警告。要覆盖此警告，请传递 `--force` 标志。如果最后一个管理节点离开 Swarm，Swarm 将变为不可用，需要采取灾难恢复措施。

For information about maintaining a quorum and disaster recovery, refer to the [Swarm administration guide]({{< ref "/manuals/DockerEngine/Swarmmode/AdministerandmaintainaswarmofDockerEngines" >}}).

​	有关保持共识和灾难恢复的信息，请参阅 [Swarm 管理指南]({{< ref "/manuals/DockerEngine/Swarmmode/AdministerandmaintainaswarmofDockerEngines" >}})。

After a node leaves the swarm, you can run `docker node rm` on a manager node to remove the node from the node list.

​	节点离开 Swarm 后，您可以在管理节点上运行 `docker node rm` 从节点列表中移除该节点。

For instance:

​	例如：

```console
$ docker node rm node-2
```

## 了解更多 Learn more

- [Swarm administration guide Swarm 管理指南]({{< ref "/manuals/DockerEngine/Swarmmode/AdministerandmaintainaswarmofDockerEngines" >}})
- [Docker Engine command line reference Docker 引擎命令行参考]({{< ref "/reference/CLIreference/docker" >}})
- [Swarm mode tutorial Swarm 模式教程]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode" >}})
