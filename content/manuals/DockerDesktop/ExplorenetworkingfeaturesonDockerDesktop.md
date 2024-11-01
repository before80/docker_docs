+++
title = "在 Docker Desktop 上探索网络功能"
date = 2024-10-23T14:54:40+08:00
weight = 110
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/networking/](https://docs.docker.com/desktop/networking/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Explore networking features on Docker Desktop - 在 Docker Desktop 上探索网络功能

Docker Desktop provides several networking features to make it easier to use.

​	Docker Desktop 提供了一些网络功能，以便更轻松地使用。

## 所有平台的网络功能 Networking features for all platforms

### VPN 直通 VPN Passthrough

Docker Desktop networking can work when attached to a VPN. To do this, Docker Desktop intercepts traffic from the containers and injects it into the host as if it originated from the Docker application.

​	Docker Desktop 网络可以在连接 VPN 时工作。为此，Docker Desktop 会拦截来自容器的流量并将其注入主机，仿佛它来自 Docker 应用。

### 端口映射 Port mapping

When you run a container with the `-p` argument, for example:

​	当您使用 `-p` 参数运行容器时，例如：



```console
$ docker run -p 80:80 -d nginx
```

Docker Desktop makes whatever is running on port 80 in the container, in this case, `nginx`, available on port 80 of `localhost`. In this example, the host and container ports are the same. If, for example, you already have something running on port 80 of your host machine, you can connect the container to a different port:

​	Docker Desktop 使容器中运行在端口 80 的内容（此处为 `nginx`）在 `localhost` 的端口 80 上可用。在此示例中，主机和容器端口相同。如果主机的端口 80 已被占用，您可以将容器连接到其他端口：



```console
$ docker run -p 8000:80 -d nginx
```

Now, connections to `localhost:8000` are sent to port 80 in the container. The syntax for `-p` is `HOST_PORT:CLIENT_PORT`.

​	现在，对 `localhost:8000` 的连接将发送到容器中的端口 80。`-p` 的语法为 `HOST_PORT:CLIENT_PORT`。

### HTTP/HTTPS 代理支持 HTTP/HTTPS Proxy support

See [Proxies](https://docs.docker.com/desktop/settings/#proxies)

​	请参见 [代理](https://docs.docker.com/desktop/settings/#proxies)。

### SOCKS5 代理支持 SOCKS5 proxy support

Introduced in Docker Desktop version [4.28.0](https://docs.docker.com/desktop/release-notes/#4280)

​	在 Docker Desktop 版本 [4.28.0](https://docs.docker.com/desktop/release-notes/#4280) 中引入。

> **Note**
>
> 
>
> Requires a Business subscription.
>
> ​	需要 Business 订阅。

SOCKS (Socket Secure) is a protocol that facilitates the routing of network packets between a client and a server through a proxy server. It provides a way to enhance privacy, security, and network performance for users and applications.

​	SOCKS（Socket Secure）是一种协议，便于通过代理服务器在客户端和服务器之间路由网络数据包。它为用户和应用提供了增强隐私、安全性和网络性能的方法。

You can enable SOCKS proxy support to allow outgoing requests, such as pulling images, and access Linux container backend IPs from the host.

​	您可以启用 SOCKS 代理支持，以允许传出请求，例如拉取镜像，并从主机访问 Linux 容器后端 IP。

To enable and set up SOCKS proxy support:

​	要启用和设置 SOCKS 代理支持：

1. Navigate to the **Resources** tab in **Settings**. 导航到 **Settings** 中的 **Resources** 选项卡。
2. From the dropdown menu select **Proxies**. 从下拉菜单中选择 **Proxies**。
3. Switch on the **Manual proxy configuration** toggle. 打开 **Manual proxy configuration** 开关。
4. In the **Secure Web Server HTTPS** box, paste your `socks5://host:port` URL. 在 **Secure Web Server HTTPS** 框中粘贴您的 `socks5://host:port` URL。

## Mac 和 Linux 的网络功能 Networking features for Mac and Linux

### SSH 代理转发 SSH agent forwarding

Docker Desktop on Mac and Linux allows you to use the host’s SSH agent inside a container. To do this:

​	Mac 和 Linux 上的 Docker Desktop 允许您在容器内使用主机的 SSH 代理。操作如下：

1. Bind mount the SSH agent socket by adding the following parameter to your `docker run` command:

   通过在 `docker run` 命令中添加以下参数来绑定挂载 SSH 代理套接字：

   ```console
   $--mount type=bind,src=/run/host-services/ssh-auth.sock,target=/run/host-services/ssh-auth.sock
   ```

2. Add the `SSH_AUTH_SOCK` environment variable in your container:

   在容器中添加 `SSH_AUTH_SOCK` 环境变量：

   ```console
   $ -e SSH_AUTH_SOCK="/run/host-services/ssh-auth.sock"
   ```

To enable the SSH agent in Docker Compose, add the following flags to your service:

​	要在 Docker Compose 中启用 SSH 代理，请将以下标志添加到服务中：

```yaml
services:
 web:
   image: nginx:alpine
   volumes:
     - type: bind
       source: /run/host-services/ssh-auth.sock
       target: /run/host-services/ssh-auth.sock
   environment:
     - SSH_AUTH_SOCK=/run/host-services/ssh-auth.sock
```

## 已知限制 Known limitations

### 更改内部 IP 地址 Changing internal IP addresses

The internal IP addresses used by Docker can be changed from **Settings**. After changing IPs, it is necessary to reset the Kubernetes cluster and to leave any active Swarm.

​	可以从 **Settings** 中更改 Docker 使用的内部 IP 地址。更改 IP 后，需重置 Kubernetes 集群并退出所有活动的 Swarm。

### 主机上没有 docker0 桥接 There is no docker0 bridge on the host

Because of the way networking is implemented in Docker Desktop, you cannot see a `docker0` interface on the host. This interface is actually within the virtual machine.

​	由于 Docker Desktop 的网络实现方式，主机上看不到 `docker0` 接口。该接口实际上在虚拟机中。

### 无法 ping 容器 I cannot ping my containers

Docker Desktop can't route traffic to Linux containers. However if you're a Windows user, you can ping the Windows containers.

​	Docker Desktop 无法将流量路由到 Linux 容器。然而，如果您是 Windows 用户，可以 ping Windows 容器。

### 无法实现每个容器的 IP 地址 Per-container IP addressing is not possible

This is because the Docker `bridge` network is not reachable from the host. However if you are a Windows user, per-container IP addressing is possible with Windows containers.

​	这是因为主机无法访问 Docker `bridge` 网络。然而，如果您是 Windows 用户，则可以在 Windows 容器上实现每个容器的 IP 地址。

## 用例和解决方法 Use cases and workarounds

### 从容器连接到主机上的服务 I want to connect from a container to a service on the host

The host has a changing IP address, or none if you have no network access. We recommend that you connect to the special DNS name `host.docker.internal`, which resolves to the internal IP address used by the host.

​	主机的 IP 地址可能会变化，或在无网络访问时没有 IP 地址。建议连接到特殊 DNS 名称 `host.docker.internal`，该名称解析为主机的内部 IP 地址。

You can also reach the gateway using `gateway.docker.internal`.

​	您还可以使用 `gateway.docker.internal` 到达网关。

If you have installed Python on your machine, use the following instructions as an example to connect from a container to a service on the host:

​	如果您的机器上安装了 Python，请使用以下步骤从容器连接到主机上的服务：

1. Run the following command to start a simple HTTP server on port 8000. 使用以下命令在端口 8000 上启动一个简单的 HTTP 服务器。

   `python -m http.server 8000`

   If you have installed Python 2.x, run `python -m SimpleHTTPServer 8000`.

   ​	如果安装了 Python 2.x，请运行 `python -m SimpleHTTPServer 8000`。

2. Now, run a container, install `curl`, and try to connect to the host using the following commands:

   现在，运行一个容器，安装 `curl`，并尝试使用以下命令连接到主机：

   ```console
   $ docker run --rm -it alpine sh
   # apk add curl
   # curl http://host.docker.internal:8000
   # exit
   ```

### 从主机连接到容器 I want to connect to a container from the host

Port forwarding works for `localhost`. `--publish`, `-p`, or `-P` all work. Ports exposed from Linux are forwarded to the host.

​	端口转发适用于 `localhost`。`--publish`、`-p` 或 `-P` 都可以使用。从 Linux 暴露的端口会转发到主机。

We recommend you publish a port, or to connect from another container. This is what you need to do even on Linux if the container is on an overlay network, not a bridge network, as these are not routed.

​	建议您发布一个端口，或从另一个容器连接。这也是 Linux 上的容器在非桥接网络（如叠加网络）上无法路由的情况下所需的操作。

For example, to run an `nginx` webserver:

​	例如，要运行一个 `nginx` Web 服务器：

```console
$ docker run -d -p 80:80 --name webserver nginx
```

To clarify the syntax, the following two commands both publish container's port `80` to host's port `8000`:

​	以下两个命令均将容器的端口 `80` 发布到主机的端口 `8000`：

```console
$ docker run --publish 8000:80 --name webserver nginx

$ docker run -p 8000:80 --name webserver nginx
```

To publish all ports, use the `-P` flag. For example, the following command starts a container (in detached mode) and the `-P` flag publishes all exposed ports of the container to random ports on the host.

​	要发布所有端口，请使用 `-P` 标志。例如，以下命令启动一个容器（以分离模式），并通过 `-P` 标志将容器的所有公开端口发布到主机的随机端口。

```console
$ docker run -d -P --name webserver nginx
```

Alternatively, you can also use [host networking](https://docs.docker.com/engine/network/drivers/host/#docker-desktop) to give the container direct access to the network stack of the host.

​	或者，您还可以使用 [host 网络](https://docs.docker.com/engine/network/drivers/host/#docker-desktop)，以便容器直接访问主机的网络栈。

See the [run command]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerrun" >}}) for more details on publish options used with `docker run`.

​	有关与 `docker run` 一起使用的发布选项的详细信息，请参阅 [run 命令]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerrun" >}})。
