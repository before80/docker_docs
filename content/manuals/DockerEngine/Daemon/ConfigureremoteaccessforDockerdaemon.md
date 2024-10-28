+++
title = "配置 Docker 守护进程的远程访问"
date = 2024-10-23T14:54:40+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/daemon/remote-access/](https://docs.docker.com/engine/daemon/remote-access/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Configure remote access for Docker daemon - 配置 Docker 守护进程的远程访问

By default, the Docker daemon listens for connections on a Unix socket to accept requests from local clients. You can configure Docker to accept requests from remote clients by configuring it to listen on an IP address and port as well as the Unix socket.

​	默认情况下，Docker 守护进程监听 Unix 套接字以接受来自本地客户端的连接请求。您可以配置 Docker 以接受远程客户端的请求，使其在 Unix 套接字的基础上还监听 IP 地址和端口。

> **Warning**
>
> 
>
> Configuring Docker to accept connections from remote clients can leave you vulnerable to unauthorized access to the host and other attacks.
>
> ​	配置 Docker 接受远程客户端连接可能会导致主机暴露在未经授权的访问和其他攻击风险之下。
>
> It's critically important that you understand the security implications of opening Docker to the network. If steps aren't taken to secure the connection, it's possible for remote non-root users to gain root access on the host.
>
> ​	开放 Docker 网络访问之前，务必了解其中的安全风险。如果没有采取措施来保护连接，远程的非 root 用户可能获得主机的 root 权限。
>
> Remote access without TLS is **not recommended**, and will require explicit opt-in in a future release. For more information on how to use TLS certificates to secure this connection, see [Protect the Docker daemon socket]({{< ref "/manuals/DockerEngine/Security/ProtecttheDockerdaemonsocket" >}}).
>
> ​	不推荐在没有 TLS 的情况下开启远程访问，并将在未来版本中需要明确选择加入。有关如何使用 TLS 证书保护该连接的详细信息，请参阅 [保护 Docker 守护进程套接字]({{< ref "/manuals/DockerEngine/Security/ProtecttheDockerdaemonsocket" >}})。

## 启用远程访问 Enable remote access

You can enable remote access to the daemon either using a `docker.service` systemd unit file for Linux distributions using systemd. Or you can use the `daemon.json` file, if your distribution doesn't use systemd.

​	您可以使用 `docker.service` 的 systemd 单元文件启用远程访问（适用于使用 systemd 的 Linux 发行版），或者如果您的发行版不使用 systemd，可以使用 `daemon.json` 文件配置。

Configuring Docker to listen for connections using both the systemd unit file and the `daemon.json` file causes a conflict that prevents Docker from starting.

​	同时使用 systemd 单元文件和 `daemon.json` 文件配置 Docker 连接监听会引起冲突，导致 Docker 无法启动。

### 使用 systemd 单元文件配置远程访问 Configuring remote access with systemd unit file

1. Use the command `sudo systemctl edit docker.service` to open an override file for `docker.service` in a text editor. 使用命令 `sudo systemctl edit docker.service` 在文本编辑器中打开 `docker.service` 的覆盖文件。

2. Add or modify the following lines, substituting your own values.

   添加或修改以下行，替换为您的值。

   ```systemd
   [Service]
   ExecStart=
   ExecStart=/usr/bin/dockerd -H fd:// -H tcp://127.0.0.1:2375
   ```

3. Save the file. 保存文件。

4. Reload the `systemctl` configuration.

   重新加载 `systemctl` 配置。

   ```console
   $ sudo systemctl daemon-reload
   ```

5. Restart Docker.

   重启 Docker。

   ```console
   $ sudo systemctl restart docker.service
   ```

6. Verify that the change has gone through.

   验证更改是否生效。

   ```console
   $ sudo netstat -lntp | grep dockerd
   tcp        0      0 127.0.0.1:2375          0.0.0.0:*               LISTEN      3758/dockerd
   ```

### 使用 `daemon.json` 配置远程访问 Configuring remote access with `daemon.json`

1. Set the `hosts` array in the `/etc/docker/daemon.json` to connect to the Unix socket and an IP address, as follows:

   在 `/etc/docker/daemon.json` 中设置 `hosts` 数组，连接到 Unix 套接字和一个 IP 地址，配置如下：

   ```json
   {
     "hosts": ["unix:///var/run/docker.sock", "tcp://127.0.0.1:2375"]
   }
   ```

2. Restart Docker. 重启 Docker。

3. Verify that the change has gone through. 

   验证更改是否生效。

   ```console
   $ sudo netstat -lntp | grep dockerd
   tcp        0      0 127.0.0.1:2375          0.0.0.0:*               LISTEN      3758/dockerd
   ```

### 通过防火墙允许远程 API 访问 Allow access to the remote API through a firewall

If you run a firewall on the same host as you run Docker, and you want to access the Docker Remote API from another remote host, you must configure your firewall to allow incoming connections on the Docker port. The default port is `2376` if you're using TLS encrypted transport, or `2375` otherwise.

​	如果您在运行 Docker 的主机上启用了防火墙，并希望从其他远程主机访问 Docker 远程 API，必须配置防火墙以允许 Docker 端口的传入连接。默认端口为 `2376`（使用 TLS 加密传输时）或 `2375`（未加密时）。

Two common firewall daemons are:

​	常见的两个防火墙守护程序是：

- [Uncomplicated Firewall (ufw)](https://help.ubuntu.com/community/UFW), often used for Ubuntu systems.
  - [简化防火墙 (ufw)](https://help.ubuntu.com/community/UFW)，通常用于 Ubuntu 系统。

- [firewalld](https://firewalld.org/), often used for RPM-based systems.
  - [firewalld](https://firewalld.org/)，通常用于基于 RPM 的系统。


Consult the documentation for your OS and firewall. The following information might help you get started. The settings used in this instruction are permissive, and you may want to use a different configuration that locks your system down more.

​	请查阅您操作系统和防火墙的文档。以下信息可能有助于您开始配置。这些设置较为宽松，您可能希望使用其他配置来增强系统的安全性。

- For ufw, set `DEFAULT_FORWARD_POLICY="ACCEPT"` in your configuration.

  - 对于 ufw，在配置中设置 `DEFAULT_FORWARD_POLICY="ACCEPT"`。

- For firewalld, add rules similar to the following to your policy. One for incoming requests, and one for outgoing requests.

  - 对于 firewalld，在策略中添加类似以下的规则。一条用于传入请求，一条用于传出请求。

  

  ```xml
  <direct>
    [ <rule ipv="ipv6" table="filter" chain="FORWARD_direct" priority="0"> -i zt0 -j ACCEPT </rule> ]
    [ <rule ipv="ipv6" table="filter" chain="FORWARD_direct" priority="0"> -o zt0 -j ACCEPT </rule> ]
  </direct>
  ```

  Make sure that the interface names and chain names are correct.

  ​	请确保接口名称和链名称正确。


## 其他信息 Additional information

For more detailed information on configuration options for remote access to the daemon, refer to the [dockerd CLI reference]({{< ref "/reference/CLIreference/dockerd#bind-docker-to-another-hostport-or-a-unix-socket">}}).

​	有关 Docker 守护进程远程访问的详细配置选项，请参阅 [dockerd CLI 参考]({{< ref "/reference/CLIreference/dockerd#bind-docker-to-another-hostport-or-a-unix-socket">}})。
