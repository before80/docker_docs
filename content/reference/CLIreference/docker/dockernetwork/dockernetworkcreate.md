+++
title = "docker network create"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/network/create/](https://docs.docker.com/reference/cli/docker/network/create/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker network create

| Description | Create a network                          |
| :---------- | ----------------------------------------- |
| Usage       | `docker network create [OPTIONS] NETWORK` |

## Description

Creates a new network. The `DRIVER` accepts `bridge` or `overlay` which are the built-in network drivers. If you have installed a third party or your own custom network driver you can specify that `DRIVER` here also. If you don't specify the `--driver` option, the command automatically creates a `bridge` network for you. When you install Docker Engine it creates a `bridge` network automatically. This network corresponds to the `docker0` bridge that Docker Engine has traditionally relied on. When you launch a new container with `docker run` it automatically connects to this bridge network. You cannot remove this default bridge network, but you can create new ones using the `network create` command.

​	创建一个新网络。`DRIVER` 接受内置的网络驱动程序 `bridge` 或 `overlay`。如果您安装了第三方或自定义网络驱动程序，也可以在此指定该 `DRIVER`。如果未指定 `--driver` 选项，命令会自动为您创建一个 `bridge` 网络。安装 Docker 引擎时会自动创建一个 `bridge` 网络，它对应于 Docker 引擎传统依赖的 `docker0` 桥接网络。使用 `docker run` 启动新容器时，它会自动连接到此桥接网络。不能删除此默认桥接网络，但可以使用 `network create` 命令创建新的桥接网络。



```console
$ docker network create -d bridge my-bridge-network
```

Bridge networks are isolated networks on a single Docker Engine installation. If you want to create a network that spans multiple Docker hosts each running Docker Engine, you must enable Swarm mode, and create an `overlay` network. To read more about overlay networks with Swarm mode, see ["*use overlay networks*"](https://docs.docker.com/network/overlay/).

​	桥接网络是单个 Docker 引擎安装上的隔离网络。如果要创建一个跨多个运行 Docker 引擎的 Docker 主机的网络，必须启用 Swarm 模式，并创建一个 `overlay` 网络。有关 Swarm 模式下的 overlay 网络的更多信息，请参阅[“使用 overlay 网络”](https://docs.docker.com/network/overlay/)。

Once you have enabled swarm mode, you can create a swarm-scoped overlay network:

​	启用 Swarm 模式后，可以创建一个 Swarm 范围的 overlay 网络：



```console
$ docker network create --scope=swarm --attachable -d overlay my-multihost-network
```

By default, swarm-scoped networks do not allow manually started containers to be attached. This restriction is added to prevent someone that has access to a non-manager node in the swarm cluster from running a container that is able to access the network stack of a swarm service.

​	默认情况下，Swarm 范围的网络不允许手动启动的容器连接，以防止非管理节点上的用户运行能够访问 Swarm 服务网络堆栈的容器。

The `--attachable` option used in the example above disables this restriction, and allows for both swarm services and manually started containers to attach to the overlay network.

​	使用上例中的 `--attachable` 选项可禁用此限制，并允许 Swarm 服务和手动启动的容器连接到 overlay 网络。

Network names must be unique. The Docker daemon attempts to identify naming conflicts but this is not guaranteed. It is the user's responsibility to avoid name conflicts.

​	网络名称必须唯一。Docker 守护程序会尝试识别命名冲突，但不能保证成功。避免命名冲突是用户的责任。

### Overlay 网络限制 Overlay network limitations

You should create overlay networks with `/24` blocks (the default), which limits you to 256 IP addresses, when you create networks using the default VIP-based endpoint-mode. This recommendation addresses [limitations with swarm mode](https://github.com/moby/moby/issues/30820). If you need more than 256 IP addresses, do not increase the IP block size. You can either use `dnsrr` endpoint mode with an external load balancer, or use multiple smaller overlay networks. See [Configure service discovery](https://docs.docker.com/engine/swarm/networking/#configure-service-discovery) for more information about different endpoint modes.

​	建议使用 `/24` 网段（默认）创建 overlay 网络，此配置限制为 256 个 IP 地址，以解决 [Swarm 模式的限制](https://github.com/moby/moby/issues/30820)。如果需要超过 256 个 IP 地址，不要增加 IP 块大小，而是使用 `dnsrr` 端点模式结合外部负载均衡器，或者使用多个较小的 overlay 网络。有关不同端点模式的详细信息，请参阅[配置服务发现](https://docs.docker.com/engine/swarm/networking/#configure-service-discovery)。

## Options

| Option                                                       | Default  | Description                                                  |
| ------------------------------------------------------------ | -------- | ------------------------------------------------------------ |
| `--attachable`                                               |          | API 1.25+ 启用手动容器连接 API 1.25+ Enable manual container attachment |
| `--aux-address`                                              |          | 网络驱动程序使用的辅助 IPv4 或 IPv6 地址 Auxiliary IPv4 or IPv6 addresses used by Network driver |
| `--config-from`                                              |          | API 1.30+ 从指定网络复制配置 API 1.30+ The network from which to copy the configuration |
| `--config-only`                                              |          | API 1.30+ 创建仅配置的网络API 1.30+ Create a configuration only network |
| `-d, --driver`                                               | `bridge` | 管理网络的驱动程序 Driver to manage the Network              |
| `--gateway`                                                  |          | 主子网的 IPv4 或 IPv6 网关 IPv4 or IPv6 Gateway for the master subnet |
| [`--ingress`](https://docs.docker.com/reference/cli/docker/network/create/#ingress) |          | API 1.29+ 创建 Swarm 路由网格网络 API 1.29+ Create swarm routing-mesh network |
| [`--internal`](https://docs.docker.com/reference/cli/docker/network/create/#internal) |          | 限制网络的外部访问Restrict external access to the network    |
| `--ip-range`                                                 |          | 从子范围分配容器 IPAllocate container ip from a sub-range    |
| `--ipam-driver`                                              |          | IP 地址管理驱动程序IP Address Management Driver              |
| `--ipam-opt`                                                 |          | 设置 IPAM 驱动程序的特定选项Set IPAM driver specific options |
| `--ipv6`                                                     |          | 启用或禁用 IPv6 网络Enable or disable IPv6 networking        |
| `--label`                                                    |          | 为网络设置元数据Set metadata on a network                    |
| `-o, --opt`                                                  |          | 设置驱动程序的特定选项Set driver specific options            |
| `--scope`                                                    |          | API 1.30+ 控制网络的范围 API 1.30+ Control the network's scope |
| `--subnet`                                                   |          | 代表网络段的 CIDR 格式子网Subnet in CIDR format that represents a network segment |

## Examples

### 连接容器 Connect containers

When you start a container, use the `--network` flag to connect it to a network. This example adds the `busybox` container to the `mynet` network:

​	启动容器时，使用 `--network` 标志将其连接到网络。以下示例将 `busybox` 容器添加到 `mynet` 网络：



```console
$ docker run -itd --network=mynet busybox
```

If you want to add a container to a network after the container is already running, use the `docker network connect` subcommand.

​	如果想在容器运行后将其添加到网络，请使用 `docker network connect` 子命令。

You can connect multiple containers to the same network. Once connected, the containers can communicate using only another container's IP address or name. For `overlay` networks or custom plugins that support multi-host connectivity, containers connected to the same multi-host network but launched from different daemons can also communicate in this way.

​	可以将多个容器连接到同一网络。连接后，容器可以仅使用另一个容器的 IP 地址或名称进行通信。对于 `overlay` 网络或支持多主机连接的自定义插件，从不同守护程序启动但连接到相同多主机网络的容器也可以以此方式通信。

You can disconnect a container from a network using the `docker network disconnect` command.

​	可以使用 `docker network disconnect` 命令将容器从网络中断开。

### 指定高级选项 Specify advanced options

When you create a network, Docker Engine creates a non-overlapping subnetwork for the network by default. This subnetwork is not a subdivision of an existing network. It is purely for ip-addressing purposes. You can override this default and specify subnetwork values directly using the `--subnet` option. On a `bridge` network you can only create a single subnet:

​	创建网络时，Docker 引擎会默认创建一个非重叠子网，此子网仅用于 IP 地址分配。可以使用 `--subnet` 选项直接指定子网值。在 `bridge` 网络中只能创建一个子网：



```console
$ docker network create --driver=bridge --subnet=192.168.0.0/16 br0
```

Additionally, you also specify the `--gateway` `--ip-range` and `--aux-address` options.

​	此外，还可以指定 `--gateway`、`--ip-range` 和 `--aux-address` 选项。



```console
$ docker network create \
  --driver=bridge \
  --subnet=172.28.0.0/16 \
  --ip-range=172.28.5.0/24 \
  --gateway=172.28.5.254 \
  br0
```

If you omit the `--gateway` flag, Docker Engine selects one for you from inside a preferred pool. For `overlay` networks and for network driver plugins that support it you can create multiple subnetworks. This example uses two `/25` subnet mask to adhere to the current guidance of not having more than 256 IPs in a single overlay network. Each of the subnetworks has 126 usable addresses.

​	如果省略 `--gateway` 标志，Docker 引擎会从首选池中为您选择一个。对于 `overlay` 网络以及支持该功能的网络驱动程序插件，您可以创建多个子网。以下示例使用 `/25` 掩码创建了两个子网，以遵循当前每个 overlay 网络不超过 256 个 IP 的指南。每个子网可用 126 个地址。



```console
$ docker network create -d overlay \
  --subnet=192.168.10.0/25 \
  --subnet=192.168.20.0/25 \
  --gateway=192.168.10.100 \
  --gateway=192.168.20.100 \
  --aux-address="my-router=192.168.10.5" --aux-address="my-switch=192.168.10.6" \
  --aux-address="my-printer=192.168.20.5" --aux-address="my-nas=192.168.20.6" \
  my-multihost-network
```

Be sure that your subnetworks do not overlap. If they do, the network create fails and Docker Engine returns an error.

​	请确保您的子网不重叠，否则网络创建失败，Docker 引擎会返回错误。

### Bridge 驱动选项 Bridge driver options

When creating a custom network, the default network driver (i.e. `bridge`) has additional options that can be passed. The following are those options and the equivalent Docker daemon flags used for docker0 bridge:

​	创建自定义网络时，默认网络驱动程序（即 `bridge`）具有可传递的额外选项。以下为这些选项及其对应的 docker0 桥接网络的 Docker 守护程序标志：

| Option                                           | Equivalent  | Description                                                  |
| ------------------------------------------------ | ----------- | ------------------------------------------------------------ |
| `com.docker.network.bridge.name`                 | -           | 创建 Linux 桥接时使用的桥接名称 Bridge name to be used when creating the Linux bridge |
| `com.docker.network.bridge.enable_ip_masquerade` | `--ip-masq` | 启用 IP 伪装 Enable IP masquerading                          |
| `com.docker.network.bridge.enable_icc`           | `--icc`     | 启用或禁用容器间的互连 Enable or Disable Inter Container Connectivity |
| `com.docker.network.bridge.host_binding_ipv4`    | `--ip`      | 绑定容器端口时使用的默认 IPDefault IP when binding container ports |
| `com.docker.network.driver.mtu`                  | `--mtu`     | 设置容器网络 MTU Set the containers network MTU              |
| `com.docker.network.container_iface_prefix`      | -           | 设置容器接口的自定义前缀 Set a custom prefix for container interfaces |

The following arguments can be passed to `docker network create` for any network driver, again with their approximate equivalents to Docker daemon flags used for the docker0 bridge:

​	以下参数可以传递给 `docker network create` 来创建任何网络驱动器，与 docker0 桥接网络所使用的 Docker 守护程序标志大致对应：

| Argument     | Equivalent     | Description                                                  |
| ------------ | -------------- | ------------------------------------------------------------ |
| `--gateway`  | -              | 主子网的 IPv4 或 IPv6 网关 IPv4 or IPv6 Gateway for the master subnet |
| `--ip-range` | `--fixed-cidr` | 从范围内分配 IP Allocate IPs from a range                    |
| `--internal` | -              | 限制对网络的外部访问 Restrict external access to the network |
| `--ipv6`     | `--ipv6`       | 启用或禁用 IPv6 网络 Enable or disable IPv6 networking       |
| `--subnet`   | `--bip`        | 网络的子网 Subnet for network                                |

For example, let's use `-o` or `--opt` options to specify an IP address binding when publishing ports:

​	例如，让我们使用 `-o` 或 `--opt` 选项在发布端口时指定一个 IP 地址绑定：



```console
$ docker network create \
    -o "com.docker.network.bridge.host_binding_ipv4"="172.19.0.1" \
    simple-network
```

### 网络内部模式（`--internal`）Network internal mode (`--internal`)

Containers on an internal network may communicate between each other, but not with any other network, as no default route is configured and firewall rules are set up to drop all traffic to or from other networks. Communication with the gateway IP address (and thus appropriately configured host services) is possible, and the host may communicate with any container IP directly.

​	在内部网络中的容器可以彼此通信，但不能与其他网络通信，因为未配置默认路由，且设置了防火墙规则以阻止所有到其他网络的流量。可以与网关 IP 地址（以及适当配置的主机服务）通信，主机也可以直接与任何容器 IP 进行通信。

By default, when you connect a container to an `overlay` network, Docker also connects a bridge network to it to provide external connectivity. If you want to create an externally isolated `overlay` network, you can specify the `--internal` option.

​	默认情况下，当你将容器连接到 `overlay` 网络时，Docker 还会将其连接到桥接网络以提供外部连接。如果想创建一个外部隔离的 `overlay` 网络，可以指定 `--internal` 选项。

### 网络入口模式（`--ingress`） Network ingress mode (`--ingress`)

You can create the network which will be used to provide the routing-mesh in the swarm cluster. You do so by specifying `--ingress` when creating the network. Only one ingress network can be created at the time. The network can be removed only if no services depend on it. Any option available when creating an overlay network is also available when creating the ingress network, besides the `--attachable` option.

​	可以创建用于在 swarm 集群中提供路由网格的网络，通过在创建网络时指定 `--ingress` 实现。每次只能创建一个入口网络，且仅在没有服务依赖于此网络时可以移除。创建入口网络时，可以使用创建 `overlay` 网络时的所有选项，但不包括 `--attachable` 选项。



```console
$ docker network create -d overlay \
  --subnet=10.11.0.0/16 \
  --ingress \
  --opt com.docker.network.driver.mtu=9216 \
  --opt encrypted=true \
  my-ingress-network
```

### 在预定义的网络上运行服务 Run services on predefined networks

You can create services on the predefined Docker networks `bridge` and `host`.

​	可以在预定义的 Docker 网络 `bridge` 和 `host` 上创建服务。



```console
$ docker service create --name my-service \
  --network host \
  --replicas 2 \
  busybox top
```

### 具有本地作用域驱动程序的 Swarm 网络 Swarm networks with local scope drivers

You can create a swarm network with local scope network drivers. You do so by promoting the network scope to `swarm` during the creation of the network. You will then be able to use this network when creating services.

​	可以使用本地作用域的网络驱动程序创建一个 swarm 网络。通过在创建网络时将网络范围提升为 `swarm` 实现。然后可以在创建服务时使用此网络。



```console
$ docker network create -d bridge \
  --scope swarm \
  --attachable \
  swarm-network
```

For network drivers which provide connectivity across hosts (ex. macvlan), if node specific configurations are needed in order to plumb the network on each host, you will supply that configuration via a configuration only network. When you create the swarm scoped network, you will then specify the name of the network which contains the configuration.

​	对于提供跨主机连接的网络驱动程序（例如 macvlan），如果每个主机需要特定配置来连接网络，可通过仅配置网络来提供此配置。创建 swarm 范围的网络时，将指定包含该配置的网络名称。



```console
node1$ docker network create --config-only --subnet 192.168.100.0/24 --gateway 192.168.100.115 mv-config
node2$ docker network create --config-only --subnet 192.168.200.0/24 --gateway 192.168.200.202 mv-config
node1$ docker network create -d macvlan --scope swarm --config-from mv-config --attachable swarm-network
```

