+++
title = "docker swarm init"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/swarm/init/](https://docs.docker.com/reference/cli/docker/swarm/init/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker swarm init

| Description | Initialize a swarm            |
| :---------- | ----------------------------- |
| Usage       | `docker swarm init [OPTIONS]` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 协调器。

## Description

Initialize a swarm. The Docker Engine targeted by this command becomes a manager in the newly created single-node swarm.

​	初始化一个 Swarm。此命令所针对的 Docker 引擎将成为新创建的单节点 Swarm 中的管理节点。

## Options

| Option                                                       | Default        | Description                                                  |
| ------------------------------------------------------------ | -------------- | ------------------------------------------------------------ |
| [`--advertise-addr`](https://docs.docker.com/reference/cli/docker/swarm/init/#advertise-addr) |                | 广播地址 Advertised address (format: `<ip|interface>[:port]`) |
| [`--autolock`](https://docs.docker.com/reference/cli/docker/swarm/init/#autolock) |                | 启用管理器自动锁定（需要解锁密钥才能启动已停止的管理器） Enable manager autolocking (requiring an unlock key to start a stopped manager) |
| [`--availability`](https://docs.docker.com/reference/cli/docker/swarm/init/#availability) | `active`       | 节点的可用性 (`active`, `pause`, `drain`) Availability of the node (`active`, `pause`, `drain`) |
| `--cert-expiry`                                              | `2160h0m0s`    | 节点证书的有效期 Validity period for node certificates (ns\|us\|ms\|s\|m\|h) |
| [`--data-path-addr`](https://docs.docker.com/reference/cli/docker/swarm/init/#data-path-addr) |                | API 1.31+ 数据路径流量的地址或接口  API 1.31+ Address or interface to use for data path traffic (format: `<ip|interface>`) |
| [`--data-path-port`](https://docs.docker.com/reference/cli/docker/swarm/init/#data-path-port) |                | API 1.40+ 数据路径流量使用的端口号（1024 - 49151）。如果未设置或设置为 0，则使用默认端口（4789） API 1.40+ Port number to use for data path traffic (1024 - 49151). If no value is set or is set to 0, the default port (4789) is used. |
| [`--default-addr-pool`](https://docs.docker.com/reference/cli/docker/swarm/init/#default-addr-pool) |                | API 1.39+ 默认地址池，CIDR 格式 API 1.39+ default address pool in CIDR format |
| `--default-addr-pool-mask-length`                            | `24`           | API 1.39+ 默认地址池子网掩码长度 API 1.39+ default address pool subnet mask length |
| `--dispatcher-heartbeat`                                     | `5s`           | 调度器心跳周期 Dispatcher heartbeat period (ns\|us\|ms\|s\|m\|h) |
| [`--external-ca`](https://docs.docker.com/reference/cli/docker/swarm/init/#external-ca) |                | 一个或多个证书签发端点的规范 Specifications of one or more certificate signing endpoints |
| [`--force-new-cluster`](https://docs.docker.com/reference/cli/docker/swarm/init/#force-new-cluster) |                | 从当前状态强制创建一个新集群  Force create a new cluster from current state |
| [`--listen-addr`](https://docs.docker.com/reference/cli/docker/swarm/init/#listen-addr) | `0.0.0.0:2377` | 监听地址 Listen address (format: `<ip|interface>[:port]`)    |
| [`--max-snapshots`](https://docs.docker.com/reference/cli/docker/swarm/init/#max-snapshots) |                | API 1.25+ 保留的额外 Raft 快照数量 API 1.25+ Number of additional Raft snapshots to retain |
| [`--snapshot-interval`](https://docs.docker.com/reference/cli/docker/swarm/init/#snapshot-interval) | `10000`        | API 1.25+ Raft 快照之间的日志条目数量 API 1.25+ Number of log entries between Raft snapshots |
| `--task-history-limit`                                       | `5`            | 任务历史记录保留限制 Task history retention limit            |

## Examples



```console
$ docker swarm init --advertise-addr 192.168.99.121

Swarm initialized: current node (bvz81updecsj6wjz393c09vti) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-3pu6hszjas19xyp7ghgosyx9k8atbfcr8p2is99znpy26u2lkl-1awxwuwd3z9j1z3puu7rcgdbx 172.17.0.2:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

The `docker swarm init` command generates two random tokens: a worker token and a manager token. When you join a new node to the swarm, the node joins as a worker or manager node based upon the token you pass to [swarm join]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmjoin" >}}).

​	`docker swarm init` 命令生成两个随机令牌：一个用于 worker，另一个用于 manager。当新节点加入 Swarm 时，节点将根据传递给 [swarm join]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmjoin" >}}) 的令牌，加入为 worker 或 manager 节点。

After you create the swarm, you can display or rotate the token using [swarm join-token]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmjoin-token" >}}).

​	创建 Swarm 后，可以使用 [swarm join-token]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmjoin-token" >}}) 显示或旋转令牌。

### 保护管理器密钥和数据（`--autolock`） Protect manager keys and data (`--autolock`)

The `--autolock` flag enables automatic locking of managers with an encryption key. The private keys and data stored by all managers are protected by the encryption key printed in the output, and is inaccessible without it. Make sure to store this key securely, in order to reactivate a manager after it restarts. Pass the key to the `docker swarm unlock` command to reactivate the manager. You can disable autolock by running `docker swarm update --autolock=false`. After disabling it, the encryption key is no longer required to start the manager, and it will start up on its own without user intervention.

​	`--autolock` 标志启用使用加密密钥自动锁定管理器。输出中的加密密钥保护所有管理器存储的私钥和数据，且在没有该密钥时无法访问。请确保将此密钥妥善存储，以便在重启后重新激活管理器。将密钥传递给 `docker swarm unlock` 命令以重新激活管理器。可以运行 `docker swarm update --autolock=false` 禁用自动锁定。禁用后，不再需要加密密钥来启动管理器，管理器将自动启动。

### 配置节点健康检查频率（`--dispatcher-heartbeat`） Configure node healthcheck frequency (`--dispatcher-heartbeat`)

The `--dispatcher-heartbeat` flag sets the frequency at which nodes are told to report their health.

​	`--dispatcher-heartbeat` 标志设置节点报告健康状况的频率。

### 使用外部证书颁发机构（`--external-ca`） Use an external certificate authority (`--external-ca`)

This flag sets up the swarm to use an external CA to issue node certificates. The value takes the form `protocol=X,url=Y`. The value for `protocol` specifies what protocol should be used to send signing requests to the external CA. Currently, the only supported value is `cfssl`. The URL specifies the endpoint where signing requests should be submitted.

​	此标志将 Swarm 配置为使用外部 CA 签发节点证书。值的格式为 `protocol=X,url=Y`。`protocol` 值指定向外部 CA 发送签名请求的协议，目前唯一支持的值是 `cfssl`。URL 指定提交签名请求的端点。

### 强制将节点重新启动为单节点管理器（`--force-new-cluster`）Force-restart node as a single-mode manager (`--force-new-cluster`)

This flag forces an existing node that was part of a quorum that was lost to restart as a single-node Manager without losing its data.

​	此标志强制已丢失仲裁的节点重新启动为单节点管理器，而不会丢失其数据。

### 指定入站控制平面流量的接口（`--listen-addr`）Specify interface for inbound control plane traffic (`--listen-addr`)

The node listens for inbound swarm manager traffic on this address. The default is to listen on `0.0.0.0:2377`. It is also possible to specify a network interface to listen on that interface's address; for example `--listen-addr eth0:2377`.

​	节点将在此地址监听入站的 Swarm 管理器流量。默认监听 `0.0.0.0:2377`。还可以指定一个网络接口以在该接口的地址上监听，例如 `--listen-addr eth0:2377`。

Specifying a port is optional. If the value is a bare IP address or interface name, the default port 2377 is used.

​	指定端口是可选的。如果值是裸 IP 地址或接口名称，将使用默认端口 2377。

### 指定出站控制平面流量的接口（`--advertise-addr`） Specify interface for outbound control plane traffic (`--advertise-addr`)

The `--advertise-addr` flag specifies the address that will be advertised to other members of the swarm for API access and overlay networking. If unspecified, Docker will check if the system has a single IP address, and use that IP address with the listening port (see `--listen-addr`). If the system has multiple IP addresses, `--advertise-addr` must be specified so that the correct address is chosen for inter-manager communication and overlay networking.

​	`--advertise-addr` 标志指定将向 Swarm 其他成员广播的 API 访问地址和覆盖网络地址。如果未指定，Docker 将检查系统是否有单个 IP 地址，并使用监听端口（参见 `--listen-addr`）的 IP 地址。如果系统有多个 IP 地址，则必须指定 `--advertise-addr` 以选择正确的地址进行管理器间通信和覆盖网络。

It is also possible to specify a network interface to advertise that interface's address; for example `--advertise-addr eth0:2377`.

​	还可以指定网络接口以广播该接口的地址，例如 `--advertise-addr eth0:2377`。

Specifying a port is optional. If the value is a bare IP address or interface name, the default port 2377 is used.

​	指定端口是可选的。如果值是裸 IP 地址或接口名称，将使用默认端口 2377。

### 指定数据流量的接口（`--data-path-addr`） Specify interface for data traffic (`--data-path-addr`)

The `--data-path-addr` flag specifies the address that global scope network drivers will publish towards other nodes in order to reach the containers running on this node. Using this parameter you can separate the container's data traffic from the management traffic of the cluster.

​	`--data-path-addr` 标志指定全局范围网络驱动程序向其他节点发布的地址，以便其他节点可以访问此节点上运行的容器。使用此参数可以将容器的数据流量与集群的管理流量分离开来。

If unspecified, the IP address or interface of the advertise address is used.

​	如果未指定，将使用广播地址的 IP 地址或接口。

Setting `--data-path-addr` does not restrict which interfaces or source IP addresses the VXLAN socket is bound to. Similar to `--advertise-addr`, the purpose of this flag is to inform other members of the swarm about which address to use for control plane traffic. To restrict access to the VXLAN port of the node, use firewall rules.

​	设置 `--data-path-addr` 并不会限制 VXLAN 套接字绑定的接口或源 IP 地址。类似于 `--advertise-addr`，此标志的目的是通知 Swarm 其他成员应该用于控制平面流量的地址。要限制对节点 VXLAN 端口的访问，请使用防火墙规则。

### 配置数据流量的端口号（`--data-path-port`） Configure port number for data traffic (`--data-path-port`)

The `--data-path-port` flag allows you to configure the UDP port number to use for data path traffic. The provided port number must be within the 1024 - 49151 range. If this flag isn't set, or if it's set to 0, the default port number 4789 is used. The data path port can only be configured when initializing the swarm, and applies to all nodes that join the swarm. The following example initializes a new Swarm, and configures the data path port to UDP port 7777;

​	`--data-path-port` 标志允许您配置数据路径流量的 UDP 端口号。提供的端口号必须在 1024 到 49151 范围内。如果未设置此标志，或者设置为 0，将使用默认端口号 4789。数据路径端口只能在初始化 Swarm 时配置，并适用于加入 Swarm 的所有节点。以下示例初始化一个新的 Swarm，并将数据路径端口配置为 UDP 端口 7777：



```console
$ docker swarm init --data-path-port=7777
```

After the swarm is initialized, use the `docker info` command to verify that the port is configured:

​	初始化 Swarm 后，使用 `docker info` 命令验证端口配置：



```console
$ docker info
<...>
ClusterID: 9vs5ygs0gguyyec4iqf2314c0
Managers: 1
Nodes: 1
Data Path Port: 7777
<...>
```

### 指定默认子网池（`--default-addr-pool`） Specify default subnet pools (`--default-addr-pool`)

The `--default-addr-pool` flag specifies default subnet pools for global scope networks. For example, to specify two address pools:

​	`--default-addr-pool` 标志为全局范围网络指定默认子网池。例如，指定两个地址池：



```console
$ docker swarm init \
  --default-addr-pool 30.30.0.0/16 \
  --default-addr-pool 40.40.0.0/16
```

Use the `--default-addr-pool-mask-length` flag to specify the default subnet pools mask length for the subnet pools.

​	使用 `--default-addr-pool-mask-length` 标志来指定子网池的默认子网掩码长度。

### 设置保留快照数量的限制 (`--max-snapshots`) Set limit for number of snapshots to keep (`--max-snapshots`)

This flag sets the number of old Raft snapshots to retain in addition to the current Raft snapshots. By default, no old snapshots are retained. This option may be used for debugging, or to store old snapshots of the swarm state for disaster recovery purposes.

​	此标志设置除当前 Raft 快照之外保留的旧快照数量。默认情况下，不保留旧快照。此选项可用于调试，或存储 Swarm 状态的旧快照以便于灾难恢复。

### 配置 Raft 快照日志间隔 (`--snapshot-interval`) Configure Raft snapshot log interval (`--snapshot-interval`)

The `--snapshot-interval` flag specifies how many log entries to allow in between Raft snapshots. Setting this to a high number will trigger snapshots less frequently. Snapshots compact the Raft log and allow for more efficient transfer of the state to new managers. However, there is a performance cost to taking snapshots frequently.

​	`--snapshot-interval` 标志指定允许 Raft 快照之间的日志条目数量。将此值设置为较大数值将减少快照触发频率。快照会压缩 Raft 日志，并允许更高效地将状态传输给新管理器。然而，频繁快照会带来性能开销。

### 配置管理器的可用性 (`--availability`) Configure the availability of a manager (`--availability`)

The `--availability` flag specifies the availability of the node at the time the node joins a master. Possible availability values are `active`, `pause`, or `drain`.

​	`--availability` 标志指定节点加入主机时的可用性。可能的可用性值为 `active`, `pause` 或 `drain`。This flag is useful in certain situations. For example, a cluster may want to have dedicated manager nodes that don't serve as worker nodes. You can do this by passing `--availability=drain` to `docker swarm init`.

​	此标志在某些情况下非常有用。例如，集群可能希望拥有仅用于管理的专用管理器节点，而这些节点不充当工作节点。可以通过传递 `--availability=drain` 给 `docker swarm init` 来实现。

