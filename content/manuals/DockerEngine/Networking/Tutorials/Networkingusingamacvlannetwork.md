+++
title = "使用 macvlan 网络的网络教程"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/network/tutorials/macvlan/](https://docs.docker.com/engine/network/tutorials/macvlan/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Networking using a macvlan network - 使用 macvlan 网络的网络教程

This series of tutorials deals with networking standalone containers which connect to `macvlan` networks. In this type of network, the Docker host accepts requests for multiple MAC addresses at its IP address, and routes those requests to the appropriate container. For other networking topics, see the [overview]({{< ref "/manuals/DockerEngine/Networking" >}}).

​	本系列教程介绍了如何在连接到 `macvlan` 网络的独立容器中进行网络设置。在这种类型的网络中，Docker 主机接受多个 MAC 地址的请求并将这些请求路由到相应的容器。有关其他网络主题，请参见[网络概述]({{< ref "/manuals/DockerEngine/Networking" >}})。

## 目标 Goal

The goal of these tutorials is to set up a bridged `macvlan` network and attach a container to it, then set up an 802.1Q trunked `macvlan` network and attach a container to it.

​	本教程的目标是设置一个桥接的 `macvlan` 网络并将容器连接到该网络，然后设置一个 802.1Q 中继的 `macvlan` 网络并将容器连接到其中。

## 前置条件 Prerequisites

- Most cloud providers block `macvlan` networking. You may need physical access to your networking equipment.
  - 大多数云提供商阻止 `macvlan` 网络，因此您可能需要对网络设备进行物理访问。

- The `macvlan` networking driver only works on Linux hosts, and is not supported on Docker Desktop for Mac, Docker Desktop for Windows, or Docker EE for Windows Server.
  - `macvlan` 网络驱动程序仅适用于 Linux 主机，不支持 Mac 和 Windows 版 Docker Desktop 以及 Windows Server 版 Docker EE。

- You need at least version 3.9 of the Linux kernel, and version 4.0 or higher is recommended.
  - 需要至少 3.9 版本的 Linux 内核，建议使用 4.0 或更高版本。

- The examples assume your ethernet interface is `eth0`. If your device has a different name, use that instead.
  - 示例假设您的以太网接口是 `eth0`。如果设备名称不同，请相应替换。

- The `macvlan` driver is not supported in rootless mode.
  - `macvlan` 驱动程序不支持无根模式。

## 桥接示例 Bridge example

In the simple bridge example, your traffic flows through `eth0` and Docker routes traffic to your container using its MAC address. To network devices on your network, your container appears to be physically attached to the network.

​	在简单的桥接示例中，您的流量通过 `eth0` 流动，Docker 使用其 MAC 地址将流量路由到您的容器。对于网络上的设备而言，您的容器似乎是物理连接到网络的。

1. Create a `macvlan` network called `my-macvlan-net`. Modify the `subnet`, `gateway`, and `parent` values to values that make sense in your environment. 创建一个名为 `my-macvlan-net` 的 `macvlan` 网络。根据您的环境调整 `subnet`、`gateway` 和 `parent` 值。

	```console
   $ docker network create -d macvlan \
     --subnet=172.16.86.0/24 \
     --gateway=172.16.86.1 \
     -o parent=eth0 \
     my-macvlan-net
   ```
   
   You can use `docker network ls` and `docker network inspect my-macvlan-net` commands to verify that the network exists and is a `macvlan` network.
   
   ​	可以使用 `docker network ls` 和 `docker network inspect my-macvlan-net` 命令验证网络是否存在且为 `macvlan` 网络。
   
2. Start an `alpine` container and attach it to the `my-macvlan-net` network. The `-dit` flags start the container in the background but allow you to attach to it. The `--rm` flag means the container is removed when it is stopped.

   启动一个 `alpine` 容器并将其连接到 `my-macvlan-net` 网络。`-dit` 标志将在后台启动容器，但允许您连接到它。`--rm` 标志表示容器停止时会被删除。

   ```console
   $ docker run --rm -dit \
     --network my-macvlan-net \
     --name my-macvlan-alpine \
     alpine:latest \
     ash
   ```

3. Inspect the `my-macvlan-alpine` container and notice the `MacAddress` key within the `Networks` key:

   检查 `my-macvlan-alpine` 容器，并注意 `Networks` 键中的 `MacAddress` 键：

   ```console
   $ docker container inspect my-macvlan-alpine
   
   ...truncated...
   "Networks": {
     "my-macvlan-net": {
         "IPAMConfig": null,
         "Links": null,
         "Aliases": [
             "bec64291cd4c"
         ],
         "NetworkID": "5e3ec79625d388dbcc03dcf4a6dc4548644eb99d58864cf8eee2252dcfc0cc9f",
         "EndpointID": "8caf93c862b22f379b60515975acf96f7b54b7cf0ba0fb4a33cf18ae9e5c1d89",
         "Gateway": "172.16.86.1",
         "IPAddress": "172.16.86.2",
         "IPPrefixLen": 24,
         "IPv6Gateway": "",
         "GlobalIPv6Address": "",
         "GlobalIPv6PrefixLen": 0,
         "MacAddress": "02:42:ac:10:56:02",
         "DriverOpts": null
     }
   }
   ...truncated
   ```

4. Check out how the container sees its own network interfaces by running a couple of `docker exec` commands.

   通过运行一些 `docker exec` 命令查看容器如何看到其自身的网络接口。

   ```console
   $ docker exec my-macvlan-alpine ip addr show eth0
   
   9: eth0@tunl0: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
   link/ether 02:42:ac:10:56:02 brd ff:ff:ff:ff:ff:ff
   inet 172.16.86.2/24 brd 172.16.86.255 scope global eth0
      valid_lft forever preferred_lft forever
   ```

   

   ```console
   $ docker exec my-macvlan-alpine ip route
   
   default via 172.16.86.1 dev eth0
   172.16.86.0/24 dev eth0 scope link  src 172.16.86.2
   ```

5. Stop the container (Docker removes it because of the `--rm` flag), and remove the network.

   停止容器（Docker 因 `--rm` 标志而删除它），并删除网络。

   ```console
   $ docker container stop my-macvlan-alpine
   
   $ docker network rm my-macvlan-net
   ```

## 802.1Q 中继桥接示例 802.1Q trunked bridge example

In the 802.1Q trunked bridge example, your traffic flows through a sub-interface of `eth0` (called `eth0.10`) and Docker routes traffic to your container using its MAC address. To network devices on your network, your container appears to be physically attached to the network.

​	在 802.1Q 中继桥接示例中，您的流量通过 `eth0` 的子接口（称为 `eth0.10`）流动，Docker 使用其 MAC 地址将流量路由到您的容器。对于网络上的设备而言，您的容器似乎是物理连接到网络的。

1. Create a `macvlan` network called `my-8021q-macvlan-net`. Modify the `subnet`, `gateway`, and `parent` values to values that make sense in your environment. 创建一个名为 `my-8021q-macvlan-net` 的 `macvlan` 网络。根据您的环境调整 `subnet`、`gateway` 和 `parent` 值。

   

   ```console
   $ docker network create -d macvlan \
     --subnet=172.16.86.0/24 \
     --gateway=172.16.86.1 \
     -o parent=eth0.10 \
     my-8021q-macvlan-net
   ```

   You can use `docker network ls` and `docker network inspect my-8021q-macvlan-net` commands to verify that the network exists, is a `macvlan` network, and has parent `eth0.10`. You can use `ip addr show` on the Docker host to verify that the interface `eth0.10` exists and has a separate IP address

   ​	您可以使用 `docker network ls` 和 `docker network inspect my-8021q-macvlan-net` 命令来验证网络是否存在，为 `macvlan` 网络，并具有父 `eth0.10`。还可以使用 `ip addr show` 在 Docker 主机上验证 `eth0.10` 接口是否存在并具有单独的 IP 地址。

2. Start an `alpine` container and attach it to the `my-8021q-macvlan-net` network. The `-dit` flags start the container in the background but allow you to attach to it. The `--rm` flag means the container is removed when it is stopped.

   启动一个 `alpine` 容器并将其连接到 `my-8021q-macvlan-net` 网络。`-dit` 标志将在后台启动容器，但允许您连接到它。`--rm` 标志表示容器停止时会被删除。

   ```console
   $ docker run --rm -itd \
     --network my-8021q-macvlan-net \
     --name my-second-macvlan-alpine \
     alpine:latest \
     ash
   ```

3. Inspect the `my-second-macvlan-alpine` container and notice the `MacAddress` key within the `Networks` key:

   检查 `my-second-macvlan-alpine` 容器，并注意 `Networks` 键中的 `MacAddress` 键。

   ```console
   $ docker container inspect my-second-macvlan-alpine
   
   ...truncated...
   "Networks": {
     "my-8021q-macvlan-net": {
         "IPAMConfig": null,
         "Links": null,
         "Aliases": [
             "12f5c3c9ba5c"
         ],
         "NetworkID": "c6203997842e654dd5086abb1133b7e6df627784fec063afcbee5893b2bb64db",
         "EndpointID": "aa08d9aa2353c68e8d2ae0bf0e11ed426ea31ed0dd71c868d22ed0dcf9fc8ae6",
         "Gateway": "172.16.86.1",
         "IPAddress": "172.16.86.2",
         "IPPrefixLen": 24,
         "IPv6Gateway": "",
         "GlobalIPv6Address": "",
         "GlobalIPv6PrefixLen": 0,
         "MacAddress": "02:42:ac:10:56:02",
         "DriverOpts": null
     }
   }
   ...truncated
   ```

4. Check out how the container sees its own network interfaces by running a couple of `docker exec` commands.

   通过运行一些 `docker exec` 命令查看容器如何看到其自身的网络接口。

   ```console
   $ docker exec my-second-macvlan-alpine ip addr show eth0
   
   11: eth0@if10: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
   link/ether 02:42:ac:10:56:02 brd ff:ff:ff:ff:ff:ff
   inet 172.16.86.2/24 brd 172.16.86.255 scope global eth0
      valid_lft forever preferred_lft forever
   ```

   

   ```console
   $ docker exec my-second-macvlan-alpine ip route
   
   default via 172.16.86.1 dev eth0
   172.16.86.0/24 dev eth0 scope link  src 172.16.86.2
   ```

5. Stop the container (Docker removes it because of the `--rm` flag), and remove the network.

   停止容器（Docker 因 `--rm` 标志而删除它），并删除网络。

   ```console
   $ docker container stop my-second-macvlan-alpine
   
   $ docker network rm my-8021q-macvlan-net
   ```

## 其他网络教程 Other networking tutorials

- [Standalone networking tutorial 独立网络教程]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingwithstandalonecontainers" >}})
- [Overlay networking tutorial - Overlay 网络教程]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingwithoverlaynetworks" >}})
- [Host networking tutorial - host 网络教程]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingusingthehostnetwork" >}})
