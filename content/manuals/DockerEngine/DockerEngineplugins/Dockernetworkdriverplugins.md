+++
title = "Docker 网络驱动插件"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/extend/plugins_network/](https://docs.docker.com/engine/extend/plugins_network/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker network driver plugins - Docker 网络驱动插件

This document describes Docker Engine network driver plugins generally available in Docker Engine. To view information on plugins managed by Docker Engine, refer to [Docker Engine plugin system]({{< ref "/manuals/DockerEngine/DockerEngineplugins" >}}).

​	本文档介绍了 Docker 引擎中普遍可用的网络驱动插件。要查看 Docker 引擎管理的插件信息，请参考 [Docker 引擎插件系统]({{< ref "/manuals/DockerEngine/DockerEngineplugins" >}})。

Docker Engine network plugins enable Engine deployments to be extended to support a wide range of networking technologies, such as VXLAN, IPVLAN, MACVLAN or something completely different. Network driver plugins are supported via the LibNetwork project. Each plugin is implemented as a "remote driver" for LibNetwork, which shares plugin infrastructure with Engine. Effectively, network driver plugins are activated in the same way as other plugins, and use the same kind of protocol.

​	Docker 引擎网络插件使部署可以扩展，以支持多种网络技术，如 VXLAN、IPVLAN、MACVLAN 或其他技术。网络驱动插件通过 LibNetwork 项目支持。每个插件都作为 LibNetwork 的 "远程驱动" 实现，与引擎共享插件基础设施。实际上，网络驱动插件的激活方式与其他插件相同，并使用相同的协议。

## 网络插件与 Swarm 模式 Network plugins and Swarm mode

[Legacy plugins]({{< ref "/manuals/DockerEngine/DockerEngineplugins/UseDockerEngineplugins" >}}) do not work in Swarm mode. However, plugins written using the [v2 plugin system]({{< ref "/manuals/DockerEngine/DockerEngineplugins" >}}) do work in Swarm mode, as long as they are installed on each Swarm worker node.

​	[传统插件]({{< ref "/manuals/DockerEngine/DockerEngineplugins/UseDockerEngineplugins" >}})不支持 Swarm 模式。然而，使用 [v2 插件系统]({{< ref "/manuals/DockerEngine/DockerEngineplugins" >}})编写的插件可以在 Swarm 模式下使用，但需确保在每个 Swarm 工作节点上安装。

## 使用网络驱动插件 Use network driver plugins

The means of installing and running a network driver plugin depend on the particular plugin. So, be sure to install your plugin according to the instructions obtained from the plugin developer.

​	安装和运行网络驱动插件的方式取决于具体的插件。因此，请根据插件开发人员提供的说明安装您的插件。

Once running however, network driver plugins are used just like the built-in network drivers: by being mentioned as a driver in network-oriented Docker commands. For example,

​	一旦运行，网络驱动插件就像内置的网络驱动一样使用，例如，在网络相关的 Docker 命令中指定驱动程序：



```console
$ docker network create --driver weave mynet
```

Some network driver plugins are listed in [plugins]({{< ref "/manuals/DockerEngine/DockerEngineplugins/UseDockerEngineplugins" >}})

​	一些网络驱动插件列在 [插件列表]({{< ref "/manuals/DockerEngine/DockerEngineplugins/UseDockerEngineplugins" >}})中。

The `mynet` network is now owned by `weave`, so subsequent commands referring to that network will be sent to the plugin,

​	现在，`mynet` 网络由 `weave` 拥有，因此引用该网络的后续命令将发送到该插件：

```console
$ docker run --network=mynet busybox top
```

## 查找网络插件 Find network plugins

Network plugins are written by third parties, and are published by those third parties, either on [Docker Hub](https://hub.docker.com/search?q=&type=plugin) or on the third party's site.

​	网络插件由第三方编写并发布，通常发布在 [Docker Hub](https://hub.docker.com/search?q=&type=plugin) 或第三方网站上。

## 编写网络插件 Write a network plugin

Network plugins implement the [Docker plugin API]({{< ref "/manuals/DockerEngine/DockerEngineplugins/DockerPluginAPI" >}}) and the network plugin protocol

​	网络插件实现 [Docker 插件 API]({{< ref "/manuals/DockerEngine/DockerEngineplugins/DockerPluginAPI" >}})和网络插件协议。

## 网络插件协议 Network plugin protocol

The network driver protocol, in addition to the plugin activation call, is documented as part of libnetwork: https://github.com/moby/moby/blob/master/libnetwork/docs/remote.md.

​	网络驱动协议以及插件激活调用的相关文档可以在 libnetwork 中找到：https://github.com/moby/moby/blob/master/libnetwork/docs/remote.md

## 相关信息 Related Information

To interact with the Docker maintainers and other interested users, see the IRC channel `#docker-network`.

​	与 Docker 维护者和其他感兴趣的用户互动，请参阅 IRC 频道 `#docker-network`。

- [Docker networks feature overview Docker 网络功能概述]({{< ref "/manuals/DockerEngine/Networking">}})

- The [LibNetwork](https://github.com/docker/libnetwork) project
