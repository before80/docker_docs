+++
title = "在 Swarm 模式下运行 Docker 引擎"
date = 2024-10-23T14:54:40+08:00
weight = 110
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/swarm-mode/](https://docs.docker.com/engine/swarm/swarm-mode/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Run Docker Engine in swarm mode - 在 Swarm 模式下运行 Docker 引擎

When you first install and start working with Docker Engine, Swarm mode is disabled by default. When you enable Swarm mode, you work with the concept of services managed through the `docker service` command.

​	首次安装并开始使用 Docker 引擎时，Swarm 模式默认处于禁用状态。当启用 Swarm 模式后，可以使用 `docker service` 命令管理服务的概念。

There are two ways to run the engine in Swarm mode:

​	有两种方式可以在 Swarm 模式下运行引擎：

- Create a new swarm, covered in this article.
  - 创建一个新的 Swarm（本教程将涵盖此内容）。

- [Join an existing swarm]({{< ref "/manuals/DockerEngine/Swarmmode/Joinnodestoaswarm" >}}).
  - [加入现有的 Swarm]({{< ref "/manuals/DockerEngine/Swarmmode/Joinnodestoaswarm" >}})。


When you run the engine in Swarm mode on your local machine, you can create and test services based upon images you've created or other available images. In your production environment, Swarm mode provides a fault-tolerant platform with cluster management features to keep your services running and available.

​	在本地机器上以 Swarm 模式运行引擎时，可以基于自己创建的镜像或其他可用镜像创建和测试服务。在生产环境中，Swarm 模式提供了一个具有集群管理功能的容错平台，确保服务的运行和可用性。

These instructions assume you have installed the Docker Engine on a machine to serve as a manager node in your swarm.

​	本教程假设您已在一台机器上安装了 Docker 引擎，并准备将其作为 Swarm 的管理节点。

If you haven't already, read through the [Swarm mode key concepts]({{< ref "/manuals/DockerEngine/Swarmmode/Swarmmodekeyconcepts" >}}) and try the [Swarm mode tutorial]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode" >}}).

​	如果尚未阅读，请先浏览 [Swarm 模式的关键概念]({{< ref "/manuals/DockerEngine/Swarmmode/Swarmmodekeyconcepts" >}})并尝试 [Swarm 模式教程]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode" >}})。

## 创建 Swarm - Create a swarm

When you run the command to create a swarm, Docker Engine starts running in Swarm mode.

​	运行创建 Swarm 的命令后，Docker 引擎将开始以 Swarm 模式运行。

Run [`docker swarm init`]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarminit" >}}) to create a single-node swarm on the current node. The engine sets up the swarm as follows:

​	使用 [`docker swarm init`]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarminit" >}}) 命令在当前节点上创建一个单节点 Swarm。引擎会按如下方式设置 Swarm：

- Switches the current node into Swarm mode. 
  - 将当前节点切换到 Swarm 模式。

- Creates a swarm named `default`. 
  - 创建一个名为 `default` 的 Swarm。

- Designates the current node as a leader manager node for the swarm. 
  - 将当前节点指定为 Swarm 的领导管理节点。

- Names the node with the machine hostname. 
  - 使用机器的主机名命名节点。

- Configures the manager to listen on an active network interface on port `2377`. 
  - 配置管理节点在活动网络接口上的端口 `2377` 上监听。

- Sets the current node to `Active` availability, meaning it can receive tasks from the scheduler. 
  - 将当前节点设置为 `Active` 可用状态，表示可以接收调度程序的任务。

- Starts an internal distributed data store for Engines participating in the swarm to maintain a consistent view of the swarm and all services running on it.
  - 为参与 Swarm 的引擎启动一个内部分布式数据存储，以保持 Swarm 和所有运行服务的视图一致。

- By default, generates a self-signed root CA for the swarm.
  - 默认情况下，为 Swarm 生成一个自签名的根 CA。

- By default, generates tokens for worker and manager nodes to join the swarm.
  - 默认情况下，为加入 Swarm 的工作节点和管理节点生成令牌。

- Creates an overlay network named `ingress` for publishing service ports external to the swarm.
  - 创建一个名为 `ingress` 的覆盖网络，用于在 Swarm 之外发布服务端口。

- Creates an overlay default IP addresses and subnet mask for your networks
  - 为网络创建覆盖默认 IP 地址和子网掩码。


The output for `docker swarm init` provides the connection command to use when you join new worker nodes to the swarm:

​	`docker swarm init` 的输出提供了在加入新的工作节点时使用的连接命令：

```console
$ docker swarm init
Swarm initialized: current node (dxn1zf6l61qsb1josjja83ngz) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
    192.168.99.100:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

### 配置默认地址池 Configuring default address pools

By default Swarm mode uses a default address pool `10.0.0.0/8` for global scope (overlay) networks. Every network that does not have a subnet specified will have a subnet sequentially allocated from this pool. In some circumstances it may be desirable to use a different default IP address pool for networks.

​	默认情况下，Swarm 模式使用 `10.0.0.0/8` 作为全局范围（覆盖）网络的默认地址池。没有指定子网的每个网络将从该池中顺序分配一个子网。在某些情况下，可能需要为网络使用不同的默认 IP 地址池。

For example, if the default `10.0.0.0/8` range conflicts with already allocated address space in your network, then it is desirable to ensure that networks use a different range without requiring swarm users to specify each subnet with the `--subnet` command.

​	例如，如果默认的 `10.0.0.0/8` 范围与您网络中已经分配的地址空间冲突，则最好确保网络使用不同的范围，而不需要 Swarm 用户为每个子网指定 `--subnet` 命令。

To configure custom default address pools, you must define pools at swarm initialization using the `--default-addr-pool` command line option. This command line option uses CIDR notation for defining the subnet mask. To create the custom address pool for Swarm, you must define at least one default address pool, and an optional default address pool subnet mask. For example, for the `10.0.0.0/27`, use the value `27`.

​	要配置自定义的默认地址池，您必须在 Swarm 初始化时使用 `--default-addr-pool` 命令行选项定义池。此选项使用 CIDR 表示法来定义子网掩码。要为 Swarm 创建自定义地址池，必须至少定义一个默认地址池，以及可选的默认地址池子网掩码。例如，对于 `10.0.0.0/27`，使用值 `27`。

Docker allocates subnet addresses from the address ranges specified by the `--default-addr-pool` option. For example, a command line option `--default-addr-pool 10.10.0.0/16` indicates that Docker will allocate subnets from that `/16` address range. If `--default-addr-pool-mask-len` were unspecified or set explicitly to 24, this would result in 256 `/24` networks of the form `10.10.X.0/24`.

​	Docker 会根据 `--default-addr-pool` 选项中指定的地址范围分配子网地址。例如，命令行选项 `--default-addr-pool 10.10.0.0/16` 表示 Docker 将从该 `/16` 地址范围分配子网。如果未指定 `--default-addr-pool-mask-len` 或显式设置为 24，则将生成 256 个形式为 `10.10.X.0/24` 的 `/24` 网络。

The subnet range comes from the `--default-addr-pool`, (such as `10.10.0.0/16`). The size of 16 there represents the number of networks one can create within that `default-addr-pool` range. The `--default-addr-pool` option may occur multiple times with each option providing additional addresses for docker to use for overlay subnets.

​	子网范围来自 `--default-addr-pool`（例如 `10.10.0.0/16`）。16 表示可以在该 `default-addr-pool` 范围内创建的网络数量。`--default-addr-pool` 选项可以多次出现，每个选项为 Docker 提供更多的覆盖子网地址。

The format of the command is:

​	命令格式如下：

```console
$ docker swarm init --default-addr-pool <IP range in CIDR> [--default-addr-pool <IP range in CIDR> --default-addr-pool-mask-length <CIDR value>]
```

The command to create a default IP address pool with a /16 (class B) for the `10.20.0.0` network looks like this:

​	创建一个默认 IP 地址池为 `10.20.0.0` 网络的 `/16`（B 类）子网的命令如下：	

```console
$ docker swarm init --default-addr-pool 10.20.0.0/16
```

The command to create a default IP address pool with a `/16` (class B) for the `10.20.0.0` and `10.30.0.0` networks, and to create a subnet mask of `/26` for each network looks like this:

​	如果需要为 `10.20.0.0` 和 `10.30.0.0` 网络创建 `/16`（B 类）默认 IP 地址池，并为每个网络创建 `/26` 子网掩码，则命令如下：

```console
$ docker swarm init --default-addr-pool 10.20.0.0/16 --default-addr-pool 10.30.0.0/16 --default-addr-pool-mask-length 26
```

In this example, `docker network create -d overlay net1` will result in `10.20.0.0/26` as the allocated subnet for `net1`, and `docker network create -d overlay net2` will result in `10.20.0.64/26` as the allocated subnet for `net2`. This continues until all the subnets are exhausted.

​	在此示例中，`docker network create -d overlay net1` 将为 `net1` 分配 `10.20.0.0/26` 子网，而 `docker network create -d overlay net2` 将为 `net2` 分配 `10.20.0.64/26` 子网。此分配将继续进行，直到所有子网被耗尽。

Refer to the following pages for more information:

​	有关更多信息，请参阅以下页面：

- [Swarm networking]({{< ref "/manuals/DockerEngine/Swarmmode/Manageswarmservicenetworks" >}}) for more information about the default address pool usage
  - [Swarm networking]({{< ref "/manuals/DockerEngine/Swarmmode/Manageswarmservicenetworks" >}})，了解默认地址池的使用

- `docker swarm init` [CLI reference]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarminit" >}}) for more detail on the `--default-addr-pool` flag.
  - `docker swarm init` [CLI 参考]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarminit" >}})，获取 `--default-addr-pool` 标志的更多细节。


### 配置广告地址 Configure the advertise address

Manager nodes use an advertise address to allow other nodes in the swarm access to the Swarmkit API and overlay networking. The other nodes on the swarm must be able to access the manager node on its advertise address.

​	管理节点使用广告地址让群集中的其他节点可以访问 Swarmkit API 和覆盖网络。群集中的其他节点必须能够通过广告地址访问管理节点。

If you don't specify an advertise address, Docker checks if the system has a single IP address. If so, Docker uses the IP address with the listening port `2377` by default. If the system has multiple IP addresses, you must specify the correct `--advertise-addr` to enable inter-manager communication and overlay networking:

​	如果不指定广告地址，Docker 将检查系统是否只有一个 IP 地址。如果只有一个 IP 地址，Docker 将默认使用该 IP 地址并监听端口 `2377`。如果系统有多个 IP 地址，则需要指定正确的 `--advertise-addr`，以启用管理节点间的通信和覆盖网络：



```console
$ docker swarm init --advertise-addr <MANAGER-IP>
```

You must also specify the `--advertise-addr` if the address where other nodes reach the first manager node is not the same address the manager sees as its own. For instance, in a cloud setup that spans different regions, hosts have both internal addresses for access within the region and external addresses that you use for access from outside that region. In this case, specify the external address with `--advertise-addr` so that the node can propagate that information to other nodes that subsequently connect to it.

​	如果其他节点访问第一个管理节点的地址与管理节点自身看到的地址不同，您也需要指定 `--advertise-addr`。例如，在跨区域的云设置中，主机既有用于区域内访问的内部地址，也有用于区域外访问的外部地址。在这种情况下，指定外部地址作为 `--advertise-addr`，使节点能够将该信息传播给后续连接的其他节点。

Refer to the `docker swarm init` [CLI reference]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarminit" >}}) for more detail on the advertise address.

​	请参阅 `docker swarm init` [CLI 参考]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarminit" >}})，了解广告地址的更多细节。

### 查看加入命令或更新群集加入令牌 View the join command or update a swarm join token

Nodes require a secret token to join the swarm. The token for worker nodes is different from the token for manager nodes. Nodes only use the join-token at the moment they join the swarm. Rotating the join token after a node has already joined a swarm does not affect the node's swarm membership. Token rotation ensures an old token cannot be used by any new nodes attempting to join the swarm.

​	节点需要一个秘密令牌才能加入群集。工作节点的令牌与管理节点的令牌不同。节点仅在加入群集时使用加入令牌。在节点加入群集后旋转加入令牌不会影响节点的群集成员资格。旋转令牌可以确保旧令牌不能被任何新的节点用于加入群集。

To retrieve the join command including the join token for worker nodes, run:

​	要检索包含工作节点加入令牌的加入命令，请运行：

```console
$ docker swarm join-token worker

To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
    192.168.99.100:2377

This node joined a swarm as a worker.
```

To view the join command and token for manager nodes, run:

​	要查看管理节点的加入命令和令牌，请运行：

```console
$ docker swarm join-token manager

To add a manager to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-59egwe8qangbzbqb3ryawxzk3jn97ifahlsrw01yar60pmkr90-bdjfnkcflhooyafetgjod97sz \
    192.168.99.100:2377
```

Pass the `--quiet` flag to print only the token:

​	使用 `--quiet` 标志可以仅打印出令牌：

```console
$ docker swarm join-token --quiet worker

SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c
```

Be careful with the join tokens because they are the secrets necessary to join the swarm. In particular, checking a secret into version control is a bad practice because it would allow anyone with access to the application source code to add new nodes to the swarm. Manager tokens are especially sensitive because they allow a new manager node to join and gain control over the whole swarm.

​	请谨慎处理加入令牌，因为它们是加入群集所需的秘密。特别是，将秘密检入版本控制是一种不好的做法，因为这样任何访问应用程序源代码的人都可以将新节点添加到群集中。管理令牌尤其敏感，因为它们允许新管理节点加入并控制整个群集。

We recommend that you rotate the join tokens in the following circumstances:

​	我们建议在以下情况下旋转加入令牌：

- If a token was checked-in by accident into a version control system, group chat or accidentally printed to your logs.
  - 如果令牌意外检入到版本控制系统、群聊或意外打印到日志中。

- If you suspect a node has been compromised.
  - 如果怀疑节点受到威胁。

- If you wish to guarantee that no new nodes can join the swarm.
  - 如果希望确保没有新节点可以加入群集。


Additionally, it is a best practice to implement a regular rotation schedule for any secret including swarm join tokens. We recommend that you rotate your tokens at least every 6 months.

​	此外，最好对所有秘密（包括群集加入令牌）实施定期轮换计划。我们建议至少每 6 个月轮换一次令牌。

Run `swarm join-token --rotate` to invalidate the old token and generate a new token. Specify whether you want to rotate the token for `worker` or `manager` nodes:

​	运行 `swarm join-token --rotate` 以使旧令牌失效并生成新令牌。指定要旋转 `worker` 还是 `manager` 节点的令牌：

```console
$ docker swarm join-token  --rotate worker

To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-2kscvs0zuymrsc9t0ocyy1rdns9dhaodvpl639j2bqx55uptag-ebmn5u927reawo27s3azntd44 \
    192.168.99.100:2377
```

## 了解更多 Learn more

- [Join nodes to a swarm 将节点加入到 Swarm 集群]({{< ref "/manuals/DockerEngine/Swarmmode/Joinnodestoaswarm" >}})
- `swarm init` [command line reference]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarminit" >}})
- [Swarm mode tutorial Swarm 模式教程]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode" >}})
