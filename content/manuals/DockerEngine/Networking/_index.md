+++
title = "Networking"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/network/](https://docs.docker.com/engine/network/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Networking overview - 网络概述

Container networking refers to the ability for containers to connect to and communicate with each other, or to non-Docker workloads.

​	容器网络指的是容器之间或与非 Docker 工作负载之间连接和通信的能力。

Containers have networking enabled by default, and they can make outgoing connections. A container has no information about what kind of network it's attached to, or whether their peers are also Docker workloads or not. A container only sees a network interface with an IP address, a gateway, a routing table, DNS services, and other networking details. That is, unless the container uses the `none` network driver.

​	容器默认启用网络功能，并且可以进行外部连接。容器不知道自己连接到的是什么类型的网络，也不知道它的对等节点是否也是 Docker 工作负载。容器只会看到带有 IP 地址、网关、路由表、DNS 服务等网络详细信息的网络接口，除非容器使用 `none` 网络驱动。

This page describes networking from the point of view of the container, and the concepts around container networking. This page doesn't describe OS-specific details about how Docker networks work. For information about how Docker manipulates `iptables` rules on Linux, see [Packet filtering and firewalls]({{< ref "/manuals/DockerEngine/Networking/Packetfilteringandfirewalls" >}}).

​	本页面从容器的角度描述网络及容器网络的相关概念。此页面不描述 Docker 网络的操作系统特定细节。关于 Docker 如何在 Linux 中操作 `iptables` 规则的信息，请参见[数据包过滤和防火墙]({{< ref "/manuals/DockerEngine/Networking/Packetfilteringandfirewalls" >}})。

## 用户定义的网络 User-defined networks

You can create custom, user-defined networks, and connect multiple containers to the same network. Once connected to a user-defined network, containers can communicate with each other using container IP addresses or container names.

​	您可以创建自定义网络，并将多个容器连接到同一个网络中。一旦连接到用户定义的网络，容器可以通过容器 IP 地址或容器名称相互通信。

The following example creates a network using the `bridge` network driver and running a container in the created network:

​	以下示例使用 `bridge` 网络驱动创建一个网络，并在创建的网络中运行一个容器：

```console
$ docker network create -d bridge my-net
$ docker run --network=my-net -itd --name=container3 busybox
```

### 网络驱动 Drivers

The following network drivers are available by default, and provide core networking functionality:

​	以下网络驱动默认可用，提供核心网络功能：

| Driver    | Description                                                  |
| :-------- | :----------------------------------------------------------- |
| `bridge`  | 默认网络驱动。 The default network driver.                   |
| `host`    | 移除容器与 Docker 主机之间的网络隔离。 Remove network isolation between the container and the Docker host. |
| `none`    | 完全隔离容器与主机及其他容器的网络。 Completely isolate a container from the host and other containers. |
| `overlay` | Overlay 网络连接多个 Docker 守护进程。 Overlay networks connect multiple Docker daemons together. |
| `ipvlan`  | IPvlan 网络提供对 IPv4 和 IPv6 地址的完全控制。 IPvlan networks provide full control over both IPv4 and IPv6 addressing. |
| `macvlan` | 为容器分配 MAC 地址。Assign a MAC address to a container.    |

For more information about the different drivers, see [Network drivers overview]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers" >}}).

​	有关不同驱动的更多信息，请参见[网络驱动概述]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers" >}})。

## 容器网络 Container networks

In addition to user-defined networks, you can attach a container to another container's networking stack directly, using the `--network container:<name|id>` flag format.

​	除了用户定义的网络外，您还可以使用 `--network container:<name|id>` 标志格式，直接将一个容器连接到另一个容器的网络栈。

The following flags aren't supported for containers using the `container:` networking mode:

​	使用 `container:` 网络模式的容器不支持以下标志：

- `--add-host`
- `--hostname`
- `--dns`
- `--dns-search`
- `--dns-option`
- `--mac-address`
- `--publish`
- `--publish-all`
- `--expose`

The following example runs a Redis container, with Redis binding to `localhost`, then running the `redis-cli` command and connecting to the Redis server over the `localhost` interface.

​	以下示例运行了一个 Redis 容器，将 Redis 绑定到 `localhost`，然后运行 `redis-cli` 命令并通过 `localhost` 接口连接到 Redis 服务器。

```console
$ docker run -d --name redis example/redis --bind 127.0.0.1
$ docker run --rm -it --network container:redis example/redis-cli -h 127.0.0.1
```

## 端口发布 Published ports

By default, when you create or run a container using `docker create` or `docker run`, containers on bridge networks don't expose any ports to the outside world. Use the `--publish` or `-p` flag to make a port available to services outside the bridge network. This creates a firewall rule in the host, mapping a container port to a port on the Docker host to the outside world. Here are some examples:

​	默认情况下，当您使用 `docker create` 或 `docker run` 创建或运行容器时，桥接网络中的容器不会对外部开放任何端口。可以使用 `--publish` 或 `-p` 标志将端口开放给桥接网络以外的服务。这会在主机上创建一个防火墙规则，将容器端口映射到 Docker 主机上的一个端口，从而对外开放。以下是一些示例：

| Flag value 标志值               | Description 描述                                             |
| ------------------------------- | ------------------------------------------------------------ |
| `-p 8080:80`                    | 将 Docker 主机上的端口 `8080` 映射到容器中的 TCP 端口 `80`。 Map port `8080` on the Docker host to TCP port `80` in the container. |
| `-p 192.168.1.100:8080:80`      | 将 Docker 主机 IP `192.168.1.100` 上的端口 `8080` 映射到容器中的 TCP 端口 `80`。 Map port `8080` on the Docker host IP `192.168.1.100` to TCP port `80` in the container. |
| `-p 8080:80/udp`                | 将 Docker 主机上的端口 `8080` 映射到容器中的 UDP 端口 `80`。 Map port `8080` on the Docker host to UDP port `80` in the container. |
| `-p 8080:80/tcp -p 8080:80/udp` | 将 Docker 主机的 TCP 端口 `8080` 映射到容器中的 TCP 端口 `80`，并将 UDP 端口 `8080` 映射到容器中的 UDP 端口 `80`。 Map TCP port `8080` on the Docker host to TCP port `80` in the container, and map UDP port `8080` on the Docker host to UDP port `80` in the container. |

> **Important**
>
> 
>
> Publishing container ports is insecure by default. Meaning, when you publish a container's ports it becomes available not only to the Docker host, but to the outside world as well.
>
> ​	发布容器端口默认是不安全的，这意味着发布的端口不仅对 Docker 主机可用，对外界也可访问。
>
> If you include the localhost IP address (`127.0.0.1`, or `::1`) with the publish flag, only the Docker host and its containers can access the published container port.
>
> ​	如果在发布标志中包含 localhost IP 地址（`127.0.0.1` 或 `::1`），则只有 Docker 主机及其容器可以访问已发布的容器端口。
>
> ```console
> $ docker run -p 127.0.0.1:8080:80 -p '[::1]:8080:80' nginx
> ```
>
> > **Warning**
> >
> > 
> >
> > Hosts within the same L2 segment (for example, hosts connected to the same network switch) can reach ports published to localhost. For more information, see [moby/moby#45610](https://github.com/moby/moby/issues/45610)
> >
> > ​	在同一 L2 网络段内的主机（例如连接到同一网络交换机的主机）可以访问发布到 localhost 的端口。详情请参见 [moby/moby#45610](https://github.com/moby/moby/issues/45610)。

If you want to make a container accessible to other containers, it isn't necessary to publish the container's ports. You can enable inter-container communication by connecting the containers to the same network, usually a [bridge network]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Bridgenetworkdriver" >}}).

​	如果您希望使容器对其他容器可访问，则不需要发布容器的端口。可以通过将容器连接到同一网络（通常是[桥接网络]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Bridgenetworkdriver" >}})）来启用容器间通信。

Ports on the host's IPv6 addresses will map to the container's IPv4 address if no host IP is given in a port mapping, the bridge network is IPv4-only, and `--userland-proxy=true` (default).

​	当端口映射中没有指定主机 IP 且桥接网络仅支持 IPv4 时，主机的 IPv6 地址的端口将映射到容器的 IPv4 地址（默认情况下 `--userland-proxy=true`）。

For more information about port mapping, including how to disable it and use direct routing to containers, see [packet filtering and firewalls]({{< ref "/manuals/DockerEngine/Networking/Packetfilteringandfirewalls" >}}).

​	更多关于端口映射的信息，包括如何禁用它和使用直接路由连接到容器的信息，请参见[数据包过滤与防火墙]({{< ref "/manuals/DockerEngine/Networking/Packetfilteringandfirewalls" >}})。

## IP 地址和主机名 IP address and hostname

By default, the container gets an IP address for every Docker network it attaches to. A container receives an IP address out of the IP subnet of the network. The Docker daemon performs dynamic subnetting and IP address allocation for containers. Each network also has a default subnet mask and gateway.

​	默认情况下，容器在连接到每个 Docker 网络时会获得一个 IP 地址。容器从网络的 IP 子网中获取 IP 地址。Docker 守护进程会为容器执行动态子网划分和 IP 地址分配。每个网络还具有默认的子网掩码和网关。

You can connect a running container to multiple networks, either by passing the `--network` flag multiple times when creating the container, or using the `docker network connect` command for already running containers. In both cases, you can use the `--ip` or `--ip6` flags to specify the container's IP address on that particular network.

​	可以在创建容器时多次传递 `--network` 标志，或者使用 `docker network connect` 命令将运行中的容器连接到多个网络。在这两种情况下，都可以使用 `--ip` 或 `--ip6` 标志指定容器在该网络中的 IP 地址。

In the same way, a container's hostname defaults to be the container's ID in Docker. You can override the hostname using `--hostname`. When connecting to an existing network using `docker network connect`, you can use the `--alias` flag to specify an additional network alias for the container on that network.

​	同样，容器的主机名默认为容器在 Docker 中的 ID。可以使用 `--hostname` 覆盖主机名。在使用 `docker network connect` 连接到现有网络时，可以使用 `--alias` 标志为容器在该网络上指定额外的网络别名。

## DNS 服务 DNS services

Containers use the same DNS servers as the host by default, but you can override this with `--dns`.

​	默认情况下，容器使用与主机相同的 DNS 服务器，但可以使用 `--dns` 覆盖。

By default, containers inherit the DNS settings as defined in the `/etc/resolv.conf` configuration file. Containers that attach to the default `bridge` network receive a copy of this file. Containers that attach to a [custom network](https://docs.docker.com/engine/network/tutorials/standalone/#use-user-defined-bridge-networks) use Docker's embedded DNS server. The embedded DNS server forwards external DNS lookups to the DNS servers configured on the host.

​	默认情况下，容器继承在 `/etc/resolv.conf` 配置文件中定义的 DNS 设置。连接到默认 `bridge` 网络的容器会收到此文件的副本。连接到[自定义网络](https://docs.docker.com/engine/network/tutorials/standalone/#use-user-defined-bridge-networks)的容器则使用 Docker 的嵌入式 DNS 服务器。嵌入式 DNS 服务器会将外部 DNS 查找转发到主机上配置的 DNS 服务器。

You can configure DNS resolution on a per-container basis, using flags for the `docker run` or `docker create` command used to start the container. The following table describes the available `docker run` flags related to DNS configuration.

​	可以在每个容器基础上配置 DNS，使用 `docker run` 或 `docker create` 命令的标志来启动容器。以下表格描述了与 DNS 配置相关的 `docker run` 标志：

| Flag           | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `--dns`        | The IP address of a DNS server. To specify multiple DNS servers, use multiple `--dns` flags. DNS requests will be forwarded from the container's network namespace so, for example, `--dns=127.0.0.1` refers to the container's own loopback address. DNS 服务器的 IP 地址。若指定多个 DNS 服务器，可使用多个 `--dns` 标志。DNS 请求会从容器的网络命名空间中转发，例如，`--dns=127.0.0.1` 指的是容器自己的回环地址。 |
| `--dns-search` | A DNS search domain to search non-fully qualified hostnames. To specify multiple DNS search prefixes, use multiple `--dns-search` flags. 用于搜索非完全限定主机名的 DNS 搜索域。若指定多个 DNS 搜索前缀，可使用多个 `--dns-search` 标志。 |
| `--dns-opt`    | A key-value pair representing a DNS option and its value. See your operating system's documentation for `resolv.conf` for valid options. 表示 DNS 选项及其值的键值对。有关有效选项，请参阅操作系统的 `resolv.conf` 文档。 |
| `--hostname`   | The hostname a container uses for itself. Defaults to the container's ID if not specified. 容器用来识别自己的主机名。若未指定，则默认为容器的 ID。 |

### 自定义主机 Custom hosts

Your container will have lines in `/etc/hosts` which define the hostname of the container itself, as well as `localhost` and a few other common things. Custom hosts, defined in `/etc/hosts` on the host machine, aren't inherited by containers. To pass additional hosts into a container, refer to [add entries to container hosts file](https://docs.docker.com/reference/cli/docker/container/run/#add-host) in the `docker run` reference documentation.

​	容器的 `/etc/hosts` 文件中会包含容器自身的主机名，以及 `localhost` 和一些常用条目。在主机上的 `/etc/hosts` 文件中定义的自定义主机不会被容器继承。如需将额外的主机传入容器，请参见 `docker run` 参考文档中的[添加到容器主机文件]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerrun#向容器的主机文件添加条目---add-host-add-entries-to-container-hosts-file---add-host">}})。

## 代理服务器 Proxy server

If your container needs to use a proxy server, see [Use a proxy server]({{< ref "/manuals/DockerEngine/Daemon/Daemonproxyconfiguration" >}}).

​	如果您的容器需要使用代理服务器，请参见[使用代理服务器]({{< ref "/manuals/DockerEngine/Daemon/Daemonproxyconfiguration" >}})。
