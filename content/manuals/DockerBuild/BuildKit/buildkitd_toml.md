+++
title = "buildkitd.toml"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/build/buildkit/toml-configuration/](https://docs.docker.com/build/buildkit/toml-configuration/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# buildkitd.toml

The TOML file used to configure the buildkitd daemon settings has a short list of global settings followed by a series of sections for specific areas of daemon configuration.

​	`buildkitd.toml` 文件用于配置 buildkitd 守护进程的设置。它包括一小部分全局设置，后跟若干用于特定守护进程配置区域的部分。

The file path is `/etc/buildkit/buildkitd.toml` for rootful mode, `~/.config/buildkit/buildkitd.toml` for rootless mode.

​	文件路径在 root 模式下为 `/etc/buildkit/buildkitd.toml`，在 rootless 模式下为 `~/.config/buildkit/buildkitd.toml`。

The following is a complete `buildkitd.toml` configuration example. Note that some configuration options are only useful in edge cases.

​	以下是一个完整的 `buildkitd.toml` 配置示例。请注意，某些配置选项仅在特定情况下有用。

```toml
# debug enables additional debug logging
# debug 启用额外的调试日志
debug = true
# trace enables additional trace logging (very verbose, with potential performance impacts)
# trace 启用额外的跟踪日志（非常详细，可能影响性能）
trace = true
# root is where all buildkit state is stored.
# root 是存储所有 buildkit 状态的目录
root = "/var/lib/buildkit"
# insecure-entitlements allows insecure entitlements, disabled by default.
# insecure-entitlements 允许不安全的授权，默认禁用
insecure-entitlements = [ "network.host", "security.insecure" ]

[log]
  # log formatter: json or text
  # 日志格式：json 或 text
  format = "text"

[dns]
  nameservers=["1.1.1.1","8.8.8.8"]
  options=["edns0"]
  searchDomains=["example.com"]

[grpc]
  address = [ "tcp://0.0.0.0:1234" ]
  # debugAddress is address for attaching go profiles and debuggers.
  # debugAddress 是用于连接 Go 程序的调试地址
  debugAddress = "0.0.0.0:6060"
  uid = 0
  gid = 0
  [grpc.tls]
    cert = "/etc/buildkit/tls.crt"
    key = "/etc/buildkit/tls.key"
    ca = "/etc/buildkit/tlsca.crt"

[otel]
  # OTEL collector trace socket path
  # OTEL 收集器跟踪套接字路径
  socketPath = "/run/buildkit/otel-grpc.sock"

# config for build history API that stores information about completed build commands
# 配置用于存储已完成构建命令信息的历史 API
[history]
  # maxAge is the maximum age of history entries to keep, in seconds.
  # maxAge 是保留历史记录条目的最大年龄，以秒为单位
  maxAge = 172800
  # maxEntries is the maximum number of history entries to keep.
  # maxEntries 是保留历史记录条目的最大数量
  maxEntries = 50

[worker.oci]
  enabled = true
  # platforms is manually configure platforms, detected automatically if unset.
  # platforms 手动配置平台，未设置时自动检测
  platforms = [ "linux/amd64", "linux/arm64" ]
  snapshotter = "auto" # overlayfs or native, default value is "auto". overlayfs 或 native，默认值为 "auto"
  rootless = false # see docs/rootless.md for the details on rootless mode. 有关 rootless 模式的详细信息，请参见 docs/rootless.md
  # Whether run subprocesses in main pid namespace or not, this is useful for
  # running rootless buildkit inside a container.
  # 是否在主 PID 命名空间中运行子进程，这对于在容器内运行无根 buildkit 有用
  noProcessSandbox = false
  gc = true
  # gckeepstorage can be an integer number of bytes (e.g. 512000000), a string
  # with a unit (e.g. "512MB"), or a string percentage of the total disk
  # space (e.g. "10%")
  # gckeepstorage 可以是整数字节数（如 512000000）、带单位的字符串（如 "512MB"）或总磁盘空间的百分比字符串（如 "10%"）
  gckeepstorage = 9000
  # alternate OCI worker binary name(example 'crun'), by default either 
  # buildkit-runc or runc binary is used
  # 替代的 OCI 工作器二进制名称（例如 'crun'），默认使用 buildkit-runc 或 runc 二进制
  binary = ""
  # name of the apparmor profile that should be used to constrain build containers.
  # the profile should already be loaded (by a higher level system) before creating a worker.
  # 要用于约束构建容器的 apparmor 配置文件的名称。
  # 该配置文件应在创建 worker 之前由更高级别的系统加载。
  apparmor-profile = ""
  # limit the number of parallel build steps that can run at the same time
  # 限制同时运行的并行构建步骤的数量
  max-parallelism = 4
  # maintain a pool of reusable CNI network namespaces to amortize the overhead
  # of allocating and releasing the namespaces
  # 维护一个可重用的 CNI 网络命名空间池，以减少分配和释放命名空间的开销
  cniPoolSize = 16

  [worker.oci.labels]
    "foo" = "bar"

  [[worker.oci.gcpolicy]]
    # keepBytes can be an integer number of bytes (e.g. 512000000), a string
    # with a unit (e.g. "512MB"), or a string percentage of the total disk
    # space (e.g. "10%")
    # keepBytes 可以是一个整数（例如 512000000）、带单位的字符串（例如 "512MB"）或总磁盘空间的百分比字符串（例如 "10%"）
    keepBytes = "512MB"
    # keepDuration can be an integer number of seconds (e.g. 172800), or a
    # string duration (e.g. "48h")
    # keepDuration 可以是一个整数秒数（例如 172800），或一个字符串表示的时长（例如 "48h"）
    keepDuration = "48h"
    filters = [ "type==source.local", "type==exec.cachemount", "type==source.git.checkout"]
  [[worker.oci.gcpolicy]]
    all = true
    keepBytes = 1024000000

[worker.containerd]
  address = "/run/containerd/containerd.sock"
  enabled = true
  platforms = [ "linux/amd64", "linux/arm64" ]
  namespace = "buildkit"
  gc = true
  # gckeepstorage sets storage limit for default gc profile, in bytes.
  # gckeepstorage 为默认 gc 配置文件设置存储限制，单位为字节。
  gckeepstorage = 9000
  # maintain a pool of reusable CNI network namespaces to amortize the overhead
  # of allocating and releasing the namespaces
  # 维护一个可重用的 CNI 网络命名空间池，以减少分配和释放命名空间的开销
  cniPoolSize = 16
  # defaultCgroupParent sets the parent cgroup of all containers.
  # defaultCgroupParent 设置所有容器的父 cgroup。
  defaultCgroupParent = "buildkit"

  [worker.containerd.labels]
    "foo" = "bar"

  # configure the containerd runtime
  # 配置 containerd 运行时
  [worker.containerd.runtime]
    name = "io.containerd.runc.v2"
    path = "/path/to/containerd/runc/shim"
    options = { BinaryName = "runc" }

  [[worker.containerd.gcpolicy]]
    keepBytes = 512000000
    keepDuration = 172800
    filters = [ "type==source.local", "type==exec.cachemount", "type==source.git.checkout"]
  [[worker.containerd.gcpolicy]]
    all = true
    keepBytes = 1024000000

# registry configures a new Docker register used for cache import or output.
# registry 配置一个新的 Docker 注册表，用于缓存导入或输出。
[registry."docker.io"]
  # mirror configuration to handle path in case a mirror registry requires a /project path rather than just a host:port
  # 镜像配置以处理路径，如果镜像注册表需要 /project 路径而非仅主机:端口
  mirrors = ["yourmirror.local:5000", "core.harbor.domain/proxy.docker.io"]
  http = true
  insecure = true
  ca=["/etc/config/myca.pem"]
  [[registry."docker.io".keypair]]
    key="/etc/config/key.pem"
    cert="/etc/config/cert.pem"

# optionally mirror configuration can be done by defining it as a registry.
# 可以通过定义 registry 来完成镜像配置。
[registry."yourmirror.local:5000"]
  http = true

# Frontend control
# 前端控制
[frontend."dockerfile.v0"]
  enabled = true

[frontend."gateway.v0"]
  enabled = true

  # If allowedRepositories is empty, all gateway sources are allowed.
  # Otherwise, only the listed repositories are allowed as a gateway source.
  # 
  # NOTE: Only the repository name (without tag) is compared.
  #
  # Example:
  # allowedRepositories = [ "docker-registry.wikimedia.org/repos/releng/blubber/buildkit" ]
  # 如果 allowedRepositories 为空，则允许所有网关源。
  # 否则，仅允许列表中的仓库作为网关源。
  # 
  # 注意：仅比较仓库名称（不带标签）。
  #
  # 示例：
  # allowedRepositories = [ "docker-registry.wikimedia.org/repos/releng/blubber/buildkit" ]
  allowedRepositories = []

[system]
  # how often buildkit scans for changes in the supported emulated platforms
  # buildkit 扫描支持的仿真平台变更的频率
  platformsCacheMaxAge = "1h"
```
