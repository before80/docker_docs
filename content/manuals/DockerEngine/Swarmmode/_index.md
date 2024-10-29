+++
title = "Swarm 模式"
date = 2024-10-23T14:54:40+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/](https://docs.docker.com/engine/swarm/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Swarm mode - Swarm 模式

> **Note**
>
> 
>
> Swarm mode is an advanced feature for managing a cluster of Docker daemons.
>
> ​	Swarm 模式是用于管理 Docker 守护进程集群的高级功能。
>
> Use Swarm mode if you intend to use Swarm as a production runtime environment.
>
> ​	如果打算将 Swarm 用作生产环境，请使用 Swarm 模式。
>
> If you're not planning on deploying with Swarm, use [Docker Compose]({{< ref "/manuals/DockerCompose" >}}) instead. If you're developing for a Kubernetes deployment, consider using the [integrated Kubernetes feature]({{< ref "/manuals/DockerDesktop/DeployonKuberneteswithDockerDesktop" >}}) in Docker Desktop.
>
> ​	如果不打算使用 Swarm 部署，请改用 [Docker Compose]({{< ref "/manuals/DockerCompose" >}})。如果要为 Kubernetes 部署开发应用，可以考虑使用 Docker Desktop 中的[集成 Kubernetes 功能]({{< ref "/manuals/DockerDesktop/DeployonKuberneteswithDockerDesktop" >}})。

Current versions of Docker include Swarm mode for natively managing a cluster of Docker Engines called a swarm. Use the Docker CLI to create a swarm, deploy application services to a swarm, and manage swarm behavior.

​	当前版本的 Docker 包含 Swarm 模式，可原生管理称为 Swarm 的 Docker 引擎集群。使用 Docker CLI 创建一个 Swarm，将应用服务部署到 Swarm，并管理 Swarm 的行为。

Docker Swarm mode is built into the Docker Engine. Do not confuse Docker Swarm mode with [Docker Classic Swarm](https://github.com/docker/classicswarm) which is no longer actively developed.

​	Docker Swarm 模式内置于 Docker 引擎中。不要将 Docker Swarm 模式与 [Docker Classic Swarm](https://github.com/docker/classicswarm) 混淆，后者已不再积极开发。

## 功能亮点 Feature highlights

### 与 Docker 引擎集成的集群管理 Cluster management integrated with Docker Engine

Use the Docker Engine CLI to create a swarm of Docker Engines where you can deploy application services. You don't need additional orchestration software to create or manage a swarm.

​	使用 Docker 引擎 CLI 创建 Docker 引擎集群，可在其中部署应用服务。无需额外的编排软件即可创建和管理 Swarm。

### 分散式设计 Decentralized design

Instead of handling differentiation between node roles at deployment time, the Docker Engine handles any specialization at runtime. You can deploy both kinds of nodes, managers and workers, using the Docker Engine. This means you can build an entire swarm from a single disk image.

​	Docker 引擎在运行时处理节点角色的区分，而不是在部署时。您可以使用 Docker 引擎部署管理和工作节点，这意味着可以从单个磁盘镜像构建整个 Swarm。

### 声明式服务模型 Declarative service model

Docker Engine uses a declarative approach to let you define the desired state of the various services in your application stack. For example, you might describe an application comprised of a web front end service with message queueing services and a database backend.

​	Docker 引擎使用声明式方法，让您定义应用栈中各种服务的期望状态。例如，您可以描述一个包含 Web 前端服务、消息队列服务和数据库后端的应用程序。

### 伸缩 Scaling

For each service, you can declare the number of tasks you want to run. When you scale up or down, the swarm manager automatically adapts by adding or removing tasks to maintain the desired state.

​	对于每个服务，您可以声明希望运行的任务数量。增减任务时，Swarm 管理器会自动通过添加或删除任务来保持期望的状态。

### 期望状态协调 Desired state reconciliation

The swarm manager node constantly monitors the cluster state and reconciles any differences between the actual state and your expressed desired state. For example, if you set up a service to run 10 replicas of a container, and a worker machine hosting two of those replicas crashes, the manager creates two new replicas to replace the replicas that crashed. The swarm manager assigns the new replicas to workers that are running and available.

​	Swarm 管理器节点不断监控集群状态，并协调实际状态与期望状态之间的差异。例如，如果设置某个服务运行 10 个容器副本，而托管其中两个副本的工作节点崩溃，管理器会创建两个新副本替换崩溃的副本，并将这些新副本分配给正在运行的工作节点。

### 多主机网络 Multi-host networking

You can specify an overlay network for your services. The swarm manager automatically assigns addresses to the containers on the overlay network when it initializes or updates the application.

​	可以为服务指定覆盖网络。Swarm 管理器在初始化或更新应用时自动为覆盖网络中的容器分配地址。

### 服务发现 Service discovery

Swarm manager nodes assign each service in the swarm a unique DNS name and load balance running containers. You can query every container running in the swarm through a DNS server embedded in the swarm.

​	Swarm 管理节点为 Swarm 中的每个服务分配一个唯一的 DNS 名称，并在运行中的容器间进行负载均衡。可以通过 Swarm 内嵌的 DNS 服务器查询在 Swarm 中运行的每个容器。

### 负载均衡 Load balancing

You can expose the ports for services to an external load balancer. Internally, the swarm lets you specify how to distribute service containers between nodes.

​	可以将服务端口暴露给外部负载均衡器。内部，Swarm 允许您指定如何在节点之间分配服务容器。

### 默认安全 Secure by default

Each node in the swarm enforces TLS mutual authentication and encryption to secure communications between itself and all other nodes. You have the option to use self-signed root certificates or certificates from a custom root CA.

​	Swarm 中的每个节点强制执行 TLS 双向认证和加密，以确保与其他节点之间的通信安全。您可以选择使用自签名根证书或来自自定义根 CA 的证书。

### 滚动更新 Rolling updates

At rollout time you can apply service updates to nodes incrementally. The swarm manager lets you control the delay between service deployment to different sets of nodes. If anything goes wrong, you can roll back to a previous version of the service.

​	在部署时，可以将服务更新逐步应用到节点。Swarm 管理器允许您控制服务在不同节点间的部署延迟。如果出现问题，可以回滚到服务的先前版本。

## 接下来 What's next?

- Learn Swarm mode [key concepts]({{< ref "/manuals/DockerEngine/Swarmmode/Swarmmodekeyconcepts" >}}).
  - 了解 Swarm 模式的[关键概念]({{< ref "/manuals/DockerEngine/Swarmmode/Swarmmodekeyconcepts" >}})。
- Get started with the [Swarm mode tutorial]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode" >}}).
  - 开始 [Swarm 模式教程]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode" >}})。
- Explore Swarm mode CLI commands 探索 Swarm 模式的 CLI 命令：
  - [swarm init]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarminit" >}})
  - [swarm join]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmjoin" >}})
  - [service create]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}})
  - [service inspect]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceinspect" >}})
  - [service ls]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicels" >}})
  - [service rm]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicerm" >}})
  - [service scale]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicescale" >}})
  - [service ps]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceps" >}})
  - [service update]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceupdate" >}})
