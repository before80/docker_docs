+++
title = "向 Swarm 添加节点"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/swarm-tutorial/add-nodes/](https://docs.docker.com/engine/swarm/swarm-tutorial/add-nodes/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Add nodes to the swarm - 向 Swarm 添加节点

Once you've [created a swarm]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Createaswarm" >}}) with a manager node, you're ready to add worker nodes.

​	在使用管理节点 [创建 Swarm]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Createaswarm" >}}) 后，即可添加工作节点。

1. Open a terminal and ssh into the machine where you want to run a worker node. This tutorial uses the name `worker1`. 打开终端并通过 `ssh` 连接到您想运行工作节点的机器。本教程将该机器命名为 `worker1`。

2. Run the command produced by the `docker swarm init` output from the [Create a swarm]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Createaswarm" >}}) tutorial step to create a worker node joined to the existing swarm:

   运行 [创建 Swarm]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Createaswarm" >}}) 教程步骤中 `docker swarm init` 输出的命令，将工作节点加入到现有的 Swarm：

   ```console
   $ docker swarm join \
     --token  SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
     192.168.99.100:2377
   
   This node joined a swarm as a worker.
   ```

   If you don't have the command available, you can run the following command on a manager node to retrieve the join command for a worker:

   如果没有可用的命令，可以在管理节点上运行以下命令以获取工作节点的加入命令：

   ```console
   $ docker swarm join-token worker
   
   To add a worker to this swarm, run the following command:
   
       docker swarm join \
       --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
       192.168.99.100:2377
   ```

3. Open a terminal and ssh into the machine where you want to run a second worker node. This tutorial uses the name `worker2`. 打开终端并通过 `ssh` 连接到您想运行第二个工作节点的机器。本教程将该机器命名为 `worker2`。

4. Run the command produced by the `docker swarm init` output from the [Create a swarm]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Createaswarm" >}}) tutorial step to create a second worker node joined to the existing swarm:

   运行 [创建 Swarm]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Createaswarm" >}}) 教程步骤中 `docker swarm init` 输出的命令，将第二个工作节点加入到现有的 Swarm：

   ```console
   $ docker swarm join \
     --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
     192.168.99.100:2377
   
   This node joined a swarm as a worker.
   ```

5. Open a terminal and ssh into the machine where the manager node runs and run the `docker node ls` command to see the worker nodes:

   打开终端并通过 `ssh` 连接到运行管理节点的机器，运行 `docker node ls` 命令以查看工作节点：

   ```console
   $ docker node ls
   ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
   03g1y59jwfg7cf99w4lt0f662    worker2   Ready   Active
   9j68exjopxe7wfl6yuxml7a7j    worker1   Ready   Active
   dxn1zf6l61qsb1josjja83ngz *  manager1  Ready   Active        Leader
   ```

   The `MANAGER` column identifies the manager nodes in the swarm. The empty status in this column for `worker1` and `worker2` identifies them as worker nodes.

   ​	`MANAGER` 列标识 Swarm 中的管理节点。对于 `worker1` 和 `worker2`，此列为空状态，表明它们为工作节点。
   
   Swarm management commands like `docker node ls` only work on manager nodes.
   
   ​	Swarm 管理命令（如 `docker node ls`）只能在管理节点上运行。

## 接下来 What's next?

Now your swarm consists of a manager and two worker nodes. Next, you'll deploy a service.

​	现在您的 Swarm 由一个管理节点和两个工作节点组成。接下来，您将部署一个服务。

[Deploy a service 部署服务]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Deployaservicetotheswarm" >}})
