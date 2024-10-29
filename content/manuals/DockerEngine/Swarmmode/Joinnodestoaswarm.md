+++
title = "将节点加入到 Swarm 集群"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/join-nodes/](https://docs.docker.com/engine/swarm/join-nodes/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Join nodes to a swarm - 将节点加入到 Swarm 集群

When you first create a swarm, you place a single Docker Engine into Swarm mode. To take full advantage of Swarm mode you can add nodes to the swarm:

​	当您首次创建 Swarm 时，您会将一个 Docker 引擎切换到 Swarm 模式。为了充分利用 Swarm 模式，您可以将节点添加到 Swarm 集群中：

- Adding worker nodes increases capacity. When you deploy a service to a swarm, the engine schedules tasks on available nodes whether they are worker nodes or manager nodes. When you add workers to your swarm, you increase the scale of the swarm to handle tasks without affecting the manager raft consensus.
  - 添加工作节点可以增加集群的容量。当您将服务部署到 Swarm 时，引擎会在可用节点上调度任务，无论它们是工作节点还是管理节点。添加工作节点后，您可以增加 Swarm 的规模来处理任务，而不会影响管理节点的 Raft 共识。

- Manager nodes increase fault-tolerance. Manager nodes perform the orchestration and cluster management functions for the swarm. Among manager nodes, a single leader node conducts orchestration tasks. If a leader node goes down, the remaining manager nodes elect a new leader and resume orchestration and maintenance of the swarm state. By default, manager nodes also run tasks.
  - 管理节点增加了容错能力。管理节点执行 Swarm 的编排和集群管理功能。在多个管理节点中，单个主节点负责编排任务。如果主节点出现故障，其余的管理节点会选举新的主节点，并继续进行编排和 Swarm 状态的维护。默认情况下，管理节点也运行任务。


Docker Engine joins the swarm depending on the **join-token** you provide to the `docker swarm join` command. The node only uses the token at join time. If you subsequently rotate the token, it doesn't affect existing swarm nodes. Refer to [Run Docker Engine in swarm mode](https://docs.docker.com/engine/swarm/swarm-mode/#view-the-join-command-or-update-a-swarm-join-token).

​	Docker 引擎根据您提供给 `docker swarm join` 命令的 **join-token** 加入 Swarm。节点仅在加入时使用该令牌。如果您随后旋转该令牌，不会影响现有的 Swarm 节点。有关详细信息，请参阅 [运行 Docker 引擎的 Swarm 模式](https://docs.docker.com/engine/swarm/swarm-mode/#view-the-join-command-or-update-a-swarm-join-token)。

## 以工作节点身份加入 Join as a worker node

To retrieve the join command including the join token for worker nodes, run the following command on a manager node:

​	要检索包含工作节点加入令牌的加入命令，请在管理节点上运行以下命令：



```console
$ docker swarm join-token worker

To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
    192.168.99.100:2377
```

Run the command from the output on the worker to join the swarm:

​	在工作节点上运行输出的命令，以加入 Swarm：



```console
$ docker swarm join \
  --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
  192.168.99.100:2377

This node joined a swarm as a worker.
```

The `docker swarm join` command does the following:

​	`docker swarm join` 命令执行以下操作：

- Switches Docker Engine on the current node into Swarm mode.
  - 将当前节点的 Docker 引擎切换到 Swarm 模式。

- Requests a TLS certificate from the manager.
  - 从管理节点请求 TLS 证书。

- Names the node with the machine hostname.
  - 使用机器主机名为节点命名。

- Joins the current node to the swarm at the manager listen address based upon the swarm token.
  - 根据 Swarm 令牌将当前节点加入到管理节点的监听地址的 Swarm 中。

- Sets the current node to `Active` availability, meaning it can receive tasks from the scheduler.
  - 将当前节点设置为 `Active` 可用性，表示它可以接收调度器的任务。

- Extends the `ingress` overlay network to the current node.
  - 将 `ingress` 覆盖网络扩展到当前节点。

## 以管理节点身份加入 Join as a manager node

When you run `docker swarm join` and pass the manager token, Docker Engine switches into Swarm mode the same as for workers. Manager nodes also participate in the raft consensus. The new nodes should be `Reachable`, but the existing manager remains the swarm `Leader`.

​	当您运行 `docker swarm join` 并传递管理节点令牌时，Docker 引擎会与工作节点相同地切换到 Swarm 模式。管理节点还参与 Raft 共识。新节点应为 `Reachable` 状态，但现有的管理节点仍然是 Swarm 的 `Leader`。

Docker recommends three or five manager nodes per cluster to implement high availability. Because Swarm-mode manager nodes share data using Raft, there must be an odd number of managers. The swarm can continue to function after as long as a quorum of more than half of the manager nodes are available.

​	Docker 建议每个集群使用三或五个管理节点，以实现高可用性。由于 Swarm 模式的管理节点使用 Raft 协议共享数据，因此管理节点数量必须为奇数。只要有超过一半的管理节点可用，Swarm 就可以继续正常工作。

For more detail about swarm managers and administering a swarm, see [Administer and maintain a swarm of Docker Engines]({{< ref "/manuals/DockerEngine/Swarmmode/AdministerandmaintainaswarmofDockerEngines" >}}).

​	有关 Swarm 管理和维护的更多详细信息，请参阅 [管理和维护 Docker 引擎的 Swarm 集群]({{< ref "/manuals/DockerEngine/Swarmmode/AdministerandmaintainaswarmofDockerEngines" >}})。

To retrieve the join command including the join token for manager nodes, run the following command on a manager node:

​	要检索包含管理节点加入令牌的加入命令，请在管理节点上运行以下命令：



```console
$ docker swarm join-token manager

To add a manager to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-61ztec5kyafptydic6jfc1i33t37flcl4nuipzcusor96k7kby-5vy9t8u35tuqm7vh67lrz9xp6 \
    192.168.99.100:2377
```

Run the command from the output on the new manager node to join it to the swarm:

​	在新的管理节点上运行输出的命令，以将其加入到 Swarm 中：



```console
$ docker swarm join \
  --token SWMTKN-1-61ztec5kyafptydic6jfc1i33t37flcl4nuipzcusor96k7kby-5vy9t8u35tuqm7vh67lrz9xp6 \
  192.168.99.100:2377

This node joined a swarm as a manager.
```

## 了解更多 Learn More

- `swarm join` [command line reference 命令行参考]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmjoin" >}})
- [Swarm mode tutorial - Swarm 模式教程]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode" >}})
