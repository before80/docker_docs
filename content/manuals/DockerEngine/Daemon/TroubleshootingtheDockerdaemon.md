+++
title = "Docker守护进程故障排除"
date = 2024-10-23T14:54:40+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/daemon/troubleshoot/](https://docs.docker.com/engine/daemon/troubleshoot/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Troubleshooting the Docker daemon - Docker守护进程故障排除

This page describes how to troubleshoot and debug the daemon if you run into issues.

​	本页面介绍了如何在遇到问题时排查和调试 Docker 守护进程。

You can turn on debugging on the daemon to learn about the runtime activity of the daemon and to aid in troubleshooting. If the daemon is unresponsive, you can also [force a full stack trace](https://docs.docker.com/engine/daemon/logs/#force-a-stack-trace-to-be-logged) of all threads to be added to the daemon log by sending the `SIGUSR` signal to the Docker daemon.

​	您可以启用守护进程的调试模式，以了解守护进程的运行活动，并帮助进行故障排除。如果守护进程没有响应，您还可以通过向 Docker 守护进程发送 `SIGUSR` 信号，强制将所有线程的[完整堆栈跟踪](https://docs.docker.com/engine/daemon/logs/#force-a-stack-trace-to-be-logged)添加到守护进程日志中。

## Daemon

### 无法连接到 Docker 守护进程 Unable to connect to the Docker daemon



```text
Cannot connect to the Docker daemon. Is 'docker daemon' running on this host?
```

This error may indicate:

​	此错误可能表示：

- The Docker daemon isn't running on your system. Start the daemon and try running the command again.

  - Docker 守护进程在您的系统上未运行。启动守护进程并重试。

- Your Docker client is attempting to connect to a Docker daemon on a different host, and that host is unreachable.

  - 您的 Docker 客户端正在尝试连接到不同主机上的 Docker 守护进程，但该主机无法访问。

  

### 检查 Docker 是否正在运行 Check whether Docker is running

The operating-system independent way to check whether Docker is running is to ask Docker, using the `docker info` command.

​	检查 Docker 是否正在运行的跨操作系统方法是使用 `docker info` 命令来询问 Docker。

You can also use operating system utilities, such as `sudo systemctl is-active docker` or `sudo status docker` or `sudo service docker status`, or checking the service status using Windows utilities.

​	您也可以使用操作系统实用程序，如 `sudo systemctl is-active docker`、`sudo status docker` 或 `sudo service docker status`，或在 Windows 上检查服务状态。

Finally, you can check in the process list for the `dockerd` process, using commands like `ps` or `top`.

​	最后，您可以通过 `ps` 或 `top` 等命令检查 `dockerd` 进程在进程列表中的存在。

#### 检查客户端连接的主机 Check which host your client is connecting to

To see which host your client is connecting to, check the value of the `DOCKER_HOST` variable in your environment.

​	要查看您的客户端正在连接的主机，请检查环境中的 `DOCKER_HOST` 变量值：

```console
$ env | grep DOCKER_HOST
```

If this command returns a value, the Docker client is set to connect to a Docker daemon running on that host. If it's unset, the Docker client is set to connect to the Docker daemon running on the local host. If it's set in error, use the following command to unset it:

​	如果此命令返回一个值，则表示 Docker 客户端已设置连接到该主机上运行的 Docker 守护进程。如果没有设置，则 Docker 客户端将连接到本地主机上运行的 Docker 守护进程。如果设置错误，可使用以下命令取消设置：

```console
$ unset DOCKER_HOST
```

You may need to edit your environment in files such as `~/.bashrc` or `~/.profile` to prevent the `DOCKER_HOST` variable from being set erroneously.

​	您可能需要编辑 `~/.bashrc` 或 `~/.profile` 等文件，防止错误地设置 `DOCKER_HOST` 变量。

If `DOCKER_HOST` is set as intended, verify that the Docker daemon is running on the remote host and that a firewall or network outage isn't preventing you from connecting.

​	如果 `DOCKER_HOST` 按预期设置，请确认远程主机上的 Docker 守护进程正在运行，并确保没有防火墙或网络故障阻止连接。

### 解决 `daemon.json` 和启动脚本之间的冲突 Troubleshoot conflicts between the `daemon.json` and startup scripts

If you use a `daemon.json` file and also pass options to the `dockerd` command manually or using start-up scripts, and these options conflict, Docker fails to start with an error such as:

​	如果您使用了 `daemon.json` 文件，同时还手动或使用启动脚本向 `dockerd` 命令传递了选项，并且这些选项冲突，Docker 将无法启动，并显示类似如下的错误：



```text
unable to configure the Docker daemon with file /etc/docker/daemon.json:
the following directives are specified both as a flag and in the configuration
file: hosts: (from flag: [unix:///var/run/docker.sock], from file: [tcp://127.0.0.1:2376])
```

If you see an error similar to this one and you are starting the daemon manually with flags, you may need to adjust your flags or the `daemon.json` to remove the conflict.

​	如果您看到类似的错误并手动使用标志启动守护进程，可能需要调整标志或 `daemon.json` 文件以消除冲突。

> **Note**
>
> 
>
> If you see this specific error message about `hosts`, continue to the [next section](https://docs.docker.com/engine/daemon/troubleshoot/#configure-the-daemon-host-with-systemd) for a workaround.
>
> ​	如果看到关于 `hosts` 的特定错误消息，请继续到[下一节](https://docs.docker.com/engine/daemon/troubleshoot/#configure-the-daemon-host-with-systemd)查找解决方法。

If you are starting Docker using your operating system's init scripts, you may need to override the defaults in these scripts in ways that are specific to the operating system.

​	如果您使用操作系统的 init 脚本启动 Docker，可能需要以特定于操作系统的方式覆盖这些脚本中的默认设置。

#### 使用 systemd 配置守护进程主机 Configure the daemon host with systemd

One notable example of a configuration conflict that's difficult to troubleshoot is when you want to specify a different daemon address from the default. Docker listens on a socket by default. On Debian and Ubuntu systems using `systemd`, this means that a host flag `-H` is always used when starting `dockerd`. If you specify a `hosts` entry in the `daemon.json`, this causes a configuration conflict and results in the Docker daemon failing to start.

​	当您希望指定不同于默认的守护进程地址时，可能会遇到一个难以排查的配置冲突问题。Docker 默认监听套接字。在使用 `systemd` 的 Debian 和 Ubuntu 系统上，这意味着启动 `dockerd` 时总会使用一个 `-H` 主机标志。如果在 `daemon.json` 中指定了 `hosts` 条目，则会导致配置冲突，并导致 Docker 守护进程无法启动。

To work around this problem, create a new file `/etc/systemd/system/docker.service.d/docker.conf` with the following contents, to remove the `-H` argument that's used when starting the daemon by default.

​	为解决此问题，创建一个新文件 `/etc/systemd/system/docker.service.d/docker.conf`，内容如下，以移除默认启动守护进程时使用的 `-H` 参数。

```systemd
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd
```

There are other times when you might need to configure `systemd` with Docker, such as [configuring a HTTP or HTTPS proxy]({{< ref "/manuals/DockerEngine/Daemon/Daemonproxyconfiguration" >}}).

​	在使用 Docker 配置 `systemd` 时，有时还可能需要其他配置，例如[配置 HTTP 或 HTTPS 代理]({{< ref "/manuals/DockerEngine/Daemon/Daemonproxyconfiguration" >}})。

> **Note**
>
> 
>
> If you override this option without specifying a `hosts` entry in the `daemon.json` or a `-H` flag when starting Docker manually, Docker fails to start.
>
> ​	如果在 `daemon.json` 中未指定 `hosts` 条目，或者在手动启动 Docker 时未使用 `-H` 标志，Docker 将无法启动。

Run `sudo systemctl daemon-reload` before attempting to start Docker. If Docker starts successfully, it's now listening on the IP address specified in the `hosts` key of the `daemon.json` instead of a socket.

​	运行 `sudo systemctl daemon-reload` 然后尝试启动 Docker。如果 Docker 成功启动，它现在监听 `daemon.json` 中 `hosts` 键指定的 IP 地址，而不是套接字。

> **Important**
>
> 
>
> Setting `hosts` in the `daemon.json` isn't supported on Docker Desktop for Windows or Docker Desktop for Mac.
>
> ​	在 Docker Desktop for Windows 或 Docker Desktop for Mac 上不支持在 `daemon.json` 中设置 `hosts`。

### 内存不足问题 Out of memory issues

If your containers attempt to use more memory than the system has available, you may experience an Out of Memory (OOM) exception, and a container, or the Docker daemon, might be stopped by the kernel OOM killer. To prevent this from happening, ensure that your application runs on hosts with adequate memory and see [Understand the risks of running out of memory](https://docs.docker.com/engine/containers/resource_constraints/#understand-the-risks-of-running-out-of-memory).

​	如果容器尝试使用超过系统可用内存的资源，可能会遇到内存不足（OOM）异常，容器或 Docker 守护进程可能会被内核的 OOM 杀手停止。为避免这种情况，请确保您的应用程序在具有足够内存的主机上运行，并参阅[了解内存不足风险](https://docs.docker.com/engine/containers/resource_constraints/#understand-the-risks-of-running-out-of-memory)。

### 内核兼容性 Kernel compatibility

Docker can't run correctly if your kernel is older than version 3.10, or if it's missing kernel modules. To check kernel compatibility, you can download and run the [`check-config.sh`](https://raw.githubusercontent.com/docker/docker/master/contrib/check-config.sh) script.

​	如果内核版本低于 3.10 或缺少必要的内核模块，Docker 无法正确运行。要检查内核兼容性，可以下载并运行 [`check-config.sh`](https://raw.githubusercontent.com/docker/docker/master/contrib/check-config.sh) 脚本：

```console
$ curl https://raw.githubusercontent.com/docker/docker/master/contrib/check-config.sh > check-config.sh

$ bash ./check-config.sh
```

The script only works on Linux.

​	该脚本仅适用于 Linux。

### 内核 cgroup 交换限制功能 Kernel cgroup swap limit capabilities

On Ubuntu or Debian hosts, you may see messages similar to the following when working with an image.

​	在 Ubuntu 或 Debian 主机上，您可能会看到类似如下的消息：



```text
WARNING: Your kernel does not support swap limit capabilities. Limitation discarded.
```

If you don't need these capabilities, you can ignore the warning.

​	如果不需要这些功能，可以忽略该警告。

You can turn on these capabilities on Ubuntu or Debian by following these instructions. Memory and swap accounting incur an overhead of about 1% of the total available memory and a 10% overall performance degradation, even when Docker isn't running.

​	可以通过以下步骤在 Ubuntu 或 Debian 上启用这些功能。内存和交换记账会产生约 1% 的总可用内存的开销，且总体性能下降 10%。

1. Log into the Ubuntu or Debian host as a user with `sudo` privileges. 以具有 `sudo` 权限的用户身份登录 Ubuntu 或 Debian 主机。

2. Edit the `/etc/default/grub` file. Add or edit the `GRUB_CMDLINE_LINUX` line to add the following two key-value pairs:

   编辑 `/etc/default/grub` 文件。添加或编辑 `GRUB_CMDLINE_LINUX` 行，添加以下两个键值对：

   ```text
   GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"
   ```

   Save and close the file.

   ​	保存并关闭文件。

3. Update the GRUB boot loader.

   更新 GRUB 引导加载程序：

   ```console
   $ sudo update-grub
   ```

   An error occurs if your GRUB configuration file has incorrect syntax. In this case, repeat steps 2 and 3.

   ​	如果 GRUB 配置文件语法不正确，会出现错误。在这种情况下，请重复步骤 2 和步骤 3。
   
   The changes take effect when you reboot the system.
   
   ​	系统重启后更改生效。

## 网络 Networking

### IP 转发问题 IP forwarding problems

If you manually configure your network using `systemd-network` with systemd version 219 or later, Docker containers may not be able to access your network. Beginning with systemd version 220, the forwarding setting for a given network (`net.ipv4.conf.<interface>.forwarding`) defaults to off. This setting prevents IP forwarding. It also conflicts with Docker's behavior of enabling the `net.ipv4.conf.all.forwarding` setting within containers.

​	如果您使用 systemd 版本 219 或更高版本，并通过 `systemd-network` 手动配置网络，Docker 容器可能无法访问您的网络。从 systemd 版本 220 开始，给定网络的转发设置（`net.ipv4.conf.<interface>.forwarding`）默认为关闭。这会阻止 IP 转发，并与 Docker 在容器内启用 `net.ipv4.conf.all.forwarding` 的行为产生冲突。

To work around this on RHEL, CentOS, or Fedora, edit the `<interface>.network` file in `/usr/lib/systemd/network/` on your Docker host, for example, `/usr/lib/systemd/network/80-container-host0.network`.

​	在 RHEL、CentOS 或 Fedora 系统上，可通过编辑 Docker 主机上的 `/usr/lib/systemd/network/` 中的 `<interface>.network` 文件解决此问题，例如 `/usr/lib/systemd/network/80-container-host0.network`。

Add the following block within the `[Network]` section.

​	在 `[Network]` 部分内添加以下代码块：

```systemd
[Network]
...
IPForward=kernel
# OR
IPForward=true
```

This configuration allows IP forwarding from the container as expected.

​	此配置允许容器正常进行 IP 转发。

### DNS 解析器问题 DNS resolver issues

```console
DNS resolver found in resolv.conf and containers can't use it
```

Linux desktop environments often have a network manager program running, that uses `dnsmasq` to cache DNS requests by adding them to `/etc/resolv.conf`. The `dnsmasq` instance runs on a loopback address such as `127.0.0.1` or `127.0.1.1`. It speeds up DNS look-ups and provides DHCP services. Such a configuration doesn't work within a Docker container. The Docker container uses its own network namespace, and resolves loopback addresses such as `127.0.0.1` to itself, and it's unlikely to be running a DNS server on its own loopback address.

​	Linux 桌面环境中通常有一个网络管理器程序运行，它使用 `dnsmasq` 缓存 DNS 请求并将它们添加到 `/etc/resolv.conf` 中。`dnsmasq` 实例运行在本地回环地址（如 `127.0.0.1` 或 `127.0.1.1`）上，加快了 DNS 查找并提供 DHCP 服务。这种配置在 Docker 容器内无法正常工作，因为容器使用其独立的网络命名空间，并将 `127.0.0.1` 等回环地址解析为其自身，而容器自身不太可能在其回环地址上运行 DNS 服务器。

If Docker detects that no DNS server referenced in `/etc/resolv.conf` is a fully functional DNS server, the following warning occurs:

​	如果 Docker 检测到 `/etc/resolv.conf` 中引用的 DNS 服务器没有一个是完整的 DNS 服务器，会出现以下警告：

```text
WARNING: Local (127.0.0.1) DNS resolver found in resolv.conf and containers
can't use it. Using default external servers : [8.8.8.8 8.8.4.4]
```

If you see this warning, first check to see if you use `dnsmasq`:

​	如果看到此警告，首先检查是否使用了 `dnsmasq`：

```console
$ ps aux | grep dnsmasq
```

If your container needs to resolve hosts which are internal to your network, the public nameservers aren't adequate. You have two choices:

​	如果您的容器需要解析内部网络中的主机，则公共 DNS 服务器不足。您有两个选择：

- Specify DNS servers for Docker to use.

  - 为 Docker 指定 DNS 服务器。

- Turn off `dnsmasq`.

  - 关闭 `dnsmasq`。

  Turning off `dnsmasq` adds the IP addresses of actual DNS nameservers to `/etc/resolv.conf`, and you lose the benefits of `dnsmasq`.

  ​	关闭 `dnsmasq` 会将实际 DNS 服务器的 IP 地址添加到 `/etc/resolv.conf` 中，但会失去 `dnsmasq` 的优势。


You only need to use one of these methods.

​	只需使用其中一种方法即可。

### 为 Docker 指定 DNS 服务器 Specify DNS servers for Docker

The default location of the configuration file is `/etc/docker/daemon.json`. You can change the location of the configuration file using the `--config-file` daemon flag. The following instruction assumes that the location of the configuration file is `/etc/docker/daemon.json`.

​	配置文件的默认位置为 `/etc/docker/daemon.json`。您可以使用 `--config-file` 守护进程标志更改配置文件的位置。以下说明假设配置文件位于 `/etc/docker/daemon.json`。

1. Create or edit the Docker daemon configuration file, which defaults to `/etc/docker/daemon.json` file, which controls the Docker daemon configuration.

   配置文件的默认位置为 `/etc/docker/daemon.json`。您可以使用 `--config-file` 守护进程标志更改配置文件的位置。以下说明假设配置文件位于 `/etc/docker/daemon.json`。

   ```console
   $ sudo nano /etc/docker/daemon.json
   ```

2. Add a `dns` key with one or more DNS server IP addresses as values.

   添加 `dns` 键，并为其提供一个或多个 DNS 服务器 IP 地址。

   ```json
   {
     "dns": ["8.8.8.8", "8.8.4.4"]
   }
   ```

   If the file has existing contents, you only need to add or edit the `dns` line. If your internal DNS server can't resolve public IP addresses, include at least one DNS server that can. Doing so allows you to connect to Docker Hub, and your containers to resolve internet domain names.

   ​	如果文件已有内容，仅需添加或编辑 `dns` 行。如果内部 DNS 服务器无法解析公共 IP 地址，请包含至少一个可以解析的 DNS 服务器，这样可以连接到 Docker Hub 并允许容器解析互联网域名。

   Save and close the file.

   ​	保存并关闭文件。

3. Restart the Docker daemon.

   重启 Docker 守护进程。

   ```console
   $ sudo service docker restart
   ```

4. Verify that Docker can resolve external IP addresses by trying to pull an image:

   验证 Docker 是否能解析外部 IP 地址，尝试拉取一个镜像：

   ```console
   $ docker pull hello-world
   ```

5. If necessary, verify that Docker containers can resolve an internal hostname by pinging it.

   如果需要，通过 ping 一个内部主机名来验证 Docker 容器是否可以解析内部主机名。

   ```console
   $ docker run --rm -it alpine ping -c4 <my_internal_host>
   
   PING google.com (192.168.1.2): 56 data bytes
   64 bytes from 192.168.1.2: seq=0 ttl=41 time=7.597 ms
   64 bytes from 192.168.1.2: seq=1 ttl=41 time=7.635 ms
   64 bytes from 192.168.1.2: seq=2 ttl=41 time=7.660 ms
   64 bytes from 192.168.1.2: seq=3 ttl=41 time=7.677 ms
   ```

### 关闭 `dnsmasq` - Turn off `dnsmasq`

{{< tabpane text=true persist=disabled >}}

{{% tab header="Ubuntu" %}}

If you prefer not to change the Docker daemon's configuration to use a specific IP address, follow these instructions to turn off `dnsmasq` in NetworkManager.

​	如果您不想更改 Docker 守护进程的配置以使用特定的 IP 地址，可通过以下步骤在 NetworkManager 中关闭 `dnsmasq`：

1. Edit the `/etc/NetworkManager/NetworkManager.conf` file. 编辑 `/etc/NetworkManager/NetworkManager.conf` 文件。

2. Comment out the `dns=dnsmasq` line by adding a `#` character to the beginning of the line. 在 `dns=dnsmasq` 行前添加 `#` 注释符：

   

   ```text
   # dns=dnsmasq
   ```

   Save and close the file.

   ​	保存并关闭文件。

3. Restart both NetworkManager and Docker. As an alternative, you can reboot your system.

   重启 NetworkManager 和 Docker，或选择重启系统。

   ```console
   $ sudo systemctl restart network-manager
   $ sudo systemctl restart docker
   ```

{{% /tab  %}}

{{% tab header="RHEL, CentOS, or Fedora" %}}

To turn off `dnsmasq` on RHEL, CentOS, or Fedora:

​	要在 RHEL、CentOS 或 Fedora 上关闭 `dnsmasq`：

1. Turn off the `dnsmasq` service:

   关闭 `dnsmasq` 服务：

   ```console
   $ sudo systemctl stop dnsmasq
   $ sudo systemctl disable dnsmasq
   ```

2. Configure the DNS servers manually using the [Red Hat documentation](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/configuring_and_managing_networking/configuring-the-order-of-dns-servers_configuring-and-managing-networking). 使用 [Red Hat 文档](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/configuring_and_managing_networking/configuring-the-order-of-dns-servers_configuring-and-managing-networking) 手动配置 DNS 服务器。

{{% /tab  %}}

{{< /tabpane >}}



------

### Docker 网络消失 Docker networks disappearing

If a Docker network, such as the `docker0` bridge or a custom network, randomly disappears or otherwise appears to be working incorrectly, it could be because another service is interfering with or modifying Docker interfaces. Tools that manage networking interfaces on the host are known to sometimes also inappropriately modify Docker interfaces.

​	如果 Docker 网络（例如 `docker0` 网桥或自定义网络）随机消失或运行不正常，可能是其他服务干扰或修改了 Docker 接口。主机上管理网络接口的工具有时会不适当地修改 Docker 接口。

Refer to the following sections for instructions on how to configure your network manager to set Docker interfaces as un-managed, depending on the network management tools that exist on the host:

​	请参考以下部分，根据主机上现有的网络管理工具设置网络管理器将 Docker 接口设为不受管理：

- If `netscript` is installed, consider [uninstalling it](https://docs.docker.com/engine/daemon/troubleshoot/#uninstall-netscript)

- - 如果安装了 `netscript`，请考虑[卸载它](https://docs.docker.com/engine/daemon/troubleshoot/#uninstall-netscript)

- Configure the network manager to [treat Docker interfaces as un-managed](https://docs.docker.com/engine/daemon/troubleshoot/#un-manage-docker-interfaces)
  - 配置网络管理器以[将 Docker 接口视为不受管理](https://docs.docker.com/engine/daemon/troubleshoot/#un-manage-docker-interfaces)

- If you're using Netplan, you may need to [apply a custom Netplan configuration](https://docs.docker.com/engine/daemon/troubleshoot/#prevent-netplan-from-overriding-network-configuration)
  - 如果使用 Netplan，可能需要[应用自定义 Netplan 配置](https://docs.docker.com/engine/daemon/troubleshoot/#prevent-netplan-from-overriding-network-configuration)

#### Uninstall `netscript`

If `netscript` is installed on your system, you can likely fix this issue by uninstalling it. For example, on a Debian-based system:

​	如果系统安装了 `netscript`，可以通过卸载它来解决此问题。例如，在基于 Debian 的系统上：



```console
$ sudo apt-get remove netscript-2.4
```

#### 将 Docker 接口设为不受管理 Un-manage Docker interfaces

In some cases, the network manager will attempt to manage Docker interfaces by default. You can try to explicitly flag Docker networks as un-managed by editing your system's network configuration settings.

​	在某些情况下，网络管理器默认会尝试管理 Docker 接口。您可以通过编辑系统的网络配置显式将 Docker 网络标记为不受管理。

{{< tabpane text=true persist=disabled >}}

{{% tab header="NetworkManager" %}}

If you're using `NetworkManager`, edit your system network configuration under `/etc/network/interfaces`

​	如果使用的是 `NetworkManager`，请编辑 `/etc/network/interfaces` 中的系统网络配置。

1. Create a file at `/etc/network/interfaces.d/20-docker0` with the following contents:

   在 `/etc/network/interfaces.d/20-docker0` 中创建文件，并添加以下内容：

   ```text
   iface docker0 inet manual
   ```

   Note that this example configuration only "un-manages" the default `docker0` bridge, not custom networks.

   ​	注意，此示例配置仅将默认的 `docker0` 网桥设为不受管理，而不影响自定义网络。

2. Restart `NetworkManager` for the configuration change to take effect.

   重启 `NetworkManager` 使配置更改生效：

   ```console
   $ systemctl restart NetworkManager
   ```

3. Verify that the `docker0` interface has the `unmanaged` state.

   验证 `docker0` 接口是否处于 `unmanaged` 状态：

   ```console
   $ nmcli device
   ```

{{% /tab  %}}

{{% tab header="systemd-networkd" %}}

If you're running Docker on a system using `systemd-networkd` as a networking daemon, configure the Docker interfaces as un-managed by creating configuration files under `/etc/systemd/network`:

​	如果您在使用 `systemd-networkd` 作为网络守护进程的系统上运行 Docker，可在 `/etc/systemd/network` 下创建配置文件，将 Docker 接口设为不受管理：

1. Create `/etc/systemd/network/docker.network` with the following contents:

   创建 `/etc/systemd/network/docker.network` 文件，并添加以下内容：

   ```ini
   # Ensure that the Docker interfaces are un-managed
   # 确保 Docker 接口不受管理
   
   [Match]
   Name=docker0 br-* veth*
   
   [Link]
   Unmanaged=yes
   ```

2. Reload the configuration.

   重新加载配置：

   ```console
   $ sudo systemctl restart systemd-networkd
   ```

3. Restart the Docker daemon.

   重启 Docker 守护进程：

   ```console
   $ sudo systemctl restart docker
   ```

4. Verify that the Docker interfaces have the `unmanaged` state.

   验证 Docker 接口是否处于 `unmanaged` 状态：

   ```console
   $ networkctl
   ```

{{% /tab  %}}

{{< /tabpane >}}



------

### 防止 Netplan 覆盖网络配置 Prevent Netplan from overriding network configuration

On systems that use [Netplan](https://netplan.io/) through [`cloud-init`](https://cloudinit.readthedocs.io/en/latest/index.html), you may need to apply a custom configuration to prevent `netplan` from overriding the network manager configuration:

​	在通过 [`cloud-init`](https://cloudinit.readthedocs.io/en/latest/index.html) 使用 [Netplan](https://netplan.io/) 的系统中，可能需要应用自定义配置以防止 `netplan` 覆盖网络管理器的配置：

1. Follow the steps in [Un-manage Docker interfaces](https://docs.docker.com/engine/daemon/troubleshoot/#un-manage-docker-interfaces) for creating the network manager configuration. 按照[将 Docker 接口设为不受管理](https://docs.docker.com/engine/daemon/troubleshoot/#un-manage-docker-interfaces)中的步骤创建网络管理器配置。

2. Create a `netplan` configuration file under `/etc/netplan/50-cloud-init.yml`. 在 `/etc/netplan/50-cloud-init.yml` 下创建 `netplan` 配置文件。

   The following example configuration file is a starting point. Adjust it to match the interfaces you want to un-manage. Incorrect configuration can lead to network connectivity issues.

   ​	以下示例配置文件为起点，根据要设为不受管理的接口进行调整。错误配置可能导致网络连接问题。

   /etc/netplan/50-cloud-init.yml

   

   ```yaml
   network:
     ethernets:
       all:
         dhcp4: true
         dhcp6: true
         match:
           # edit this filter to match whatever makes sense for your system
           # 根据系统编辑此筛选器
           name: en*
     renderer: networkd
     version: 2
   ```

3. Apply the new Netplan configuration.

   应用新的 Netplan 配置：

   ```console
   $ sudo netplan apply
   ```

4. Restart the Docker daemon:

   重启 Docker 守护进程：

   ```console
   $ sudo systemctl restart docker
   ```

5. Verify that the Docker interfaces have the `unmanaged` state.

   验证 Docker 接口是否处于 `unmanaged` 状态：

   ```console
   $ networkctl
   ```

## 卷 Volumes

### 无法移除文件系统 Unable to remove filesystem



```text
Error: Unable to remove filesystem
```

Some container-based utilities, such as [Google cAdvisor](https://github.com/google/cadvisor), mount Docker system directories, such as `/var/lib/docker/`, into a container. For instance, the documentation for `cadvisor` instructs you to run the `cadvisor` container as follows:

​	一些基于容器的工具（如 [Google cAdvisor](https://github.com/google/cadvisor)）会将 Docker 系统目录（如 `/var/lib/docker/`）挂载到容器中。例如，`cAdvisor` 的文档指示按以下方式运行 `cAdvisor` 容器：



```console
$ sudo docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  google/cadvisor:latest
```

When you bind-mount `/var/lib/docker/`, this effectively mounts all resources of all other running containers as filesystems within the container which mounts `/var/lib/docker/`. When you attempt to remove any of these containers, the removal attempt may fail with an error like the following:

​	当绑定挂载 `/var/lib/docker/` 时，实际上会将所有其他运行容器的资源作为文件系统挂载到该容器内。如果尝试移除这些容器，可能会因以下错误而失败：



```none
Error: Unable to remove filesystem for
74bef250361c7817bee19349c93139621b272bc8f654ae112dd4eb9652af9515:
remove /var/lib/docker/containers/74bef250361c7817bee19349c93139621b272bc8f654ae112dd4eb9652af9515/shm:
Device or resource busy
```

The problem occurs if the container which bind-mounts `/var/lib/docker/` uses `statfs` or `fstatfs` on filesystem handles within `/var/lib/docker/` and does not close them.

​	如果绑定挂载 `/var/lib/docker/` 的容器使用 `statfs` 或 `fstatfs` 访问 `/var/lib/docker/` 内的文件系统句柄并未关闭它们，就会出现此问题。

Typically, we would advise against bind-mounting `/var/lib/docker` in this way. However, `cAdvisor` requires this bind-mount for core functionality.

​	通常，建议避免以这种方式绑定挂载 `/var/lib/docker`。然而，`cAdvisor` 出于核心功能的需求需要这样做。

If you are unsure which process is causing the path mentioned in the error to be busy and preventing it from being removed, you can use the `lsof` command to find its process. For instance, for the error above:

​	如果不确定哪个进程导致路径被占用并阻止其删除，可以使用 `lsof` 命令找到进程。例如，对于上面的错误：



```console
$ sudo lsof /var/lib/docker/containers/74bef250361c7817bee19349c93139621b272bc8f654ae112dd4eb9652af9515/shm
```

To work around this problem, stop the container which bind-mounts `/var/lib/docker` and try again to remove the other container.

​	要解决此问题，请停止绑定挂载 `/var/lib/docker` 的容器，然后重试删除其他容器。
