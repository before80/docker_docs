+++
title = "数据包过滤与防火墙"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/network/packet-filtering-firewalls/](https://docs.docker.com/engine/network/packet-filtering-firewalls/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Packet filtering and firewalls - 数据包过滤与防火墙

On Linux, Docker creates `iptables` and `ip6tables` rules to implement network isolation, port publishing and filtering.

​	在 Linux 上，Docker 使用 `iptables` 和 `ip6tables` 规则来实现网络隔离、端口发布和过滤。

Because these rules are required for the correct functioning of Docker bridge networks, you should not modify the rules created by Docker.

​	由于这些规则对于 Docker 桥接网络的正常工作至关重要，因此建议不要修改由 Docker 创建的规则。

But, if you are running Docker on a host exposed to the internet, you will probably want to add iptables policies that prevent unauthorized access to containers or other services running on your host. This page describes how to achieve that, and the caveats you need to be aware of.

​	但是，如果你在暴露于互联网的主机上运行 Docker，可能希望添加 `iptables` 策略以防止未经授权的访问。这一页面描述了实现这些策略的方法以及需要注意的事项。

> **Note**
>
> 
>
> Docker creates `iptables` rules for bridge networks.
>
> ​	Docker 会为桥接网络创建 `iptables` 规则。
>
> No `iptables` rules are created for `ipvlan`, `macvlan` or `host` networking.
>
> ​	不会为 `ipvlan`、`macvlan` 或 `host` 网络创建 `iptables` 规则。

## Docker 和 iptables 链 Docker and iptables chains

In the `filter` table, Docker sets the default policy to `DROP`, and creates the following custom `iptables` chains:

​	在 `filter` 表中，Docker 将默认策略设置为 `DROP`，并创建以下自定义 `iptables` 链：

- `DOCKER-USER`
  - A placeholder for user-defined rules that will be processed before rules in the `DOCKER` chain.
    - 用户定义规则的占位符，在 `DOCKER` 链中的规则之前处理。

- `DOCKER`
  - Rules that determine whether a packet that is not part of an established connection should be accepted, based on the port forwarding configuration of running containers.
    - 基于容器的端口转发配置规则，决定非已建立连接的数据包是否被接受。

- `DOCKER-ISOLATION-STAGE-1` and `DOCKER-ISOLATION-STAGE-2`
  - Rules to isolate Docker networks from each other.
    - 用于隔离不同 Docker 网络的规则。

In the `FORWARD` chain, Docker adds rules that pass packets that are not related to established connections to these custom chains, as well as rules to accept packets that are part of established connections.

​	在 `FORWARD` 链中，Docker 添加了规则，将与已建立连接无关的数据包传递到这些自定义链，以及接受已建立连接数据包的规则。

In the `nat` table, Docker creates chain `DOCKER` and adds rules to implement masquerading and port-mapping.

​	在 `nat` 表中，Docker 创建了 `DOCKER` 链，并添加了规则来实现伪装和端口映射。

### 在 Docker 规则之前添加 iptables 策略 Add iptables policies before Docker's rules

Packets that get accepted or rejected by rules in these custom chains will not be seen by user-defined rules appended to the `FORWARD` chain. So, to add additional rules to filter these packets, use the `DOCKER-USER` chain.

​	在这些自定义链中的规则所接受或拒绝的数据包不会被 `FORWARD` 链中追加的用户定义规则看到。因此，如果要为这些数据包添加额外的过滤规则，请使用 `DOCKER-USER` 链。

### 匹配请求的原始 IP 和端口 Match the original IP and ports for requests

When packets arrive to the `DOCKER-USER` chain, they have already passed through a Destination Network Address Translation (DNAT) filter. That means that the `iptables` flags you use can only match internal IP addresses and ports of containers.

​	当数据包到达 `DOCKER-USER` 链时，它们已经通过了目标网络地址转换 (DNAT) 过滤。这意味着您可以使用的 `iptables` 标志只能匹配容器的内部 IP 地址和端口。

If you want to match traffic based on the original IP and port in the network request, you must use the [`conntrack` iptables extension](https://ipset.netfilter.org/iptables-extensions.man.html#lbAO). For example:

​	如果您希望基于网络请求中的原始 IP 和端口进行流量匹配，必须使用 [`conntrack` iptables 扩展](https://ipset.netfilter.org/iptables-extensions.man.html#lbAO)。例如：

```console
$ sudo iptables -I DOCKER-USER -p tcp -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
$ sudo iptables -I DOCKER-USER -p tcp -m conntrack --ctorigdst 198.51.100.2 --ctorigdstport 80 -j ACCEPT
```

> **Important**
>
> 
>
> Using the `conntrack` extension may result in degraded performance.
>
> ​	使用 `conntrack` 扩展可能会导致性能下降。

## 端口发布与映射 Port publishing and mapping

By default, for both IPv4 and IPv6, the daemon blocks access to ports that have not been published. Published container ports are mapped to host IP addresses. To do this, it uses iptables to perform Network Address Translation (NAT), Port Address Translation (PAT), and masquerading.

​	默认情况下，对于 IPv4 和 IPv6，守护进程会阻止未发布端口的访问。已发布的容器端口会映射到主机的 IP 地址。为此，Docker 使用 `iptables` 执行网络地址转换 (NAT)、端口地址转换 (PAT) 和伪装。

For example, `docker run -p 8080:80 [...]` creates a mapping between port 8080 on any address on the Docker host, and the container's port 80. Outgoing connections from the container will masquerade, using the Docker host's IP address.

​	例如，`docker run -p 8080:80 [...]` 在 Docker 主机的任意地址与容器的端口 80 之间创建了一个映射。来自容器的外发连接将使用主机的 IP 地址进行伪装。

### 限制对容器的外部连接 Restrict external connections to containers

By default, all external source IPs are allowed to connect to ports that have been published to the Docker host's addresses.

​	默认情况下，所有外部源 IP 都可以连接到发布到 Docker 主机地址的端口。

To allow only a specific IP or network to access the containers, insert a negated rule at the top of the `DOCKER-USER` filter chain. For example, the following rule drops packets from all IP addresses except `192.0.2.2`:

​	要仅允许特定 IP 或网络访问容器，请在 `DOCKER-USER` 过滤链的顶部插入一个排除规则。例如，以下规则丢弃除 `192.0.2.2` 外的所有 IP 地址的数据包：

```console
$ iptables -I DOCKER-USER -i ext_if ! -s 192.0.2.2 -j DROP
```

You will need to change `ext_if` to correspond with your host's actual external interface. You could instead allow connections from a source subnet. The following rule only allows access from the subnet `192.0.2.0/24`:

​	您需要将 `ext_if` 替换为主机的实际外部接口。还可以允许来自源子网的连接。以下规则仅允许来自 `192.0.2.0/24` 子网的访问：

```console
$ iptables -I DOCKER-USER -i ext_if ! -s 192.0.2.0/24 -j DROP
```

Finally, you can specify a range of IP addresses to accept using `--src-range` (Remember to also add `-m iprange` when using `--src-range` or `--dst-range`):

​	最后，您可以使用 `--src-range` 指定一个接受的 IP 地址范围（使用 `--src-range` 或 `--dst-range` 时请记得添加 `-m iprange`）：

```console
$ iptables -I DOCKER-USER -m iprange -i ext_if ! --src-range 192.0.2.1-192.0.2.3 -j DROP
```

You can combine `-s` or `--src-range` with `-d` or `--dst-range` to control both the source and destination. For instance, if the Docker host has addresses `2001:db8:1111::2` and `2001:db8:2222::2`, you can make rules specific to `2001:db8:1111::2` and leave `2001:db8:2222::2` open.

​	您可以将 `-s` 或 `--src-range` 与 `-d` 或 `--dst-range` 结合使用，以控制源和目标地址。例如，如果 Docker 主机有地址 `2001:db8:1111::2` 和 `2001:db8:2222::2`，您可以对 `2001:db8:1111::2` 设置特定规则，同时让 `2001:db8:2222::2` 保持开放。

`iptables` is complicated. There is a lot more information at [Netfilter.org HOWTO](https://www.netfilter.org/documentation/HOWTO/NAT-HOWTO.html).

​	`iptables` 非常复杂。您可以在 [Netfilter.org HOWTO](https://www.netfilter.org/documentation/HOWTO/NAT-HOWTO.html) 查看更多信息。

### 直接路由 Direct routing

Port mapping ensures that published ports are accessible on the host's network addresses, which are likely to be routable for any external clients. No routes are normally set up in the host's network for container addresses that exist within a host.

​	端口映射确保发布的端口可通过主机的网络地址访问，这些地址可能对任何外部客户端是可路由的。通常情况下，不会在主机网络中为主机内存在的容器地址设置路由。

But, particularly with IPv6 you may prefer to avoid using NAT and instead arrange for external routing to container addresses.

​	但是，特别是对于 IPv6，您可能更倾向于避免使用 NAT，而是安排对容器地址的外部路由。

To access containers on a bridge network from outside the Docker host, you must set up routing to the bridge network via an address on the Docker host. This can be achieved using static routes, Border Gateway Protocol (BGP), or any other means appropriate for your network.

​	要从 Docker 主机外部访问桥接网络上的容器，必须通过 Docker 主机上的一个地址设置到桥接网络的路由。这可以通过静态路由、边界网关协议 (BGP) 或适合您网络的任何其他方法来实现。

The bridge network driver has options `com.docker.network.bridge.gateway_mode_ipv6=<nat|routed>` and `com.docker.network.bridge.gateway_mode_ipv4=<nat|routed>`.

​	桥接网络驱动程序具有选项 `com.docker.network.bridge.gateway_mode_ipv6=<nat|routed>` 和 `com.docker.network.bridge.gateway_mode_ipv4=<nat|routed>`。

The default is `nat`, NAT and masquerading rules are set up for each published container port. With mode `routed`, no NAT or masquerading rules are set up, but `iptables` are still set up so that only published container ports are accessible.

​	默认模式是 `nat`，即为每个发布的容器端口设置 NAT 和伪装规则。在 `routed` 模式下，不设置 NAT 或伪装规则，但仍然设置 `iptables` 规则，以便只有发布的容器端口可以访问。

In `routed` mode, a host port in a `-p` or `--publish` port mapping is not used, and the host address is only used to decide whether to apply the mapping to IPv4 or IPv6. So, when a mapping only applies to `routed` mode, only addresses `0.0.0.0` or `::1` are allowed, and a host port must not be given.

​	在 `routed` 模式下，`-p` 或 `--publish` 端口映射中的主机端口不会使用，主机地址仅用于决定将映射应用于 IPv4 还是 IPv6。因此，当映射仅适用于 `routed` 模式时，只允许地址 `0.0.0.0` 或 `::1`，且不得指定主机端口。

Mapped container ports, in `nat` or `routed` mode, are accessible from any remote address, if routing is set up in the network, unless the Docker host's firewall has additional restrictions.

​	无论在 `nat` 还是 `routed` 模式下，如果网络中设置了路由，映射的容器端口可以从任何远程地址访问，除非 Docker 主机的防火墙有额外的限制。

#### Example

Create a network suitable for direct routing for IPv6, with NAT enabled for IPv4:

​	创建一个适用于 IPv6 直接路由、启用了 IPv4 NAT 的网络：



```console
$ docker network create --ipv6 --subnet 2001:db8::/64 -o com.docker.network.bridge.gateway_mode_ipv6=routed mynet
```

Create a container with a published port:

​	创建一个带有已发布端口的容器：



```console
$ docker run --network=mynet -p 8080:80 myimage
```

Then:

​	此时：

- Only container port 80 will be open, for IPv4 and IPv6. It is accessible from anywhere, if there is routing to the container's address, and access is not blocked by the host's firewall.
  - 仅容器的端口 80 会开放，适用于 IPv4 和 IPv6。如果有路由到容器的地址，并且主机的防火墙未阻止访问，则该端口可从任何地方访问。

- For IPv6, using `routed` mode, port 80 will be open on the container's IP address. Port 8080 will not be opened on the host's IP addresses, and outgoing packets will use the container's IP address.
  - 对于 IPv6，使用 `routed` 模式，端口 80 将在容器的 IP 地址上开放。端口 8080 不会在主机的 IP 地址上打开，出站数据包将使用容器的 IP 地址。

- For IPv4, using the default `nat` mode, the container's port 80 will be accessible via port 8080 on the host's IP addresses, as well as directly. Connections originating from the container will masquerade, using the host's IP address.
  - 对于 IPv4，使用默认的 `nat` 模式，容器的端口 80 可通过主机 IP 地址上的端口 8080 直接访问。来自容器的连接将伪装，使用主机的 IP 地址。


In `docker inspect`, this port mapping will be shown as follows. Note that there is no `HostPort` for IPv6, because it is using `routed` mode:

​	在 `docker inspect` 中，此端口映射将显示如下。请注意，IPv6 没有 `HostPort`，因为它使用的是 `routed` 模式：



```console
$ docker container inspect <id> --format "{{json .NetworkSettings.Ports}}"
{"80/tcp":[{"HostIp":"0.0.0.0","HostPort":"8080"},{"HostIp":"::","HostPort":""}]}
```

Alternatively, to make the mapping IPv6-only, disabling IPv4 access to the container's port 80, use the unspecified IPv6 address `[::]` and do not include a host port number:

​	或者，要使映射仅适用于 IPv6，禁用对容器端口 80 的 IPv4 访问，可以使用未指定的 IPv6 地址 `[::]`，并且不包含主机端口号：



```console
$ docker run --network mynet -p '[::]::80'
```

### 设置容器的默认绑定地址 Setting the default bind address for containers

By default, when a container's ports are mapped without any specific host address, the Docker daemon binds published container ports to all host addresses (`0.0.0.0` and `[::]`).

​	默认情况下，当容器端口映射没有指定主机地址时，Docker 守护进程会将已发布的容器端口绑定到所有主机地址 (`0.0.0.0` 和 `[::]`)。

For example, the following command publishes port 8080 to all network interfaces on the host, on both IPv4 and IPv6 addresses, potentially making them available to the outside world.

​	例如，以下命令将端口 8080 发布到主机上的所有网络接口，包括 IPv4 和 IPv6 地址，从而可能使它们对外部世界开放。



```console
docker run -p 8080:80 nginx
```

You can change the default binding address for published container ports so that they're only accessible to the Docker host by default. To do that, you can configure the daemon to use the loopback address (`127.0.0.1`) instead.

​	您可以更改已发布容器端口的默认绑定地址，使其仅对 Docker 主机可访问。为此，可以将守护进程配置为使用回环地址 (`127.0.0.1`)。

> **Warning**
>
> 
>
> Hosts within the same L2 segment (for example, hosts connected to the same network switch) can reach ports published to localhost. For more information, see [moby/moby#45610](https://github.com/moby/moby/issues/45610)
>
> ​	同一 L2 网络段中的主机（例如，连接到同一网络交换机的主机）可以访问发布到本地主机的端口。更多信息请参见 [moby/moby#45610](https://github.com/moby/moby/issues/45610)

To configure this setting for user-defined bridge networks, use the `com.docker.network.bridge.host_binding_ipv4` [driver option](https://docs.docker.com/engine/network/drivers/bridge/#options) when you create the network.

​	要为用户定义的桥接网络配置此设置，请在创建网络时使用 `com.docker.network.bridge.host_binding_ipv4` [驱动程序选项](https://docs.docker.com/engine/network/drivers/bridge/#options)。

```console
$ docker network create mybridge \
  -o "com.docker.network.bridge.host_binding_ipv4=127.0.0.1"
```

> **Note**
>
> 
>
> - Setting the default binding address to `::` means port bindings with no host address specified will work for any IPv6 address on the host. But, `0.0.0.0` means any IPv4 or IPv6 address.
>   - 将默认绑定地址设置为 `::` 意味着没有指定主机地址的端口绑定将适用于主机上的任何 IPv6 地址。而 `0.0.0.0` 表示任何 IPv4 或 IPv6 地址。
> - Changing the default bind address doesn't have any effect on Swarm services. Swarm services are always exposed on the `0.0.0.0` network interface.
>   - 更改默认绑定地址对 Swarm 服务没有影响。Swarm 服务始终暴露在 `0.0.0.0` 网络接口上。

#### 默认桥接 Default bridge

To set the default binding for the default bridge network, configure the `"ip"` key in the `daemon.json` configuration file:

​	要为默认桥接网络设置默认绑定地址，请在 `daemon.json` 配置文件中配置 `"ip"` 键：



```json
{
  "ip": "127.0.0.1"
}
```

This changes the default binding address to `127.0.0.1` for published container ports on the default bridge network. Restart the daemon for this change to take effect. Alternatively, you can use the `dockerd --ip` flag when starting the daemon.

​	这将默认绑定地址更改为 `127.0.0.1`，适用于默认桥接网络上已发布的容器端口。重启守护进程以使更改生效。或者，可以在启动守护进程时使用 `dockerd --ip` 标志。

## 路由器上的 Docker - Docker on a router

Docker sets the policy for the `FORWARD` chain to `DROP`. This will prevent your Docker host from acting as a router.

​	Docker 将 `FORWARD` 链的策略设置为 `DROP`，这会阻止您的 Docker 主机充当路由器。

If you want your system to function as a router, you must add explicit `ACCEPT` rules to the `DOCKER-USER` chain. For example:

​	如果您希望系统充当路由器，则必须在 `DOCKER-USER` 链中添加显式 `ACCEPT` 规则。例如：

```console
$ iptables -I DOCKER-USER -i src_if -o dst_if -j ACCEPT
```

## 防止 Docker 操控 iptables - Prevent Docker from manipulating iptables

It is possible to set the `iptables` or `ip6tables` keys to `false` in [daemon configuration]({{< ref "/reference/CLIreference/dockerd" >}}), but this option is not appropriate for most users. It is likely to break container networking for the Docker Engine.

​	可以在 [守护进程配置]({{< ref "/reference/CLIreference/dockerd" >}}) 中将 `iptables` 或 `ip6tables` 键设置为 `false`，但大多数用户不应使用此选项。这可能会破坏 Docker 引擎的容器网络。

All ports of all containers will be accessible from the network, and none will be mapped from Docker host IP addresses.

​	所有容器的所有端口都将从网络中可访问，而无需通过 Docker 主机的 IP 地址映射。

It is not possible to completely prevent Docker from creating `iptables` rules, and creating rules after-the-fact is extremely involved and beyond the scope of these instructions.

​	完全阻止 Docker 创建 `iptables` 规则是不可能的，事后手动创建规则极为复杂，超出了本指南的范围。

## 与 firewalld 的集成 Integration with firewalld

If you are running Docker with the `iptables` option set to `true`, and [firewalld](https://firewalld.org/) is enabled on your system, Docker automatically creates a `firewalld` zone called `docker`, with target `ACCEPT`.

​	如果您在将 `iptables` 选项设置为 `true` 的情况下运行 Docker，且系统中启用了 [firewalld](https://firewalld.org/)，Docker 会自动创建一个名为 `docker` 的 `firewalld` 区域，目标设置为 `ACCEPT`。

All network interfaces created by Docker (for example, `docker0`) are inserted into the `docker` zone.

​	Docker 创建的所有网络接口（例如 `docker0`）将被插入到 `docker` 区域中。

Docker also creates a forwarding policy called `docker-forwarding` that allows forwarding from `ANY` zone to the `docker` zone.

​	Docker 还创建了一个名为 `docker-forwarding` 的转发策略，允许从 `ANY` 区域到 `docker` 区域的转发。

## Docker and ufw

[Uncomplicated Firewall](https://launchpad.net/ufw) (ufw) is a frontend that ships with Debian and Ubuntu, and it lets you manage firewall rules. Docker and ufw use iptables in ways that make them incompatible with each other.

​	[Uncomplicated Firewall](https://launchpad.net/ufw) (ufw) 是 Debian 和 Ubuntu 自带的防火墙前端管理工具，用于管理防火墙规则。Docker 和 ufw 使用 `iptables` 的方式导致它们不兼容。

When you publish a container's ports using Docker, traffic to and from that container gets diverted before it goes through the ufw firewall settings. Docker routes container traffic in the `nat` table, which means that packets are diverted before it reaches the `INPUT` and `OUTPUT` chains that ufw uses. Packets are routed before the firewall rules can be applied, effectively ignoring your firewall configuration.

​	当您使用 Docker 发布容器的端口时，容器的流量在到达 ufw 防火墙设置之前会被重定向。Docker 在 `nat` 表中路由容器流量，这意味着数据包在到达 ufw 使用的 `INPUT` 和 `OUTPUT` 链之前就已被重定向。这样，数据包在应用防火墙规则之前就被路由，导致您的防火墙配置被有效地忽略。
