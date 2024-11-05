+++
title = "docker service create"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/service/create/](https://docs.docker.com/reference/cli/docker/service/create/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker service create

| Description | Create a new service                                       |
| :---------- | ---------------------------------------------------------- |
| Usage       | `docker service create [OPTIONS] IMAGE [COMMAND] [ARG...]` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 该命令适用于 Swarm 协调器。

## Description

Creates a service as described by the specified parameters.

​	根据指定的参数创建服务。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理节点和工作节点，请参考文档中的 Swarm 模式部分。

## Options

| Option                                                       | Default      | Description                                                  |
| ------------------------------------------------------------ | ------------ | ------------------------------------------------------------ |
| `--cap-add`                                                  |              | API 1.41+ 添加 Linux 能力 API 1.41+ Add Linux capabilities   |
| `--cap-drop`                                                 |              | API 1.41+ 删除 Linux 能力 API 1.41+ Drop Linux capabilities  |
| [`--config`](https://docs.docker.com/reference/cli/docker/service/create/#config) |              | API 1.30+ 指定要暴露给服务的配置 API 1.30+ Specify configurations to expose to the service |
| [`--constraint`](https://docs.docker.com/reference/cli/docker/service/create/#constraint) |              | 放置约束 Placement constraints                               |
| `--container-label`                                          |              | 容器标签 Container labels                                    |
| `--credential-spec`                                          |              | API 1.29+ 用于托管服务帐户的凭据规范（仅适用于 Windows） API 1.29+ Credential spec for managed service account (Windows only) |
| `-d, --detach`                                               |              | API 1.29+ 立即退出，而不是等待服务收敛 API 1.29+ Exit immediately instead of waiting for the service to converge |
| `--dns`                                                      |              | API 1.25+ 设置自定义 DNS 服务器 API 1.25+ Set custom DNS servers |
| `--dns-option`                                               |              | API 1.25+ 设置 DNS 选项 API 1.25+ Set DNS options            |
| `--dns-search`                                               |              | API 1.25+ 设置自定义 DNS 搜索域 API 1.25+ Set custom DNS search domains |
| `--endpoint-mode`                                            | `vip`        | 端点模式（vip 或 dnsrr） Endpoint mode (vip or dnsrr)        |
| `--entrypoint`                                               |              | 覆盖镜像的默认 ENTRYPOINT Overwrite the default ENTRYPOINT of the image |
| [`-e, --env`](https://docs.docker.com/reference/cli/docker/service/create/#env) |              | 设置环境变量 Set environment variables                       |
| `--env-file`                                                 |              | 从文件中读取环境变量 Read in a file of environment variables |
| `--generic-resource`                                         |              | 用户定义的资源 User defined resources                        |
| `--group`                                                    |              | API 1.25+ 为容器设置一个或多个附加用户组 API 1.25+ Set one or more supplementary user groups for the container |
| `--health-cmd`                                               |              | API 1.25+ 运行的健康检查命令 API 1.25+ Command to run to check health |
| `--health-interval`                                          |              | API 1.25+ 运行检查的时间间隔(ms\|s\|m\|h)  API 1.25+ Time between running the check (ms\|s\|m\|h) |
| `--health-retries`                                           |              | API 1.25+ 连续失败次数以报告不健康 API 1.25+ Consecutive failures needed to report unhealthy |
| `--health-start-interval`                                    |              | API 1.44+ 启动期间运行检查的间隔时间(ms\|s\|m\|h)  API 1.44+ Time between running the check during the start period (ms\|s\|m\|h) |
| `--health-start-period`                                      |              | API 1.29+ 容器初始化的启动期(ms\|s\|m\|h)  API 1.29+ Start period for the container to initialize before counting retries towards unstable (ms\|s\|m\|h) |
| `--health-timeout`                                           |              | API 1.25+ 检查运行的最大时间 (ms\|s\|m\|h) API 1.25+ Maximum time to allow one check to run (ms\|s\|m\|h) |
| `--host`                                                     |              | API 1.25+ 设置一个或多个自定义主机到 IP 的映射 (host:ip)  API 1.25+ Set one or more custom host-to-IP mappings (host:ip) |
| [`--hostname`](https://docs.docker.com/reference/cli/docker/service/create/#hostname) |              | API 1.25+ 容器主机名 API 1.25+ Container hostname            |
| `--init`                                                     |              | API 1.37+ 在每个服务容器中使用 init 以转发信号和收割进程 API 1.37+ Use an init inside each service container to forward signals and reap processes |
| [`--isolation`](https://docs.docker.com/reference/cli/docker/service/create/#isolation) |              | API 1.35+ 服务容器隔离模式 API 1.35+ Service container isolation mode |
| [`-l, --label`](https://docs.docker.com/reference/cli/docker/service/create/#label) |              | 服务标签 Service labels                                      |
| `--limit-cpu`                                                |              | 限制 CPU 使用 Limit CPUs                                     |
| `--limit-memory`                                             |              | 限制内存使用 Limit Memory                                    |
| `--limit-pids`                                               |              | API 1.41+ 限制进程最大数量（默认 0 = 无限制） API 1.41+ Limit maximum number of processes (default 0 = unlimited) |
| `--log-driver`                                               |              | 服务的日志驱动 Logging driver for service                    |
| `--log-opt`                                                  |              | 日志驱动选项 Logging driver options                          |
| `--max-concurrent`                                           |              | API 1.41+ 运行并发作业任务的数量（默认与 --replicas 相同） API 1.41+ Number of job tasks to run concurrently (default equal to --replicas) |
| `--mode`                                                     | `replicated` | 服务模式（`replicated`、`global`、`replicated-job`、`global-job`） Service mode (`replicated`, `global`, `replicated-job`, `global-job`) |
| [`--mount`](https://docs.docker.com/reference/cli/docker/service/create/#mount) |              | 将文件系统挂载到服务 Attach a filesystem mount to the service |
| `--name`                                                     |              | 服务名称 Service name                                        |
| [`--network`](https://docs.docker.com/reference/cli/docker/service/create/#network) |              | 网络附件 Network attachments                                 |
| `--no-healthcheck`                                           |              | API 1.25+ 禁用容器指定的任何健康检查 API 1.25+ Disable any container-specified HEALTHCHECK |
| `--no-resolve-image`                                         |              | API 1.30+ 不查询注册表以解析镜像摘要和支持的平台 API 1.30+ Do not query the registry to resolve image digest and supported platforms |
| `--oom-score-adj`                                            |              | API 1.46+ 调整主机的 OOM 偏好（-1000 到 1000）  API 1.46+ Tune host's OOM preferences (-1000 to 1000) |
| [`--placement-pref`](https://docs.docker.com/reference/cli/docker/service/create/#placement-pref) |              | API 1.28+ 添加放置优先级 API 1.28+ Add a placement preference |
| [`-p, --publish`](https://docs.docker.com/reference/cli/docker/service/create/#publish) |              | 将端口发布为节点端口 Publish a port as a node port           |
| `-q, --quiet`                                                |              | 抑制进度输出 Suppress progress output                        |
| `--read-only`                                                |              | API 1.28+ 将容器的根文件系统设置为只读 API 1.28+ Mount the container's root filesystem as read only |
| [`--replicas`](https://docs.docker.com/reference/cli/docker/service/create/#replicas) |              | 任务数量  Number of tasks                                    |
| [`--replicas-max-per-node`](https://docs.docker.com/reference/cli/docker/service/create/#replicas-max-per-node) |              | API 1.40+ 每个节点的任务最大数量（默认 0 = 无限） API 1.40+ Maximum number of tasks per node (default 0 = unlimited) |
| `--reserve-cpu`                                              |              | 保留 CPU Reserve CPUs                                        |
| [`--reserve-memory`](https://docs.docker.com/reference/cli/docker/service/create/#reserve-memory) |              | 保留内存  Reserve Memory                                     |
| `--restart-condition`                                        |              | 满足条件时重启（`none`、`on-failure`、`any`）（默认 `any`） Restart when condition is met (`none`, `on-failure`, `any`) (default `any`) |
| `--restart-delay`                                            |              | 重启尝试之间的延迟 (ns\|us\|ms\|s\|m\|h) (default 5s) Delay between restart attempts (ns\|us\|ms\|s\|m\|h) (default 5s) |
| `--restart-max-attempts`                                     |              | 最大重启次数 Maximum number of restarts before giving up     |
| `--restart-window`                                           |              | 用于评估重启策略的窗口(ns\|us\|ms\|s\|m\|h)   Window used to evaluate the restart policy (ns\|us\|ms\|s\|m\|h) |
| `--rollback-delay`                                           |              | API 1.28+ 任务回滚之间的延迟(ns\|us\|ms\|s\|m\|h) (default 0s)  API 1.28+ Delay between task rollbacks (ns\|us\|ms\|s\|m\|h) (default 0s) |
| `--rollback-failure-action`                                  |              | API 1.28+ 回滚失败时的操作（`pause`、`continue`）（默认 `pause`） API 1.28+ Action on rollback failure (`pause`, `continue`) (default `pause`) |
| `--rollback-max-failure-ratio`                               |              | API 1.28+ 回滚期间容忍的失败率（默认 0） API 1.28+ Failure rate to tolerate during a rollback (default 0) |
| `--rollback-monitor`                                         |              | API 1.28+ 每次任务回滚后监视失败的持续时间 (ns\|us\|ms\|s\|m\|h) (default 5s)  API 1.28+ Duration after each task rollback to monitor for failure (ns\|us\|ms\|s\|m\|h) (default 5s) |
| `--rollback-order`                                           |              | API 1.29+ 回滚顺序（`start-first`、`stop-first`）（默认 `stop-first`） API 1.29+ Rollback order (`start-first`, `stop-first`) (default `stop-first`) |
| `--rollback-parallelism`                                     | `1`          | API 1.28+ 同时回滚的任务最大数量（0 表示一次性回滚全部） API 1.28+ Maximum number of tasks rolled back simultaneously (0 to roll back all at once) |
| [`--secret`](https://docs.docker.com/reference/cli/docker/service/create/#secret) |              | API 1.25+ 指定要暴露给服务的机密 API 1.25+ Specify secrets to expose to the service |
| `--stop-grace-period`                                        |              | 强制终止容器前等待的时间 (ns\|us\|ms\|s\|m\|h) (default 10s) Time to wait before force killing a container (ns\|us\|ms\|s\|m\|h) (default 10s) |
| `--stop-signal`                                              |              | API 1.28+ 停止容器的信号 API 1.28+ Signal to stop the container |
| `--sysctl`                                                   |              | API 1.40+ Sysctl 选项  API 1.40+ Sysctl options              |
| `-t, --tty`                                                  |              | API 1.25+ 分配一个伪 TTY  API 1.25+ Allocate a pseudo-TTY    |
| `--ulimit`                                                   |              | API 1.41+ Ulimit 选项  API 1.41+ Ulimit options              |
| [`--update-delay`](https://docs.docker.com/reference/cli/docker/service/create/#update-delay) |              | 更新之间的延迟 (ns\|us\|ms\|s\|m\|h) (default 0s)   Delay between updates (ns\|us\|ms\|s\|m\|h) (default 0s) |
| `--update-failure-action`                                    |              | 更新失败时的操作（`pause`、`continue`、`rollback`）（默认 `pause`）  Action on update failure (`pause`, `continue`, `rollback`) (default `pause`) |
| `--update-max-failure-ratio`                                 |              | API 1.25+ 更新期间容忍的失败率（默认 0） API 1.25+ Failure rate to tolerate during an update (default 0) |
| `--update-monitor`                                           |              | API 1.25+ 每次任务更新后监视失败的持续时间 (ns\|us\|ms\|s\|m\|h) (default 5s)  API 1.25+ Duration after each task update to monitor for failure (ns\|us\|ms\|s\|m\|h) (default 5s) |
| `--update-order`                                             |              | API 1.29+ 更新顺序（`start-first`、`stop-first`）（默认 `stop-first`）  API 1.29+ Update order (`start-first`, `stop-first`) (default `stop-first`) |
| `--update-parallelism`                                       | `1`          | 同时更新的任务最大数量（0 表示一次性更新全部） Maximum number of tasks updated simultaneously (0 to update all at once) |
| `-u, --user`                                                 |              | 用户名或 UID     - Username or UID (format: <name\|uid>[:<group\|gid>]) |
| [`--with-registry-auth`](https://docs.docker.com/reference/cli/docker/service/create/#with-registry-auth) |              | 将注册表认证详细信息发送到 Swarm 代理 Send registry authentication details to swarm agents |
| `-w, --workdir`                                              |              | 容器内的工作目录  Working directory inside the container     |

## Examples

### 创建一个服务 Create a service



```console
$ docker service create --name redis redis:3.0.6

dmu1ept4cxcfe8k8lhtux3ro3

$ docker service create --mode global --name redis2 redis:3.0.6

a8q9dasaafudfs8q8w32udass

$ docker service ls

ID            NAME    MODE        REPLICAS  IMAGE
dmu1ept4cxcf  redis   replicated  1/1       redis:3.0.6
a8q9dasaafud  redis2  global      1/1       redis:3.0.6
```

#### 使用私有注册表中的镜像创建服务（`--with-registry-auth`） Create a service using an image on a private registry (`--with-registry-auth`)

If your image is available on a private registry which requires login, use the `--with-registry-auth` flag with `docker service create`, after logging in. If your image is stored on `registry.example.com`, which is a private registry, use a command like the following:

​	如果镜像在需要登录的私有注册表中，请在 `docker service create` 时使用 `--with-registry-auth` 标志，并在登录后运行此命令。假设镜像存储在私有注册表 `registry.example.com` 中，示例命令如下：



```console
$ docker login registry.example.com

$ docker service  create \
  --with-registry-auth \
  --name my_service \
  registry.example.com/acme/my_image:latest
```

This passes the login token from your local client to the swarm nodes where the service is deployed, using the encrypted WAL logs. With this information, the nodes are able to log in to the registry and pull the image.

​	该命令将登录令牌从本地客户端传递到部署服务的 Swarm 节点，节点使用加密的 WAL 日志登录注册表并拉取镜像。

### 使用 5 个副本任务创建服务（`--replicas`） Create a service with 5 replica tasks (`--replicas`)

Use the `--replicas` flag to set the number of replica tasks for a replicated service. The following command creates a `redis` service with `5` replica tasks:

​	使用 `--replicas` 标志为复制服务设置副本任务数量。以下命令创建一个 `redis` 服务，并设置 `5` 个副本任务：

```console
$ docker service create --name redis --replicas=5 redis:3.0.6

4cdgfyky7ozwh3htjfw0d12qv
```

The above command sets the *desired* number of tasks for the service. Even though the command returns immediately, actual scaling of the service may take some time. The `REPLICAS` column shows both the actual and desired number of replica tasks for the service.

​	上面的命令设置了服务的*期望*任务数量。尽管命令会立即返回，但实际服务的扩展可能需要一些时间。`REPLICAS` 列显示服务的实际副本任务数和期望的副本任务数。

In the following example the desired state is `5` replicas, but the current number of `RUNNING` tasks is `3`:

​	以下示例中，期望状态为 `5` 个副本，但当前 `RUNNING` 的任务数为 `3`：

```console
$ docker service ls

ID            NAME   MODE        REPLICAS  IMAGE
4cdgfyky7ozw  redis  replicated  3/5       redis:3.0.7
```

Once all the tasks are created and `RUNNING`, the actual number of tasks is equal to the desired number:

​	一旦所有任务创建并 `RUNNING`，实际任务数将等于期望任务数：

```console
$ docker service ls

ID            NAME   MODE        REPLICAS  IMAGE
4cdgfyky7ozw  redis  replicated  5/5       redis:3.0.7
```

### 使用机密创建服务（`--secret`） Create a service with secrets (`--secret`)

Use the `--secret` flag to give a container access to a [secret]({{< ref "/reference/CLIreference/docker/dockersecret/dockersecretcreate" >}}).

​	使用 `--secret` 标志允许容器访问 机密。

Create a service specifying a secret:

​	指定机密创建服务：

```console
$ docker service create --name redis --secret secret.json redis:3.0.6

4cdgfyky7ozwh3htjfw0d12qv
```

Create a service specifying the secret, target, user/group ID, and mode:

​	指定机密、目标、用户/组 ID 和模式创建服务：

```console
$ docker service create --name redis \
    --secret source=ssh-key,target=ssh \
    --secret source=app-key,target=app,uid=1000,gid=1001,mode=0400 \
    redis:3.0.6

4cdgfyky7ozwh3htjfw0d12qv
```

To grant a service access to multiple secrets, use multiple `--secret` flags.

​	要为服务授予多个机密的访问权限，请使用多个 `--secret` 标志。

Secrets are located in `/run/secrets` in the container if no target is specified. If no target is specified, the name of the secret is used as the in memory file in the container. If a target is specified, that is used as the filename. In the example above, two files are created: `/run/secrets/ssh` and `/run/secrets/app` for each of the secret targets specified.

​	如果未指定目标，机密将位于容器中的 `/run/secrets`。如果未指定目标，机密名称将作为容器内的内存文件名。如果指定了目标，则使用目标作为文件名。在上面的示例中，为每个指定的机密目标创建了两个文件：`/run/secrets/ssh` 和 `/run/secrets/app`。

### 使用配置创建服务（`--config`） Create a service with configs (`--config`)

Use the `--config` flag to give a container access to a [config]({{< ref "/reference/CLIreference/docker/dockerconfig/dockerconfigcreate" >}}).

​	使用 `--config` 标志允许容器访问 配置。

Create a service with a config. The config will be mounted into `redis-config`, be owned by the user who runs the command inside the container (often `root`), and have file mode `0444` or world-readable. You can specify the `uid` and `gid` as numerical IDs or names. When using names, the provided group/user names must pre-exist in the container. The `mode` is specified as a 4-number sequence such as `0755`.

​	通过配置创建服务。配置将挂载到 `redis-config`，由容器内运行命令的用户拥有（通常是 `root`），并具有文件模式 `0444` 或全局可读。可以将 `uid` 和 `gid` 指定为数字 ID 或名称。使用名称时，所提供的组/用户名必须已存在于容器中。模式以 4 位数格式（如 `0755`）指定。



```console
$ docker service create --name=redis --config redis-conf redis:3.0.6
```

Create a service with a config and specify the target location and file mode:

​	通过指定目标位置和文件模式创建具有配置的服务：



```console
$ docker service create --name redis \
  --config source=redis-conf,target=/etc/redis/redis.conf,mode=0400 redis:3.0.6
```

To grant a service access to multiple configs, use multiple `--config` flags.

​	要为服务授予多个配置的访问权限，请使用多个 `--config` 标志。

Configs are located in `/` in the container if no target is specified. If no target is specified, the name of the config is used as the name of the file in the container. If a target is specified, that is used as the filename.

​	如果未指定目标，配置将位于容器中的 `/`。如果未指定目标，则配置名称将用作容器内文件的名称。如果指定了目标，则使用目标作为文件名。

### 使用滚动更新策略创建服务 Create a service with a rolling update policy



```console
$ docker service create \
  --replicas 10 \
  --name redis \
  --update-delay 10s \
  --update-parallelism 2 \
  redis:3.0.6
```

When you run a [service update]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceupdate" >}}), the scheduler updates a maximum of 2 tasks at a time, with `10s` between updates. For more information, refer to the [rolling updates tutorial]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Applyrollingupdatestoaservice" >}}).

​	当您运行 服务更新 时，调度程序一次最多更新 2 个任务，更新间隔为 `10s`。有关更多信息，请参考滚动更新教程。

### 设置环境变量 (`-e, --env`) Set environment variables (`-e, --env`)

This sets an environment variable for all tasks in a service. For example:

​	此命令为服务中的所有任务设置环境变量。例如：



```console
$ docker service create \
  --name redis_2 \
  --replicas 5 \
  --env MYVAR=foo \
  redis:3.0.6
```

To specify multiple environment variables, specify multiple `--env` flags, each with a separate key-value pair.

​	要指定多个环境变量，需分别指定多个 `--env` 标志，每个标志包含一个键值对。



```console
$ docker service create \
  --name redis_2 \
  --replicas 5 \
  --env MYVAR=foo \
  --env MYVAR2=bar \
  redis:3.0.6
```

### 使用特定主机名创建服务 (`--hostname`) Create a service with specific hostname (`--hostname`)

This option sets the docker service containers hostname to a specific string. For example:

​	该选项将 Docker 服务容器的主机名设置为特定字符串。例如：



```console
$ docker service create --name redis --hostname myredis redis:3.0.6
```

### 在服务上设置元数据 (`-l, --label`) Set metadata on a service (`-l, --label`)

A label is a `key=value` pair that applies metadata to a service. To label a service with two labels:

​	标签是一个 `key=value` 对，用于为服务应用元数据。要为服务添加两个标签：

```console
$ docker service create \
  --name redis_2 \
  --label com.example.foo="bar" \
  --label bar=baz \
  redis:3.0.6
```

For more information about labels, refer to [apply custom metadata](https://docs.docker.com/config/labels-custom-metadata/).

​	有关标签的更多信息，请参阅[应用自定义元数据](https://docs.docker.com/config/labels-custom-metadata/)。

### 添加绑定挂载、卷或内存文件系统 (`--mount`) Add bind mounts, volumes or memory filesystems (`--mount`)

Docker supports three different kinds of mounts, which allow containers to read from or write to files or directories, either on the host operating system, or on memory filesystems. These types are data volumes (often referred to simply as volumes), bind mounts, tmpfs, and named pipes.

​	Docker 支持三种不同类型的挂载，它们允许容器从主机操作系统或内存文件系统中读取或写入文件或目录。这些类型包括数据卷（通常简称为卷）、绑定挂载、tmpfs 和命名管道。

A **bind mount** makes a file or directory on the host available to the container it is mounted within. A bind mount may be either read-only or read-write. For example, a container might share its host's DNS information by means of a bind mount of the host's `/etc/resolv.conf` or a container might write logs to its host's `/var/log/myContainerLogs` directory. If you use bind mounts and your host and containers have different notions of permissions, access controls, or other such details, you will run into portability issues.

​	**绑定挂载**将主机上的文件或目录提供给容器。绑定挂载可以是只读或读写。例如，容器可以通过绑定挂载主机的 `/etc/resolv.conf` 文件来共享主机的 DNS 信息，或者将日志写入主机的 `/var/log/myContainerLogs` 目录。如果使用绑定挂载，并且主机和容器对权限、访问控制等有不同的概念，可能会遇到兼容性问题。

A **named volume** is a mechanism for decoupling persistent data needed by your container from the image used to create the container and from the host machine. Named volumes are created and managed by Docker, and a named volume persists even when no container is currently using it. Data in named volumes can be shared between a container and the host machine, as well as between multiple containers. Docker uses a *volume driver* to create, manage, and mount volumes. You can back up or restore volumes using Docker commands.

​	**命名卷**是一种机制，用于将容器所需的持久数据与创建容器的镜像及主机分离。命名卷由 Docker 创建和管理，即使没有容器使用它也会持续存在。命名卷中的数据可以在容器和主机之间共享，也可以在多个容器之间共享。Docker 使用*卷驱动程序*来创建、管理和挂载卷。您可以使用 Docker 命令备份或恢复卷。

A **tmpfs** mounts a tmpfs inside a container for volatile data.

​	**tmpfs**在容器内挂载一个 tmpfs，用于存储易失数据。

A **npipe** mounts a named pipe from the host into the container.

​	**npipe**将主机中的命名管道挂载到容器中。

Consider a situation where your image starts a lightweight web server. You could use that image as a base image, copy in your website's HTML files, and package that into another image. Each time your website changed, you'd need to update the new image and redeploy all of the containers serving your website. A better solution is to store the website in a named volume which is attached to each of your web server containers when they start. To update the website, you just update the named volume.

​	假设您的镜像启动了一个轻量级的 Web 服务器。您可以将该镜像用作基础镜像，复制网站的 HTML 文件，并将其打包成另一个镜像。每当网站更改时，您需要更新镜像并重新部署所有服务网站的容器。更好的解决方案是将网站存储在一个命名卷中，并在每次启动 Web 服务器容器时将其附加。这样更新网站时，只需更新命名卷即可。

For more information about named volumes, see [Data Volumes](https://docs.docker.com/storage/volumes/).

​	有关命名卷的更多信息，请参阅[数据卷](https://docs.docker.com/storage/volumes/)。

The following table describes options which apply to both bind mounts and named volumes in a service:

​	下表描述了适用于服务中的绑定挂载和命名卷的选项：

| Option                                   | Required                         | Description                                                  |
| ---------------------------------------- | -------------------------------- | ------------------------------------------------------------ |
| **type**                                 |                                  | 挂载类型，可以是 `volume`、`bind`、`tmpfs` 或 `npipe`。如果未指定类型，则默认为 `volume`。`volume`：将托管卷挂载到容器中。`bind`：将主机上的目录或文件绑定挂载到容器中。`tmpfs`：在容器中挂载 tmpfs。`npipe`：将主机中的命名管道挂载到容器中（仅限 Windows 容器）。 The type of mount, can be either `volume`, `bind`, `tmpfs`, or `npipe`. Defaults to `volume` if no type is specified.`volume`: mounts a [managed volume]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumecreate" >}}) into the container.`bind`: bind-mounts a directory or file from the host into the container.`tmpfs`: mount a tmpfs in the container`npipe`: mounts named pipe from the host into the container (Windows containers only). |
| **src** or **source**                    | for `type=bind` and `type=npipe` | `type=volume`：`src` 是指定卷名称的可选方式（例如 `src=my-volume`）。如果命名卷不存在，将自动创建。如果未指定 `src`，卷会被分配一个随机名称，该名称在主机上唯一，但可能在集群中不唯一。随机命名的卷与其容器具有相同的生命周期，当容器销毁时，卷也会被销毁（例如在 `service update` 时，或在服务扩展或重新平衡时）。`type=bind`：`src` 是必需的，指定要绑定挂载的文件或目录的绝对路径（例如 `src=/path/on/host/`）。如果文件或目录不存在，将产生错误。`type=tmpfs`：`src` 不支持。  `type=volume`: `src` is an optional way to specify the name of the volume (for example, `src=my-volume`). If the named volume does not exist, it is automatically created. If no `src` is specified, the volume is assigned a random name which is guaranteed to be unique on the host, but may not be unique cluster-wide. A randomly-named volume has the same lifecycle as its container and is destroyed when the *container* is destroyed (which is upon `service update`, or when scaling or re-balancing the service)`type=bind`: `src` is required, and specifies an absolute path to the file or directory to bind-mount (for example, `src=/path/on/host/`). An error is produced if the file or directory does not exist.`type=tmpfs`: `src` is not supported. |
| **dst** or **destination** or **target** | yes                              | 容器内的挂载路径，例如 `/some/path/in/container/`。如果该路径在容器文件系统中不存在，Docker 引擎会在指定位置创建目录，然后再挂载卷或绑定挂载。 Mount path inside the container, for example `/some/path/in/container/`. If the path does not exist in the container's filesystem, the Engine creates a directory at the specified location before mounting the volume or bind mount. |
| **readonly** or **ro**                   |                                  | 默认情况下，绑定和卷是 `读写` 挂载的，除非在挂载时指定了 `readonly` 选项。注意，设置绑定挂载的 `readonly` 可能不会使其子挂载也只读，这取决于内核版本。另见 `bind-recursive`。`true` 或 `1` 或无值：只读挂载绑定或卷。`false` 或 `0`：读写挂载绑定或卷。 The Engine mounts binds and volumes `read-write` unless `readonly` option is given when mounting the bind or volume. Note that setting `readonly` for a bind-mount may not make its submounts `readonly` depending on the kernel version. See also `bind-recursive`.`true` or `1` or no value: Mounts the bind or volume read-only.`false` or `0`: Mounts the bind or volume read-write. |

#### 绑定挂载选项 Options for bind mounts

The following options can only be used for bind mounts (`type=bind`):

​	以下选项仅适用于绑定挂载（`type=bind`）：

| Option                | Description                                                  |
| --------------------- | ------------------------------------------------------------ |
| **bind-propagation**  | 查看[绑定传播部分](https://docs.docker.com/reference/cli/docker/service/create/#bind-propagation)。 See the [bind propagation section](https://docs.docker.com/reference/cli/docker/service/create/#bind-propagation). |
| **consistency**       | 挂载的一致性要求。`default`：等同于 `consistent`。`consistent`：完全一致性。容器运行时和主机始终保持挂载视图相同。`cached`：主机对挂载的视图是权威的。主机上的更新在容器内可见可能有延迟。`delegated`：容器运行时对挂载的视图是权威的。在主机上可见容器内的更新可能有延迟。  The consistency requirements for the mount; one of`default`: Equivalent to `consistent`.`consistent`: Full consistency. The container runtime and the host maintain an identical view of the mount at all times.`cached`: The host's view of the mount is authoritative. There may be delays before updates made on the host are visible within a container.`delegated`: The container runtime's view of the mount is authoritative. There may be delays before updates made in a container are visible on the host. |
| **bind-recursive**    | 默认情况下，子挂载也是递归绑定挂载的。然而，当绑定挂载配置为 `readonly` 选项时，这种行为可能会导致混淆，因为子挂载可能不会被设为只读，具体取决于内核版本。设置 `bind-recursive` 来控制递归绑定挂载的行为。值可以为：`enabled`：启用递归绑定挂载。只读挂载在内核 v5.12 或更高版本时会递归设为只读，否则不会。`disabled`：禁用递归绑定挂载。`writable`：启用递归绑定挂载。只读挂载不会递归设为只读。`readonly`：启用递归绑定挂载。只读挂载在内核 v5.12 或更高版本时会递归设为只读，否则引擎会报错。若未指定此选项，默认行为为 `enabled`。 By default, submounts are recursively bind-mounted as well. However, this behavior can be confusing when a bind mount is configured with `readonly` option, because submounts may not be mounted as read-only, depending on the kernel version. Set `bind-recursive` to control the behavior of the recursive bind-mount.  A value is one of:  <`enabled`: Enables recursive bind-mount. Read-only mounts are made recursively read-only if kernel is v5.12 or later. Otherwise they are not made recursively read-only.<`disabled`: Disables recursive bind-mount.<`writable`: Enables recursive bind-mount. Read-only mounts are not made recursively read-only.<`readonly`: Enables recursive bind-mount. Read-only mounts are made recursively read-only if kernel is v5.12 or later. Otherwise the Engine raises an error.When the option is not specified, the default behavior correponds to setting `enabled`. |
| **bind-nonrecursive** | 自 Docker Engine v25.0 起，`bind-nonrecursive` 已弃用。请改用 `bind-recursive`。该值是可选的：`true` 或 `1`：等同于 `bind-recursive=disabled`。`false` 或 `0`：等同于 `bind-recursive=enabled`。 `bind-nonrecursive` is deprecated since Docker Engine v25.0. Use `bind-recursive`instead.  A value is optional:  `true` or `1`: Equivalent to `bind-recursive=disabled`.`false` or `0`: Equivalent to `bind-recursive=enabled`. |

##### 绑定传播 Bind propagation

Bind propagation refers to whether or not mounts created within a given bind mount or named volume can be propagated to replicas of that mount. Consider a mount point `/mnt`, which is also mounted on `/tmp`. The propagation settings control whether a mount on `/tmp/a` would also be available on `/mnt/a`. Each propagation setting has a recursive counterpoint. In the case of recursion, consider that `/tmp/a` is also mounted as `/foo`. The propagation settings control whether `/mnt/a` and/or `/tmp/a` would exist.

​	绑定传播决定了在给定绑定挂载或命名卷内创建的挂载是否可以传播到该挂载的副本。考虑一个挂载点 `/mnt`，它也被挂载在 `/tmp` 上。传播设置控制 `/tmp/a` 上的挂载是否也可在 `/mnt/a` 上访问。每个传播设置都有一个递归对应项。对于递归情况，考虑 `/tmp/a` 也被挂载为 `/foo`。传播设置控制 `/mnt/a` 和/或 `/tmp/a` 是否存在。

The `bind-propagation` option defaults to `rprivate` for both bind mounts and volume mounts, and is only configurable for bind mounts. In other words, named volumes do not support bind propagation.

​	`bind-propagation` 选项在绑定挂载和卷挂载上默认为 `rprivate`，且仅绑定挂载可配置。换句话说，命名卷不支持绑定传播。

- **`shared`**: Sub-mounts of the original mount are exposed to replica mounts, and sub-mounts of replica mounts are also propagated to the original mount.
  - **`shared`**：原始挂载的子挂载对副本挂载可见，副本挂载的子挂载也会传播到原始挂载。

- **`slave`**: similar to a shared mount, but only in one direction. If the original mount exposes a sub-mount, the replica mount can see it. However, if the replica mount exposes a sub-mount, the original mount cannot see it.
  - **`slave`**：与共享挂载类似，但仅单向。如果原始挂载公开了子挂载，副本挂载可以看到它。然而，如果副本挂载公开了子挂载，原始挂载无法看到它。

- **`private`**: The mount is private. Sub-mounts within it are not exposed to replica mounts, and sub-mounts of replica mounts are not exposed to the original mount.
  - **`private`**：挂载是私有的。内部的子挂载对副本挂载不可见，副本挂载的子挂载对原始挂载不可见。

- **`rshared`**: The same as shared, but the propagation also extends to and from mount points nested within any of the original or replica mount points.
  - **`rshared`**：与共享类似，但传播也扩展到嵌套在任何原始或副本挂载点内的挂载点。

- **`rslave`**: The same as `slave`, but the propagation also extends to and from mount points nested within any of the original or replica mount points.
  - **`rslave`**：与 `slave` 相同，但传播也扩展到嵌套在任何原始或副本挂载点内的挂载点。

- **`rprivate`**: The default. The same as `private`, meaning that no mount points anywhere within the original or replica mount points propagate in either direction.
  - **`rprivate`**：默认值，与 `private` 相同，意味着无论在原始或副本挂载点内的任何挂载点都不会传播。


For more information about bind propagation, see the [Linux kernel documentation for shared subtree](https://www.kernel.org/doc/Documentation/filesystems/sharedsubtree.txt).

​	有关绑定传播的更多信息，请参阅 [Linux 内核文档](https://www.kernel.org/doc/Documentation/filesystems/sharedsubtree.txt)。

#### 命名卷选项 Options for named volumes

The following options can only be used for named volumes (`type=volume`):

​	以下选项仅适用于命名卷（`type=volume`）：

| Option            | Description                                                  |
| ----------------- | ------------------------------------------------------------ |
| **volume-driver** | 卷驱动程序插件的名称。默认值为 `"local"`，用于创建卷（如果卷不存在）。 Name of the volume-driver plugin to use for the volume. Defaults to `"local"`, to use the local volume driver to create the volume if the volume does not exist. |
| **volume-label**  | 一个或多个自定义元数据（“标签”），在创建卷时应用。例如，`volume-label=mylabel=hello-world,my-other-label=hello-mars`。有关标签的更多信息，请参阅[应用自定义元数据](https://docs.docker.com/config/labels-custom-metadata/)。 One or more custom metadata ("labels") to apply to the volume upon creation. For example, `volume-label=mylabel=hello-world,my-other-label=hello-mars`. For more information about labels, refer to [apply custom metadata](https://docs.docker.com/config/labels-custom-metadata/). |
| **volume-nocopy** | 默认情况下，如果将一个空卷附加到容器，并且在容器的挂载路径中（`dst`）已存在文件或目录，则引擎会将这些文件和目录复制到卷中，允许主机访问它们。设置 `volume-nocopy` 以禁用从容器文件系统复制文件到卷中并挂载空卷。 该值是可选的：`true` 或 `1`：如果未提供值，则为默认值。禁用复制。`false` 或 `0`：启用复制。 By default, if you attach an empty volume to a container, and files or directories already existed at the mount-path in the container (`dst`), the Engine copies those files and directories into the volume, allowing the host to access them. Set `volume-nocopy` to disable copying files from the container's filesystem to the volume and mount the empty volume.  A value is optional:  `true` or `1`: Default if you do not provide a value. Disables copying.`false` or `0`: Enables copying. |
| **volume-opt**    | 特定于给定卷驱动程序的选项，在创建卷时传递给驱动程序。选项以逗号分隔的键/值对列表提供，例如 `volume-opt=some-option=some-value,volume-opt=some-other-option=some-other-value`。有关给定驱动程序的可用选项，请参阅该驱动程序的文档。  Options specific to a given volume driver, which will be passed to the driver when creating the volume. Options are provided as a comma-separated list of key/value pairs, for example, `volume-opt=some-option=some-value,volume-opt=some-other-option=some-other-value`. For available options for a given driver, refer to that driver's documentation. |

#### tmpfs 选项 Options for tmpfs

The following options can only be used for tmpfs mounts (`type=tmpfs`);

​	以下选项仅适用于 tmpfs 挂载（`type=tmpfs`）：

| Option         | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| **tmpfs-size** | tmpfs 挂载的大小（以字节为单位）。在 Linux 中默认无限制。 Size of the tmpfs mount in bytes. Unlimited by default in Linux. |
| **tmpfs-mode** | tmpfs 的文件模式，八进制格式（例如 `"700"` 或 `"0700"`）。在 Linux 中默认为 `"1777"`。 File mode of the tmpfs in octal. (e.g. `"700"` or `"0700"`.) Defaults to `"1777"` in Linux. |

#### "`--mount`"  和  "`--volume`"之间的区别 Differences between "`--mount`" and "`--volume`"

The `--mount` flag supports most options that are supported by the `-v` or `--volume` flag for `docker run`, with some important exceptions:

​	`--mount` 标志支持 `docker run` 中的 `-v` 或 `--volume` 标志支持的大多数选项，但有一些重要例外：

- The `--mount` flag allows you to specify a volume driver and volume driver options *per volume*, without creating the volumes in advance. In contrast, `docker run` allows you to specify a single volume driver which is shared by all volumes, using the `--volume-driver` flag.
  - `--mount` 标志允许您为每个卷指定卷驱动程序和卷驱动程序选项，而无需提前创建卷。相比之下，`docker run` 允许您使用 `--volume-driver` 标志为所有卷指定单个卷驱动程序。

- The `--mount` flag allows you to specify custom metadata ("labels") for a volume, before the volume is created.

  - `--mount` 标志允许您在卷创建之前为卷指定自定义元数据（“标签”）。

- When you use `--mount` with `type=bind`, the host-path must refer to an *existing* path on the host. The path will not be created for you and the service will fail with an error if the path does not exist.

  - 当您将 `--mount` 与 `type=bind` 一起使用时，主机路径必须指向主机上的*现有*路径。该路径不会自动创建，如果路径不存在，服务将因错误而失败。

- The `--mount` flag does not allow you to relabel a volume with `Z` or `z` flags, which are used for `selinux` labeling.

  - `--mount` 标志不允许您使用 `Z` 或 `z` 标志重新标记卷，这些标志用于 `selinux` 标签。

  

#### 使用命名卷创建服务 Create a service using a named volume

The following example creates a service that uses a named volume:

​	以下示例创建了一个使用命名卷的服务：

```console
$ docker service create \
  --name my-service \
  --replicas 3 \
  --mount type=volume,source=my-volume,destination=/path/in/container,volume-label="color=red",volume-label="shape=round" \
  nginx:alpine
```

For each replica of the service, the engine requests a volume named "my-volume" from the default ("local") volume driver where the task is deployed. If the volume does not exist, the engine creates a new volume and applies the "color" and "shape" labels.

​	服务的每个副本在任务部署的地方从默认的 `local` 卷驱动程序请求一个名为 "my-volume" 的卷。如果该卷不存在，引擎会创建一个新卷并应用 "color" 和 "shape" 标签。

When the task is started, the volume is mounted on `/path/in/container/` inside the container.

​	在任务启动时，卷会挂载到容器内的 `/path/in/container/` 路径。

Be aware that the default ("local") volume is a locally scoped volume driver. This means that depending on where a task is deployed, either that task gets a *new* volume named "my-volume", or shares the same "my-volume" with other tasks of the same service. Multiple containers writing to a single shared volume can cause data corruption if the software running inside the container is not designed to handle concurrent processes writing to the same location. Also take into account that containers can be re-scheduled by the Swarm orchestrator and be deployed on a different node.

​	需要注意的是，默认的 “local” 卷是一个本地范围的卷驱动程序。这意味着，根据任务的部署位置，任务可能会获得一个名为 "my-volume" 的*新*卷，或者与同一服务的其他任务共享相同的 "my-volume"。多个容器写入单个共享卷可能会导致数据损坏，尤其是当容器内运行的软件未设计用于处理并发进程写入同一位置时。此外，还需要考虑到 Swarm 协调器可能会重新调度容器并将其部署到其他节点上。

#### 创建使用匿名卷的服务 Create a service that uses an anonymous volume

The following command creates a service with three replicas with an anonymous volume on `/path/in/container`:

​	以下命令创建了一个具有匿名卷的服务，包含三个副本，挂载在 `/path/in/container` 路径下：



```console
$ docker service create \
  --name my-service \
  --replicas 3 \
  --mount type=volume,destination=/path/in/container \
  nginx:alpine
```

In this example, no name (`source`) is specified for the volume, so a new volume is created for each task. This guarantees that each task gets its own volume, and volumes are not shared between tasks. Anonymous volumes are removed after the task using them is complete.

​	在此示例中，卷未指定名称（`source`），因此为每个任务创建一个新卷。这保证了每个任务都有自己的卷，卷在任务之间不共享。匿名卷在使用它的任务完成后会被移除。

#### 创建使用绑定挂载主机目录的服务 Create a service that uses a bind-mounted host directory

The following example bind-mounts a host directory at `/path/in/container` in the containers backing the service:

​	以下示例将主机目录绑定挂载到容器的 `/path/in/container` 中：

```console
$ docker service create \
  --name my-service \
  --mount type=bind,source=/path/on/host,destination=/path/in/container \
  nginx:alpine
```

### 设置服务模式 (`--mode`) Set service mode (`--mode`)

The service mode determines whether this is a *replicated* service or a *global* service. A replicated service runs as many tasks as specified, while a global service runs on each active node in the swarm.

​	服务模式决定该服务是*复制*模式还是*全局*模式。复制服务运行指定数量的任务，而全局服务则在 Swarm 中的每个活动节点上运行。

The following command creates a global service:

​	以下命令创建了一个全局服务：

```console
$ docker service create \
 --name redis_2 \
 --mode global \
 redis:3.0.6
```

### 指定服务约束 (`--constraint`) Specify service constraints (`--constraint`)

You can limit the set of nodes where a task can be scheduled by defining constraint expressions. Constraint expressions can either use a *match* (`==`) or *exclude* (`!=`) rule. Multiple constraints find nodes that satisfy every expression (AND match). Constraints can match node or Docker Engine labels as follows:

​	您可以通过定义约束表达式限制任务可以被调度到的节点集合。约束表达式可以使用*匹配* (`==`) 或*排除* (`!=`) 规则。多个约束要求满足每个表达式（AND 匹配）。约束可以匹配节点或 Docker 引擎标签，如下所示：

| node attribute       | matches                                                      | example                                       |
| -------------------- | ------------------------------------------------------------ | --------------------------------------------- |
| `node.id`            | 节点 ID Node ID                                              | `node.id==2ivku8v2gvtg4`                      |
| `node.hostname`      | 节点主机名 Node hostname                                     | `node.hostname!=node-2`                       |
| `node.role`          | 节点角色 (`manager`/`worker`) Node role (`manager`/`worker`) | `node.role==manager`                          |
| `node.platform.os`   | 节点操作系统 Node operating system                           | `node.platform.os==windows`                   |
| `node.platform.arch` | 节点架构 Node architecture                                   | `node.platform.arch==x86_64`                  |
| `node.labels`        | 用户定义的节点标签 User-defined node labels                  | `node.labels.security==high`                  |
| `engine.labels`      | Docker 引擎标签 Docker Engine's labels                       | `engine.labels.operatingsystem==ubuntu-24.04` |

`engine.labels` apply to Docker Engine labels like operating system, drivers, etc. Swarm administrators add `node.labels` for operational purposes by using the [`docker node update`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodeupdate" >}}) command.

​	`engine.labels` 适用于操作系统、驱动程序等的 Docker 引擎标签。Swarm 管理员可以使用 `docker node update` 命令添加 `node.labels` 以便于运维。

For example, the following limits tasks for the redis service to nodes where the node type label equals queue:

​	例如，以下命令将 redis 服务的任务限制在节点类型标签等于 queue 的节点上：



```console
$ docker service create \
  --name redis_2 \
  --constraint node.platform.os==linux \
  --constraint node.labels.type==queue \
  redis:3.0.6
```

If the service constraints exclude all nodes in the cluster, a message is printed that no suitable node is found, but the scheduler will start a reconciliation loop and deploy the service once a suitable node becomes available.

​	如果服务的约束排除了集群中的所有节点，则会显示一条消息，提示没有找到合适的节点，但调度器会开始执行协调循环，一旦有适合的节点可用就会部署服务。

In the example below, no node satisfying the constraint was found, causing the service to not reconcile with the desired state:

​	在以下示例中，未找到符合约束的节点，导致服务无法满足期望状态：



```console
$ docker service create \
  --name web \
  --constraint node.labels.region==east \
  nginx:alpine

lx1wrhhpmbbu0wuk0ybws30bc
overall progress: 0 out of 1 tasks
1/1: no suitable node (scheduling constraints not satisfied on 5 nodes)

$ docker service ls
ID                  NAME     MODE         REPLICAS   IMAGE               PORTS
b6lww17hrr4e        web      replicated   0/1        nginx:alpine
```

After adding the `region=east` label to a node in the cluster, the service reconciles, and the desired number of replicas are deployed:

​	在集群中的某个节点上添加 `region=east` 标签后，服务达成协调，部署了期望数量的副本：



```console
$ docker node update --label-add region=east yswe2dm4c5fdgtsrli1e8ya5l
yswe2dm4c5fdgtsrli1e8ya5l

$ docker service ls
ID                  NAME     MODE         REPLICAS   IMAGE               PORTS
b6lww17hrr4e        web      replicated   1/1        nginx:alpine
```

### 指定服务的放置偏好 (`--placement-pref`) Specify service placement preferences (`--placement-pref`)

You can set up the service to divide tasks evenly over different categories of nodes. One example of where this can be useful is to balance tasks over a set of datacenters or availability zones. The example below illustrates this:

​	您可以设置服务以在不同节点类别之间平均分配任务。例如，您可以在一组数据中心或可用区之间平衡任务。以下示例说明了这种情况：



```console
$ docker service create \
  --replicas 9 \
  --name redis_2 \
  --placement-pref spread=node.labels.datacenter \
  redis:3.0.6
```

This uses `--placement-pref` with a `spread` strategy (currently the only supported strategy) to spread tasks evenly over the values of the `datacenter` node label. In this example, we assume that every node has a `datacenter` node label attached to it. If there are three different values of this label among nodes in the swarm, one third of the tasks will be placed on the nodes associated with each value. This is true even if there are more nodes with one value than another. For example, consider the following set of nodes:

​	这使用了 `--placement-pref` 和 `spread` 策略（当前唯一支持的策略）来根据 `datacenter` 节点标签的值均匀分布任务。在此示例中，我们假设每个节点都有一个 `datacenter` 标签。如果集群中该标签有三个不同的值，那么任务将被平分到与每个值相关的节点上。即使某个标签值的节点比其他标签值的节点多也是如此。例如，考虑以下节点集合：

- Three nodes with `node.labels.datacenter=east`
  - 三个节点带有 `node.labels.datacenter=east`

- Two nodes with `node.labels.datacenter=south`
  - 两个节点带有 `node.labels.datacenter=south`

- One node with `node.labels.datacenter=west`
  - 一个节点带有 `node.labels.datacenter=west`


Since we are spreading over the values of the `datacenter` label and the service has 9 replicas, 3 replicas will end up in each datacenter. There are three nodes associated with the value `east`, so each one will get one of the three replicas reserved for this value. There are two nodes with the value `south`, and the three replicas for this value will be divided between them, with one receiving two replicas and another receiving just one. Finally, `west` has a single node that will get all three replicas reserved for `west`.

​	由于任务基于 `datacenter` 标签的值进行分布，且服务有 9 个副本，因此每个数据中心将分配到 3 个副本。与 `east` 值关联的三个节点，每个节点会分配一个副本；两个 `south` 节点分配三个副本，其中一个获得两个副本，另一个获得一个；`west` 节点是单节点，将获得 `west` 值的全部三个副本。

If the nodes in one category (for example, those with `node.labels.datacenter=south`) can't handle their fair share of tasks due to constraints or resource limitations, the extra tasks will be assigned to other nodes instead, if possible.

​	如果某一类别的节点（例如 `node.labels.datacenter=south`）由于约束或资源限制无法处理其任务分配，额外的任务将被分配到其他节点上（如果可能）。

Both engine labels and node labels are supported by placement preferences. The example above uses a node label, because the label is referenced with `node.labels.datacenter`. To spread over the values of an engine label, use `--placement-pref spread=engine.labels.<labelname>`.

​	引擎标签和节点标签均支持放置偏好。上述示例使用节点标签，标签通过 `node.labels.datacenter` 引用。若需基于引擎标签值进行分布，请使用 `--placement-pref spread=engine.labels.<labelname>`。

It is possible to add multiple placement preferences to a service. This establishes a hierarchy of preferences, so that tasks are first divided over one category, and then further divided over additional categories. One example of where this may be useful is dividing tasks fairly between datacenters, and then splitting the tasks within each datacenter over a choice of racks. To add multiple placement preferences, specify the `--placement-pref` flag multiple times. The order is significant, and the placement preferences will be applied in the order given when making scheduling decisions.

​	可以为服务添加多个放置偏好，这样可以建立偏好的层级关系，使任务先在一个类别之间分布，再在额外的类别之间进一步细分任务。例如，可在数据中心间公平分配任务，然后在每个数据中心的不同机架间分布任务。要添加多个放置偏好，可多次指定 `--placement-pref` 标志。顺序很重要，放置偏好将在调度决策时按给定顺序应用。

The following example sets up a service with multiple placement preferences. Tasks are spread first over the various datacenters, and then over racks (as indicated by the respective labels):

​	以下示例设置了具有多个放置偏好的服务。任务首先在各数据中心之间分布，然后在各机架间分布（根据相应的标签指示）：

```console
$ docker service create \
  --replicas 9 \
  --name redis_2 \
  --placement-pref 'spread=node.labels.datacenter' \
  --placement-pref 'spread=node.labels.rack' \
  redis:3.0.6
```

When updating a service with `docker service update`, `--placement-pref-add` appends a new placement preference after all existing placement preferences. `--placement-pref-rm` removes an existing placement preference that matches the argument.

​	在使用 `docker service update` 更新服务时，`--placement-pref-add` 将在所有现有放置偏好之后附加新的放置偏好。`--placement-pref-rm` 则移除与参数匹配的现有放置偏好。

### 为服务指定内存需求和约束（`--reserve-memory` 和 `--limit-memory`） Specify memory requirements and constraints for a service (`--reserve-memory` and `--limit-memory`)

If your service needs a minimum amount of memory in order to run correctly, you can use `--reserve-memory` to specify that the service should only be scheduled on a node with this much memory available to reserve. If no node is available that meets the criteria, the task is not scheduled, but remains in a pending state.

​	如果您的服务需要最低内存才能正确运行，可以使用 `--reserve-memory` 指定服务仅在可保留此内存的节点上调度。如果没有符合条件的节点，任务不会被调度，而是保持待定状态。

The following example requires that 4GB of memory be available and reservable on a given node before scheduling the service to run on that node.

​	以下示例要求在调度服务之前，节点上需有 4GB 的内存可用并保留：

```console
$ docker service create --reserve-memory=4GB --name=too-big nginx:alpine
```

The managers won't schedule a set of containers on a single node whose combined reservations exceed the memory available on that node.

​	管理器不会在单个节点上调度超出该节点可用内存总量的容器集合。

After a task is scheduled and running, `--reserve-memory` does not enforce a memory limit. Use `--limit-memory` to ensure that a task uses no more than a given amount of memory on a node. This example limits the amount of memory used by the task to 4GB. The task will be scheduled even if each of your nodes has only 2GB of memory, because `--limit-memory` is an upper limit.

​	在任务调度和运行后，`--reserve-memory` 不会强制执行内存上限。使用 `--limit-memory` 确保任务在节点上使用的内存不超过指定的值。此示例将任务的内存使用限制为 4GB。即使每个节点只有 2GB 内存，任务仍会被调度，因为 `--limit-memory` 是上限。



```console
$ docker service create --limit-memory=4GB --name=too-big nginx:alpine
```

Using `--reserve-memory` and `--limit-memory` does not guarantee that Docker will not use more memory on your host than you want. For instance, you could create many services, the sum of whose memory usage could exhaust the available memory.

​	使用 `--reserve-memory` 和 `--limit-memory` 并不能保证 Docker 不会使用超过预期的主机内存。例如，您可能创建了多个服务，导致其总内存使用量耗尽了可用内存。

You can prevent this scenario from exhausting the available memory by taking into account other (non-containerized) software running on the host as well. If `--reserve-memory` is greater than or equal to `--limit-memory`, Docker won't schedule a service on a host that doesn't have enough memory. `--limit-memory` will limit the service's memory to stay within that limit, so if every service has a memory-reservation and limit set, Docker services will be less likely to saturate the host. Other non-service containers or applications running directly on the Docker host could still exhaust memory.

​	您可以通过考虑在主机上运行的其他（非容器化）软件来防止内存耗尽。如果 `--reserve-memory` 大于或等于 `--limit-memory`，Docker 将不会在内存不足的主机上调度服务。`--limit-memory` 会限制服务的内存使用在该上限内，如果每个服务都设置了内存保留和限制，则 Docker 服务将较少填满主机。但直接在 Docker 主机上运行的非服务容器或应用程序仍可能导致内存耗尽。

There is a downside to this approach. Reserving memory also means that you may not make optimum use of the memory available on the node. Consider a service that under normal circumstances uses 100MB of memory, but depending on load can "peak" at 500MB. Reserving 500MB for that service (to guarantee can have 500MB for those "peaks") results in 400MB of memory being wasted most of the time.

​	这种方式的缺点在于，保留内存可能会导致节点上的内存不能得到最佳利用。考虑一个通常使用 100MB 内存的服务，但在负载高峰时可能“峰值”达到 500MB。为该服务保留 500MB（以确保有足够内存应对“峰值”）大多数时间会浪费 400MB 的内存。

In short, you can take a more conservative or more flexible approach:

​	简而言之，可以选择更保守或更灵活的方式：

- **Conservative**: reserve 500MB, and limit to 500MB. Basically you're now treating the service containers as VMs, and you may be losing a big advantage containers, which is greater density of services per host.
  - **保守**：保留 500MB，并限制在 500MB。这实际上是在将服务容器视为虚拟机，从而可能失去容器的一个优势，即更高的服务密度。

- **Flexible**: limit to 500MB in the assumption that if the service requires more than 500MB, it is malfunctioning. Reserve something between the 100MB "normal" requirement and the 500MB "peak" requirement". This assumes that when this service is at "peak", other services or non-container workloads probably won't be.
  - **灵活**：限制在 500MB，假设如果服务需要超过 500MB 的内存则可能出现故障。保留在 100MB 到 500MB 的“峰值”需求之间的某个值。假定在服务处于“峰值”时，其他服务或非容器化工作负载可能不会存在。


The approach you take depends heavily on the memory-usage patterns of your workloads. You should test under normal and peak conditions before settling on an approach.

​	您采取的方式取决于工作负载的内存使用模式。应在正常和峰值条件下测试以决定合适的方式。

On Linux, you can also limit a service's overall memory footprint on a given host at the level of the host operating system, using `cgroups` or other relevant operating system tools.

​	在 Linux 上，还可以使用 `cgroups` 或其他相关的操作系统工具限制服务在特定主机上的总体内存占用。

### 为每个节点指定最大副本数（`--replicas-max-per-node`） Specify maximum replicas per node (`--replicas-max-per-node`)

Use the `--replicas-max-per-node` flag to set the maximum number of replica tasks that can run on a node. The following command creates a nginx service with 2 replica tasks but only one replica task per node.

​	使用 `--replicas-max-per-node` 标志设置每个节点上可以运行的最大副本任务数。以下命令创建了一个 nginx 服务，具有 2 个副本任务，但每个节点上只允许一个副本任务。

One example where this can be useful is to balance tasks over a set of data centers together with `--placement-pref` and let `--replicas-max-per-node` setting make sure that replicas are not migrated to another datacenter during maintenance or datacenter failure.

​	这种方式的一个应用场景是与 `--placement-pref` 结合使用，在多个数据中心之间平衡任务，同时通过 `--replicas-max-per-node` 确保在维护或数据中心故障期间，副本不会迁移到其他数据中心。

The example below illustrates this:

​	以下示例说明了该操作：

```console
$ docker service create \
  --name nginx \
  --replicas 2 \
  --replicas-max-per-node 1 \
  --placement-pref 'spread=node.labels.datacenter' \
  nginx
```

### 将服务附加到现有网络 (`--network`) Attach a service to an existing network (`--network`)

You can use overlay networks to connect one or more services within the swarm.

​	您可以使用覆盖网络连接 Swarm 内的一个或多个服务。

First, create an overlay network on a manager node the docker network create command:

​	首先，在管理节点上使用 `docker network create` 命令创建一个覆盖网络：

```console
$ docker network create --driver overlay my-network

etjpu59cykrptrgw0z0hk5snf
```

After you create an overlay network in swarm mode, all manager nodes have access to the network.

​	在 Swarm 模式中创建覆盖网络后，所有管理节点都可以访问该网络。

When you create a service and pass the `--network` flag to attach the service to the overlay network:

​	创建服务时，通过 `--network` 标志将服务附加到覆盖网络：

```console
$ docker service create \
  --replicas 3 \
  --network my-network \
  --name my-web \
  nginx

716thylsndqma81j6kkkb5aus
```

The swarm extends my-network to each node running the service.

​	Swarm 将 my-network 扩展到运行该服务的每个节点。

Containers on the same network can access each other using [service discovery](https://docs.docker.com/engine/network/drivers/overlay/#container-discovery).

​	同一网络上的容器可以使用[服务发现](https://docs.docker.com/engine/network/drivers/overlay/#container-discovery)进行相互访问。

Long form syntax of `--network` allows to specify list of aliases and driver options: `--network name=my-network,alias=web1,driver-opt=field1=value1`

​	`--network` 的长格式语法允许指定别名和驱动选项列表：`--network name=my-network,alias=web1,driver-opt=field1=value1`

### 向 Swarm 外部发布服务端口 (`-p, --publish`) Publish service ports externally to the swarm (`-p, --publish`)

You can publish service ports to make them available externally to the swarm using the `--publish` flag. The `--publish` flag can take two different styles of arguments. The short version is positional, and allows you to specify the published port and target port separated by a colon (`:`).

​	可以使用 `--publish` 标志发布服务端口，以便它们在 Swarm 外部可用。`--publish` 标志可以接受两种不同样式的参数。短格式是位置参数，允许您指定已发布端口和目标端口，用冒号 (`:`) 分隔。



```console
$ docker service create --name my_web --replicas 3 --publish 8080:80 nginx
```

There is also a long format, which is easier to read and allows you to specify more options. The long format is preferred. You cannot specify the service's mode when using the short format. Here is an example of using the long format for the same service as above:

​	还有一种长格式，更易于阅读，并允许指定更多选项。建议使用长格式。使用短格式时无法指定服务模式。以下是使用长格式的示例：



```console
$ docker service create --name my_web --replicas 3 --publish published=8080,target=80 nginx
```

The options you can specify are:

​	您可以指定的选项如下：

| Option                                     | Short syntax                                               | Long syntax                                       | Description                                                  |
| ------------------------------------------ | ---------------------------------------------------------- | ------------------------------------------------- | ------------------------------------------------------------ |
| published and target port 已发布和目标端口 | `--publish 8080:80`                                        | `--publish published=8080,target=80`              | 容器内的目标端口及节点上的映射端口，使用路由网格 (`ingress`) 或主机级网络。推荐使用键值语法，便于自述。 The target port within the container and the port to map it to on the nodes, using the routing mesh (`ingress`) or host-level networking. More options are available, later in this table. The key-value syntax is preferred, because it is somewhat self-documenting. |
| mode                                       | 无法使用短语法设置 Not possible to set using short syntax. | `--publish published=8080,target=80,mode=host`    | 端口绑定模式，`ingress` 或 `host`。默认为 `ingress` 使用路由网格。 The mode to use for binding the port, either `ingress` or `host`. Defaults to `ingress` to use the routing mesh. |
| protocol                                   | `--publish 8080:80/tcp`                                    | `--publish published=8080,target=80,protocol=tcp` | 使用的协议，`tcp`、`udp` 或 `sctp`。默认为 `tcp`。要绑定到多个协议，需多次指定 `-p` 或 `--publish` 标志。 The protocol to use, `tcp` , `udp`, or `sctp`. Defaults to `tcp`. To bind a port for both protocols, specify the `-p` or `--publish` flag twice. |

When you publish a service port using `ingress` mode, the swarm routing mesh makes the service accessible at the published port on every node regardless if there is a task for the service running on the node. If you use `host` mode, the port is only bound on nodes where the service is running, and a given port on a node can only be bound once. You can only set the publication mode using the long syntax. For more information refer to [Use swarm mode routing mesh]({{< ref "/manuals/DockerEngine/Swarmmode/UseSwarmmoderoutingmesh" >}}).

​	当使用 `ingress` 模式发布服务端口时，Swarm 路由网格使服务在每个节点的已发布端口上可访问，即使该节点上没有运行服务任务。如果使用 `host` 模式，端口只绑定在运行该服务的节点上，且节点上的特定端口只能绑定一次。只能使用长格式设置发布模式。有关更多信息，请参阅使用 Swarm 模式路由网格。

### 为托管服务账户提供凭据规范 (`--credentials-spec`) Provide credential specs for managed service accounts (`--credentials-spec`)

This option is only used for services using Windows containers. The `--credential-spec` must be in the format `file://<filename>` or `registry://<value-name>`.

​	此选项仅适用于使用 Windows 容器的服务。`--credential-spec` 必须采用格式 `file://<filename>` 或 `registry://<value-name>`。

When using the `file://<filename>` format, the referenced file must be present in the `CredentialSpecs` subdirectory in the docker data directory, which defaults to `C:\ProgramData\Docker\` on Windows. For example, specifying `file://spec.json` loads `C:\ProgramData\Docker\CredentialSpecs\spec.json`.

​	使用 `file://<filename>` 格式时，引用的文件必须位于 Docker 数据目录下的 `CredentialSpecs` 子目录中，默认路径为 Windows 上的 `C:\ProgramData\Docker\`。例如，指定 `file://spec.json` 会加载 `C:\ProgramData\Docker\CredentialSpecs\spec.json`。

When using the `registry://<value-name>` format, the credential spec is read from the Windows registry on the daemon's host. The specified registry value must be located in:

​	使用 `registry://<value-name>` 格式时，凭据规范从守护进程主机上的 Windows 注册表中读取。指定的注册表值必须位于：

```
HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Virtualization\Containers\CredentialSpecs
```

### 使用模板创建服务 Create services using templates

You can use templates for some flags of `service create`, using the syntax provided by the Go's [text/template](https://pkg.go.dev/text/template) package.

​	可以为 `service create` 的某些标志使用模板，使用 Go 的 [text/template](https://pkg.go.dev/text/template) 包提供的语法。

The supported flags are the following :

​	支持的标志包括：

- `--hostname`
- `--mount`
- `--env`

Valid placeholders for the Go template are listed below:

​	Go 模板的有效占位符如下：

| Placeholder       | Description               |
| ----------------- | ------------------------- |
| `.Service.ID`     | 服务 ID  Service ID       |
| `.Service.Name`   | 服务名称  Service name    |
| `.Service.Labels` | 服务标签  Service labels  |
| `.Node.ID`        | 节点 ID  Node ID          |
| `.Node.Hostname`  | 节点主机名  Node Hostname |
| `.Task.ID`        | 任务 ID  Task ID          |
| `.Task.Name`      | 任务名称  Task name       |
| `.Task.Slot`      | 任务槽位  Task slot       |

#### 模板示例 Template example

In this example, we are going to set the template of the created containers based on the service's name, the node's ID and hostname where it sits.

​	在此示例中，我们将根据服务名称、节点 ID 和主机名设置创建的容器模板。



```console
$ docker service create \
    --name hosttempl \
    --hostname="{{.Node.Hostname}}-{{.Node.ID}}-{{.Service.Name}}"\
    busybox top

va8ew30grofhjoychbr6iot8c

$ docker service ps va8ew30grofhjoychbr6iot8c

ID            NAME         IMAGE                                                                                   NODE          DESIRED STATE  CURRENT STATE               ERROR  PORTS
wo41w8hg8qan  hosttempl.1  busybox:latest@sha256:29f5d56d12684887bdfa50dcd29fc31eea4aaf4ad3bec43daf19026a7ce69912  2e7a8a9c4da2  Running        Running about a minute ago

$ docker inspect --format="{{.Config.Hostname}}" 2e7a8a9c4da2-wo41w8hg8qanxwjwsg4kxpprj-hosttempl

x3ti0erg11rjpg64m75kej2mz-hosttempl
```

### 指定 Windows 上的隔离模式 (`--isolation`) Specify isolation mode on Windows (`--isolation`)

By default, tasks scheduled on Windows nodes are run using the default isolation mode configured for this particular node. To force a specific isolation mode, you can use the `--isolation` flag:

​	默认情况下，调度在 Windows 节点上的任务使用该节点配置的默认隔离模式。要强制使用特定隔离模式，可以使用 `--isolation` 标志：



```console
$ docker service create --name myservice --isolation=process microsoft/nanoserver
```

Supported isolation modes on Windows are:

​	Windows 上支持的隔离模式包括：

- `default`: use default settings specified on the node running the task
  - `default`：使用运行任务的节点上指定的默认设置

- `process`: use process isolation (Windows server only)

  - `process`：使用进程隔离（仅限 Windows Server）

- `hyperv`: use Hyper-V isolation

  - `hyperv`：使用 Hyper-V 隔离

  

### 请求通用资源的服务 (`--generic-resources`) Create services requesting Generic Resources (`--generic-resources`)

You can narrow the kind of nodes your task can land on through the using the `--generic-resource` flag (if the nodes advertise these resources):

​	可以使用 `--generic-resource` 标志缩小任务可调度的节点范围（如果节点公开这些资源）：

```console
$ docker service create \
    --name cuda \
    --generic-resource "NVIDIA-GPU=2" \
    --generic-resource "SSD=1" \
    nvidia/cuda
```

### 以作业形式运行 Running as a job

Jobs are a special kind of service designed to run an operation to completion and then stop, as opposed to running long-running daemons. When a Task belonging to a job exits successfully (return value 0), the Task is marked as "Completed", and is not run again.

​	作业是一种特殊的服务类型，旨在运行一个操作直至完成，然后停止，与长期运行的守护进程不同。当属于作业的任务成功退出（返回值为 0）时，该任务被标记为“已完成”，不再重新运行。

Jobs are started by using one of two modes, `replicated-job` or `global-job`

​	作业可以通过 `replicated-job` 或 `global-job` 两种模式启动：



```console
$ docker service create --name myjob \
                        --mode replicated-job \
                        bash "true"
```

This command will run one Task, which will, using the `bash` image, execute the command `true`, which will return 0 and then exit.

​	此命令将运行一个任务，该任务使用 `bash` 镜像执行命令 `true`，返回 0 然后退出。

Though Jobs are ultimately a different kind of service, they a couple of caveats compared to other services:

​	尽管作业最终是一种不同的服务类型，与其他服务相比有一些注意事项：

- None of the update or rollback configuration options are valid. Jobs can be updated, but cannot be rolled out or rolled back, making these configuration options moot.
  - 更新或回滚配置选项均无效。作业可以更新，但不能进行回滚或回退，因此这些配置选项没有意义。

- Jobs are never restarted on reaching the `Complete` state. This means that for jobs, setting `--restart-condition` to `any` is the same as setting it to `on-failure`.
  - 作业在达到 `Complete` 状态后不会重启。这意味着对于作业，将 `--restart-condition` 设置为 `any` 等同于设置为 `on-failure`。


Jobs are available in both replicated and global modes.

​	作业可以以复制模式和全局模式运行。

#### 复制作业 Replicated Jobs

A replicated job is like a replicated service. Setting the `--replicas` flag will specify total number of iterations of a job to execute.

​	复制作业类似于复制服务。设置 `--replicas` 标志将指定要执行的作业总数。

By default, all replicas of a replicated job will launch at once. To control the total number of replicas that are executing simultaneously at any one time, the `--max-concurrent` flag can be used:

​	默认情况下，复制作业的所有副本将同时启动。要控制任何时刻正在执行的副本总数，可以使用 `--max-concurrent` 标志：



```console
$ docker service create \
    --name mythrottledjob \
    --mode replicated-job \
    --replicas 10 \
    --max-concurrent 2 \
    bash "true"
```

The above command will execute 10 Tasks in total, but only 2 of them will be run at any given time.

​	上述命令将总共执行 10 个任务，但同时只会运行其中的 2 个任务。

#### 全局作业 Global Jobs

Global jobs are like global services, in that a Task is executed once on each node matching placement constraints. Global jobs are represented by the mode `global-job`.

​	全局作业类似于全局服务，即在匹配放置约束的每个节点上执行一次任务。全局作业由 `global-job` 模式表示。

Note that after a Global job is created, any new Nodes added to the cluster will have a Task from that job started on them. The Global Job does not as a whole have a "done" state, except insofar as every Node meeting the job's constraints has a Completed task.

​	请注意，创建全局作业后，添加到集群的任何新节点都会启动来自该作业的任务。除非所有符合作业约束的节点任务均已完成，否则全局作业整体不会进入“完成”状态。
