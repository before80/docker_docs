+++
title = "Macvlan 网络驱动"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/network/drivers/macvlan/](https://docs.docker.com/engine/network/drivers/macvlan/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Macvlan network driver - Macvlan 网络驱动

Some applications, especially legacy applications or applications which monitor network traffic, expect to be directly connected to the physical network. In this type of situation, you can use the `macvlan` network driver to assign a MAC address to each container's virtual network interface, making it appear to be a physical network interface directly connected to the physical network. In this case, you need to designate a physical interface on your Docker host to use for the Macvlan, as well as the subnet and gateway of the network. You can even isolate your Macvlan networks using different physical network interfaces.

​	一些应用程序，特别是旧应用程序或用于监控网络流量的应用程序，需要直接连接到物理网络。在这种情况下，可以使用 `macvlan` 网络驱动为每个容器的虚拟网络接口分配一个 MAC 地址，使其看起来像直接连接到物理网络的物理网络接口。这种情况下，您需要在 Docker 主机上指定一个用于 Macvlan 的物理接口，以及网络的子网和网关。您甚至可以使用不同的物理网络接口来隔离 Macvlan 网络。

Keep the following things in mind:

​	请注意以下几点：

- You may unintentionally degrade your network due to IP address exhaustion or to "VLAN spread", a situation that occurs when you have an inappropriately large number of unique MAC addresses in your network.
  - 由于 IP 地址耗尽或 “VLAN 扩展” 问题（即网络中出现过多唯一 MAC 地址的情况），可能会无意中使网络性能下降。

- Your networking equipment needs to be able to handle "promiscuous mode", where one physical interface can be assigned multiple MAC addresses.
  - 您的网络设备需要支持“混杂模式”，在这种模式下，一个物理接口可以分配多个 MAC 地址。

- If your application can work using a bridge (on a single Docker host) or overlay (to communicate across multiple Docker hosts), these solutions may be better in the long term.
  - 如果您的应用程序可以使用桥接（在单个 Docker 主机上）或覆盖（在多个 Docker 主机间通信），这些方案从长期来看可能更好。


## Options

The following table describes the driver-specific options that you can pass to `--option` when creating a network using the `macvlan` driver.

​	下表描述了在使用 `macvlan` 驱动创建网络时可以通过 `--option` 传递的特定选项：

| Option 选项    | Default 默认值 | Description 描述                                             |
| -------------- | -------------- | ------------------------------------------------------------ |
| `macvlan_mode` | `bridge`       | Sets the Macvlan mode. Can be one of: `bridge`, `vepa`, `passthru`, `private` 设置 Macvlan 模式。可以是：`bridge`、`vepa`、`passthru`、`private` |
| `parent`       |                | Specifies the parent interface to use. 指定要使用的父接口。  |

## 创建一个 Macvlan 网络 Create a Macvlan network

When you create a Macvlan network, it can either be in bridge mode or 802.1Q trunk bridge mode.

​	创建 Macvlan 网络时，可以选择桥接模式或 802.1Q 主干桥接模式。

- In bridge mode, Macvlan traffic goes through a physical device on the host.
  - 在桥接模式中，Macvlan 流量通过主机上的物理设备。

- In 802.1Q trunk bridge mode, traffic goes through an 802.1Q sub-interface which Docker creates on the fly. This allows you to control routing and filtering at a more granular level.
  - 在 802.1Q 主干桥接模式中，流量通过 Docker 动态创建的 802.1Q 子接口，从而实现更细粒度的路由和过滤控制。


### 桥接模式 Bridge mode

To create a `macvlan` network which bridges with a given physical network interface, use `--driver macvlan` with the `docker network create` command. You also need to specify the `parent`, which is the interface the traffic will physically go through on the Docker host.

​	要创建与指定物理网络接口桥接的 `macvlan` 网络，请使用 `--driver macvlan` 和 `docker network create` 命令，还需指定 `parent`，即流量在 Docker 主机上物理传输的接口。



```console
$ docker network create -d macvlan \
  --subnet=172.16.86.0/24 \
  --gateway=172.16.86.1 \
  -o parent=eth0 pub_net
```

If you need to exclude IP addresses from being used in the `macvlan` network, such as when a given IP address is already in use, use `--aux-addresses`:

​	如果需要将某些 IP 地址排除在 `macvlan` 网络之外，例如某个 IP 地址已被占用，可以使用 `--aux-addresses`：

```console
$ docker network create -d macvlan \
  --subnet=192.168.32.0/24 \
  --ip-range=192.168.32.128/25 \
  --gateway=192.168.32.254 \
  --aux-address="my-router=192.168.32.129" \
  -o parent=eth0 macnet32
```

### 802.1Q 主干桥接模式 802.1Q trunk bridge mode

If you specify a `parent` interface name with a dot included, such as `eth0.50`, Docker interprets that as a sub-interface of `eth0` and creates the sub-interface automatically.

​	如果在指定 `parent` 接口名称时包含了点号（例如 `eth0.50`），Docker 会将其解释为 `eth0` 的子接口，并自动创建子接口。



```console
$ docker network create -d macvlan \
    --subnet=192.168.50.0/24 \
    --gateway=192.168.50.1 \
    -o parent=eth0.50 macvlan50
```

### 使用 IPvlan 代替 Macvlan - Use an IPvlan instead of Macvlan

In the above example, you are still using a L3 bridge. You can use `ipvlan` instead, and get an L2 bridge. Specify `-o ipvlan_mode=l2`.

​	在上述示例中，您仍在使用 L3 桥接。您可以使用 `ipvlan` 以获得 L2 桥接效果。指定 `-o ipvlan_mode=l2`。



```console
$ docker network create -d ipvlan \
    --subnet=192.168.210.0/24 \
    --subnet=192.168.212.0/24 \
    --gateway=192.168.210.254 \
    --gateway=192.168.212.254 \
     -o ipvlan_mode=l2 -o parent=eth0 ipvlan210
```

## Use IPv6

If you have [configured the Docker daemon to allow IPv6]({{< ref "/manuals/DockerEngine/Daemon/UseIPv6networking" >}}), you can use dual-stack IPv4/IPv6 `macvlan` networks.

​	如果您已配置 Docker 守护进程允许 IPv6 ，可以使用双栈 IPv4/IPv6 的 `macvlan` 网络。

```console
$ docker network create -d macvlan \
    --subnet=192.168.216.0/24 --subnet=192.168.218.0/24 \
    --gateway=192.168.216.1 --gateway=192.168.218.1 \
    --subnet=2001:db8:abc8::/64 --gateway=2001:db8:abc8::10 \
     -o parent=eth0.218 \
     -o macvlan_mode=bridge macvlan216
```

## 接下来 Next steps

Learn how to use the Macvlan driver in the [Macvlan networking tutorial]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingusingamacvlannetwork" >}}).

​	了解如何在 [Macvlan 网络教程]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingusingamacvlannetwork" >}}) 中使用 Macvlan 驱动。
