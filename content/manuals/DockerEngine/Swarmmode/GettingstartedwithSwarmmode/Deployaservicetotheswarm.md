+++
title = "将服务部署到 Swarm 集群"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/swarm-tutorial/deploy-service/](https://docs.docker.com/engine/swarm/swarm-tutorial/deploy-service/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Deploy a service to the swarm - 将服务部署到 Swarm 集群

After you [create a swarm]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Createaswarm" >}}), you can deploy a service to the swarm. For this tutorial, you also [added worker nodes]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Addnodestotheswarm" >}}), but that is not a requirement to deploy a service.

​	在您 [创建 Swarm 集群]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Createaswarm" >}}) 后，您可以将服务部署到 Swarm 中。在本教程中，您还可以 [添加工作节点]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Addnodestotheswarm" >}})，但这不是部署服务的必要条件。

1. Open a terminal and ssh into the machine where you run your manager node. For example, the tutorial uses a machine named `manager1`. 打开终端并通过 `ssh` 连接到运行管理节点的机器，例如 `manager1`。

2. Run the following command:

   运行以下命令：

   ```console
   $ docker service create --replicas 1 --name helloworld alpine ping docker.com
   
   9uk4639qpg7npwf3fn2aasksr
   ```

   - The `docker service create` command creates the service.
     - `docker service create` 命令创建服务。
   - The `--name` flag names the service `helloworld`.
     - `--name` 标志将服务命名为 `helloworld`。
   - The `--replicas` flag specifies the desired state of 1 running instance.
     - `--replicas` 标志指定期望的状态为 1 个运行实例。
   - The arguments `alpine ping docker.com` define the service as an Alpine Linux container that executes the command `ping docker.com`.
     - `alpine ping docker.com` 指定该服务为一个执行 `ping docker.com` 命令的 Alpine Linux 容器。

3. Run `docker service ls` to see the list of running services:

   运行 `docker service ls` 查看正在运行的服务列表：

   ```console
   $ docker service ls
   
   ID            NAME        SCALE  IMAGE   COMMAND
   9uk4639qpg7n  helloworld  1/1    alpine  ping docker.com
   ```

## Next steps

Now you're ready to inspect the service.

​	现在您已准备好检查该服务。

[Inspect the service 检查服务]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Inspectaserviceontheswarm" >}})
