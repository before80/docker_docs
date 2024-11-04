+++
title = "docker ps"
date = 2024-10-23T14:54:43+08:00
weight = 230
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/ls/](https://docs.docker.com/reference/cli/docker/container/ls/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container ls

| Description | List containers                                         |
| :---------- | ------------------------------------------------------- |
| Usage       | `docker container ls [OPTIONS]`                         |
| Aliases     | `docker container list``docker container ps``docker ps` |

## Description

List containers

​	列出容器

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-a, --all`](https://docs.docker.com/reference/cli/docker/container/ls/#all) |         | 显示所有容器（默认仅显示运行中的容器） Show all containers (default shows just running) |
| [`-f, --filter`](https://docs.docker.com/reference/cli/docker/container/ls/#filter) |         | 根据提供的条件过滤输出 Filter output based on conditions provided |
| [`--format`](https://docs.docker.com/reference/cli/docker/container/ls/#format) |         | 使用自定义模板格式化输出：'table'：以表格格式打印输出（默认） 'table TEMPLATE'：使用给定的 Go 模板以表格格式打印输出 'json'：以 JSON 格式打印 'TEMPLATE'：使用给定的 Go 模板打印输出。有关使用模板格式化输出的更多信息，请参见 https://docs.docker.com/go/formatting/  Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `-n, --last`                                                 | `-1`    | 显示 n 个最近创建的容器（包括所有状态） Show n last created containers (includes all states) |
| `-l, --latest`                                               |         | 显示最近创建的容器（包括所有状态） Show the latest created container (includes all states) |
| [`--no-trunc`](https://docs.docker.com/reference/cli/docker/container/ls/#no-trunc) |         | 不截断输出 Don't truncate output                             |
| `-q, --quiet`                                                |         | 仅显示容器 ID Only display container IDs                     |
| [`-s, --size`](https://docs.docker.com/reference/cli/docker/container/ls/#size) |         | 显示每个容器的总文件大小 Display total file sizes            |

## Examples

### 不截断输出 (`--no-trunc`) Do not truncate output (`--no-trunc`)

Running `docker ps --no-trunc` showing 2 linked containers.

​	运行 `docker ps --no-trunc` 显示 2 个链接的容器。



```console
$ docker ps --no-trunc

CONTAINER ID                                                     IMAGE                        COMMAND                CREATED              STATUS              PORTS               NAMES
ca5534a51dd04bbcebe9b23ba05f389466cf0c190f1f8f182d7eea92a9671d00 ubuntu:24.04                 bash                   17 seconds ago       Up 16 seconds       3300-3310/tcp       webapp
9ca9747b233100676a48cc7806131586213fa5dab86dd1972d6a8732e3a84a4d crosbymichael/redis:latest   /redis-server --dir    33 minutes ago       Up 33 minutes       6379/tcp            redis,webapp/db
```

### 显示运行和停止的容器 (`-a`, `--all`) Show both running and stopped containers (`-a`, `--all`)

The `docker ps` command only shows running containers by default. To see all containers, use the `--all` (or `-a`) flag:

​	`docker ps` 命令默认只显示运行中的容器。要查看所有容器，请使用 `--all`（或 `-a`）标志：



```console
$ docker ps -a
```

`docker ps` groups exposed ports into a single range if possible. E.g., a container that exposes TCP ports `100, 101, 102` displays `100-102/tcp` in the `PORTS` column.

​	`docker ps` 将暴露的端口组合成一个范围（如果可能）。例如，暴露 TCP 端口 `100, 101, 102` 的容器在 `PORTS` 列中显示为 `100-102/tcp`。

​	

### 显示容器的磁盘使用情况 (`--size`) Show disk usage by container (`--size`)

The `docker ps --size` (or `-s`) command displays two different on-disk-sizes for each container:

​	`docker ps --size`（或 `-s`）命令显示每个容器的两种不同的磁盘大小：



```console
$ docker ps --size

CONTAINER ID   IMAGE          COMMAND                  CREATED        STATUS       PORTS   NAMES        SIZE
e90b8831a4b8   nginx          "/bin/bash -c 'mkdir "   11 weeks ago   Up 4 hours           my_nginx     35.58 kB (virtual 109.2 MB)
00c6131c5e30   telegraf:1.5   "/entrypoint.sh"         11 weeks ago   Up 11 weeks          my_telegraf  0 B (virtual 209.5 MB)
```

- The "size" information shows the amount of data (on disk) that is used for the *writable* layer of each container
  - “大小”信息显示每个容器的可写层在磁盘上使用的数据量。

- The "virtual size" is the total amount of disk-space used for the read-only *image* data used by the container and the writable layer.
  - “虚拟大小”是容器使用的只读镜像数据和可写层所占用的总磁盘空间。


For more information, refer to the [container size on disk](https://docs.docker.com/engine/storage/drivers/#container-size-on-disk) section.

​	有关更多信息，请参阅 [容器在磁盘上的大小](https://docs.docker.com/engine/storage/drivers/#container-size-on-disk) 部分。

### Filtering (`--filter`)

The `--filter` (or `-f`) flag format is a `key=value` pair. If there is more than one filter, then pass multiple flags (e.g. `--filter "foo=bar" --filter "bif=baz"`).

​	`--filter`（或 `-f`）标志的格式为 `key=value` 对。如果有多个过滤条件，请传递多个标志（例如 `--filter "foo=bar" --filter "bif=baz"`）。

The currently supported filters are:

​	当前支持的过滤条件有：

| Filter                | Description                                                  |
| :-------------------- | :----------------------------------------------------------- |
| `id`                  | 容器的 ID - Container's ID                                   |
| `name`                | 容器的名称 Container's name                                  |
| `label`               | 一个任意字符串，表示键或键值对。表示为 `<key>` 或 `<key>=<value>` - An arbitrary string representing either a key or a key-value pair. Expressed as `<key>` or `<key>=<value>` |
| `exited`              | 一个整数，表示容器的退出代码。仅在 `--all` 时有用。 An integer representing the container's exit code. Only useful with `--all`. |
| `status`              | One of `created`, `restarting`, `running`, `removing`, `paused`, `exited`, or `dead` |
| `ancestor`            | 过滤共享给定镜像作为祖先的容器。表示为 `<image-name>[:<tag>]`、`<image id>` 或 `<image@digest>` - Filters containers which share a given image as an ancestor. Expressed as `<image-name>[:<tag>]`, `<image id>`, or `<image@digest>` |
| `before` or `since`   | 过滤在给定容器 ID 或名称之前或之后创建的容器 Filters containers created before or after a given container ID or name |
| `volume`              | 过滤挂载了给定卷或绑定挂载的运行容器。 Filters running containers which have mounted a given volume or bind mount. |
| `network`             | 过滤连接到给定网络的运行容器。 Filters running containers connected to a given network. |
| `publish` or `expose` | 过滤发布或暴露给定端口的容器。表示为 `<port>[/<proto>]` 或 `<startport-endport>/[<proto>]` - Filters containers which publish or expose a given port. Expressed as `<port>[/<proto>]` or `<startport-endport>/[<proto>]` |
| `health`              | 根据健康检查状态过滤容器。之一 `starting`、`healthy`、`unhealthy` 或 `none`。 - Filters containers based on their healthcheck status. One of `starting`, `healthy`, `unhealthy` or `none`. |
| `isolation`           | Windows 守护进程专用。之一 `default`、`process` 或 `hyperv`。 Windows daemon only. One of `default`, `process`, or `hyperv`. |
| `is-task`             | 过滤作为服务的“任务”的容器。布尔选项（`true` 或 `false`）  Filters containers that are a "task" for a service. Boolean option (`true` or `false`) |

#### label

The `label` filter matches containers based on the presence of a `label` alone or a `label` and a value.

​	`label` 过滤器根据标签的存在与否，或者标签和值的组合来匹配容器。

The following filter matches containers with the `color` label regardless of its value.

​	以下过滤器匹配所有具有 `color` 标签的容器，无论其值如何。



```console
$ docker ps --filter "label=color"

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
673394ef1d4c        busybox             "top"               47 seconds ago      Up 45 seconds                           nostalgic_shockley
d85756f57265        busybox             "top"               52 seconds ago      Up 51 seconds                           high_albattani
```

The following filter matches containers with the `color` label with the `blue` value.

​	以下过滤器匹配具有值为 `blue` 的 `color` 标签的容器。

```console
$ docker ps --filter "label=color=blue"

CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
d85756f57265        busybox             "top"               About a minute ago   Up About a minute                       high_albattani
```

#### name

The `name` filter matches on all or part of a container's name.

​	`name` 过滤器根据容器名称的全部或部分匹配。

The following filter matches all containers with a name containing the `nostalgic_stallman` string.

​	以下过滤器匹配所有名称包含 `nostalgic_stallman` 字符串的容器。



```console
$ docker ps --filter "name=nostalgic_stallman"

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
9b6247364a03        busybox             "top"               2 minutes ago       Up 2 minutes                            nostalgic_stallman
```

You can also filter for a substring in a name as this shows:

​	您还可以按名称的子字符串进行过滤，如下所示：



```console
$ docker ps --filter "name=nostalgic"

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
715ebfcee040        busybox             "top"               3 seconds ago       Up 1 second                             i_am_nostalgic
9b6247364a03        busybox             "top"               7 minutes ago       Up 7 minutes                            nostalgic_stallman
673394ef1d4c        busybox             "top"               38 minutes ago      Up 38 minutes                           nostalgic_shockley
```

#### exited

The `exited` filter matches containers by exist status code. For example, to filter for containers that have exited successfully:

​	`exited` 过滤器根据退出状态码匹配容器。例如，过滤已成功退出的容器：

```console
$ docker ps -a --filter 'exited=0'

CONTAINER ID        IMAGE             COMMAND                CREATED             STATUS                   PORTS                      NAMES
ea09c3c82f6e        registry:latest   /srv/run.sh            2 weeks ago         Exited (0) 2 weeks ago   127.0.0.1:5000->5000/tcp   desperate_leakey
106ea823fe4e        fedora:latest     /bin/sh -c 'bash -l'   2 weeks ago         Exited (0) 2 weeks ago                              determined_albattani
48ee228c9464        fedora:20         bash                   2 weeks ago         Exited (0) 2 weeks ago                              tender_torvalds
```

#### 按退出信号过滤 Filter by exit signal

You can use a filter to locate containers that exited with status of `137` meaning a `SIGKILL(9)` killed them.

​	您可以使用过滤器来定位以状态 `137` 退出的容器，这意味着它们被 `SIGKILL(9)` 杀死。



```console
$ docker ps -a --filter 'exited=137'

CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                       PORTS               NAMES
b3e1c0ed5bfe        ubuntu:latest       "sleep 1000"           12 seconds ago      Exited (137) 5 seconds ago                       grave_kowalevski
a2eb5558d669        redis:latest        "/entrypoint.sh redi   2 hours ago         Exited (137) 2 hours ago                         sharp_lalande
```

Any of these events result in a `137` status:

​	导致 `137` 状态的任何事件包括：

- the `init` process of the container is killed manually

  - 容器的 `init` 进程被手动杀死

- `docker kill` kills the container

  - `docker kill` 杀死容器

- Docker daemon restarts which kills all running containers

  - Docker 守护进程重启，杀死所有正在运行的容器

  

#### status

The `status` filter matches containers by status. The possible values for the container status are:

​	`status` 过滤器根据状态匹配容器。容器状态可能的值包括：

| Status       | Description                                                  |
| :----------- | :----------------------------------------------------------- |
| `created`    | 从未启动的容器。 A container that has never been started.    |
| `running`    | 运行中的容器，由 `docker start` 或 `docker run` 启动。 A running container, started by either `docker start` or `docker run`. |
| `paused`     | 暂停的容器。请参见 `docker pause`。 A paused container. See `docker pause`. |
| `restarting` | 根据指定的重启策略正在启动的容器。 A container which is starting due to the designated restart policy for that container. |
| `exited`     | 不再运行的容器。例如，容器内部的进程完成或通过 `docker stop` 命令停止。 A container which is no longer running. For example, the process inside the container completed or the container was stopped using the `docker stop` command. |
| `removing`   | 正在被删除的容器。请参见 `docker rm`。 A container which is in the process of being removed. See `docker rm`. |
| `dead`       | “僵尸”容器；例如，部分删除的容器，因为外部进程占用了资源。`dead` 容器不能被（重新）启动，只能删除。 A "defunct" container; for example, a container that was only partially removed because resources were kept busy by an external process. `dead` containers cannot be (re)started, only removed. |

For example, to filter for `running` containers:

​	例如，要过滤 `running` 状态的容器：

```console
$ docker ps --filter status=running

CONTAINER ID        IMAGE                  COMMAND             CREATED             STATUS              PORTS               NAMES
715ebfcee040        busybox                "top"               16 minutes ago      Up 16 minutes                           i_am_nostalgic
d5c976d3c462        busybox                "top"               23 minutes ago      Up 23 minutes                           top
9b6247364a03        busybox                "top"               24 minutes ago      Up 24 minutes                           nostalgic_stallman
```

To filter for `paused` containers:

​	要过滤 `paused` 状态的容器：

```console
$ docker ps --filter status=paused

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
673394ef1d4c        busybox             "top"               About an hour ago   Up About an hour (Paused)                       nostalgic_shockley
```

#### ancestor

The `ancestor` filter matches containers based on its image or a descendant of it. The filter supports the following image representation:

​	`ancestor` 过滤器过滤运行中或已停止的容器，根据镜像 ID 或名称过滤容器。例如：

- `image`
- `image:tag`
- `image:tag@digest`
- `short-id`
- `full-id`

If you don't specify a `tag`, the `latest` tag is used. For example, to filter for containers that use the latest `ubuntu` image:

​	如果您没有指定 `tag`，则会使用 `latest` 标签。例如，要过滤使用最新 `ubuntu` 镜像的容器：



```console
$ docker ps --filter ancestor=ubuntu

CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
919e1179bdb8        ubuntu-c1           "top"               About a minute ago   Up About a minute                       admiring_lovelace
5d1e4a540723        ubuntu-c2           "top"               About a minute ago   Up About a minute                       admiring_sammet
82a598284012        ubuntu              "top"               3 minutes ago        Up 3 minutes                            sleepy_bose
bab2a34ba363        ubuntu              "top"               3 minutes ago        Up 3 minutes                            focused_yonath
```

Match containers based on the `ubuntu-c1` image which, in this case, is a child of `ubuntu`:

​	基于 `ubuntu-c1` 镜像匹配容器，在这种情况下，它是 `ubuntu` 的子镜像：



```console
$ docker ps --filter ancestor=ubuntu-c1

CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
919e1179bdb8        ubuntu-c1           "top"               About a minute ago   Up About a minute                       admiring_lovelace
```

Match containers based on the `ubuntu` version `24.04` image:

​	基于 `ubuntu` 版本 `24.04` 镜像匹配容器：



```console
$ docker ps --filter ancestor=ubuntu:24.04

CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
82a598284012        ubuntu:24.04        "top"               3 minutes ago        Up 3 minutes                            sleepy_bose
```

The following matches containers based on the layer `d0e008c6cf02` or an image that have this layer in its layer stack.

​	以下匹配基于层 `d0e008c6cf02` 或具有该层的镜像：



```console
$ docker ps --filter ancestor=d0e008c6cf02

CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
82a598284012        ubuntu:24.04        "top"               3 minutes ago        Up 3 minutes                            sleepy_bose
```

#### Create time

##### before

The `before` filter shows only containers created before the container with a given ID or name. For example, having these containers created:

​	`before` 过滤器仅显示在具有给定 ID 或名称的容器之前创建的容器。例如，创建了这些容器：



```console
$ docker ps

CONTAINER ID        IMAGE       COMMAND       CREATED              STATUS              PORTS              NAMES
9c3527ed70ce        busybox     "top"         14 seconds ago       Up 15 seconds                          desperate_dubinsky
4aace5031105        busybox     "top"         48 seconds ago       Up 49 seconds                          focused_hamilton
6e63f6ff38b0        busybox     "top"         About a minute ago   Up About a minute                      distracted_fermat
```

Filtering with `before` would give:

​	使用 `before` 过滤的结果：



```console
$ docker ps -f before=9c3527ed70ce

CONTAINER ID        IMAGE       COMMAND       CREATED              STATUS              PORTS              NAMES
4aace5031105        busybox     "top"         About a minute ago   Up About a minute                      focused_hamilton
6e63f6ff38b0        busybox     "top"         About a minute ago   Up About a minute                      distracted_fermat
```

##### since

The `since` filter shows only containers created since the container with a given ID or name. For example, with the same containers as in `before` filter:

​	`since` 过滤器仅显示在具有给定 ID 或名称的容器之后创建的容器。例如，使用与 `before` 过滤器相同的容器：



```console
$ docker ps -f since=6e63f6ff38b0

CONTAINER ID        IMAGE       COMMAND       CREATED             STATUS              PORTS               NAMES
9c3527ed70ce        busybox     "top"         10 minutes ago      Up 10 minutes                           desperate_dubinsky
4aace5031105        busybox     "top"         10 minutes ago      Up 10 minutes                           focused_hamilton
```

#### volume

The `volume` filter shows only containers that mount a specific volume or have a volume mounted in a specific path:

​	`volume` 过滤器仅显示挂载特定卷或在特定路径下挂载卷的容器：



```console
$ docker ps --filter volume=remote-volume --format "table {{.ID}}\t{{.Mounts}}"

CONTAINER ID        MOUNTS
9c3527ed70ce        remote-volume

$ docker ps --filter volume=/data --format "table {{.ID}}\t{{.Mounts}}"

CONTAINER ID        MOUNTS
9c3527ed70ce        remote-volume
```

#### network

The `network` filter shows only containers that are connected to a network with a given name or ID.

​	`network` 过滤器仅显示连接到具有给定名称或 ID 的网络的容器。

The following filter matches all containers that are connected to a network with a name containing `net1`.

​	以下过滤器匹配所有连接到名称包含 `net1` 的网络的容器：



```console
$ docker run -d --net=net1 --name=test1 ubuntu top
$ docker run -d --net=net2 --name=test2 ubuntu top

$ docker ps --filter network=net1

CONTAINER ID        IMAGE       COMMAND       CREATED             STATUS              PORTS               NAMES
9d4893ed80fe        ubuntu      "top"         10 minutes ago      Up 10 minutes                           test1
```

The network filter matches on both the network's name and ID. The following example shows all containers that are attached to the `net1` network, using the network ID as a filter:

​	网络过滤器匹配网络的名称和 ID。以下示例显示所有附加到 `net1` 网络的容器，使用网络 ID 作为过滤器：



```console
$ docker network inspect --format "{{.ID}}" net1

8c0b4110ae930dbe26b258de9bc34a03f98056ed6f27f991d32919bfe401d7c5

$ docker ps --filter network=8c0b4110ae930dbe26b258de9bc34a03f98056ed6f27f991d32919bfe401d7c5

CONTAINER ID        IMAGE       COMMAND       CREATED             STATUS              PORTS               NAMES
9d4893ed80fe        ubuntu      "top"         10 minutes ago      Up 10 minutes                           test1
```

#### publish and expose

The `publish` and `expose` filters show only containers that have published or exposed port with a given port number, port range, and/or protocol. The default protocol is `tcp` when not specified.

​	`publish` 和 `expose` 过滤器仅显示发布或暴露给定端口号、端口范围和/或协议的容器。默认协议为 `tcp`，未指定时。

The following filter matches all containers that have published port of 80:

​	以下过滤器匹配所有发布了端口 80 的容器：



```console
$ docker run -d --publish=80 busybox top
$ docker run -d --expose=8080 busybox top

$ docker ps -a

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                   NAMES
9833437217a5        busybox             "top"               5 seconds ago       Up 4 seconds        8080/tcp                dreamy_mccarthy
fc7e477723b7        busybox             "top"               50 seconds ago      Up 50 seconds       0.0.0.0:32768->80/tcp   admiring_roentgen

$ docker ps --filter publish=80

CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS                   NAMES
fc7e477723b7        busybox             "top"               About a minute ago   Up About a minute   0.0.0.0:32768->80/tcp   admiring_roentgen
```

The following filter matches all containers that have exposed TCP port in the range of `8000-8080`:

​	以下过滤器匹配所有暴露了 TCP 端口范围为 `8000-8080` 的容器：



```console
$ docker ps --filter expose=8000-8080/tcp

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
9833437217a5        busybox             "top"               21 seconds ago      Up 19 seconds       8080/tcp            dreamy_mccarthy
```

The following filter matches all containers that have exposed UDP port `80`:

​	以下过滤器匹配所有暴露了 UDP 端口 `80` 的容器：



```console
$ docker ps --filter publish=80/udp

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

### 格式化输出 (`--format`) Format the output (`--format`)

The formatting option (`--format`) pretty-prints container output using a Go template.

​	格式化选项（`--format`）使用 Go 模板对容器输出进行美化。

Valid placeholders for the Go template are listed below:

​	有效的 Go 模板占位符如下：

| Placeholder   | Description                                                  |
| :------------ | :----------------------------------------------------------- |
| `.ID`         | 容器 ID - Container ID                                       |
| `.Image`      | 镜像 ID - Image ID                                           |
| `.Command`    | 引号命令 Quoted command                                      |
| `.CreatedAt`  | 容器创建时的时间 Time when the container was created.        |
| `.RunningFor` | 容器启动以来的时间 Elapsed time since the container was started. |
| `.Ports`      | 暴露的端口。 Exposed ports.                                  |
| `.State`      | 容器状态 Container status (for example; "created", "running", "exited"). |
| `.Status`     | 容器状态，包含持续时间和健康状态的详细信息。 Container status with details about duration and health-status. |
| `.Size`       | 容器磁盘大小。 Container disk size.                          |
| `.Names`      | 容器名称。 Container names.                                  |
| `.Labels`     | 分配给容器的所有标签。 All labels assigned to the container. |
| `.Label`      | 该容器特定标签的值。例如 Value of a specific label for this container. For example `'{{.Label "com.docker.swarm.cpu"}}'` |
| `.Mounts`     | 此容器挂载的卷的名称。 Names of the volumes mounted in this container. |
| `.Networks`   | 附加到此容器的网络名称。 Names of the networks attached to this container. |

When using the `--format` option, the `ps` command will either output the data exactly as the template declares or, when using the `table` directive, includes column headers as well.

​	使用 `--format` 选项时，`ps` 命令将根据模板声明精确输出数据，或者在使用 `table` 指令时，包括列标题。

The following example uses a template without headers and outputs the `ID` and `Command` entries separated by a colon (`:`) for all running containers:

​	以下示例使用没有标题的模板，并输出 `ID` 和 `Command` 条目，以冒号（`:`）分隔，适用于所有运行中的容器：



```console
$ docker ps --format "{{.ID}}: {{.Command}}"

a87ecb4f327c: /bin/sh -c #(nop) MA
01946d9d34d8: /bin/sh -c #(nop) MA
c1d3b0166030: /bin/sh -c yum -y up
41d50ecd2f57: /bin/sh -c #(nop) MA
```

To list all running containers with their labels in a table format you can use:

​	要以表格格式列出所有运行中的容器及其标签，您可以使用：



```console
$ docker ps --format "table {{.ID}}\t{{.Labels}}"

CONTAINER ID        LABELS
a87ecb4f327c        com.docker.swarm.node=ubuntu,com.docker.swarm.storage=ssd
01946d9d34d8
c1d3b0166030        com.docker.swarm.node=debian,com.docker.swarm.cpu=6
41d50ecd2f57        com.docker.swarm.node=fedora,com.docker.swarm.cpu=3,com.docker.swarm.storage=ssd
```

To list all running containers in JSON format, use the `json` directive:

​	要以 JSON 格式列出所有运行中的容器，可以使用 `json` 指令：



```console
$ docker ps --format json
{"Command":"\"/docker-entrypoint.…\"","CreatedAt":"2021-03-10 00:15:05 +0100 CET","ID":"a762a2b37a1d","Image":"nginx","Labels":"maintainer=NGINX Docker Maintainers \u003cdocker-maint@nginx.com\u003e","LocalVolumes":"0","Mounts":"","Names":"boring_keldysh","Networks":"bridge","Ports":"80/tcp","RunningFor":"4 seconds ago","Size":"0B","State":"running","Status":"Up 3 seconds"}
```

