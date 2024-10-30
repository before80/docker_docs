+++
title = "扩容 Swarm 中的服务"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/swarm-tutorial/scale-service/](https://docs.docker.com/engine/swarm/swarm-tutorial/scale-service/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Scale the service in the swarm - 扩容 Swarm 中的服务

Once you have [deployed a service]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Deployaservicetotheswarm" >}}) to a swarm, you are ready to use the Docker CLI to scale the number of containers in the service. Containers running in a service are called tasks.

​	一旦你在 Swarm 中[部署了一个服务]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Deployaservicetotheswarm" >}})，你可以使用 Docker CLI 来扩容该服务中的容器数量。在服务中运行的容器被称为任务（tasks）。

1. If you haven't already, open a terminal and ssh into the machine where you run your manager node. For example, the tutorial uses a machine named `manager1`. 如果你还没有这样做，请打开终端并通过 SSH 连接到运行管理节点的机器上。本教程以名为 `manager1` 的机器为例。

2. Run the following command to change the desired state of the service running in the swarm:

   运行以下命令以更改在 Swarm 中运行的服务的目标状态：

   ```console
   $ docker service scale <SERVICE-ID>=<NUMBER-OF-TASKS>
   ```

   For example:

   

   ```console
   $ docker service scale helloworld=5
   
   helloworld scaled to 5
   ```

3. Run `docker service ps <SERVICE-ID>` to see the updated task list:

   运行 `docker service ps <SERVICE-ID>` 查看更新后的任务列表：

   ```console
   $ docker service ps helloworld
   
   NAME                                    IMAGE   NODE      DESIRED STATE  CURRENT STATE
   helloworld.1.8p1vev3fq5zm0mi8g0as41w35  alpine  worker2   Running        Running 7 minutes
   helloworld.2.c7a7tcdq5s0uk3qr88mf8xco6  alpine  worker1   Running        Running 24 seconds
   helloworld.3.6crl09vdcalvtfehfh69ogfb1  alpine  worker1   Running        Running 24 seconds
   helloworld.4.auky6trawmdlcne8ad8phb0f1  alpine  manager1  Running        Running 24 seconds
   helloworld.5.ba19kca06l18zujfwxyc5lkyn  alpine  worker2   Running        Running 24 seconds
   ```

   You can see that swarm has created 4 new tasks to scale to a total of 5 running instances of Alpine Linux. The tasks are distributed between the three nodes of the swarm. One is running on `manager1`.

   ​	你可以看到，Swarm 已创建了 4 个新任务，将 Alpine Linux 的运行实例数量扩容至 5 个。任务被分布在 Swarm 的三个节点上，其中一个在 `manager1` 上运行。

4. Run `docker ps` to see the containers running on the node where you're connected. The following example shows the tasks running on `manager1`:

   运行 `docker ps` 查看当前节点上运行的容器。以下示例展示了在 `manager1` 上运行的任务：

   ```console
   $ docker ps
   
   CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
   528d68040f95        alpine:latest       "ping docker.com"   About a minute ago   Up About a minute                       helloworld.4.auky6trawmdlcne8ad8phb0f1
   ```

   If you want to see the containers running on other nodes, ssh into those nodes and run the `docker ps` command.
   
   ​	如果你想查看其他节点上运行的容器，可以通过 SSH 连接到这些节点并运行 `docker ps` 命令。

## Next steps

At this point in the tutorial, you're finished with the `helloworld` service. Next, you'll delete the service

​	到此为止，本教程中的 `helloworld` 服务部分已完成。接下来，你将删除该服务。

[Delete the service 删除服务]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Deletetheservicerunningontheswarm" >}})
