+++
title = "docker stack ps"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/stack/ps/](https://docs.docker.com/reference/cli/docker/stack/ps/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker stack ps

| Description | List the tasks in the stack       |
| :---------- | --------------------------------- |
| Usage       | `docker stack ps [OPTIONS] STACK` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 编排器。

## Description

Lists the tasks that are running as part of the specified stack.

​	列出指定栈中的任务。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理器和工作节点，请参阅文档中的 Swarm 模式部分。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --filter`](https://docs.docker.com/reference/cli/docker/stack/ps/#filter) |         | 根据提供的条件过滤输出 Filter output based on conditions provided |
| [`--format`](https://docs.docker.com/reference/cli/docker/stack/ps/#format) |         | 使用自定义模板格式化输出：`table`：以表格式显示输出并带列标题（默认） `table TEMPLATE`：使用指定的 Go 模板以表格式显示输出 `json`：以 JSON 格式显示输出 `TEMPLATE`：使用指定的 Go 模板显示输出。详细信息请参阅 [模板格式化文档](https://docs.docker.com/go/formatting/)。  Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| [`--no-resolve`](https://docs.docker.com/reference/cli/docker/stack/ps/#no-resolve) |         | 不将 ID 映射为名称 Do not map IDs to Names                   |
| [`--no-trunc`](https://docs.docker.com/reference/cli/docker/stack/ps/#no-trunc) |         | 不截断输出  Do not truncate output                           |
| [`-q, --quiet`](https://docs.docker.com/reference/cli/docker/stack/ps/#quiet) |         | 仅显示任务 ID  Only display task IDs                         |

## Examples

### 列出栈中的任务 List the tasks that are part of a stack

The following command shows all the tasks that are part of the `voting` stack:

​	以下命令显示 `voting` 栈中的所有任务：



```console
$ docker stack ps voting

ID                  NAME                  IMAGE                                          NODE   DESIRED STATE  CURRENT STATE          ERROR  PORTS
xim5bcqtgk1b        voting_worker.1       dockersamples/examplevotingapp_worker:latest   node2  Running        Running 2 minutes ago
q7yik0ks1in6        voting_result.1       dockersamples/examplevotingapp_result:before   node1  Running        Running 2 minutes ago
rx5yo0866nfx        voting_vote.1         dockersamples/examplevotingapp_vote:before     node3  Running        Running 2 minutes ago
tz6j82jnwrx7        voting_db.1           postgres:9.4                                   node1  Running        Running 2 minutes ago
w48spazhbmxc        voting_redis.1        redis:alpine                                   node2  Running        Running 3 minutes ago
6jj1m02freg1        voting_visualizer.1   dockersamples/visualizer:stable                node1  Running        Running 2 minutes ago
kqgdmededccb        voting_vote.2         dockersamples/examplevotingapp_vote:before     node2  Running        Running 2 minutes ago
t72q3z038jeh        voting_redis.2        redis:alpine                                   node3  Running        Running 3 minutes ago
```

### Filtering (`--filter`)

The filtering flag (`-f` or `--filter`) format is a `key=value` pair. If there is more than one filter, then pass multiple flags (e.g. `--filter "foo=bar" --filter "bif=baz"`). Multiple filter flags are combined as an `OR` filter. For example, `-f name=redis.1 -f name=redis.7` returns both `redis.1` and `redis.7` tasks.

​	过滤标志 (`-f` 或 `--filter`) 的格式为 `key=value`。若有多个过滤器，请传递多个标志（例如 `--filter "foo=bar" --filter "bif=baz"`）。多个过滤标志组合为 `OR` 过滤。例如，`-f name=redis.1 -f name=redis.7` 返回 `redis.1` 和 `redis.7` 任务。

The currently supported filters are:

​	当前支持的过滤器包括：

- [id](https://docs.docker.com/reference/cli/docker/stack/ps/#id)
- [name](https://docs.docker.com/reference/cli/docker/stack/ps/#name)
- [node](https://docs.docker.com/reference/cli/docker/stack/ps/#node)
- [desired-state](https://docs.docker.com/reference/cli/docker/stack/ps/#desired-state)

#### id

The `id` filter matches on all or a prefix of a task's ID.

​	`id` 过滤器匹配任务 ID 的完整或前缀部分。

```console
$ docker stack ps -f "id=t" voting

ID                  NAME                IMAGE               NODE         DESIRED STATE       CURRENTSTATE            ERROR  PORTS
tz6j82jnwrx7        voting_db.1         postgres:9.4        node1        Running             Running 14 minutes ago
t72q3z038jeh        voting_redis.2      redis:alpine        node3        Running             Running 14 minutes ago
```

#### name

The `name` filter matches on task names.

​	`name` 过滤器匹配任务名称。

```console
$ docker stack ps -f "name=voting_redis" voting

ID                  NAME                IMAGE               NODE         DESIRED STATE       CURRENTSTATE            ERROR  PORTS
w48spazhbmxc        voting_redis.1      redis:alpine        node2        Running             Running 17 minutes ago
t72q3z038jeh        voting_redis.2      redis:alpine        node3        Running             Running 17 minutes ago
```

#### node

The `node` filter matches on a node name or a node ID.

​	`node` 过滤器匹配节点名称或节点 ID。

```console
$ docker stack ps -f "node=node1" voting

ID                  NAME                  IMAGE                                          NODE   DESIRED STATE  CURRENT STATE          ERROR  PORTS
q7yik0ks1in6        voting_result.1       dockersamples/examplevotingapp_result:before   node1  Running        Running 18 minutes ago
tz6j82jnwrx7        voting_db.1           postgres:9.4                                   node1  Running        Running 18 minutes ago
6jj1m02freg1        voting_visualizer.1   dockersamples/visualizer:stable                node1  Running        Running 18 minutes ago
```

#### desired-state

The `desired-state` filter can take the values `running`, `shutdown`, `ready` or `accepted`.

​	`desired-state` 过滤器可取值为 `running`、`shutdown`、`ready` 或 `accepted`。



```console
$ docker stack ps -f "desired-state=running" voting

ID                  NAME                  IMAGE                                          NODE   DESIRED STATE  CURRENT STATE           ERROR  PORTS
xim5bcqtgk1b        voting_worker.1       dockersamples/examplevotingapp_worker:latest   node2  Running        Running 21 minutes ago
q7yik0ks1in6        voting_result.1       dockersamples/examplevotingapp_result:before   node1  Running        Running 21 minutes ago
rx5yo0866nfx        voting_vote.1         dockersamples/examplevotingapp_vote:before     node3  Running        Running 21 minutes ago
tz6j82jnwrx7        voting_db.1           postgres:9.4                                   node1  Running        Running 21 minutes ago
w48spazhbmxc        voting_redis.1        redis:alpine                                   node2  Running        Running 21 minutes ago
6jj1m02freg1        voting_visualizer.1   dockersamples/visualizer:stable                node1  Running        Running 21 minutes ago
kqgdmededccb        voting_vote.2         dockersamples/examplevotingapp_vote:before     node2  Running        Running 21 minutes ago
t72q3z038jeh        voting_redis.2        redis:alpine                                   node3  Running        Running 21 minutes ago
```

### Format the output (`--format`)

The formatting options (`--format`) pretty-prints tasks output using a Go template.

​	格式化选项 (`--format`) 使用 Go 模板对任务输出进行漂亮地打印。

Valid placeholders for the Go template are listed below:

​	Go 模板的有效占位符如下：

| Placeholder     | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| `.ID`           | Task ID                                                      |
| `.Name`         | Task name                                                    |
| `.Image`        | Task image                                                   |
| `.Node`         | Node ID                                                      |
| `.DesiredState` | 任务的期望状态（`running`、`shutdown` 或 `accepted`） Desired state of the task (`running`, `shutdown`, or `accepted`) |
| `.CurrentState` | Current state of the task                                    |
| `.Error`        | Error                                                        |
| `.Ports`        | 任务已发布端口 Task published ports                          |

When using the `--format` option, the `stack ps` command will either output the data exactly as the template declares or, when using the `table` directive, includes column headers as well.

​	使用 `--format` 选项时，`stack ps` 命令将按模板指定的格式输出数据，或者在使用 `table` 指令时也包括列标题。

The following example uses a template without headers and outputs the `Name` and `Image` entries separated by a colon (`:`) for all tasks:

​	以下示例使用无标题模板，并以冒号 (`:`) 分隔，输出所有任务的 `Name` 和 `Image` 条目：



```console
$ docker stack ps --format "{{.Name}}: {{.Image}}" voting

voting_worker.1: dockersamples/examplevotingapp_worker:latest
voting_result.1: dockersamples/examplevotingapp_result:before
voting_vote.1: dockersamples/examplevotingapp_vote:before
voting_db.1: postgres:9.4
voting_redis.1: redis:alpine
voting_visualizer.1: dockersamples/visualizer:stable
voting_vote.2: dockersamples/examplevotingapp_vote:before
voting_redis.2: redis:alpine
```

To list all tasks in JSON format, use the `json` directive:

​	若要以 JSON 格式列出所有任务，请使用 `json` 指令：



```console
$ docker stack ps --format json myapp
{"CurrentState":"Preparing 23 seconds ago","DesiredState":"Running","Error":"","ID":"2ufjubh79tn0","Image":"localstack/localstack:latest","Name":"myapp_localstack.1","Node":"docker-desktop","Ports":""}
{"CurrentState":"Running 20 seconds ago","DesiredState":"Running","Error":"","ID":"roee387ngf5r","Image":"redis:6.0.9-alpine3.12","Name":"myapp_redis.1","Node":"docker-desktop","Ports":""}
{"CurrentState":"Preparing 13 seconds ago","DesiredState":"Running","Error":"","ID":"yte68ouq7glh","Image":"postgres:13.2-alpine","Name":"myapp_repos-db.1","Node":"docker-desktop","Ports":""}
```

### 不将 ID 映射为名称 (`--no-resolve`) Do not map IDs to Names (`--no-resolve`)

The `--no-resolve` option shows IDs for task name, without mapping IDs to Names.

​	`--no-resolve` 选项显示任务名称的 ID，而不将 ID 映射为名称：



```console
$ docker stack ps --no-resolve voting

ID                  NAME                          IMAGE                                          NODE                        DESIRED STATE  CURRENT STATE            ERROR  PORTS
xim5bcqtgk1b        10z9fjfqzsxnezo4hb81p8mqg.1   dockersamples/examplevotingapp_worker:latest   qaqt4nrzo775jrx6detglho01   Running        Running 30 minutes ago
q7yik0ks1in6        hbxltua1na7mgqjnidldv5m65.1   dockersamples/examplevotingapp_result:before   mxpaef1tlh23s052erw88a4w5   Running        Running 30 minutes ago
rx5yo0866nfx        qyprtqw1g5nrki557i974ou1d.1   dockersamples/examplevotingapp_vote:before     kanqcxfajd1r16wlnqcblobmm   Running        Running 31 minutes ago
tz6j82jnwrx7        122f0xxngg17z52be7xspa72x.1   postgres:9.4                                   mxpaef1tlh23s052erw88a4w5   Running        Running 31 minutes ago
w48spazhbmxc        tg61x8myx563ueo3urmn1ic6m.1   redis:alpine                                   qaqt4nrzo775jrx6detglho01   Running        Running 31 minutes ago
6jj1m02freg1        8cqlyi444kzd3panjb7edh26v.1   dockersamples/visualizer:stable                mxpaef1tlh23s052erw88a4w5   Running        Running 31 minutes ago
kqgdmededccb        qyprtqw1g5nrki557i974ou1d.2   dockersamples/examplevotingapp_vote:before     qaqt4nrzo775jrx6detglho01   Running        Running 31 minutes ago
t72q3z038jeh        tg61x8myx563ueo3urmn1ic6m.2   redis:alpine                                   kanqcxfajd1r16wlnqcblobmm   Running        Running 31 minutes ago
```

### 不截断输出 (`--no-trunc`) Do not truncate output (`--no-trunc`)

When deploying a service, docker resolves the digest for the service's image, and pins the service to that digest. The digest is not shown by default, but is printed if `--no-trunc` is used. The `--no-trunc` option also shows the non-truncated task IDs, and error-messages, as can be seen below:

​	部署服务时，Docker 会解析服务镜像的摘要并将服务固定到该摘要。默认情况下不显示摘要，但如果使用 `--no-trunc` 则会打印出来。`--no-trunc` 选项还会显示非截断的任务 ID 和错误消息：



```console
$ docker stack ps --no-trunc voting

ID                          NAME                  IMAGE                                                                                                                 NODE   DESIRED STATE  CURREN STATE           ERROR  PORTS
xim5bcqtgk1bxqz91jzo4a1s5   voting_worker.1       dockersamples/examplevotingapp_worker:latest@sha256:3e4ddf59c15f432280a2c0679c4fc5a2ee5a797023c8ef0d3baf7b1385e9fed   node2  Running        Runnin 32 minutes ago
q7yik0ks1in6kv32gg6y6yjf7   voting_result.1       dockersamples/examplevotingapp_result:before@sha256:83b56996e930c292a6ae5187fda84dd6568a19d97cdb933720be15c757b7463   node1  Running        Runnin 32 minutes ago
rx5yo0866nfxc58zf4irsss6n   voting_vote.1         dockersamples/examplevotingapp_vote:before@sha256:8e64b182c87de902f2b72321c89b4af4e2b942d76d0b772532ff27ec4c6ebf6     node3  Running        Runnin 32 minutes ago
tz6j82jnwrx7n2offljp3mn03   voting_db.1           postgres:9.4@sha256:6046af499eae34d2074c0b53f9a8b404716d415e4a03e68bc1d2f8064f2b027                                   node1  Running        Runnin 32 minutes ago
w48spazhbmxcmbjfi54gs7x90   voting_redis.1        redis:alpine@sha256:9cd405cd1ec1410eaab064a1383d0d8854d1ef74a54e1e4a92fb4ec7bdc3ee7                                   node2  Running        Runnin 32 minutes ago
6jj1m02freg1n3z9n1evrzsbl   voting_visualizer.1   dockersamples/visualizer:stable@sha256:f924ad66c8e94b10baaf7bdb9cd491ef4e982a1d048a56a17e02bf5945401e5                node1  Running        Runnin 32 minutes ago
kqgdmededccbhz2wuc0e9hx7g   voting_vote.2         dockersamples/examplevotingapp_vote:before@sha256:8e64b182c87de902f2b72321c89b4af4e2b942d76d0b772532ff27ec4c6ebf6     node2  Running        Runnin 32 minutes ago
t72q3z038jehe1wbh9gdum076   voting_redis.2        redis:alpine@sha256:9cd405cd1ec1410eaab064a1383d0d8854d1ef74a54e1e4a92fb4ec7bdc3ee7                                   node3  Running        Runnin 32 minutes ago
```

### 仅显示任务 ID (`-q, --quiet`) Only display task IDs (`-q, --quiet`)

The `-q`or `--quiet` option only shows IDs of the tasks in the stack. This example outputs all task IDs of the `voting` stack:

​	`-q` 或 `--quiet` 选项仅显示栈中任务的 ID。以下示例输出 `voting` 栈的所有任务 ID：



```console
$ docker stack ps -q voting
xim5bcqtgk1b
q7yik0ks1in6
rx5yo0866nfx
tz6j82jnwrx7
w48spazhbmxc
6jj1m02freg1
kqgdmededccb
t72q3z038jeh
```

This option can be used to perform batch operations. For example, you can use the task IDs as input for other commands, such as `docker inspect`. The following example inspects all tasks of the `voting` stac

​	此选项可用于执行批处理操作。例如，可以将任务 ID 用作其他命令的输入，例如 `docker inspect`。以下示例检查 `voting` 栈的所有任务：



```console
$ docker inspect $(docker stack ps -q voting)

[
    {
        "ID": "xim5bcqtgk1b1gk0krq1",
        "Version": {
<...>
```
