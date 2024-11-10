+++
title = "Overlay 网络驱动"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/network/drivers/overlay/](https://docs.docker.com/engine/network/drivers/overlay/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Overlay network driver - Overlay 网络驱动

The `overlay` network driver creates a distributed network among multiple Docker daemon hosts. This network sits on top of (overlays) the host-specific networks, allowing containers connected to it to communicate securely when encryption is enabled. Docker transparently handles routing of each packet to and from the correct Docker daemon host and the correct destination container.

​	`overlay` 网络驱动创建了一个分布式网络，使多个 Docker 守护进程主机之间的容器可以相互通信。该网络覆盖在主机特定的网络之上，使连接到它的容器能够在启用加密时安全地通信。Docker 会自动处理每个数据包的路由，将其发送到正确的 Docker 守护进程主机和目的地容器。

You can create user-defined `overlay` networks using `docker network create`, in the same way that you can create user-defined `bridge` networks. Services or containers can be connected to more than one network at a time. Services or containers can only communicate across networks they're each connected to.

​	您可以使用 `docker network create` 创建用户定义的 `overlay` 网络，就像可以创建用户定义的 `bridge` 网络一样。服务或容器可以同时连接到多个网络。服务或容器只能在它们共同连接的网络之间通信。

Overlay networks are often used to create a connection between Swarm services, but you can also use it to connect standalone containers running on different hosts. When using standalone containers, it's still required that you use Swarm mode to establish a connection between the hosts.

​	Overlay 网络通常用于创建 Swarm 服务之间的连接，但也可以用于连接运行在不同主机上的独立容器。使用独立容器时，仍然需要使用 Swarm 模式来建立主机之间的连接。

This page describes overlay networks in general, and when used with standalone containers. For information about overlay for Swarm services, see [Manage Swarm service networks]({{< ref "/manuals/DockerEngine/Swarmmode/Manageswarmservicenetworks" >}}).

​	本页面描述了通用的 Overlay 网络，以及它们如何与独立容器一起使用。有关 Swarm 服务的 Overlay 网络的信息，请参见 [管理 Swarm 服务网络]({{< ref "/manuals/DockerEngine/Swarmmode/Manageswarmservicenetworks" >}})

## 创建一个 Overlay 网络 Create an overlay network

Before you start, you must ensure that participating nodes can communicate over the network. The following table lists ports that need to be open to each host participating in an overlay network:

​	在开始之前，您必须确保参与的节点能够通过网络通信。下表列出了每个参与 Overlay 网络的主机需要开放的端口：

| Ports                  | Description                                                  |
| :--------------------- | :----------------------------------------------------------- |
| `2377/tcp`             | The default Swarm control plane port, is configurable with [`docker swarm join --listen-addr`](https://docs.docker.com/reference/cli/docker/swarm/join/#--listen-addr-value) 默认的 Swarm 控制平面端口，可以通过 [`docker swarm join --listen-addr`](https://docs.docker.com/reference/cli/docker/swarm/join/#--listen-addr-value) 进行配置 |
| `4789/udp`             | The default overlay traffic port, configurable with [`docker swarm init --data-path-addr`](https://docs.docker.com/reference/cli/docker/swarm/init/#data-path-port) 默认的 Overlay 流量端口，可以通过 [`docker swarm init --data-path-addr`](https://docs.docker.com/reference/cli/docker/swarm/init/#data-path-port) 进行配置 |
| `7946/tcp`, `7946/udp` | Used for communication among nodes, not configurable 用于节点间的通信，不可配置 |

To create an overlay network that containers on other Docker hosts can connect to, run the following command:

​	要创建一个其他 Docker 主机上的容器可以连接的 Overlay 网络，请运行以下命令：

```console
$ docker network create -d overlay --attachable my-attachable-overlay
```

The `--attachable` option enables both standalone containers and Swarm services to connect to the overlay network. Without `--attachable`, only Swarm services can connect to the network.

​	`--attachable` 选项使得独立容器和 Swarm 服务都可以连接到该 Overlay 网络。如果没有 `--attachable`，则只有 Swarm 服务可以连接到该网络。

You can specify the IP address range, subnet, gateway, and other options. See `docker network create --help` for details.

​	您可以指定 IP 地址范围、子网、网关等选项。有关详细信息，请参阅 `docker network create --help`。

## 加密 Overlay 网络上的流量 Encrypt traffic on an overlay network

Use the `--opt encrypted` flag to encrypt the application data transmitted over the overlay network:

​	使用 `--opt encrypted` 标志来加密在 Overlay 网络上传输的应用数据：

```console
$ docker network create \
  --opt encrypted \
  --driver overlay \
  --attachable \
  my-attachable-multi-host-network
```

This enables IPsec encryption at the level of the Virtual Extensible LAN (VXLAN). This encryption imposes a non-negligible performance penalty, so you should test this option before using it in production.

​	这会在虚拟扩展 LAN (VXLAN) 层启用 IPsec 加密。此加密会带来一定的性能损耗，因此建议在生产环境中使用之前先进行测试。

> **Warning**
>
> 
>
> Don't attach Windows containers to encrypted overlay networks.
>
> ​	不要将 Windows 容器连接到加密的 Overlay 网络。
>
> Overlay network encryption isn't supported on Windows. Swarm doesn't report an error when a Windows host attempts to connect to an encrypted overlay network, but networking for the Windows containers is affected as follows:
>
> ​	Windows 上不支持 Overlay 网络加密。当 Windows 主机尝试连接到加密的 Overlay 网络时，Swarm 不会报告错误，但 Windows 容器的网络会受到以下影响：
>
> - Windows containers can't communicate with Linux containers on the network
>   - Windows 容器无法与网络中的 Linux 容器通信
> - Data traffic between Windows containers on the network isn't encrypted
>   - 网络中 Windows 容器之间的数据流量不被加密

## 将容器连接到 Overlay 网络 Attach a container to an overlay network

Adding containers to an overlay network gives them the ability to communicate with other containers without having to set up routing on the individual Docker daemon hosts. A prerequisite for doing this is that the hosts have joined the same Swarm.

​	将容器添加到 Overlay 网络，使其能够与其他容器通信，而不需要在各个 Docker 守护进程主机上设置路由。前提是这些主机已经加入了同一个 Swarm。

To join an overlay network named `multi-host-network` with a `busybox` container:

​	要在名为 `multi-host-network` 的 Overlay 网络中加入一个 `busybox` 容器：



```console
$ docker run --network multi-host-network busybox sh
```

> **Note**
>
> 
>
> This only works if the overlay network is attachable (created with the `--attachable` flag).
>
> ​	这仅在 Overlay 网络是可附加的（使用 `--attachable` 标志创建）时有效。

## 容器发现 Container discovery

Publishing ports of a container on an overlay network opens the ports to other containers on the same network. Containers are discoverable by doing a DNS lookup using the container name.

​	在 Overlay 网络中发布容器的端口会使该端口对同一网络上的其他容器开放。可以通过容器名称进行 DNS 查找来发现容器。

| Flag value                      | Description                                                  |
| ------------------------------- | ------------------------------------------------------------ |
| `-p 8080:80`                    | Map TCP port 80 in the container to port `8080` on the overlay network. 将容器内的 TCP 端口 80 映射到 Overlay 网络上的端口 `8080`。 |
| `-p 8080:80/udp`                | Map UDP port 80 in the container to port `8080` on the overlay network. 将容器内的 UDP 端口 80 映射到 Overlay 网络上的端口 `8080`。 |
| `-p 8080:80/sctp`               | Map SCTP port 80 in the container to port `8080` on the overlay network. 将容器内的 SCTP 端口 80 映射到 Overlay 网络上的端口 `8080`。 |
| `-p 8080:80/tcp -p 8080:80/udp` | Map TCP port 80 in the container to TCP port `8080` on the overlay network, and map UDP port 80 in the container to UDP port `8080` on the overlay network. 将容器内的 TCP 端口 80 映射到 Overlay 网络上的 TCP 端口 `8080`，并将容器内的 UDP 端口 80 映射到 Overlay 网络上的 UDP 端口 `8080`。 |

## Overlay 网络的连接限制 Connection limit for overlay networks

Due to limitations set by the Linux kernel, overlay networks become unstable and inter-container communications may break when 1000 containers are co-located on the same host.

​	由于 Linux 内核的限制，当同一主机上共存的容器数量达到 1000 时，Overlay 网络会变得不稳定，容器间的通信可能中断。

For more information about this limitation, see [moby/moby#44973](https://github.com/moby/moby/issues/44973#issuecomment-1543747718).

​	有关此限制的更多信息，请参见 [moby/moby#44973](https://github.com/moby/moby/issues/44973#issuecomment-1543747718)。

## 接下来 Next steps

- Go through the [overlay networking tutorial]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingwithoverlaynetworks" >}})
  - 通过 [Overlay 网络教程]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingwithoverlaynetworks" >}}) 学习更多内容
- Learn about [networking from the container's point of view]({{< ref "/manuals/DockerEngine/Networking" >}})
  - 了解 [从容器的视角看网络]({{< ref "/manuals/DockerEngine/Networking" >}})
- Learn about [standalone bridge networks]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Bridgenetworkdriver" >}})
  - 学习 [独立桥接网络]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Bridgenetworkdriver" >}})
- Learn about [Macvlan networks]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Macvlannetworkdriver" >}})
  - 学习 [Macvlan 网络]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Macvlannetworkdriver" >}})
