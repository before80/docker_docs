+++
title = "在 Swarm 中将节点设置为 Drain 模式"
date = 2024-10-23T14:54:40+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/swarm-tutorial/drain-node/](https://docs.docker.com/engine/swarm/swarm-tutorial/drain-node/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Drain a node on the swarm - 在 Swarm 中将节点设置为 Drain 模式

In earlier steps of the tutorial, all the nodes have been running with `Active` availability. The swarm manager can assign tasks to any `Active` node, so up to now all nodes have been available to receive tasks.

​	在教程的前几步中，所有节点的可用性均为 `Active`，因此 Swarm 管理器可以将任务分配给任何 `Active` 节点，所有节点都可以接收任务。

Sometimes, such as planned maintenance times, you need to set a node to `Drain` availability. `Drain` availability prevents a node from receiving new tasks from the swarm manager. It also means the manager stops tasks running on the node and launches replica tasks on a node with `Active` availability.

​	有时候，比如计划中的维护期间，您需要将节点设置为 `Drain` 可用性。`Drain` 可用性会阻止节点接收来自 Swarm 管理器的新任务，同时意味着管理器会停止在该节点上运行的任务，并在其他 `Active` 节点上启动任务副本。

> **Important**
>
> 
>
> Setting a node to `Drain` does not remove standalone containers from that node, such as those created with `docker run`, `docker compose up`, or the Docker Engine API. A node's status, including `Drain`, only affects the node's ability to schedule swarm service workloads.
>
> ​	将节点设置为 `Drain` 不会删除该节点上的独立容器，例如通过 `docker run`、`docker compose up` 或 Docker Engine API 创建的容器。节点的状态（包括 `Drain`）只影响该节点调度 Swarm 服务工作负载的能力。

1. If you haven't already, open a terminal and ssh into the machine where you run your manager node. For example, the tutorial uses a machine named `manager1`. 如果还没有，打开终端并通过 ssh 连接到运行管理节点的机器。例如，教程使用的机器名为 `manager1`。

2. Verify that all your nodes are actively available.

   验证所有节点是否处于 `Active` 可用性。

   ```console
   $ docker node ls
   
   ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
   1bcef6utixb0l0ca7gxuivsj0    worker2   Ready   Active
   38ciaotwjuritcdtn9npbnkuz    worker1   Ready   Active
   e216jshn25ckzbvmwlnh5jr3g *  manager1  Ready   Active        Leader
   ```

3. If you aren't still running the `redis` service from the [rolling update]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Applyrollingupdatestoaservice" >}}) tutorial, start it now:

   如果您还没有运行来自 [滚动更新]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Applyrollingupdatestoaservice" >}})教程中的 `redis` 服务，请现在启动它：

   ```console
   $ docker service create --replicas 3 --name redis --update-delay 10s redis:3.0.6
   
   c5uo6kdmzpon37mgj9mwglcfw
   ```

4. Run `docker service ps redis` to see how the swarm manager assigned the tasks to different nodes:

   运行 `docker service ps redis` 以查看 Swarm 管理器如何将任务分配给不同的节点：

   ```console
   $ docker service ps redis
   
   NAME                               IMAGE        NODE     DESIRED STATE  CURRENT STATE
   redis.1.7q92v0nr1hcgts2amcjyqg3pq  redis:3.0.6  manager1 Running        Running 26 seconds
   redis.2.7h2l8h3q3wqy5f66hlv9ddmi6  redis:3.0.6  worker1  Running        Running 26 seconds
   redis.3.9bg7cezvedmkgg6c8yzvbhwsd  redis:3.0.6  worker2  Running        Running 26 seconds
   ```

   In this case the swarm manager distributed one task to each node. You may see the tasks distributed differently among the nodes in your environment.

   ​	在本例中，Swarm 管理器将每个任务分配给一个节点。您的环境中，任务可能分布不同。

5. Run `docker node update --availability drain <NODE-ID>` to drain a node that had a task assigned to it:

   运行 `docker node update --availability drain <NODE-ID>` 以将任务分配到的节点设置为 `Drain` 模式：

   ```console
   $ docker node update --availability drain worker1
   
   worker1
   ```

6. Inspect the node to check its availability:

   检查节点以确认其可用性：

   ```console
   $ docker node inspect --pretty worker1
   
   ID:			38ciaotwjuritcdtn9npbnkuz
   Hostname:		worker1
   Status:
    State:			Ready
    Availability:		Drain
   ...snip...
   ```

   The drained node shows `Drain` for `Availability`. 被排空的节点在 `Availability` 中显示为 `Drain`。

7. Run `docker service ps redis` to see how the swarm manager updated the task assignments for the `redis` service:

   运行 `docker service ps redis` 以查看 Swarm 管理器如何更新 `redis` 服务的任务分配：

   ```console
   $ docker service ps redis
   
   NAME                                    IMAGE        NODE      DESIRED STATE  CURRENT STATE           ERROR
   redis.1.7q92v0nr1hcgts2amcjyqg3pq       redis:3.0.6  manager1  Running        Running 4 minutes
   redis.2.b4hovzed7id8irg1to42egue8       redis:3.0.6  worker2   Running        Running About a minute
    \_ redis.2.7h2l8h3q3wqy5f66hlv9ddmi6   redis:3.0.6  worker1   Shutdown       Shutdown 2 minutes ago
   redis.3.9bg7cezvedmkgg6c8yzvbhwsd       redis:3.0.6  worker2   Running        Running 4 minutes
   ```

   The swarm manager maintains the desired state by ending the task on a node with `Drain` availability and creating a new task on a node with `Active` availability.

   ​	Swarm 管理器通过终止处于 `Drain` 可用性节点上的任务并在 `Active` 可用性节点上创建新任务来维持期望的状态。

8. Run `docker node update --availability active <NODE-ID>` to return the drained node to an active state:

   运行 `docker node update --availability active <NODE-ID>` 将排空的节点恢复为活跃状态：

   ```console
   $ docker node update --availability active worker1
   
   worker1
   ```

9. Inspect the node to see the updated state:

   检查节点以查看更新后的状态：

   ```console
   $ docker node inspect --pretty worker1
   
   ID:			38ciaotwjuritcdtn9npbnkuz
   Hostname:		worker1
   Status:
    State:			Ready
    Availability:		Active
   ...snip...
   ```

   When you set the node back to `Active` availability, it can receive new tasks:

   ​	当节点重新设置为 `Active` 可用性后，它可以接收新任务：
   
   - during a service update to scale up
     - 在服务更新以扩展规模时
   
   - during a rolling update
     - 在滚动更新过程中
   - when you set another node to `Drain` availability
     - 当另一个节点设置为 `Drain` 可用性时
   - when a task fails on another active node
     - 当另一个活跃节点上的任务失败时

## Next steps

Next, you'll learn how to use a Swarm mode routing mesh

​	接下来，您将学习如何使用 Swarm 模式的路由网格

[Use a Swarm mode routing mesh 使用 Swarm 模式路由网格]({{< ref "/manuals/DockerEngine/Swarmmode/UseSwarmmoderoutingmesh" >}})
