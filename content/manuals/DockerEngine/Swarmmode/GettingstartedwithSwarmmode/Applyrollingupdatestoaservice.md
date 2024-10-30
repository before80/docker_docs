+++
title = "对服务应用滚动更新"
date = 2024-10-23T14:54:40+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/swarm-tutorial/rolling-update/](https://docs.docker.com/engine/swarm/swarm-tutorial/rolling-update/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Apply rolling updates to a service - 对服务应用滚动更新

In a previous step of the tutorial, you [scaled]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Scaletheserviceintheswarm" >}}) the number of instances of a service. In this part of the tutorial, you deploy a service based on the Redis 3.0.6 container tag. Then you upgrade the service to use the Redis 3.0.7 container image using rolling updates.

​	在教程的前一步中，您已经[扩展]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Scaletheserviceintheswarm" >}})了服务实例的数量。在本教程中，您将基于 Redis 3.0.6 容器标签部署一个服务，并使用滚动更新将服务升级为 Redis 3.0.7 容器镜像。

1. If you haven't already, open a terminal and ssh into the machine where you run your manager node. For example, the tutorial uses a machine named `manager1`. 如果尚未完成，请打开终端并通过 `ssh` 连接到运行管理节点的机器，例如，教程使用的机器名为 `manager1`。

2. Deploy your Redis tag to the swarm and configure the swarm with a 10 second update delay. Note that the following example shows an older Redis tag:

   将您的 Redis 标签部署到 Swarm，并配置 Swarm 以 10 秒更新延迟。以下示例展示了一个较旧的 Redis 标签：

   ```console
   $ docker service create \
     --replicas 3 \
     --name redis \
     --update-delay 10s \
     redis:3.0.6
   
   0u6a4s31ybk7yw2wyvtikmu50
   ```

   You configure the rolling update policy at service deployment time.

   ​	您可以在服务部署时配置滚动更新策略。

   The `--update-delay` flag configures the time delay between updates to a service task or sets of tasks. You can describe the time `T` as a combination of the number of seconds `Ts`, minutes `Tm`, or hours `Th`. So `10m30s` indicates a 10 minute 30 second delay.

   ​	`--update-delay` 标志配置更新服务任务或任务集之间的时间延迟。您可以使用秒 `Ts`、分钟 `Tm` 或小时 `Th` 的组合表示时间 `T`。例如，`10m30s` 表示 10 分钟 30 秒的延迟。

   By default the scheduler updates 1 task at a time. You can pass the `--update-parallelism` flag to configure the maximum number of service tasks that the scheduler updates simultaneously.

   ​	默认情况下，调度器每次更新 1 个任务。您可以通过 `--update-parallelism` 标志配置调度器同时更新的最大服务任务数量。

   By default, when an update to an individual task returns a state of `RUNNING`, the scheduler schedules another task to update until all tasks are updated. If at any time during an update a task returns `FAILED`, the scheduler pauses the update. You can control the behavior using the `--update-failure-action` flag for `docker service create` or `docker service update`.

   ​	默认情况下，当一个任务的更新返回 `RUNNING` 状态时，调度器会调度另一个任务进行更新，直到所有任务都更新完成。如果在更新过程中任何任务返回 `FAILED` 状态，调度器会暂停更新。您可以通过 `--update-failure-action` 标志控制 `docker service create` 或 `docker service update` 的行为。

3. Inspect the `redis` service:

   检查 `redis` 服务：

   ```console
   $ docker service inspect --pretty redis
   
   ID:             0u6a4s31ybk7yw2wyvtikmu50
   Name:           redis
   Service Mode:   Replicated
    Replicas:      3
   Placement:
    Strategy:	    Spread
   UpdateConfig:
    Parallelism:   1
    Delay:         10s
   ContainerSpec:
    Image:         redis:3.0.6
   Resources:
   Endpoint Mode:  vip
   ```

4. Now you can update the container image for `redis`. The swarm manager applies the update to nodes according to the `UpdateConfig` policy:

   现在您可以更新 `redis` 的容器镜像。Swarm 管理器会根据 `UpdateConfig` 策略将更新应用于节点：

   ```console
   $ docker service update --image redis:3.0.7 redis
   redis
   ```

   The scheduler applies rolling updates as follows by default:

   ​	调度器按默认方式执行滚动更新：

   - Stop the first task.
     - 停止第一个任务。
   - Schedule update for the stopped task.
     - 为停止的任务调度更新。
   - Start the container for the updated task.
     - 启动更新任务的容器。
   - If the update to a task returns `RUNNING`, wait for the specified delay period then start the next task.
     - 如果任务更新返回 `RUNNING` 状态，等待指定的延迟时间后启动下一个任务。
   - If, at any time during the update, a task returns `FAILED`, pause the update.
     - 如果在更新过程中任何任务返回 `FAILED` 状态，暂停更新。

5. Run `docker service inspect --pretty redis` to see the new image in the desired state:

   运行 `docker service inspect --pretty redis` 查看新镜像的目标状态：

   ```console
   $ docker service inspect --pretty redis
   
   ID:             0u6a4s31ybk7yw2wyvtikmu50
   Name:           redis
   Service Mode:   Replicated
    Replicas:      3
   Placement:
    Strategy:	    Spread
   UpdateConfig:
    Parallelism:   1
    Delay:         10s
   ContainerSpec:
    Image:         redis:3.0.7
   Resources:
   Endpoint Mode:  vip
   ```

   The output of `service inspect` shows if your update paused due to failure:

   `service inspect` 的输出显示了更新是否因故障而暂停：

   ```console
   $ docker service inspect --pretty redis
   
   ID:             0u6a4s31ybk7yw2wyvtikmu50
   Name:           redis
   ...snip...
   Update status:
    State:      paused
    Started:    11 seconds ago
    Message:    update paused due to failure or early termination of task 9p7ith557h8ndf0ui9s0q951b
   ...snip...
   ```

   To restart a paused update run `docker service update <SERVICE-ID>`. For example:

   要重新启动已暂停的更新，请运行 `docker service update <SERVICE-ID>`。例如：

   ```console
   $ docker service update redis
   ```

   To avoid repeating certain update failures, you may need to reconfigure the service by passing flags to `docker service update`.

   ​	为避免重复某些更新故障，您可能需要通过为 `docker service update` 传递标志来重新配置服务。

6. Run `docker service ps <SERVICE-ID>` to watch the rolling update:

   运行 `docker service ps <SERVICE-ID>` 观察滚动更新的进程：

   ```console
   $ docker service ps redis
   
   NAME                                   IMAGE        NODE       DESIRED STATE  CURRENT STATE            ERROR
   redis.1.dos1zffgeofhagnve8w864fco      redis:3.0.7  worker1    Running        Running 37 seconds
    \_ redis.1.88rdo6pa52ki8oqx6dogf04fh  redis:3.0.6  worker2    Shutdown       Shutdown 56 seconds ago
   redis.2.9l3i4j85517skba5o7tn5m8g0      redis:3.0.7  worker2    Running        Running About a minute
    \_ redis.2.66k185wilg8ele7ntu8f6nj6i  redis:3.0.6  worker1    Shutdown       Shutdown 2 minutes ago
   redis.3.egiuiqpzrdbxks3wxgn8qib1g      redis:3.0.7  worker1    Running        Running 48 seconds
    \_ redis.3.ctzktfddb2tepkr45qcmqln04  redis:3.0.6  mmanager1  Shutdown       Shutdown 2 minutes ago
   ```

   Before Swarm updates all of the tasks, you can see that some are running `redis:3.0.6` while others are running `redis:3.0.7`. The output above shows the state once the rolling updates are done.
   
   ​	在 Swarm 完成所有任务的更新之前，您可以看到一些任务在运行 `redis:3.0.6`，而另一些任务在运行 `redis:3.0.7`。上面的输出显示了滚动更新完成后的状态。

## 接下来 Next steps

Next, you'll learn how to drain a node in the swarm.

​	接下来，您将学习如何在 Swarm 中排空一个节点。

[Drain a node 排空节点]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Drainanodeontheswarm" >}})
