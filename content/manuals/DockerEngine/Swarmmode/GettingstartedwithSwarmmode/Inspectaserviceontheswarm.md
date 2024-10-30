+++
title = "在 Swarm 中检查服务"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/swarm-tutorial/inspect-service/](https://docs.docker.com/engine/swarm/swarm-tutorial/inspect-service/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Inspect a service on the swarm - 在 Swarm 中检查服务

When you have [deployed a service]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Deployaservicetotheswarm" >}}) to your swarm, you can use the Docker CLI to see details about the service running in the swarm.

​	当您已将服务 [部署到 Swarm]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Deployaservicetotheswarm" >}}) 时，可以使用 Docker CLI 查看该服务在 Swarm 中的详细信息。

1. If you haven't already, open a terminal and ssh into the machine where you run your manager node. For example, the tutorial uses a machine named `manager1`. 打开终端并通过 `ssh` 连接到运行管理节点的机器，例如 `manager1`。

2. Run `docker service inspect --pretty <SERVICE-ID>` to display the details about a service in an easily readable format. 运行 `docker service inspect --pretty <SERVICE-ID>` 以易读格式显示有关服务的详细信息。

   To see the details on the `helloworld` service:

   查看 `helloworld` 服务的详细信息：

   ```console
   [manager1]$ docker service inspect --pretty helloworld
   
   ID:		9uk4639qpg7npwf3fn2aasksr
   Name:		helloworld
   Service Mode:	REPLICATED
    Replicas:		1
   Placement:
   UpdateConfig:
    Parallelism:	1
   ContainerSpec:
    Image:		alpine
    Args:	ping docker.com
   Resources:
   Endpoint Mode:  vip
   ```

   > **Tip**
   >
   > To return the service details in json format, run the same command without the `--pretty` flag.
   >
   > ​	要以 JSON 格式返回服务详细信息，请在运行相同命令时去掉 `--pretty` 标志。

   

   ```console
   [manager1]$ docker service inspect helloworld
   [
   {
       "ID": "9uk4639qpg7npwf3fn2aasksr",
       "Version": {
           "Index": 418
       },
       "CreatedAt": "2016-06-16T21:57:11.622222327Z",
       "UpdatedAt": "2016-06-16T21:57:11.622222327Z",
       "Spec": {
           "Name": "helloworld",
           "TaskTemplate": {
               "ContainerSpec": {
                   "Image": "alpine",
                   "Args": [
                       "ping",
                       "docker.com"
                   ]
               },
               "Resources": {
                   "Limits": {},
                   "Reservations": {}
               },
               "RestartPolicy": {
                   "Condition": "any",
                   "MaxAttempts": 0
               },
               "Placement": {}
           },
           "Mode": {
               "Replicated": {
                   "Replicas": 1
               }
           },
           "UpdateConfig": {
               "Parallelism": 1
           },
           "EndpointSpec": {
               "Mode": "vip"
           }
       },
       "Endpoint": {
           "Spec": {}
       }
   }
   ]
   ```

3. Run `docker service ps <SERVICE-ID>` to see which nodes are running the service:

   运行 `docker service ps <SERVICE-ID>` 以查看运行该服务的节点：

   ```console
   [manager1]$ docker service ps helloworld
   
   NAME                                    IMAGE   NODE     DESIRED STATE  CURRENT STATE           ERROR               PORTS
   helloworld.1.8p1vev3fq5zm0mi8g0as41w35  alpine  worker2  Running        Running 3 minutes
   ```

   In this case, the one instance of the `helloworld` service is running on the `worker2` node. You may see the service running on your manager node. By default, manager nodes in a swarm can execute tasks just like worker nodes.

   ​	在此示例中，`helloworld` 服务的一个实例正在 `worker2` 节点上运行。您可能会在管理节点上看到该服务正在运行。默认情况下，Swarm 中的管理节点与工作节点一样可以执行任务。

   Swarm also shows you the `DESIRED STATE` and `CURRENT STATE` of the service task so you can see if tasks are running according to the service definition.

   ​	Swarm 还会显示服务任务的 `DESIRED STATE` 和 `CURRENT STATE`，以便您查看任务是否按照服务定义在运行。

4. Run `docker ps` on the node where the task is running to see details about the container for the task. 在任务运行的节点上运行 `docker ps` 以查看任务容器的详细信息。

   > **Tip**
   >
   > If `helloworld` is running on a node other than your manager node, you must ssh to that node.
   >
   > ​	如果 `helloworld` 运行在您的管理节点之外的其他节点上，您需要使用 ssh 连接到该节点。

   
   
   ```console
   [worker2]$ docker ps
   
   CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
   e609dde94e47        alpine:latest       "ping docker.com"   3 minutes ago       Up 3 minutes                            helloworld.1.8p1vev3fq5zm0mi8g0as41w35
   ```

## Next steps

Next, you'll change the scale for the service running in the swarm.

​	接下来，您将更改 Swarm 中运行的服务的规模。

[Change the scale 更改服务规模]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Scaletheserviceintheswarm" >}})

