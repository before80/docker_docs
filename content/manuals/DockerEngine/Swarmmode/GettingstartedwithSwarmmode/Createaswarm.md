+++
title = "创建一个 Swarm 集群"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/swarm-tutorial/create-swarm/](https://docs.docker.com/engine/swarm/swarm-tutorial/create-swarm/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Create a swarm - 创建一个 Swarm 集群

After you complete the [tutorial setup]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode" >}}) steps, you're ready to create a swarm. Make sure the Docker Engine daemon is started on the host machines.

​	在完成了 [教程设置]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode" >}}) 步骤后，您已经准备好创建一个 Swarm 集群。确保在主机上已启动 Docker Engine 守护进程。

1. Open a terminal and ssh into the machine where you want to run your manager node. This tutorial uses a machine named `manager1`. 打开终端并通过 `ssh` 连接到您要运行管理节点的机器。此教程使用的机器名称为 `manager1`。

2. Run the following command to create a new swarm:

   运行以下命令以创建一个新的 Swarm 集群：

   ```console
   $ docker swarm init --advertise-addr <MANAGER-IP>
   ```

   In the tutorial, the following command creates a swarm on the `manager1` machine:

   在本教程中，以下命令在 `manager1` 机器上创建了一个 Swarm 集群：

   ```console
   $ docker swarm init --advertise-addr 192.168.99.100
   Swarm initialized: current node (dxn1zf6l61qsb1josjja83ngz) is now a manager.
   
   To add a worker to this swarm, run the following command:
   
       docker swarm join \
       --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
       192.168.99.100:2377
   
   To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
   ```

   The `--advertise-addr` flag configures the manager node to publish its address as `192.168.99.100`. The other nodes in the swarm must be able to access the manager at the IP address.

   ​	`--advertise-addr` 标志将管理节点的地址配置为 `192.168.99.100`。Swarm 集群中的其他节点必须能够通过该 IP 地址访问管理节点。

   The output includes the commands to join new nodes to the swarm. Nodes will join as managers or workers depending on the value for the `--token` flag.

   ​	输出中包含了将新节点加入到 Swarm 集群的命令。根据 `--token` 标志的值，节点将作为管理节点或工作节点加入。

3. Run `docker info` to view the current state of the swarm:

   运行 `docker info` 以查看当前 Swarm 集群的状态：

   ```console
   $ docker info
   
   Containers: 2
   Running: 0
   Paused: 0
   Stopped: 2
     ...snip...
   Swarm: active
     NodeID: dxn1zf6l61qsb1josjja83ngz
     Is Manager: true
     Managers: 1
     Nodes: 1
     ...snip...
   ```

4. Run the `docker node ls` command to view information about nodes:

   运行 `docker node ls` 命令查看有关节点的信息：

   ```console
   $ docker node ls
   
   ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
   dxn1zf6l61qsb1josjja83ngz *  manager1  Ready   Active        Leader
   ```

   The `*` next to the node ID indicates that you're currently connected on this node.

   ​	节点 ID 旁边的 `*` 表示您当前连接在此节点上。
   
   Docker Engine Swarm mode automatically names the node with the machine host name. The tutorial covers other columns in later steps.
   
   ​	Docker Engine Swarm 模式会自动将节点命名为机器的主机名。教程中的后续步骤将详细介绍其他列。

## Next steps

Next, you'll add two more nodes to the cluster.

​	接下来，您将向集群中添加另外两个节点。

[Add two more nodes 添加两个节点]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Addnodestotheswarm" >}})
