+++
title = "管理和维护 Docker 引擎群集"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/admin_guide/](https://docs.docker.com/engine/swarm/admin_guide/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Administer and maintain a swarm of Docker Engines - 管理和维护 Docker 引擎群集

When you run a swarm of Docker Engines, manager nodes are the key components for managing the swarm and storing the swarm state. It is important to understand some key features of manager nodes to properly deploy and maintain the swarm.

​	当您运行 Docker 引擎的群集时，管理节点是管理群集和存储群集状态的关键组件。了解管理节点的一些关键特性对于正确部署和维护群集非常重要。

Refer to [How nodes work]({{< ref "/manuals/DockerEngine/Swarmmode/Howswarmworks/Hownodeswork" >}}) for a brief overview of Docker Swarm mode and the difference between manager and worker nodes.

​	请参考[节点如何工作]({{< ref "/manuals/DockerEngine/Swarmmode/Howswarmworks/Hownodeswork" >}})了解 Docker 群集模式和管理节点与工作节点的区别。

## 操作群集中的管理节点 Operate manager nodes in a swarm

Swarm manager nodes use the [Raft Consensus Algorithm]({{< ref "/manuals/DockerEngine/Swarmmode/Raftconsensusinswarmmode" >}}) to manage the swarm state. You only need to understand some general concepts of Raft in order to manage a swarm.

​	群集的管理节点使用[Raft 一致性算法]({{< ref "/manuals/DockerEngine/Swarmmode/Raftconsensusinswarmmode" >}})来管理群集状态。您只需了解 Raft 算法的一些基本概念即可管理群集。

There is no limit on the number of manager nodes. The decision about how many manager nodes to implement is a trade-off between performance and fault-tolerance. Adding manager nodes to a swarm makes the swarm more fault-tolerant. However, additional manager nodes reduce write performance because more nodes must acknowledge proposals to update the swarm state. This means more network round-trip traffic.

​	管理节点数量没有限制。决定实施多少管理节点是在性能和容错性之间的权衡。增加管理节点可以提高群集的容错性，但额外的管理节点会降低写性能，因为更多节点需要确认群集状态的更新。也就是说，需要更多的网络往返流量。

Raft requires a majority of managers, also called the quorum, to agree on proposed updates to the swarm, such as node additions or removals. Membership operations are subject to the same constraints as state replication.

​	Raft 要求多数管理节点（称为法定人数）同意群集的更新，例如添加或移除节点。成员操作也受状态复制的相同限制。

### 维护管理节点的法定人数 Maintain the quorum of managers

If the swarm loses the quorum of managers, the swarm cannot perform management tasks. If your swarm has multiple managers, always have more than two. To maintain quorum, a majority of managers must be available. An odd number of managers is recommended, because the next even number does not make the quorum easier to keep. For instance, whether you have 3 or 4 managers, you can still only lose 1 manager and maintain the quorum. If you have 5 or 6 managers, you can still only lose two.

​	如果群集失去管理节点的法定人数，则无法执行管理任务。对于具有多个管理节点的群集，建议始终保持两个以上管理节点。要维持法定人数，必须有多数管理节点可用。推荐使用奇数个管理节点，因为偶数个节点不会更容易维持法定人数。例如，不管是 3 个还是 4 个管理节点，只要丢失一个节点，法定人数就会受到影响。如果有 5 个或 6 个管理节点，则可以容忍丢失两个节点。

Even if a swarm loses the quorum of managers, swarm tasks on existing worker nodes continue to run. However, swarm nodes cannot be added, updated, or removed, and new or existing tasks cannot be started, stopped, moved, or updated.

​	即使群集失去管理节点的法定人数，现有的工作节点上的任务仍然会继续运行。但是，无法添加、更新或删除群集节点，新的或现有的任务也无法启动、停止、移动或更新。

See [Recovering from losing the quorum](https://docs.docker.com/engine/swarm/admin_guide/#recover-from-losing-the-quorum) for troubleshooting steps if you do lose the quorum of managers.

​	参见[恢复丢失法定人数](https://docs.docker.com/engine/swarm/admin_guide/#recover-from-losing-the-quorum)以获取丢失法定人数的故障排除步骤。

## 配置管理节点以在静态 IP 地址上广播 Configure the manager to advertise on a static IP address

When initiating a swarm, you must specify the `--advertise-addr` flag to advertise your address to other manager nodes in the swarm. For more information, see [Run Docker Engine in swarm mode](https://docs.docker.com/engine/swarm/swarm-mode/#configure-the-advertise-address). Because manager nodes are meant to be a stable component of the infrastructure, you should use a *fixed IP address* for the advertise address to prevent the swarm from becoming unstable on machine reboot.

​	在初始化群集时，您必须指定 `--advertise-addr` 标志，以便将地址广播给群集中的其他管理节点。有关详细信息，请参阅[在群集模式下运行 Docker 引擎](https://docs.docker.com/engine/swarm/swarm-mode/#configure-the-advertise-address)。由于管理节点是基础设施中的稳定组件，因此应为广播地址使用*固定 IP 地址*，以防止群集在机器重启时不稳定。

If the whole swarm restarts and every manager node subsequently gets a new IP address, there is no way for any node to contact an existing manager. Therefore the swarm is hung while nodes try to contact one another at their old IP addresses.

​	如果整个群集重启，每个管理节点获取一个新的 IP 地址，那么没有任何节点可以联系到现有的管理节点，导致群集挂起，节点尝试通过旧的 IP 地址互相连接。

Dynamic IP addresses are OK for worker nodes.

​	动态 IP 地址对于工作节点是可以接受的。

## 为容错添加管理节点Add manager nodes for fault tolerance

You should maintain an odd number of managers in the swarm to support manager node failures. Having an odd number of managers ensures that during a network partition, there is a higher chance that the quorum remains available to process requests if the network is partitioned into two sets. Keeping the quorum is not guaranteed if you encounter more than two network partitions.

​	您应在群集中保持奇数个管理节点，以支持管理节点故障。保持奇数个管理节点可确保在网络分区时，法定人数仍有更高的可能性能够继续处理请求。如果网络分区超过两个部分，则无法保证法定人数的可用性。

| Swarm Size 群集大小 | Majority | Fault Tolerance 容错能力 |
| :-----------------: | :------: | :----------------------: |
|          1          |    1     |            0             |
|          2          |    2     |            0             |
|        **3**        |    2     |          **1**           |
|          4          |    3     |            1             |
|        **5**        |    3     |          **2**           |
|          6          |    4     |            2             |
|        **7**        |    4     |          **3**           |
|          8          |    5     |            3             |
|        **9**        |    5     |          **4**           |

For example, in a swarm with *5 nodes*, if you lose *3 nodes*, you don't have a quorum. Therefore you can't add or remove nodes until you recover one of the unavailable manager nodes or recover the swarm with disaster recovery commands. See [Recover from disaster](https://docs.docker.com/engine/swarm/admin_guide/#recover-from-disaster).

​	例如，在一个包含*5 个节点*的群集中，如果丢失了*3 个节点*，则无法维持法定人数。因此，直到恢复其中一个不可用的管理节点或使用灾难恢复命令恢复群集之前，您无法添加或删除节点。参见[从灾难中恢复](https://docs.docker.com/engine/swarm/admin_guide/#recover-from-disaster)。

While it is possible to scale a swarm down to a single manager node, it is impossible to demote the last manager node. This ensures you maintain access to the swarm and that the swarm can still process requests. Scaling down to a single manager is an unsafe operation and is not recommended. If the last node leaves the swarm unexpectedly during the demote operation, the swarm becomes unavailable until you reboot the node or restart with `--force-new-cluster`.

​	虽然可以将群集缩小到仅一个管理节点，但无法降级最后一个管理节点。这确保了您仍然可以访问群集，并且群集能够处理请求。将群集缩小到单个管理节点是不安全的操作，不推荐这样做。如果在降级操作期间最后一个节点意外离开群集，群集将变得不可用，直到重新启动该节点或使用 `--force-new-cluster` 重新启动。

You manage swarm membership with the `docker swarm` and `docker node` subsystems. Refer to [Add nodes to a swarm]({{< ref "/manuals/DockerEngine/Swarmmode/Joinnodestoaswarm" >}}) for more information on how to add worker nodes and promote a worker node to be a manager.

​	您可以使用 `docker swarm` 和 `docker node` 子系统来管理群集成员。有关如何添加工作节点并将工作节点提升为管理节点的更多信息，请参考[将节点添加到群集]({{< ref "/manuals/DockerEngine/Swarmmode/Joinnodestoaswarm" >}})。

### 分布管理节点 Distribute manager nodes

In addition to maintaining an odd number of manager nodes, pay attention to datacenter topology when placing managers. For optimal fault-tolerance, distribute manager nodes across a minimum of 3 availability-zones to support failures of an entire set of machines or common maintenance scenarios. If you suffer a failure in any of those zones, the swarm should maintain the quorum of manager nodes available to process requests and rebalance workloads.

​	除了保持奇数个管理节点，还应注意在数据中心拓扑中放置管理节点。为了获得最佳的容错性，建议将管理节点分布在至少 3 个可用区，以支持整个机器集的故障或常见维护场景。如果任何一个区域出现故障，群集应能够保持管理节点的法定人数，从而继续处理请求并重新平衡工作负载。

| Swarm manager nodes 群集管理节点数 | Repartition (on 3 Availability zones) 重分区（在 3 个可用区） |
| :--------------------------------: | :----------------------------------------------------------: |
|                 3                  |                            1-1-1                             |
|                 5                  |                            2-2-1                             |
|                 7                  |                            3-2-2                             |
|                 9                  |                            3-3-3                             |

### 仅运行管理节点 Run manager-only nodes

By default manager nodes also act as a worker nodes. This means the scheduler can assign tasks to a manager node. For small and non-critical swarms assigning tasks to managers is relatively low-risk as long as you schedule services using resource constraints for cpu and memory.

​	默认情况下，管理节点也作为工作节点运行。这意味着调度程序可以将任务分配给管理节点。对于小型和非关键群集，将任务分配给管理节点的风险相对较低，只要您使用 CPU 和内存资源约束来安排服务。

However, because manager nodes use the Raft consensus algorithm to replicate data in a consistent way, they are sensitive to resource starvation. You should isolate managers in your swarm from processes that might block swarm operations like swarm heartbeat or leader elections.

​	然而，由于管理节点使用 Raft 一致性算法以一致的方式复制数据，因此对资源的竞争非常敏感。您应将群集中的管理节点与可能阻碍群集操作的进程（如群集心跳或领导选举）隔离开来。

To avoid interference with manager node operation, you can drain manager nodes to make them unavailable as worker nodes:

​	要避免对管理节点操作的干扰，可以将管理节点置于不可用状态，以防止其作为工作节点：



```console
$ docker node update --availability drain NODE
```

When you drain a node, the scheduler reassigns any tasks running on the node to other available worker nodes in the swarm. It also prevents the scheduler from assigning tasks to the node.

​	将节点设置为不可用状态后，调度程序会将该节点上运行的任何任务重新分配给群集中其他可用的工作节点。

## 添加工作节点以实现负载均衡 Add worker nodes for load balancing

[Add nodes to the swarm]({{< ref "/manuals/DockerEngine/Swarmmode/Joinnodestoaswarm" >}}) to balance your swarm's load. Replicated service tasks are distributed across the swarm as evenly as possible over time, as long as the worker nodes are matched to the requirements of the services. When limiting a service to run on only specific types of nodes, such as nodes with a specific number of CPUs or amount of memory, remember that worker nodes that do not meet these requirements cannot run these tasks.

​	[向集群添加节点]({{< ref "/manuals/DockerEngine/Swarmmode/Joinnodestoaswarm" >}})以平衡集群的负载。服务的复制任务会尽可能均匀地分配到整个集群中，只要工作节点符合服务的要求即可。当限制服务仅在特定类型的节点上运行（例如具有特定数量的 CPU 或内存的节点），请记住，不符合这些要求的工作节点无法运行这些任务。

## 监控集群健康状况 Monitor swarm health

You can monitor the health of manager nodes by querying the docker `nodes` API in JSON format through the `/nodes` HTTP endpoint. Refer to the [nodes API documentation](https://docs.docker.com/reference/api/engine/v1.25/#tag/Node) for more information.

​	可以通过 `/nodes` HTTP 端点以 JSON 格式查询 Docker `nodes` API 来监控管理节点的健康状态。有关更多信息，请参阅 [nodes API 文档](https://docs.docker.com/reference/api/engine/v1.25/#tag/Node)。

From the command line, run `docker node inspect <id-node>` to query the nodes. For instance, to query the reachability of the node as a manager:

​	在命令行中运行 `docker node inspect <id-node>` 查询节点。例如，查询该节点作为管理节点的可达性：

```console
$ docker node inspect manager1 --format "{{ .ManagerStatus.Reachability }}"
reachable
```

To query the status of the node as a worker that accept tasks:

​	查询该节点作为工作节点接受任务的状态：



```console
$ docker node inspect manager1 --format "{{ .Status.State }}"
ready
```

From those commands, we can see that `manager1` is both at the status `reachable` as a manager and `ready` as a worker.

​	通过这些命令可以看到，`manager1` 的状态是作为管理节点的 `reachable` 和作为工作节点的 `ready`。

An `unreachable` health status means that this particular manager node is unreachable from other manager nodes. In this case you need to take action to restore the unreachable manager:

​	如果健康状态显示 `unreachable`，意味着该管理节点与其他管理节点无法通信。在这种情况下，需要采取以下措施恢复不可达的管理节点：

- Restart the daemon and see if the manager comes back as reachable.
  - 重启守护进程并查看管理节点是否变为可达。

- Reboot the machine.
  - 重启机器。

- If neither restarting nor rebooting works, you should add another manager node or promote a worker to be a manager node. You also need to cleanly remove the failed node entry from the manager set with `docker node demote <NODE>` and `docker node rm <id-node>`.
  - 如果重启无效，您可以添加另一个管理节点或提升一个工作节点为管理节点。此外，还需要使用 `docker node demote <NODE>` 和 `docker node rm <id-node>` 清理故障节点的条目。


Alternatively you can also get an overview of the swarm health from a manager node with `docker node ls`:

​	您还可以使用 `docker node ls` 从管理节点概览整个集群的健康状况：



```console
$ docker node ls
ID                           HOSTNAME  MEMBERSHIP  STATUS  AVAILABILITY  MANAGER STATUS
1mhtdwhvsgr3c26xxbnzdc3yp    node05    Accepted    Ready   Active
516pacagkqp2xc3fk9t1dhjor    node02    Accepted    Ready   Active        Reachable
9ifojw8of78kkusuc4a6c23fx *  node01    Accepted    Ready   Active        Leader
ax11wdpwrrb6db3mfjydscgk7    node04    Accepted    Ready   Active
bb1nrq2cswhtbg4mrsqnlx1ck    node03    Accepted    Ready   Active        Reachable
di9wxgz8dtuh9d2hn089ecqkf    node06    Accepted    Ready   Active
```

## 排查管理节点故障 Troubleshoot a manager node

You should never restart a manager node by copying the `raft` directory from another node. The data directory is unique to a node ID. A node can only use a node ID once to join the swarm. The node ID space should be globally unique.

​	切勿通过复制其他节点的 `raft` 目录来重启管理节点。数据目录对节点 ID 是唯一的，每个节点只能使用一次节点 ID 加入集群，且节点 ID 空间应全局唯一。

To cleanly re-join a manager node to a cluster:

​	要将管理节点清除后重新加入集群：

1. Demote the node to a worker using `docker node demote <NODE>`. 使用 `docker node demote <NODE>` 将该节点降级为工作节点。
2. Remove the node from the swarm using `docker node rm <NODE>`. 使用 `docker node rm <NODE>` 将该节点从集群中移除。
3. Re-join the node to the swarm with a fresh state using `docker swarm join`. 使用 `docker swarm join` 将节点以新状态重新加入集群。

For more information on joining a manager node to a swarm, refer to [Join nodes to a swarm]({{< ref "/manuals/DockerEngine/Swarmmode/Joinnodestoaswarm" >}}).

​	有关将管理节点加入集群的更多信息，请参阅 [加入集群的节点]({{< ref "/manuals/DockerEngine/Swarmmode/Joinnodestoaswarm" >}})。

## 强制删除节点 Forcibly remove a node

In most cases, you should shut down a node before removing it from a swarm with the `docker node rm` command. If a node becomes unreachable, unresponsive, or compromised you can forcefully remove the node without shutting it down by passing the `--force` flag. For instance, if `node9` becomes compromised:

​	在大多数情况下，应在关闭节点后使用 `docker node rm` 命令将其从集群中移除。如果节点变得不可达、无响应或受损，可以使用 `--force` 标志强制删除节点而无需先关闭。例如，如果 `node9` 被破坏：

```none
$ docker node rm node9

Error response from daemon: rpc error: code = 9 desc = node node9 is not down and can't be removed

$ docker node rm --force node9

Node node9 removed from swarm
```

Before you forcefully remove a manager node, you must first demote it to the worker role. Make sure that you always have an odd number of manager nodes if you demote or remove a manager.

​	在强制删除管理节点之前，必须先将其降级为工作节点。如果您降级或删除了一个管理节点，请确保始终保持奇数个管理节点。

## 备份集群 Back up the swarm

Docker manager nodes store the swarm state and manager logs in the `/var/lib/docker/swarm/` directory. This data includes the keys used to encrypt the Raft logs. Without these keys, you cannot restore the swarm.

​	Docker 管理节点将集群状态和管理日志存储在 `/var/lib/docker/swarm/` 目录中。该数据包括用于加密 Raft 日志的密钥，丢失这些密钥将无法恢复集群。

You can back up the swarm using any manager. Use the following procedure.

​	可以通过任何管理节点备份集群，操作步骤如下：

1. If the swarm has auto-lock enabled, you need the unlock key to restore the swarm from backup. Retrieve the unlock key if necessary and store it in a safe location. If you are unsure, read [Lock your swarm to protect its encryption key]({{< ref "/manuals/DockerEngine/Swarmmode/Lockyourswarmtoprotectitsencryptionkey" >}}). 如果集群启用了自动锁定，则在从备份恢复集群时需要解锁密钥。如有需要，检索解锁密钥并将其存放在安全位置。如有疑问，请阅读 [锁定集群以保护其加密密钥]({{< ref "/manuals/DockerEngine/Swarmmode/Lockyourswarmtoprotectitsencryptionkey" >}})。

2. Stop Docker on the manager before backing up the data, so that no data is being changed during the backup. It is possible to take a backup while the manager is running (a "hot" backup), but this is not recommended and your results are less predictable when restoring. While the manager is down, other nodes continue generating swarm data that is not part of this backup. 在备份数据之前，停止管理节点上的 Docker，以确保备份期间数据不会发生更改。可以在管理节点运行时进行“热”备份，但不建议这样做，恢复时结果可能不稳定。管理节点停机期间，其他节点仍会生成不在此次备份范围内的集群数据。

   > **Note**
   >
   > 
   >
   > Be sure to maintain the quorum of swarm managers. During the time that a manager is shut down, your swarm is more vulnerable to losing the quorum if further nodes are lost. The number of managers you run is a trade-off. If you regularly take down managers to do backups, consider running a five manager swarm, so that you can lose an additional manager while the backup is running, without disrupting your services.
   >
   > ​	请确保保持集群管理节点的法定人数。在管理节点停机时，集群更易因丢失更多节点而失去法定人数。管理节点数量是一种权衡。如果您经常停用管理节点进行备份，请考虑运行五个管理节点的集群，以便在备份运行期间仍可以额外失去一个管理节点而不影响服务。

3. Back up the entire `/var/lib/docker/swarm` directory. 备份整个 `/var/lib/docker/swarm` 目录。

4. Restart the manager. 重启管理节点。

To restore, see [Restore from a backup](https://docs.docker.com/engine/swarm/admin_guide/#restore-from-a-backup).

​	要恢复，请参见 [从备份恢复](https://docs.docker.com/engine/swarm/admin_guide/#restore-from-a-backup)。

## 灾难恢复 Recover from disaster

### 从备份恢复 Restore from a backup

After backing up the swarm as described in [Back up the swarm](https://docs.docker.com/engine/swarm/admin_guide/#back-up-the-swarm), use the following procedure to restore the data to a new swarm.

​	在 [备份集群](https://docs.docker.com/engine/swarm/admin_guide/#back-up-the-swarm) 后，使用以下步骤将数据恢复到新集群：

1. Shut down Docker on the target host machine for the restored swarm. 在目标主机上关闭 Docker。

2. Remove the contents of the `/var/lib/docker/swarm` directory on the new swarm. 删除新集群中 `/var/lib/docker/swarm` 目录的内容。

3. Restore the `/var/lib/docker/swarm` directory with the contents of the backup. 使用备份内容恢复 `/var/lib/docker/swarm` 目录。

   > **Note**
   >
   > 
   >
   > The new node uses the same encryption key for on-disk storage as the old one. It is not possible to change the on-disk storage encryption keys at this time.
   >
   > ​	新节点使用与旧节点相同的磁盘存储加密密钥。当前无法更改磁盘存储加密密钥。
   >
   > In the case of a swarm with auto-lock enabled, the unlock key is also the same as on the old swarm, and the unlock key is needed to restore the swarm.
   >
   > ​	对于启用了自动锁定的集群，解锁密钥与旧集群相同，恢复集群时需要该密钥。

4. Start Docker on the new node. Unlock the swarm if necessary. Re-initialize the swarm using the following command, so that this node does not attempt to connect to nodes that were part of the old swarm, and presumably no longer exist.

   启动新节点上的 Docker。必要时解锁集群，并使用以下命令重新初始化集群，使该节点不再尝试连接旧集群中的节点：

   ```console
   $ docker swarm init --force-new-cluster
   ```

5. Verify that the state of the swarm is as expected. This may include application-specific tests or simply checking the output of `docker service ls` to be sure that all expected services are present. 验证集群状态是否符合预期。这可能包括应用程序特定的测试或简单地检查 `docker service ls` 的输出，以确保所有期望的服务都存在。

6. If you use auto-lock, [rotate the unlock key](https://docs.docker.com/engine/swarm/swarm_manager_locking/#rotate-the-unlock-key). 如果使用自动锁定，请[旋转解锁密钥](https://docs.docker.com/engine/swarm/swarm_manager_locking/#rotate-the-unlock-key)。

7. Add manager and worker nodes to bring your new swarm up to operating capacity. 添加管理节点和工作节点以将新集群恢复到正常运行状态。

8. Reinstate your previous backup regimen on the new swarm. 在新集群上重新设置以前的备份流程。

### 从丢失法定人数中恢复 Recover from losing the quorum

Swarm is resilient to failures and can recover from any number of temporary node failures (machine reboots or crash with restart) or other transient errors. However, a swarm cannot automatically recover if it loses a quorum. Tasks on existing worker nodes continue to run, but administrative tasks are not possible, including scaling or updating services and joining or removing nodes from the swarm. The best way to recover is to bring the missing manager nodes back online. If that is not possible, continue reading for some options for recovering your swarm.

​	Swarm 对故障具有较强的弹性，可以从任何数量的临时节点故障（例如机器重启或崩溃后重启）或其他瞬态错误中恢复。然而，如果集群失去法定人数则无法自动恢复。现有工作节点上的任务将继续运行，但无法执行管理任务，包括扩展或更新服务以及加入或移除节点。恢复的最佳方法是将丢失的管理节点重新上线。如果无法实现，请继续阅读其他恢复集群的选项。

In a swarm of `N` managers, a quorum (a majority) of manager nodes must always be available. For example, in a swarm with five managers, a minimum of three must be operational and in communication with each other. In other words, the swarm can tolerate up to `(N-1)/2` permanent failures beyond which requests involving swarm management cannot be processed. These types of failures include data corruption or hardware failures.

​	在一个拥有 `N` 个管理节点的集群中，始终需要多数（即法定人数）的管理节点可用。例如，对于五个管理节点的集群，至少有三个管理节点应在运行并相互通信。换句话说，集群最多可以容忍 `(N-1)/2` 个永久性故障，再多则无法处理集群管理相关的请求。这些类型的故障包括数据损坏或硬件故障。

If you lose the quorum of managers, you cannot administer the swarm. If you have lost the quorum and you attempt to perform any management operation on the swarm, an error occurs:

​	如果您失去了法定人数，无法管理集群。如果尝试对失去法定人数的集群执行任何管理操作，会出现错误：

```none
Error response from daemon: rpc error: code = 4 desc = context deadline exceeded
```

The best way to recover from losing the quorum is to bring the failed nodes back online. If you can't do that, the only way to recover from this state is to use the `--force-new-cluster` action from a manager node. This removes all managers except the manager the command was run from. The quorum is achieved because there is now only one manager. Promote nodes to be managers until you have the desired number of managers.

​	恢复法定人数的最佳方法是让故障节点重新上线。如果无法做到，唯一的恢复方式是在管理节点上使用 `--force-new-cluster` 命令。此操作会删除除当前管理节点外的所有管理节点。由于现在只有一个管理节点，法定人数恢复。将节点提升为管理节点，直到达到所需的管理节点数。

From the node to recover, run:

​	在需要恢复的节点上运行：

```console
$ docker swarm init --force-new-cluster --advertise-addr node01:2377
```

When you run the `docker swarm init` command with the `--force-new-cluster` flag, the Docker Engine where you run the command becomes the manager node of a single-node swarm which is capable of managing and running services. The manager has all the previous information about services and tasks, worker nodes are still part of the swarm, and services are still running. You need to add or re-add manager nodes to achieve your previous task distribution and ensure that you have enough managers to maintain high availability and prevent losing the quorum.

​	使用 `--force-new-cluster` 标志运行 `docker swarm init` 命令时，执行命令的 Docker 引擎成为可以管理和运行服务的单节点集群的管理节点。该管理节点保留了所有关于服务和任务的之前信息，工作节点仍然是集群的一部分，服务也在运行中。您需要添加或重新添加管理节点，以恢复之前的任务分配，并确保有足够的管理节点以维持高可用性并防止法定人数丢失。

## 强制集群重新平衡 Force the swarm to rebalance

Generally, you do not need to force the swarm to rebalance its tasks. When you add a new node to a swarm, or a node reconnects to the swarm after a period of unavailability, the swarm does not automatically give a workload to the idle node. This is a design decision. If the swarm periodically shifted tasks to different nodes for the sake of balance, the clients using those tasks would be disrupted. The goal is to avoid disrupting running services for the sake of balance across the swarm. When new tasks start, or when a node with running tasks becomes unavailable, those tasks are given to less busy nodes. The goal is eventual balance, with minimal disruption to the end user.

​	通常情况下，无需强制集群重新平衡任务。当向集群添加新节点或节点在一段时间不可用后重新连接到集群时，集群不会自动将任务分配给空闲节点。这是一种设计决策，如果集群为平衡的目的定期将任务转移到不同的节点，客户端会受到干扰。目标是避免为追求集群的平衡而打扰正在运行的服务。新任务启动时，或运行任务的节点不可用时，这些任务会分配给负载较低的节点。目标是最终平衡，尽量减少对终端用户的干扰。

You can use the `--force` or `-f` flag with the `docker service update` command to force the service to redistribute its tasks across the available worker nodes. This causes the service tasks to restart. Client applications may be disrupted. If you have configured it, your service uses a [rolling update]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Applyrollingupdatestoaservice" >}}).

​	您可以使用 `docker service update` 命令的 `--force` 或 `-f` 标志，强制服务在可用的工作节点之间重新分配任务。这会导致服务任务重新启动，可能会中断客户端应用。如果配置了滚动更新，服务会使用[滚动更新]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Applyrollingupdatestoaservice" >}})。

If you use an earlier version and you want to achieve an even balance of load across workers and don't mind disrupting running tasks, you can force your swarm to re-balance by temporarily scaling the service upward. Use `docker service inspect --pretty <servicename>` to see the configured scale of a service. When you use `docker service scale`, the nodes with the lowest number of tasks are targeted to receive the new workloads. There may be multiple under-loaded nodes in your swarm. You may need to scale the service up by modest increments a few times to achieve the balance you want across all the nodes.

​	如果您使用的是较早版本，并希望实现负载在工作节点之间的均匀分布且不介意中断运行中的任务，可以通过临时扩展服务来强制集群重新平衡。使用 `docker service inspect --pretty <servicename>` 查看服务的当前规模。使用 `docker service scale` 时，负载较低的节点会优先接收新工作负载。您的集群中可能有多个负载不足的节点，可以通过小幅度扩展服务几次，达到在所有节点之间的均衡分布。

When the load is balanced to your satisfaction, you can scale the service back down to the original scale. You can use `docker service ps` to assess the current balance of your service across nodes.

​	当负载达到满意的平衡时，您可以将服务缩减回原始规模。可以使用 `docker service ps` 查看服务在各节点之间的当前负载平衡状态。

See also [`docker service scale`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicescale" >}}) and [`docker service ps`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceps" >}}).

​	另请参阅 [`docker service scale`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicescale" >}}) 和 [`docker service ps`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceps" >}})。
