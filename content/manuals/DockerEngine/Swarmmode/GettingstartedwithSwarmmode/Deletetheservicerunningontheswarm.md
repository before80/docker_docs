+++
title = "删除在 Swarm 中运行的服务"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/swarm-tutorial/delete-service/](https://docs.docker.com/engine/swarm/swarm-tutorial/delete-service/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Delete the service running on the swarm - 删除在 Swarm 中运行的服务

The remaining steps in the tutorial don't use the `helloworld` service, so now you can delete the service from the swarm.

​	教程的接下来的步骤不再使用 `helloworld` 服务，因此您可以从 Swarm 中删除该服务。

1. If you haven't already, open a terminal and ssh into the machine where you run your manager node. For example, the tutorial uses a machine named `manager1`. 如果尚未完成，请打开终端并通过 `ssh` 连接到运行管理节点的机器。例如，本教程使用的机器名为 `manager1`。

2. Run `docker service rm helloworld` to remove the `helloworld` service.

   运行 `docker service rm helloworld` 删除 `helloworld` 服务。

   ```console
   $ docker service rm helloworld
   
   helloworld
   ```

3. Run `docker service inspect <SERVICE-ID>` to verify that the swarm manager removed the service. The CLI returns a message that the service is not found:

   运行 `docker service inspect <SERVICE-ID>` 验证 Swarm 管理器是否已删除该服务。CLI 返回服务未找到的消息：

   ```console
   $ docker service inspect helloworld
   []
   Status: Error: no such service: helloworld, Code: 1
   ```

4. Even though the service no longer exists, the task containers take a few seconds to clean up. You can use `docker ps` on the nodes to verify when the tasks have been removed.

   即使服务不再存在，任务容器的清理仍需要几秒钟。您可以在各个节点上使用 `docker ps` 验证任务何时被移除。

   ```console
   $ docker ps
   
   CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS     NAMES
   db1651f50347        alpine:latest       "ping docker.com"        44 minutes ago      Up 46 seconds                 helloworld.5.9lkmos2beppihw95vdwxy1j3w
   43bf6e532a92        alpine:latest       "ping docker.com"        44 minutes ago      Up 46 seconds                 helloworld.3.a71i8rp6fua79ad43ycocl4t2
   5a0fb65d8fa7        alpine:latest       "ping docker.com"        44 minutes ago      Up 45 seconds                 helloworld.2.2jpgensh7d935qdc857pxulfr
   afb0ba67076f        alpine:latest       "ping docker.com"        44 minutes ago      Up 46 seconds                 helloworld.4.1c47o7tluz7drve4vkm2m5olx
   688172d3bfaa        alpine:latest       "ping docker.com"        45 minutes ago      Up About a minute             helloworld.1.74nbhb3fhud8jfrhigd7s29we
   
   $ docker ps
   CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS     NAMES
   ```

## Next steps

Next, you'll set up a new service and apply a rolling update.

​	接下来，您将设置一个新服务并应用滚动更新。

[Apply rolling updates 应用滚动更新]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Applyrollingupdatestoaservice" >}})
