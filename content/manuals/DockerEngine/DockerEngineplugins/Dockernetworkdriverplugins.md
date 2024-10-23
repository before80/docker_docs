+++
title = "Docker network driver plugins"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/engine/extend/plugins_network/](https://docs.docker.com/engine/extend/plugins_network/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker network driver plugins

This document describes Docker Engine network driver plugins generally available in Docker Engine. To view information on plugins managed by Docker Engine, refer to [Docker Engine plugin system](https://docs.docker.com/engine/extend/).

Docker Engine network plugins enable Engine deployments to be extended to support a wide range of networking technologies, such as VXLAN, IPVLAN, MACVLAN or something completely different. Network driver plugins are supported via the LibNetwork project. Each plugin is implemented as a "remote driver" for LibNetwork, which shares plugin infrastructure with Engine. Effectively, network driver plugins are activated in the same way as other plugins, and use the same kind of protocol.

## [Network plugins and Swarm mode](https://docs.docker.com/engine/extend/plugins_network/#network-plugins-and-swarm-mode)

[Legacy plugins](https://docs.docker.com/engine/extend/legacy_plugins/) do not work in Swarm mode. However, plugins written using the [v2 plugin system](https://docs.docker.com/engine/extend/) do work in Swarm mode, as long as they are installed on each Swarm worker node.

## [Use network driver plugins](https://docs.docker.com/engine/extend/plugins_network/#use-network-driver-plugins)

The means of installing and running a network driver plugin depend on the particular plugin. So, be sure to install your plugin according to the instructions obtained from the plugin developer.

Once running however, network driver plugins are used just like the built-in network drivers: by being mentioned as a driver in network-oriented Docker commands. For example,



```console
$ docker network create --driver weave mynet
```

Some network driver plugins are listed in [plugins](https://docs.docker.com/engine/extend/legacy_plugins/)

The `mynet` network is now owned by `weave`, so subsequent commands referring to that network will be sent to the plugin,



```console
$ docker run --network=mynet busybox top
```

## [Find network plugins](https://docs.docker.com/engine/extend/plugins_network/#find-network-plugins)

Network plugins are written by third parties, and are published by those third parties, either on [Docker Hub](https://hub.docker.com/search?q=&type=plugin) or on the third party's site.

## [Write a network plugin](https://docs.docker.com/engine/extend/plugins_network/#write-a-network-plugin)

Network plugins implement the [Docker plugin API](https://docs.docker.com/engine/extend/plugin_api/) and the network plugin protocol

## [Network plugin protocol](https://docs.docker.com/engine/extend/plugins_network/#network-plugin-protocol)

The network driver protocol, in addition to the plugin activation call, is documented as part of libnetwork: https://github.com/moby/moby/blob/master/libnetwork/docs/remote.md.

## [Related Information](https://docs.docker.com/engine/extend/plugins_network/#related-information)

To interact with the Docker maintainers and other interested users, see the IRC channel `#docker-network`.

- [Docker networks feature overview](https://docs.docker.com/engine/userguide/networking/)
- The [LibNetwork](https://github.com/docker/libnetwork) project
