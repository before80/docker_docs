+++
title = "docker service update"
date = 2024-10-23T14:54:43+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/service/update/](https://docs.docker.com/reference/cli/docker/service/update/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker service update

| Description | Update a service                          |
| :---------- | ----------------------------------------- |
| Usage       | `docker service update [OPTIONS] SERVICE` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 该命令适用于 Swarm 编排器。

## Description

Updates a service as described by the specified parameters. The parameters are the same as [`docker service create`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}}). Refer to the description there for further information.

​	根据指定的参数更新服务。参数与 `docker service create` 中描述的相同。请参阅该文档获取更多信息。

Normally, updating a service will only cause the service's tasks to be replaced with new ones if a change to the service requires recreating the tasks for it to take effect. For example, only changing the `--update-parallelism` setting will not recreate the tasks, because the individual tasks are not affected by this setting. However, the `--force` flag will cause the tasks to be recreated anyway. This can be used to perform a rolling restart without any changes to the service parameters.

​	通常，更新服务只有在服务需要重新创建任务以使更改生效时才会替换其任务。例如，仅更改 `--update-parallelism` 设置不会重建任务，因为该设置不会影响各个任务。但是，使用 `--force` 标志将强制重新创建任务，即使没有服务参数的更改。这可以用来在没有参数变化的情况下执行滚动重启。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。有关管理节点和工作节点的详细信息，请参阅文档中的 Swarm 模式部分。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| `--args`                                                     |         | 服务命令参数 Service command args                            |
| `--cap-add`                                                  |         | API 1.41+ 增加 Linux 能力 API 1.41+ Add Linux capabilities   |
| `--cap-drop`                                                 |         | API 1.41+ 删除 Linux 能力 API 1.41+ Drop Linux capabilities  |
| `--config-add`                                               |         | API 1.30+ 添加或更新服务中的配置文件 API 1.30+ Add or update a config file on a service |
| `--config-rm`                                                |         | API 1.30+ 删除配置文件 API 1.30+ Remove a configuration file |
| `--constraint-add`                                           |         | 添加或更新部署约束 Add or update a placement constraint      |
| `--constraint-rm`                                            |         | 删除部署约束 Remove a constraint                             |
| `--container-label-add`                                      |         | 添加或更新容器标签 Add or update a container label           |
| `--container-label-rm`                                       |         | 按键删除容器标签 Remove a container label by its key         |
| `--credential-spec`                                          |         | API 1.29+ 管理服务账户的凭据规范（仅限 Windows） API 1.29+ Credential spec for managed service account (Windows only) |
| `-d, --detach`                                               |         | API 1.29+ 立即退出而不是等待服务收敛 API 1.29+ Exit immediately instead of waiting for the service to converge |
| `--dns-add`                                                  |         | API 1.25+ 添加或更新自定义 DNS 服务器 API 1.25+ Add or update a custom DNS server |
| `--dns-option-add`                                           |         | API 1.25+ 添加或更新 DNS 选项 API 1.25+ Add or update a DNS option |
| `--dns-option-rm`                                            |         | API 1.25+ 删除 DNS 选项 API 1.25+ Remove a DNS option        |
| `--dns-rm`                                                   |         | API 1.25+ 删除自定义 DNS 服务器 API 1.25+ Remove a custom DNS server |
| `--dns-search-add`                                           |         | API 1.25+ 添加或更新自定义 DNS 搜索域 API 1.25+ Add or update a custom DNS search domain |
| `--dns-search-rm`                                            |         | API 1.25+ 删除 DNS 搜索域 API 1.25+ Remove a DNS search domain |
| `--endpoint-mode`                                            |         | 端点模式（vip 或 dnsrr） Endpoint mode (vip or dnsrr)        |
| `--entrypoint`                                               |         | 覆盖镜像的默认 ENTRYPOINT - Overwrite the default ENTRYPOINT of the image |
| `--env-add`                                                  |         | 添加或更新环境变量 Add or update an environment variable     |
| `--env-rm`                                                   |         | 删除环境变量 Remove an environment variable                  |
| `--force`                                                    |         | API 1.25+ 即使无更改也强制更新 API 1.25+ Force update even if no changes require it |
| `--generic-resource-add`                                     |         | 添加通用资源 Add a Generic resource                          |
| `--generic-resource-rm`                                      |         | 删除通用资源 Remove a Generic resource                       |
| `--group-add`                                                |         | API 1.25+ 为容器添加附加的补充用户组  API 1.25+ Add an additional supplementary user group to the container |
| `--group-rm`                                                 |         | API 1.25+ 从容器中删除之前添加的补充用户组 API 1.25+ Remove a previously added supplementary user group from the container |
| `--health-cmd`                                               |         | API 1.25+ 用于检查健康状况的命令  API 1.25+ Command to run to check health |
| `--health-interval`                                          |         | API 1.25+ 运行检查的时间间隔  API 1.25+ Time between running the check (ms\|s\|m\|h) |
| `--health-retries`                                           |         | API 1.25+ 需要连续失败次数才能报告不健康  API 1.25+ Consecutive failures needed to report unhealthy |
| `--health-start-interval`                                    |         | API 1.44+ 启动期间运行检查的间隔  API 1.44+ Time between running the check during the start period (ms\|s\|m\|h) |
| `--health-start-period`                                      |         | API 1.29+ 容器初始化的启动期间 API 1.29+ Start period for the container to initialize before counting retries towards unstable (ms\|s\|m\|h) |
| `--health-timeout`                                           |         | API 1.25+ 允许单个检查运行的最大时间 API 1.25+ Maximum time to allow one check to run (ms\|s\|m\|h) |
| `--host-add`                                                 |         | API 1.32+ 添加自定义主机到 IP 的映射 API 1.32+ Add a custom host-to-IP mapping (`host:ip`) |
| `--host-rm`                                                  |         | API 1.25+ 删除自定义主机到 IP 的映射 API 1.25+ Remove a custom host-to-IP mapping (`host:ip`) |
| `--hostname`                                                 |         | API 1.25+ 容器主机名 API 1.25+ Container hostname            |
| `--image`                                                    |         | 服务镜像标签 Service image tag                               |
| `--init`                                                     |         | API 1.37+ 在每个服务容器中使用 init 进程以转发信号并清理进程 API 1.37+ Use an init inside each service container to forward signals and reap processes |
| [`--isolation`](https://docs.docker.com/reference/cli/docker/service/update/#isolation) |         | API 1.35+ 服务容器隔离模式 API 1.35+ Service container isolation mode |
| `--label-add`                                                |         | 添加或更新服务标签 Add or update a service label             |
| `--label-rm`                                                 |         | 按键删除标签 Remove a label by its key                       |
| `--limit-cpu`                                                |         | 限制 CPU 数量 Limit CPUs                                     |
| `--limit-memory`                                             |         | 限制内存大小 Limit Memory                                    |
| `--limit-pids`                                               |         | API 1.41+ 限制最大进程数（默认为 0 = unlimited） API 1.41+ Limit maximum number of processes (default 0 = unlimited) |
| `--log-driver`                                               |         | 服务的日志驱动 Logging driver for service                    |
| `--log-opt`                                                  |         | 日志驱动选项 Logging driver options                          |
| `--max-concurrent`                                           |         | API 1.41+ 并发运行的作业任务数量（默认为 `--replicas`） API 1.41+ Number of job tasks to run concurrently (default equal to `--replicas`) |
| [`--mount-add`](https://docs.docker.com/reference/cli/docker/service/update/#mount-add) |         | 添加或更新服务的挂载点 Add or update a mount on a service    |
| `--mount-rm`                                                 |         | 按目标路径删除挂载点 Remove a mount by its target path       |
| [`--network-add`](https://docs.docker.com/reference/cli/docker/service/update/#network-add) |         | API 1.29+ 添加网络 API 1.29+ Add a network                   |
| `--network-rm`                                               |         | API 1.29+ 删除网络 API 1.29+ Remove a network                |
| `--no-healthcheck`                                           |         | API 1.25+ 禁用任何容器指定的健康检查  API 1.25+ Disable any container-specified HEALTHCHECK |
| `--no-resolve-image`                                         |         | API 1.30+ 不查询注册表以解析镜像摘要和支持的平台 API 1.30+ Do not query the registry to resolve image digest and supported platforms |
| `--oom-score-adj`                                            |         | API 1.46+ 调整主机的 OOM 优先级（-1000 到 1000） API 1.46+ Tune host's OOM preferences (-1000 to 1000) |
| `--placement-pref-add`                                       |         | API 1.28+ 添加部署偏好 API 1.28+ Add a placement preference  |
| `--placement-pref-rm`                                        |         | API 1.28+ 删除部署偏好 API 1.28+ Remove a placement preference |
| [`--publish-add`](https://docs.docker.com/reference/cli/docker/service/update/#publish-add) |         | 添加或更新发布的端口 Add or update a published port          |
| `--publish-rm`                                               |         | 按目标端口删除发布的端口 Remove a published port by its target port |
| `-q, --quiet`                                                |         | 抑制进度输出 Suppress progress output                        |
| `--read-only`                                                |         | API 1.28+ 将容器的根文件系统挂载为只读 API 1.28+ Mount the container's root filesystem as read only |
| `--replicas`                                                 |         | 任务数量 Number of tasks                                     |
| `--replicas-max-per-node`                                    |         | API 1.40+ 每节点最大任务数（默认 0 = 不限） API 1.40+ Maximum number of tasks per node (default 0 = unlimited) |
| `--reserve-cpu`                                              |         | 保留 CPU 数量 Reserve CPUs                                   |
| `--reserve-memory`                                           |         | 保留内存大小 Reserve Memory                                  |
| `--restart-condition`                                        |         | 在条件满足时重启（`none`、`on-failure`、`any`） Restart when condition is met (`none`, `on-failure`, `any`) |
| `--restart-delay`                                            |         | 重启尝试之间的延迟 Delay between restart attempts (ns\|us\|ms\|s\|m\|h) |
| `--restart-max-attempts`                                     |         | 重启最大尝试次数 Maximum number of restarts before giving up |
| `--restart-window`                                           |         | 用于评估重启策略的窗口期 Window used to evaluate the restart policy (ns\|us\|ms\|s\|m\|h) |
| [`--rollback`](https://docs.docker.com/reference/cli/docker/service/update/#rollback) |         | API 1.25+ 回滚到上一个配置 API 1.25+ Rollback to previous specification |
| `--rollback-delay`                                           |         | API 1.28+ 任务回滚之间的延迟 API 1.28+ Delay between task rollbacks (ns\|us\|ms\|s\|m\|h) |
| `--rollback-failure-action`                                  |         | API 1.28+ 回滚失败后的操作（`pause`、`continue`） API 1.28+ Action on rollback failure (`pause`, `continue`) |
| `--rollback-max-failure-ratio`                               |         | API 1.28+ 回滚时可容忍的失败率 API 1.28+ Failure rate to tolerate during a rollback |
| `--rollback-monitor`                                         |         | API 1.28+ 每个任务回滚后的监控时长 API 1.28+ Duration after each task rollback to monitor for failure (ns\|us\|ms\|s\|m\|h) |
| `--rollback-order`                                           |         | API 1.29+ 回滚顺序（`start-first`、`stop-first`） API 1.29+ Rollback order (`start-first`, `stop-first`) |
| `--rollback-parallelism`                                     |         | API 1.28+ 回滚时同时回滚的最大任务数（0 表示全部同时回滚） API 1.28+ Maximum number of tasks rolled back simultaneously (0 to roll back all at once) |
| [`--secret-add`](https://docs.docker.com/reference/cli/docker/service/update/#secret-add) |         | API 1.25+ 添加或更新服务的秘密信息 API 1.25+ Add or update a secret on a service |
| `--secret-rm`                                                |         | API 1.25+ 删除秘密信息 API 1.25+ Remove a secret             |
| `--stop-grace-period`                                        |         | 强制终止容器前的等待时间 Time to wait before force killing a container (ns\|us\|ms\|s\|m\|h) |
| `--stop-signal`                                              |         | API 1.28+ 停止容器的信号 API 1.28+ Signal to stop the container |
| `--sysctl-add`                                               |         | API 1.40+ 添加或更新 Sysctl 选项 API 1.40+ Add or update a Sysctl option |
| `--sysctl-rm`                                                |         | API 1.40+ 删除 Sysctl 选项 API 1.40+ Remove a Sysctl option  |
| `-t, --tty`                                                  |         | API 1.25+ 分配伪 TTY  - API 1.25+ Allocate a pseudo-TTY      |
| `--ulimit-add`                                               |         | API 1.41+ 添加或更新 ulimit 选项 API 1.41+ Add or update a ulimit option |
| `--ulimit-rm`                                                |         | API 1.41+ 删除 ulimit 选项 API 1.41+ Remove a ulimit option  |
| `--update-delay`                                             |         | 更新之间的延迟 Delay between updates (ns\|us\|ms\|s\|m\|h)   |
| `--update-failure-action`                                    |         | 更新失败时的操作（`pause`、`continue`、`rollback`） Action on update failure (`pause`, `continue`, `rollback`) |
| `--update-max-failure-ratio`                                 |         | API 1.25+ 更新时可容忍的失败率 API 1.25+ Failure rate to tolerate during an update |
| `--update-monitor`                                           |         | API 1.25+ 每个任务更新后的监控时长 API 1.25+ Duration after each task update to monitor for failure (ns\|us\|ms\|s\|m\|h) |
| `--update-order`                                             |         | API 1.29+ 更新顺序（`start-first`、`stop-first`） API 1.29+ Update order (`start-first`, `stop-first`) |
| [`--update-parallelism`](https://docs.docker.com/reference/cli/docker/service/update/#update-parallelism) |         | 同时更新的任务数量上限（0 表示全部同时更新） Maximum number of tasks updated simultaneously (0 to update all at once) |
| `-u, --user`                                                 |         | 用户名或 UID - Username or UID (format: <name\|uid>[:<group\|gid>]) |
| `--with-registry-auth`                                       |         | 将注册表认证信息发送到 Swarm 代理 Send registry authentication details to swarm agents |
| `-w, --workdir`                                              |         | 容器内的工作目录 Working directory inside the container      |

## Examples

### 更新服务 Update a service



```console
$ docker service update --limit-cpu 2 redis
```

### 无参数更改的滚动重启 Perform a rolling restart with no parameter changes



```console
$ docker service update --force --update-parallelism 1 --update-delay 30s redis
```

In this example, the `--force` flag causes the service's tasks to be shut down and replaced with new ones even though none of the other parameters would normally cause that to happen. The `--update-parallelism 1` setting ensures that only one task is replaced at a time (this is the default behavior). The `--update-delay 30s` setting introduces a 30 second delay between tasks, so that the rolling restart happens gradually.

​	在该示例中，`--force` 标志强制关闭并替换服务的任务，即使其他参数不会引发此操作。`--update-parallelism 1` 设置确保一次仅替换一个任务（这是默认行为）。`--update-delay 30s` 设置在任务之间引入 30 秒的延迟，使滚动重启逐步进行。

### 添加或删除挂载点（`--mount-add`、`--mount-rm`） Add or remove mounts (`--mount-add`, `--mount-rm`)

Use the `--mount-add` or `--mount-rm` options add or remove a service's bind mounts or volumes.

​	使用 `--mount-add` 或 `--mount-rm` 选项添加或删除服务的绑定挂载或卷。

The following example creates a service which mounts the `test-data` volume to `/somewhere`. The next step updates the service to also mount the `other-volume` volume to `/somewhere-else`volume, The last step unmounts the `/somewhere` mount point, effectively removing the `test-data` volume. Each command returns the service name.

​	以下示例创建一个将 `test-data` 卷挂载到 `/somewhere` 的服务。下一步更新服务，另一个卷 `other-volume` 被挂载到 `/somewhere-else`。最后一步卸载 `/somewhere` 挂载点，有效地移除了 `test-data` 卷。每个命令返回服务名。

- The `--mount-add` flag takes the same parameters as the `--mount` flag on `service create`. Refer to the [volumes and bind mounts](https://docs.docker.com/reference/cli/docker/service/create/#mount) section in the `service create` reference for details.
  - `--mount-add` 标志接受与 `service create` 中 `--mount` 标志相同的参数。详细信息请参阅 [卷和绑定挂载](https://docs.docker.com/reference/cli/docker/service/create/#mount)。

- The `--mount-rm` flag takes the `target` path of the mount.
  - `--mount-rm` 标志接受挂载的 `target` 路径。



```console
$ docker service create \
    --name=myservice \
    --mount type=volume,source=test-data,target=/somewhere \
    nginx:alpine

myservice

$ docker service update \
    --mount-add type=volume,source=other-volume,target=/somewhere-else \
    myservice

myservice

$ docker service update --mount-rm /somewhere myservice

myservice
```

### 添加或删除已发布的服务端口（`--publish-add`、`--publish-rm`） Add or remove published service ports (`--publish-add`, `--publish-rm`)

Use the `--publish-add` or `--publish-rm` flags to add or remove a published port for a service. You can use the short or long syntax discussed in the [docker service create](https://docs.docker.com/reference/cli/docker/service/create/#publish) reference.

​	使用 `--publish-add` 或 `--publish-rm` 标志添加或删除服务的已发布端口。可以使用 [docker service create](https://docs.docker.com/reference/cli/docker/service/create/#publish) 参考中的简短或详细语法。

The following example adds a published service port to an existing service.

​	以下示例将已发布的服务端口添加到现有服务。



```console
$ docker service update \
  --publish-add published=8080,target=80 \
  myservice
```

### 添加或删除网络（`--network-add`、`--network-rm`） Add or remove network (`--network-add`, `--network-rm`)

Use the `--network-add` or `--network-rm` flags to add or remove a network for a service. You can use the short or long syntax discussed in the [docker service create](https://docs.docker.com/reference/cli/docker/service/create/#network) reference.

​	使用 `--network-add` 或 `--network-rm` 标志为服务添加或删除网络。可以使用 [docker service create](https://docs.docker.com/reference/cli/docker/service/create/#network) 参考中的简短或详细语法。

The following example adds a new alias name to an existing service already connected to network my-network:

​	以下示例为已连接到 `my-network` 网络的现有服务添加新别名。

```console
$ docker service update \
  --network-rm my-network \
  --network-add name=my-network,alias=web1 \
  myservice
```

### 回滚到服务的上一个版本（`--rollback`） Roll back to the previous version of a service (`--rollback`)

Use the `--rollback` option to roll back to the previous version of the service.

​	使用 `--rollback` 选项回滚到服务的上一个版本。

This will revert the service to the configuration that was in place before the most recent `docker service update` command.

​	这会将服务恢复到最近一次 `docker service update` 命令之前的配置。

The following example updates the number of replicas for the service from 4 to 5, and then rolls back to the previous configuration.

​	以下示例将服务的副本数量从 4 更新到 5，然后回滚到先前的配置。



```console
$ docker service update --replicas=5 web

web

$ docker service ls

ID            NAME  MODE        REPLICAS  IMAGE
80bvrzp6vxf3  web   replicated  0/5       nginx:alpine
```

The following example rolls back the `web` service:

​	以下示例回滚 `web` 服务：



```console
$ docker service update --rollback web

web

$ docker service ls

ID            NAME  MODE        REPLICAS  IMAGE
80bvrzp6vxf3  web   replicated  0/4       nginx:alpine
```

Other options can be combined with `--rollback` as well, for example, `--update-delay 0s` to execute the rollback without a delay between tasks:

​	可以结合其他选项使用 `--rollback`，例如 `--update-delay 0s` 来执行任务之间无延迟的回滚：



```console
$ docker service update \
  --rollback \
  --update-delay 0s
  web

web
```

Services can also be set up to roll back to the previous version automatically when an update fails. To set up a service for automatic rollback, use `--update-failure-action=rollback`. A rollback will be triggered if the fraction of the tasks which failed to update successfully exceeds the value given with `--update-max-failure-ratio`.

​	也可以在服务更新失败时自动回滚到上一个版本。使用 `--update-failure-action=rollback` 设置服务为自动回滚。如果更新成功的任务比例未达到 `--update-max-failure-ratio` 指定的值，则会触发回滚。

The rate, parallelism, and other parameters of a rollback operation are determined by the values passed with the following flags:

​	回滚操作的速率、并发性和其他参数由以下标志决定：

- `--rollback-delay`

- `--rollback-failure-action`
- `--rollback-max-failure-ratio`
- `--rollback-monitor`
- `--rollback-parallelism`

For example, a service set up with `--update-parallelism 1 --rollback-parallelism 3` will update one task at a time during a normal update, but during a rollback, 3 tasks at a time will get rolled back. These rollback parameters are respected both during automatic rollbacks and for rollbacks initiated manually using `--rollback`.

​	例如，设置 `--update-parallelism 1 --rollback-parallelism 3` 的服务将在正常更新时每次更新一个任务，但在回滚时每次回滚 3 个任务。这些回滚参数在自动回滚和手动使用 `--rollback` 启动的回滚中都适用。

### 添加或删除秘密信息（`--secret-add`、`--secret-rm`） Add or remove secrets (`--secret-add`, `--secret-rm`)

Use the `--secret-add` or `--secret-rm` options add or remove a service's secrets.

​	使用 `--secret-add` 或 `--secret-rm` 选项添加或删除服务的秘密信息。

The following example adds a secret named `ssh-2` and removes `ssh-1`:

​	以下示例添加名为 `ssh-2` 的秘密并删除 `ssh-1`：



```console
$ docker service update \
    --secret-add source=ssh-2,target=ssh-2 \
    --secret-rm ssh-1 \
    myservice
```

### 使用模板更新服务 Update services using templates

Some flags of `service update` support the use of templating. See [`service create`](https://docs.docker.com/reference/cli/docker/service/create/#create-services-using-templates) for the reference.

​	`service update` 的某些标志支持使用模板。请参阅 [`service create`](https://docs.docker.com/reference/cli/docker/service/create/#create-services-using-templates) 以获取参考。

### 指定 Windows 上的隔离模式（`--isolation`） Specify isolation mode on Windows (`--isolation`)

`service update` supports the same `--isolation` flag as `service create` See [`service create`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}}) for the reference.

​	`service update` 支持与 `service create` 相同的 `--isolation` 标志。请参阅 `service create` 以获取参考。

### 更新作业 Updating Jobs

When a service is created as a job, by setting its mode to `replicated-job` or to `global-job` when doing `service create`, options for updating it are limited.

​	当服务在 `service create` 中将其模式设置为 `replicated-job` 或 `global-job` 创建为作业时，更新选项受到限制。

Updating a Job immediately stops any Tasks that are in progress. The operation creates a new set of Tasks for the job and effectively resets its completion status. If any Tasks were running before the update, they are stopped, and new Tasks are created.

​	更新作业会立即停止任何正在进行的任务。此操作将为作业创建一组新任务，有效地重置其完成状态。如果在更新前有任务正在运行，它们将被停止，并创建新任务。

Jobs cannot be rolled out or rolled back. None of the flags for configuring update or rollback settings are valid with job modes.

​	作业不能滚动发布或回滚。作业模式不支持配置更新或回滚的任何标志。

To run a job again with the same parameters that it was run previously, it can be force updated with the `--force` flag.

​	要以先前的参数重新运行作业，可以使用 `--force` 标志强制更新。
