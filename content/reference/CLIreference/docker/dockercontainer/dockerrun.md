+++
title = "docker run"
date = 2024-10-23T14:54:43+08:00
weight = 240
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/run/](https://docs.docker.com/reference/cli/docker/container/run/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container run

| Description | Create and run a new container from an image              |
| :---------- | --------------------------------------------------------- |
| Usage       | `docker container run [OPTIONS] IMAGE [COMMAND] [ARG...]` |
| Aliases     | `docker run`                                              |

## Description

The `docker run` command runs a command in a new container, pulling the image if needed and starting the container.

​	`docker run` 命令在一个新容器中运行命令，如果需要会拉取镜像并启动容器。

You can restart a stopped container with all its previous changes intact using `docker start`. Use `docker ps -a` to view a list of all containers, including those that are stopped.

​	你可以使用 `docker start` 重新启动一个已停止的容器，并保留其之前的所有更改。使用 `docker ps -a` 可以查看所有容器的列表，包括那些已停止的容器。

## Options

| Option                                                       | Default   | Description                                                  |
| ------------------------------------------------------------ | --------- | ------------------------------------------------------------ |
| [`--add-host`](https://docs.docker.com/reference/cli/docker/container/run/#add-host) |           | 添加自定义的主机到IP映射 (host:ip) Add a custom host-to-IP mapping (host:ip) |
| `--annotation`                                               |           | API 1.43+ 为容器添加注释（传递给OCI运行时） API 1.43+ Add an annotation to the container (passed through to the OCI runtime) |
| [`-a, --attach`](https://docs.docker.com/reference/cli/docker/container/run/#attach) |           | 附加到STDIN、STDOUT或STDERR Attach to STDIN, STDOUT or STDERR |
| `--blkio-weight`                                             |           | 块IO（相对权重），在10到1000之间，或0禁用（默认0） Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0) |
| `--blkio-weight-device`                                      |           | 块IO权重（相对设备权重） Block IO weight (relative device weight) |
| `--cap-add`                                                  |           | 添加Linux能力 Add Linux capabilities                         |
| `--cap-drop`                                                 |           | 删除Linux能力 Drop Linux capabilities                        |
| [`--cgroup-parent`](https://docs.docker.com/reference/cli/docker/container/run/#cgroup-parent) |           | 容器的可选父cgroup Optional parent cgroup for the container  |
| `--cgroupns`                                                 |           | API 1.41+ Cgroup namespace to use (host\|private) <br />API 1.41+ 使用的cgroup命名空间<br />'host': Run the container in the Docker host's cgroup namespace<br />'host'：在Docker主机的cgroup命名空间中运行容器；<br /> 'private': Run the container in its own private cgroup namespace <br />'private'：在其自己的私有cgroup命名空间中运行容器；<br />'': Use the cgroup namespace as configured by the default-cgroupns-mode option on the daemon (default)<br />''：使用守护进程上默认cgroupns模式选项配置的cgroup命名空间 |
| [`--cidfile`](https://docs.docker.com/reference/cli/docker/container/run/#cidfile) |           | 将容器ID写入文件 Write the container ID to the file          |
| `--cpu-count`                                                |           | CPU数量（仅适用于Windows） CPU count (Windows only)          |
| `--cpu-percent`                                              |           | CPU百分比（仅适用于Windows） CPU percent (Windows only)      |
| `--cpu-period`                                               |           | 限制CPU CFS（完全公平调度器）周期 Limit CPU CFS (Completely Fair Scheduler) period |
| `--cpu-quota`                                                |           | 限制CPU CFS（完全公平调度器）配额 Limit CPU CFS (Completely Fair Scheduler) quota |
| `--cpu-rt-period`                                            |           | API 1.25+ 限制CPU实时周期（微秒） API 1.25+ Limit CPU real-time period in microseconds |
| `--cpu-rt-runtime`                                           |           | API 1.25+ 限制CPU实时运行时（微秒） API 1.25+ Limit CPU real-time runtime in microseconds |
| `-c, --cpu-shares`                                           |           | CPU份额（相对权重） CPU shares (relative weight)             |
| `--cpus`                                                     |           | API 1.25+ CPU数量 API 1.25+ Number of CPUs                   |
| `--cpuset-cpus`                                              |           | 允许执行的CPU（0-3，0,1） CPUs in which to allow execution (0-3, 0,1) |
| `--cpuset-mems`                                              |           | 允许执行的内存（0-3，0,1） MEMs in which to allow execution (0-3, 0,1) |
| [`-d, --detach`](https://docs.docker.com/reference/cli/docker/container/run/#detach) |           | 在后台运行容器并打印容器ID - Run container in background and print container ID |
| [`--detach-keys`](https://docs.docker.com/reference/cli/docker/container/run/#detach-keys) |           | 重写分离容器的按键序列 Override the key sequence for detaching a container |
| [`--device`](https://docs.docker.com/reference/cli/docker/container/run/#device) |           | 向容器添加主机设备 Add a host device to the container        |
| [`--device-cgroup-rule`](https://docs.docker.com/reference/cli/docker/container/run/#device-cgroup-rule) |           | 向cgroup允许设备列表添加规则 Add a rule to the cgroup allowed devices list |
| `--device-read-bps`                                          |           | 限制设备的读取速率（每秒字节数） Limit read rate (bytes per second) from a device |
| `--device-read-iops`                                         |           | 限制设备的读取速率（每秒IO） Limit read rate (IO per second) from a device |
| `--device-write-bps`                                         |           | 限制设备的写入速率（每秒字节数） Limit write rate (bytes per second) to a device |
| `--device-write-iops`                                        |           | 限制设备的写入速率（每秒IO） Limit write rate (IO per second) to a device |
| `--disable-content-trust`                                    | `true`    | 跳过镜像验证 Skip image verification                         |
| `--dns`                                                      |           | 设置自定义DNS服务器 Set custom DNS servers                   |
| `--dns-option`                                               |           | 设置DNS选项 Set DNS options                                  |
| `--dns-search`                                               |           | 设置自定义DNS搜索域 Set custom DNS search domains            |
| `--domainname`                                               |           | 容器的NIS域名 Container NIS domain name                      |
| `--entrypoint`                                               |           | 重写镜像的默认ENTRYPOINT - Overwrite the default ENTRYPOINT of the image |
| [`-e, --env`](https://docs.docker.com/reference/cli/docker/container/run/#env) |           | 设置环境变量 Set environment variables                       |
| `--env-file`                                                 |           | 从文件中读取环境变量 Read in a file of environment variables |
| `--expose`                                                   |           | 暴露一个端口或端口范围 Expose a port or a range of ports     |
| [`--gpus`](https://docs.docker.com/reference/cli/docker/container/run/#gpus) |           | API 1.40+ 要添加到容器的GPU设备（'all'传递所有GPU） API 1.40+ GPU devices to add to the container ('all' to pass all GPUs) |
| `--group-add`                                                |           | 添加额外的组加入 Add additional groups to join               |
| `--health-cmd`                                               |           | 检查健康的命令 Command to run to check health                |
| `--health-interval`                                          |           | 运行检查之间的时间 (ms\|s\|m\|h) (default 0s)  Time between running the check (ms\|s\|m\|h) (default 0s) |
| `--health-retries`                                           |           | 报告不健康所需的连续失败次数 Consecutive failures needed to report unhealthy |
| `--health-start-interval`                                    |           | API 1.44+ 在启动期间运行检查之间的时间 API 1.44+ Time between running the check during the start period (ms\|s\|m\|h) (default 0s) |
| `--health-start-period`                                      |           | API 1.29+ 容器初始化的开始周期，在启动健康检查计数倒计时之前 (ms\|s\|m\|h) (default 0s)  API 1.29+ Start period for the container to initialize before starting health-retries countdown (ms\|s\|m\|h) (default 0s) |
| `--health-timeout`                                           |           | 允许单次检查运行的最长时间 (ms\|s\|m\|h) (default 0s) Maximum time to allow one check to run (ms\|s\|m\|h) (default 0s) |
| `--help`                                                     |           | 打印用法 Print usage                                         |
| `-h, --hostname`                                             |           | 容器主机名 Container host name                               |
| [`--init`](https://docs.docker.com/reference/cli/docker/container/run/#init) |           | API 1.25+ 在容器内运行一个init进程，转发信号并清理进程 API 1.25+ Run an init inside the container that forwards signals and reaps processes |
| [`-i, --interactive`](https://docs.docker.com/reference/cli/docker/container/run/#interactive) |           | 保持STDIN开放，即使未附加 Keep STDIN open even if not attached |
| `--io-maxbandwidth`                                          |           | 系统驱动的最大IO带宽限制（仅适用于Windows） Maximum IO bandwidth limit for the system drive (Windows only) |
| `--io-maxiops`                                               |           | 系统驱动的最大IOps限制（仅适用于Windows） Maximum IOps limit for the system drive (Windows only) |
| `--ip`                                                       |           | IPv4地址（例如，172.30.100.104） IPv4 address (e.g., 172.30.100.104) |
| `--ip6`                                                      |           | IPv6地址（例如，2001:db8::33） IPv6 address (e.g., 2001:db8::33) |
| [`--ipc`](https://docs.docker.com/reference/cli/docker/container/run/#ipc) |           | 使用的IPC模式 IPC mode to use                                |
| [`--isolation`](https://docs.docker.com/reference/cli/docker/container/run/#isolation) |           | 容器隔离技术 Container isolation technology                  |
| `--kernel-memory`                                            |           | 内核内存限制 Kernel memory limit                             |
| [`-l, --label`](https://docs.docker.com/reference/cli/docker/container/run/#label) |           | 设置容器的元数据 Set meta data on a container                |
| `--label-file`                                               |           | 从以行分隔的标签文件中读取 Read in a line delimited file of labels |
| `--link`                                                     |           | 添加链接到另一个容器 Add link to another container           |
| `--link-local-ip`                                            |           | 容器的IPv4/IPv6链路本地地址 Container IPv4/IPv6 link-local addresses |
| [`--log-driver`](https://docs.docker.com/reference/cli/docker/container/run/#log-driver) |           | 容器的日志驱动 Logging driver for the container              |
| `--log-opt`                                                  |           | 日志驱动选项 Log driver options                              |
| `--mac-address`                                              |           | 容器的MAC地址（例如，92:d0:c6:0a:29:33） Container MAC address (e.g., 92:d0:c6:0a:29:33) |
| [`-m, --memory`](https://docs.docker.com/reference/cli/docker/container/run/#memory) |           | 内存限制 Memory limit                                        |
| `--memory-reservation`                                       |           | 内存软限制 Memory soft limit                                 |
| `--memory-swap`                                              |           | 交换限制等于内存加交换：'-1'以启用无限交换 Swap limit equal to memory plus swap: '-1' to enable unlimited swap |
| `--memory-swappiness`                                        | `-1`      | 调整容器内存交换行为（-1为禁用） Tune container memory swappiness (0 to 100) |
| [`--mount`](https://docs.docker.com/reference/cli/docker/container/run/#mount) |           | 将文件系统挂载到容器 Attach a filesystem mount to the container |
| [`--name`](https://docs.docker.com/reference/cli/docker/container/run/#name) |           | 为容器分配一个名称 Assign a name to the container            |
| [`--network`](https://docs.docker.com/reference/cli/docker/container/run/#network) |           | 将容器连接到一个网络 Connect a container to a network        |
| `--network-alias`                                            |           | 为容器添加网络范围的别名 Add network-scoped alias for the container |
| `--no-healthcheck`                                           |           | 禁用任何容器指定的健康检查 Disable any container-specified HEALTHCHECK |
| `--oom-kill-disable`                                         |           | 禁用OOM杀手 Disable OOM Killer                               |
| `--oom-score-adj`                                            |           | 调整主机的OOM偏好（-1000到1000） Tune host's OOM preferences (-1000 to 1000) |
| [`--pid`](https://docs.docker.com/reference/cli/docker/container/run/#pid) |           | 使用的PID命名空间 PID namespace to use                       |
| `--pids-limit`                                               |           | 调整容器的PID限制（设置为-1表示无限） Tune container pids limit (set -1 for unlimited) |
| `--platform`                                                 |           | API 1.32+ 如果服务器支持多平台，设置平台 API 1.32+ Set platform if server is multi-platform capable |
| [`--privileged`](https://docs.docker.com/reference/cli/docker/container/run/#privileged) |           | 给予此容器扩展权限 Give extended privileges to this container |
| [`-p, --publish`](https://docs.docker.com/reference/cli/docker/container/run/#publish) |           | 将容器的端口发布到主机 Publish a container's port(s) to the host |
| [`-P, --publish-all`](https://docs.docker.com/reference/cli/docker/container/run/#publish-all) |           | 将所有暴露的端口发布到随机端口 Publish all exposed ports to random ports |
| [`--pull`](https://docs.docker.com/reference/cli/docker/container/run/#pull) | `missing` | 运行前拉取镜像（`always`、`missing`、`never`） Pull image before running (`always`, `missing`, `never`) |
| `-q, --quiet`                                                |           | 抑制拉取输出 Suppress the pull output                        |
| [`--read-only`](https://docs.docker.com/reference/cli/docker/container/run/#read-only) |           | 将容器的根文件系统挂载为只读 Mount the container's root filesystem as read only |
| [`--restart`](https://docs.docker.com/reference/cli/docker/container/run/#restart) | `no`      | 当容器退出时应用的重启策略 Restart policy to apply when a container exits |
| [`--rm`](https://docs.docker.com/reference/cli/docker/container/run/#rm) |           | 容器退出时自动删除容器及其关联的匿名卷 Automatically remove the container and its associated anonymous volumes when it exits |
| `--runtime`                                                  |           | 该容器使用的运行时 Runtime to use for this container         |
| [`--security-opt`](https://docs.docker.com/reference/cli/docker/container/run/#security-opt) |           | 安全选项 Security Options                                    |
| `--shm-size`                                                 |           | /dev/shm的大小 Size of /dev/shm                              |
| `--sig-proxy`                                                | `true`    | 将接收到的信号代理到进程 Proxy received signals to the process |
| [`--stop-signal`](https://docs.docker.com/reference/cli/docker/container/run/#stop-signal) |           | 停止容器的信号 Signal to stop the container                  |
| [`--stop-timeout`](https://docs.docker.com/reference/cli/docker/container/run/#stop-timeout) |           | API 1.25+ 停止容器的超时时间（以秒为单位） API 1.25+ Timeout (in seconds) to stop a container |
| [`--storage-opt`](https://docs.docker.com/reference/cli/docker/container/run/#storage-opt) |           | 容器的存储驱动选项 Storage driver options for the container  |
| [`--sysctl`](https://docs.docker.com/reference/cli/docker/container/run/#sysctl) |           | Sysctl选项 Sysctl options                                    |
| [`--tmpfs`](https://docs.docker.com/reference/cli/docker/container/run/#tmpfs) |           | 挂载一个tmpfs目录 Mount a tmpfs directory                    |
| [`-t, --tty`](https://docs.docker.com/reference/cli/docker/container/run/#tty) |           | 分配一个伪终端 Allocate a pseudo-TTY                         |
| [`--ulimit`](https://docs.docker.com/reference/cli/docker/container/run/#ulimit) |           | Ulimit选项 Ulimit options                                    |
| `-u, --user`                                                 |           | 用户名或UID -  Username or UID (format: <name\|uid>[:<group\|gid>]) |
| [`--userns`](https://docs.docker.com/reference/cli/docker/container/run/#userns) |           | 使用的用户命名空间 User namespace to use                     |
| [`--uts`](https://docs.docker.com/reference/cli/docker/container/run/#uts) |           | 使用的UTS命名空间 UTS namespace to use                       |
| [`-v, --volume`](https://docs.docker.com/reference/cli/docker/container/run/#volume) |           | 绑定挂载一个卷 Bind mount a volume                           |
| `--volume-driver`                                            |           | 容器的可选卷驱动程序 Optional volume driver for the container |
| [`--volumes-from`](https://docs.docker.com/reference/cli/docker/container/run/#volumes-from) |           | 从指定容器挂载卷 Mount volumes from the specified container(s) |
| [`-w, --workdir`](https://docs.docker.com/reference/cli/docker/container/run/#workdir) |           | 容器内的工作目录 Working directory inside the container      |

## Examples

### 指定名称（`--name`） Assign name (`--name`)

The `--name` flag lets you specify a custom identifier for a container. The following example runs a container named `test` using the `nginx:alpine` image in [detached mode](https://docs.docker.com/reference/cli/docker/container/run/#detach).

​	`--name` 标志让你为容器指定一个自定义标识符。以下示例运行一个名为 `test` 的容器，使用 `nginx:alpine` 镜像以[分离模式](https://docs.docker.com/reference/cli/docker/container/run/#detach)运行。



```console
$ docker run --name test -d nginx:alpine
4bed76d3ad428b889c56c1ecc2bf2ed95cb08256db22dc5ef5863e1d03252a19
$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED        STATUS                  PORTS     NAMES
4bed76d3ad42   nginx:alpine   "/docker-entrypoint.…"   1 second ago   Up Less than a second   80/tcp    test
```

You can reference the container by name with other commands. For example, the following commands stop and remove a container named `test`:

​	你可以通过名称引用容器与其他命令。例如，以下命令停止并删除名为 `test` 的容器：

```console
$ docker stop test
test
$ docker rm test
test
```

If you don't specify a custom name using the `--name` flag, the daemon assigns a randomly generated name, such as `vibrant_cannon`, to the container. Using a custom-defined name provides the benefit of having an easy-to-remember ID for a container.

​	如果您没有使用 `--name` 标志指定自定义名称，守护进程会为容器分配一个随机生成的名称，如 `vibrant_cannon`。使用自定义定义的名称可以为容器提供一个易于记忆的标识符。

Moreover, if you connect the container to a user-defined bridge network, other containers on the same network can refer to the container by name via DNS.

​	此外，如果您将容器连接到用户定义的桥接网络，同一网络上的其他容器可以通过DNS按名称引用该容器。



```console
$ docker network create mynet
cb79f45948d87e389e12013fa4d969689ed2c3316985dd832a43aaec9a0fe394
$ docker run --name test --net mynet -d nginx:alpine
58df6ecfbc2ad7c42d088ed028d367f9e22a5f834d7c74c66c0ab0485626c32a
$ docker run --net mynet busybox:latest ping test
PING test (172.18.0.2): 56 data bytes
64 bytes from 172.18.0.2: seq=0 ttl=64 time=0.073 ms
64 bytes from 172.18.0.2: seq=1 ttl=64 time=0.411 ms
64 bytes from 172.18.0.2: seq=2 ttl=64 time=0.319 ms
64 bytes from 172.18.0.2: seq=3 ttl=64 time=0.383 ms
...
```

### 捕获容器ID (`--cidfile`) Capture container ID (`--cidfile`)

To help with automation, you can have Docker write the container ID out to a file of your choosing. This is similar to how some programs might write out their process ID to a file (you might've seen them as PID files):

​	为了帮助自动化，您可以让Docker将容器ID写入您选择的文件。这类似于一些程序可能将其进程ID写入文件（您可能见过它们作为PID文件）：



```console
$ docker run --cidfile /tmp/docker_test.cid ubuntu echo "test"
```

This creates a container and prints `test` to the console. The `cidfile` flag makes Docker attempt to create a new file and write the container ID to it. If the file exists already, Docker returns an error. Docker closes this file when `docker run` exits.

​	这将创建一个容器并将 `test` 输出到控制台。`cidfile` 标志使Docker尝试创建一个新文件并将容器ID写入其中。如果该文件已经存在，Docker将返回错误。Docker在 `docker run` 退出时关闭此文件。

### PID设置 (`--pid`)  PID settings (`--pid`)



```text
--pid=""  : Set the PID (Process) Namespace mode for the container,  设置容器的PID（进程）命名空间模式，
             'container:<name|id>': joins another container's PID namespace
             'host': use the host's PID namespace inside the container
             'container:<name|id>'：加入另一个容器的PID命名空间
             'host'：在容器内使用主机的PID命名空间
```

By default, all containers have the PID namespace enabled.

​	默认情况下，所有容器都有启用的PID命名空间。

PID namespace provides separation of processes. The PID Namespace removes the view of the system processes, and allows process ids to be reused including PID 1.

​	PID命名空间提供进程的隔离。PID命名空间移除了系统进程的视图，并允许进程ID重用，包括PID 1。

In certain cases you want your container to share the host's process namespace, allowing processes within the container to see all of the processes on the system. For example, you could build a container with debugging tools like `strace` or `gdb`, but want to use these tools when debugging processes within the container.

​	在某些情况下，您希望容器共享主机的进程命名空间，使容器内的进程可以看到系统上的所有进程。例如，您可以构建一个包含调试工具如 `strace` 或 `gdb` 的容器，但希望在调试容器内的进程时使用这些工具。

#### 示例：在容器内运行htop Example: run htop inside a container

To run `htop` in a container that shares the process namespac of the host:

​	要在共享主机进程命名空间的容器中运行 `htop`：

1. Run an alpine container with the `--pid=host` option:

   使用 `--pid=host` 选项运行一个alpine容器：

   ```console
   $ docker run --rm -it --pid=host alpine
   ```

2. Install `htop` in the container:

   在容器内安装 `htop`：

   ```console
   / # apk add --quiet htop
   ```

3. Invoke the `htop` command.

   调用 `htop` 命令。

   ```console
   / # htop
   ```

#### 示例：加入另一个容器的PID命名空间 Example, join another container's PID namespace

Joining another container's PID namespace can be useful for debugging that container.

​	加入另一个容器的PID命名空间对调试该容器非常有用。

1. Start a container running a Redis server:

   启动一个运行Redis服务器的容器：

   ```console
   $ docker run --rm --name my-nginx -d nginx:alpine
   ```

2. Run an Alpine container that attaches the `--pid` namespace to the `my-nginx` container:

   运行一个将 `--pid` 命名空间附加到 `my-nginx` 容器的Alpine容器：

   ```console
   $ docker run --rm -it --pid=container:my-nginx \
     --cap-add SYS_PTRACE \
     --security-opt seccomp=unconfined \
     alpine
   ```

3. Install `strace` in the Alpine container:

   在Alpine容器中安装 `strace`：

   ```console
   / # apk add strace
   ```

4. Attach to process 1, the process ID of the `my-nginx` container:

   附加到进程1，即 `my-nginx` 容器的进程ID：

   ```console
   / # strace -p 1
   strace: Process 1 attached
   ```

### 禁用容器的命名空间重映射 (`--userns`) Disable namespace remapping for a container (`--userns`)

If you enable user namespaces on the daemon, all containers are started with user namespaces enabled by default. To disable user namespace remapping for a specific container, you can set the `--userns` flag to `host`.

​	如果您在守护进程上启用用户命名空间，所有容器默认启动时将启用用户命名空间。要为特定容器禁用用户命名空间重映射，可以将 `--userns` 标志设置为 `host`。



```console
docker run --userns=host hello-world
```

`host` is the only valid value for the `--userns` flag.

​	`host` 是 `--userns` 标志的唯一有效值。

For more information, refer to [Isolate containers with a user namespace]({{< ref "/manuals/DockerEngine/Security/Isolatecontainerswithausernamespace" >}}).

​	有关更多信息，请参考 [使用用户命名空间隔离容器]({{< ref "/manuals/DockerEngine/Security/Isolatecontainerswithausernamespace" >}})。

### UTS设置 (`--uts`) UTS settings (`--uts`)



```text
--uts=""  : Set the UTS namespace mode for the container
            'host': use the host's UTS namespace inside the container
            设置容器的UTS命名空间模式
            'host'：在容器内使用主机的UTS命名空间
```

The UTS namespace is for setting the hostname and the domain that's visible to running processes in that namespace. By default, all containers, including those with `--network=host`, have their own UTS namespace. Setting `--uts` to `host` results in the container using the same UTS namespace as the host.

​	UTS命名空间用于设置在该命名空间中运行的进程可见的主机名和域名。默认情况下，所有容器（包括那些使用 `--network=host` 的容器）都有自己的UTS命名空间。将 `--uts` 设置为 `host` 会导致容器使用与主机相同的UTS命名空间。

> **Note**
>
> Docker disallows combining the `--hostname` and `--domainname` flags with `--uts=host`. This is to prevent containers running in the host's UTS namespace from attempting to change the hosts' configuration.
>
> ​	Docker不允许将 `--hostname` 和 `--domainname` 标志与 `--uts=host` 一起使用。这是为了防止在主机的UTS命名空间中运行的容器尝试更改主机的配置。

You may wish to share the UTS namespace with the host if you would like the hostname of the container to change as the hostname of the host changes. A more advanced use case would be changing the host's hostname from a container.

​	如果您希望容器的主机名在主机的主机名变化时发生变化，您可能希望与主机共享UTS命名空间。更高级的用例是从容器中更改主机的主机名。

### IPC设置 (`--ipc`) IPC settings (`--ipc`)



```text
--ipc="MODE"  : Set the IPC mode for the container 设置容器的IPC模式
```

The `--ipc` flag accepts the following values:

​	`--ipc` 标志接受以下值：

| Value                      | Description                                                  |
| :------------------------- | :----------------------------------------------------------- |
| ""                         | 使用守护进程的默认值。 Use daemon's default.                 |
| "none"                     | 自有私有IPC命名空间，/dev/shm未挂载。Own private IPC namespace, with /dev/shm not mounted. |
| "private"                  | 自有私有IPC命名空间。Own private IPC namespace.              |
| "shareable"                | 自有私有IPC命名空间，有可能与其他容器共享。Own private IPC namespace, with a possibility to share it with other containers. |
| "container:<*name-or-ID*>" | 加入另一个（"shareable"）容器的IPC命名空间。 Join another ("shareable") container's IPC namespace. |
| "host"                     | 使用主机系统的IPC命名空间。 Use the host system's IPC namespace. |

If not specified, daemon default is used, which can either be `"private"` or `"shareable"`, depending on the daemon version and configuration.

​	如果未指定，将使用守护进程的默认值，这可以是 `"private"` 或 `"shareable"`，具体取决于守护进程的版本和配置。

[System V interprocess communication (IPC)](https://linux.die.net/man/5/ipc) namespaces provide separation of named shared memory segments, semaphores and message queues.

​	[System V进程间通信（IPC）](https://linux.die.net/man/5/ipc) 命名空间提供了对命名共享内存段、信号量和消息队列的隔离。

Shared memory segments are used to accelerate inter-process communication at memory speed, rather than through pipes or through the network stack. Shared memory is commonly used by databases and custom-built (typically C/OpenMPI, C++/using boost libraries) high performance applications for scientific computing and financial services industries. If these types of applications are broken into multiple containers, you might need to share the IPC mechanisms of the containers, using `"shareable"` mode for the main (i.e. "donor") container, and `"container:<donor-name-or-ID>"` for other containers.

​	共享内存段用于加速内存速度的进程间通信，而不是通过管道或网络栈。共享内存通常被数据库和定制（通常是C/OpenMPI、C++/使用boost库）高性能应用程序用于科学计算和金融服务行业。如果这些类型的应用程序被拆分成多个容器，您可能需要共享容器的IPC机制，为主容器（即“捐赠者”容器）使用 `"shareable"` 模式，而为其他容器使用 `"container:<捐赠者名称或ID>"`。

### 提升容器权限 (`--privileged`) Escalate container privileges (`--privileged`)

The `--privileged` flag gives the following capabilities to a container:

​	`--privileged` 标志赋予容器以下能力：

- Enables all Linux kernel capabilities
  - 启用所有Linux内核能力

- Disables the default seccomp profile
  - 禁用默认的seccomp配置文件

- Disables the default AppArmor profile
  - 禁用默认的AppArmor配置文件

- Disables the SELinux process label
  - 禁用SELinux进程标签

- Grants access to all host devices
  - 授予对所有主机设备的访问

- Makes `/sys` read-write
  - 将 `/sys` 设置为可读写

- Makes cgroups mounts read-write
  - 将cgroups挂载设置为可读写


In other words, the container can then do almost everything that the host can do. This flag exists to allow special use-cases, like running Docker within Docker.

​	换句话说，容器可以做几乎所有主机可以做的事情。此标志存在是为了允许特殊用例，例如在Docker中运行Docker。

> **Warning**
>
> Use the `--privileged` flag with caution. A container with `--privileged` is not a securely sandboxed process. Containers in this mode can get a root shell on the host and take control over the system.
>
> ​	使用 `--privileged` 标志时请谨慎。带有 `--privileged` 的容器不是安全的沙箱进程。在这种模式下运行的容器可以获取主机的root shell并控制系统。
>
> For most use cases, this flag should not be the preferred solution. If your container requires escalated privileges, you should prefer to explicitly grant the necessary permissions, for example by adding individual kernel capabilities with `--cap-add`.
>
> ​	对于大多数用例，此标志不应是首选解决方案。如果您的容器需要提升权限，您应优先明确授予必要的权限，例如通过使用 `--cap-add` 添加单个内核能力。
>
> For more information, see [Runtime privilege and Linux capabilities](https://docs.docker.com/engine/containers/run/#runtime-privilege-and-linux-capabilities)
>
> ​	有关更多信息，请参见 [运行时特权和Linux能力](https://docs.docker.com/engine/containers/run/#runtime-privilege-and-linux-capabilities)

The following example doesn't work, because by default, Docker drops most potentially dangerous kernel capabilities, including `CAP_SYS_ADMIN`(which is required to mount filesystems).

​	以下示例不起作用，因为默认情况下，Docker会放弃大多数潜在危险的内核能力，包括 `CAP_SYS_ADMIN`（这是挂载文件系统所需的）。



```console
$ docker run -t -i --rm ubuntu bash
root@bc338942ef20:/# mount -t tmpfs none /mnt
mount: permission denied
```

It works when you add the `--privileged` flag:

​	添加 `--privileged` 标志时可以正常工作：

```console
$ docker run -t -i --privileged ubuntu bash
root@50e3f57e16e6:/# mount -t tmpfs none /mnt
root@50e3f57e16e6:/# df -h
Filesystem      Size  Used Avail Use% Mounted on
none            1.9G     0  1.9G   0% /mnt
```

### 设置工作目录 (`-w`, `--workdir`) Set working directory (`-w`, `--workdir`)



```console
$ docker run -w /path/to/dir/ -i -t ubuntu pwd
```

The `-w` option runs the command executed inside the directory specified, in this example, `/path/to/dir/`. If the path doesn't exist, Docker creates it inside the container.

​	`-w` 选项使执行的命令在指定的目录内运行，在本示例中为 `/path/to/dir/`。如果路径不存在，Docker将在容器内创建它。

### 每个容器设置存储驱动选项 (`--storage-opt`) Set storage driver options per container (`--storage-opt`)



```console
$ docker run -it --storage-opt size=120G fedora /bin/bash
```

This (size) constraints the container filesystem size to 120G at creation time. This option is only available for the `btrfs`, `overlay2`, `windowsfilter`, and `zfs` storage drivers.

​	此选项（size）在创建时限制容器文件系统的大小为120G。此选项仅适用于 `btrfs`、`overlay2`、`windowsfilter` 和 `zfs` 存储驱动程序。

For the `overlay2` storage driver, the size option is only available if the backing filesystem is `xfs` and mounted with the `pquota` mount option. Under these conditions, you can pass any size less than the backing filesystem size.

​	对于 `overlay2` 存储驱动程序，只有在支持文件系统为 `xfs` 且以 `pquota` 挂载选项挂载的情况下，大小选项才可用。在这些条件下，您可以传递小于支持文件系统大小的任何大小。

For the `windowsfilter`, `btrfs`, and `zfs` storage drivers, you cannot pass a size less than the Default BaseFS Size.

​	对于 `windowsfilter`、`btrfs` 和 `zfs` 存储驱动程序，您不能传递小于默认基础文件系统大小的大小。

### 挂载 tmpfs (`--tmpfs`) Mount tmpfs (`--tmpfs`)

The `--tmpfs` flag lets you create a `tmpfs` mount.

​	`--tmpfs` 标志允许您创建一个 `tmpfs` 挂载。

The options that you can pass to `--tmpfs` are identical to the Linux `mount -t tmpfs -o` command. The following example mounts an empty `tmpfs` into the container with the `rw`, `noexec`, `nosuid`, `size=65536k` options.

​	可以传递给 `--tmpfs` 的选项与 Linux `mount -t tmpfs -o` 命令相同。以下示例将一个空的 `tmpfs` 挂载到容器中，并使用 `rw`、`noexec`、`nosuid`、`size=65536k` 选项。



```console
$ docker run -d --tmpfs /run:rw,noexec,nosuid,size=65536k my_image
```

For more information, see [tmpfs mounts](https://docs.docker.com/storage/tmpfs/).

​	有关更多信息，请参见 [tmpfs 挂载](https://docs.docker.com/storage/tmpfs/)。

### 挂载卷 (`-v`) Mount volume (`-v`)



```console
$ docker  run  -v $(pwd):$(pwd) -w $(pwd) -i -t  ubuntu pwd
```

The example above mounts the current directory into the container at the same path using the `-v` flag, sets it as the working directory, and then runs the `pwd` command inside the container.

​	上面的示例使用 `-v` 标志将当前目录挂载到容器中的相同路径，设置为工作目录，然后在容器内运行 `pwd` 命令。

As of Docker Engine version 23, you can use relative paths on the host.

​	从 Docker Engine 版本 23 开始，您可以在主机上使用相对路径。



```console
$ docker  run  -v ./content:/content -w /content -i -t  ubuntu pwd
```

The example above mounts the `content` directory in the current directory into the container at the `/content` path using the `-v` flag, sets it as the working directory, and then runs the `pwd` command inside the container.

​	上面的示例使用 `-v` 标志将当前目录中的 `content` 目录挂载到容器中的 `/content` 路径，设置为工作目录，然后在容器内运行 `pwd` 命令。



```console
$ docker run -v /doesnt/exist:/foo -w /foo -i -t ubuntu bash
```

When the host directory of a bind-mounted volume doesn't exist, Docker automatically creates this directory on the host for you. In the example above, Docker creates the `/doesnt/exist` folder before starting your container.

​	当绑定挂载的卷的主机目录不存在时，Docker 会自动为您在主机上创建此目录。在上面的示例中，Docker 在启动容器之前创建了 `/doesnt/exist` 文件夹。

### 以只读方式挂载卷 (`--read-only`) Mount volume read-only (`--read-only`)



```console
$ docker run --read-only -v /icanwrite busybox touch /icanwrite/here
```

You can use volumes in combination with the `--read-only` flag to control where a container writes files. The `--read-only` flag mounts the container's root filesystem as read only prohibiting writes to locations other than the specified volumes for the container.

​	您可以将卷与 `--read-only` 标志结合使用，以控制容器写入文件的位置。`--read-only` 标志将容器的根文件系统挂载为只读，禁止写入指定卷之外的位置。



```console
$ docker run -t -i -v /var/run/docker.sock:/var/run/docker.sock -v /path/to/static-docker-binary:/usr/bin/docker busybox sh
```

By bind-mounting the Docker Unix socket and statically linked Docker binary (refer to [get the Linux binary](https://docs.docker.com/engine/install/binaries/#install-static-binaries)), you give the container the full access to create and manipulate the host's Docker daemon.

​	通过绑定挂载 Docker Unix 套接字和静态链接的 Docker 二进制文件（参见 [获取 Linux 二进制文件](https://docs.docker.com/engine/install/binaries/#install-static-binaries)），您为容器提供了完全访问权限，以创建和操作主机的 Docker 守护进程。

On Windows, you must specify the paths using Windows-style path semantics.

​	在 Windows 上，您必须使用 Windows 风格的路径语义来指定路径。



```powershell
PS C:\> docker run -v c:\foo:c:\dest microsoft/nanoserver cmd /s /c type c:\dest\somefile.txt
Contents of file

PS C:\> docker run -v c:\foo:d: microsoft/nanoserver cmd /s /c type d:\somefile.txt
Contents of file
```

The following examples fails when using Windows-based containers, as the destination of a volume or bind mount inside the container must be one of: a non-existing or empty directory; or a drive other than `C:`. Further, the source of a bind mount must be a local directory, not a file.

​	在使用基于 Windows 的容器时，以下示例会失败，因为容器内部的卷或绑定挂载的目标必须是以下之一：不存在或为空的目录；或者是 `C:` 之外的驱动器。此外，绑定挂载的源必须是本地目录，而不是文件。



```powershell
net use z: \\remotemachine\share
docker run -v z:\foo:c:\dest ...
docker run -v \\uncpath\to\directory:c:\dest ...
docker run -v c:\foo\somefile.txt:c:\dest ...
docker run -v c:\foo:c: ...
docker run -v c:\foo:c:\existing-directory-with-contents ...
```

For in-depth information about volumes, refer to [manage data in containers](https://docs.docker.com/storage/volumes/)

​	有关卷的深入信息，请参见 [管理容器中的数据](https://docs.docker.com/storage/volumes/)。

### 使用 `--mount` 标志添加绑定挂载或卷 Add bind mounts or volumes using the `--mount` flag

The `--mount` flag allows you to mount volumes, host-directories, and `tmpfs` mounts in a container.

​	`--mount` 标志允许您在容器中挂载卷、主机目录和 `tmpfs` 挂载。

The `--mount` flag supports most options supported by the `-v` or the `--volume` flag, but uses a different syntax. For in-depth information on the `--mount` flag, and a comparison between `--volume` and `--mount`, refer to [Bind mounts](https://docs.docker.com/storage/bind-mounts/).

​	`--mount` 标志支持 `-v` 或 `--volume` 标志支持的大多数选项，但使用不同的语法。有关 `--mount` 标志的详细信息，以及 `--volume` 和 `--mount` 之间的比较，请参见 [绑定挂载](https://docs.docker.com/storage/bind-mounts/)。

Even though there is no plan to deprecate `--volume`, usage of `--mount` is recommended.

​	尽管没有计划弃用 `--volume`，但建议使用 `--mount`。

Examples: 示例：



```console
$ docker run --read-only --mount type=volume,target=/icanwrite busybox touch /icanwrite/here
```



```console
$ docker run -t -i --mount type=bind,src=/data,dst=/data busybox sh
```

### 发布或暴露端口 (`-p`, `--expose`) Publish or expose port (`-p`, `--expose`)



```console
$ docker run -p 127.0.0.1:80:8080/tcp nginx:alpine
```

This binds port `8080` of the container to TCP port `80` on `127.0.0.1` of the host. You can also specify `udp` and `sctp` ports. The [Networking overview page](https://docs.docker.com/network/) explains in detail how to publish ports with Docker.

​	这将容器的 `8080` 端口绑定到主机的 `127.0.0.1` 上的 TCP `80` 端口。您还可以指定 `udp` 和 `sctp` 端口。有关如何使用 Docker 发布端口的详细信息，请参见 [网络概述页面](https://docs.docker.com/network/)。

> **Note**
>
> If you don't specify an IP address (i.e., `-p 80:80` instead of `-p 127.0.0.1:80:80`) when publishing a container's ports, Docker publishes the port on all interfaces (address `0.0.0.0`) by default. These ports are externally accessible. This also applies if you configured UFW to block this specific port, as Docker manages its own iptables rules. [Read more](https://docs.docker.com/network/packet-filtering-firewalls/)
>
> ​	如果在发布容器的端口时不指定 IP 地址（即，使用 `-p 80:80` 而不是 `-p 127.0.0.1:80:80`），Docker 默认会在所有接口（地址 `0.0.0.0`）上发布该端口。这些端口是外部可访问的。如果您配置了 UFW 阻止特定端口，因为 Docker 管理自己的 iptables 规则，这也适用。[阅读更多](https://docs.docker.com/network/packet-filtering-firewalls/)。



```console
$ docker run --expose 80 nginx:alpine
```

This exposes port `80` of the container without publishing the port to the host system's interfaces.

​	这会暴露容器的 `80` 端口，但不会将端口发布到主机系统的接口上。

### 发布所有暴露的端口 (`-P`, `--publish-all`) Publish all exposed ports (`-P`, `--publish-all`)



```console
$ docker run -P nginx:alpine
```

The `-P`, or `--publish-all`, flag publishes all the exposed ports to the host. Docker binds each exposed port to a random port on the host.

​	`-P` 或 `--publish-all` 标志将所有暴露的端口发布到主机。Docker 将每个暴露的端口绑定到主机上的随机端口。

The `-P` flag only publishes port numbers that are explicitly flagged as exposed, either using the Dockerfile `EXPOSE` instruction or the `--expose` flag for the `docker run` command.

​	`-P` 标志仅发布明确标记为暴露的端口号，可以通过 Dockerfile 的 `EXPOSE` 指令或 `docker run` 命令的 `--expose` 标志来设置。

The range of ports are within an *ephemeral port range* defined by `/proc/sys/net/ipv4/ip_local_port_range`. Use the `-p` flag to explicitly map a single port or range of ports.

​	端口范围在 `/proc/sys/net/ipv4/ip_local_port_range` 定义的 *临时端口范围* 内。使用 `-p` 标志显式映射单个端口或端口范围。

### 设置拉取策略 (`--pull`) Set the pull policy (`--pull`)

Use the `--pull` flag to set the image pull policy when creating (and running) the container.

​	使用 `--pull` 标志可以在创建（和运行）容器时设置镜像的拉取策略。

The `--pull` flag can take one of these values:

​	`--pull` 标志可接受以下值：

| Value               | Description                                                  |
| :------------------ | :----------------------------------------------------------- |
| `missing` (default) | 如果镜像未在镜像缓存中找到则拉取，否则使用缓存中的镜像。 Pull the image if it was not found in the image cache, or use the cached image otherwise. |
| `never`             | 不拉取镜像，即使镜像缺失也不会拉取，如果镜像不在缓存中会产生错误。 Do not pull the image, even if it's missing, and produce an error if the image does not exist in the image cache. |
| `always`            | 在创建容器之前始终执行拉取操作。 Always perform a pull before creating the container. |

When creating (and running) a container from an image, the daemon checks if the image exists in the local image cache. If the image is missing, an error is returned to the CLI, allowing it to initiate a pull.

​	在从镜像创建（和运行）容器时，守护进程会检查镜像是否存在于本地镜像缓存中。如果镜像缺失，会向 CLI 返回一个错误，允许它启动拉取。

The default (`missing`) is to only pull the image if it's not present in the daemon's image cache. This default allows you to run images that only exist locally (for example, images you built from a Dockerfile, but that have not been pushed to a registry), and reduces networking.

​	默认值为 `missing`，即仅在镜像不在守护进程的缓存中时才拉取镜像。此默认值允许运行仅本地存在的镜像（例如，您从 Dockerfile 构建的镜像，尚未推送到仓库的镜像），并减少网络负载。

The `always` option always initiates a pull before creating the container. This option makes sure the image is up-to-date, and prevents you from using outdated images, but may not be suitable in situations where you want to test a locally built image before pushing (as pulling the image overwrites the existing image in the image cache).

​	选项 `always` 在创建容器之前始终启动拉取。此选项确保镜像是最新的，防止使用过时镜像，但可能不适用于想要在推送前测试本地构建镜像的情况（因为拉取操作会覆盖镜像缓存中的现有镜像）。

The `never` option disables (implicit) pulling images when creating containers, and only uses images that are available in the image cache. If the specified image is not found, an error is produced, and the container is not created. This option is useful in situations where networking is not available, or to prevent images from being pulled implicitly when creating containers.

​	选项 `never` 禁用在创建容器时自动拉取镜像，仅使用镜像缓存中的镜像。如果未找到指定的镜像，则会产生错误，容器不会被创建。此选项适用于无法访问网络的情况，或想防止在创建容器时隐式拉取镜像的情况。

The following example shows `docker run` with the `--pull=never` option set, which produces en error as the image is missing in the image-cache:

​	以下示例展示了 `docker run` 使用 `--pull=never` 选项设置的情况，由于镜像在镜像缓存中缺失，会产生错误：



```console
$ docker run --pull=never hello-world
docker: Error response from daemon: No such image: hello-world:latest.
```

### 设置环境变量 (`-e`, `--env`, `--env-file`) Set environment variables (`-e`, `--env`, `--env-file`)



```console
$ docker run -e MYVAR1 --env MYVAR2=foo --env-file ./env.list ubuntu bash
```

Use the `-e`, `--env`, and `--env-file` flags to set simple (non-array) environment variables in the container you're running, or overwrite variables defined in the Dockerfile of the image you're running.

​	使用 `-e`、`--env` 和 `--env-file` 标志可在运行的容器中设置简单（非数组）的环境变量，或覆盖镜像 Dockerfile 中定义的变量。

You can define the variable and its value when running the container:

​	可以在运行容器时定义变量及其值：

```console
$ docker run --env VAR1=value1 --env VAR2=value2 ubuntu env | grep VAR
VAR1=value1
VAR2=value2
```

You can also use variables exported to your local environment:

​	也可以使用本地环境中导出的变量：

```console
export VAR1=value1
export VAR2=value2

$ docker run --env VAR1 --env VAR2 ubuntu env | grep VAR
VAR1=value1
VAR2=value2
```

When running the command, the Docker CLI client checks the value the variable has in your local environment and passes it to the container. If no `=` is provided and that variable isn't exported in your local environment, the variable is unset in the container.

​	运行命令时，Docker CLI 客户端检查本地环境中变量的值并将其传递给容器。如果未提供 `=` 且该变量未在本地环境中导出，则该变量在容器中未设置。

You can also load the environment variables from a file. This file should use the syntax `<variable>=value` (which sets the variable to the given value) or `<variable>` (which takes the value from the local environment), and `#` for comments. Lines beginning with `#` are treated as line comments and are ignored, whereas a `#` appearing anywhere else in a line is treated as part of the variable value.

​	您还可以从文件加载环境变量。该文件应使用 `<variable>=value` 语法（将变量设置为给定值）或 `<variable>`（从本地环境中获取值），并使用 `#` 表示注释。以 `#` 开头的行被视为行注释并被忽略，而行中其他位置的 `#` 被视为变量值的一部分。



```console
$ cat env.list
# This is a comment
# 这是一个注释
VAR1=value1
VAR2=value2
USER

$ docker run --env-file env.list ubuntu env | grep -E 'VAR|USER'
VAR1=value1
VAR2=value2
USER=jonzeolla
```

### 设置容器的元数据 (`-l`, `--label`, `--label-file`) Set metadata on container (`-l`, `--label`, `--label-file`)

A label is a `key=value` pair that applies metadata to a container. To label a container with two labels:

​	标签是一个 `key=value` 对，用于将元数据应用于容器。要为容器添加两个标签：



```console
$ docker run -l my-label --label com.example.foo=bar ubuntu bash
```

The `my-label` key doesn't specify a value so the label defaults to an empty string (`""`). To add multiple labels, repeat the label flag (`-l` or `--label`).

​	`my-label` 键未指定值，因此标签默认为空字符串 (`""`)。要添加多个标签，请重复使用标签标志 (`-l` 或 `--label`)。

The `key=value` must be unique to avoid overwriting the label value. If you specify labels with identical keys but different values, each subsequent value overwrites the previous. Docker uses the last `key=value` you supply.

​	`key=value` 必须唯一，以避免覆盖标签值。如果指定了具有相同键但不同值的标签，则后续的值会覆盖之前的值。Docker 使用您提供的最后一个 `key=value`。

Use the `--label-file` flag to load multiple labels from a file. Delimit each label in the file with an EOL mark. The example below loads labels from a labels file in the current directory:

​	使用 `--label-file` 标志可以从文件加载多个标签。文件中的每个标签以换行符分隔。下面的示例从当前目录下的标签文件加载标签：



```console
$ docker run --label-file ./labels ubuntu bash
```

The label-file format is similar to the format for loading environment variables. (Unlike environment variables, labels are not visible to processes running inside a container.) The following example shows a label-file format:

​	标签文件格式类似于加载环境变量的格式（与环境变量不同，标签在容器内的进程中不可见）。以下示例展示了标签文件格式：



```console
com.example.label1="a label"

# this is a comment
com.example.label2=another\ label
com.example.label3
```

You can load multiple label-files by supplying multiple `--label-file` flags.

​	您可以通过多次提供 `--label-file` 标志来加载多个标签文件。

For additional information on working with labels, see [Labels](https://docs.docker.com/config/labels-custom-metadata/).

​	有关使用标签的更多信息，请参见 [标签](https://docs.docker.com/config/labels-custom-metadata/)。

### 将容器连接到网络 (`--network`) Connect a container to a network (`--network`)

To start a container and connect it to a network, use the `--network` option.

​	要启动并连接容器到网络，请使用 `--network` 选项。

If you want to add a running container to a network use the `docker network connect` subcommand.

​	如果要将正在运行的容器添加到网络，请使用 `docker network connect` 子命令。

You can connect multiple containers to the same network. Once connected, the containers can communicate using only another container's IP address or name. For `overlay` networks or custom plugins that support multi-host connectivity, containers connected to the same multi-host network but launched from different Engines can also communicate in this way.

​	您可以将多个容器连接到同一网络。一旦连接，这些容器可以使用彼此的 IP 地址或名称进行通信。对于支持多主机连接的 `overlay` 网络或自定义插件，从不同 Docker 引擎启动的容器也可以通过相同的多主机网络进行通信。

> **Note**
>
> The default bridge network only allows containers to communicate with each other using internal IP addresses. User-created bridge networks provide DNS resolution between containers using container names.
>
> ​	默认的桥接网络仅允许容器使用内部 IP 地址相互通信。用户创建的桥接网络提供基于容器名称的 DNS 解析。

You can disconnect a container from a network using the `docker network disconnect` command.

​	您可以使用 `docker network disconnect` 命令将容器从网络断开连接。

The following commands create a network named `my-net` and add a `busybox` container to the `my-net` network.

​	以下命令创建一个名为 `my-net` 的网络并将一个 `busybox` 容器添加到 `my-net` 网络中。

```console
$ docker network create my-net
$ docker run -itd --network=my-net busybox
```

You can also choose the IP addresses for the container with `--ip` and `--ip6` flags when you start the container on a user-defined network. To assign a static IP to containers, you must specify subnet block for the network.

​	您还可以在启动容器时使用 `--ip` 和 `--ip6` 标志为容器选择 IP 地址。要为容器分配静态 IP，您必须为网络指定子网块。



```console
$ docker network create --subnet 192.0.2.0/24 my-net
$ docker run -itd --network=my-net --ip=192.0.2.69 busybox
```

To connect the container to more than one network, repeat the `--network` option.

​	要将容器连接到多个网络，请重复使用 `--network` 选项。



```console
$ docker network create --subnet 192.0.2.0/24 my-net1
$ docker network create --subnet 192.0.3.0/24 my-net2
$ docker run -itd --network=my-net1 --network=my-net2 busybox
```

To specify options when connecting to more than one network, use the extended syntax for the `--network` flag. Comma-separated options that can be specified in the extended `--network` syntax are:

​	要在连接到多个网络时指定选项，请使用 `--network` 标志的扩展语法。在扩展 `--network` 语法中可以指定的逗号分隔选项包括：

| Option          | Top-level Equivalent                  | Description                                                  |
| --------------- | ------------------------------------- | ------------------------------------------------------------ |
| `name`          |                                       | 网络名称（必需） The name of the network (mandatory)         |
| `alias`         | `--network-alias`                     | 为容器添加网络范围的别名 Add network-scoped alias for the container |
| `ip`            | `--ip`                                | IPv4 地址（例如，172.30.100.104） IPv4 address (e.g., 172.30.100.104) |
| `ip6`           | `--ip6`                               | IPv6 地址（例如，2001:db8::33）IPv6 address (e.g., 2001:db8::33) |
| `mac-address`   | `--mac-address`                       | 容器 MAC 地址（例如，92:d0:c6:0a:29:33）Container MAC address (e.g., 92:d0:c6:0a:29:33) |
| `link-local-ip` | `--link-local-ip`                     | 容器的 IPv4/IPv6 链路本地地址 Container IPv4/IPv6 link-local addresses |
| `driver-opt`    | `docker network connect --driver-opt` | 网络驱动程序选项 Network driver options                      |



```console
$ docker network create --subnet 192.0.2.0/24 my-net1
$ docker network create --subnet 192.0.3.0/24 my-net2
$ docker run -itd --network=name=my-net1,ip=192.0.2.42 --network=name=my-net2,ip=192.0.3.42 busybox
```

`sysctl` settings that start with `net.ipv4.`, `net.ipv6.` or `net.mpls.` can be set per-interface using `driver-opt` label `com.docker.network.endpoint.sysctls`. The interface name must be the string `IFNAME`.

​	以 `net.ipv4.`、`net.ipv6.` 或 `net.mpls.` 开头的 `sysctl` 设置可以使用 `driver-opt` 标签 `com.docker.network.endpoint.sysctls` 逐接口进行设置。接口名称必须为字符串 `IFNAME`。

To set more than one `sysctl` for an interface, quote the whole `driver-opt` field, remembering to escape the quotes for the shell if necessary. For example, if the interface to `my-net` is given name `eth0`, the following example sets sysctls `net.ipv4.conf.eth0.log_martians=1` and `net.ipv4.conf.eth0.forwarding=0`, and assigns the IPv4 address `192.0.2.42`.

​	要为接口设置多个 `sysctl`，引用整个 `driver-opt` 字段，并在必要时记得为 shell 转义引号。例如，如果将 `my-net` 的接口名称指定为 `eth0`，以下示例设置 `net.ipv4.conf.eth0.log_martians=1` 和 `net.ipv4.conf.eth0.forwarding=0` sysctl，并分配 IPv4 地址 `192.0.2.42`。



```console
$ docker network create --subnet 192.0.2.0/24 my-net
$ docker run -itd --network=name=my-net,\"driver-opt=com.docker.network.endpoint.sysctls=net.ipv4.conf.IFNAME.log_martians=1,net.ipv4.conf.IFNAME.forwarding=0\",ip=192.0.2.42 busybox
```

> **Note**
>
> Network drivers may restrict the sysctl settings that can be modified and, to protect the operation of the network, new restrictions may be added in the future.
>
> ​	网络驱动程序可能会限制可以修改的 sysctl 设置，为保护网络操作，未来可能会添加新限制。

For more information on connecting a container to a network when using the `run` command, see the [Docker network overview](https://docs.docker.com/network/).

​	有关使用 `run` 命令连接容器到网络的更多信息，请参见 [Docker 网络概述](https://docs.docker.com/network/)。

### 从容器挂载卷 (`--volumes-from`) Mount volumes from container (`--volumes-from`)



```console
$ docker run --volumes-from 777f7dc92da7 --volumes-from ba8c0c54f0f2:ro -i -t ubuntu pwd
```

The `--volumes-from` flag mounts all the defined volumes from the referenced containers. You can specify more than one container by repetitions of the `--volumes-from` argument. The container ID may be optionally suffixed with `:ro` or `:rw` to mount the volumes in read-only or read-write mode, respectively. By default, Docker mounts the volumes in the same mode (read write or read only) as the reference container.

​	`--volumes-from` 标志从引用的容器挂载所有定义的卷。您可以通过多次使用 `--volumes-from` 参数指定多个容器。可以选择在容器 ID 后面加上 `:ro` 或 `:rw` 来将卷挂载为只读或读写模式。默认情况下，Docker 会按照引用容器的相同模式（读写或只读）挂载卷。

Labeling systems like SELinux require placing proper labels on volume content mounted into a container. Without a label, the security system might prevent the processes running inside the container from using the content. By default, Docker does not change the labels set by the OS.

​	像 SELinux 这样的标签系统需要在挂载到容器中的卷内容上设置适当的标签。没有标签，安全系统可能会阻止容器内运行的进程使用该内容。默认情况下，Docker 不会更改操作系统设置的标签。

To change the label in the container context, you can add either of two suffixes `:z` or `:Z` to the volume mount. These suffixes tell Docker to relabel file objects on the shared volumes. The `z` option tells Docker that two containers share the volume content. As a result, Docker labels the content with a shared content label. Shared volume labels allow all containers to read/write content. The `Z` option tells Docker to label the content with a private unshared label. Only the current container can use a private volume.

​	要更改容器上下文中的标签，您可以为卷挂载添加 `:z` 或 `:Z` 后缀。这些后缀告诉 Docker 在共享卷上的文件对象上重新标记。`z` 选项告诉 Docker 该卷内容由两个容器共享。结果，Docker 使用共享内容标签标记内容。共享卷标签允许所有容器读写内容。`Z` 选项告诉 Docker 使用私有未共享的标签标记内容。只有当前容器可以使用私有卷。

### 分离模式 (`-d`, `--detach`) Detached mode (`-d`, `--detach`)

The `--detach` (or `-d`) flag starts a container as a background process that doesn't occupy your terminal window. By design, containers started in detached mode exit when the root process used to run the container exits, unless you also specify the `--rm` option. If you use `-d` with `--rm`, the container is removed when it exits or when the daemon exits, whichever happens first.

​	`--detach`（或 `-d`）标志以后台进程的方式启动容器，不会占用您的终端窗口。默认情况下，在分离模式下启动的容器，当用于运行容器的根进程退出时，容器也会退出，除非同时指定 `--rm` 选项。如果您同时使用 `-d` 和 `--rm`，容器在退出或守护进程退出时会被移除，以先发生者为准。

Don't pass a `service x start` command to a detached container. For example, this command attempts to start the `nginx` service.

​	不要向分离模式的容器传递 `service x start` 命令。例如，以下命令尝试启动 `nginx` 服务。



```console
$ docker run -d -p 80:80 my_image service nginx start
```

This succeeds in starting the `nginx` service inside the container. However, it fails the detached container paradigm in that, the root process (`service nginx start`) returns and the detached container stops as designed. As a result, the `nginx` service starts but can't be used. Instead, to start a process such as the `nginx` web server do the following:

​	这可以成功启动容器内的 `nginx` 服务。然而，这不符合分离容器的设计理念，因为根进程（`service nginx start`）返回后，分离容器就会按设计停止。因此，`nginx` 服务启动了，但无法使用。为了启动诸如 `nginx` 的 Web 服务器进程，执行以下操作：



```console
$ docker run -d -p 80:80 my_image nginx -g 'daemon off;'
```

To do input/output with a detached container use network connections or shared volumes. These are required because the container is no longer listening to the command line where `docker run` was run.

​	要与分离的容器进行输入/输出操作，请使用网络连接或共享卷，因为容器已不再监听运行 `docker run` 的命令行。

### 覆盖分离序列 (`--detach-keys`) Override the detach sequence (`--detach-keys`)

Use the `--detach-keys` option to override the Docker key sequence for detach. This is useful if the Docker default sequence conflicts with key sequence you use for other applications. There are two ways to define your own detach key sequence, as a per-container override or as a configuration property on your entire configuration.

​	使用 `--detach-keys` 选项可以覆盖 Docker 的默认分离键序列。这对于 Docker 的默认序列与您用于其他应用的键序列冲突时非常有用。您可以通过每个容器覆盖或整个配置属性的方式定义自己的分离键序列。

To override the sequence for an individual container, use the `--detach-keys="<sequence>"` flag with the `docker attach` command. The format of the `<sequence>` is either a letter [a-Z], or the `ctrl-` combined with any of the following:

​	要为单个容器覆盖分离键序列，请在 `docker attach` 命令中使用 `--detach-keys="<sequence>"` 标志。`<sequence>` 的格式可以是字母 [a-Z]，或与以下任一字符组合的 `ctrl-`：

- `a-z` (a single lowercase alpha character )
  - `a-z`（单个小写字母）

- `@` (at sign)
  - `@`（@ 符号）

- `[` (left bracket)
  - `[`（左括号）

- `\\` (two backward slashes)
  - `\\`（两个反斜杠）

- `_` (underscore)
  - `_`（下划线）

- `^` (caret)
  - `^`（插入符号）


These `a`, `ctrl-a`, `X`, or `ctrl-\\` values are all examples of valid key sequences. To configure a different configuration default key sequence for all containers, see [**Configuration file** section](https://docs.docker.com/reference/cli/docker/#configuration-files).

​	例如，`a`、`ctrl-a`、`X` 或 `ctrl-\\` 都是有效的键序列。要为所有容器配置一个不同的默认键序列，请参见 [**配置文件** 部分](https://docs.docker.com/reference/cli/docker/#configuration-files)。

### 添加主机设备到容器 (`--device`) Add host device to container (`--device`)



```console
$ docker run -it --rm \
    --device=/dev/sdc:/dev/xvdc \
    --device=/dev/sdd \
    --device=/dev/zero:/dev/foobar \
    ubuntu ls -l /dev/{xvdc,sdd,foobar}

brw-rw---- 1 root disk 8, 2 Feb  9 16:05 /dev/xvdc
brw-rw---- 1 root disk 8, 3 Feb  9 16:05 /dev/sdd
crw-rw-rw- 1 root root 1, 5 Feb  9 16:05 /dev/foobar
```

It's often necessary to directly expose devices to a container. The `--device` option enables that. For example, adding a specific block storage device or loop device or audio device to an otherwise unprivileged container (without the `--privileged` flag) and have the application directly access it.

​	有时需要直接将设备暴露给容器。`--device` 选项可以实现这一功能。例如，将特定的块存储设备、循环设备或音频设备添加到非特权容器（未使用 `--privileged` 标志），并让应用直接访问它。

By default, the container is able to `read`, `write` and `mknod` these devices. This can be overridden using a third `:rwm` set of options to each `--device` flag. If the container is running in privileged mode, then Docker ignores the specified permissions.

​	默认情况下，容器可以对这些设备进行 `read`、`write` 和 `mknod` 操作。您可以通过为每个 `--device` 标志设置第三个 `:rwm` 参数来覆盖此设置。如果容器在特权模式下运行，Docker 会忽略指定的权限。



```console
$ docker run --device=/dev/sda:/dev/xvdc --rm -it ubuntu fdisk  /dev/xvdc

Command (m for help): q
$ docker run --device=/dev/sda:/dev/xvdc:r --rm -it ubuntu fdisk  /dev/xvdc
You will not be able to write the partition table.

Command (m for help): q

$ docker run --device=/dev/sda:/dev/xvdc:rw --rm -it ubuntu fdisk  /dev/xvdc

Command (m for help): q

$ docker run --device=/dev/sda:/dev/xvdc:m --rm -it ubuntu fdisk  /dev/xvdc
fdisk: unable to open /dev/xvdc: Operation not permitted
```

> **Note**
>
> The `--device` option cannot be safely used with ephemeral devices. You shouldn't add block devices that may be removed to untrusted containers with `--device`.
>
> ​	`--device` 选项不能安全地用于短暂设备。请勿使用 `--device` 向不受信任的容器添加可能会被移除的块设备。

For Windows, the format of the string passed to the `--device` option is in the form of `--device=<IdType>/<Id>`. Beginning with Windows Server 2019 and Windows 10 October 2018 Update, Windows only supports an IdType of `class` and the Id as a [device interface class GUID](https://docs.microsoft.com/en-us/windows-hardware/drivers/install/overview-of-device-interface-classes). Refer to the table defined in the [Windows container docs](https://docs.microsoft.com/en-us/virtualization/windowscontainers/deploy-containers/hardware-devices-in-containers) for a list of container-supported device interface class GUIDs.

​	对于 Windows，传递给 `--device` 选项的字符串格式为 `--device=<IdType>/<Id>`。自 Windows Server 2019 和 Windows 10 2018 年 10 月更新版以来，Windows 仅支持 `class` 类型的 IdType，并将 Id 作为 [设备接口类 GUID](https://docs.microsoft.com/en-us/windows-hardware/drivers/install/overview-of-device-interface-classes)。有关容器支持的设备接口类 GUID 的列表，请参见 [Windows 容器文档](https://docs.microsoft.com/en-us/virtualization/windowscontainers/deploy-containers/hardware-devices-in-containers) 中的表格。

If you specify this option for a process-isolated Windows container, Docker makes *all* devices that implement the requested device interface class GUID available in the container. For example, the command below makes all COM ports on the host visible in the container.

​	如果为进程隔离的 Windows 容器指定此选项，Docker 会使实现所请求设备接口类 GUID 的所有设备在容器中可用。例如，以下命令使主机上的所有 COM 端口在容器中可见。



```powershell
PS C:\> docker run --device=class/86E0D1E0-8089-11D0-9CE4-08003E301F73 mcr.microsoft.com/windows/servercore:ltsc2019
```

> **Note**
>
> The `--device` option is only supported on process-isolated Windows containers, and produces an error if the container isolation is `hyperv`.
>
> ​	`--device` 选项仅支持进程隔离的 Windows 容器，如果容器隔离是 `hyperv`，则会产生错误。

#### CDI devices

> **Note**
>
> The CDI feature is experimental, and potentially subject to change. CDI is currently only supported for Linux containers.
>
> ​	CDI 功能是实验性的，可能会有变动。CDI 当前仅支持 Linux 容器。

[Container Device Interface (CDI)](https://github.com/cncf-tags/container-device-interface/blob/main/SPEC.md) is a standardized mechanism for container runtimes to create containers which are able to interact with third party devices.

​	[容器设备接口 (CDI)](https://github.com/cncf-tags/container-device-interface/blob/main/SPEC.md) 是一种标准机制，允许容器运行时创建能够与第三方设备交互的容器。

With CDI, device configurations are declaratively defined using a JSON or YAML file. In addition to enabling the container to interact with the device node, it also lets you specify additional configuration for the device, such as environment variables, host mounts (such as shared objects), and executable hooks.

​	使用 CDI，可以使用 JSON 或 YAML 文件以声明的方式定义设备配置。除了启用容器与设备节点的交互，还可以为设备指定其他配置，例如环境变量、主机挂载（如共享对象）和可执行挂钩。

You can reference a CDI device with the `--device` flag using the fully-qualified name of the device, as shown in the following example:

​	您可以使用 `--device` 标志引用 CDI 设备，使用设备的全限定名称，如以下示例所示：



```console
$ docker run --device=vendor.com/class=device-name --rm -it ubuntu
```

This starts an `ubuntu` container with access to the specified CDI device, `vendor.com/class=device-name`, assuming that:

​	这将启动一个 `ubuntu` 容器，并允许其访问指定的 CDI 设备 `vendor.com/class=device-name`，前提是：

- A valid CDI specification (JSON or YAML file) for the requested device is available on the system running the daemon, in one of the configured CDI specification directories.
  - 请求的设备的有效 CDI 规范（JSON 或 YAML 文件）在运行守护进程的系统上可用，并位于配置的 CDI 规范目录之一。

- The CDI feature has been enabled in the daemon; see [Enable CDI devices](https://docs.docker.com/reference/cli/dockerd/#enable-cdi-devices).
  - 守护进程中启用了 CDI 功能；参见 [启用 CDI 设备](https://docs.docker.com/reference/cli/dockerd/#enable-cdi-devices)。


### 附加到 STDIN/STDOUT/STDERR (`-a`, `--attach`) Attach to STDIN/STDOUT/STDERR (`-a`, `--attach`)

The `--attach` (or `-a`) flag tells `docker run` to bind to the container's `STDIN`, `STDOUT` or `STDERR`. This makes it possible to manipulate the output and input as needed. You can specify to which of the three standard streams (`STDIN`, `STDOUT`, `STDERR`) you'd like to connect instead, as in:

​	`--attach`（或 `-a`）标志告诉 `docker run` 绑定到容器的 `STDIN`、`STDOUT` 或 `STDERR`。这样可以根据需要操作输出和输入。您可以选择连接到三个标准流（`STDIN`、`STDOUT`、`STDERR`）中的任何一个，如下所示：



```console
$ docker run -a stdin -a stdout -i -t ubuntu /bin/bash
```

The following example pipes data into a container and prints the container's ID by attaching only to the container's `STDIN`.

​	以下示例将数据传输到容器中，并通过仅连接到容器的 `STDIN` 打印容器的 ID。



```console
$ echo "test" | docker run -i -a stdin ubuntu cat -
```

The following example doesn't print anything to the console unless there's an error because output is only attached to the `STDERR` of the container. The container's logs still store what's written to `STDERR` and `STDOUT`.

​	以下示例仅连接到容器的 `STDERR`，因此不会向控制台打印任何内容，除非出现错误。容器的日志仍会记录写入 `STDERR` 和 `STDOUT` 的内容。



```console
$ docker run -a stderr ubuntu echo test
```

The following example shows a way of using `--attach` to pipe a file into a container. The command prints the container's ID after the build completes and you can retrieve the build logs using `docker logs`. This is useful if you need to pipe a file or something else into a container and retrieve the container's ID once the container has finished running.

​	以下示例展示了使用 `--attach` 将文件传输到容器中的一种方法。命令在构建完成后打印容器的 ID，您可以使用 `docker logs` 检索构建日志。这对于在容器中传输文件或其他内容并在容器运行完毕后检索容器 ID 很有用。



```console
$ cat somefile | docker run -i -a stdin mybuilder dobuild
```

> **Note**
>
> A process running as PID 1 inside a container is treated specially by Linux: it ignores any signal with the default action. So, the process doesn't terminate on `SIGINT` or `SIGTERM` unless it's coded to do so.
>
> ​	在容器中以 PID 1 运行的进程在 Linux 中会被特殊处理：它会忽略默认操作的任何信号。因此，除非特别编写，进程不会在 `SIGINT` 或 `SIGTERM` 信号下终止。

See also [the `docker cp` command]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainercp" >}}).

​	另请参见 [`docker cp` 命令]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainercp" >}})。

### 保持 STDIN 打开 (`-i`, `--interactive`) Keep STDIN open (`-i`, `--interactive`)

The `--interactive` (or `-i`) flag keeps the container's `STDIN` open, and lets you send input to the container through standard input.

​	`--interactive`（或 `-i`）标志保持容器的 `STDIN` 打开，并允许您通过标准输入向容器发送输入。

```console
$ echo hello | docker run --rm -i busybox cat
hello
```

The `-i` flag is most often used together with the `--tty` flag to bind the I/O streams of the container to a pseudo terminal, creating an interactive terminal session for the container. See [Allocate a pseudo-TTY](https://docs.docker.com/reference/cli/docker/container/run/#tty) for more examples.

​	`-i` 标志通常与 `--tty` 标志一起使用，将容器的 I/O 流绑定到伪终端，从而创建容器的交互式终端会话。请参见 [分配伪终端](https://docs.docker.com/reference/cli/docker/container/run/#tty) 获取更多示例。

```console
$ docker run -it debian
root@10a3e71492b0:/# factor 90
90: 2 3 3 5
root@10a3e71492b0:/# exit
exit
```

Using the `-i` flag on its own allows for composition, such as piping input to containers:

​	单独使用 `-i` 标志允许组合操作，例如将输入传递到多个容器：

```console
$ docker run --rm -i busybox echo "foo bar baz" \
  | docker run --rm -i busybox awk '{ print $2 }' \
  | docker run --rm -i busybox rev
rab
```

### 指定初始化进程 Specify an init process

You can use the `--init` flag to indicate that an init process should be used as the PID 1 in the container. Specifying an init process ensures the usual responsibilities of an init system, such as reaping zombie processes, are performed inside the created container.

​	您可以使用 `--init` 标志来指示将初始化进程作为容器中的 PID 1 使用。指定初始化进程可以确保初始化系统的常规职责，例如清除僵尸进程，在创建的容器中执行。

The default init process used is the first `docker-init` executable found in the system path of the Docker daemon process. This `docker-init` binary, included in the default installation, is backed by [tini](https://github.com/krallin/tini).

​	默认使用的初始化进程是 Docker 守护进程路径中的第一个 `docker-init` 可执行文件。默认安装的 `docker-init` 二进制文件由 [tini](https://github.com/krallin/tini) 支持。

### 分配伪终端 (`-t`, `--tty`) Allocate a pseudo-TTY (`-t`, `--tty`)

The `--tty` (or `-t`) flag attaches a pseudo-TTY to the container, connecting your terminal to the I/O streams of the container. Allocating a pseudo-TTY to the container means that you get access to input and output feature that TTY devices provide.

​	`--tty`（或 `-t`）标志将伪终端分配给容器，将您的终端连接到容器的 I/O 流。为容器分配伪终端意味着可以使用 TTY 设备提供的输入和输出功能。

For example, the following command runs the `passwd` command in a `debian` container, to set a new password for the `root` user.

​	例如，以下命令在 `debian` 容器中运行 `passwd` 命令，以为 `root` 用户设置新密码。



```console
$ docker run -i debian passwd root
New password: karjalanpiirakka9
Retype new password: karjalanpiirakka9
passwd: password updated successfully
```

If you run this command with only the `-i` flag (which lets you send text to `STDIN` of the container), the `passwd` prompt displays the password in plain text. However, if you try the same thing but also adding the `-t` flag, the password is hidden:

​	如果只使用 `-i` 标志（允许向容器的 `STDIN` 发送文本），则 `passwd` 提示符会将密码显示为明文。但是，如果添加 `-t` 标志，则密码会隐藏：



```console
$ docker run -it debian passwd root
New password:
Retype new password:
passwd: password updated successfully
```

This is because `passwd` can suppress the output of characters to the terminal using the echo-off TTY feature.

​	这是因为 `passwd` 可以通过 TTY 的 echo-off 功能来隐藏密码。

You can use the `-t` flag without `-i` flag. This still allocates a pseudo-TTY to the container, but with no way of writing to `STDIN`. The only time this might be useful is if the output of the container requires a TTY environment.

​	您可以仅使用 `-t` 标志而不使用 `-i` 标志。这仍然会为容器分配伪终端，但无法向 `STDIN` 写入内容。仅当容器的输出需要 TTY 环境时，此用法可能有用。

### 指定自定义 cgroups - Specify custom cgroups

Using the `--cgroup-parent` flag, you can pass a specific cgroup to run a container in. This allows you to create and manage cgroups on their own. You can define custom resources for those cgroups and put containers under a common parent group.

​	使用 `--cgroup-parent` 标志，您可以传递特定的 cgroup 来运行容器。这允许您独立创建和管理 cgroups。您可以为这些 cgroups 定义自定义资源，并将容器放在共同的父组下。

### 使用动态创建的设备 (`--device-cgroup-rule`) Using dynamically created devices (`--device-cgroup-rule`)

Docker assigns devices available to a container at creation time. The assigned devices are added to the cgroup.allow file and created into the container when it runs. This poses a problem when you need to add a new device to running container.

​	Docker 会在容器创建时分配可用设备，并将分配的设备添加到 cgroup.allow 文件中，并在容器运行时创建。这在您需要向运行中的容器添加新设备时会遇到问题。

One solution is to add a more permissive rule to a container allowing it access to a wider range of devices. For example, supposing the container needs access to a character device with major `42` and any number of minor numbers (added as new devices appear), add the following rule:

​	一种解决方案是向容器添加更宽松的规则，允许其访问更广泛的设备范围。例如，假设容器需要访问主设备为 `42` 且次设备号范围可变的字符设备（当新设备出现时自动添加），可以添加以下规则：



```console
$ docker run -d --device-cgroup-rule='c 42:* rmw' --name my-container my-image
```

Then, a user could ask `udev` to execute a script that would `docker exec my-container mknod newDevX c 42 <minor>` the required device when it is added.

​	然后，用户可以请求 `udev` 执行脚本，当需要的设备添加时，通过 `docker exec my-container mknod newDevX c 42 <minor>` 创建所需的设备。

> **Note**
>
> You still need to explicitly add initially present devices to the `docker run` / `docker create` command.
>
> ​	初始存在的设备仍需在 `docker run` / `docker create` 命令中显式添加。

### 访问 NVIDIA GPU

The `--gpus` flag allows you to access NVIDIA GPU resources. First you need to install the [nvidia-container-runtime](https://nvidia.github.io/nvidia-container-runtime/).

​	`--gpus` 标志允许访问 NVIDIA GPU 资源。首先需要安装 [nvidia-container-runtime](https://nvidia.github.io/nvidia-container-runtime/)。

> **Note**
>
> You can also specify a GPU as a CDI device with the `--device` flag, see [CDI devices](https://docs.docker.com/reference/cli/docker/container/run/#cdi-devices).
>
> ​	您也可以使用 `--device` 标志将 GPU 作为 CDI 设备指定，详见 [CDI 设备](https://docs.docker.com/reference/cli/docker/container/run/#cdi-devices)。

Read [Specify a container's resources](https://docs.docker.com/config/containers/resource_constraints/) for more information.

​	有关更多信息，请参阅 [指定容器资源](https://docs.docker.com/config/containers/resource_constraints/)。

To use `--gpus`, specify which GPUs (or all) to use. If you provide no value, Docker uses all available GPUs. The example below exposes all available GPUs.

​	使用 `--gpus` 时，可以指定使用哪些 GPU（或全部）。如果未提供值，Docker 使用所有可用的 GPU。以下示例暴露所有可用的 GPU：



```console
$ docker run -it --rm --gpus all ubuntu nvidia-smi
```

Use the `device` option to specify GPUs. The example below exposes a specific GPU.

​	使用 `device` 选项指定 GPU。以下示例暴露特定的 GPU：



```console
$ docker run -it --rm --gpus device=GPU-3a23c669-1f69-c64e-cf85-44e9b07e7a2a ubuntu nvidia-smi
```

The example below exposes the first and third GPUs.

​	以下示例暴露第一个和第三个 GPU：



```console
$ docker run -it --rm --gpus '"device=0,2"' ubuntu nvidia-smi
```

### 重启策略 (`--restart`) Restart policies (`--restart`)

Use the `--restart` flag to specify a container's *restart policy*. A restart policy controls whether the Docker daemon restarts a container after exit. Docker supports the following restart policies:

​	使用 `--restart` 标志可以指定容器的 *重启策略*。重启策略控制 Docker 守护进程在容器退出后是否重新启动容器。Docker 支持以下重启策略：

| Policy                     | Result                                                       |
| :------------------------- | :----------------------------------------------------------- |
| `no`                       | 容器退出时不自动重启容器。这是默认设置。Do not automatically restart the container when it exits. This is the default. |
| `on-failure[:max-retries]` | 仅在容器以非零状态退出时重启。可选地，限制 Docker 守护进程尝试重启的次数。 Restart only if the container exits with a non-zero exit status. Optionally, limit the number of restart retries the Docker daemon attempts. |
| `unless-stopped`           | 除非容器被明确停止或 Docker 自身被停止或重启，否则重启容器。 Restart the container unless it's explicitly stopped or Docker itself is stopped or restarted. |
| `always`                   | 无论退出状态如何，总是重启容器。当指定 `always` 时，Docker 守护进程会无限期地尝试重启容器。即使在守护进程启动时容器当前是停止状态，容器也会被启动。 Always restart the container regardless of the exit status. When you specify always, the Docker daemon tries to restart the container indefinitely. The container always starts on daemon startup, regardless of the current state of the container. |



```console
$ docker run --restart=always redis
```

This runs the `redis` container with a restart policy of **always**. If the container exits, Docker restarts it.

​	此命令以 **always** 的重启策略运行 `redis` 容器。如果容器退出，Docker 会重启它。

When a restart policy is active on a container, it shows as either `Up` or `Restarting` in [`docker ps`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerps" >}}). It can also be useful to use [`docker events`]({{< ref "/reference/CLIreference/docker/dockersystem/dockersystemevents" >}}) to see the restart policy in effect.

​	当容器上启用了重启策略时，它在 [`docker ps`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerps" >}}) 中显示为 `Up` 或 `Restarting`。您也可以使用 [`docker events`]({{< ref "/reference/CLIreference/docker/dockersystem/dockersystemevents" >}}) 查看重启策略的效果。

An increasing delay (double the previous delay, starting at 100 milliseconds) is added before each restart to prevent flooding the server. This means the daemon waits for 100 ms, then 200 ms, 400, 800, 1600, and so on until either the `on-failure` limit, the maximum delay of 1 minute is hit, or when you `docker stop` or `docker rm -f` the container.

​	为防止服务器被频繁重启干扰，每次重启之前都会增加延迟（前一次延迟的两倍，初始为 100 毫秒）。这意味着守护进程在重启之间会等待 100 毫秒、200 毫秒、400 毫秒、800 毫秒、1600 毫秒等，直到达到 `on-failure` 限制、最大延迟 1 分钟，或使用 `docker stop` 或 `docker rm -f` 停止容器。

If a container is successfully restarted (the container is started and runs for at least 10 seconds), the delay is reset to its default value of 100 ms.

​	如果容器成功重启（即容器启动并运行至少 10 秒），延迟重置为默认值 100 毫秒。

#### 指定重启尝试的限制 Specify a limit for restart attempts

You can specify the maximum amount of times Docker attempts to restart the container when using the **on-failure** policy. By default, Docker never stops attempting to restart the container.

​	在使用 **on-failure** 策略时，可以指定 Docker 尝试重启容器的最大次数。默认情况下，Docker 会无限制地尝试重启容器。

The following example runs the `redis` container with a restart policy of **on-failure** and a maximum restart count of 10.

​	以下示例以 **on-failure** 重启策略和最大重启次数 10 运行 `redis` 容器。



```console
$ docker run --restart=on-failure:10 redis
```

If the `redis` container exits with a non-zero exit status more than 10 times in a row, Docker stops trying to restart the container. Providing a maximum restart limit is only valid for the **on-failure** policy.

​	如果 `redis` 容器连续 10 次以非零状态退出，Docker 将停止尝试重启容器。仅 **on-failure** 策略可以提供最大重启限制。

#### 检查容器重启次数 Inspect container restarts

The number of (attempted) restarts for a container can be obtained using the [`docker inspect`]({{< ref "/reference/CLIreference/docker/dockerinspect" >}}) command. For example, to get the number of restarts for container "my-container";

​	可以使用 [`docker inspect`]({{< ref "/reference/CLIreference/docker/dockerinspect" >}}) 命令获取容器的（尝试）重启次数。例如，获取容器 "my-container" 的重启次数：



```console
$ docker inspect -f "{{ .RestartCount }}" my-container
2
```

Or, to get the last time the container was (re)started;

​	或获取容器的最后一次（重新）启动时间：

```console
$ docker inspect -f "{{ .State.StartedAt }}" my-container
2015-03-04T23:47:07.691840179Z
```

Combining `--restart` (restart policy) with the `--rm` (clean up) flag results in an error. On container restart, attached clients are disconnected.

​	将 `--restart`（重启策略）与 `--rm`（清理）标志组合使用会导致错误。容器重启时，连接的客户端将断开连接。

### 清理 (`--rm`) Clean up (`--rm`)

By default, a container's file system persists even after the container exits. This makes debugging a lot easier, since you can inspect the container's final state and you retain all your data.

​	默认情况下，容器的文件系统在容器退出后仍然存在。这使调试更容易，因为可以检查容器的最终状态并保留所有数据。

If you are running short-term **foreground** processes, these container file systems can start to pile up. If you'd like Docker to automatically clean up the container and remove the file system when the container exits, use the `--rm` flag:

​	如果您运行短期 **前台** 进程，这些容器文件系统可能会开始堆积。如果希望 Docker 在容器退出后自动清理并移除文件系统，请使用 `--rm` 标志：

```text
--rm: Automatically remove the container when it exits 容器退出时自动移除容器
```

> **Note**
>
> If you set the `--rm` flag, Docker also removes the anonymous volumes associated with the container when the container is removed. This is similar to running `docker rm -v my-container`. Only volumes that are specified without a name are removed. For example, when running the following command, volume `/foo` is removed, but not `/bar`:
>
> ​	如果设置了 `--rm` 标志，Docker 也会在移除容器时删除与之关联的匿名卷。这类似于运行 `docker rm -v my-container`。只有未指定名称的卷会被删除。例如，在以下命令中，卷 `/foo` 会被删除，而 `/bar` 不会：
>
> 
>
> ```console
> $ docker run --rm -v /foo -v awesome:/bar busybox top
> ```
>
> Volumes inherited via `--volumes-from` are removed with the same logic: if the original volume was specified with a name it isn't removed.
>
> ​	通过 `--volumes-from` 继承的卷也遵循相同的逻辑：如果原始卷指定了名称，则不会删除。

### 向容器的主机文件添加条目 (`--add-host`) Add entries to container hosts file (`--add-host`)

You can add other hosts into a container's `/etc/hosts` file by using one or more `--add-host` flags. This example adds a static address for a host named `my-hostname`:

​	可以使用一个或多个 `--add-host` 标志将其他主机添加到容器的 `/etc/hosts` 文件中。此示例为主机 `my-hostname` 添加了一个静态地址：



```console
$ docker run --add-host=my-hostname=8.8.8.8 --rm -it alpine

/ # ping my-hostname
PING my-hostname (8.8.8.8): 56 data bytes
64 bytes from 8.8.8.8: seq=0 ttl=37 time=93.052 ms
64 bytes from 8.8.8.8: seq=1 ttl=37 time=92.467 ms
64 bytes from 8.8.8.8: seq=2 ttl=37 time=92.252 ms
^C
--- my-hostname ping statistics ---
4 packets transmitted, 4 packets received, 0% packet loss
round-trip min/avg/max = 92.209/92.495/93.052 ms
```

You can wrap an IPv6 address in square brackets:

​	可以将 IPv6 地址用方括号包裹：

```console
$ docker run --add-host my-hostname=[2001:db8::33] --rm -it alpine
```

The `--add-host` flag supports a special `host-gateway` value that resolves to the internal IP address of the host. This is useful when you want containers to connect to services running on the host machine.

​	`--add-host` 标志支持一个特殊的 `host-gateway` 值，解析为主机的内部 IP 地址。当需要容器连接运行在主机上的服务时，这非常有用。

It's conventional to use `host.docker.internal` as the hostname referring to `host-gateway`. Docker Desktop automatically resolves this hostname, see [Explore networking features](https://docs.docker.com/desktop/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host).

​	按照惯例，`host.docker.internal` 被用作指向 `host-gateway` 的主机名。Docker Desktop 会自动解析此主机名，详见 [探索网络功能](https://docs.docker.com/desktop/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host)。

The following example shows how the special `host-gateway` value works. The example runs an HTTP server that serves a file from host to container over the `host.docker.internal` hostname, which resolves to the host's internal IP.

​	以下示例展示了特殊的 `host-gateway` 值的工作方式。该示例通过 `host.docker.internal` 主机名（解析为主机的内部 IP）运行一个 HTTP 服务器，将主机的文件服务传输到容器中。



```console
$ echo "hello from host!" > ./hello
$ python3 -m http.server 8000
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
$ docker run \
  --add-host host.docker.internal=host-gateway \
  curlimages/curl -s host.docker.internal:8000/hello
hello from host!
```

The `--add-host` flag also accepts a `:` separator, for example:

​	`--add-host` 标志还接受 `:` 分隔符，例如：

```console
$ docker run --add-host=my-hostname:8.8.8.8 --rm -it alpine
```

### 日志驱动 (`--log-driver`) Logging drivers (`--log-driver`)

The container can have a different logging driver than the Docker daemon. Use the `--log-driver=<DRIVER>` with the `docker run` command to configure the container's logging driver.

​	容器可以使用不同于 Docker 守护进程的日志驱动。使用 `--log-driver=<DRIVER>` 和 `docker run` 命令配置容器的日志驱动。

To learn about the supported logging drivers and how to use them, refer to [Configure logging drivers]({{< ref "/manuals/DockerEngine/Logsandmetrics/Configureloggingdrivers" >}}).

​	要了解支持的日志驱动及其使用方式，请参阅 [配置日志驱动]({{< ref "/manuals/DockerEngine/Logsandmetrics/Configureloggingdrivers" >}})。

To disable logging for a container, set the `--log-driver` flag to `none`:

​	要禁用容器日志记录，将 `--log-driver` 标志设置为 `none`：

```console
$ docker run --log-driver=none -d nginx:alpine
5101d3b7fe931c27c2ba0e65fd989654d297393ad65ae238f20b97a020e7295b
$ docker logs 5101d3b
Error response from daemon: configured logging driver does not support reading
```

### 在容器中设置 ulimits (`--ulimit`) Set ulimits in container (`--ulimit`)

Since setting `ulimit` settings in a container requires extra privileges not available in the default container, you can set these using the `--ulimit` flag. Specify `--ulimit` with a soft and hard limit in the format `<type>=<soft limit>[:<hard limit>]`. For example:

​	由于在容器中设置 `ulimit` 需要默认容器不可用的额外权限，因此可以使用 `--ulimit` 标志进行设置。使用软限制和硬限制格式 `<type>=<soft limit>[:<hard limit>]` 指定 `--ulimit`。例如：



```console
$ docker run --ulimit nofile=1024:1024 --rm debian sh -c "ulimit -n"
1024
```

> **Note**
>
> If you don't provide a hard limit value, Docker uses the soft limit value for both values. If you don't provide any values, they are inherited from the default `ulimits` set on the daemon.
>
> ​	如果未提供硬限制值，Docker 将软限制值用于两者。如果未提供任何值，则从守护进程的默认 `ulimits` 集合继承。

> **Note**
>
> The `as` option is deprecated. In other words, the following script is not supported:
>
> ​	`as` 选项已弃用。换句话说，以下脚本不受支持：
>
> ```console
> $ docker run -it --ulimit as=1024 fedora /bin/bash
> ```

Docker sends the values to the appropriate OS `syscall` and doesn't perform any byte conversion. Take this into account when setting the values.

​	Docker 将值发送到相应的 OS `syscall`，不执行字节转换。设置值时请考虑这一点。

#### 对 `nproc` 的使用 For `nproc` usage

Be careful setting `nproc` with the `ulimit` flag as Linux uses `nproc` to set the maximum number of processes available to a user, not to a container. For example, start four containers with `daemon` user:

​	使用 `ulimit` 标志设置 `nproc` 时请小心，因为 Linux 使用 `nproc` 设置用户可用的最大进程数，而不是容器。例如，用 `daemon` 用户启动四个容器：



```console
$ docker run -d -u daemon --ulimit nproc=3 busybox top

$ docker run -d -u daemon --ulimit nproc=3 busybox top

$ docker run -d -u daemon --ulimit nproc=3 busybox top

$ docker run -d -u daemon --ulimit nproc=3 busybox top
```

The 4th container fails and reports a "[8] System error: resource temporarily unavailable" error. This fails because the caller set `nproc=3` resulting in the first three containers using up the three processes quota set for the `daemon` user.

​	第 4 个容器会失败并报告 "[8] 系统错误：资源暂时不可用" 错误。失败的原因是调用者设置了 `nproc=3`，导致前三个容器占用了为 `daemon` 用户设置的三个进程配额。

### 使用信号停止容器 (`--stop-signal`) Stop container with signal (`--stop-signal`)

The `--stop-signal` flag sends the system call signal to the container to exit. This signal can be a signal name in the format `SIG<NAME>`, for instance `SIGKILL`, or an unsigned number that matches a position in the kernel's syscall table, for instance `9`.

​	`--stop-signal` 标志向容器发送系统调用信号以退出。此信号可以是格式 `SIG<NAME>` 的信号名称，例如 `SIGKILL`，或匹配内核 syscall 表中位置的无符号数字，例如 `9`。

The default value is defined by [`STOPSIGNAL`](https://docs.docker.com/reference/dockerfile/#stopsignal) in the image, or `SIGTERM` if the image has no `STOPSIGNAL` defined.

​	默认值由镜像中的 [`STOPSIGNAL`](https://docs.docker.com/reference/dockerfile/#stopsignal) 定义，如果镜像没有定义 `STOPSIGNAL`，则为 `SIGTERM`。

### 可选的安全选项 (`--security-opt`) Optional security options (`--security-opt`)

| Option                                    | Description                                                  |
| :---------------------------------------- | :----------------------------------------------------------- |
| `--security-opt="label=user:USER"`        | 为容器设置标签用户 Set the label user for the container      |
| `--security-opt="label=role:ROLE"`        | 为容器设置标签角色 Set the label role for the container      |
| `--security-opt="label=type:TYPE"`        | 为容器设置标签类型 Set the label type for the container      |
| `--security-opt="label=level:LEVEL"`      | 为容器设置标签级别 Set the label level for the container     |
| `--security-opt="label=disable"`          | 关闭容器的标签限制 Turn off label confinement for the container |
| `--security-opt="apparmor=PROFILE"`       | 设置容器要应用的 apparmor 配置文件 Set the apparmor profile to be applied to the container |
| `--security-opt="no-new-privileges=true"` | 禁止容器进程获得新权限 Disable container processes from gaining new privileges |
| `--security-opt="seccomp=unconfined"`     | 关闭容器的 seccomp 限制 Turn off seccomp confinement for the container |
| `--security-opt="seccomp=builtin"`        | 使用容器的默认（内置） seccomp 配置文件。这可用于在守护进程配置了自定义默认配置文件或禁用了 seccomp（"unconfined"）的情况下为容器启用 seccomp。 Use the default (built-in) seccomp profile for the container. This can be used to enable seccomp for a container running on a daemon with a custom default profile set, or with seccomp disabled ("unconfined"). |
| `--security-opt="seccomp=profile.json"`   | 用于 seccomp 过滤的 syscalls 白名单 Json 文件 White-listed syscalls seccomp Json file to be used as a seccomp filter |
| `--security-opt="systempaths=unconfined"` | 关闭容器的系统路径限制（屏蔽路径、只读路径） Turn off confinement for system paths (masked paths, read-only paths) for the container |

The `--security-opt` flag lets you override the default labeling scheme for a container. Specifying the level in the following command allows you to share the same content between containers.

​	`--security-opt` 标志允许覆盖容器的默认标签方案。在以下命令中指定级别可以在容器间共享相同内容。



```console
$ docker run --security-opt label=level:s0:c100,c200 -it fedora bash
```

> **Note**
>
> Automatic translation of MLS labels isn't supported.
>
> ​	不支持 MLS 标签的自动翻译。

To disable the security labeling for a container entirely, you can use `label=disable`:

​	要完全禁用容器的安全标签，可以使用 `label=disable`：



```console
$ docker run --security-opt label=disable -it ubuntu bash
```

If you want a tighter security policy on the processes within a container, you can specify a custom `type` label. The following example runs a container that's only allowed to listen on Apache ports:

​	如果希望对容器内的进程实施更严格的安全策略，可以指定自定义的 `type` 标签。以下示例运行一个仅允许监听 Apache 端口的容器：



```console
$ docker run --security-opt label=type:svirt_apache_t -it ubuntu bash
```

> **Note**
>
> You would have to write policy defining a `svirt_apache_t` type.
>
> ​	您需要编写定义 `svirt_apache_t` 类型的策略。

To prevent your container processes from gaining additional privileges, you can use the following command:

​	要阻止容器进程获得额外权限，可以使用以下命令：



```console
$ docker run --security-opt no-new-privileges -it ubuntu bash
```

This means that commands that raise privileges such as `su` or `sudo` no longer work. It also causes any seccomp filters to be applied later, after privileges have been dropped which may mean you can have a more restrictive set of filters. For more details, see the [kernel documentation](https://www.kernel.org/doc/Documentation/prctl/no_new_privs.txt).

​	这意味着诸如 `su` 或 `sudo` 等提升权限的命令将不再有效。它还会导致在权限被放弃后再应用任何 seccomp 过滤，这可能意味着您可以拥有更严格的过滤集。详细信息请参阅 [内核文档](https://www.kernel.org/doc/Documentation/prctl/no_new_privs.txt)。

On Windows, you can use the `--security-opt` flag to specify the `credentialspec` option. The `credentialspec` must be in the format `file://spec.txt` or `registry://keyname`.

​	在 Windows 上，您可以使用 `--security-opt` 标志指定 `credentialspec` 选项。`credentialspec` 必须采用 `file://spec.txt` 或 `registry://keyname` 格式。

### 使用超时停止容器 (`--stop-timeout`) Stop container with timeout (`--stop-timeout`)

The `--stop-timeout` flag sets the number of seconds to wait for the container to stop after sending the pre-defined (see `--stop-signal`) system call signal. If the container does not exit after the timeout elapses, it's forcibly killed with a `SIGKILL` signal.

​	`--stop-timeout` 标志设置在发送预定义系统调用信号（请参见 `--stop-signal`）后等待容器停止的秒数。如果在超时到期后容器未退出，则强制通过 `SIGKILL` 信号杀死容器。

If you set `--stop-timeout` to `-1`, no timeout is applied, and the daemon waits indefinitely for the container to exit.

​	如果将 `--stop-timeout` 设置为 `-1`，则不适用超时，守护进程将无限期等待容器退出。

The Daemon determines the default, and is 10 seconds for Linux containers, and 30 seconds for Windows containers.

​	默认超时由守护进程确定，Linux 容器为 10 秒，Windows 容器为 30 秒。

### 指定容器的隔离技术 (`--isolation`) Specify isolation technology for container (`--isolation`)

This option is useful in situations where you are running Docker containers on Windows. The `--isolation=<value>` option sets a container's isolation technology. On Linux, the only supported is the `default` option which uses Linux namespaces. These two commands are equivalent on Linux:

​	在 Windows 上运行 Docker 容器时，此选项很有用。`--isolation=<value>` 选项设置容器的隔离技术。在 Linux 上，唯一支持的选项是 `default`，使用 Linux 命名空间。这两个命令在 Linux 上等效：

```console
$ docker run -d busybox top
$ docker run -d --isolation default busybox top
```

On Windows, `--isolation` can take one of these values:

​	在 Windows 上，`--isolation` 可以接受以下值：

| Value     | Description                                                  |
| :-------- | :----------------------------------------------------------- |
| `default` | 使用 Docker 守护进程的 `--exec-opt` 或系统默认值（见下文）。 Use the value specified by the Docker daemon's `--exec-opt` or system default (see below). |
| `process` | 共享内核命名空间隔离。 Shared-kernel namespace isolation.    |
| `hyperv`  | 基于 Hyper-V 管理程序分区的隔离。 Hyper-V hypervisor partition-based isolation. |

The default isolation on Windows server operating systems is `process`, and `hyperv` on Windows client operating systems, such as Windows 10. Process isolation has better performance, but requires that the image and host use the same kernel version.

​	在 Windows 服务器操作系统上，默认隔离是 `process`，而在 Windows 客户端操作系统（如 Windows 10）上是 `hyperv`。进程隔离性能更好，但要求镜像和主机使用相同的内核版本。

On Windows server, assuming the default configuration, these commands are equivalent and result in `process` isolation:

​	在 Windows 服务器上，假设默认配置，以下命令是等效的，并会导致 `process` 隔离：



```powershell
PS C:\> docker run -d microsoft/nanoserver powershell echo process
PS C:\> docker run -d --isolation default microsoft/nanoserver powershell echo process
PS C:\> docker run -d --isolation process microsoft/nanoserver powershell echo process
```

If you have set the `--exec-opt isolation=hyperv` option on the Docker `daemon`, or are running against a Windows client-based daemon, these commands are equivalent and result in `hyperv` isolation:

​	如果在 Docker `daemon` 上设置了 `--exec-opt isolation=hyperv` 选项，或运行的是 Windows 客户端守护进程，则以下命令等效并导致 `hyperv` 隔离：



```powershell
PS C:\> docker run -d microsoft/nanoserver powershell echo hyperv
PS C:\> docker run -d --isolation default microsoft/nanoserver powershell echo hyperv
PS C:\> docker run -d --isolation hyperv microsoft/nanoserver powershell echo hyperv
```

### 设置容器的内存硬限制 (`-m`, `--memory`) Specify hard limits on memory available to containers (`-m`, `--memory`)

These parameters always set an upper limit on the memory available to the container. Linux sets this on the cgroup and applications in a container can query it at `/sys/fs/cgroup/memory/memory.limit_in_bytes`.

​	这些参数始终设置容器可用内存的上限。Linux 将其设置在 cgroup 上，容器中的应用程序可以在 `/sys/fs/cgroup/memory/memory.limit_in_bytes` 查询。

On Windows, this affects containers differently depending on what type of isolation you use.

​	在 Windows 上，这会根据隔离类型对容器产生不同影响。

- With `process` isolation, Windows reports the full memory of the host system, not the limit to applications running inside the container

  对于 `process` 隔离，Windows 报告的是主机系统的全部内存，而不是容器内应用程序的内存限制。

  ```powershell
  PS C:\> docker run -it -m 2GB --isolation=process microsoft/nanoserver powershell Get-ComputerInfo *memory*
  
  CsTotalPhysicalMemory      : 17064509440
  CsPhyicallyInstalledMemory : 16777216
  OsTotalVisibleMemorySize   : 16664560
  OsFreePhysicalMemory       : 14646720
  OsTotalVirtualMemorySize   : 19154928
  OsFreeVirtualMemory        : 17197440
  OsInUseVirtualMemory       : 1957488
  OsMaxProcessMemorySize     : 137438953344
  ```

- With `hyperv` isolation, Windows creates a utility VM that is big enough to hold the memory limit, plus the minimal OS needed to host the container. That size is reported as "Total Physical Memory."

  对于 `hyperv` 隔离，Windows 会创建一个足够大的实用程序虚拟机来容纳内存限制以及托管容器所需的最小 OS。该大小显示为 "Total Physical Memory"。

  ```powershell
  PS C:\> docker run -it -m 2GB --isolation=hyperv microsoft/nanoserver powershell Get-ComputerInfo *memory*
  
  CsTotalPhysicalMemory      : 2683355136
  CsPhyicallyInstalledMemory :
  OsTotalVisibleMemorySize   : 2620464
  OsFreePhysicalMemory       : 2306552
  OsTotalVirtualMemorySize   : 2620464
  OsFreeVirtualMemory        : 2356692
  OsInUseVirtualMemory       : 263772
  OsMaxProcessMemorySize     : 137438953344
  ```

### 在运行时配置命名空间内核参数 (`--sysctl`) Configure namespaced kernel parameters (sysctls) at runtime (`--sysctl`)

The `--sysctl` sets namespaced kernel parameters (sysctls) in the container. For example, to turn on IP forwarding in the containers network namespace, run this command:

​	`--sysctl` 在容器中设置命名空间内核参数（sysctls）。例如，在容器的网络命名空间中开启 IP 转发功能，运行以下命令：

```console
$ docker run --sysctl net.ipv4.ip_forward=1 someimage
```

> **Note**
>
> Not all sysctls are namespaced. Docker does not support changing sysctls inside of a container that also modify the host system. As the kernel evolves we expect to see more sysctls become namespaced.
>
> ​	并非所有 sysctls 都是命名空间的。Docker 不支持修改容器内部的 sysctls，这些修改也会影响主机系统。随着内核的演变，预计会有更多 sysctls 成为命名空间的。

#### 目前支持的 sysctls - Currently supported sysctls

IPC Namespace:

​	IPC 命名空间：

- `kernel.msgmax`, `kernel.msgmnb`, `kernel.msgmni`, `kernel.sem`, `kernel.shmall`, `kernel.shmmax`, `kernel.shmmni`, `kernel.shm_rmid_forced`.
- Sysctls beginning with `fs.mqueue.*`
  - 以 `fs.mqueue.*` 开头的 sysctls。

- If you use the `--ipc=host` option these sysctls are not allowed.
  - 如果使用 `--ipc=host` 选项，这些 sysctls 不允许使用。


Network Namespace:

​	网络命名空间：

- Sysctls beginning with `net.*`
  - 以 `net.*` 开头的 sysctls。
- If you use the `--network=host` option using these sysctls are not allowed.
  - 如果使用 `--network=host` 选项，禁止使用这些 sysctls。
