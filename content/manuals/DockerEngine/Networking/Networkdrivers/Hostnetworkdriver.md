+++
title = "Host 网络驱动"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/network/drivers/host/](https://docs.docker.com/engine/network/drivers/host/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Host network driver - Host 网络驱动

If you use the `host` network mode for a container, that container's network stack isn't isolated from the Docker host (the container shares the host's networking namespace), and the container doesn't get its own IP-address allocated. For instance, if you run a container which binds to port 80 and you use `host` networking, the container's application is available on port 80 on the host's IP address.

​	如果为容器使用 `host` 网络模式，该容器的网络栈将不与 Docker 主机隔离（容器共享主机的网络命名空间），并且该容器不会获得单独的 IP 地址。例如，如果运行一个绑定到端口 80 的容器并使用 `host` 网络模式，则该容器的应用将在主机的 IP 地址上的端口 80 提供服务。

> **Note**
>
> 
>
> Given that the container does not have its own IP-address when using `host` mode networking, [port-mapping](https://docs.docker.com/engine/network/#published-ports) doesn't take effect, and the `-p`, `--publish`, `-P`, and `--publish-all` option are ignored, producing a warning instead:
>
> ​	由于使用 `host` 网络模式时容器没有自己的 IP 地址，[端口映射]({{< ref "/manuals/DockerEngine/Networking#端口发布-published-ports">}})将不起作用，`-p`、`--publish`、`-P` 和 `--publish-all` 选项将被忽略，并显示警告：
>
> 
>
> ```console
> WARNING: Published ports are discarded when using host network mode
> ```

Host mode networking can be useful for the following use cases:

​	主机模式网络在以下情况下可能很有用：

- To optimize performance
  - 优化性能

- In situations where a container needs to handle a large range of ports
  - 容器需要处理大量端口时


This is because it doesn't require network address translation (NAT), and no "userland-proxy" is created for each port.

​	这是因为它不需要网络地址转换 (NAT)，并且不会为每个端口创建“用户空间代理”。

The host networking driver is supported on Docker Engine (Linux only) and Docker Desktop version 4.34 and later.

​	`host`网络驱动支持 Docker Engine（仅限 Linux）和 Docker Desktop 4.34 及更高版本。

You can also use a `host` network for a swarm service, by passing `--network host` to the `docker service create` command. In this case, control traffic (traffic related to managing the swarm and the service) is still sent across an overlay network, but the individual swarm service containers send data using the Docker daemon's host network and ports. This creates some extra limitations. For instance, if a service container binds to port 80, only one service container can run on a given swarm node.

​	您还可以通过向 `docker service create` 命令传递 `--network host` 为 swarm 服务使用 `host` 网络模式。在这种情况下，控制流量（与管理 swarm 和服务相关的流量）仍通过`overlay`网络传输，但单个 swarm 服务容器将使用 Docker 守护进程的`host`网络和端口传输数据。这会产生一些额外的限制。例如，如果一个服务容器绑定到端口 80，则在给定的 swarm 节点上只能运行一个该服务容器。

## Docker Desktop

Host networking is supported on Docker Desktop version 4.34 and later. To enable this feature:

​	Docker Desktop 4.34 及更高版本支持`host`网络。启用此功能：

1. Sign in to your Docker account in Docker Desktop. 在 Docker Desktop 中登录到您的 Docker 账户。
2. Navigate to **Settings**. 进入 **Settings（设置）**。
3. Under the **Resources** tab, select **Network**. 在 **Resources（资源）** 标签下，选择 **Network（网络）**。
4. Check the **Enable host networking** option. 勾选 **Enable host networking（启用`host`网络）** 选项。
5. Select **Apply and restart**. 选择 **Apply and restart（应用并重启）**。

This feature works in both directions. This means you can access a server that is running in a container from your host and you can access servers running on your host from any container that is started with host networking enabled. TCP as well as UDP are supported as communication protocols.

​	此功能支持双向通信。这意味着您可以从主机访问容器中运行的服务，也可以从启用了`host`网络的容器访问主机上的服务。TCP 和 UDP 协议均受支持。

### Examples

The following command starts netcat in a container that listens on port `8000`:

​	以下命令在容器中启动 netcat，并监听端口 `8000`：

```console
$ docker run --rm -it --net=host nicolaka/netshoot nc -lkv 0.0.0.0 8000
```

Port `8000` will then be available on the host and you can connect to it with the following command from another terminal:

​	端口 `8000` 将在主机上可用，您可以通过另一个终端使用以下命令连接到它：



```console
$ nc localhost 8000
```

What you type in here will then appear on the terminal where the container is running.

​	您在此终端输入的内容将出现在容器运行的终端上。

To access a service running on the host from the container, you can start a container with host networking enabled with this command:

​	要从容器访问主机上运行的服务，可以使用以下命令启动启用`host`网络的容器：



```console
$ docker run --rm -it --net=host nicolaka/netshoot
```

> 个人注释
>
> ​	原本以为这里应该使用的`--network`来代替`--net`，一运行发现还是可以正常启动容器！

If you then want to access a service on your host from the container (in this example a web server running on port `80`), you can do it like this:

​	如果您想从容器访问主机上的某个服务（例如在端口 `80` 上运行的 Web 服务器），可以像这样操作：



```console
$ nc localhost 80
```

### 限制 Limitations

- Processes inside the container cannot bind to the IP addresses of the host because the container has no direct access to the interfaces of the host.
  - 容器内的进程不能绑定到主机的 IP 地址，因为容器无法直接访问主机的接口。

- The host network feature of Docker Desktop works on layer 4. This means that unlike with Docker on Linux, network protocols that operate below TCP or UDP are not supported.
  - Docker Desktop 的`host`网络功能在第 4 层运行，这意味着与 Linux 上的 Docker 不同，不支持操作在 TCP 或 UDP 之下的网络协议。

- This feature doesn't work with Enhanced Container Isolation enabled, since isolating your containers from the host and allowing them access to the host network contradict each other.
  - 此功能不适用于启用增强容器隔离的情况，因为隔离容器和允许其访问`host`网络是相互矛盾的。

- Only Linux containers are supported. Host networking does not work with Windows containers.
  - 仅支持 Linux 容器。`host`网络不支持 Windows 容器。


## 接下来 Next steps

- Go through the [host networking tutorial]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingusingthehostnetwork" >}})
  - 通过[`host`网络教程]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingusingthehostnetwork" >}})
- Learn about [networking from the container's point of view]({{< ref "/manuals/DockerEngine/Networking" >}})
  - 了解[从容器的角度看网络]({{< ref "/manuals/DockerEngine/Networking" >}})
- Learn about [bridge networks]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Bridgenetworkdriver" >}})
  - 了解[桥接网络]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Bridgenetworkdriver" >}})
- Learn about [overlay networks]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Overlaynetworkdriver" >}})
  - 了解[覆盖网络]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Overlaynetworkdriver" >}})
- Learn about [Macvlan networks]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Macvlannetworkdriver" >}})
  - 了解[Macvlan 网络]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Macvlannetworkdriver" >}})
