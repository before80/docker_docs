+++
title = "使用 IPv6 网络"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/daemon/ipv6/](https://docs.docker.com/engine/daemon/ipv6/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use IPv6 networking - 使用 IPv6 网络

IPv6 is only supported on Docker daemons running on Linux hosts.

​	IPv6 仅支持在运行于 Linux 主机上的 Docker 守护进程上使用。

## 创建 IPv6 网络 Create an IPv6 network

- Using `docker network create`:

  使用 `docker network create`：

  ```console
  $ docker network create --ipv6 ip6net
  ```

- Using `docker network create`, specifying an IPv6 subnet:

  使用 `docker network create`，指定一个 IPv6 子网：

  ```console
  $ docker network create --ipv6 --subnet 2001:db8::/64 ip6net
  ```

- Using a Docker Compose file:

  使用 Docker Compose 文件：

  ```yaml
   networks:
     ip6net:
       enable_ipv6: true
       ipam:
         config:
           - subnet: 2001:db8::/64
  ```

You can now run containers that attach to the `ip6net` network.

​	现在您可以运行连接到 `ip6net` 网络的容器。

```console
$ docker run --rm --network ip6net -p 80:80 traefik/whoami
```

This publishes port 80 on both IPv6 and IPv4. You can verify the IPv6 connection by running curl, connecting to port 80 on the IPv6 loopback address:

​	这将同时在 IPv6 和 IPv4 上发布端口 80。您可以通过在 IPv6 本地回环地址上运行 curl 来验证 IPv6 连接：

```console
$ curl http://[::1]:80
Hostname: ea1cfde18196
IP: 127.0.0.1
IP: ::1
IP: 172.17.0.2
IP: 2001:db8::2
IP: fe80::42:acff:fe11:2
RemoteAddr: [2001:db8::1]:37574
GET / HTTP/1.1
Host: [::1]
User-Agent: curl/8.1.2
Accept: */*
```

## 在默认桥接网络中使用 IPv6 - Use IPv6 for the default bridge network

The following steps show you how to use IPv6 on the default bridge network.

​	以下步骤显示了如何在默认桥接网络中使用 IPv6。

1. Edit the Docker daemon configuration file, located at `/etc/docker/daemon.json`. Configure the following parameters:

   编辑 Docker 守护进程配置文件，位于 `/etc/docker/daemon.json`。配置以下参数：

   ```json
   {
     "ipv6": true,
     "fixed-cidr-v6": "2001:db8:1::/64"
   }
   ```

   - `ipv6` enables IPv6 networking on the default network.
     - `ipv6` 启用默认网络的 IPv6 网络。
   - `fixed-cidr-v6` assigns a subnet to the default bridge network, enabling dynamic IPv6 address allocation.
     - `fixed-cidr-v6` 为默认桥接网络分配一个子网，从而启用动态 IPv6 地址分配。
   - `ip6tables` enables additional IPv6 packet filter rules, providing network isolation and port mapping. It is enabled by-default, but can be disabled.
     - `ip6tables` 启用额外的 IPv6 数据包过滤规则，提供网络隔离和端口映射。默认启用，但可以禁用。

2. Save the configuration file. 保存配置文件。

3. Restart the Docker daemon for your changes to take effect.

   重启 Docker 守护进程以使更改生效。

   ```console
   $ sudo systemctl restart docker
   ```

You can now run containers on the default bridge network.

​	现在您可以在默认桥接网络中运行容器。

```console
$ docker run --rm -p 80:80 traefik/whoami
```

This publishes port 80 on both IPv6 and IPv4. You can verify the IPv6 connection by making a request to port 80 on the IPv6 loopback address:

​	这将同时在 IPv6 和 IPv4 上发布端口 80。您可以通过在 IPv6 本地回环地址上发出请求来验证 IPv6 连接：

```console
$ curl http://[::1]:80
Hostname: ea1cfde18196
IP: 127.0.0.1
IP: ::1
IP: 172.17.0.2
IP: 2001:db8:1::242:ac12:2
IP: fe80::42:acff:fe12:2
RemoteAddr: [2001:db8:1::1]:35558
GET / HTTP/1.1
Host: [::1]
User-Agent: curl/8.1.2
Accept: */*
```

## 动态 IPv6 子网分配 Dynamic IPv6 subnet allocation

If you don't explicitly configure subnets for user-defined networks, using `docker network create --subnet=<your-subnet>`, those networks use the default address pools of the daemon as a fallback. This also applies to networks created from a Docker Compose file, with `enable_ipv6` set to `true`.

​	如果未明确为用户定义的网络配置子网（例如使用 `docker network create --subnet=<your-subnet>`），这些网络会作为后备使用守护进程的默认地址池。这也适用于从 Docker Compose 文件创建的网络，并且 `enable_ipv6` 设置为 `true`。

If no IPv6 pools are included in Docker Engine's `default-address-pools`, and no `--subnet` option is given, [Unique Local Addresses (ULAs)](https://en.wikipedia.org/wiki/Unique_local_address) will be used when IPv6 is enabled. These `/64` subnets include a 40-bit Global ID based on the Docker Engine's randomly generated ID, to give a high probability of uniqueness.

​	如果 Docker Engine 的 `default-address-pools` 中没有包含 IPv6 池且未指定 `--subnet` 选项，则在启用 IPv6 时将使用 [唯一本地地址 (ULAs)](https://en.wikipedia.org/wiki/Unique_local_address)。这些 `/64` 子网包括基于 Docker Engine 随机生成的 ID 的 40 位全球 ID，从而提高了唯一性概率。

To use different pools of IPv6 subnets for dynamic address allocation, you must manually configure address pools of the daemon to include:

​	要为动态地址分配使用不同的 IPv6 子网池，必须手动配置守护进程的地址池，包括：

- The default IPv4 address pools
  - 默认的 IPv4 地址池

- One or more IPv6 pools of your own
  - 一个或多个自定义的 IPv6 地址池


The default address pool configuration is:

​	默认地址池配置如下：

```json
{
  "default-address-pools": [
    { "base": "172.17.0.0/16", "size": 16 },
    { "base": "172.18.0.0/16", "size": 16 },
    { "base": "172.19.0.0/16", "size": 16 },
    { "base": "172.20.0.0/14", "size": 16 },
    { "base": "172.24.0.0/14", "size": 16 },
    { "base": "172.28.0.0/14", "size": 16 },
    { "base": "192.168.0.0/16", "size": 20 }
  ]
}
```

The following example shows a valid configuration with the default values and an IPv6 pool. The IPv6 pool in the example provides up to 256 IPv6 subnets of size `/64`, from an IPv6 pool of prefix length `/56`.

​	以下示例显示了一个包含默认值和 IPv6 池的有效配置。示例中的 IPv6 池提供了一个 `/56` 前缀的 IPv6 池，用于分配大小为 `/64` 的 256 个 IPv6 子网。

```json
{
  "default-address-pools": [
    { "base": "172.17.0.0/16", "size": 16 },
    { "base": "172.18.0.0/16", "size": 16 },
    { "base": "172.19.0.0/16", "size": 16 },
    { "base": "172.20.0.0/14", "size": 16 },
    { "base": "172.24.0.0/14", "size": 16 },
    { "base": "172.28.0.0/14", "size": 16 },
    { "base": "192.168.0.0/16", "size": 20 },
    { "base": "2001:db8::/56", "size": 64 }
  ]
}
```

> **Note**
>
> 
>
> The address `2001:db8::` in this example is [reserved for use in documentation](https://en.wikipedia.org/wiki/Reserved_IP_addresses#IPv6). Replace it with a valid IPv6 network.
>
> ​	`2001:db8::` 地址在此示例中保留供文档使用，请替换为有效的 IPv6 网络。
>
> The default IPv4 pools are from the private address range, similar to the default IPv6 [ULA](https://en.wikipedia.org/wiki/Unique_local_address) networks.
>
> ​	默认的 IPv4 池来自私有地址范围，类似于默认的 IPv6 [ULA](https://en.wikipedia.org/wiki/Unique_local_address) 网络。

## Docker in Docker

On a host using `xtables` (legacy `iptables`) instead of `nftables`, kernel module `ip6_tables` must be loaded before an IPv6 Docker network can be created, It is normally loaded automatically when Docker starts.

​	在使用 `xtables`（旧版 `iptables`）而非 `nftables` 的主机上，内核模块 `ip6_tables` 必须在创建 IPv6 Docker 网络之前加载。通常，Docker 启动时会自动加载它。

However, if you running Docker in Docker that is not based on a recent version of the [official `docker` image](https://hub.docker.com/_/docker), you may need to run `modprobe ip6_tables` on your host. Alternatively, use daemon option `--ip6tables=false` to disable `ip6tables` for the containerized Docker Engine.

​	但是，如果您运行的 Docker in Docker 基于旧版的 [官方 `docker` 镜像](https://hub.docker.com/_/docker)，则可能需要在主机上运行 `modprobe ip6_tables`。或者，可以使用守护进程选项 `--ip6tables=false` 禁用容器化 Docker 引擎的 `ip6tables`。

## 接下来 Next steps

- [Networking overview 网络概述]({{< ref "/manuals/DockerEngine/Networking" >}})
