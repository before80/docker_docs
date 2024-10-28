+++
title = "独立容器的网络配置"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/network/tutorials/standalone/](https://docs.docker.com/engine/network/tutorials/standalone/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Networking with standalone containers - 独立容器的网络配置

This series of tutorials deals with networking for standalone Docker containers. For networking with swarm services, see [Networking with swarm services](https://docs.docker.com/engine/network/tutorials/overlay/). If you need to learn more about Docker networking in general, see the [overview](https://docs.docker.com/engine/network/tutorials/standalone/).

​	本教程系列介绍了如何配置独立 Docker 容器的网络。关于 swarm 服务的网络配置，请参阅 [swarm 服务的网络配置](https://docs.docker.com/engine/network/tutorials/overlay/)。若需了解 Docker 网络的更多内容，请参阅 [概述](https://docs.docker.com/engine/network/tutorials/standalone/)。

This topic includes two different tutorials. You can run each of them on Linux, Windows, or a Mac, but for the last one, you need a second Docker host running elsewhere.

​	本主题包括两个不同的教程。你可以在 Linux、Windows 或 Mac 上运行它们，但对于最后一个，你需要在其他位置运行第二个 Docker 主机。

- [Use the default bridge network](https://docs.docker.com/engine/network/tutorials/standalone/#use-the-default-bridge-network) demonstrates how to use the default `bridge` network that Docker sets up for you automatically. This network is not the best choice for production systems.
  - [使用默认桥接网络](https://docs.docker.com/engine/network/tutorials/standalone/#use-the-default-bridge-network) 演示如何使用 Docker 自动设置的默认 `bridge` 网络。该网络不适合用于生产系统。

- [Use user-defined bridge networks](https://docs.docker.com/engine/network/tutorials/standalone/#use-user-defined-bridge-networks) shows how to create and use your own custom bridge networks, to connect containers running on the same Docker host. This is recommended for standalone containers running in production.
  - [使用用户定义的桥接网络](https://docs.docker.com/engine/network/tutorials/standalone/#use-user-defined-bridge-networks) 展示了如何创建并使用自定义桥接网络，以连接在同一 Docker 主机上运行的容器。这对于生产中的独立容器是推荐的。


Although [overlay networks](https://docs.docker.com/engine/network/drivers/overlay/) are generally used for swarm services, you can also use an overlay network for standalone containers. That's covered as part of the [tutorial on using overlay networks](https://docs.docker.com/engine/network/tutorials/overlay/#use-an-overlay-network-for-standalone-containers).

​	尽管 [overlay 网络](https://docs.docker.com/engine/network/drivers/overlay/) 通常用于 swarm 服务，你也可以在独立容器中使用 overlay 网络。可以在 [使用 overlay 网络的教程](https://docs.docker.com/engine/network/tutorials/overlay/#use-an-overlay-network-for-standalone-containers) 中查看相关内容。

## 使用默认桥接网络 Use the default bridge network

In this example, you start two different `alpine` containers on the same Docker host and do some tests to understand how they communicate with each other. You need to have Docker installed and running.

​	在此示例中，你将在同一 Docker 主机上启动两个不同的 `alpine` 容器，并进行一些测试以了解它们如何相互通信。你需要确保 Docker 已安装并运行。

1. Open a terminal window. List current networks before you do anything else. Here's what you should see if you've never added a network or initialized a swarm on this Docker daemon. You may see different networks, but you should at least see these (the network IDs will be different):

   打开终端窗口。在开始其他操作之前，先列出当前的网络。如果你从未在该 Docker 守护进程上添加过网络或初始化过 swarm，你应该看到类似以下内容（网络 ID 会有所不同）：

   ```console
   $ docker network ls
   
   NETWORK ID          NAME                DRIVER              SCOPE
   17e324f45964        bridge              bridge              local
   6ed54d316334        host                host                local
   7092879f2cc8        none                null                local
   ```

   The default `bridge` network is listed, along with `host` and `none`. The latter two are not fully-fledged networks, but are used to start a container connected directly to the Docker daemon host's networking stack, or to start a container with no network devices. This tutorial will connect two containers to the `bridge` network.

   ​	默认的 `bridge` 网络会列出，同时列出 `host` 和 `none`。后两个并非完全的网络，而是用于启动直接连接到 Docker 守护进程主机的网络栈的容器，或启动没有网络设备的容器。本教程将把两个容器连接到 `bridge` 网络。

2. Start two `alpine` containers running `ash`, which is Alpine's default shell rather than `bash`. The `-dit` flags mean to start the container detached (in the background), interactive (with the ability to type into it), and with a TTY (so you can see the input and output). Since you are starting it detached, you won't be connected to the container right away. Instead, the container's ID will be printed. Because you have not specified any `--network` flags, the containers connect to the default `bridge` network.

   启动两个运行 `ash` 的 `alpine` 容器，`ash` 是 Alpine 的默认 shell。`-dit` 参数表示后台启动（分离模式），支持交互（可以输入命令）并启用 TTY（可以看到输入输出）。由于在分离模式下启动，你不会直接连接到容器，而是会显示容器的 ID。因为未指定任何 `--network` 参数，所以容器将连接到默认的 `bridge` 网络。

   ```console
   $ docker run -dit --name alpine1 alpine ash
   
   $ docker run -dit --name alpine2 alpine ash
   ```

   Check that both containers are actually started:

   检查两个容器是否已启动：

   ```console
   $ docker container ls
   
   CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
   602dbf1edc81        alpine              "ash"               4 seconds ago       Up 3 seconds                            alpine2
   da33b7aa74b0        alpine              "ash"               17 seconds ago      Up 16 seconds                           alpine1
   ```

3. Inspect the `bridge` network to see what containers are connected to it.

   检查 `bridge` 网络以查看哪些容器连接到该网络。

   ```console
   $ docker network inspect bridge
   
   [
       {
           "Name": "bridge",
           "Id": "17e324f459648a9baaea32b248d3884da102dde19396c25b30ec800068ce6b10",
           "Created": "2017-06-22T20:27:43.826654485Z",
           "Scope": "local",
           "Driver": "bridge",
           "EnableIPv6": false,
           "IPAM": {
               "Driver": "default",
               "Options": null,
               "Config": [
                   {
                       "Subnet": "172.17.0.0/16",
                       "Gateway": "172.17.0.1"
                   }
               ]
           },
           "Internal": false,
           "Attachable": false,
           "Containers": {
               "602dbf1edc81813304b6cf0a647e65333dc6fe6ee6ed572dc0f686a3307c6a2c": {
                   "Name": "alpine2",
                   "EndpointID": "03b6aafb7ca4d7e531e292901b43719c0e34cc7eef565b38a6bf84acf50f38cd",
                   "MacAddress": "02:42:ac:11:00:03",
                   "IPv4Address": "172.17.0.3/16",
                   "IPv6Address": ""
               },
               "da33b7aa74b0bf3bda3ebd502d404320ca112a268aafe05b4851d1e3312ed168": {
                   "Name": "alpine1",
                   "EndpointID": "46c044a645d6afc42ddd7857d19e9dcfb89ad790afb5c239a35ac0af5e8a5bc5",
                   "MacAddress": "02:42:ac:11:00:02",
                   "IPv4Address": "172.17.0.2/16",
                   "IPv6Address": ""
               }
           },
           "Options": {
               "com.docker.network.bridge.default_bridge": "true",
               "com.docker.network.bridge.enable_icc": "true",
               "com.docker.network.bridge.enable_ip_masquerade": "true",
               "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
               "com.docker.network.bridge.name": "docker0",
               "com.docker.network.driver.mtu": "1500"
           },
           "Labels": {}
       }
   ]
   ```

   Near the top, information about the `bridge` network is listed, including the IP address of the gateway between the Docker host and the `bridge` network (`172.17.0.1`). Under the `Containers` key, each connected container is listed, along with information about its IP address (`172.17.0.2` for `alpine1` and `172.17.0.3` for `alpine2`).

   ​	在顶部，可以看到 `bridge` 网络的信息，包括 Docker 主机和 `bridge` 网络之间的网关 IP 地址（`172.17.0.1`）。在 `Containers` 键下，列出了每个连接的容器以及其 IP 地址信息（`alpine1` 为 `172.17.0.2`，`alpine2` 为 `172.17.0.3`）。

4. The containers are running in the background. Use the `docker attach` command to connect to `alpine1`.

   容器在后台运行。使用 `docker attach` 命令连接到 `alpine1`。

   ```console
   $ docker attach alpine1
   
   / #
   ```

   The prompt changes to `#` to indicate that you are the `root` user within the container. Use the `ip addr show` command to show the network interfaces for `alpine1` as they look from within the container:

   提示符变为 `#`，表示你是容器中的 `root` 用户。使用 `ip addr show` 命令查看容器内 `alpine1` 的网络接口：

   ```console
   # ip addr show
   
   1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1
       link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
       inet 127.0.0.1/8 scope host lo
          valid_lft forever preferred_lft forever
       inet6 ::1/128 scope host
          valid_lft forever preferred_lft forever
   27: eth0@if28: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
       link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff
       inet 172.17.0.2/16 scope global eth0
          valid_lft forever preferred_lft forever
       inet6 fe80::42:acff:fe11:2/64 scope link
          valid_lft forever preferred_lft forever
   ```

   The first interface is the loopback device. Ignore it for now. Notice that the second interface has the IP address `172.17.0.2`, which is the same address shown for `alpine1` in the previous step.

   ​	第一个接口是环回设备，可忽略。注意，第二个接口的 IP 地址是 `172.17.0.2`，与上一步显示的 `alpine1` 地址相同。

5. From within `alpine1`, make sure you can connect to the internet by pinging `google.com`. The `-c 2` flag limits the command to two `ping` attempts.

   在 `alpine1` 中，ping `google.com` 以确保可以连接到互联网。`-c 2` 参数限制 ping 两次。

   ```console
   # ping -c 2 google.com
   
   PING google.com (172.217.3.174): 56 data bytes
   64 bytes from 172.217.3.174: seq=0 ttl=41 time=9.841 ms
   64 bytes from 172.217.3.174: seq=1 ttl=41 time=9.897 ms
   
   --- google.com ping statistics ---
   2 packets transmitted, 2 packets received, 0% packet loss
   round-trip min/avg/max = 9.841/9.869/9.897 ms
   ```

6. Now try to ping the second container. First, ping it by its IP address, `172.17.0.3`:

   现在尝试 ping 第二个容器。首先，通过 IP 地址 `172.17.0.3` 进行 ping：

   ```console
   # ping -c 2 172.17.0.3
   
   PING 172.17.0.3 (172.17.0.3): 56 data bytes
   64 bytes from 172.17.0.3: seq=0 ttl=64 time=0.086 ms
   64 bytes from 172.17.0.3: seq=1 ttl=64 time=0.094 ms
   
   --- 172.17.0.3 ping statistics ---
   2 packets transmitted, 2 packets received, 0% packet loss
   round-trip min/avg/max = 0.086/0.090/0.094 ms
   ```

   This succeeds. Next, try pinging the `alpine2` container by container name. This will fail.

   此操作成功。接下来，尝试通过容器名称 ping `alpine2`。该操作将失败：

   ```console
   # ping -c 2 alpine2
   
   ping: bad address 'alpine2'
   ```

7. Detach from `alpine1` without stopping it by using the detach sequence, `CTRL` + `p` `CTRL` + `q` (hold down `CTRL` and type `p` followed by `q`). If you wish, attach to `alpine2` and repeat steps 4, 5, and 6 there, substituting `alpine1` for `alpine2`. 使用分离序列 `CTRL` + `p` `CTRL` + `q` 退出 `alpine1` 而不停止它（按住 `CTRL` 键，依次输入 `p` 和 `q`）。如果需要，可以连接 `alpine2` 并在其中重复步骤 4、5 和 6，将 `alpine2` 替换为 `alpine1`。

8. Stop and remove both containers.

   停止并删除两个容器。

   ```console
   $ docker container stop alpine1 alpine2
   $ docker container rm alpine1 alpine2
   ```

Remember, the default `bridge` network is not recommended for production. To learn about user-defined bridge networks, continue to the [next tutorial](https://docs.docker.com/engine/network/tutorials/standalone/#use-user-defined-bridge-networks).

​	请记住，默认的 `bridge` 网络不建议用于生产环境。要了解用户定义的桥接网络，请继续阅读 [下一个教程](https://docs.docker.com/engine/network/tutorials/standalone/#use-user-defined-bridge-networks)。

## 使用用户定义的桥接网络 Use user-defined bridge networks

In this example, we again start two `alpine` containers, but attach them to a user-defined network called `alpine-net` which we have already created. These containers are not connected to the default `bridge` network at all. We then start a third `alpine` container which is connected to the `bridge` network but not connected to `alpine-net`, and a fourth `alpine` container which is connected to both networks.

​	在本示例中，我们将再次启动两个 `alpine` 容器，但将它们连接到我们已创建的名为 `alpine-net` 的用户定义网络。这些容器完全不会连接到默认的 `bridge` 网络。然后，我们将启动一个第三个连接到 `bridge` 网络的 `alpine` 容器，但不连接到 `alpine-net`，以及一个连接到这两个网络的第四个 `alpine` 容器。

1. Create the `alpine-net` network. You do not need the `--driver bridge` flag since it's the default, but this example shows how to specify it.

   创建 `alpine-net` 网络。你不需要使用 `--driver bridge` 标志，因为 `bridge` 是默认值，但在本示例中展示了如何指定它。

   ```console
   $ docker network create --driver bridge alpine-net
   ```

2. List Docker's networks:

   列出 Docker 的网络：

   ```console
   $ docker network ls
   
   NETWORK ID          NAME                DRIVER              SCOPE
   e9261a8c9a19        alpine-net          bridge              local
   17e324f45964        bridge              bridge              local
   6ed54d316334        host                host                local
   7092879f2cc8        none                null                local
   ```

   Inspect the `alpine-net` network. This shows you its IP address and the fact that no containers are connected to it:

   检查 `alpine-net` 网络。这会显示其 IP 地址，并显示尚无容器连接到该网络：

   ```console
   $ docker network inspect alpine-net
   
   [
       {
           "Name": "alpine-net",
           "Id": "e9261a8c9a19eabf2bf1488bf5f208b99b1608f330cff585c273d39481c9b0ec",
           "Created": "2017-09-25T21:38:12.620046142Z",
           "Scope": "local",
           "Driver": "bridge",
           "EnableIPv6": false,
           "IPAM": {
               "Driver": "default",
               "Options": {},
               "Config": [
                   {
                       "Subnet": "172.18.0.0/16",
                       "Gateway": "172.18.0.1"
                   }
               ]
           },
           "Internal": false,
           "Attachable": false,
           "Containers": {},
           "Options": {},
           "Labels": {}
       }
   ]
   ```

   Notice that this network's gateway is `172.18.0.1`, as opposed to the default bridge network, whose gateway is `172.17.0.1`. The exact IP address may be different on your system.

   ​	请注意，此网络的网关是 `172.18.0.1`，而默认 `bridge` 网络的网关是 `172.17.0.1`。在你的系统上具体 IP 地址可能有所不同。

3. Create your four containers. Notice the `--network` flags. You can only connect to one network during the `docker run` command, so you need to use `docker network connect` afterward to connect `alpine4` to the `bridge` network as well.

   创建四个容器。注意 `--network` 参数。在 `docker run` 命令中只能连接一个网络，因此你需要之后使用 `docker network connect` 将 `alpine4` 连接到 `bridge` 网络。

   ```console
   $ docker run -dit --name alpine1 --network alpine-net alpine ash
   
   $ docker run -dit --name alpine2 --network alpine-net alpine ash
   
   $ docker run -dit --name alpine3 alpine ash
   
   $ docker run -dit --name alpine4 --network alpine-net alpine ash
   
   $ docker network connect bridge alpine4
   ```

   Verify that all containers are running:

   确认所有容器正在运行：

   ```console
   $ docker container ls
   
   CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
   156849ccd902        alpine              "ash"               41 seconds ago       Up 41 seconds                           alpine4
   fa1340b8d83e        alpine              "ash"               51 seconds ago       Up 51 seconds                           alpine3
   a535d969081e        alpine              "ash"               About a minute ago   Up About a minute                       alpine2
   0a02c449a6e9        alpine              "ash"               About a minute ago   Up About a minute                       alpine1
   ```

4. Inspect the `bridge` network and the `alpine-net` network again:

   再次检查 `bridge` 网络和 `alpine-net` 网络：

   ```console
   $ docker network inspect bridge
   
   [
       {
           "Name": "bridge",
           "Id": "17e324f459648a9baaea32b248d3884da102dde19396c25b30ec800068ce6b10",
           "Created": "2017-06-22T20:27:43.826654485Z",
           "Scope": "local",
           "Driver": "bridge",
           "EnableIPv6": false,
           "IPAM": {
               "Driver": "default",
               "Options": null,
               "Config": [
                   {
                       "Subnet": "172.17.0.0/16",
                       "Gateway": "172.17.0.1"
                   }
               ]
           },
           "Internal": false,
           "Attachable": false,
           "Containers": {
               "156849ccd902b812b7d17f05d2d81532ccebe5bf788c9a79de63e12bb92fc621": {
                   "Name": "alpine4",
                   "EndpointID": "7277c5183f0da5148b33d05f329371fce7befc5282d2619cfb23690b2adf467d",
                   "MacAddress": "02:42:ac:11:00:03",
                   "IPv4Address": "172.17.0.3/16",
                   "IPv6Address": ""
               },
               "fa1340b8d83eef5497166951184ad3691eb48678a3664608ec448a687b047c53": {
                   "Name": "alpine3",
                   "EndpointID": "5ae767367dcbebc712c02d49556285e888819d4da6b69d88cd1b0d52a83af95f",
                   "MacAddress": "02:42:ac:11:00:02",
                   "IPv4Address": "172.17.0.2/16",
                   "IPv6Address": ""
               }
           },
           "Options": {
               "com.docker.network.bridge.default_bridge": "true",
               "com.docker.network.bridge.enable_icc": "true",
               "com.docker.network.bridge.enable_ip_masquerade": "true",
               "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
               "com.docker.network.bridge.name": "docker0",
               "com.docker.network.driver.mtu": "1500"
           },
           "Labels": {}
       }
   ]
   ```

   Containers `alpine3` and `alpine4` are connected to the `bridge` network.

   容器 `alpine3` 和 `alpine4` 已连接到 `bridge` 网络。

   ```console
   $ docker network inspect alpine-net
   
   [
       {
           "Name": "alpine-net",
           "Id": "e9261a8c9a19eabf2bf1488bf5f208b99b1608f330cff585c273d39481c9b0ec",
           "Created": "2017-09-25T21:38:12.620046142Z",
           "Scope": "local",
           "Driver": "bridge",
           "EnableIPv6": false,
           "IPAM": {
               "Driver": "default",
               "Options": {},
               "Config": [
                   {
                       "Subnet": "172.18.0.0/16",
                       "Gateway": "172.18.0.1"
                   }
               ]
           },
           "Internal": false,
           "Attachable": false,
           "Containers": {
               "0a02c449a6e9a15113c51ab2681d72749548fb9f78fae4493e3b2e4e74199c4a": {
                   "Name": "alpine1",
                   "EndpointID": "c83621678eff9628f4e2d52baf82c49f974c36c05cba152db4c131e8e7a64673",
                   "MacAddress": "02:42:ac:12:00:02",
                   "IPv4Address": "172.18.0.2/16",
                   "IPv6Address": ""
               },
               "156849ccd902b812b7d17f05d2d81532ccebe5bf788c9a79de63e12bb92fc621": {
                   "Name": "alpine4",
                   "EndpointID": "058bc6a5e9272b532ef9a6ea6d7f3db4c37527ae2625d1cd1421580fd0731954",
                   "MacAddress": "02:42:ac:12:00:04",
                   "IPv4Address": "172.18.0.4/16",
                   "IPv6Address": ""
               },
               "a535d969081e003a149be8917631215616d9401edcb4d35d53f00e75ea1db653": {
                   "Name": "alpine2",
                   "EndpointID": "198f3141ccf2e7dba67bce358d7b71a07c5488e3867d8b7ad55a4c695ebb8740",
                   "MacAddress": "02:42:ac:12:00:03",
                   "IPv4Address": "172.18.0.3/16",
                   "IPv6Address": ""
               }
           },
           "Options": {},
           "Labels": {}
       }
   ]
   ```

   Containers `alpine1`, `alpine2`, and `alpine4` are connected to the `alpine-net` network.

   ​	容器 `alpine1`、`alpine2` 和 `alpine4` 已连接到 `alpine-net` 网络。

5. On user-defined networks like `alpine-net`, containers can not only communicate by IP address, but can also resolve a container name to an IP address. This capability is called automatic service discovery. Let's connect to `alpine1` and test this out. `alpine1` should be able to resolve `alpine2` and `alpine4` (and `alpine1`, itself) to IP addresses.在 `alpine-net` 等用户定义的网络中，容器不仅可以通过 IP 地址通信，还可以通过容器名称解析为 IP 地址。这种功能称为自动服务发现。让我们连接到 `alpine1` 并测试此功能。`alpine1` 应能够解析 `alpine2` 和 `alpine4`（以及自身）到 IP 地址。

   > **Note**
   >
   > 
   >
   > Automatic service discovery can only resolve custom container names, not default automatically generated container names,
   >
   > ​	自动服务发现仅能解析自定义的容器名称，无法解析默认自动生成的容器名称。

   

   ```console
   $ docker container attach alpine1
   
   # ping -c 2 alpine2
   
   PING alpine2 (172.18.0.3): 56 data bytes
   64 bytes from 172.18.0.3: seq=0 ttl=64 time=0.085 ms
   64 bytes from 172.18.0.3: seq=1 ttl=64 time=0.090 ms
   
   --- alpine2 ping statistics ---
   2 packets transmitted, 2 packets received, 0% packet loss
   round-trip min/avg/max = 0.085/0.087/0.090 ms
   
   # ping -c 2 alpine4
   
   PING alpine4 (172.18.0.4): 56 data bytes
   64 bytes from 172.18.0.4: seq=0 ttl=64 time=0.076 ms
   64 bytes from 172.18.0.4: seq=1 ttl=64 time=0.091 ms
   
   --- alpine4 ping statistics ---
   2 packets transmitted, 2 packets received, 0% packet loss
   round-trip min/avg/max = 0.076/0.083/0.091 ms
   
   # ping -c 2 alpine1
   
   PING alpine1 (172.18.0.2): 56 data bytes
   64 bytes from 172.18.0.2: seq=0 ttl=64 time=0.026 ms
   64 bytes from 172.18.0.2: seq=1 ttl=64 time=0.054 ms
   
   --- alpine1 ping statistics ---
   2 packets transmitted, 2 packets received, 0% packet loss
   round-trip min/avg/max = 0.026/0.040/0.054 ms
   ```

6. From `alpine1`, you should not be able to connect to `alpine3` at all, since it is not on the `alpine-net` network.

   从 `alpine1` 不能连接到 `alpine3`，因为它不在 `alpine-net` 网络中。

   ```console
   # ping -c 2 alpine3
   
   ping: bad address 'alpine3'
   ```

   Not only that, but you can't connect to `alpine3` from `alpine1` by its IP address either. Look back at the `docker network inspect` output for the `bridge` network and find `alpine3`'s IP address: `172.17.0.2` Try to ping it.

   此外，无法通过 `alpine1` 的 IP 地址连接 `alpine3`。请查看 `docker network inspect` 输出中的 `bridge` 网络，找到 `alpine3` 的 IP 地址 `172.17.0.2`，尝试 ping 它。

   ```console
   # ping -c 2 172.17.0.2
   
   PING 172.17.0.2 (172.17.0.2): 56 data bytes
   
   --- 172.17.0.2 ping statistics ---
   2 packets transmitted, 0 packets received, 100% packet loss
   ```

   Detach from `alpine1` using detach sequence, `CTRL` + `p` `CTRL` + `q` (hold down `CTRL` and type `p` followed by `q`).

   ​	使用分离组合键从 `alpine1` 中分离，组合键为 `CTRL` + `p` 和 `CTRL` + `q`（按住 `CTRL`，然后依次按 `p` 和 `q`）。

7. Remember that `alpine4` is connected to both the default `bridge` network and `alpine-net`. It should be able to reach all of the other containers. However, you will need to address `alpine3` by its IP address. Attach to it and run the tests.

   记住 `alpine4` 同时连接了默认的 `bridge` 网络和 `alpine-net`。它应该可以连接到所有其他容器，但需要使用 `alpine3` 的 IP 地址进行连接。连接到 `alpine4` 并进行测试。

   ```console
   $ docker container attach alpine4
   
   # ping -c 2 alpine1
   
   PING alpine1 (172.18.0.2): 56 data bytes
   64 bytes from 172.18.0.2: seq=0 ttl=64 time=0.074 ms
   64 bytes from 172.18.0.2: seq=1 ttl=64 time=0.082 ms
   
   --- alpine1 ping statistics ---
   2 packets transmitted, 2 packets received, 0% packet loss
   round-trip min/avg/max = 0.074/0.078/0.082 ms
   
   # ping -c 2 alpine2
   
   PING alpine2 (172.18.0.3): 56 data bytes
   64 bytes from 172.18.0.3: seq=0 ttl=64 time=0.075 ms
   64 bytes from 172.18.0.3: seq=1 ttl=64 time=0.080 ms
   
   --- alpine2 ping statistics ---
   2 packets transmitted, 2 packets received, 0% packet loss
   round-trip min/avg/max = 0.075/0.077/0.080 ms
   
   # ping -c 2 alpine3
   ping: bad address 'alpine3'
   
   # ping -c 2 172.17.0.2
   
   PING 172.17.0.2 (172.17.0.2): 56 data bytes
   64 bytes from 172.17.0.2: seq=0 ttl=64 time=0.089 ms
   64 bytes from 172.17.0.2: seq=1 ttl=64 time=0.075 ms
   
   --- 172.17.0.2 ping statistics ---
   2 packets transmitted, 2 packets received, 0% packet loss
   round-trip min/avg/max = 0.075/0.082/0.089 ms
   
   # ping -c 2 alpine4
   
   PING alpine4 (172.18.0.4): 56 data bytes
   64 bytes from 172.18.0.4: seq=0 ttl=64 time=0.033 ms
   64 bytes from 172.18.0.4: seq=1 ttl=64 time=0.064 ms
   
   --- alpine4 ping statistics ---
   2 packets transmitted, 2 packets received, 0% packet loss
   round-trip min/avg/max = 0.033/0.048/0.064 ms
   ```

8. As a final test, make sure your containers can all connect to the internet by pinging `google.com`. You are already attached to `alpine4` so start by trying from there. Next, detach from `alpine4` and connect to `alpine3` (which is only attached to the `bridge` network) and try again. Finally, connect to `alpine1` (which is only connected to the `alpine-net` network) and try again.

   最后进行测试，确保所有容器都可以通过 ping `google.com` 连接到互联网。你已经连接到 `alpine4`，因此先从这里开始测试。然后从 `alpine4` 分离，连接到仅连接到 `bridge` 网络的 `alpine3` 并再次测试。最后，连接到仅连接到 `alpine-net` 网络的 `alpine1` 再次测试。

   ```console
   # ping -c 2 google.com
   
   PING google.com (172.217.3.174): 56 data bytes
   64 bytes from 172.217.3.174: seq=0 ttl=41 time=9.778 ms
   64 bytes from 172.217.3.174: seq=1 ttl=41 time=9.634 ms
   
   --- google.com ping statistics ---
   2 packets transmitted, 2 packets received, 0% packet loss
   round-trip min/avg/max = 9.634/9.706/9.778 ms
   
   CTRL+p CTRL+q
   
   $ docker container attach alpine3
   
   # ping -c 2 google.com
   
   PING google.com (172.217.3.174): 56 data bytes
   64 bytes from 172.217.3.174: seq=0 ttl=41 time=9.706 ms
   64 bytes from 172.217.3.174: seq=1 ttl=41 time=9.851 ms
   
   --- google.com ping statistics ---
   2 packets transmitted, 2 packets received, 0% packet loss
   round-trip min/avg/max = 9.706/9.778/9.851 ms
   
   CTRL+p CTRL+q
   
   $ docker container attach alpine1
   
   # ping -c 2 google.com
   
   PING google.com (172.217.3.174): 56 data bytes
   64 bytes from 172.217.3.174: seq=0 ttl=41 time=9.606 ms
   64 bytes from 172.217.3.174: seq=1 ttl=41 time=9.603 ms
   
   --- google.com ping statistics ---
   2 packets transmitted, 2 packets received, 0% packet loss
   round-trip min/avg/max = 9.603/9.604/9.606 ms
   
   CTRL+p CTRL+q
   ```

9. Stop and remove all containers and the `alpine-net` network.

   停止并删除所有容器以及 `alpine-net` 网络。

   ```console
   $ docker container stop alpine1 alpine2 alpine3 alpine4
   
   $ docker container rm alpine1 alpine2 alpine3 alpine4
   
   $ docker network rm alpine-net
   ```

## 其他网络教程 Other networking tutorials

- [Host networking tutorial](https://docs.docker.com/engine/network/tutorials/host/)
- [Overlay networking tutorial](https://docs.docker.com/engine/network/tutorials/overlay/)
- [Macvlan networking tutorial](https://docs.docker.com/engine/network/tutorials/macvlan/)
