+++
title = "Swarm 模式关键概念"
date = 2024-10-23T14:54:40+08:00
weight = 130
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/key-concepts/](https://docs.docker.com/engine/swarm/key-concepts/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Swarm mode key concepts - Swarm 模式关键概念

This topic introduces some of the concepts unique to the cluster management and orchestration features of Docker Engine 1.12.

​	本主题介绍了 Docker Engine 1.12 中集群管理和编排功能的独特概念。

## 什么是 Swarm？ What is a swarm?

The cluster management and orchestration features embedded in Docker Engine are built using [swarmkit](https://github.com/docker/swarmkit/). Swarmkit is a separate project which implements Docker's orchestration layer and is used directly within Docker.

​	Docker Engine 中嵌入的集群管理和编排功能是基于 [swarmkit](https://github.com/docker/swarmkit/) 构建的。Swarmkit 是一个实现 Docker 编排层的独立项目，直接用于 Docker 中。

A swarm consists of multiple Docker hosts which run in Swarm mode and act as managers, to manage membership and delegation, and workers, which run [swarm services](https://docs.docker.com/engine/swarm/key-concepts/#services-and-tasks). A given Docker host can be a manager, a worker, or perform both roles. When you create a service, you define its optimal state - number of replicas, network and storage resources available to it, ports the service exposes to the outside world, and more. Docker works to maintain that desired state. For instance, if a worker node becomes unavailable, Docker schedules that node's tasks on other nodes. A task is a running container which is part of a swarm service and is managed by a swarm manager, as opposed to a standalone container.

​	Swarm 由多个运行在 Swarm 模式下的 Docker 主机构成，这些主机可作为管理节点，负责管理成员和任务分配；也可作为工作节点，运行 [swarm 服务](https://docs.docker.com/engine/swarm/key-concepts/#services-and-tasks)。一个 Docker 主机可以是管理节点、工作节点，或同时执行两者角色。当您创建服务时，可以定义其最佳状态（例如副本数、可用的网络和存储资源、服务向外部暴露的端口等）。Docker 会努力维护该理想状态。例如，如果某个工作节点不可用，Docker 会将该节点的任务重新调度到其他节点上。任务是一个运行中的容器，它是 Swarm 服务的一部分，由 Swarm 管理节点管理，而不是独立的容器。

One of the key advantages of swarm services over standalone containers is that you can modify a service's configuration, including the networks and volumes it is connected to, without the need to manually restart the service. Docker will update the configuration, stop the service tasks with out of date configuration, and create new ones matching the desired configuration.

​	Swarm 服务相比独立容器的一个关键优势是，您可以在无需手动重启服务的情况下，修改服务的配置，包括其连接的网络和卷。Docker 会更新配置，停止配置过期的服务任务，并创建符合所需配置的新任务。

When Docker is running in Swarm mode, you can still run standalone containers on any of the Docker hosts participating in the swarm, as well as swarm services. A key difference between standalone containers and swarm services is that only swarm managers can manage a swarm, while standalone containers can be started on any daemon. Docker daemons can participate in a swarm as managers, workers, or both.

​	当 Docker 在 Swarm 模式下运行时，您仍可以在 Swarm 中的任一 Docker 主机上运行独立容器或 Swarm 服务。独立容器与 Swarm 服务的关键区别在于，只有 Swarm 管理节点可以管理 Swarm，而独立容器可以在任一守护进程上启动。Docker 守护进程可以作为管理节点、工作节点或两者一起加入 Swarm。

In the same way that you can use [Docker Compose]({{< ref "/manuals/DockerCompose" >}}) to define and run containers, you can define and run [Swarm service]({{< ref "/manuals/DockerEngine/Swarmmode/Deployservicestoaswarm" >}}) stacks.

​	正如您可以使用 [Docker Compose]({{< ref "/manuals/DockerCompose" >}}) 来定义和运行容器，您也可以定义和运行 [Swarm 服务]({{< ref "/manuals/DockerEngine/Swarmmode/Deployservicestoaswarm" >}})堆栈。

Keep reading for details about concepts related to Docker swarm services, including nodes, services, tasks, and load balancing.

​	继续阅读以了解有关 Docker Swarm 服务的概念，包括节点、服务、任务和负载均衡的详细信息。

## 节点 Nodes

A node is an instance of the Docker engine participating in the swarm. You can also think of this as a Docker node. You can run one or more nodes on a single physical computer or cloud server, but production swarm deployments typically include Docker nodes distributed across multiple physical and cloud machines.

​	节点是参与 Swarm 的 Docker 引擎实例，您也可以将其视为 Docker 节点。您可以在单个物理计算机或云服务器上运行一个或多个节点，但生产环境的 Swarm 部署通常包括分布在多台物理和云机器上的 Docker 节点。

To deploy your application to a swarm, you submit a service definition to a manager node. The manager node dispatches units of work called [tasks](https://docs.docker.com/engine/swarm/key-concepts/#services-and-tasks) to worker nodes.

​	要将应用部署到 Swarm，您需要向管理节点提交服务定义。管理节点会将工作单元 [任务](https://docs.docker.com/engine/swarm/key-concepts/#services-and-tasks) 分派到工作节点。

Manager nodes also perform the orchestration and cluster management functions required to maintain the desired state of the swarm. Manager nodes select a single leader to conduct orchestration tasks.

​	管理节点还执行编排和集群管理功能，以维持 Swarm 的理想状态。管理节点会选择一个领导者来执行编排任务。

Worker nodes receive and execute tasks dispatched from manager nodes. By default manager nodes also run services as worker nodes, but you can configure them to run manager tasks exclusively and be manager-only nodes. An agent runs on each worker node and reports on the tasks assigned to it. The worker node notifies the manager node of the current state of its assigned tasks so that the manager can maintain the desired state of each worker.

​	工作节点接收并执行管理节点分派的任务。默认情况下，管理节点也作为工作节点运行服务，但您可以配置它们仅运行管理任务，成为仅管理节点。每个工作节点上都运行一个代理，报告分配给它的任务。工作节点将其分配任务的当前状态通知管理节点，以便管理节点维持每个工作节点的理想状态。

## 服务和任务 Services and tasks

A service is the definition of the tasks to execute on the manager or worker nodes. It is the central structure of the swarm system and the primary root of user interaction with the swarm.

​	服务是定义要在管理节点或工作节点上执行的任务的结构。它是 Swarm 系统的核心结构，也是用户与 Swarm 交互的主要接口。

When you create a service, you specify which container image to use and which commands to execute inside running containers.

​	创建服务时，您需要指定要使用的容器镜像以及在容器中运行的命令。

In the replicated services model, the swarm manager distributes a specific number of replica tasks among the nodes based upon the scale you set in the desired state.

​	在复制服务模型中，Swarm 管理器会根据您设定的理想状态在节点之间分配特定数量的副本任务。

For global services, the swarm runs one task for the service on every available node in the cluster.

​	对于全局服务，Swarm 会在集群中每个可用节点上为服务运行一个任务。

A task carries a Docker container and the commands to run inside the container. It is the atomic scheduling unit of swarm. Manager nodes assign tasks to worker nodes according to the number of replicas set in the service scale. Once a task is assigned to a node, it cannot move to another node. It can only run on the assigned node or fail.

​	任务携带一个 Docker 容器和在容器中运行的命令，是 Swarm 的基本调度单元。管理节点根据服务规模中的副本数量将任务分配给工作节点。任务一旦分配到某节点，就无法移动到其他节点。它只能在分配的节点上运行或失败。

## 负载均衡 Load balancing

The swarm manager uses ingress load balancing to expose the services you want to make available externally to the swarm. The swarm manager can automatically assign the service a published port or you can configure a published port for the service. You can specify any unused port. If you do not specify a port, the swarm manager assigns the service a port in the 30000-32767 range.

​	Swarm 管理器使用入口负载均衡将您希望公开的服务暴露给 Swarm。Swarm 管理器可以自动为服务分配发布端口，或者您可以为服务配置一个发布端口。您可以指定任何未使用的端口。如果未指定端口，Swarm 管理器会在 30000-32767 范围内为服务分配一个端口。

External components, such as cloud load balancers, can access the service on the published port of any node in the cluster whether or not the node is currently running the task for the service. All nodes in the swarm route ingress connections to a running task instance.

​	外部组件（例如云负载均衡器）可以通过集群中任意节点的发布端口访问服务，无论该节点当前是否运行该服务的任务。Swarm 中的所有节点会将入口连接路由到正在运行的任务实例。

Swarm mode has an internal DNS component that automatically assigns each service in the swarm a DNS entry. The swarm manager uses internal load balancing to distribute requests among services within the cluster based upon the DNS name of the service.

​	Swarm 模式中有一个内部 DNS 组件，会自动为 Swarm 中的每个服务分配一个 DNS 条目。Swarm 管理器使用内部负载均衡根据服务的 DNS 名称在集群内分配请求。

## 接下来 What's next?

- Read the [Swarm mode overview]({{< ref "/manuals/DockerEngine/Swarmmode" >}}).
  - 阅读 [Swarm 模式概述]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。
- Get started with the [Swarm mode tutorial]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode" >}}).
  - 从 [Swarm 模式教程]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode" >}}) 开始。
