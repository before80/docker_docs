+++
title = "节点的工作原理"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/](https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# How nodes work - 节点的工作原理

Swarm mode lets you create a cluster of one or more Docker Engines called a swarm. A swarm consists of one or more nodes: physical or virtual machines running Docker Engine.

​	Swarm 模式允许你创建一个由一个或多个 Docker 引擎组成的集群，称为 Swarm。一个 Swarm 由一个或多个节点组成，这些节点可以是运行 Docker 引擎的物理或虚拟机。

There are two types of nodes: [managers](https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/#manager-nodes) and [workers](https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/#worker-nodes).

​	节点分为两种类型：[管理节点](https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/#manager-nodes)和[工作节点](https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/#worker-nodes)。

![Swarm mode cluster](Hownodeswork_img/swarm-diagram.webp)

If you haven't already, read through the [Swarm mode overview]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) and [key concepts]({{< ref "/manuals/DockerEngine/Swarmmode/Swarmmodekeyconcepts" >}}).

​	如果你还没有阅读，请先查看 [Swarm 模式概述]({{< ref "/manuals/DockerEngine/Swarmmode" >}})和[关键概念]({{< ref "/manuals/DockerEngine/Swarmmode/Swarmmodekeyconcepts" >}})。

## 管理节点 Manager nodes

Manager nodes handle cluster management tasks:

​	管理节点负责集群管理任务：

- Maintaining cluster state
  - 维护集群状态

- Scheduling services
  - 调度服务

- Serving Swarm mode [HTTP API endpoints]({{< ref "/reference/APIreference/DockerEngineAPI" >}})
  - 提供 Swarm 模式的 [HTTP API 端点]({{< ref "/reference/APIreference/DockerEngineAPI" >}})


Using a [Raft](https://raft.github.io/raft.pdf) implementation, the managers maintain a consistent internal state of the entire swarm and all the services running on it. For testing purposes it is OK to run a swarm with a single manager. If the manager in a single-manager swarm fails, your services continue to run, but you need to create a new cluster to recover.

​	通过使用 [Raft](https://raft.github.io/raft.pdf) 算法，管理节点能够保持整个 Swarm 及其运行服务的内部状态一致性。用于测试时，可以使用单一管理节点的 Swarm。如果单一管理节点出现故障，服务将继续运行，但需要创建一个新集群来恢复。

To take advantage of Swarm mode's fault-tolerance features, we recommend you implement an odd number of nodes according to your organization's high-availability requirements. When you have multiple managers you can recover from the failure of a manager node without downtime.

​	为充分利用 Swarm 模式的容错功能，建议根据组织的高可用性需求实施奇数数量的节点。当有多个管理节点时，即使管理节点出现故障也不会造成停机。

- A three-manager swarm tolerates a maximum loss of one manager.
  - 三个管理节点的 Swarm 最多能容忍一个管理节点的故障。

- A five-manager swarm tolerates a maximum simultaneous loss of two manager nodes.

  - 五个管理节点的 Swarm 最多能同时容忍两个管理节点的故障。

- An odd number `N` of manager nodes in the cluster tolerates the loss of at most `(N-1)/2` managers. Docker recommends a maximum of seven manager nodes for a swarm.

  - 集群中的奇数个 `N` 管理节点最多能容忍 `(N-1)/2` 个管理节点故障。Docker 推荐 Swarm 的管理节点数最多为七个。

    > **Important**
    >
    > Adding more managers does NOT mean increased scalability or higher performance. In general, the opposite is true.
    >
    > ​	增加管理节点的数量**不会**提升扩展性或性能。通常情况下，反而会降低性能。


## 工作节点 Worker nodes

Worker nodes are also instances of Docker Engine whose sole purpose is to execute containers. Worker nodes don't participate in the Raft distributed state, make scheduling decisions, or serve the swarm mode HTTP API.

​	工作节点同样是 Docker 引擎实例，其唯一任务是执行容器。工作节点不参与 Raft 分布式状态，不做调度决策，也不提供 Swarm 模式 HTTP API。

You can create a swarm of one manager node, but you cannot have a worker node without at least one manager node. By default, all managers are also workers. In a single manager node cluster, you can run commands like `docker service create` and the scheduler places all tasks on the local engine.

​	你可以创建一个包含单个管理节点的 Swarm，但不能在没有管理节点的情况下仅存在工作节点。默认情况下，所有管理节点也会作为工作节点。在单管理节点集群中，可以运行类似 `docker service create` 的命令，调度器会将所有任务放置在本地引擎上。

To prevent the scheduler from placing tasks on a manager node in a multi-node swarm, set the availability for the manager node to `Drain`. The scheduler gracefully stops tasks on nodes in `Drain` mode and schedules the tasks on an `Active` node. The scheduler does not assign new tasks to nodes with `Drain` availability.

​	在多节点 Swarm 中，若要防止调度器将任务放置在管理节点上，可以将该管理节点的可用性设置为 `Drain`。调度器会优雅地停止在 `Drain` 模式下节点上的任务，并将任务重新调度到 `Active` 节点上。调度器不会将新任务分配给可用性为 `Drain` 的节点。

Refer to the [`docker node update`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodeupdate" >}}) command line reference to see how to change node availability.

​	请参考 [`docker node update`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodeupdate" >}}) 命令行参考以查看如何更改节点的可用性。

## 更改角色 Change roles

You can promote a worker node to be a manager by running `docker node promote`. For example, you may want to promote a worker node when you take a manager node offline for maintenance. See [node promote]({{< ref "/reference/CLIreference/docker/dockernode/dockernodepromote" >}}).

​	你可以通过运行 `docker node promote` 命令将工作节点提升为管理节点。例如，当你要进行管理节点的维护时，可以将一个工作节点提升为管理节点。查看 [节点提升]({{< ref "/reference/CLIreference/docker/dockernode/dockernodepromote" >}})。

You can also demote a manager node to a worker node. See [node demote]({{< ref "/reference/CLIreference/docker/dockernode/dockernodedemote" >}}).

​	你还可以将管理节点降级为工作节点。请参阅 [节点降级]({{< ref "/reference/CLIreference/docker/dockernode/dockernodedemote" >}})。

## Learn more

- Read about how Swarm mode [services]({{< ref "/manuals/DockerEngine/Swarmmode/Howswarmworks/Howserviceswork" >}}) work.
  - 阅读 Swarm 模式中 [服务]({{< ref "/manuals/DockerEngine/Swarmmode/Howswarmworks/Howserviceswork" >}}) 的工作原理。
- Learn how [PKI]({{< ref "/manuals/DockerEngine/Swarmmode/Howswarmworks/ManageswarmsecuritywithpublickeyinfrastructurePKI" >}}) works in Swarm mode.
  - 了解 Swarm 模式中的 [PKI]({{< ref "/manuals/DockerEngine/Swarmmode/Howswarmworks/ManageswarmsecuritywithpublickeyinfrastructurePKI" >}}) 的工作方式。
