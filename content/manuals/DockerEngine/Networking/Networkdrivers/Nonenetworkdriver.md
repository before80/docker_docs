+++
title = "None 网络驱动"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/network/drivers/none/](https://docs.docker.com/engine/network/drivers/none/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# None network driver - None 网络驱动

If you want to completely isolate the networking stack of a container, you can use the `--network none` flag when starting the container. Within the container, only the loopback device is created.

​	如果您想完全隔离容器的网络栈，可以在启动容器时使用 `--network none` 标志。在容器内，只有回环设备会被创建

The following example shows the output of `ip link show` in an `alpine` container using the `none` network driver.

​	以下示例展示了使用 `none` 网络驱动的 `alpine` 容器中运行 `ip link show` 的输出结果。



```console
$ docker run --rm --network none alpine:latest ip link show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
```

No IPv6 loopback address is configured for containers using the `none` driver.

​	对于使用 `none` 驱动的容器，没有配置 IPv6 回环地址。



```console
$ docker run --rm --network none --name no-net-alpine alpine:latest ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
```

## 接下来 Next steps

- Go through the [host nworking tutorial]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingusingthehostnetwork" >}})
  - 了解 [host网络教程]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingusingthehostnetwork" >}})
- Learn about [networking from t container's point of view]({{< ref "/manuals/DockerEngine/Networking" >}})
  - 学习 [从容器的角度了解网络]({{< ref "/manuals/DockerEngine/Networking" >}})
- Learn about [bridge networks]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Bridgenetworkdriver" >}})
  - 学习 [桥接网络]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Bridgenetworkdriver" >}})
- Learn about [overlay networks]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Overlaynetworkdriver" >}})
  - 学习 [覆盖网络]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Overlaynetworkdriver" >}})
- Learn about [Macvlan networks]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Macvlannetworkdriver" >}})
  - 学习 [Macvlan 网络]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Macvlannetworkdriver" >}})
