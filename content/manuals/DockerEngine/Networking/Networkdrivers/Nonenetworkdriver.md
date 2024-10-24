+++
title = "None network driver"
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

# None network driver

If you want to completely isolate the networking stack of a container, you can use the `--network none` flag when starting the container. Within the container, only the loopback device is created.

The following example shows the output of `ip link show` in an `alpine` container using the `none` network driver.



```console
$ docker run --rm --network none alpine:latest ip link show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
```

No IPv6 loopback address is configured for containers using the `none` driver.



```console
$ docker run --rm --network none --name no-net-alpine alpine:latest ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
```

## Next steps

- Go through the [host networking tutorial]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingusingthehostnetwork" >}})
- Learn about [networking from the container's point of view]({{< ref "/manuals/DockerEngine/Networking" >}})
- Learn about [bridge networks]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Bridgenetworkdriver" >}})
- Learn about [overlay networks]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Overlaynetworkdriver" >}})
- Learn about [Macvlan networks]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Macvlannetworkdriver" >}})
