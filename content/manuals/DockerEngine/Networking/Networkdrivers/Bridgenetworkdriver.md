+++
title = "Bridge 网络驱动"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/network/drivers/bridge/](https://docs.docker.com/engine/network/drivers/bridge/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Bridge network driver - Bridge 网络驱动

In terms of networking, a bridge network is a Link Layer device which forwards traffic between network segments. A bridge can be a hardware device or a software device running within a host machine's kernel.

​	在网络方面，桥接网络是一种链路层设备，它在网络段之间转发流量。桥接设备可以是硬件设备，也可以是运行在主机内核中的软件设备。

In terms of Docker, a bridge network uses a software bridge which lets containers connected to the same bridge network communicate, while providing isolation from containers that aren't connected to that bridge network. The Docker bridge driver automatically installs rules in the host machine so that containers on different bridge networks can't communicate directly with each other.

​	在 Docker 中，桥接网络使用软件桥接，让连接到同一桥接网络的容器相互通信，同时与未连接到该桥接网络的容器隔离开。Docker 的桥接驱动会自动在主机上安装规则，以确保不同桥接网络上的容器不能直接通信。

Bridge networks apply to containers running on the same Docker daemon host. For communication among containers running on different Docker daemon hosts, you can either manage routing at the OS level, or you can use an [overlay network]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Overlaynetworkdriver" >}}).

​	桥接网络适用于运行在同一 Docker 守护进程主机上的容器。对于运行在不同 Docker 守护进程主机上的容器之间的通信，可以在操作系统级别管理路由，或者使用 [overlay 网络]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Overlaynetworkdriver" >}})。

When you start Docker, a [default bridge network](https://docs.docker.com/engine/network/drivers/bridge/#use-the-default-bridge-network) (also called `bridge`) is created automatically, and newly-started containers connect to it unless otherwise specified. You can also create user-defined custom bridge networks. **User-defined bridge networks are superior to the default `bridge` network.**

​	启动 Docker 时，会自动创建一个[默认桥接网络](https://docs.docker.com/engine/network/drivers/bridge/#use-the-default-bridge-network)（称为 `bridge`），除非另有指定，否则新启动的容器会连接到它。您也可以创建用户定义的自定义桥接网络。**用户定义的桥接网络优于默认的 `bridge` 网络。**

## 用户定义的桥接和默认桥接的区别 Differences between user-defined bridges and the default bridge

- **User-defined bridges provide automatic DNS resolution between containers**. **用户定义的桥接提供容器之间的自动 DNS 解析**。

  Containers on the default bridge network can only access each other by IP addresses, unless you use the [`--link` option]({{< ref "/manuals/DockerEngine/Networking/Legacycontainerlinks" >}}), which is considered legacy. On a user-defined bridge network, containers can resolve each other by name or alias.

  ​	在默认桥接网络中，容器只能通过 IP 地址相互访问，除非使用被认为已过时的[`--link` 选项]({{< ref "/manuals/DockerEngine/Networking/Legacycontainerlinks" >}})。在用户定义的桥接网络中，容器可以通过名称或别名相互解析。

  Imagine an application with a web front-end and a database back-end. If you call your containers `web` and `db`, the web container can connect to the db container at `db`, no matter which Docker host the application stack is running on.

  ​	例如，一个应用有一个 web 前端和一个数据库后端。如果将容器命名为 `web` 和 `db`，那么 web 容器可以通过 `db` 名称连接到 db 容器，而不论应用栈运行在哪个 Docker 主机上。

  If you run the same application stack on the default bridge network, you need to manually create links between the containers (using the legacy `--link` flag). These links need to be created in both directions, so you can see this gets complex with more than two containers which need to communicate. Alternatively, you can manipulate the `/etc/hosts` files within the containers, but this creates problems that are difficult to debug.

  ​	如果将同一应用栈运行在默认桥接网络中，需要手动在容器之间创建链接（使用已过时的 `--link` 标志）。这些链接需要双向创建，这在两个以上需要通信的容器中显得复杂。或者，可以在容器内操作 `/etc/hosts` 文件，但这会导致难以调试的问题。

- **User-defined bridges provide better isolation**. **用户定义的桥接提供更好的隔离性**。

  All containers without a `--network` specified, are attached to the default bridge network. This can be a risk, as unrelated stacks/services/containers are then able to communicate.

  ​	所有未指定 `--network` 的容器都会连接到默认桥接网络。这可能带来风险，因为不相关的栈/服务/容器可能能够相互通信。

  Using a user-defined network provides a scoped network in which only containers attached to that network are able to communicate.

  ​	使用用户定义的网络提供了一个作用域网络，只有连接到该网络的容器能够相互通信。

- **Containers can be attached and detached from user-defined networks on the fly**. **容器可以在生命周期内动态连接和断开用户定义的网络**。

  During a container's lifetime, you can connect or disconnect it from user-defined networks on the fly. To remove a container from the default bridge network, you need to stop the container and recreate it with different network options.

  ​	在容器的生命周期中，可以动态地将其连接或断开用户定义的网络。要将容器从默认桥接网络中移除，需要停止容器并使用不同的网络选项重新创建它。

- **Each user-defined network creates a configurable bridge**. **每个用户定义的网络创建一个可配置的桥接**。

  If your containers use the default bridge network, you can configure it, but all the containers use the same settings, such as MTU and `iptables` rules. In addition, configuring the default bridge network happens outside of Docker itself, and requires a restart of Docker.

  ​	如果容器使用默认桥接网络，可以对其进行配置，但所有容器使用相同的设置，例如 MTU 和 `iptables` 规则。此外，配置默认桥接网络需要在 Docker 之外进行，并需要重启 Docker。

  User-defined bridge networks are created and configured using `docker network create`. If different groups of applications have different network requirements, you can configure each user-defined bridge separately, as you create it.

  ​	使用 `docker network create` 可以创建和配置用户定义的桥接网络。如果不同的应用组有不同的网络需求，可以在创建时分别配置每个用户定义的桥接。

- **Linked containers on the default bridge network share environment variables**. **默认桥接网络上链接的容器共享环境变量**。

  Originally, the only way to share environment variables between two containers was to link them using the [`--link` flag]({{< ref "/manuals/DockerEngine/Networking/Legacycontainerlinks" >}}). This type of variable sharing isn't possible with user-defined networks. However, there are superior ways to share environment variables. A few ideas:

  ​	最初，在两个容器之间共享环境变量的唯一方法是使用 [`--link` 标志]({{< ref "/manuals/DockerEngine/Networking/Legacycontainerlinks" >}})链接它们。用户定义的网络不支持这种变量共享方式。但是，有更优的方法可以共享环境变量，例如：
  
  - Multiple containers can mount a file or directory containing the shared information, using a Docker volume.
    - 多个容器可以使用 Docker 卷挂载一个包含共享信息的文件或目录。
  - Multiple containers can be started together using `docker-compose` and the compose file can define the shared variables.
    - 多个容器可以通过 `docker-compose` 一起启动，并在 compose 文件中定义共享变量。
  - You can use swarm services instead of standalone containers, and take advantage of shared [secrets]({{< ref "/manuals/DockerEngine/Swarmmode/ManagesensitivedatawithDockersecrets" >}}) and [configs]({{< ref "/manuals/DockerEngine/Swarmmode/StoreconfigurationdatausingDockerConfigs" >}}).
    - 可以使用 swarm 服务代替独立容器，利用共享的[密钥]({{< ref "/manuals/DockerEngine/Swarmmode/ManagesensitivedatawithDockersecrets" >}})和[配置]({{< ref "/manuals/DockerEngine/Swarmmode/StoreconfigurationdatausingDockerConfigs" >}})。

Containers connected to the same user-defined bridge network effectively expose all ports to each other. For a port to be accessible to containers or non-Docker hosts on different networks, that port must be *published* using the `-p` or `--publish` flag.

​	连接到同一用户定义桥接网络的容器可以相互访问所有端口。要使其他网络的容器或非 Docker 主机可以访问某个端口，必须使用 `-p` 或 `--publish` 标志*发布*该端口。

## 选项 Options

The following table describes the driver-specific options that you can pass to `--option` when creating a custom network using the `bridge` driver.

​	下表描述了在使用 `bridge` 驱动创建自定义网络时，可以传递给 `--option` 的驱动特定选项。

| Option                                                       | Default                     | Description                                                  |
| ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------------ |
| `com.docker.network.bridge.name`                             |                             | Interface name to use when creating the Linux bridge. 创建 Linux 桥接时使用的接口名称。 |
| `com.docker.network.bridge.enable_ip_masquerade`             | `true`                      | Enable IP masquerading. 启用 IP 假冒。                       |
| `com.docker.network.bridge.gateway_mode_ipv4` `com.docker.network.bridge.gateway_mode_ipv6` | `nat`                       | Enable NAT and masquerading (`nat`), or only allow direct routing to the container (`routed`). 启用 NAT 和伪装（`nat`），或仅允许直接路由到容器（`routed`）。 |
| `com.docker.network.bridge.enable_icc`                       | `true`                      | Enable or Disable inter-container connectivity. 启用或禁用容器间连接。 |
| `com.docker.network.bridge.host_binding_ipv4`                | all IPv4 and IPv6 addresses | Default IP when binding container ports. 绑定容器端口时的默认 IP。 |
| `com.docker.network.driver.mtu`                              | `0` (no limit)              | Set the containers network Maximum Transmission Unit (MTU). 设置容器网络的最大传输单元 (MTU)。 |
| `com.docker.network.container_iface_prefix`                  | `eth`                       | Set a custom prefix for container interfaces. 设置容器接口的自定义前缀。 |
| `com.docker.network.bridge.inhibit_ipv4`                     | `false`                     | Prevent Docker from [assigning an IP address](https://docs.docker.com/engine/network/drivers/bridge/#skip-ip-address-configuration) to the network. 阻止 Docker [分配 IP 地址](https://docs.docker.com/engine/network/drivers/bridge/#skip-ip-address-configuration) 给网络。 |

Some of these options are also available as flags to the `dockerd` CLI, and you can use them to configure the default `docker0` bridge when starting the Docker daemon. The following table shows which options have equivalent flags in the `dockerd` CLI.

​	这些选项中的一些也可以作为 `dockerd` CLI 的标志，并可以用来在启动 Docker 守护进程时配置默认的 `docker0` 桥接。下表显示了哪些选项在 `dockerd` CLI 中具有等效标志。

| Option                                           | Flag        |
| ------------------------------------------------ | ----------- |
| `com.docker.network.bridge.name`                 | -           |
| `com.docker.network.bridge.enable_ip_masquerade` | `--ip-masq` |
| `com.docker.network.bridge.enable_icc`           | `--icc`     |
| `com.docker.network.bridge.host_binding_ipv4`    | `--ip`      |
| `com.docker.network.driver.mtu`                  | `--mtu`     |
| `com.docker.network.container_iface_prefix`      | -           |

The Docker daemon supports a `--bridge` flag, which you can use to define your own `docker0` bridge. Use this option if you want to run multiple daemon instances on the same host. For details, see [Run multiple daemons](https://docs.docker.com/reference/cli/dockerd/#run-multiple-daemons).

​	Docker 守护进程支持 `--bridge` 标志，您可以使用它来定义自己的 `docker0` 桥接。如果要在同一主机上运行多个守护进程实例，请使用此选项。详情请参见 [运行多个守护进程]({{< ref "/reference/CLIreference/dockerd#运行多个守护进程-run-multiple-daemons">}})。

### 默认主机绑定地址 Default host binding address

When no host address is given in port publishing options like `-p 80` or `-p 8080:80`, the default is to make the container's port 80 available on all host addresses, IPv4 and IPv6.

​	当发布端口的选项（如 `-p 80` 或 `-p 8080:80`）中未指定主机地址时，默认是将容器的 80 端口发布到所有主机的 IPv4 和 IPv6 地址。

The bridge network driver option `com.docker.network.bridge.host_binding_ipv4` can be used to modify the default address for published ports.

​	桥接网络驱动的 `com.docker.network.bridge.host_binding_ipv4` 选项可以用于修改发布端口的默认地址。

Despite the option's name, it is possible to specify an IPv6 address.

​	尽管名称如此，也可以指定 IPv6 地址。

When the default binding address is an address assigned to a specific interface, the container's port will only be accessible via that address.

​	将默认绑定地址设置为分配给特定接口的地址时，容器的端口将仅能通过该地址访问。

Setting the default binding address to `::` means published ports will only be available on the host's IPv6 addresses. However, setting it to `0.0.0.0` means it will be available on the host's IPv4 and IPv6 addresses.

​	将默认绑定地址设置为 `::` 表示发布的端口仅在主机的 IPv6 地址上可用；而设置为 `0.0.0.0` 则表示在主机的 IPv4 和 IPv6 地址上都可用。

To restrict a published port to IPv4 only, the address must be included in the container's publishing options. For example, `-p 0.0.0.0:8080:80`.

​	若要限制发布端口仅在 IPv4 上访问，必须在容器发布选项中包含该地址。例如 `-p 0.0.0.0:8080:80`。

## 管理用户定义的桥接 Manage a user-defined bridge

Use the `docker network create` command to create a user-defined bridge network.

​	使用 `docker network create` 命令创建用户定义的桥接网络。

```console
$ docker network create my-net
```

You can specify the subnet, the IP address range, the gateway, and other options. See the [docker network create](https://docs.docker.com/reference/cli/docker/network/create/#specify-advanced-options) reference or the output of `docker network create --help` for details.

​	可以指定子网、IP 地址范围、网关等选项。有关详细信息，请参见 [docker network create]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkcreate#指定高级选项-specify-advanced-options">}}) 参考或运行 `docker network create --help` 查看帮助。

Use the `docker network rm` command to remove a user-defined bridge network. If containers are currently connected to the network, [disconnect them](https://docs.docker.com/engine/network/drivers/bridge/#disconnect-a-container-from-a-user-defined-bridge) first.

​	使用 `docker network rm` 命令删除用户定义的桥接网络。如果网络中有容器连接，请先[断开它们的连接](#从用户定义的桥接断开容器-disconnect-a-container-from-a-user-defined-bridge)。

```console
$ docker network rm my-net
```

> **What's really happening? 实际发生了什么？**
>
> When you create or remove a user-defined bridge or connect or disconnect a container from a user-defined bridge, Docker uses tools specific to the operating system to manage the underlying network infrastructure (such as adding or removing bridge devices or configuring `iptables` rules on Linux). These details should be considered implementation details. Let Docker manage your user-defined networks for you.
>
> ​	当您创建或删除用户定义的桥接，或连接或断开容器时，Docker 使用特定于操作系统的工具来管理底层网络基础设施（如添加或删除桥接设备或配置 Linux 上的 `iptables` 规则）。这些细节应视为实现细节。让 Docker 为您管理用户定义的网络。

## 连接容器到用户定义的桥接 Connect a container to a user-defined bridge

When you create a new container, you can specify one or more `--network` flags. This example connects an Nginx container to the `my-net` network. It also publishes port 80 in the container to port 8080 on the Docker host, so external clients can access that port. Any other container connected to the `my-net` network has access to all ports on the `my-nginx` container, and vice versa.

​	创建新容器时，可以指定一个或多个 `--network` 标志。此示例将 Nginx 容器连接到 `my-net` 网络，并将容器的 80 端口发布到 Docker 主机的 8080 端口，使外部客户端能够访问该端口。任何其他连接到 `my-net` 网络的容器都可以访问 `my-nginx` 容器上的所有端口，反之亦然。

```console
$ docker create --name my-nginx \
  --network my-net \
  --publish 8080:80 \
  nginx:latest
```

To connect a **running** container to an existing user-defined bridge, use the `docker network connect` command. The following command connects an already-running `my-nginx` container to an already-existing `my-net` network:

​	要将**运行中的**容器连接到现有的用户定义桥接，请使用 `docker network connect` 命令。以下命令将一个已运行的 `my-nginx` 容器连接到一个已存在的 `my-net` 网络：

```console
$ docker network connect my-net my-nginx
```

## 从用户定义的桥接断开容器 Disconnect a container from a user-defined bridge

To disconnect a running container from a user-defined bridge, use the `docker network disconnect` command. The following command disconnects the `my-nginx` container from the `my-net` network.

​	要将运行中的容器从用户定义的桥接中断开，使用 `docker network disconnect` 命令。以下命令将 `my-nginx` 容器从 `my-net` 网络断开。

```console
$ docker network disconnect my-net my-nginx
```

## 在用户定义的桥接网络中使用 IPv6 - Use IPv6 in a user-defined bridge network

When you create your network, you can specify the `--ipv6` flag to enable IPv6.

​	创建网络时，可以指定 `--ipv6` 标志以启用 IPv6。

```console
$ docker network create --ipv6 --subnet 2001:db8:1234::/64 my-net
```

## 使用默认桥接网络 Use the default bridge network

The default `bridge` network is considered a legacy detail of Docker and is not recommended for production use. Configuring it is a manual operation, and it has [technical shortcomings](https://docs.docker.com/engine/network/drivers/bridge/#differences-between-user-defined-bridges-and-the-default-bridge).

​	默认的 `bridge` 网络被认为是 Docker 的一个传统细节，不建议用于生产环境。配置它是一个手动操作，并且它有[技术缺陷](#用户定义的桥接和默认桥接的区别-differences-between-user-defined-bridges-and-the-default-bridge)。

### 连接容器到默认桥接网络 Connect a container to the default bridge network

If you do not specify a network using the `--network` flag, and you do specify a network driver, your container is connected to the default `bridge` network by default. Containers connected to the default `bridge` network can communicate, but only by IP address, unless they're linked using the [legacy `--link` flag]({{< ref "/manuals/DockerEngine/Networking/Legacycontainerlinks" >}}).

​	如果没有使用 `--network` 标志指定网络，并且没有指定网络驱动，则容器默认连接到默认的 `bridge` 网络。连接到默认 `bridge` 网络的容器可以通信，但只能通过 IP 地址，除非它们使用[传统 `--link` 标志]({{< ref "/manuals/DockerEngine/Networking/Legacycontainerlinks" >}})链接。

### 配置默认桥接网络 Configure the default bridge network

To configure the default `bridge` network, you specify options in `daemon.json`. Here is an example `daemon.json` with several options specified. Only specify the settings you need to customize.

​	要配置默认 `bridge` 网络，请在 `daemon.json` 中指定选项。以下是一个包含几个选项的 `daemon.json` 示例。只指定需要自定义的设置。

```json
{
  "bip": "192.168.1.1/24",
  "fixed-cidr": "192.168.1.0/25",
  "mtu": 1500,
  "default-gateway": "192.168.1.254",
  "dns": ["10.20.1.2","10.20.1.3"]
}
```

Restart Docker for the changes to take effect.

​	重启 Docker 以使更改生效。

### 在默认桥接网络中使用 IPv6 - Use IPv6 with the default bridge network

IPv6 can be enabled for the default bridge using the following options in `daemon.json`, or their command line equivalents.

​	可以通过在 `daemon.json` 中使用以下选项启用默认桥接的 IPv6，或使用相应的命令行选项。

These three options only affect the default bridge, they are not used by user-defined networks. The addresses in below are examples from the IPv6 documentation range.

​	这些选项仅影响默认桥接，不用于用户定义的网络。以下地址为 IPv6 文档范围中的示例。

- Option `ipv6` is required
  - 必须启用 `ipv6` 选项

- Option `fixed-cidr-v6` is required, it specifies the network prefix to be used. 必须启用`fixed-cidr-v6` 选项，指定要使用的网络前缀。

  - The prefix should normally be `/64` or shorter.
    - 前缀通常应为 `/64` 或更短。
  - For experimentation on a local network, it is better to use a Unique Local prefix (matching `fd00::/8`) than a Link Local prefix (matching `fe80::/10`).
    - 对于本地网络实验，使用唯一本地前缀（匹配 `fd00::/8`）比使用链路本地前缀（匹配 `fe80::/10`）更好。
  
- Option `default-gateway-v6` is optional. If unspecified, the default is the first address in the `fixed-cidr-v6` subnet.
  - `default-gateway-v6` 选项是可选的。如果未指定，默认值是 `fixed-cidr-v6` 子网中的第一个地址。




```json
{
  "ipv6": true,
  "fixed-cidr-v6": "2001:db8::/64",
  "default-gateway-v6": "2001:db8:abcd::89"
}
```

## 桥接网络的连接限制 Connection limit for bridge networks

Due to limitations set by the Linux kernel, bridge networks become unstable and inter-container communications may break when 1000 containers or more connect to a single network.

​	由于 Linux 内核的限制，当 1000 个或更多容器连接到同一网络时，桥接网络会变得不稳定，容器间的通信可能中断。

For more information about this limitation, see [moby/moby#44973](https://github.com/moby/moby/issues/44973#issuecomment-1543747718).

​	有关此限制的更多信息，请参见 [moby/moby#44973](https://github.com/moby/moby/issues/44973#issuecomment-1543747718)。

## 跳过 IP 地址配置 Skip IP address configuration

The `com.docker.network.bridge.inhibit_ipv4` option lets you create a network that uses an existing bridge and have Docker skip configuring the IPv4 address on the bridge. This is useful if you want to configure the IP address for the bridge manually. For instance if you add a physical interface to your bridge, and need to move its IP address to the bridge interface.

​	`com.docker.network.bridge.inhibit_ipv4` 选项允许创建使用现有桥接的网络，并让 Docker 跳过在桥接上配置 IPv4 地址。这对于手动配置桥接的 IP 地址非常有用。例如，如果将物理接口添加到桥接，并需要将其 IP 地址移到桥接接口上。

To use this option, you should first configure the Docker daemon to use a self-managed bridge, using the `bridge` option in the `daemon.json` or the `dockerd --bridge` flag.

​	要使用此选项，应首先配置 Docker 守护进程使用自管理的桥接，在 `daemon.json` 中使用 `bridge` 选项或使用 `dockerd --bridge` 标志。

With this configuration, north-south traffic won't work unless you've manually configured the IP address for the bridge.

​	使用此配置，除非手动配置桥接的 IP 地址，否则南北向流量将无法工作。

## 接下来 Next steps

- Go through the [standalone networking tutorial]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingwithstandalonecontainers" >}})
  - 通过[独立网络教程]({{< ref "/manuals/DockerEngine/Networking/Tutorials/Networkingwithstandalonecontainers" >}})
- Learn about [networking from the container's point of view]({{< ref "/manuals/DockerEngine/Networking" >}})
  - 了解[从容器的角度看网络]({{< ref "/manuals/DockerEngine/Networking" >}})
- Learn about [overlay networks]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Overlaynetworkdriver" >}})
  - 了解[覆盖网络]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Overlaynetworkdriver" >}})
- Learn about [Macvlan networks]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Macvlannetworkdriver" >}})
  - 了解[Macvlan 网络]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Macvlannetworkdriver" >}})
