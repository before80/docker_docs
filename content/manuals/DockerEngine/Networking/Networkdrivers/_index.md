+++
title = "网络驱动程序"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/network/drivers/](https://docs.docker.com/engine/network/drivers/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Network drivers - 网络驱动程序

Docker's networking subsystem is pluggable, using drivers. Several drivers exist by default, and provide core networking functionality:

​	Docker 的网络子系统是可插拔的，使用驱动程序。默认情况下存在多个驱动程序，提供核心的网络功能：

- `bridge`: The default network driver. If you don't specify a driver, this is the type of network you are creating. Bridge networks are commonly used when your application runs in a container that needs to communicate with other containers on the same host. See [Bridge network driver]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Bridgenetworkdriver" >}}).
  - `bridge`：默认网络驱动程序。如果不指定驱动程序，这就是您创建的网络类型。当应用程序运行在需要与同一主机上的其他容器通信的容器中时，通常使用桥接网络。参见 [桥接网络驱动程序]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Bridgenetworkdriver" >}})。

- `host`: Remove network isolation between the container and the Docker host, and use the host's networking directly. See [Host network driver]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Hostnetworkdriver" >}}).
  - `host`：移除容器与 Docker 主机之间的网络隔离，直接使用主机的网络。参见 [主机网络驱动程序]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Hostnetworkdriver" >}})。

- `overlay`: Overlay networks connect multiple Docker daemons together and enable Swarm services and containers to communicate across nodes. This strategy removes the need to do OS-level routing. See [Overlay network driver]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Overlaynetworkdriver" >}}).
  - `overlay`：覆盖网络连接多个 Docker 守护进程，允许 Swarm 服务和容器跨节点通信。这种策略消除了操作系统级别路由的需要。参见 [覆盖网络驱动程序]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Overlaynetworkdriver" >}})。

- `ipvlan`: IPvlan networks give users total control over both IPv4 and IPv6 addressing. The VLAN driver builds on top of that in giving operators complete control of layer 2 VLAN tagging and even IPvlan L3 routing for users interested in underlay network integration. See [IPvlan network driver]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/IPvlannetworkdriver" >}}).
  - `ipvlan`：IPvlan 网络使用户可以完全控制 IPv4 和 IPv6 地址。VLAN 驱动程序在此基础上为操作人员提供了第 2 层 VLAN 标记控制，甚至为需要集成底层网络的用户提供了 IPvlan L3 路由。参见 [IPvlan 网络驱动程序]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/IPvlannetworkdriver" >}})。

- `macvlan`: Macvlan networks allow you to assign a MAC address to a container, making it appear as a physical device on your network. The Docker daemon routes traffic to containers by their MAC addresses. Using the `macvlan` driver is sometimes the best choice when dealing with legacy applications that expect to be directly connected to the physical network, rather than routed through the Docker host's network stack. See [Macvlan network driver]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Macvlannetworkdriver" >}}).
  - `macvlan`：Macvlan 网络允许您为容器分配 MAC 地址，使其在网络中表现为物理设备。Docker 守护进程通过容器的 MAC 地址路由流量。当处理希望直接连接到物理网络而不是通过 Docker 主机的网络堆栈路由的旧应用程序时，`macvlan` 驱动程序有时是最佳选择。参见 [Macvlan 网络驱动程序]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Macvlannetworkdriver" >}})。

- `none`: Completely isolate a container from the host and other containers. `none` is not available for Swarm services. See [None network driver]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Nonenetworkdriver" >}}).
  - `none`：完全隔离容器，使其无法访问主机或其他容器。`none` 不适用于 Swarm 服务。参见 [无网络驱动程序]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Nonenetworkdriver" >}})。

- [Network plugins]({{< ref "/manuals/DockerEngine/DockerEngineplugins/Dockernetworkdriverplugins" >}}): You can install and use third-party network plugins with Docker.
  - [网络插件]({{< ref "/manuals/DockerEngine/DockerEngineplugins/Dockernetworkdriverplugins" >}})：您可以在 Docker 中安装和使用第三方网络插件。


### 网络驱动程序概览 Network driver summary

- The default bridge network is good for running containers that don't require special networking capabilities.

  - 默认的桥接网络适合运行不需要特殊网络功能的容器。

- User-defined bridge networks enable containers on the same Docker host to communicate with each other. A user-defined network typically defines an isolated network for multiple containers belonging to a common project or component.

  - 用户定义的桥接网络允许同一 Docker 主机上的容器相互通信。用户定义的网络通常为属于同一项目或组件的多个容器定义了一个独立网络。

- Host network shares the host's network with the container. When you use this driver, the container's network isn't isolated from the host.

  - 主机网络将主机的网络共享给容器。使用此驱动程序时，容器的网络不会与主机隔离。

- Overlay networks are best when you need containers running on different Docker hosts to communicate, or when multiple applications work together using Swarm services.

  - 当需要不同 Docker 主机上的容器进行通信，或者多个应用程序使用 Swarm 服务协同工作时，覆盖网络是最佳选择。

- Macvlan networks are best when you are migrating from a VM setup or need your containers to look like physical hosts on your network, each with a unique MAC address.

  - Macvlan 网络最适合从虚拟机迁移或需要容器在网络上看起来像物理主机（每个都有唯一的 MAC 地址）的情况。

- IPvlan is similar to Macvlan, but doesn't assign unique MAC addresses to containers. Consider using IPvlan when there's a restriction on the number of MAC addresses that can be assigned to a network interface or port.

  - IPvlan 类似于 Macvlan，但不会为容器分配唯一的 MAC 地址。当对网络接口或端口的 MAC 地址数量有限制时，可以考虑使用 IPvlan。

- Third-party network plugins allow you to integrate Docker with specialized network stacks.

  - 第三方网络插件允许您将 Docker 与特定网络栈集成。

  

## 网络教程 Networking tutorials

Now that you understand the basics about Docker networks, deepen your understanding using the following tutorials:

​	了解 Docker 网络的基础知识后，可以通过以下教程进一步加深理解：

- [Standalone networking tutorial 独立网络教程]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingwithstandalonecontainers" >}})
- [Host networking tutorial 主机网络教程]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingusingthehostnetwork" >}})
- [Overlay networking tutorial 覆盖网络教程]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingwithoverlaynetworks" >}})
- [Macvlan networking tutorial Macvlan 网络教程]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingusingamacvlannetwork" >}})
