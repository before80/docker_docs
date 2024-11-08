+++
title = "dockerd"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/dockerd/](https://docs.docker.com/reference/cli/dockerd/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# dockerd

# daemon



```markdown
Usage: dockerd [OPTIONS]

A self-sufficient runtime for containers.
一个独立的容器运行时。

Options:
      --add-runtime runtime                   注册一个额外的 OCI 兼容运行时（默认为 []） Register an additional OCI compatible runtime (default [])
      --allow-nondistributable-artifacts list 允许向注册表推送不可分发的工件 Allow push of nondistributable artifacts to registry
      --api-cors-header string                设置引擎 API 中的 CORS 头 Set CORS headers in the Engine API
      --authorization-plugin list             加载授权插件 Authorization plugins to load
      --bip string                            指定网络桥接 IP Specify network bridge IP
  -b, --bridge string                         将容器连接到网络桥接 Attach containers to a network bridge
      --cdi-spec-dir list                     要使用的 CDI 规范目录 CDI specification directories to use
      --cgroup-parent string                  为所有容器设置父 cgroup - Set parent cgroup for all containers
      --config-file string                    守护进程配置文件（默认为 "/etc/docker/daemon.json"） Daemon configuration file (default "/etc/docker/daemon.json")
      --containerd string                     要使用的 containerd 命名空间（默认为 "moby"） containerd grpc 地址 containerd grpc address
      --containerd-namespace string           插件的 containerd 命名空间（默认为 "plugins.moby"） Containerd namespace to use (default "moby")
      --containerd-plugins-namespace string   限制父 cgroup 的 CPU 实时周期，以微秒为单位（不支持 cgroups v2） Containerd namespace to use for plugins (default "plugins.moby")
      --cpu-rt-period int                     Limit the CPU real-time period in microseconds for the
                                              parent cgroup for all containers (not supported with cgroups v2)
                                              限制父 cgroup 的 CPU 实时周期，以微秒为单位（不支持 cgroups v2）
                                              
      --cpu-rt-runtime int                    Limit the CPU real-time runtime in microseconds for the
                                              parent cgroup for all containers (not supported with cgroups v2)
                                              限制父 cgroup 的 CPU 实时运行时，以微秒为单位（不支持 cgroups v2）
                                              
      --cri-containerd                        启动 containerd 并启用 cri - start containerd with cri
      --data-root string                      Docker 持久状态的根目录（默认为 "/var/lib/docker"） Root directory of persistent Docker state (default "/var/lib/docker")
  -D, --debug                                 启用调试模式 Enable debug mode
      --default-address-pool pool-options     节点特定本地网络的默认地址池 Default address pools for node specific local networks
      --default-cgroupns-mode string          容器 cgroup 命名空间的默认模式（"host" | "private"，默认为 "private"） Default mode for containers cgroup namespace ("host" | "private") (default "private")
      --default-gateway ip                    容器的默认网关 IPv4 地址 Container default gateway IPv4 address
      --default-gateway-v6 ip                 容器的默认网关 IPv6 地址 Container default gateway IPv6 address
      --default-ipc-mode string               容器 ipc 的默认模式（"shareable" | "private"，默认为 "private"） Default mode for containers ipc ("shareable" | "private") (default "private")
      --default-network-opt mapmap            默认网络选项（默认为 map[]） Default network options (default map[])
      --default-runtime string                容器的默认 OCI 运行时（默认为 "runc"） Default OCI runtime for containers (default "runc")
      --default-shm-size bytes                容器的默认共享内存大小（默认为 64MiB） Default shm size for containers (default 64MiB)
      --default-ulimit ulimit                 容器的默认 ulimits（默认为 []） Default ulimits for containers (default [])
      --dns list                              使用的 DNS 服务器 DNS server to use
      --dns-opt list                          使用的 DNS 选项 DNS options to use
      --dns-search list                       使用的 DNS 搜索域 DNS search domains to use
      --exec-opt list                         运行时执行选项 Runtime execution options
      --exec-root string                      执行状态文件的根目录（默认为 "/var/run/docker"） Root directory for execution state files (default "/var/run/docker")
      --experimental                          启用实验功能 Enable experimental features
      --feature map                           启用守护进程中的功能 Enable feature in the daemon
      --fixed-cidr string                     固定 IP 的 IPv4 子网 IPv4 subnet for fixed IPs
      --fixed-cidr-v6 string                  固定 IP 的 IPv6 子网 IPv6 subnet for fixed IPs
  -G, --group string                          unix 套接字的组（默认为 "docker"） Group for the unix socket (default "docker")
      --help                                  打印使用方法 Print usage
  -H, --host list                             要连接的守护进程套接字 Daemon socket(s) to connect to
      --host-gateway-ip ip                    IP address that the special 'host-gateway' string in --add-host resolves to.
                                              Defaults to the IP address of the default bridge
                                              `--add-host` 解析的特定 'host-gateway' 字符串的 IP 地址。
                                              默认为默认桥接的 IP 地址
                                             
      --http-proxy string                     出站流量使用的 HTTP 代理 URL - HTTP proxy URL to use for outgoing traffic
      --https-proxy string                    出站流量使用的 HTTPS 代理 URL - HTTPS proxy URL to use for outgoing traffic
      --icc                                   启用容器间通信（默认为 true） Enable inter-container communication (default true)
      --init                                  在容器中运行 init 以转发信号并清理进程 Run an init in the container to forward signals and reap processes
      --init-path string                      docker-init 二进制文件的路径 Path to the docker-init binary
      --insecure-registry list                启用不安全的注册表通信 Enable insecure registry communication
      --ip ip                                 绑定容器端口时的默认 IP（默认为 0.0.0.0） Default IP when binding container ports (default 0.0.0.0)
      --ip-forward                            启用 net.ipv4.ip_forward（默认为 true） Enable net.ipv4.ip_forward (default true)
      --ip-masq                               启用 IP 假面（默认为 true） Enable IP masquerading (default true)
      --ip6tables                             启用 ip6tables 规则的添加（实验性） Enable addition of ip6tables rules (experimental)
      --iptables                              启用 iptables 规则的添加（默认为 true） Enable addition of iptables rules (default true)
      --ipv6                                  启用 IPv6 网络 Enable IPv6 networking
      --label list                            为守护进程设置 key=value 标签 Set key=value labels to the daemon
      --live-restore                          启用 Docker 的实时恢复，以便容器仍在运行时恢复 Enable live restore of docker when containers are still running
      --log-driver string                     容器日志的默认驱动程序（默认为 "json-file"） Default driver for container logs (default "json-file")
      --log-format string                     设置日志格式（"text"|"json"，默认为 "text"） Set the logging format ("text"|"json") (default "text")
  -l, --log-level string                      设置日志级别（"debug"|"info"|"warn"|"error"|"fatal"，默认为 "info"） Set the logging level ("debug"|"info"|"warn"|"error"|"fatal") (default "info")
      --log-opt map                           容器的默认日志驱动选项（默认为 map[]） Default log driver options for containers (default map[])
      --max-concurrent-downloads int          设置最大并发下载数（默认为 3） Set the max concurrent downloads (default 3)
      --max-concurrent-uploads int            设置最大并发上传数（默认为 5） Set the max concurrent uploads (default 5)
      --max-download-attempts int             设置每次拉取的最大下载尝试次数（默认为 5） Set the max download attempts for each pull (default 5)
      --metrics-addr string                   设置用于服务指标 API 的默认地址和端口 Set default address and port to serve the metrics api on
      --mtu int                               设置容器网络的 MTU（默认为 1500） Set the containers network MTU (default 1500)
      --network-control-plane-mtu int         网络控制平面 MTU（默认为 1500） Network Control plane MTU (default 1500)
      --no-new-privileges                     默认为新容器设置 no-new-privileges - Set no-new-privileges by default for new containers
      --no-proxy string                       用于跳过代理的主机或 IP 地址的逗号分隔列表 Comma-separated list of hosts or IP addresses for which the proxy is skipped
      --node-generic-resource list            广告用户定义的资源 Advertise user-defined resource
      --oom-score-adjust int                  设置守护进程的 oom_score_adj Set the oom_score_adj for the daemon
  -p, --pidfile string                        用于守护进程 PID 文件的路径（默认为 "/var/run/docker.pid"） Path to use for daemon PID file (default "/var/run/docker.pid")
      --raw-logs                              完整的时间戳，无 ANSI 颜色 Full timestamps without ANSI coloring
      --registry-mirror list                  首选注册表镜像 Preferred registry mirror
      --rootless                              启用无根模式；通常与 RootlessKit 一起使用 Enable rootless mode; typically used with RootlessKit
      --seccomp-profile string                seccomp 配置文件的路径。使用 "unconfined" 禁用默认 seccomp 配置文件（默认为 "builtin"）Path to seccomp profile. Use "unconfined" to disable the default seccomp profile (default "builtin")
      --selinux-enabled                       启用 selinux 支持 Enable selinux support
      --shutdown-timeout int                  设置默认关机超时时间（默认为 15） Set the default shutdown timeout (default 15)
  -s, --storage-driver string                 要使用的存储驱动程序 Storage driver to use
      --storage-opt list                      存储驱动选项 Storage driver options
      --swarm-default-advertise-addr string   设置 swarm 广告地址的默认地址或接口 Set default address or interface for swarm advertised address
      --tls                                   使用 TLS；由 --tlsverify 隐含 Use TLS; implied by --tlsverify
      --tlscacert string                      信任仅由此 CA 签署的证书（默认为 "~/.docker/ca.pem"） Trust certs signed only by this CA (default "~/.docker/ca.pem")
      --tlscert string                        TLS 证书文件的路径（默认为 "~/.docker/cert.pem"） Path to TLS certificate file (default "~/.docker/cert.pem")
      --tlskey string                         TLS 密钥文件的路径（默认为 "~/.docker/key.pem"） Path to TLS key file (default "~/.docker/key.pem")
      --tlsverify                             使用 TLS 并验证远程 Use TLS and verify the remote
      --userland-proxy                        对环回流量使用用户代理（默认为 true） Use userland proxy for loopback traffic (default true)
      --userland-proxy-path string            用户代理二进制文件的路径 Path to the userland proxy binary
      --userns-remap string                   用户命名空间的用户/组设置 User/Group setting for user namespaces
      --validate                              验证守护进程配置并退出 Validate daemon configuration and exit
  -v, --version                               打印版本信息并退出 Print version information and quit
```

Options with [] may be specified multiple times.

​	包含 `[]` 的选项可以多次指定。

## 描述 Description

`dockerd` is the persistent process that manages containers. Docker uses different binaries for the daemon and client. To run the daemon you type `dockerd`.

​	`dockerd` 是管理容器的持久进程。Docker 使用不同的二进制文件来分别运行守护进程和客户端。要运行守护进程，请键入 `dockerd`。

To run the daemon with debug output, use `dockerd --debug` or add `"debug": true` to [the `daemon.json` file](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file).

​	要使用调试输出运行守护进程，请使用 `dockerd --debug` 或在 [`daemon.json` 文件](#daemon-configuration-file)中添加 `"debug": true`。

> **Note**
>
> **Enabling experimental features**
>
> Enable experimental features by starting `dockerd` with the `--experimental` flag or adding `"experimental": true` to the `daemon.json` file.
>
> ​	启用实验性功能可通过启动 `dockerd` 并添加 `--experimental` 标志，或在 `daemon.json` 文件中添加 `"experimental": true`。

### 环境变量 Environment variables

The following list of environment variables are supported by the `dockerd` daemon. Some of these environment variables are supported both by the Docker Daemon and the `docker` CLI. Refer to [Environment variables](https://docs.docker.com/reference/cli/docker/#environment-variables) to learn about environment variables supported by the `docker` CLI.

​	以下是 `dockerd` 守护进程支持的环境变量列表，其中一些环境变量也由 Docker 守护进程和 `docker` CLI 支持。有关 `docker` CLI 支持的环境变量，参阅 [环境变量]({{< ref "/reference/CLIreference/docker#environment-variables">}})。

| Variable            | Description                                                  |
| :------------------ | :----------------------------------------------------------- |
| `DOCKER_CERT_PATH`  | 认证密钥的位置。此变量同时用于 [`docker` CLI]({{< ref "/reference/CLIreference/docker" >}}) 和 `dockerd` 守护进程。 Location of your authentication keys. This variable is used both by the [`docker` CLI]({{< ref "/reference/CLIreference/docker" >}}) and the `dockerd` daemon. |
| `DOCKER_DRIVER`     | 要使用的存储驱动。The storage driver to use.                 |
| `DOCKER_RAMDISK`    | 如果设置则禁用 `pivot_root`。If set this disables `pivot_root`. |
| `DOCKER_TLS_VERIFY` | 设置后 Docker 使用 TLS 并验证远程。此变量同时用于 [`docker` CLI]({{< ref "/reference/CLIreference/docker" >}}) 和 `dockerd` 守护进程。When set Docker uses TLS and verifies the remote. This variable is used both by the [`docker` CLI]({{< ref "/reference/CLIreference/docker" >}}) and the `dockerd` daemon. |
| `DOCKER_TMPDIR`     | 守护进程创建的临时文件的位置。Location for temporary files created by the daemon. |
| `HTTP_PROXY`        | HTTP 请求的代理 URL，除非被 NoProxy 覆盖。详情参见 [Go 规范](https://pkg.go.dev/golang.org/x/net/http/httpproxy#Config)。Proxy URL for HTTP requests unless overridden by NoProxy. See the [Go specification](https://pkg.go.dev/golang.org/x/net/http/httpproxy#Config) for details. |
| `HTTPS_PROXY`       | HTTPS 请求的代理 URL，除非被 NoProxy 覆盖。详情参见 [Go 规范](https://pkg.go.dev/golang.org/x/net/http/httpproxy#Config)。Proxy URL for HTTPS requests unless overridden by NoProxy. See the [Go specification](https://pkg.go.dev/golang.org/x/net/http/httpproxy#Config) for details. |
| `MOBY_DISABLE_PIGZ` | 禁用 `unpigz` 来并行解压图层，即使已安装。Disables the use of [`unpigz`](https://linux.die.net/man/1/pigz) to decompress layers in parallel when pulling images, even if it is installed. |
| `NO_PROXY`          | 用逗号分隔的指定主机，不应使用代理。详情参见 [Go 规范](https://pkg.go.dev/golang.org/x/net/http/httpproxy#Config)。Comma-separated values specifying hosts that should be excluded from proxying. See the [Go specification](https://pkg.go.dev/golang.org/x/net/http/httpproxy#Config) for details. |

## Examples

### Proxy configuration

> **Note**
>
> Refer to the [Docker Desktop manual](https://docs.docker.com/desktop/networking/#httphttps-proxy-support) if you are running [Docker Desktop]({{< ref "/manuals/DockerDesktop" >}}).
>
> ​	如果您使用的是 [Docker Desktop]({{< ref "/manuals/DockerDesktop" >}})，请参考 [Docker Desktop 手册]({{< ref "/manuals/DockerDesktop/ExplorenetworkingfeaturesonDockerDesktop#httphttps-代理支持-httphttps-proxy-support">}})。

If you are behind an HTTP proxy server, for example in corporate settings, you may have to configure the Docker daemon to use the proxy server for operations such as pulling and pushing images. The daemon can be configured in three ways:

​	如果您处在 HTTP 代理服务器后，例如在公司环境中，可能需要配置 Docker 守护进程以通过代理服务器进行镜像拉取和推送操作。守护进程可以通过以下三种方式进行配置：

1. Using environment variables (`HTTP_PROXY`, `HTTPS_PROXY`, and `NO_PROXY`). 使用环境变量 (`HTTP_PROXY`、`HTTPS_PROXY` 和 `NO_PROXY`)。
2. Using the `http-proxy`, `https-proxy`, and `no-proxy` fields in the [daemon configuration file](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file) (Docker Engine version 23.0 or later). 在 [守护进程配置文件]({{< ref "/reference/CLIreference/dockerd#daemon-configuration-file">}})中使用 `http-proxy`、`https-proxy` 和 `no-proxy` 字段（Docker 引擎版本 23.0 或更高）。
3. Using the `--http-proxy`, `--https-proxy`, and `--no-proxy` command-line options. (Docker Engine version 23.0 or later). 使用命令行选项 `--http-proxy`、`--https-proxy` 和 `--no-proxy`（Docker 引擎版本 23.0 或更高）。

The command-line and configuration file options take precedence over environment variables. Refer to [control and configure Docker with systemd]({{< ref "/manuals/DockerEngine/Daemon/Daemonproxyconfiguration" >}}) to set these environment variables on a host using `systemd`.

​	命令行和配置文件选项优先于环境变量。有关在使用 `systemd` 的主机上设置这些环境变量的信息，请参阅 [使用 systemd 控制和配置 Docker]({{< ref "/manuals/DockerEngine/Daemon/Daemonproxyconfiguration" >}})。

### 守护进程套接字选项 Daemon socket option

The Docker daemon can listen for [Docker Engine API](https://docs.docker.com/engine/api/) requests via three different types of Socket: `unix`, `tcp`, and `fd`.

​	Docker 守护进程可以通过三种不同类型的套接字监听 [Docker Engine API](https://docs.docker.com/engine/api/) 请求：`unix`、`tcp` 和 `fd`。

By default, a `unix` domain socket (or IPC socket) is created at `/var/run/docker.sock`, requiring either `root` permission, or `docker` group membership.

​	默认情况下，会创建一个 `unix` 域套接字（或 IPC 套接字）在 `/var/run/docker.sock`，这需要 `root` 权限或 `docker` 组成员权限。

If you need to access the Docker daemon remotely, you need to enable the tcp Socket. When using a TCP socket, the Docker daemon provides un-encrypted and un-authenticated direct access to the Docker daemon by default. You should secure the daemon either using the [built in HTTPS encrypted socket]({{< ref "/manuals/DockerEngine/Security/ProtecttheDockerdaemonsocket" >}}), or by putting a secure web proxy in front of it. You can listen on port `2375` on all network interfaces with `-H tcp://0.0.0.0:2375`, or on a particular network interface using its IP address: `-H tcp://192.168.59.103:2375`. It is conventional to use port `2375` for un-encrypted, and port `2376` for encrypted communication with the daemon.

​	如果需要远程访问 Docker 守护进程，则需要启用 TCP 套接字。当使用 TCP 套接字时，Docker 守护进程默认提供未加密和未经验证的直接访问。应通过使用 [内置的 HTTPS 加密套接字]({{< ref "/manuals/DockerEngine/Security/ProtecttheDockerdaemonsocket" >}}) 或在其前方设置一个安全的 Web 代理来保护守护进程。可以通过 `-H tcp://0.0.0.0:2375` 在所有网络接口上监听端口 `2375`，也可以使用特定网络接口的 IP 地址，如：`-H tcp://192.168.59.103:2375`。通常端口 `2375` 用于未加密通信，端口 `2376` 用于加密通信。

> **Note**
>
> If you're using an HTTPS encrypted socket, keep in mind that only TLS version 1.0 and higher is supported. Protocols SSLv3 and below are not supported for security reasons.
>
> ​	如果使用 HTTPS 加密套接字，请注意仅支持 TLS 1.0 及以上版本。由于安全原因，SSLv3 及以下版本不受支持。

On systemd based systems, you can communicate with the daemon via [systemd socket activation](https://0pointer.de/blog/projects/socket-activation.html), with `dockerd -H fd://`. Using `fd://` works for most setups, but you can also specify individual sockets: `dockerd -H fd://3`. If the specified socket activated files aren't found, the daemon exits. You can find examples of using systemd socket activation with Docker and systemd in the [Docker source tree](https://github.com/docker/docker/tree/master/contrib/init/systemd/).

​	在基于 systemd 的系统上，可以通过 [systemd 套接字激活](https://0pointer.de/blog/projects/socket-activation.html)与守护进程通信，使用 `dockerd -H fd://`。`fd://` 适用于大多数设置，也可以指定个别套接字：`dockerd -H fd://3`。如果找不到指定的套接字文件，守护进程会退出。可以在 [Docker 源代码树](https://github.com/docker/docker/tree/master/contrib/init/systemd/) 中找到使用 Docker 和 systemd 进行 systemd 套接字激活的示例。

You can configure the Docker daemon to listen to multiple sockets at the same time using multiple `-H` options:

​	可以使用多个 `-H` 选项配置 Docker 守护进程同时监听多个套接字：

The example below runs the daemon listening on the default Unix socket, and on 2 specific IP addresses on this host:

​	以下示例展示了守护进程在默认的 Unix 套接字和此主机上的两个特定 IP 地址上监听：

```console
$ sudo dockerd -H unix:///var/run/docker.sock -H tcp://192.168.59.106 -H tcp://10.10.10.2
```

The Docker client honors the `DOCKER_HOST` environment variable to set the `-H` flag for the client. Use **one** of the following commands:

​	Docker 客户端会使用 `DOCKER_HOST` 环境变量为客户端设置 `-H` 标志。使用以下任意一条命令：



```console
$ docker -H tcp://0.0.0.0:2375 ps
```



```console
$ export DOCKER_HOST="tcp://0.0.0.0:2375"

$ docker ps
```

Setting the `DOCKER_TLS_VERIFY` environment variable to any value other than the empty string is equivalent to setting the `--tlsverify` flag. The following are equivalent:

​	将 `DOCKER_TLS_VERIFY` 环境变量设置为非空值相当于设置 `--tlsverify` 标志。以下命令等效：



```console
$ docker --tlsverify ps
# or
$ export DOCKER_TLS_VERIFY=1
$ docker ps
```

The Docker client honors the `HTTP_PROXY`, `HTTPS_PROXY`, and `NO_PROXY` environment variables (or the lowercase versions thereof). `HTTPS_PROXY` takes precedence over `HTTP_PROXY`.

​	Docker 客户端支持 `HTTP_PROXY`、`HTTPS_PROXY` 和 `NO_PROXY` 环境变量（或其小写版本）。`HTTPS_PROXY` 优先于 `HTTP_PROXY`。

The Docker client supports connecting to a remote daemon via SSH:

​	Docker 客户端支持通过 SSH 连接到远程守护进程：



```console
$ docker -H ssh://me@example.com:22/var/run/docker.sock ps
$ docker -H ssh://me@example.com:22 ps
$ docker -H ssh://me@example.com ps
$ docker -H ssh://example.com ps
```

To use SSH connection, you need to set up `ssh` so that it can reach the remote host with public key authentication. Password authentication is not supported. If your key is protected with passphrase, you need to set up `ssh-agent`.

​	要使用 SSH 连接，需要设置 `ssh` 以便可以通过公钥身份验证访问远程主机。不支持密码身份验证。如果密钥有密码保护，需要设置 `ssh-agent`。

#### 将 Docker 绑定到另一个主机/端口或 Unix 套接字 Bind Docker to another host/port or a Unix socket

> **Warning**
>
> Changing the default `docker` daemon binding to a TCP port or Unix `docker` user group introduces security risks, as it may allow non-root users to gain root access on the host. Make sure you control access to `docker`. If you are binding to a TCP port, anyone with access to that port has full Docker access; so it's not advisable on an open network.
>
> ​	将默认的 `docker` 守护进程绑定到 TCP 端口或 Unix `docker` 用户组会带来安全风险，因为这可能允许非 root 用户获得主机的 root 访问权限。请确保控制对 `docker` 的访问。如果绑定到 TCP 端口，任何可以访问该端口的人将拥有完全的 Docker 访问权限，因此不建议在开放网络上使用。

With `-H` it's possible to make the Docker daemon to listen on a specific IP and port. By default, it listens on `unix:///var/run/docker.sock` to allow only local connections by the root user. You could set it to `0.0.0.0:2375` or a specific host IP to give access to everybody, but that isn't recommended because someone could gain root access to the host where the daemon is running.

​	通过 `-H` 可以让 Docker 守护进程监听特定的 IP 和端口。默认情况下，它监听 `unix:///var/run/docker.sock` 以仅允许 root 用户进行本地连接。可以将其设置为 `0.0.0.0:2375` 或特定的主机 IP 来允许所有人访问，但不建议这样做，因为有人可能获得运行守护进程的主机的 root 访问权限。

Similarly, the Docker client can use `-H` to connect to a custom port. The Docker client defaults to connecting to `unix:///var/run/docker.sock` on Linux, and `tcp://127.0.0.1:2376` on Windows.

​	同样，Docker 客户端也可以使用 `-H` 连接到自定义端口。Docker 客户端默认连接到 Linux 上的 `unix:///var/run/docker.sock`，在 Windows 上连接到 `tcp://127.0.0.1:2376`。

`-H` accepts host and port assignment in the following format:

​	`-H` 接受以下格式的主机和端口分配：

```text
tcp://[host]:[port][path] or unix://path
```

For example:

- `tcp://` -> TCP connection to `127.0.0.1` on either port `2376` when TLS encryption is on, or port `2375` when communication is in plain text.
  - `tcp://` -> 当启用 TLS 加密时连接到 `127.0.0.1` 的端口 `2376`，当以纯文本通信时连接到端口 `2375`。

- `tcp://host:2375` -> TCP connection on host:2375
  - `tcp://host:2375` -> 连接到主机 host 的端口 2375

- `tcp://host:2375/path` -> TCP connection on host:2375 and prepend path to all requests
  - `tcp://host:2375/path` -> 连接到主机 host 的端口 2375，并在所有请求前添加路径

- `unix://path/to/socket` -> Unix socket located at `path/to/socket`
  - `unix://path/to/socket` -> 位于 `path/to/socket` 的 Unix 套接字


`-H`, when empty, defaults to the same value as when no `-H` was passed in.

​	`-H` 为空时，默认值与未传入 `-H` 时相同。

`-H` also accepts short form for TCP bindings: `host:` or `host:port` or `:port`
	`-H`也接受 TCP 绑定的简写形式：`host:` 或 `host:port` 或 `:port`

Run Docker in daemon mode:

​	以守护模式运行 Docker：

```console
$ sudo <path to>/dockerd -H 0.0.0.0:5555 &
```

Download an `ubuntu` image:

​	下载一个 `ubuntu` 镜像：

```console
$ docker -H :5555 pull ubuntu
```

You can use multiple `-H`, for example, if you want to listen on both TCP and a Unix socket

​	可以使用多个 `-H`，例如，如果希望同时监听 TCP 和 Unix 套接字：



```console
$ sudo dockerd -H tcp://127.0.0.1:2375 -H unix:///var/run/docker.sock &
# Download an ubuntu image, use default Unix socket
# 下载 ubuntu 镜像，使用默认 Unix 套接字
$ docker pull ubuntu
# OR use the TCP port
# 或者使用 TCP 端口
$ docker -H tcp://127.0.0.1:2375 pull ubuntu
```

### 守护进程存储驱动程序 Daemon storage-driver

On Linux, the Docker daemon has support for several different image layer storage drivers: `overlay2`, `fuse-overlayfs`, `btrfs`, and `zfs`.

​	在 Linux 上，Docker 守护进程支持多种不同的镜像层存储驱动程序：`overlay2`、`fuse-overlayfs`、`btrfs` 和 `zfs`。

`overlay2` is the preferred storage driver for all currently supported Linux distributions, and is selected by default. Unless users have a strong reason to prefer another storage driver, `overlay2` should be used.

​	`overlay2` 是所有当前支持的 Linux 发行版的首选存储驱动程序，并且默认选用。除非用户有强烈的理由选择其他存储驱动程序，否则应使用 `overlay2`。

You can find out more about storage drivers and how to select one in [Select a storage driver]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers/Selectastoragedriver" >}}).

​	可以在 [选择存储驱动程序]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers/Selectastoragedriver" >}}) 中了解有关存储驱动程序及其选择的更多信息。

On Windows, the Docker daemon only supports the `windowsfilter` storage driver.

​	在 Windows 上，Docker 守护进程仅支持 `windowsfilter` 存储驱动程序。

### 每种存储驱动的选项 Options per storage driver

Particular storage-driver can be configured with options specified with `--storage-opt` flags. Options for `zfs` start with `zfs`, and options for `btrfs` start with `btrfs`.

​	可以通过 `--storage-opt` 标志指定的选项来配置特定的存储驱动程序。`zfs` 的选项以 `zfs` 开头，`btrfs` 的选项以 `btrfs` 开头。

#### ZFS options

##### `zfs.fsname`

Specifies the ZFS filesystem that the daemon should use to create its datasets. By default, the ZFS filesystem in `/var/lib/docker` is used.

​	指定守护进程用于创建数据集的 ZFS 文件系统。默认情况下使用 `/var/lib/docker` 中的 ZFS 文件系统。

###### Example



```console
$ sudo dockerd -s zfs --storage-opt zfs.fsname=zroot/docker
```

#### Btrfs options

##### `btrfs.min_space`

Specifies the minimum size to use when creating the subvolume which is used for containers. If user uses disk quota for btrfs when creating or running a container with **--storage-opt size** option, Docker should ensure the **size** can't be smaller than **btrfs.min_space**.

​	指定创建用于容器的子卷时的最小大小。如果用户在创建或运行容器时使用 **--storage-opt size** 选项的 btrfs 磁盘配额，Docker 应确保 **size** 不能小于 **btrfs.min_space**。

###### Example



```console
$ sudo dockerd -s btrfs --storage-opt btrfs.min_space=10G
```

#### Overlay2 options

##### `overlay2.size`

Sets the default max size of the container. It is supported only when the backing filesystem is `xfs` and mounted with `pquota` mount option. Under these conditions the user can pass any size less than the backing filesystem size.

​	设置容器的默认最大大小。仅当支持文件系统为 `xfs` 并且使用 `pquota` 挂载选项时支持。在这些条件下，用户可以传递小于支持文件系统大小的任何大小。

###### Example



```console
$ sudo dockerd -s overlay2 --storage-opt overlay2.size=1G
```

#### Windowsfilter options

##### `size`

Specifies the size to use when creating the sandbox which is used for containers. Defaults to 20G.

​	指定创建用于容器的沙箱大小。默认值为 20G。

###### Example



```powershell
C:\> dockerd --storage-opt size=40G
```

### Runtime options

The Docker daemon relies on a [OCI](https://github.com/opencontainers/runtime-spec) compliant runtime (invoked via the `containerd` daemon) as its interface to the Linux kernel `namespaces`, `cgroups`, and `SELinux`.

​	Docker 守护进程依赖于符合 [OCI](https://github.com/opencontainers/runtime-spec) 的运行时（通过 `containerd` 守护进程调用）作为其与 Linux 内核 `namespaces`、`cgroups` 和 `SELinux` 的接口。

#### Configure container runtimes

By default, the Docker daemon uses runc as a container runtime. You can configure the daemon to add additional runtimes.

​	默认情况下，Docker 守护进程使用 runc 作为容器运行时。可以配置守护进程以添加其他运行时。

containerd shims installed on `PATH` can be used directly, without the need to edit the daemon's configuration. For example, if you install the Kata Containers shim (`containerd-shim-kata-v2`) on `PATH`, then you can select that runtime with `docker run` without having to edit the daemon's configuration:

​	安装在 `PATH` 上的 containerd shim 可以直接使用，无需编辑守护进程的配置。例如，如果在 `PATH` 上安装了 Kata Containers shim（`containerd-shim-kata-v2`），则可以通过 `docker run` 选择该运行时，而无需编辑守护进程的配置：

```console
$ docker run --runtime io.containerd.kata.v2
```

Container runtimes that don't implement containerd shims, or containerd shims installed outside of `PATH`, must be registered with the daemon, either via the configuration file or using the `--add-runtime` command line flag.

​	未实现 containerd shim 的容器运行时或安装在 `PATH` 之外的 containerd shim 必须通过配置文件或 `--add-runtime` 命令行标志注册到守护进程。

For examples on how to use other container runtimes, see [Alternative container runtimes]({{< ref "/manuals/DockerEngine/Daemon/Alternativecontainerruntimes" >}})

​	有关如何使用其他容器运行时的示例，请参阅 [Alternative container runtimes]({{< ref "/manuals/DockerEngine/Daemon/Alternativecontainerruntimes" >}})。

##### Configure runtimes using `daemon.json`

To register and configure container runtimes using the daemon's configuration file, add the runtimes as entries under `runtimes`:

​	要使用守护进程的配置文件注册和配置容器运行时，请在 `runtimes` 下添加运行时条目：



```json
{
  "runtimes": {
    "<runtime>": {}
  }
}
```

The key of the entry (`<runtime>` in the previous example) represents the name of the runtime. This is the name that you reference when you run a container, using `docker run --runtime <runtime>`.

​	条目的键（上例中的 `<runtime>`）表示运行时的名称。运行容器时，可以使用 `docker run --runtime <runtime>` 来引用此名称。

The runtime entry contains an object specifying the configuration for your runtime. The properties of the object depends on what kind of runtime you're looking to register:

​	运行时条目包含一个对象，用于指定运行时的配置。对象的属性取决于要注册的运行时类型：

- If the runtime implements its own containerd shim, the object shall contain a `runtimeType` field and an optional `options` field.

  如果运行时实现了自己的 containerd shim，该对象应包含 `runtimeType` 字段和可选的 `options` 字段。

  ```json
  {
    "runtimes": {
      "<runtime>": {
        "runtimeType": "<name-or-path>",
        "options": {}
      }
    }
  }
  ```

  See [Configure shims](https://docs.docker.com/reference/cli/dockerd/#configure-containerd-shims).

  ​	有关详细信息，请参阅 [配置 shims](https://docs.docker.com/reference/cli/dockerd/#configure-containerd-shims)。

- If the runtime is designed to be a drop-in replacement for runc, the object contains a `path` field, and an optional `runtimeArgs` field.

  如果运行时设计为 runc 的替代方案，则对象包含一个 `path` 字段和可选的 `runtimeArgs` 字段。

  ```json
  {
    "runtimes": {
      "<runtime>": {
        "path": "/path/to/bin",
        "runtimeArgs": ["...args"]
      }
    }
  }
  ```

  See [Configure runc drop-in replacements](https://docs.docker.com/reference/cli/dockerd/#configure-runc-drop-in-replacements).
  
  ​	查看 [配置 runc 替代方案](https://docs.docker.com/reference/cli/dockerd/#configure-runc-drop-in-replacements)。

After changing the runtimes configuration in the configuration file, you must reload or restart the daemon for changes to take effect:

​	更改配置文件中的运行时配置后，必须重新加载或重启守护进程以使更改生效：

```console
$ sudo systemctl reload dockerd
```

##### Configure containerd shims

If the runtime that you want to register implements a containerd shim, or if you want to register a runtime which uses the runc shim, use the following format for the runtime entry:

​	如果要注册的运行时实现了 containerd shim 或使用 runc shim，则可以使用以下格式为运行时条目进行配置：

```json
{
  "runtimes": {
    "<runtime>": {
      "runtimeType": "<name-or-path>",
      "options": {}
    }
  }
}
```

`runtimeType` refers to either:

​	`runtimeType` 指的是以下任一项：

- A fully qualified name of a containerd shim. 完整限定名称的 containerd shim。

  The fully qualified name of a shim is the same as the `runtime_type` used to register the runtime in containerd's CRI configuration. For example, `io.containerd.runsc.v1`.

  ​	shim 的完整名称与 containerd 的 CRI 配置中注册运行时所使用的 `runtime_type` 相同。例如，`io.containerd.runsc.v1`。

- The path of a containerd shim binary. containerd shim 二进制文件的路径。

  This option is useful if you installed the containerd shim binary outside of `PATH`.
  
  ​	如果将 containerd shim 安装在 `PATH` 之外，这个选项非常有用。

`options` is optional. It lets you specify the runtime configuration that you want to use for the shim. The configuration parameters that you can specify in `options` depends on the runtime you're registering. For most shims, the supported configuration options are `TypeUrl` and `ConfigPath`. For example:

​	`options` 是可选的，它允许您为 shim 指定运行时配置。可以在 `options` 中指定的配置参数取决于您注册的运行时。对于大多数 shims，支持的配置选项包括 `TypeUrl` 和 `ConfigPath`。例如：

```json
{
  "runtimes": {
    "gvisor": {
      "runtimeType": "io.containerd.runsc.v1",
      "options": {
        "TypeUrl": "io.containerd.runsc.v1.options",
        "ConfigPath": "/etc/containerd/runsc.toml"
      }
    }
  }
}
```

You can configure multiple runtimes using the same runtimeType. For example:

​	您可以使用相同的 `runtimeType` 配置多个运行时。例如：

```json
{
  "runtimes": {
    "gvisor-foo": {
      "runtimeType": "io.containerd.runsc.v1",
      "options": {
        "TypeUrl": "io.containerd.runsc.v1.options",
        "ConfigPath": "/etc/containerd/runsc-foo.toml"
      }
    },
    "gvisor-bar": {
      "runtimeType": "io.containerd.runsc.v1",
      "options": {
        "TypeUrl": "io.containerd.runsc.v1.options",
        "ConfigPath": "/etc/containerd/runsc-bar.toml"
      }
    }
  }
}
```

The `options` field takes a special set of configuration parameters when used with `"runtimeType": "io.containerd.runc.v2"`. For more information about runc parameters, refer to the runc configuration section in [CRI Plugin Config Guide](https://github.com/containerd/containerd/blob/v1.7.2/docs/cri/config.md#full-configuration).

​	在使用 `"runtimeType": "io.containerd.runc.v2"` 时，`options` 字段接受一组特殊的配置参数。有关 runc 参数的更多信息，请参阅 [CRI 插件配置指南](https://github.com/containerd/containerd/blob/v1.7.2/docs/cri/config.md#full-configuration) 中的 runc 配置部分。

##### 配置 runc 的替代项 Configure runc drop-in replacements

If the runtime that you want to register can act as a drop-in replacement for runc, you can register the runtime either using the daemon configuration file, or using the `--add-runtime` flag for the `dockerd` cli.

​	如果要注册的运行时可以作为 runc 的替代项，则可以通过守护进程配置文件或 `dockerd` CLI 的 `--add-runtime` 标志来注册运行时。

When you use the configuration file, the entry uses the following format:

​	使用配置文件时，条目格式如下：

```json
{
  "runtimes": {
    "<runtime>": {
      "path": "/path/to/binary",
      "runtimeArgs": ["...args"]
    }
  }
}
```

Where `path` is either the absolute path to the runtime executable, or the name of an executable installed on `PATH`:

​	其中 `path` 是运行时可执行文件的绝对路径，或安装在 `PATH` 上的可执行文件名称：

```json
{
  "runtimes": {
    "runc": {
      "path": "runc"
    }
  }
}
```

And `runtimeArgs` lets you optionally pass additional arguments to the runtime. Entries with this format use the containerd runc shim to invoke a custom runtime binary.

​	`runtimeArgs` 允许您选择性地为运行时传递额外的参数。使用这种格式的条目将通过 containerd runc shim 调用自定义运行时二进制文件。

When you use the `--add-runtime` CLI flag, use the following format:

​	使用 `--add-runtime` CLI 标志时，格式如下：

```console
$ sudo dockerd --add-runtime <runtime>=<path>
```

Defining runtime arguments via the command line is not supported.

​	不支持通过命令行定义运行时参数。

For an example configuration for a runc drop-in replacment, see [Alternative container runtimes > youki](https://docs.docker.com/engine/daemon/alternative-runtimes/#youki)

​	有关 runc 替代项的配置示例，请参阅 [Alternative container runtimes > youki](https://docs.docker.com/engine/daemon/alternative-runtimes/#youki)。

##### Configure the default container runtime

You can specify either the name of a fully qualified containerd runtime shim, or the name of a registered runtime. You can specify the default runtime either using the daemon configuration file, or using the `--default-runtime` flag for the `dockerd` cli.

​	可以指定完整限定的 containerd 运行时 shim 名称或已注册的运行时名称。可以使用守护进程配置文件或 `dockerd` CLI 的 `--default-runtime` 标志指定默认运行时。

When you use the configuration file, the entry uses the following format:

​	使用配置文件时，条目格式如下：

```json
{
  "default-runtime": "io.containerd.runsc.v1"
}
```

When you use the `--default-runtime` CLI flag, use the following format:

​	使用 `--default-runtime` CLI 标志时，格式如下：

```console
$ dockerd --default-runtime io.containerd.runsc.v1
```

#### 独立运行 containerd - Run containerd standalone

By default, the Docker daemon automatically starts `containerd`. If you want to control `containerd` startup, manually start `containerd` and pass the path to the `containerd` socket using the `--containerd` flag. For example:

​	默认情况下，Docker 守护进程会自动启动 `containerd`。如果想要控制 `containerd` 的启动，手动启动 `containerd` 并使用 `--containerd` 标志传递 `containerd` socket 的路径。例如：

```console
$ sudo dockerd --containerd /run/containerd/containerd.sock
```

#### Configure cgroup driver

You can configure how the runtime should manage container cgroups, using the `--exec-opt native.cgroupdriver` CLI flag.

​	可以使用 `--exec-opt native.cgroupdriver` CLI 标志配置运行时如何管理容器的 cgroups。

You can only specify `cgroupfs` or `systemd`. If you specify `systemd` and it is not available, the system errors out. If you omit the `native.cgroupdriver` option,`cgroupfs` is used on cgroup v1 hosts, `systemd` is used on cgroup v2 hosts with systemd available.

​	只能指定 `cgroupfs` 或 `systemd`。如果指定 `systemd` 且不可用，系统会报错。如果省略 `native.cgroupdriver` 选项，cgroup v1 主机上使用 `cgroupfs`，cgroup v2 主机上有 systemd 可用时使用 `systemd`。

This example sets the `cgroupdriver` to `systemd`:

​	以下示例将 `cgroupdriver` 设置为 `systemd`：

```console
$ sudo dockerd --exec-opt native.cgroupdriver=systemd
```

Setting this option applies to all containers the daemon launches.

​	此选项适用于守护进程启动的所有容器。

#### 配置容器隔离技术（Windows） Configure container isolation technology (Windows)

For Windows containers, you can specify the default container isolation technology to use, using the `--exec-opt isolation` flag.

​	对于 Windows 容器，可以使用 `--exec-opt isolation` 标志指定默认容器隔离技术。

The following example makes `hyperv` the default isolation technology:

​	以下示例将 `hyperv` 设置为默认隔离技术：

```console
> dockerd --exec-opt isolation=hyperv
```

If no isolation value is specified on daemon start, on Windows client, the default is `hyperv`, and on Windows server, the default is `process`.

​	如果在守护进程启动时未指定隔离值，Windows 客户端的默认值为 `hyperv`，Windows 服务器的默认值为 `process`。

### Daemon DNS options

To set the DNS server for all Docker containers, use:

​	要为所有 Docker 容器设置 DNS 服务器，请使用：

```console
$ sudo dockerd --dns 8.8.8.8
```

To set the DNS search domain for all Docker containers, use:

​	要为所有 Docker 容器设置 DNS 搜索域，请使用：

```console
$ sudo dockerd --dns-search example.com
```

### 允许推送非可分发的工件 Allow push of non-distributable artifacts

Some images (e.g., Windows base images) contain artifacts whose distribution is restricted by license. When these images are pushed to a registry, restricted artifacts are not included.

​	某些映像（例如，Windows 基础映像）包含受许可限制的分发工件。将这些映像推送到注册表时，不包含受限工件。

To override this behavior for specific registries, use the `--allow-nondistributable-artifacts` option in one of the following forms:

​	要为特定注册表覆盖此行为，请使用 `--allow-nondistributable-artifacts` 选项，格式如下：

- `--allow-nondistributable-artifacts myregistry:5000` tells the Docker daemon to push non-distributable artifacts to myregistry:5000.
  - `--allow-nondistributable-artifacts myregistry:5000` 告诉 Docker 守护进程将非可分发工件推送到 myregistry:5000。

- `--allow-nondistributable-artifacts 10.1.0.0/16` tells the Docker daemon to push non-distributable artifacts to all registries whose resolved IP address is within the subnet described by the CIDR syntax.
  - `--allow-nondistributable-artifacts 10.1.0.0/16` 告诉 Docker 守护进程将非可分发工件推送到所有 IP 地址在指定 CIDR 子网范围内的注册表。


This option can be used multiple times.

​	此选项可以多次使用。

This option is useful when pushing images containing non-distributable artifacts to a registry on an air-gapped network so hosts on that network can pull the images without connecting to another server.

​	当将包含非可分发工件的映像推送到隔离网络上的注册表时，此选项很有用，使得该网络上的主机可以在不连接到其他服务器的情况下拉取这些映像。

> **Warning**
>
> Non-distributable artifacts typically have restrictions on how and where they can be distributed and shared. Only use this feature to push artifacts to private registries and ensure that you are in compliance with any terms that cover redistributing non-distributable artifacts.
>
> ​	非可分发工件通常对分发和共享方式有限制。仅将此功能用于推送到私有注册表，并确保遵守任何有关再分发非可分发工件的条款。

### 不安全的注册表 Insecure registries

In this section, "registry" refers to a private registry, and `myregistry:5000` is a placeholder example of a private registry.

​	在本节中，“注册表”指私有注册表，`myregistry:5000` 是私有注册表的占位示例。

Docker considers a private registry either secure or insecure. A secure registry uses TLS and a copy of its CA certificate is placed on the Docker host at `/etc/docker/certs.d/myregistry:5000/ca.crt`. An insecure registry is either not using TLS (i.e., listening on plain text HTTP), or is using TLS with a CA certificate not known by the Docker daemon. The latter can happen when the certificate wasn't found under `/etc/docker/certs.d/myregistry:5000/`, or if the certificate verification failed (i.e., wrong CA).

​	Docker 将私有注册表视为安全或不安全。安全注册表使用 TLS，并将其 CA 证书副本放置在 Docker 主机的 `/etc/docker/certs.d/myregistry:5000/ca.crt`。不安全注册表要么不使用 TLS（即在纯文本 HTTP 上监听），要么使用 TLS 但 Docker 守护进程未识别的 CA 证书。当 `/etc/docker/certs.d/myregistry:5000/` 下未找到证书，或者证书验证失败（如错误的 CA）时，可能发生后者。

By default, Docker assumes all registries to be secure, except for local registries. Communicating with an insecure registry isn't possible if Docker assumes that registry is secure. In order to communicate with an insecure registry, the Docker daemon requires `--insecure-registry` in one of the following two forms:

​	默认情况下，Docker 假定所有注册表都是安全的，除了本地注册表。如果 Docker 假定某个注册表安全，则无法与不安全的注册表通信。要与不安全的注册表通信，Docker 守护进程需要以下格式的 `--insecure-registry`：

- `--insecure-registry myregistry:5000` tells the Docker daemon that myregistry:5000 should be considered insecure.
  - `--insecure-registry myregistry:5000` 告诉 Docker 守护进程将 myregistry:5000 视为不安全。

- `--insecure-registry 10.1.0.0/16` tells the Docker daemon that all registries whose domain resolve to an IP address is part of the subnet described by the CIDR syntax, should be considered insecure.
  - `--insecure-registry 10.1.0.0/16` 告诉 Docker 守护进程将所有域解析到 IP 地址在 CIDR 描述的子网范围内的注册表视为不安全。


The flag can be used multiple times to allow multiple registries to be marked as insecure.

​	此标志可多次使用，以允许多个注册表被标记为不安全。

If an insecure registry isn't marked as insecure, `docker pull`, `docker push`, and `docker search` result in error messages, prompting the user to either secure or pass the `--insecure-registry` flag to the Docker daemon as described above.

​	如果未将不安全的注册表标记为不安全，`docker pull`、`docker push` 和 `docker search` 会显示错误消息，提示用户要么启用安全，要么根据上文所述向 Docker 守护进程传递 `--insecure-registry` 标志。

Local registries, whose IP address falls in the 127.0.0.0/8 range, are automatically marked as insecure as of Docker 1.3.2. It isn't recommended to rely on this, as it may change in the future.

​	Docker 1.3.2 起，IP 地址在 127.0.0.0/8 范围内的本地注册表自动被标记为不安全。不建议依赖此行为，因为它可能在未来发生变化。

Enabling `--insecure-registry`, i.e., allowing un-encrypted and/or untrusted communication, can be useful when running a local registry. However, because its use creates security vulnerabilities it should only be enabled for testing purposes. For increased security, users should add their CA to their system's list of trusted CAs instead of enabling `--insecure-registry`.

​	启用 `--insecure-registry`，即允许未加密和/或不受信任的通信，在运行本地注册表时可能很有用。然而，由于其使用会带来安全漏洞，仅应在测试目的时启用。为提高安全性，用户应将其 CA 添加到系统的受信任 CA 列表，而不是启用 `--insecure-registry`。

#### 旧版注册表 Legacy Registries

Operations against registries supporting only the legacy v1 protocol are no longer supported. Specifically, the daemon doesn't attempt to push, pull or sign in to v1 registries. The exception to this is `search` which can still be performed on v1 registries.

​	对仅支持旧版 v1 协议的注册表的操作已不再受支持。具体而言，守护进程不再尝试将数据推送、拉取或登录到 v1 注册表。例外情况是可以对 v1 注册表执行 `search` 操作。

### Running a Docker daemon behind an HTTPS_PROXY

When running inside a LAN that uses an `HTTPS` proxy, the proxy's certificates replace Docker Hub's certificates. These certificates must be added to your Docker host's configuration:

​	在使用 `HTTPS` 代理的局域网中运行时，代理的证书会替换 Docker Hub 的证书。必须将这些证书添加到 Docker 主机的配置中：

1. Install the `ca-certificates` package for your distribution 为您的发行版安装 `ca-certificates` 软件包。
2. Ask your network admin for the proxy's CA certificate and append them to `/etc/pki/tls/certs/ca-bundle.crt` 向网络管理员请求代理的 CA 证书并将其附加到 `/etc/pki/tls/certs/ca-bundle.crt`。
3. Then start your Docker daemon with `HTTPS_PROXY=http://username:password@proxy:port/ dockerd`. The `username:` and `password@` are optional - and are only needed if your proxy is set up to require authentication. 使用 `HTTPS_PROXY=http://username:password@proxy:port/ dockerd` 启动 Docker 守护进程。`username:` 和 `password@` 是可选项，仅在代理需要身份验证时才需要。

This only adds the proxy and authentication to the Docker daemon's requests. To use the proxy when building images and running containers, see [Configure Docker to use a proxy server]({{< ref "/manuals/DockerEngine/CLI/Proxyconfiguration" >}})

​	这仅为 Docker 守护进程的请求添加代理和身份验证。要在构建映像和运行容器时使用代理，请参阅 [配置 Docker 使用代理服务器]({{< ref "/manuals/DockerEngine/CLI/Proxyconfiguration" >}})。

### 默认 `ulimit` 设置 Default `ulimit` settings

The `--default-ulimit` flag lets you set the default `ulimit` options to use for all containers. It takes the same options as `--ulimit` for `docker run`. If these defaults aren't set, `ulimit` settings are inherited from the Docker daemon. Any `--ulimit` options passed to `docker run` override the daemon defaults.

​	`--default-ulimit` 标志允许设置所有容器的默认 `ulimit` 选项。它与 `docker run` 的 `--ulimit` 选项相同。如果未设置这些默认值，`ulimit` 设置将继承自 Docker 守护进程。传递给 `docker run` 的任何 `--ulimit` 选项会覆盖守护进程的默认值。

Be careful setting `nproc` with the `ulimit` flag, as `nproc` is designed by Linux to set the maximum number of processes available to a user, not to a container. For details, see [`docker run` reference](https://docs.docker.com/reference/cli/docker/container/run/#ulimit).

​	使用 `ulimit` 标志设置 `nproc` 时要小心，因为 `nproc` 旨在设置用户可用的最大进程数，而不是容器。详情请参见 [`docker run` 参考](https://docs.docker.com/reference/cli/docker/container/run/#ulimit)。

### 访问授权 Access authorization

Docker's access authorization can be extended by authorization plugins that your organization can purchase or build themselves. You can install one or more authorization plugins when you start the Docker `daemon` using the `--authorization-plugin=PLUGIN_ID` option.

​	Docker 的访问授权可以通过授权插件进行扩展，这些插件可以由您的组织购买或自行构建。可以在启动 Docker 守护进程时使用 `--authorization-plugin=PLUGIN_ID` 选项安装一个或多个授权插件。



```console
$ sudo dockerd --authorization-plugin=plugin1 --authorization-plugin=plugin2,...
```

The `PLUGIN_ID` value is either the plugin's name or a path to its specification file. The plugin's implementation determines whether you can specify a name or path. Consult with your Docker administrator to get information about the plugins available to you.

​	`PLUGIN_ID` 值可以是插件的名称或其规范文件的路径。插件的实现决定了可以指定名称或路径。请咨询您的 Docker 管理员以获取可用插件的信息。

Once a plugin is installed, requests made to the `daemon` through the command line or Docker's Engine API are allowed or denied by the plugin. If you have multiple plugins installed, each plugin, in order, must allow the request for it to complete.

​	安装插件后，通过命令行或 Docker 的引擎 API 向守护进程发出的请求将由插件允许或拒绝。如果安装了多个插件，必须按顺序获得每个插件的允许才能完成请求。

For information about how to create an authorization plugin, refer to the [authorization plugin]({{< ref "/manuals/DockerEngine/DockerEngineplugins/Accessauthorizationplugin" >}}) section.

​	有关如何创建授权插件的信息，请参阅 [authorization plugin]({{< ref "/manuals/DockerEngine/DockerEngineplugins/Accessauthorizationplugin" >}}) 部分。

### 守护进程用户命名空间选项 Daemon user namespace options

The Linux kernel [user namespace support](https://man7.org/linux/man-pages/man7/user_namespaces.7.html) provides additional security by enabling a process, and therefore a container, to have a unique range of user and group IDs which are outside the traditional user and group range utilized by the host system. One of the most important security improvements is that, by default, container processes running as the `root` user have expected administrative privileges it expects (with some restrictions) inside the container, but are effectively mapped to an unprivileged `uid` on the host.

​	Linux 内核 [用户命名空间支持](https://man7.org/linux/man-pages/man7/user_namespaces.7.html) 通过为进程（因此为容器）提供一个独特的用户和组 ID 范围来增加额外的安全性，这些范围不在主机系统使用的传统用户和组范围内。最重要的安全改进之一是，默认情况下，作为 `root` 用户运行的容器进程在容器内部具有预期的管理权限（带有某些限制），但在主机上实际上映射为一个非特权 `uid`。

For details about how to use this feature, as well as limitations, see [Isolate containers with a user namespace]({{< ref "/manuals/DockerEngine/Security/Isolatecontainerswithausernamespace" >}}).

​	有关如何使用此功能以及限制的详细信息，请参阅 [Isolate containers with a user namespace]({{< ref "/manuals/DockerEngine/Security/Isolatecontainerswithausernamespace" >}})。

### 配置主机网关 IP Configure host gateway IP

The Docker daemon supports a special `host-gateway` value for the `--add-host` flag for the `docker run` and `docker build` commands. This value resolves to the host's gateway IP and lets containers connect to services running on the host.

​	Docker 守护进程支持 `docker run` 和 `docker build` 命令的 `--add-host` 标志中的特殊 `host-gateway` 值。此值解析为主机的网关 IP，使容器可以连接到主机上运行的服务。

By default, `host-gateway` resolves to the IP address of the default bridge. You can configure this to resolve to a different IP using the `--host-gateway-ip` flag for the dockerd command line interface, or the `host-gateway-ip` key in the daemon configuration file.

​	默认情况下，`host-gateway` 解析为默认桥的 IP 地址。可以通过 dockerd 命令行接口的 `--host-gateway-ip` 标志或守护进程配置文件中的 `host-gateway-ip` 键将其配置为解析为其他 IP。

```console
$ cat > /etc/docker/daemon.json
{ "host-gateway-ip": "192.0.2.0" }
$ sudo systemctl restart docker
$ docker run -it --add-host host.docker.internal:host-gateway \
  busybox ping host.docker.internal 
PING host.docker.internal (192.0.2.0): 56 data bytes
```

### 启用 CDI 设备 Enable CDI devices

> **Note**
>
> This is experimental feature and as such doesn't represent a stable API.
>
> ​	这是实验性功能，因此不代表稳定的 API。
>
> This feature isn't enabled by default. To this feature, set `features.cdi` to `true` in the `daemon.json` configuration file.
>
> ​	默认情况下未启用此功能。要启用此功能，请在 `daemon.json` 配置文件中设置 `features.cdi` 为 `true`。

Container Device Interface (CDI) is a [standardized](https://github.com/cncf-tags/container-device-interface/blob/main/SPEC.md) mechanism for container runtimes to create containers which are able to interact with third party devices.

​	容器设备接口（CDI）是一种 [标准化](https://github.com/cncf-tags/container-device-interface/blob/main/SPEC.md) 的机制，使容器运行时可以创建能够与第三方设备交互的容器。

The Docker daemon supports running containers with CDI devices if the requested device specifications are available on the filesystem of the daemon.

​	如果请求的设备规范在守护进程的文件系统上可用，Docker 守护进程支持运行带有 CDI 设备的容器。

The default specification directors are:

​	默认的规范目录是：

- `/etc/cdi/` for static CDI Specs
  - `/etc/cdi/` 用于静态 CDI 规范

- `/var/run/cdi` for generated CDI Specs
  - `/var/run/cdi` 用于生成的 CDI 规范


Alternatively, you can set custom locations for CDI specifications using the `cdi-spec-dirs` option in the `daemon.json` configuration file, or the `--cdi-spec-dir` flag for the `dockerd` CLI.

​	或者，可以在 `daemon.json` 配置文件中使用 `cdi-spec-dirs` 选项，或在 `dockerd` CLI 中使用 `--cdi-spec-dir` 标志来设置 CDI 规范的自定义位置。

```json
{
  "features": {
     "cdi": true
  },
  "cdi-spec-dirs": ["/etc/cdi/", "/var/run/cdi"]
}
```

When CDI is enabled for a daemon, you can view the configured CDI specification directories using the `docker info` command.

​	为守护进程启用 CDI 后，可以使用 `docker info` 命令查看配置的 CDI 规范目录。

#### 守护进程日志格式 Daemon logging format

The `--log-format` option or "log-format" option in the [daemon configuration file](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file) lets you set the format for logs produced by the daemon. The logging format should only be configured either through the `--log-format` command line option or through the "log-format" field in the configuration file; using both the command-line option and the "log-format" field in the configuration file produces an error. If this option is not set, the default is "text".

​	`--log-format` 选项或 [守护进程配置文件](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file) 中的 "log-format" 选项允许您设置守护进程生成的日志格式。日志格式应仅通过 `--log-format` 命令行选项或配置文件中的 "log-format" 字段配置；如果同时使用命令行选项和配置文件中的 "log-format" 字段，会产生错误。如果未设置此选项，默认格式为 "text"。

The following example configures the daemon through the `--log-format` command line option to use `json` formatted logs;

​	以下示例通过 `--log-format` 命令行选项配置守护进程以使用 `json` 格式的日志：

```console
$ dockerd --log-format=json
# ...
{"level":"info","msg":"API listen on /var/run/docker.sock","time":"2024-09-16T11:06:08.558145428Z"}
```

The following example shows a `daemon.json` configuration file with the "log-format" set;

​	以下示例显示了在 `daemon.json` 配置文件中设置 "log-format" 的配置：

```json
{
  "log-format": "json"
}
```

### 其他选项 Miscellaneous options

IP masquerading uses address translation to allow containers without a public IP to talk to other machines on the internet. This may interfere with some network topologies, and can be disabled with `--ip-masq=false`.

​	IP 伪装使用地址转换允许没有公共 IP 的容器与互联网中的其他机器通信。这可能会影响某些网络拓扑，可通过 `--ip-masq=false` 禁用。

Docker supports soft links for the Docker data directory (`/var/lib/docker`) and for `/var/lib/docker/tmp`. The `DOCKER_TMPDIR` and the data directory can be set like this:

​	Docker 支持 Docker 数据目录 (`/var/lib/docker`) 和 `/var/lib/docker/tmp` 的软链接。可以按如下方式设置 `DOCKER_TMPDIR` 和数据目录：



```console
$ export DOCKER_TMPDIR=/mnt/disk2/tmp
$ sudo -E dockerd --data-root /var/lib/docker -H unix://
```

#### Default cgroup parent

The `--cgroup-parent` option lets you set the default cgroup parent for containers. If this option isn't set, it defaults to `/docker` for the cgroupfs driver, and `system.slice` for the systemd cgroup driver.

​	`--cgroup-parent` 选项允许为容器设置默认的 cgroup 父级。如果未设置此选项，`cgroupfs` 驱动程序的默认值为 `/docker`，而 `systemd` cgroup 驱动程序的默认值为 `system.slice`。

If the cgroup has a leading forward slash (`/`), the cgroup is created under the root cgroup, otherwise the cgroup is created under the daemon cgroup.

​	如果 cgroup 以正斜杠 (`/`) 开头，则该 cgroup 将在根 cgroup 下创建；否则，该 cgroup 将在守护进程 cgroup 下创建。

Assuming the daemon is running in cgroup `daemoncgroup`, `--cgroup-parent=/foobar` creates a cgroup in `/sys/fs/cgroup/memory/foobar`, whereas using `--cgroup-parent=foobar` creates the cgroup in `/sys/fs/cgroup/memory/daemoncgroup/foobar`

​	假设守护进程运行在 `daemoncgroup` cgroup 中，使用 `--cgroup-parent=/foobar` 将在 `/sys/fs/cgroup/memory/foobar` 中创建一个 cgroup，而使用 `--cgroup-parent=foobar` 则会在 `/sys/fs/cgroup/memory/daemoncgroup/foobar` 中创建该 cgroup。

The systemd cgroup driver has different rules for `--cgroup-parent`. systemd represents hierarchy by slice and the name of the slice encodes the location in the tree. So `--cgroup-parent` for systemd cgroups should be a slice name. A name can consist of a dash-separated series of names, which describes the path to the slice from the root slice. For example, `--cgroup-parent=user-a-b.slice` means the memory cgroup for the container is created in `/sys/fs/cgroup/memory/user.slice/user-a.slice/user-a-b.slice/docker-<id>.scope`.

​	systemd cgroup 驱动程序对 `--cgroup-parent` 有不同的规则。systemd 使用切片表示层级，并通过切片名称对层级进行编码。因此，systemd cgroup 的 `--cgroup-parent` 应为切片名称。名称可以由连字符分隔的一系列名称组成，描述从根切片到目标切片的路径。例如，`--cgroup-parent=user-a-b.slice` 表示容器的内存 cgroup 将在 `/sys/fs/cgroup/memory/user.slice/user-a.slice/user-a-b.slice/docker-<id>.scope` 中创建。

This setting can also be set per container, using the `--cgroup-parent` option on `docker create` and `docker run`, and takes precedence over the `--cgroup-parent` option on the daemon.

​	此设置也可以在每个容器上设置，使用 `docker create` 和 `docker run` 的 `--cgroup-parent` 选项，优先于守护进程的 `--cgroup-parent` 设置。

#### Daemon metrics

The `--metrics-addr` option takes a TCP address to serve the metrics API. This feature is still experimental, therefore, the daemon must be running in experimental mode for this feature to work.

​	`--metrics-addr` 选项接受一个 TCP 地址来提供指标 API。由于此功能仍处于实验阶段，因此守护进程必须在实验模式下运行才能使该功能正常工作。

To serve the metrics API on `localhost:9323` you would specify `--metrics-addr 127.0.0.1:9323`, allowing you to make requests on the API at `127.0.0.1:9323/metrics` to receive metrics in the [prometheus](https://prometheus.io/docs/instrumenting/exposition_formats/) format.

​	要在 `localhost:9323` 上提供指标 API，可以指定 `--metrics-addr 127.0.0.1:9323`，允许在 `127.0.0.1:9323/metrics` 上请求 API，以 [prometheus](https://prometheus.io/docs/instrumenting/exposition_formats/) 格式接收指标。

Port `9323` is the [default port associated with Docker metrics](https://github.com/prometheus/prometheus/wiki/Default-port-allocations) to avoid collisions with other Prometheus exporters and services.

​	端口 `9323` 是 [Docker 指标的默认端口](https://github.com/prometheus/prometheus/wiki/Default-port-allocations)，避免与其他 Prometheus 导出程序和服务冲突。

If you are running a Prometheus server you can add this address to your scrape configs to have Prometheus collect metrics on Docker. For more information, see [Collect Docker metrics with Prometheus]({{< ref "/manuals/DockerEngine/Daemon/CollectDockermetricswithPrometheus" >}}).

​	如果您运行了 Prometheus 服务器，可以将该地址添加到您的抓取配置中，以便 Prometheus 收集 Docker 指标。详情请参阅 [使用 Prometheus 收集 Docker 指标]({{< ref "/manuals/DockerEngine/Daemon/CollectDockermetricswithPrometheus" >}})。

#### Node generic resources

The `--node-generic-resources` option takes a list of key-value pair (`key=value`) that allows you to advertise user defined resources in a Swarm cluster.

​	`--node-generic-resources` 选项接受一个键值对（`key=value`）列表，用于在 Swarm 集群中发布用户定义的资源。

The current expected use case is to advertise NVIDIA GPUs so that services requesting `NVIDIA-GPU=[0-16]` can land on a node that has enough GPUs for the task to run.

​	当前预期用例是发布 NVIDIA GPU，以便请求 `NVIDIA-GPU=[0-16]` 的服务可以在拥有足够 GPU 的节点上运行。

Example of usage:



```json
{
  "node-generic-resources": [
    "NVIDIA-GPU=UUID1",
    "NVIDIA-GPU=UUID2"
  ]
}
```

### Enable feature in the daemon (`--feature`)

The `--feature` option lets you enable or disable a feature in the daemon. This option corresponds with the "features" field in the [daemon.json configuration file](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file). Features should only be configured either through the `--feature` command line option or through the "features" field in the configuration file; using both the command-line option and the "features" field in the configuration file produces an error. The feature option can be specified multiple times to configure multiple features. The `--feature` option accepts a name and optional boolean value. When omitting the value, the default is `true`.

​	`--feature` 选项允许您在守护进程中启用或禁用某项功能。此选项与 [daemon.json 配置文件](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file) 中的 "features" 字段对应。功能应仅通过 `--feature` 命令行选项或配置文件中的 "features" 字段配置；同时使用命令行选项和配置文件中的 "features" 字段会产生错误。可以多次指定 `--feature` 选项来配置多个功能。`--feature` 选项接受名称和可选的布尔值。不提供值时，默认值为 `true`。

The following example runs the daemon with the `cdi` and `containerd-snapshotter` features enabled. The `cdi` option is provided with a value;

​	以下示例在守护进程中启用 `cdi` 和 `containerd-snapshotter` 功能，其中 `cdi` 功能提供了一个值：



```console
$ dockerd --feature cdi=true --feature containerd-snapshotter
```

The following example is the equivalent using the `daemon.json` configuration file;

​	以下示例是使用 `daemon.json` 配置文件的等效配置：



```json
{
  "features": {
    "cdi": true,
    "containerd-snapshotter": true
  }
}
```

### Daemon configuration file

The `--config-file` option allows you to set any configuration option for the daemon in a JSON format. This file uses the same flag names as keys, except for flags that allow several entries, where it uses the plural of the flag name, e.g., `labels` for the `label` flag.

​	`--config-file` 选项允许以 JSON 格式设置守护进程的任何配置选项。此文件使用与标志相同的键名，除非某些标志允许多个条目，文件中则使用标志的复数形式，如 `label` 标志对应 `labels`。

The options set in the configuration file must not conflict with options set using flags. The Docker daemon fails to start if an option is duplicated between the file and the flags, regardless of their value. This is intentional, and avoids silently ignore changes introduced in configuration reloads. For example, the daemon fails to start if you set daemon labels in the configuration file and also set daemon labels via the `--label` flag. Options that are not present in the file are ignored when the daemon starts.

​	配置文件中设置的选项不得与通过标志设置的选项冲突。如果某个选项在文件和标志中重复设置，Docker 守护进程将无法启动，无论它们的值如何。这种设计是有意的，以避免在配置重载中出现对更改的静默忽略。例如，如果在配置文件中设置了守护进程标签，同时又通过 `--label` 标志设置了守护进程标签，守护进程将无法启动。在守护进程启动时，文件中未出现的选项将被忽略。

The `--validate` option allows to validate a configuration file without starting the Docker daemon. A non-zero exit code is returned for invalid configuration files.

​	`--validate` 选项允许在不启动 Docker 守护进程的情况下验证配置文件。对于无效的配置文件，将返回非零退出代码。

```console
$ dockerd --validate --config-file=/tmp/valid-config.json
configuration OK

$ echo $?
0

$ dockerd --validate --config-file /tmp/invalid-config.json
unable to configure the Docker daemon with file /tmp/invalid-config.json: the following directives don't match any configuration option: unknown-option

$ echo $?
1
```

##### On Linux

The default location of the configuration file on Linux is `/etc/docker/daemon.json`. Use the `--config-file` flag to specify a non-default location.

​	在 Linux 上，配置文件的默认位置为 `/etc/docker/daemon.json`。使用 `--config-file` 标志可指定非默认位置。

The following is a full example of the allowed configuration options on Linux:

​	以下是在 Linux 上允许的完整配置选项示例：



```json
{
  "allow-nondistributable-artifacts": [],
  "api-cors-header": "",
  "authorization-plugins": [],
  "bip": "",
  "bridge": "",
  "builder": {
    "gc": {
      "enabled": true,
      "defaultKeepStorage": "10GB",
      "policy": [
        { "keepStorage": "10GB", "filter": ["unused-for=2200h"] },
        { "keepStorage": "50GB", "filter": ["unused-for=3300h"] },
        { "keepStorage": "100GB", "all": true }
      ]
    }
  },
  "cgroup-parent": "",
  "containerd": "/run/containerd/containerd.sock",
  "containerd-namespace": "docker",
  "containerd-plugins-namespace": "docker-plugins",
  "data-root": "",
  "debug": true,
  "default-address-pools": [
    {
      "base": "172.30.0.0/16",
      "size": 24
    },
    {
      "base": "172.31.0.0/16",
      "size": 24
    }
  ],
  "default-cgroupns-mode": "private",
  "default-gateway": "",
  "default-gateway-v6": "",
  "default-network-opts": {},
  "default-runtime": "runc",
  "default-shm-size": "64M",
  "default-ulimits": {
    "nofile": {
      "Hard": 64000,
      "Name": "nofile",
      "Soft": 64000
    }
  },
  "dns": [],
  "dns-opts": [],
  "dns-search": [],
  "exec-opts": [],
  "exec-root": "",
  "experimental": false,
  "features": {
    "cdi": true,
    "containerd-snapshotter": true
  },
  "fixed-cidr": "",
  "fixed-cidr-v6": "",
  "group": "",
  "host-gateway-ip": "",
  "hosts": [],
  "proxies": {
    "http-proxy": "http://proxy.example.com:80",
    "https-proxy": "https://proxy.example.com:443",
    "no-proxy": "*.test.example.com,.example.org"
  },
  "icc": false,
  "init": false,
  "init-path": "/usr/libexec/docker-init",
  "insecure-registries": [],
  "ip": "0.0.0.0",
  "ip-forward": false,
  "ip-masq": false,
  "iptables": false,
  "ip6tables": false,
  "ipv6": false,
  "labels": [],
  "live-restore": true,
  "log-driver": "json-file",
  "log-format": "text",
  "log-level": "",
  "log-opts": {
    "cache-disabled": "false",
    "cache-max-file": "5",
    "cache-max-size": "20m",
    "cache-compress": "true",
    "env": "os,customer",
    "labels": "somelabel",
    "max-file": "5",
    "max-size": "10m"
  },
  "max-concurrent-downloads": 3,
  "max-concurrent-uploads": 5,
  "max-download-attempts": 5,
  "mtu": 0,
  "no-new-privileges": false,
  "node-generic-resources": [
    "NVIDIA-GPU=UUID1",
    "NVIDIA-GPU=UUID2"
  ],
  "oom-score-adjust": 0,
  "pidfile": "",
  "raw-logs": false,
  "registry-mirrors": [],
  "runtimes": {
    "cc-runtime": {
      "path": "/usr/bin/cc-runtime"
    },
    "custom": {
      "path": "/usr/local/bin/my-runc-replacement",
      "runtimeArgs": [
        "--debug"
      ]
    }
  },
  "seccomp-profile": "",
  "selinux-enabled": false,
  "shutdown-timeout": 15,
  "storage-driver": "",
  "storage-opts": [],
  "swarm-default-advertise-addr": "",
  "tls": true,
  "tlscacert": "",
  "tlscert": "",
  "tlskey": "",
  "tlsverify": true,
  "userland-proxy": false,
  "userland-proxy-path": "/usr/libexec/docker-proxy",
  "userns-remap": ""
}
```

> **Note**
>
> You can't set options in `daemon.json` that have already been set on daemon startup as a flag. On systems that use systemd to start the Docker daemon, `-H` is already set, so you can't use the `hosts` key in `daemon.json` to add listening addresses. See [custom Docker daemon options](https://docs.docker.com/engine/daemon/proxy/#systemd-unit-file) for an example on how to configure the daemon using systemd drop-in files.
>
> ​	不能在 `daemon.json` 中设置已在守护进程启动时作为标志设置的选项。在使用 systemd 启动 Docker 守护进程的系统上，已设置 `-H`，因此不能在 `daemon.json` 中使用 `hosts` 键来添加监听地址。有关如何使用 systemd 附加文件配置守护进程的示例，请参阅 [自定义 Docker 守护进程选项](https://docs.docker.com/engine/daemon/proxy/#systemd-unit-file)。

##### On Windows

The default location of the configuration file on Windows is `%programdata%\docker\config\daemon.json`. Use the `--config-file` flag to specify a non-default location.

​	在 Windows 上，配置文件的默认位置为 `%programdata%\docker\config\daemon.json`。使用 `--config-file` 标志可指定非默认位置。

The following is a full example of the allowed configuration options on Windows:

​	以下是 Windows 上允许的完整配置选项示例：

```json
{
  "allow-nondistributable-artifacts": [],
  "authorization-plugins": [],
  "bridge": "",
  "containerd": "\\\\.\\pipe\\containerd-containerd",
  "containerd-namespace": "docker",
  "containerd-plugins-namespace": "docker-plugins",
  "data-root": "",
  "debug": true,
  "default-network-opts": {},
  "default-runtime": "",
  "default-ulimits": {},
  "dns": [],
  "dns-opts": [],
  "dns-search": [],
  "exec-opts": [],
  "experimental": false,
  "features": {},
  "fixed-cidr": "",
  "group": "",
  "host-gateway-ip": "",
  "hosts": [],
  "insecure-registries": [],
  "labels": [],
  "log-driver": "",
  "log-format": "text",
  "log-level": "",
  "max-concurrent-downloads": 3,
  "max-concurrent-uploads": 5,
  "max-download-attempts": 5,
  "mtu": 0,
  "pidfile": "",
  "raw-logs": false,
  "registry-mirrors": [],
  "shutdown-timeout": 15,
  "storage-driver": "",
  "storage-opts": [],
  "swarm-default-advertise-addr": "",
  "tlscacert": "",
  "tlscert": "",
  "tlskey": "",
  "tlsverify": true
}
```

The `default-runtime` option is by default unset, in which case dockerd automatically detects the runtime. This detection is based on if the `containerd` flag is set.

​	`default-runtime` 选项默认未设置，此时 dockerd 会自动检测运行时。检测依据是否设置了 `containerd` 标志。

Accepted values:

​	接受的值：

- `com.docker.hcsshim.v1` - This is the built-in runtime that Docker has used since Windows supported was first added and uses the v1 HCS API's in Windows.
  - `com.docker.hcsshim.v1` - 这是 Docker 自 Windows 支持以来一直使用的内置运行时，使用 Windows 中的 v1 HCS API。

- `io.containerd.runhcs.v1` - This is uses the containerd `runhcs` shim to run the container and uses the v2 HCS API's in Windows.
  - `io.containerd.runhcs.v1` - 使用 containerd 的 `runhcs` shim 运行容器，使用 Windows 中的 v2 HCS API。


#### Feature options

The optional field `features` in `daemon.json` lets you enable or disable specific daemon features.

​	在 `daemon.json` 中的可选字段 `features` 可让您启用或禁用特定的守护进程功能。

```json
{
  "features": {
    "some-feature": true,
    "some-disabled-feature-enabled-by-default": false
  }
}
```

The list of feature options include:

​	功能选项列表包括：

- `containerd-snapshotter`: when set to `true`, the daemon uses containerd snapshotters instead of the classic storage drivers for storing image and container data. For more information, see [containerd storage]({{< ref "/manuals/DockerEngine/Storage/containerdimagestore" >}}).

  - `containerd-snapshotter`：设置为 `true` 时，守护进程使用 containerd 快照程序代替传统存储驱动程序来存储镜像和容器数据。更多信息请参见 [containerd 存储]({{< ref "/manuals/DockerEngine/Storage/containerdimagestore" >}})。

- `windows-dns-proxy`: when set to `true`, the daemon's internal DNS resolver will forward requests to external servers. Without this, most applications running in the container will still be able to use secondary DNS servers configured in the container itself, but `nslookup` won't be able to resolve external names. The current default is `false`, it will change to `true` in a future release. This option is only allowed on Windows.

  - `windows-dns-proxy`：设置为 `true` 时，守护进程的内部 DNS 解析器将请求转发给外部服务器。若未启用，大多数运行在容器中的应用程序仍然可以使用容器中配置的备用 DNS 服务器，但 `nslookup` 无法解析外部名称。目前的默认值为 `false`，将在未来版本中改为 `true`。此选项仅限于 Windows。

  > **Warning**
  >
  > The `windows-dns-proxy` feature flag will be removed in a future release.
  >
  > `windows-dns-proxy` 功能标志将在未来版本中移除。


#### Configuration reload behavior

Some options can be reconfigured when the daemon is running without requiring to restart the process. The daemon uses the `SIGHUP` signal in Linux to reload, and a global event in Windows with the key `Global\docker-daemon-config-$PID`. You can modify the options in the configuration file, but the daemon still checks for conflicting settings with the specified CLI flags. The daemon fails to reconfigure itself if there are conflicts, but it won't stop execution.

​	某些选项可在守护进程运行时重新配置，而无需重新启动进程。守护进程在 Linux 上使用 `SIGHUP` 信号重新加载，在 Windows 上使用 `Global\docker-daemon-config-$PID` 键进行全局事件触发。您可以修改配置文件中的选项，但守护进程仍会检查与指定 CLI 标志冲突的设置。如果有冲突，守护进程无法重新配置自身，但不会停止运行。

The list of currently supported options that can be reconfigured is this:

​	以下选项目前支持重新配置：

| Option                             | Description                                                  |
| ---------------------------------- | ------------------------------------------------------------ |
| `debug`                            | 切换守护进程的调试模式。 Toggles debug mode of the daemon.   |
| `labels`                           | 用新标签集替换守护进程标签。Replaces the daemon labels with a new set of labels. |
| `live-restore`                     | 切换 [实时恢复](https://docs.docker.com/engine/containers/live-restore/) 功能。Toggles [live restore](https://docs.docker.com/engine/containers/live-restore/). |
| `max-concurrent-downloads`         | 配置每次拉取的最大并发下载数。Configures the max concurrent downloads for each pull. |
| `max-concurrent-uploads`           | 配置每次推送的最大并发上传数。Configures the max concurrent uploads for each push. |
| `max-download-attempts`            | 配置每次拉取的最大下载尝试次数。Configures the max download attempts for each pull. |
| `default-runtime`                  | 配置在容器创建时未指定的默认运行时。Configures the runtime to be used if not is specified at container creation. |
| `runtimes`                         | 配置可用于运行容器的可用 OCI 运行时列表。Configures the list of available OCI runtimes that can be used to run containers. |
| `authorization-plugin`             | 指定使用的授权插件。Specifies the authorization plugins to use. |
| `allow-nondistributable-artifacts` | 指定守护进程将推送非分发性工件的注册表列表。Specifies a list of registries to which the daemon will push non-distributable artifacts. |
| `insecure-registries`              | 指定守护进程应视为不安全的注册表列表。Specifies a list of registries that the daemon should consider insecure. |
| `registry-mirrors`                 | 指定注册表镜像列表。Specifies a list of registry mirrors.    |
| `shutdown-timeout`                 | 配置守护进程关闭所有容器的超时。Configures the daemon's existing configuration timeout with a new timeout for shutting down all containers. |
| `features`                         | 启用或禁用特定功能。Enables or disables specific features.   |

### 运行多个守护进程 Run multiple daemons

> **Note**
>
> Running multiple daemons on a single host is considered experimental. You may encounter unsolved problems, and things may not work as expected in some cases.
>
> ​	在单个主机上运行多个守护进程属于实验性功能。您可能会遇到未解决的问题，某些情况可能无法正常工作。

This section describes how to run multiple Docker daemons on a single host. To run multiple daemons, you must configure each daemon so that it doesn't conflict with other daemons on the same host. You can set these options either by providing them as flags, or by using a [daemon configuration file](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file).

​	本节描述如何在单个主机上运行多个 Docker 守护进程。要运行多个守护进程，必须配置每个守护进程，以便它们不会与同一主机上的其他守护进程发生冲突。您可以通过标志或 [守护进程配置文件](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file)来设置这些选项。

The following daemon options must be configured for each daemon:

​	每个守护进程必须配置以下守护进程选项：



```text
-b, --bridge=                          将容器附加到网络桥 Attach containers to a network bridge
--exec-root=/var/run/docker            Docker execdriver 的根目录 Root of the Docker execdriver
--data-root=/var/lib/docker            Docker 持久化数据的根目录 Root of persisted Docker data
-p, --pidfile=/var/run/docker.pid      守护进程 PID 文件的路径 Path to use for daemon PID file
-H, --host=[]                          要连接的守护进程套接字 Daemon socket(s) to connect to
--iptables=true                        启用添加 iptables 规则 Enable addition of iptables rules
--config-file=/etc/docker/daemon.json  守护进程配置文件 Daemon configuration file
--tlscacert="~/.docker/ca.pem"         仅信任由此 CA 签名的证书 Trust certs signed only by this CA
--tlscert="~/.docker/cert.pem"         TLS 证书文件路径 Path to TLS certificate file
--tlskey="~/.docker/key.pem"           TLS 密钥文件路径 Path to TLS key file
```

When your daemons use different values for these flags, you can run them on the same host without any problems. It is important that you understand the meaning of these options and to use them correctly.

​	如果您的守护进程使用这些标志的不同值，则可以在同一主机上运行它们而不会出现问题。了解这些选项的含义并正确使用它们非常重要。

- The `-b, --bridge=` flag is set to `docker0` as default bridge network. It is created automatically when you install Docker. If you aren't using the default, you must create and configure the bridge manually, or set it to 'none': `--bridge=none`
  - `-b, --bridge=` 标志默认设置为 `docker0` 作为默认桥接网络。安装 Docker 时会自动创建它。如果您不使用默认桥，必须手动创建和配置桥，或将其设置为“none”：`--bridge=none`

- `--exec-root` is the path where the container state is stored. The default value is `/var/run/docker`. Specify the path for your running daemon here.
  - `--exec-root` 是存储容器状态的路径。默认值为 `/var/run/docker`。在此处指定运行守护进程的路径。

- `--data-root` is the path where persisted data such as images, volumes, and cluster state are stored. The default value is `/var/lib/docker`. To avoid any conflict with other daemons, set this parameter separately for each daemon.
  - `--data-root` 是存储持久化数据（如镜像、卷和集群状态）的路径。默认值为 `/var/lib/docker`。为避免与其他守护进程冲突，需为每个守护进程分别设置该参数。

- `-p, --pidfile=/var/run/docker.pid` is the path where the process ID of the daemon is stored. Specify the path for your PID file here.
  - `-p, --pidfile=/var/run/docker.pid` 是存储守护进程进程 ID 的路径。指定 PID 文件的路径。

- `--host=[]` specifies where the Docker daemon listens for client connections. If unspecified, it defaults to `/var/run/docker.sock`.
  - `--host=[]` 指定 Docker 守护进程侦听客户端连接的位置。若未指定，默认为 `/var/run/docker.sock`。

- `--iptables=false` prevents the Docker daemon from adding iptables rules. If multiple daemons manage iptables rules, they may overwrite rules set by another daemon. Be aware that disabling this option requires you to manually add iptables rules to expose container ports. If you prevent Docker from adding iptables rules, Docker also doesn't add IP masquerading rules, even if you set `--ip-masq` to `true`. Without IP masquerading rules, Docker containers can't connect to external hosts or the internet when using network other than default bridge.
  - `--iptables=false` 防止 Docker 守护进程添加 iptables 规则。如果多个守护进程管理 iptables 规则，可能会覆盖其他守护进程设置的规则。请注意，禁用此选项需要手动添加 iptables 规则来暴露容器端口。如果禁止 Docker 添加 iptables 规则，即使将 `--ip-masq` 设置为 `true`，Docker 也不会添加 IP 伪装规则。没有 IP 伪装规则，使用非默认桥接网络的 Docker 容器无法连接到外部主机或互联网。

- `--config-file=/etc/docker/daemon.json` is the path where configuration file is stored. You can use it instead of daemon flags. Specify the path for each daemon.
  - `--config-file=/etc/docker/daemon.json` 是配置文件的存储路径。可以用它代替守护进程标志。为每个守护进程指定路径。

- `--tls*` Docker daemon supports `--tlsverify` mode that enforces encrypted and authenticated remote connections. The `--tls*` options enable use of specific certificates for individual daemons.
  - `--tls*` Docker 守护进程支持 `--tlsverify` 模式，该模式强制加密和认证的远程连接。`--tls*` 选项允许为各个守护进程使用特定的证书。


Example script for a separate “bootstrap” instance of the Docker daemon without network:

​	示例脚本用于没有网络的单独“bootstrap” Docker 守护进程实例：



```console
$ sudo dockerd \
        -H unix:///var/run/docker-bootstrap.sock \
        -p /var/run/docker-bootstrap.pid \
        --iptables=false \
        --ip-masq=false \
        --bridge=none \
        --data-root=/var/lib/docker-bootstrap \
        --exec-root=/var/run/docker-bootstrap
```

### 默认网络选项 Default network options

The `default-network-opts` key in the `daemon.json` configuration file, and the equivalent `--default-network-opt` CLI flag, let you specify default values for driver network driver options for new networks.

​	在 `daemon.json` 配置文件中的 `default-network-opts` 键和等效的 `--default-network-opt` CLI 标志允许您为新网络指定网络驱动程序选项的默认值。

The following example shows how to configure options for the `bridge` driver using the `daemon.json` file.

​	以下示例展示了如何使用 `daemon.json` 文件为 `bridge` 驱动程序配置选项。



```json
{
  "default-network-opts": {
    "bridge": {
      "com.docker.network.bridge.host_binding_ipv4": "127.0.0.1",
      "com.docker.network.driver.mtu": "1234"
    }
  }
}
```

This example uses the `bridge` network driver. Refer to the [bridge network driver page](https://docs.docker.com/engine/network/drivers/bridge/#options) for an overview of available driver options.

​	此示例使用 `bridge` 网络驱动程序。有关可用驱动程序选项的概述，请参见 [桥接网络驱动程序页面](https://docs.docker.com/engine/network/drivers/bridge/#options)。

After changing the configuration and restarting the daemon, new networks that you create use these option configurations as defaults.

​	更改配置并重新启动守护进程后，您创建的新网络将使用这些选项配置作为默认值。



```console
$ docker network create mynet
$ docker network inspect mynet --format "{{json .Options}}"
{"com.docker.network.bridge.host_binding_ipv4":"127.0.0.1","com.docker.network.driver.mtu":"1234"}
```

Note that changing this daemon configuration doesn't affect pre-existing networks.

​	请注意，更改此守护进程配置不会影响现有网络。

Using the `--default-network-opt` CLI flag is useful for testing and debugging purposes, but you should prefer using the `daemon.json` file for persistent daemon configuration. The CLI flag expects a value with the following format: `driver=opt=value`, for example:

​	使用 `--default-network-opt` CLI 标志对于测试和调试非常有用，但为了持久的守护进程配置，建议使用 `daemon.json` 文件。CLI 标志期望值格式为：`driver=opt=value`，例如：



```console
$ sudo dockerd \
  --default-network-opt bridge=com.docker.network.bridge.host_binding_ipv4=127.0.0.1 \
  --default-network-opt bridge=com.docker.network.driver.mtu=1234
```
