+++
title = "Swarm 模式下的 Raft 共识"
date = 2024-10-23T14:54:40+08:00
weight = 100
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/raft/](https://docs.docker.com/engine/swarm/raft/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Raft consensus in swarm mode - Swarm 模式下的 Raft 共识

When Docker Engine runs in Swarm mode, manager nodes implement the [Raft Consensus Algorithm](http://thesecretlivesofdata.com/raft/) to manage the global cluster state.

​	当 Docker 引擎以 Swarm 模式运行时，管理节点使用 [Raft 共识算法](http://thesecretlivesofdata.com/raft/) 来管理全局集群状态。

The reason why Swarm mode is using a consensus algorithm is to make sure that all the manager nodes that are in charge of managing and scheduling tasks in the cluster are storing the same consistent state.

​	Swarm 模式使用共识算法的原因是确保负责管理和调度集群任务的所有管理节点都存储相同的一致状态。

Having the same consistent state across the cluster means that in case of a failure, any Manager node can pick up the tasks and restore the services to a stable state. For example, if the Leader Manager which is responsible for scheduling tasks in the cluster dies unexpectedly, any other Manager can pick up the task of scheduling and re-balance tasks to match the desired state.

​	在集群中保持相同的一致状态意味着在发生故障时，任何管理节点都可以接管任务并将服务恢复到稳定状态。例如，如果负责调度集群任务的主管理节点意外宕机，任何其他管理节点都可以接管调度任务并重新平衡任务以匹配所需状态。

Systems using consensus algorithms to replicate logs in a distributed systems do require special care. They ensure that the cluster state stays consistent in the presence of failures by requiring a majority of nodes to agree on values.

​	在分布式系统中使用共识算法来复制日志需要特别注意。通过要求多数节点就值达成一致，它们确保集群状态在故障发生时保持一致。

Raft tolerates up to `(N-1)/2` failures and requires a majority or quorum of `(N/2)+1` members to agree on values proposed to the cluster. This means that in a cluster of 5 Managers running Raft, if 3 nodes are unavailable, the system cannot process any more requests to schedule additional tasks. The existing tasks keep running but the scheduler cannot rebalance tasks to cope with failures if the manager set is not healthy.

​	Raft 容忍最多 `(N-1)/2` 个故障，并要求 `(N/2)+1` 个成员的多数或法定人数就提议的值达成一致。这意味着在运行 Raft 的 5 个管理节点的集群中，如果有 3 个节点不可用，系统将无法处理调度额外任务的请求。现有任务会继续运行，但如果管理节点集不健康，调度程序无法重新平衡任务以应对故障。

The implementation of the consensus algorithm in Swarm mode means it features the properties inherent to distributed systems:

​	Swarm 模式中共识算法的实现意味着它具有分布式系统固有的属性：

- Agreement on values in a fault tolerant system. (Refer to [FLP impossibility theorem](https://www.the-paper-trail.org/post/2008-08-13-a-brief-tour-of-flp-impossibility/) and the [Raft Consensus Algorithm paper](https://www.usenix.org/system/files/conference/atc14/atc14-paper-ongaro.pdf))
  - 在容错系统中就值达成一致。（参见 [FLP 不可能定理](https://www.the-paper-trail.org/post/2008-08-13-a-brief-tour-of-flp-impossibility/) 和 [Raft 共识算法论文](https://www.usenix.org/system/files/conference/atc14/atc14-paper-ongaro.pdf)）

- Mutual exclusion through the leader election process
  - 通过领导者选举过程实现互斥
- Cluster membership management
  - 集群成员管理
- Globally consistent object sequencing and CAS (compare-and-swap) primitives
  - 全局一致的对象排序和 CAS（比较并交换）原语
