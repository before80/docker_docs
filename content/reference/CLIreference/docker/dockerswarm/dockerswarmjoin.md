+++
title = "docker swarm join"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/swarm/join/](https://docs.docker.com/reference/cli/docker/swarm/join/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker swarm join

| Description | Join a swarm as a node and/or manager   |
| :---------- | --------------------------------------- |
| Usage       | `docker swarm join [OPTIONS] HOST:PORT` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 协调器。

## Description

Join a node to a swarm. The node joins as a manager node or worker node based upon the token you pass with the `--token` flag. If you pass a manager token, the node joins as a manager. If you pass a worker token, the node joins as a worker.

​	将节点加入 Swarm。节点将根据 `--token` 标志传递的令牌作为管理节点或工作节点加入。如果传递管理令牌，节点将作为管理节点加入；如果传递工作令牌，节点将作为工作节点加入。

## Options

| Option             | Default        | Description                                                  |
| ------------------ | -------------- | ------------------------------------------------------------ |
| `--advertise-addr` |                | 公告地址  Advertised address (format: `<ip|interface>[:port]`) |
| `--availability`   | `active`       | 节点的可用性（`active`、`pause`、`drain`） Availability of the node (`active`, `pause`, `drain`) |
| `--data-path-addr` |                | API 1.31+ 用于数据路径流量的地址或接口 API 1.31+ Address or interface to use for data path traffic (format: `<ip|interface>`) |
| `--listen-addr`    | `0.0.0.0:2377` | 监听地址 Listen address (format: `<ip|interface>[:port]`)    |
| `--token`          |                | 进入 Swarm 的令牌 Token for entry into the swarm             |

## Examples

### 将节点作为管理节点加入 Swarm Join a node to swarm as a manager

The example below demonstrates joining a manager node using a manager token.

​	以下示例展示了使用管理令牌加入管理节点：



```console
$ docker swarm join --token SWMTKN-1-3pu6hszjas19xyp7ghgosyx9k8atbfcr8p2is99znpy26u2lkl-7p73s1dx5in4tatdymyhg9hu2 192.168.99.121:2377
This node joined a swarm as a manager.

$ docker node ls
ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
dkp8vy1dq1kxleu9g4u78tlag *  manager2  Ready   Active        Reachable
dvfxp4zseq4s0rih1selh0d20    manager1  Ready   Active        Leader
```

A cluster should only have 3-7 managers at most, because a majority of managers must be available for the cluster to function. Nodes that aren't meant to participate in this management quorum should join as workers instead. Managers should be stable hosts that have static IP addresses.

​	一个集群应该最多只有 3-7 个管理节点，因为集群要正常运行需要大多数管理节点在线。不参与管理仲裁的节点应作为工作节点加入。管理节点应是具有静态 IP 地址的稳定主机。

### 将节点作为工作节点加入 Swarm - Join a node to swarm as a worker

The example below demonstrates joining a worker node using a worker token.

​	以下示例展示了使用工作令牌加入工作节点：



```console
$ docker swarm join --token SWMTKN-1-3pu6hszjas19xyp7ghgosyx9k8atbfcr8p2is99znpy26u2lkl-1awxwuwd3z9j1z3puu7rcgdbx 192.168.99.121:2377
This node joined a swarm as a worker.

$ docker node ls
ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
7ln70fl22uw2dvjn2ft53m3q5    worker2   Ready   Active
dkp8vy1dq1kxleu9g4u78tlag    worker1   Ready   Active        Reachable
dvfxp4zseq4s0rih1selh0d20 *  manager1  Ready   Active        Leader
```

### `--listen-addr value`

If the node is a manager, it will listen for inbound swarm manager traffic on this address. The default is to listen on 0.0.0.0:2377. It is also possible to specify a network interface to listen on that interface's address; for example `--listen-addr eth0:2377`.

​	如果节点是管理节点，它将使用此地址监听入站的 Swarm 管理流量。默认情况下，监听地址为 0.0.0.0:2377。也可以指定一个网络接口，以在该接口的地址上监听；例如 `--listen-addr eth0:2377`。

Specifying a port is optional. If the value is a bare IP address, or interface name, the default port 2377 will be used.

​	指定端口是可选的。如果值为纯 IP 地址或接口名称，将使用默认端口 2377。

This flag is generally not necessary when joining an existing swarm.

​	在加入现有 Swarm 时通常不需要此标志。

### `--advertise-addr value`

This flag specifies the address that will be advertised to other members of the swarm for API access. If unspecified, Docker will check if the system has a single IP address, and use that IP address with the listening port (see `--listen-addr`). If the system has multiple IP addresses, `--advertise-addr` must be specified so that the correct address is chosen for inter-manager communication and overlay networking.

​	此标志指定将向 Swarm 其他成员公告的地址，用于 API 访问。如果未指定，Docker 将检查系统是否只有一个 IP 地址，并使用该 IP 地址和监听端口（见 `--listen-addr`）。如果系统有多个 IP 地址，则必须指定 `--advertise-addr` 以选择正确的地址用于管理节点间的通信和覆盖网络。

It is also possible to specify a network interface to advertise that interface's address; for example `--advertise-addr eth0:2377`.

​	也可以指定一个网络接口，以公告该接口的地址；例如 `--advertise-addr eth0:2377`。

Specifying a port is optional. If the value is a bare IP address, or interface name, the default port 2377 will be used.

​	指定端口是可选的。如果值为纯 IP 地址或接口名称，将使用默认端口 2377。

This flag is generally not necessary when joining an existing swarm. If you're joining new nodes through a load balancer, you should use this flag to ensure the node advertises its IP address and not the IP address of the load balancer.

​	在加入现有 Swarm 时通常不需要此标志。如果通过负载均衡器加入新节点，应使用此标志以确保节点公告的是其 IP 地址，而不是负载均衡器的 IP 地址。

### `--data-path-addr`

This flag specifies the address that global scope network drivers will publish towards other nodes in order to reach the containers running on this node. Using this parameter it is then possible to separate the container's data traffic from the management traffic of the cluster. If unspecified, Docker will use the same IP address or interface that is used for the advertise address.

​	此标志指定全局范围网络驱动程序将向其他节点发布的地址，以便到达该节点上运行的容器。使用此参数，可以将容器的数据流量与集群的管理流量分开。如果未指定，Docker 将使用公告地址的相同 IP 地址或接口。

### `--token string`

Secret value required for nodes to join the swarm

​	加入 Swarm 所需的密钥

### `--availability`

This flag specifies the availability of the node at the time the node joins a master. Possible availability values are `active`, `pause`, or `drain`.

​	此标志指定节点加入主机时的可用性。可能的可用性值为 `active`、`pause` 或 `drain`。

This flag is useful in certain situations. For example, a cluster may want to have dedicated manager nodes that are not served as worker nodes. This could be achieved by passing `--availability=drain` to `docker swarm join`.

​	此标志在某些情况下非常有用。例如，一个集群可能希望拥有不作为工作节点的专用管理节点。可以通过向 `docker swarm join` 传递 `--availability=drain` 来实现。
