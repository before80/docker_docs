+++
title = "管理 Swarm 服务网络"
date = 2024-10-23T14:54:40+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/networking/](https://docs.docker.com/engine/swarm/networking/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Manage swarm service networks - 管理 Swarm 服务网络

This page describes networking for swarm services.

​	本页面介绍了 Swarm 服务的网络。

## Swarm 和流量类型 Swarm and types of traffic

A Docker swarm generates two different kinds of traffic:

​	一个 Docker Swarm 会产生两种不同类型的流量：

- Control and management plane traffic: This includes swarm management messages, such as requests to join or leave the swarm. This traffic is always encrypted.
  - 控制和管理平面流量：包括 Swarm 管理消息，如加入或离开 Swarm 的请求。此流量始终加密。

- Application data plane traffic: This includes container traffic and traffic to and from external clients.
  - 应用数据平面流量：包括容器之间以及外部客户端的流量。


## 关键网络概念 Key network concepts

The following three network concepts are important to swarm services:

​	以下三个网络概念对 Swarm 服务很重要：

- Overlay networks manage communications among the Docker daemons participating in the swarm. You can create overlay networks, in the same way as user-defined networks for standalone containers. You can attach a service to one or more existing overlay networks as well, to enable service-to-service communication. Overlay networks are Docker networks that use the `overlay` network driver.
  - **Overlay 网络**：用于管理参与 Swarm 的 Docker 守护进程之间的通信。可以像为独立容器创建用户定义网络一样创建 Overlay 网络。也可以将服务附加到一个或多个现有的 Overlay 网络上，以启用服务间通信。Overlay 网络使用 `overlay` 网络驱动。

- The ingress network is a special overlay network that facilitates load balancing among a service's nodes. When any swarm node receives a request on a published port, it hands that request off to a module called `IPVS`. `IPVS` keeps track of all the IP addresses participating in that service, selects one of them, and routes the request to it, over the `ingress` network.

  - **Ingress 网络**：这是一个特殊的 Overlay 网络，用于在服务的节点之间实现负载均衡。当 Swarm 节点在一个发布的端口上收到请求时，会将请求传递给名为 `IPVS` 的模块。`IPVS` 跟踪该服务中所有参与的 IP 地址，从中选择一个并将请求通过 `ingress` 网络路由到它。


  The `ingress` network is created automatically when you initialize or join a swarm. Most users do not need to customize its configuration, but Docker allows you to do so.

  `ingress` 网络在初始化或加入 Swarm 时自动创建。大多数用户不需要自定义其配置，但 Docker 允许这么做。

- The docker_gwbridge is a bridge network that connects the overlay networks (including the `ingress` network) to an individual Docker daemon's physical network. By default, each container a service is running is connected to its local Docker daemon host's `docker_gwbridge` network.

  - **docker_gwbridge**：这是一个桥接网络，用于将 Overlay 网络（包括 `ingress` 网络）连接到各个 Docker 守护进程的物理网络。默认情况下，每个服务运行的容器都连接到本地 Docker 守护进程主机的 `docker_gwbridge` 网络。

  The `docker_gwbridge` network is created automatically when you initialize or join a swarm. Most users do not need to customize its configuration, but Docker allows you to do so.`docker_gwbridge` 网络在初始化或加入 Swarm 时自动创建。大多数用户不需要自定义其配置，但 Docker 允许这么做。


> **Tip**
>
> 
>
> See also [Networking overview]({{< ref "/manuals/DockerEngine/Networking" >}}) for more details about Swarm networking in general.
>
> ​	参阅 [网络概述]({{< ref "/manuals/DockerEngine/Networking" >}}) 以了解 Swarm 网络的更多信息。

## 防火墙注意事项 Firewall considerations

Docker daemons participating in a swarm need the ability to communicate with each other over the following ports:

​	参与 Swarm 的 Docker 守护进程需要能够通过以下端口相互通信：

- Port `7946` TCP/UDP for container network discovery.
  - 端口 `7946` 的 TCP/UDP 用于容器网络发现。

- Port `4789` UDP (configurable) for the overlay network (including ingress) data path.
  - 端口 `4789` 的 UDP（可配置）用于 Overlay 网络（包括 ingress）的数据路径。


When setting up networking in a Swarm, special care should be taken. Consult the [tutorial](https://docs.docker.com/engine/swarm/swarm-tutorial/#open-protocols-and-ports-between-the-hosts) for an overview.

​	在 Swarm 中设置网络时应特别注意。请参阅 [教程](https://docs.docker.com/engine/swarm/swarm-tutorial/#open-protocols-and-ports-between-the-hosts) 获取概述。

## Overlay 网络 Overlay networking

When you initialize a swarm or join a Docker host to an existing swarm, two new networks are created on that Docker host:

​	初始化 Swarm 或将 Docker 主机加入现有 Swarm 时，Docker 主机上会创建两个新网络：

- An overlay network called `ingress`, which handles the control and data traffic related to swarm services. When you create a swarm service and do not connect it to a user-defined overlay network, it connects to the `ingress` network by default.
  - 一个名为 `ingress` 的 Overlay 网络，处理与 Swarm 服务相关的控制和数据流量。如果创建 Swarm 服务且未将其连接到用户定义的 Overlay 网络，则默认连接到 `ingress` 网络。

- A bridge network called `docker_gwbridge`, which connects the individual Docker daemon to the other daemons participating in the swarm.
  - 一个名为 `docker_gwbridge` 的桥接网络，将各个 Docker 守护进程连接到参与 Swarm 的其他守护进程。


### 创建一个 Overlay 网络 Create an overlay network

To create an overlay network, specify the `overlay` driver when using the `docker network create` command:

​	要创建 Overlay 网络，在使用 `docker network create` 命令时指定 `overlay` 驱动：



```console
$ docker network create \
  --driver overlay \
  my-network
```

The above command doesn't specify any custom options, so Docker assigns a subnet and uses default options. You can see information about the network using `docker network inspect`.

​	上述命令未指定任何自定义选项，因此 Docker 分配一个子网并使用默认选项。可以使用 `docker network inspect` 查看网络信息。

When no containers are connected to the overlay network, its configuration is not very exciting:

​	当没有容器连接到 Overlay 网络时，其配置不会显示太多内容：



```console
$ docker network inspect my-network
[
    {
        "Name": "my-network",
        "Id": "fsf1dmx3i9q75an49z36jycxd",
        "Created": "0001-01-01T00:00:00Z",
        "Scope": "swarm",
        "Driver": "overlay",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": []
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "Containers": null,
        "Options": {
            "com.docker.network.driver.overlay.vxlanid_list": "4097"
        },
        "Labels": null
    }
]
```

In the above output, notice that the driver is `overlay` and that the scope is `swarm`, rather than `local`, `host`, or `global` scopes you might see in other types of Docker networks. This scope indicates that only hosts which are participating in the swarm can access this network.

​	在上述输出中，注意驱动程序为 `overlay`，范围为 `swarm`不是其他 Docker 网络中常见的 `local`、`host` 或 `global` 范围。该范围表示只有参与 Swarm 的主机才能访问此网络。

The network's subnet and gateway are dynamically configured when a service connects to the network for the first time. The following example shows the same network as above, but with three containers of a `redis` service connected to i

​	网络的子网和网关在首次有服务连接到网络时动态配置。以下示例显示了连接了三个 `redis` 服务容器的相同网络：



```console
$ docker network inspect my-network
[
    {
        "Name": "my-network",
        "Id": "fsf1dmx3i9q75an49z36jycxd",
        "Created": "2017-05-31T18:35:58.877628262Z",
        "Scope": "swarm",
        "Driver": "overlay",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "10.0.0.0/24",
                    "Gateway": "10.0.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "Containers": {
            "0e08442918814c2275c31321f877a47569ba3447498db10e25d234e47773756d": {
                "Name": "my-redis.1.ka6oo5cfmxbe6mq8qat2djgyj",
                "EndpointID": "950ce63a3ace13fe7ef40724afbdb297a50642b6d47f83a5ca8636d44039e1dd",
                "MacAddress": "02:42:0a:00:00:03",
                "IPv4Address": "10.0.0.3/24",
                "IPv6Address": ""
            },
            "88d55505c2a02632c1e0e42930bcde7e2fa6e3cce074507908dc4b827016b833": {
                "Name": "my-redis.2.s7vlybipal9xlmjfqnt6qwz5e",
                "EndpointID": "dd822cb68bcd4ae172e29c321ced70b731b9994eee5a4ad1d807d9ae80ecc365",
                "MacAddress": "02:42:0a:00:00:05",
                "IPv4Address": "10.0.0.5/24",
                "IPv6Address": ""
            },
            "9ed165407384f1276e5cfb0e065e7914adbf2658794fd861cfb9b991eddca754": {
                "Name": "my-redis.3.hbz3uk3hi5gb61xhxol27hl7d",
                "EndpointID": "f62c686a34c9f4d70a47b869576c37dffe5200732e1dd6609b488581634cf5d2",
                "MacAddress": "02:42:0a:00:00:04",
                "IPv4Address": "10.0.0.4/24",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.driver.overlay.vxlanid_list": "4097"
        },
        "Labels": {},
        "Peers": [
            {
                "Name": "moby-e57c567e25e2",
                "IP": "192.168.65.2"
            }
        ]
    }
]
```

### 自定义 Overlay 网络 Customize an overlay network

There may be situations where you don't want to use the default configuration for an overlay network. For a full list of configurable options, run the command `docker network create --help`. The following are some of the most common options to change.

​	在某些情况下，可能不希望使用 Overlay 网络的默认配置。要查看所有可配置的选项，请运行命令 `docker network create --help`。以下是一些常见的选项：

#### 配置子网和网关 Configure the subnet and gateway

By default, the network's subnet and gateway are configured automatically when the first service is connected to the network. You can configure these when creating a network using the `--subnet` and `--gateway` flags. The following example extends the previous one by configuring the subnet and gateway.



```console
$ docker network create \
  --driver overlay \
  --subnet 10.0.9.0/24 \
  --gateway 10.0.9.99 \
  my-network
```

##### 使用自定义默认地址池 Using custom default address pools

To customize subnet allocation for your Swarm networks, you can [optionally configure them]({{< ref "/manuals/DockerEngine/Swarmmode/RunDockerEngineinswarmmode" >}}) during `swarm init`.

​	若要为 Swarm 网络自定义子网分配，可以在 `swarm init` 时[选择性地配置它们]({{< ref "/manuals/DockerEngine/Swarmmode/RunDockerEngineinswarmmode" >}})。

For example, the following command is used when initializing Swarm:

​	例如，以下命令用于初始化 Swarm：

```console
$ docker swarm init --default-addr-pool 10.20.0.0/16 --default-addr-pool-mask-length 26
```

Whenever a user creates a network, but does not use the `--subnet` command line option, the subnet for this network will be allocated sequentially from the next available subnet from the pool. If the specified network is already allocated, that network will not be used for Swarm.

​	每当用户创建网络但未使用 `--subnet` 命令行选项时，该网络的子网将从池中下一个可用的子网中分配。若指定的网络已被分配，Swarm 将不会使用该网络。

Multiple pools can be configured if discontiguous address space is required. However, allocation from specific pools is not supported. Network subnets will be allocated sequentially from the IP pool space and subnets will be reused as they are deallocated from networks that are deleted.

​	如果需要不连续的地址空间，可以配置多个地址池。然而，暂不支持从特定池中进行分配。网络子网将按顺序从 IP 池空间中分配，并在网络删除后重新使用子网。

The default mask length can be configured and is the same for all networks. It is set to `/24` by default. To change the default subnet mask length, use the `--default-addr-pool-mask-length` command line option.

​	默认子网掩码长度可配置，适用于所有网络，默认为 `/24`。要更改默认子网掩码长度，请使用 `--default-addr-pool-mask-length` 命令行选项。

> **Note**
>
> 
>
> Default address pools can only be configured on `swarm init` and cannot be altered after cluster creation.
>
> ​	默认地址池只能在 `swarm init` 时配置，创建集群后无法更改。

##### Overlay 网络大小限制 Overlay network size limitations

Docker recommends creating overlay networks with `/24` blocks. The `/24` overlay network blocks limit the network to 256 IP addresses.

​	Docker 建议使用 `/24` 块创建 Overlay 网络。这种网络块大小限制了网络至 256 个 IP 地址。

This recommendation addresses [limitations with swarm mode](https://github.com/moby/moby/issues/30820). If you need more than 256 IP addresses, do not increase the IP block size. You can either use `dnsrr` endpoint mode with an external load balancer, or use multiple smaller overlay networks. See [Configure service discovery](https://docs.docker.com/engine/swarm/networking/#configure-service-discovery) for more information about different endpoint modes.

​	该建议是为了应对 [Swarm 模式的限制](https://github.com/moby/moby/issues/30820)。如果需要超过 256 个 IP 地址，请勿增加 IP 块大小。可以使用带有外部负载均衡器的 `dnsrr` 端点模式，或使用多个较小的 Overlay 网络。有关不同端点模式的更多信息，请参阅 [配置服务发现](https://docs.docker.com/engine/swarm/networking/#configure-service-discovery)。

#### 配置应用数据加密 Configure encryption of application data

Management and control plane data related to a swarm is always encrypted. For more details about the encryption mechanisms, see the [Docker swarm mode overlay network security model]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Overlaynetworkdriver" >}}).

​	Swarm 相关的管理和控制平面数据始终加密。有关加密机制的详细信息，请参阅 [Docker Swarm 模式 Overlay 网络安全模型]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Overlaynetworkdriver" >}})。

Application data among swarm nodes is not encrypted by default. To encrypt this traffic on a given overlay network, use the `--opt encrypted` flag on `docker network create`. This enables IPSEC encryption at the level of the vxlan. This encryption imposes a non-negligible performance penalty, so you should test this option before using it in production.

​	Swarm 节点之间的应用数据默认未加密。要在给定的 Overlay 网络上加密此类流量，请在 `docker network create` 命令中使用 `--opt encrypted` 标志。这将在 vxlan 层启用 IPSEC 加密。此加密会对性能产生一定影响，因此在生产环境中使用前应进行测试。

> **Note**
>
> 
>
> You must [customize the automatically created ingress](https://docs.docker.com/engine/swarm/networking/#customize-ingress) to enable encryption. By default, all ingress traffic is unencrypted, as encryption is a network-level option.
>
> ​	必须[自定义自动创建的 ingress](https://docs.docker.com/engine/swarm/networking/#customize-ingress) 才能启用加密。默认情况下，所有 ingress 流量不加密，因为加密是网络级选项。

## 将服务附加到 Overlay 网络 Attach a service to an overlay network

To attach a service to an existing overlay network, pass the `--network` flag to `docker service create`, or the `--network-add` flag to `docker service update`.

​	要将服务附加到现有的 Overlay 网络，请在 `docker service create` 中传递 `--network` 标志，或在 `docker service update` 中使用 `--network-add` 标志。



```console
$ docker service create \
  --replicas 3 \
  --name my-web \
  --network my-network \
  nginx
```

Service containers connected to an overlay network can communicate with each other across it.

​	连接到 Overlay 网络的服务容器可以通过网络彼此通信。

To see which networks a service is connected to, use `docker service ls` to find the name of the service, then `docker service ps <service-name>` to list the networks. Alternately, to see which services' containers are connected to a network, use `docker network inspect <network-name>`. You can run these commands from any swarm node which is joined to the swarm and is in a `running` state.

​	要查看服务连接了哪些网络，可以使用 `docker service ls` 找到服务的名称，然后使用 `docker service ps <service-name>` 列出网络。或者，要查看网络中连接了哪些服务容器，可以使用 `docker network inspect <network-name>`。可以从任何加入 Swarm 且处于 `running` 状态的节点运行这些命令。

### 配置服务发现 Configure service discovery

Service discovery is the mechanism Docker uses to route a request from your service's external clients to an individual swarm node, without the client needing to know how many nodes are participating in the service or their IP addresses or ports. You don't need to publish ports which are used between services on the same network. For instance, if you have a [WordPress service that stores its data in a MySQL service](https://training.play-with-docker.com/swarm-service-discovery/), and they are connected to the same overlay network, you do not need to publish the MySQL port to the client, only the WordPress HTTP port.

​	服务发现是 Docker 用于将外部客户端的请求路由到单个 Swarm 节点的机制，客户端不需要知道参与服务的节点数量或其 IP 地址或端口。在同一网络上的服务之间使用的端口无需发布。例如，如果有一个 [使用 MySQL 服务存储数据的 WordPress 服务](https://training.play-with-docker.com/swarm-service-discovery/)，并且它们连接到同一 Overlay 网络，则无需向客户端发布 MySQL 端口，只需发布 WordPress 的 HTTP 端口。

Service discovery can work in two different ways: internal connection-based load-balancing at Layers 3 and 4 using the embedded DNS and a virtual IP (VIP), or external and customized request-based load-balancing at Layer 7 using DNS round robin (DNSRR). You can configure this per service.

​	服务发现可以通过两种不同的方式工作：基于内部连接的第 3 层和第 4 层负载均衡（使用嵌入的 DNS 和虚拟 IP (VIP)），或者基于请求的第 7 层负载均衡（使用 DNS 轮询 (DNSRR)）。可以为每个服务进行配置。

- By default, when you attach a service to a network and that service publishes one or more ports, Docker assigns the service a virtual IP (VIP), which is the "front end" for clients to reach the service. Docker keeps a list of all worker nodes in the service, and routes requests between the client and one of the nodes. Each request from the client might be routed to a different node.
  - 默认情况下，当将服务附加到网络并且该服务发布一个或多个端口时，Docker 会为该服务分配一个虚拟 IP (VIP)，客户端通过该 IP 访问服务。Docker 会保留服务中所有工作节点的列表，并在客户端和某个节点之间路由请求。客户端的每个请求可能会被路由到不同的节点。

- If you configure a service to use DNS round-robin (DNSRR) service discovery, there is not a single virtual IP. Instead, Docker sets up DNS entries for the service such that a DNS query for the service name returns a list of IP addresses, and the client connects directly to one of these.

  - 如果配置服务使用 DNS 轮询 (DNSRR) 服务发现，则不会有单一的虚拟 IP。相反，Docker 会为服务设置 DNS 条目，使得对服务名称的 DNS 查询返回 IP 地址列表，客户端直接连接到其中一个地址。
  
  DNS round-robin is useful in cases where you want to use your own load balancer, such as HAProxy. To configure a service to use DNSRR, use the flag `--endpoint-mode dnsrr` when creating a new service or updating an existing one. 
  
  DNS 轮询适用于希望使用自定义负载均衡器（如 HAProxy）的情况。要配置服务使用 DNSRR，请在创建新服务或更新现有服务时使用 `--endpoint-mode dnsrr` 标志。

## 自定义 ingress 网络 Customize the ingress network

Most users never need to configure the `ingress` network, but Docker allows you to do so. This can be useful if the automatically-chosen subnet conflicts with one that already exists on your network, or you need to customize other low-level network settings such as the MTU, or if you want to [enable encryption](https://docs.docker.com/engine/swarm/networking/#encryption).

​	大多数用户不需要配置 `ingress` 网络，但 Docker 允许这么做。如果自动选择的子网与网络上现有的子网冲突，或者需要自定义其他低级网络设置（例如 MTU），或需要[启用加密](https://docs.docker.com/engine/swarm/networking/#encryption)，则可以自定义 `ingress` 网络。

Customizing the `ingress` network involves removing and recreating it. This is usually done before you create any services in the swarm. If you have existing services which publish ports, those services need to be removed before you can remove the `ingress` network.

​	自定义 `ingress` 网络涉及删除并重新创建它。通常在创建 Swarm 中的任何服务之前完成。如果存在发布端口的服务，这些服务需要在删除 `ingress` 网络前停止。

During the time that no `ingress` network exists, existing services which do not publish ports continue to function but are not load-balanced. This affects services which publish ports, such as a WordPress service which publishes port 80.

​	在没有 `ingress` 网络的情况下，现有的未发布端口的服务仍可继续运行，但不会进行负载均衡。这会影响那些发布了端口的服务，例如发布了 80 端口的 WordPress 服务。

1. Inspect the `ingress` network using `docker network inspect ingress`, and remove any services whose containers are connected to it. These are services that publish ports, such as a WordPress service which publishes port 80. If all such services are not stopped, the next step fails. 使用 `docker network inspect ingress` 检查 `ingress` 网络，并删除所有连接到它的服务容器。这些通常是发布了端口的服务，例如发布了 80 端口的 WordPress 服务。如果未停止所有此类服务，则下一步操作将失败。

2. Remove the existing `ingress` network:

   删除现有的 `ingress` 网络：

   ```console
   $ docker network rm ingress
   
   WARNING! Before removing the routing-mesh network, make sure all the nodes
   in your swarm run the same docker engine version. Otherwise, removal may not
   be effective and functionality of newly created ingress networks will be
   impaired.
   Are you sure you want to continue? [y/N]
   ```

3. Create a new overlay network using the `--ingress` flag, along with the custom options you want to set. This example sets the MTU to 1200, sets the subnet to `10.11.0.0/16`, and sets the gateway to `10.11.0.2`.

   使用 `--ingress` 标志以及想要设置的自定义选项创建一个新的 Overlay 网络。例如，以下设置将 MTU 设置为 1200，子网设置为 `10.11.0.0/16`，网关设置为 `10.11.0.2`：

   ```console
   $ docker network create \
     --driver overlay \
     --ingress \
     --subnet=10.11.0.0/16 \
     --gateway=10.11.0.2 \
     --opt com.docker.network.driver.mtu=1200 \
     my-ingress
   ```

   > **Note**
   >
   > 
   >
   > You can name your `ingress` network something other than `ingress`, but you can only have one. An attempt to create a second one fails.
   >
   > ​	您可以将 `ingress` 网络命名为其他名称，但只能有一个 `ingress` 网络。尝试创建第二个将会失败。

4. Restart the services that you stopped in the first step. 重启在第 1 步中停止的服务。

## 自定义 docker_gwbridge - Customize the docker_gwbridge

The `docker_gwbridge` is a virtual bridge that connects the overlay networks (including the `ingress` network) to an individual Docker daemon's physical network. Docker creates it automatically when you initialize a swarm or join a Docker host to a swarm, but it is not a Docker device. It exists in the kernel of the Docker host. If you need to customize its settings, you must do so before joining the Docker host to the swarm, or after temporarily removing the host from the swarm.

​	`docker_gwbridge` 是一个虚拟网桥，将 Overlay 网络（包括 `ingress` 网络）连接到 Docker 守护进程的物理网络。Docker 会在初始化 Swarm 或将 Docker 主机加入 Swarm 时自动创建它，但它并不是 Docker 设备，而是在 Docker 主机的内核中。如果需要自定义它的设置，必须在将主机加入 Swarm 之前或临时移出 Swarm 后执行。

You need to have the `brctl` application installed on your operating system in order to delete an existing bridge. The package name is `bridge-utils`.

​	您需要在操作系统上安装 `brctl` 应用程序，以删除现有的网桥。该软件包名为 `bridge-utils`。

1. Stop Docker. 您需要在操作系统上安装 `brctl` 应用程序，以删除现有的网桥。该软件包名为 `bridge-utils`。

2. Use the `brctl show docker_gwbridge` command to check whether a bridge device exists called `docker_gwbridge`. If so, remove it using `brctl delbr docker_gwbridge`. 使用 `brctl show docker_gwbridge` 命令检查是否存在名为 `docker_gwbridge` 的网桥设备。如有，请使用 `brctl delbr docker_gwbridge` 将其删除。

3. Start Docker. Do not join or initialize the swarm. 启动 Docker，但不要加入或初始化 Swarm。

4. Create or re-create the `docker_gwbridge` bridge with your custom settings. This example uses the subnet `10.11.0.0/16`. For a full list of customizable options, see [Bridge driver options](https://docs.docker.com/reference/cli/docker/network/create/#bridge-driver-options).

   使用自定义设置创建或重新创建 `docker_gwbridge` 网桥。例如，该示例使用子网 `10.11.0.0/16`。有关可自定义选项的完整列表，请参阅 [Bridge 驱动程序选项](https://docs.docker.com/reference/cli/docker/network/create/#bridge-driver-options)。

   ```console
   $ docker network create \
   --subnet 10.11.0.0/16 \
   --opt com.docker.network.bridge.name=docker_gwbridge \
   --opt com.docker.network.bridge.enable_icc=false \
   --opt com.docker.network.bridge.enable_ip_masquerade=true \
   docker_gwbridge
   ```

5. Initialize or join the swarm. 初始化或加入 Swarm。

## 使用独立接口处理控制和数据流量 Use a separate interface for control and data traffic

By default, all swarm traffic is sent over the same interface, including control and management traffic for maintaining the swarm itself and data traffic to and from the service containers.

​	默认情况下，所有 Swarm 流量都通过同一接口发送，包括用于维护 Swarm 的控制和管理流量以及来自服务容器的数据流量。

You can separate this traffic by passing the `--data-path-addr` flag when initializing or joining the swarm. If there are multiple interfaces, `--advertise-addr` must be specified explicitly, and `--data-path-addr` defaults to `--advertise-addr` if not specified. Traffic about joining, leaving, and managing the swarm is sent over the `--advertise-addr` interface, and traffic among a service's containers is sent over the `--data-path-addr` interface. These flags can take an IP address or a network device name, such as `eth0`.

​	可以在初始化或加入 Swarm 时通过 `--data-path-addr` 标志分隔这些流量。若存在多个接口，需显式指定 `--advertise-addr`，若未指定 `--data-path-addr`，则默认为 `--advertise-addr`。Swarm 的加入、离开和管理流量会通过 `--advertise-addr` 接口发送，而服务容器之间的流量则通过 `--data-path-addr` 接口传输。这些标志可以接受 IP 地址或网络设备名称，例如 `eth0`。

This example initializes a swarm with a separate `--data-path-addr`. It assumes that your Docker host has two different network interfaces: 10.0.0.1 should be used for control and management traffic and 192.168.0.1 should be used for traffic relating to services.

​	以下示例使用单独的 `--data-path-addr` 初始化 Swarm。假设您的 Docker 主机有两个不同的网络接口：10.0.0.1 用于控制和管理流量，192.168.0.1 用于服务相关的流量。

```console
$ docker swarm init --advertise-addr 10.0.0.1 --data-path-addr 192.168.0.1
```

This example joins the swarm managed by host `192.168.99.100:2377` and sets the `--advertise-addr` flag to `eth0` and the `--data-path-addr` flag to `eth1`.

​	以下示例加入由主机 `192.168.99.100:2377` 管理的 Swarm，并将 `--advertise-addr` 标志设置为 `eth0`，`--data-path-addr` 标志设置为 `eth1`。

```console
$ docker swarm join \
  --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2d7c \
  --advertise-addr eth0 \
  --data-path-addr eth1 \
  192.168.99.100:2377
```

## 在 Overlay 网络上发布端口 Publish ports on an overlay network

Swarm services connected to the same overlay network effectively expose all ports to each other. For a port to be accessible outside of the service, that port must be *published* using the `-p` or `--publish` flag on `docker service create` or `docker service update`. Both the legacy colon-separated syntax and the newer comma-separated value syntax are supported. The longer syntax is preferred because it is somewhat self-documenting.

​	连接到同一 Overlay 网络的 Swarm 服务能够相互访问彼此的端口。要使端口在服务之外可访问，必须在 `docker service create` 或 `docker service update` 中使用 `-p` 或 `--publish` 标志 *发布* 该端口。支持传统的冒号分隔语法和新的逗号分隔值语法。推荐使用较长的语法，因为它具有一定的自解释性。

| Flag value                                                   | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `-p 8080:80` or `-p published=8080,target=80`                | 将服务上的 TCP 端口 80 映射到路由网格上的端口 8080。 Map TCP port 80 on the service to port 8080 on the routing mesh. |
| `-p 8080:80/udp` or `-p published=8080,target=80,protocol=udp` | 将服务上的 UDP 端口 80 映射到路由网格上的端口 8080。Map UDP port 80 on the service to port 8080 on the routing mesh. |
| `-p 8080:80/tcp -p 8080:80/udp` or `-p published=8080,target=80,protocol=tcp -p published=8080,target=80,protocol=udp` | 将服务上的 TCP 端口 80 映射到路由网格上的 TCP 端口 8080，同时将服务上的 UDP 端口 80 映射到路由网格上的 UDP 端口 8080。Map TCP port 80 on the service to TCP port 8080 on the routing mesh, and map UDP port 80 on the service to UDP port 8080 on the routing mesh. |

## 了解更多 Learn more

- [Deploy services to a swarm 向 Swarm 部署服务]({{< ref "/manuals/DockerEngine/Swarmmode/Deployservicestoaswarm" >}})
- [Swarm administration guide - Swarm 管理指南]({{< ref "/manuals/DockerEngine/Swarmmode/AdministerandmaintainaswarmofDockerEngines" >}})
- [Swarm mode tutorial - Swarm 模式教程]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode" >}})
- [Networking overview - 网络概述]({{< ref "/manuals/DockerEngine/Networking" >}})
- [Docker CLI reference - Docker CLI 参考]({{< ref "/reference/CLIreference/docker" >}})
