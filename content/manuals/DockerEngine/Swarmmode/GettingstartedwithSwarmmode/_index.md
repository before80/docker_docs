+++
title = "Getting started with Swarm mode"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/engine/swarm/swarm-tutorial/](https://docs.docker.com/engine/swarm/swarm-tutorial/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Getting started with Swarm mode - Swarm 模式入门

This tutorial introduces you to the features of Docker Engine Swarm mode. You may want to familiarize yourself with the [key concepts]({{< ref "/manuals/DockerEngine/Swarmmode/Swarmmodekeyconcepts" >}}) before you begin.

​	本教程将带您了解 Docker Engine Swarm 模式的功能。您可能希望在开始之前先熟悉一下 [关键概念]({{< ref "/manuals/DockerEngine/Swarmmode/Swarmmodekeyconcepts" >}})。

The tutorial guides you through:

​	本教程涵盖以下内容：

- Initializing a cluster of Docker Engines in swarm mode
  - 在 Swarm 模式下初始化 Docker Engine 集群

- Adding nodes to the swarm
  - 向 Swarm 中添加节点

- Deploying application services to the swarm
  - 将应用程序服务部署到 Swarm 中

- Managing the swarm once you have everything running
  - 在一切运行后管理 Swarm


This tutorial uses Docker Engine CLI commands entered on the command line of a terminal window.

​	本教程使用 Docker Engine CLI 命令，这些命令在终端窗口的命令行中输入。

If you are brand new to Docker, see [About Docker Engine]({{< ref "/manuals/DockerEngine" >}}).

​	如果您是 Docker 的新手，请参阅 [关于 Docker Engine]({{< ref "/manuals/DockerEngine" >}})。

## Set up

To run this tutorial, you need:

​	要运行本教程，您需要：

- [Three Linux hosts which can communicate over a network, with Docker installed 三台可以通过网络通信且已安装 Docker 的 Linux 主机](https://docs.docker.com/engine/swarm/swarm-tutorial/#three-networked-host-machines)
- [The IP address of the manager machine 管理器机器的 IP 地址](https://docs.docker.com/engine/swarm/swarm-tutorial/#the-ip-address-of-the-manager-machine)
- [Open ports between the hosts 主机之间的开放端口](https://docs.docker.com/engine/swarm/swarm-tutorial/#open-protocols-and-ports-between-the-hosts)

### 三台联网的主机 Three networked host machines

This tutorial requires three Linux hosts which have Docker installed and can communicate over a network. These can be physical machines, virtual machines, Amazon EC2 instances, or hosted in some other way. Check out [Deploy to Swarm](https://docs.docker.com/guides/swarm-deploy/#prerequisites) for one possible set-up for the hosts.

​	本教程需要三台已安装 Docker 并且可以通过网络通信的 Linux 主机。这些主机可以是物理机、虚拟机、Amazon EC2 实例或其他托管方式。请查看 [部署到 Swarm](https://docs.docker.com/guides/swarm-deploy/#prerequisites) 以获取主机的可能设置方法。

One of these machines is a manager (called `manager1`) and two of them are workers (`worker1` and `worker2`).

​	其中一台机器是管理器（称为 `manager1`），另外两台是工作节点（`worker1` 和 `worker2`）。

> **Note**
>
> You can follow many of the tutorial steps to test single-node swarm as well, in which case you need only one host. Multi-node commands do not work, but you can initialize a swarm, create services, and scale them.
>
> ​	您也可以按照教程中的大部分步骤测试单节点 Swarm，在这种情况下，只需要一台主机。虽然多节点命令将不起作用，但您可以初始化 Swarm、创建服务并对其进行扩展。

#### 在 Linux 机器上安装 Docker Engine - Install Docker Engine on Linux machines

If you are using Linux based physical computers or cloud-provided computers as hosts, simply follow the [Linux install instructions]({{< ref "/manuals/DockerEngine/Install" >}}) for your platform. Spin up the three machines, and you are ready. You can test both single-node and multi-node swarm scenarios on Linux machines.

​	如果您使用基于 Linux 的物理计算机或云提供的计算机作为主机，只需按照您的平台的 [Linux 安装说明]({{< ref "/manuals/DockerEngine/Install" >}})。启动三台机器后，您就可以开始了。您可以在 Linux 机器上测试单节点和多节点的 Swarm 场景。

### 管理器机器的 IP 地址 The IP address of the manager machine

The IP address must be assigned to a network interface available to the host operating system. All nodes in the swarm need to connect to the manager at the IP address.

​	该 IP 地址必须分配给主机操作系统可用的网络接口。Swarm 中的所有节点都需要在该 IP 地址上连接到管理器。

Because other nodes contact the manager node on its IP address, you should use a fixed IP address.

​	由于其他节点通过 IP 地址联系管理器节点，建议使用固定 IP 地址。

You can run `ifconfig` on Linux or macOS to see a list of the available network interfaces.

​	您可以在 Linux 或 macOS 上运行 `ifconfig` 以查看可用网络接口的列表。

The tutorial uses `manager1` : `192.168.99.100`.

​	本教程使用 `manager1` 的 IP 地址：`192.168.99.100`。

### 主机之间的开放协议和端口 Open protocols and ports between the hosts

The following ports must be available. On some systems, these ports are open by default.

​	以下端口必须是可用的。在某些系统中，这些端口默认开放。

- Port `2377` TCP for communication with and between manager nodes
  - `2377` 端口 TCP 用于与管理节点之间的通信

- Port `7946` TCP/UDP for overlay network node discovery
  - `7946` 端口 TCP/UDP 用于覆盖网络节点发现

- Port `4789` UDP (configurable) for overlay network traffic
  - `4789` 端口 UDP（可配置）用于覆盖网络流量


If you plan on creating an overlay network with encryption (`--opt encrypted`), you also need to ensure IP protocol 50 (IPSec ESP) traffic is allowed.

​	如果计划创建带加密的覆盖网络（`--opt encrypted`），还需要确保允许 IP 协议 50（IPSec ESP）流量。

Port `4789` is the default value for the Swarm data path port, also known as the VXLAN port. It is important to prevent any untrusted traffic from reaching this port, as VXLAN does not provide authentication. This port should only be opened to a trusted network, and never at a perimeter firewall.

​	`4789` 端口是 Swarm 数据路径端口的默认值，也称为 VXLAN 端口。应防止任何不受信任的流量到达此端口，因为 VXLAN 不提供身份验证。此端口仅应向受信任的网络开放，绝不能在外部防火墙处开放。

If the network which Swarm traffic traverses is not fully trusted, it is strongly suggested that encrypted overlay networks be used. If encrypted overlay networks are in exclusive use, some additional hardening is suggested:

​	如果 Swarm 流量通过的网络不是完全可信的，强烈建议使用加密的覆盖网络。如果仅使用加密的覆盖网络，建议进行一些额外的加固：

- [Customize the default ingress network]({{< ref "/manuals/DockerEngine/Swarmmode/Manageswarmservicenetworks" >}}) to use encryption
  - [自定义默认的入口网络]({{< ref "/manuals/DockerEngine/Swarmmode/Manageswarmservicenetworks" >}}) 以使用加密

- Only accept encrypted packets on the Data Path Port:
  - 仅接受数据路径端口上的加密数据包：




```bash
# Example iptables rule (order and other tools may require customization)
# 示例 iptables 规则（顺序和其他工具可能需要自定义）
iptables -I INPUT -m udp --dport 4789 -m policy --dir in --pol none -j DROP
```

## 接下来 Next steps

Next, you'll create a swarm.

​	接下来，您将创建一个 Swarm。

[Create a swarm]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Createaswarm" >}}) [创建 Swarm]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Createaswarm" >}})
