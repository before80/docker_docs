+++
title = "docker network connect"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/network/connect/](https://docs.docker.com/reference/cli/docker/network/connect/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker network connect

| Description | Connect a container to a network                     |
| :---------- | ---------------------------------------------------- |
| Usage       | `docker network connect [OPTIONS] NETWORK CONTAINER` |

## Description

Connects a container to a network. You can connect a container by name or by ID. Once connected, the container can communicate with other containers in the same network.

​	将容器连接到网络。您可以通过名称或 ID 来连接容器。一旦连接，容器就可以与同一网络中的其他容器进行通信。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`--alias`](https://docs.docker.com/reference/cli/docker/network/connect/#alias) |         | 为容器添加网络作用域内的别名 Add network-scoped alias for the container |
| `--driver-opt`                                               |         | 网络的驱动程序选项 driver options for the network            |
| [`--ip`](https://docs.docker.com/reference/cli/docker/network/connect/#ip) |         | IPv4 地址（例如，`172.30.100.104`） IPv4 address (e.g., `172.30.100.104`) |
| `--ip6`                                                      |         | IPv6 地址（例如，`2001:db8::33`） IPv6 address (e.g., `2001:db8::33`) |
| [`--link`](https://docs.docker.com/reference/cli/docker/network/connect/#link) |         | 添加到另一个容器的链接 Add link to another container         |
| `--link-local-ip`                                            |         | 为容器添加本地链路地址 Add a link-local address for the container |

## Examples

### 将运行中的容器连接到网络 Connect a running container to a network



```console
$ docker network connect multi-host-network container1
```

### 在启动时将容器连接到网络 Connect a container to a network when it starts

You can also use the `docker run --network=<network-name>` option to start a container and immediately connect it to a network.

​	您还可以使用 `docker run --network=<network-name>` 选项来启动一个容器并立即将其连接到网络。



```console
$ docker run -itd --network=multi-host-network busybox
```

### 指定容器在网络中使用的 IP 地址（`--ip`） Specify the IP address a container will use on a given network (`--ip`)

You can specify the IP address you want to be assigned to the container's interface.



```console
$ docker network connect --ip 10.10.36.122 multi-host-network container2
```

### 使用旧的 `--link` 选项（`--link`） Use the legacy `--link` option (`--link`)

You can use `--link` option to link another container with a preferred alias.

​	您可以使用 `--link` 选项为另一个容器创建一个首选别名。



```console
$ docker network connect --link container1:c1 multi-host-network container2
```

### 为容器创建网络别名（`--alias`） Create a network alias for a container (`--alias`)

`--alias` option can be used to resolve the container by another name in the network being connected to.

​	`--alias` 选项可用于在所连接的网络中以另一个名称解析容器。



```console
$ docker network connect --alias db --alias mysql multi-host-network container2
```

### 设置容器接口的 sysctls 参数（`--driver-opt`） Set sysctls for a container's interface (`--driver-opt`)

`sysctl` settings that start with `net.ipv4.` and `net.ipv6.` can be set per-interface using `--driver-opt` label `com.docker.network.endpoint.sysctls`. The name of the interface must be replaced by `IFNAME`.

​	以 `net.ipv4.` 和 `net.ipv6.` 开头的 `sysctl` 设置可以使用 `--driver-opt` 标签 `com.docker.network.endpoint.sysctls` 为每个接口设置。接口名称必须用 `IFNAME` 替代。

To set more than one `sysctl` for an interface, quote the whole value of the `driver-opt` field, remembering to escape the quotes for the shell if necessary. For example, if the interface to `my-net` is given name `eth3`, the following example sets `net.ipv4.conf.eth3.log_martians=1` and `net.ipv4.conf.eth3.forwarding=0`.

​	要为一个接口设置多个 `sysctl` 参数，请对 `driver-opt` 字段的值进行引用，如果必要，请记得为 shell 转义引号。例如，如果接口 `my-net` 名称为 `eth3`，以下示例设置 `net.ipv4.conf.eth3.log_martians=1` 和 `net.ipv4.conf.eth3.forwarding=0`。



```console
$ docker network connect --driver-opt=\"com.docker.network.endpoint.sysctls=net.ipv4.conf.IFNAME.log_martians=1,net.ipv4.conf.IFNAME.forwarding=0\" multi-host-network container2
```

> **Note**
>
> Network drivers may restrict the sysctl settings that can be modified and, to protect the operation of the network, new restrictions may be added in the future.
>
> ​	网络驱动程序可能会限制可以修改的 sysctl 设置，为了保护网络操作，未来可能会增加新的限制。

### 停止、暂停或重启容器的网络影响 Network implications of stopping, pausing, or restarting containers

You can pause, restart, and stop containers that are connected to a network. A container connects to its configured networks when it runs.

​	您可以暂停、重启和停止连接到网络的容器。当容器运行时会连接到其配置的网络。

If specified, the container's IP address(es) is reapplied when a stopped container is restarted. If the IP address is no longer available, the container fails to start. One way to guarantee that the IP address is available is to specify an `--ip-range` when creating the network, and choose the static IP address(es) from outside that range. This ensures that the IP address is not given to another container while this container is not on the network.

​	如果指定了 IP 地址，则在重新启动停止的容器时会重新应用该 IP 地址。如果该 IP 地址不可用，容器将无法启动。确保 IP 地址可用的一种方法是在创建网络时指定 `--ip-range`，并从该范围外选择静态 IP 地址。这样可以确保在容器不在网络时，该 IP 地址不会被分配给其他容器。



```console
$ docker network create --subnet 172.20.0.0/16 --ip-range 172.20.240.0/20 multi-host-network
```



```console
$ docker network connect --ip 172.20.128.2 multi-host-network container2
```

To verify the container is connected, use the `docker network inspect` command. Use `docker network disconnect` to remove a container from the network.

​	要验证容器已连接，请使用 `docker network inspect` 命令。使用 `docker network disconnect` 可以将容器从网络中移除。

Once connected in network, containers can communicate using only another container's IP address or name. For `overlay` networks or custom plugins that support multi-host connectivity, containers connected to the same multi-host network but launched from different Engines can also communicate in this way.

​	一旦连接到网络，容器可以仅使用另一个容器的 IP 地址或名称进行通信。对于 `overlay` 网络或支持多主机连接的自定义插件，从不同的引擎启动但连接到相同多主机网络的容器也可以通过这种方式通信。

You can connect a container to one or more networks. The networks need not be the same type. For example, you can connect a single container bridge and overlay networks.

​	您可以将一个容器连接到一个或多个网络。这些网络不必是同一类型。例如，您可以将一个容器同时连接到桥接和 overlay 网络。
