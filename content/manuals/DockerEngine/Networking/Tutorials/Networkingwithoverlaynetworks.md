+++
title = "使用 Overlay 网络的网络教程"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/network/tutorials/overlay/](https://docs.docker.com/engine/network/tutorials/overlay/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Networking with overlay networks - 使用 Overlay 网络的网络教程

This series of tutorials deals with networking for swarm services. For networking with standalone containers, see [Networking with standalone containers](https://docs.docker.com/engine/network/tutorials/standalone/). If you need to learn more about Docker networking in general, see the [overview](https://docs.docker.com/engine/network/).

​	本系列教程介绍了 swarm 服务的网络设置。关于独立容器的网络设置，请参见[独立容器的网络教程](https://docs.docker.com/engine/network/tutorials/standalone/)。如果需要了解更多 Docker 网络的概述，请参见[网络概述](https://docs.docker.com/engine/network/)。

This page includes the following tutorials. You can run each of them on Linux, Windows, or a Mac, but for the last one, you need a second Docker host running elsewhere.

​	此页面包含以下教程。您可以在 Linux、Windows 或 Mac 上运行这些教程，但最后一个示例需要第二个 Docker 主机。

- [Use the default overlay network](https://docs.docker.com/engine/network/tutorials/overlay/#use-the-default-overlay-network) demonstrates how to use the default overlay network that Docker sets up for you automatically when you initialize or join a swarm. This network is not the best choice for production systems.
  - [使用默认 Overlay 网络](https://docs.docker.com/engine/network/tutorials/overlay/#use-the-default-overlay-network) 演示如何使用 Docker 在初始化或加入 swarm 时自动设置的默认 overlay 网络。此网络不适合用于生产系统。

- [Use user-defined overlay networks](https://docs.docker.com/engine/network/tutorials/overlay/#use-a-user-defined-overlay-network) shows how to create and use your own custom overlay networks, to connect services. This is recommended for services running in production.
  - [使用用户定义的 Overlay 网络](https://docs.docker.com/engine/network/tutorials/overlay/#use-a-user-defined-overlay-network) 介绍如何创建并使用自定义 overlay 网络来连接服务。推荐生产环境中使用此方法。

- [Use an overlay network for standalone containers](https://docs.docker.com/engine/network/tutorials/overlay/#use-an-overlay-network-for-standalone-containers) shows how to communicate between standalone containers on different Docker daemons using an overlay network.
  - [为独立容器使用 Overlay 网络](https://docs.docker.com/engine/network/tutorials/overlay/#use-an-overlay-network-for-standalone-containers) 展示如何使用 overlay 网络在不同的 Docker 守护进程上的独立容器之间进行通信。

## 前置条件 Prerequisites

These require you to have at least a single-node swarm, which means that you have started Docker and run `docker swarm init` on the host. You can run the examples on a multi-node swarm as well.

​	这些教程需要至少一个单节点的 swarm，也就是说您需要启动 Docker 并在主机上运行 `docker swarm init`。示例也可以在多节点的 swarm 上运行。

## 使用默认的 overlay 网络 Use the default overlay network

In this example, you start an `alpine` service and examine the characteristics of the network from the point of view of the individual service containers.

​	在此示例中，您将启动一个 `alpine` 服务，并从单个服务容器的角度检查网络的特性。

This tutorial does not go into operation-system-specific details about how overlay networks are implemented, but focuses on how the overlay functions from the point of view of a service.

​	本教程不涉及与操作系统相关的 overlay 网络实现细节，而是重点关注服务视角下的 overlay 网络功能。

### 前置条件 Prerequisites

This tutorial requires three physical or virtual Docker hosts which can all communicate with one another. This tutorial assumes that the three hosts are running on the same network with no firewall involved.

​	本教程需要三个可以相互通信的物理或虚拟 Docker 主机。本教程假定这三台主机在没有防火墙的同一网络中运行。

These hosts will be referred to as `manager`, `worker-1`, and `worker-2`. The `manager` host will function as both a manager and a worker, which means it can both run service tasks and manage the swarm. `worker-1` and `worker-2` will function as workers only,

​	这些主机将被称为 `manager`、`worker-1` 和 `worker-2`。`manager` 主机将既作为管理节点又作为工作节点，既可以运行服务任务，也可以管理 swarm。`worker-1` 和 `worker-2` 仅作为工作节点。

If you don't have three hosts handy, an easy solution is to set up three Ubuntu hosts on a cloud provider such as Amazon EC2, all on the same network with all communications allowed to all hosts on that network (using a mechanism such as EC2 security groups), and then to follow the [installation instructions for Docker Engine - Community on Ubuntu](https://docs.docker.com/engine/install/ubuntu/).

​	如果没有三台主机，您可以在云提供商（如 Amazon EC2）上设置三台 Ubuntu 主机，所有主机位于同一网络中，并且允许相互通信（使用类似于 EC2 安全组的机制）。然后按照 [Docker Engine - Community 在 Ubuntu 上的安装说明](https://docs.docker.com/engine/install/ubuntu/) 进行安装。

### 操作步骤 Walkthrough

#### Create the swarm

At the end of this procedure, all three Docker hosts will be joined to the swarm and will be connected together using an overlay network called `ingress`.

​	完成该过程后，所有三台 Docker 主机将加入到 swarm 中，并通过一个名为 `ingress` 的 overlay 网络连接在一起。

1. On `manager`. initialize the swarm. If the host only has one network interface, the `--advertise-addr` flag is optional.

   在 `manager` 上初始化 swarm。如果主机只有一个网络接口，`--advertise-addr` 参数是可选的。

   ```console
   $ docker swarm init --advertise-addr=<IP-ADDRESS-OF-MANAGER>
   ```

   Make a note of the text that is printed, as this contains the token that you will use to join `worker-1` and `worker-2` to the swarm. It is a good idea to store the token in a password manager.

   ​	请记下打印的内容，其中包含用于让 `worker-1` 和 `worker-2` 加入 swarm 的令牌。建议将该令牌存储在密码管理器中。

2. On `worker-1`, join the swarm. If the host only has one network interface, the `--advertise-addr` flag is optional.

   在 `worker-1` 上加入 swarm。如果主机只有一个网络接口，`--advertise-addr` 参数是可选的。

   ```console
   $ docker swarm join --token TOKEN \
     --advertise-addr <IP-ADDRESS-OF-WORKER-1> \
     <IP-ADDRESS-OF-MANAGER>:2377
   ```

3. On `worker-2`, join the swarm. If the host only has one network interface, the `--advertise-addr` flag is optional.

   在 `worker-2` 上加入 swarm。如果主机只有一个网络接口，`--advertise-addr` 参数是可选的。

   ```console
   $ docker swarm join --token TOKEN \
     --advertise-addr <IP-ADDRESS-OF-WORKER-2> \
     <IP-ADDRESS-OF-MANAGER>:2377
   ```

4. On `manager`, list all the nodes. This command can only be done from a manager.

   在 `manager` 上列出所有节点。此命令只能在管理节点上执行。

   ```console
   $ docker node ls
   
   ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
   d68ace5iraw6whp7llvgjpu48 *   ip-172-31-34-146    Ready               Active              Leader
   nvp5rwavvb8lhdggo8fcf7plg     ip-172-31-35-151    Ready               Active
   ouvx2l7qfcxisoyms8mtkgahw     ip-172-31-36-89     Ready               Active
   ```

   You can also use the `--filter` flag to filter by role:

   您也可以使用 `--filter` 参数按角色过滤：

   ```console
   $ docker node ls --filter role=manager
   
   ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
   d68ace5iraw6whp7llvgjpu48 *   ip-172-31-34-146    Ready               Active              Leader
   
   $ docker node ls --filter role=worker
   
   ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
   nvp5rwavvb8lhdggo8fcf7plg     ip-172-31-35-151    Ready               Active
   ouvx2l7qfcxisoyms8mtkgahw     ip-172-31-36-89     Ready               Active
   ```

5. List the Docker networks on `manager`, `worker-1`, and `worker-2` and notice that each of them now has an overlay network called `ingress` and a bridge network called `docker_gwbridge`. Only the listing for `manager` is shown here:

   在 `manager`、`worker-1` 和 `worker-2` 上列出 Docker 网络，并注意每个主机现在都有一个名为 `ingress` 的 overlay 网络和一个名为 `docker_gwbridge` 的桥接网络。

   ```console
   $ docker network ls
   
   NETWORK ID          NAME                DRIVER              SCOPE
   495c570066be        bridge              bridge              local
   961c6cae9945        docker_gwbridge     bridge              local
   ff35ceda3643        host                host                local
   trtnl4tqnc3n        ingress             overlay             swarm
   c8357deec9cb        none                null                local
   ```

The `docker_gwbridge` connects the `ingress` network to the Docker host's network interface so that traffic can flow to and from swarm managers and workers. If you create swarm services and do not specify a network, they are connected to the `ingress` network. It is recommended that you use separate overlay networks for each application or group of applications which will work together. In the next procedure, you will create two overlay networks and connect a service to each of them.

​	`docker_gwbridge` 将 `ingress` 网络连接到 Docker 主机的网络接口，从而使流量能够在 swarm 管理节点和工作节点之间流动。如果您创建了 swarm 服务并未指定网络，它们将连接到 `ingress` 网络。建议为每个应用程序或相互协作的一组应用程序使用单独的 overlay 网络。在接下来的步骤中，您将创建两个 overlay 网络并将服务连接到它们中的每一个。

#### Create the services

1. On `manager`, create a new overlay network called `nginx-net`:

   在 `manager` 上创建一个名为 `nginx-net` 的新 overlay 网络：

   ```console
   $ docker network create -d overlay nginx-net
   ```

   You don't need to create the overlay network on the other nodes, because it will be automatically created when one of those nodes starts running a service task which requires it.

   ​	您无需在其他节点上创建 overlay 网络，因为当这些节点开始运行需要该网络的服务任务时，它会自动创建。

2. On `manager`, create a 5-replica Nginx service connected to `nginx-net`. The service will publish port 80 to the outside world. All of the service task containers can communicate with each other without opening any ports. 在 `manager` 上创建一个连接到 `nginx-net` 的 5 副本 Nginx 服务。该服务将端口 80 发布到外部。所有服务任务容器无需打开任何端口即可相互通信。

   > **Note**
   >
   > 
   >
   > Services can only be created on a manager.
   >
   > ​	服务只能在管理节点上创建。

   

   ```console
   $ docker service create \
     --name my-nginx \
     --publish target=80,published=80 \
     --replicas=5 \
     --network nginx-net \
     nginx
   ```

   The default publish mode of `ingress`, which is used when you do not specify a `mode` for the `--publish` flag, means that if you browse to port 80 on `manager`, `worker-1`, or `worker-2`, you will be connected to port 80 on one of the 5 service tasks, even if no tasks are currently running on the node you browse to. If you want to publish the port using `host` mode, you can add `mode=host` to the `--publish` output. However, you should also use `--mode global` instead of `--replicas=5` in this case, since only one service task can bind a given port on a given node.

   ​	`ingress` 的默认发布模式用于未指定 `--publish` 标志的 `mode` 时，表示当您在 `manager`、`worker-1` 或 `worker-2` 上访问端口 80 时，将连接到 5 个服务任务之一，即使当前节点上没有任务在运行。如果希望使用 `host` 模式发布端口，可以将 `mode=host` 添加到 `--publish` 参数中，但在这种情况下，应使用 `--mode global` 而不是 `--replicas=5`，因为在给定节点上只能有一个服务任务绑定到特定端口。

3. Run `docker service ls` to monitor the progress of service bring-up, which may take a few seconds. 运行 `docker service ls` 监视服务启动的进度，可能需要几秒钟。

4. Inspect the `nginx-net` network on `manager`, `worker-1`, and `worker-2`. Remember that you did not need to create it manually on `worker-1` and `worker-2` because Docker created it for you. The output will be long, but notice the `Containers` and `Peers` sections. `Containers` lists all service tasks (or standalone containers) connected to the overlay network from that host. 在 `manager`、`worker-1` 和 `worker-2` 上检查 `nginx-net` 网络。请记住，您无需在 `worker-1` 和 `worker-2` 上手动创建它，因为 Docker 已为您自动创建。输出会比较长，请注意 `Containers` 和 `Peers` 部分，`Containers` 列出该主机上连接到 overlay 网络的所有服务任务（或独立容器）。

5. From `manager`, inspect the service using `docker service inspect my-nginx` and notice the information about the ports and endpoints used by the service. 从 `manager` 使用 `docker service inspect my-nginx` 检查服务，注意服务使用的端口和端点信息。

6. Create a new network `nginx-net-2`, then update the service to use this network instead of `nginx-net`:

   创建一个新网络 `nginx-net-2`，然后更新服务以使用该网络而不是 `nginx-net`：

   ```console
   $ docker network create -d overlay nginx-net-2
   ```

   

   ```console
   $ docker service update \
     --network-add nginx-net-2 \
     --network-rm nginx-net \
     my-nginx
   ```

7. Run `docker service ls` to verify that the service has been updated and all tasks have been redeployed. Run `docker network inspect nginx-net` to verify that no containers are connected to it. Run the same command for `nginx-net-2` and notice that all the service task containers are connected to it. 运行 `docker service ls` 验证服务已更新并重新部署所有任务。运行 `docker network inspect nginx-net` 验证没有容器连接到它。对 `nginx-net-2` 执行相同命令，并注意所有服务任务容器已连接到它。

   > **Note**
   >
   > 
   >
   > Even though overlay networks are automatically created on swarm worker nodes as needed, they are not automatically removed.
   >
   > ​	虽然根据需要在 swarm 工作节点上自动创建 overlay 网络，但它们不会自动移除。

8. Clean up the service and the networks. From `manager`, run the following commands. The manager will direct the workers to remove the networks automatically.

   清理服务和网络。在 `manager` 上运行以下命令。管理节点将指示工作节点自动删除这些网络。

   ```console
   $ docker service rm my-nginx
   $ docker network rm nginx-net nginx-net-2
   ```

## 使用用户定义的 overlay 网络 Use a user-defined overlay network

### 前置条件 Prerequisites

This tutorial assumes the swarm is already set up and you are on a manager.

​	本教程假设 swarm 已经设置完毕，并且您在管理节点上。

### 操作步骤 Walkthrough

1. Create the user-defined overlay network.

   创建用户定义的 overlay 网络。

   ```console
   $ docker network create -d overlay my-overlay
   ```

2. Start a service using the overlay network and publishing port 80 to port 8080 on the Docker host.

   使用 overlay 网络启动服务，并将端口 80 发布到 Docker 主机的端口 8080。

   ```console
   $ docker service create \
     --name my-nginx \
     --network my-overlay \
     --replicas 1 \
     --publish published=8080,target=80 \
     nginx:latest
   ```

3. Run `docker network inspect my-overlay` and verify that the `my-nginx` service task is connected to it, by looking at the `Containers` section. 运行 `docker network inspect my-overlay`，在 `Containers` 部分验证 `my-nginx` 服务任务是否连接到网络。

4. Remove the service and the network.

   删除服务和网络。

   ```console
   $ docker service rm my-nginx
   
   $ docker network rm my-overlay
   ```

## 为独立容器使用 Overlay 网络 Use an overlay network for standalone containers

This example demonstrates DNS container discovery -- specifically, how to communicate between standalone containers on different Docker daemons using an overlay network. Steps are:

​	该示例演示了 DNS 容器发现，具体为如何使用 overlay 网络在不同 Docker 守护进程上的独立容器之间通信。步骤如下：

- On `host1`, initialize the node as a swarm (manager).
  - 在 `host1` 上，将节点初始化为 swarm（管理节点）。

- On `host2`, join the node to the swarm (worker).
  - 在 `host2` 上，将节点加入到 swarm（工作节点）。

- On `host1`, create an attachable overlay network (`test-net`).
  - 在 `host1` 上创建一个可连接的 overlay 网络 (`test-net`)。

- On `host1`, run an interactive [alpine](https://hub.docker.com/_/alpine/) container (`alpine1`) on `test-net`.
  - 在 `host1` 上运行一个连接到 `test-net` 的交互式 [alpine](https://hub.docker.com/_/alpine/) 容器 (`alpine1`)。

- On `host2`, run an interactive, and detached, [alpine](https://hub.docker.com/_/alpine/) container (`alpine2`) on `test-net`.
  - 在 `host2` 上运行一个连接到 `test-net` 的交互式、分离模式 [alpine](https://hub.docker.com/_/alpine/) 容器 (`alpine2`)。

- On `host1`, from within a session of `alpine1`, ping `alpine2`.
  - 在 `host1` 上，从 `alpine1` 会话中 ping `alpine2`。

### 前置条件 Prerequisites

For this test, you need two different Docker hosts that can communicate with each other. Each host must have the following ports open between the two Docker hosts:

​	本测试需要两个能够相互通信的不同 Docker 主机。每个主机必须在两台 Docker 主机之间开放以下端口：

- TCP port 2377
- TCP and UDP port 7946
- UDP port 4789

One easy way to set this up is to have two VMs (either local or on a cloud provider like AWS), each with Docker installed and running. If you're using AWS or a similar cloud computing platform, the easiest configuration is to use a security group that opens all incoming ports between the two hosts and the SSH port from your client's IP address.

​	一种简便方法是设置两台带有 Docker 的虚拟机（本地或云平台如 AWS）。如果使用 AWS 或类似的云计算平台，最简单的配置是使用一个安全组，打开两台主机之间的所有传入端口和来自客户端 IP 地址的 SSH 端口。

This example refers to the two nodes in our swarm as `host1` and `host2`. This example also uses Linux hosts, but the same commands work on Windows.

​	本示例将 swarm 中的两个节点称为 `host1` 和 `host2`。本示例使用 Linux 主机，但相同的命令在 Windows 上也能运行。

### 操作步骤 Walk-through

1. Set up the swarm. 设置 swarm。

   a. On `host1`, initialize a swarm (and if prompted, use `--advertise-addr` to specify the IP address for the interface that communicates with other hosts in the swarm, for instance, the private IP address on AWS):

   在 `host1` 上初始化 swarm（如有提示，使用 `--advertise-addr` 指定用于与 swarm 中其他主机通信的接口 IP 地址，例如 AWS 的私有 IP 地址）：

   ```console
   $ docker swarm init
   Swarm initialized: current node (vz1mm9am11qcmo979tlrlox42) is now a manager.
   
   To add a worker to this swarm, run the following command:
   
       docker swarm join --token SWMTKN-1-5g90q48weqrtqryq4kj6ow0e8xm9wmv9o6vgqc5j320ymybd5c-8ex8j0bc40s6hgvy5ui5gl4gy 172.31.47.252:2377
   
   To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
   ```

   b. On `host2`, join the swarm as instructed above:

   在 `host2` 上按照提示加入 swarm：

   ```console
   $ docker swarm join --token <your_token> <your_ip_address>:2377
   This node joined a swarm as a worker.
   ```

   If the node fails to join the swarm, the `docker swarm join` command times out. To resolve, run `docker swarm leave --force` on `host2`, verify your network and firewall settings, and try again.

   ​	如果节点无法加入 swarm，`docker swarm join` 命令会超时。为解决该问题，请在 `host2` 上运行 `docker swarm leave --force`，检查您的网络和防火墙设置，然后重试。

2. On `host1`, create an attachable overlay network called `test-net`:

   在 `host1` 上创建一个可连接的 overlay 网络 `test-net`：

   ```console
   $ docker network create --driver=overlay --attachable test-net
   uqsof8phj3ak0rq9k86zta6ht
   ```

   > Notice the returned **NETWORK ID** -- you will see it again when you connect to it from `host2`.
   >
   > ​	注意返回的 **NETWORK ID**——当您从 `host2` 连接到它时，您将再次看到它。

3. On `host1`, start an interactive (`-it`) container (`alpine1`) that connects to `test-net`:

   在 `host1` 上启动一个连接到 `test-net` 的交互式 (`-it`) 容器 (`alpine1`)：

   ```console
   $ docker run -it --name alpine1 --network test-net alpine
   / #
   ```

4. On `host2`, list the available networks -- notice that `test-net` does not yet exist:

   在 `host2` 上列出可用网络——请注意 `test-net` 尚未存在：

   ```console
   $ docker network ls
   NETWORK ID          NAME                DRIVER              SCOPE
   ec299350b504        bridge              bridge              local
   66e77d0d0e9a        docker_gwbridge     bridge              local
   9f6ae26ccb82        host                host                local
   omvdxqrda80z        ingress             overlay             swarm
   b65c952a4b2b        none                null                local
   ```

5. On `host2`, start a detached (`-d`) and interactive (`-it`) container (`alpine2`) that connects to `test-net`:

   在 `host2` 上启动一个连接到 `test-net` 的分离模式 (`-d`) 和交互式 (`-it`) 容器 (`alpine2`)：

   ```console
   $ docker run -dit --name alpine2 --network test-net alpine
   fb635f5ece59563e7b8b99556f816d24e6949a5f6a5b1fbd92ca244db17a4342
   ```

   > **Note**
   >
   > 
   >
   > Automatic DNS container discovery only works with unique container names.
   >
   > ​	自动 DNS 容器发现仅适用于唯一的容器名称。

6. On `host2`, verify that `test-net` was created (and has the same NETWORK ID as `test-net` on `host1`):

   在 `host2` 上验证 `test-net` 已创建（且其 NETWORK ID 与 `host1` 上的相同）：

   ```console
   $ docker network ls
   NETWORK ID          NAME                DRIVER              SCOPE
   ...
   uqsof8phj3ak        test-net            overlay             swarm
   ```

7. On `host1`, ping `alpine2` within the interactive terminal of `alpine1`:

   在 `host1` 中的 `alpine1` 交互式终端内 ping `alpine2`：

   ```console
   / # ping -c 2 alpine2
   PING alpine2 (10.0.0.5): 56 data bytes
   64 bytes from 10.0.0.5: seq=0 ttl=64 time=0.600 ms
   64 bytes from 10.0.0.5: seq=1 ttl=64 time=0.555 ms
   
   --- alpine2 ping statistics ---
   2 packets transmitted, 2 packets received, 0% packet loss
   round-trip min/avg/max = 0.555/0.577/0.600 ms
   ```

   The two containers communicate with the overlay network connecting the two hosts. If you run another alpine container on `host2` that is *not detached*, you can ping `alpine1` from `host2` (and here we add the [remove option](https://docs.docker.com/reference/cli/docker/container/run/#rm) for automatic container cleanup):

   这两个容器通过连接两台主机的 overlay 网络进行通信。如果您在 `host2` 上启动另一个不使用分离模式的 alpine 容器，您可以从 `host2` ping `alpine1`（这里我们添加 [删除选项](https://docs.docker.com/reference/cli/docker/container/run/#rm) 以自动清理容器）：

   ```sh
   $ docker run -it --rm --name alpine3 --network test-net alpine
   / # ping -c 2 alpine1
   / # exit
   ```

8. On `host1`, close the `alpine1` session (which also stops the container):

   在 `host1` 上关闭 `alpine1` 会话（这也会停止容器）：

   ```console
   / # exit
   ```

9. Clean up your containers and networks: 清理您的容器和网络：

   You must stop and remove the containers on each host independently because Docker daemons operate independently and these are standalone containers. You only have to remove the network on `host1` because when you stop `alpine2` on `host2`, `test-net` disappears.

   ​	您必须在每个主机上独立停止并删除容器，因为 Docker 守护进程是独立操作的，而这些是独立容器。您只需在 `host1` 上删除网络，因为当您在 `host2` 上停止 `alpine2` 时，`test-net` 会消失。

   a. On `host2`, stop `alpine2`, check that `test-net` was removed, then remove `alpine2`:

   在 `host2` 上，停止 `alpine2`，检查 `test-net` 已被移除，然后删除 `alpine2`：
   
   ```console
   $ docker container stop alpine2
   $ docker network ls
   $ docker container rm alpine2
   ```

   a. On `host1`, remove `alpine1` and `test-net`:

   在 `host1` 上，删除 `alpine1` 和 `test-net`：
   
   ```console
   $ docker container rm alpine1
   $ docker network rm test-net
   ```

## 其他网络教程 Other networking tutorials

- [Host networking tutorial](https://docs.docker.com/engine/network/tutorials/host/)
- [Standalone networking tutorial](https://docs.docker.com/engine/network/tutorials/standalone/)
- [Macvlan networking tutorial](https://docs.docker.com/engine/network/tutorials/macvlan/)
