+++
title = "docker container stats"
date = 2024-10-23T14:54:43+08:00
weight = 160
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/stats/](https://docs.docker.com/reference/cli/docker/container/stats/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container stats

| Description | Display a live stream of container(s) resource usage statistics |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker container stats [OPTIONS] [CONTAINER...]`            |
| Aliases     | `docker stats`                                               |

## Description

The `docker stats` command returns a live data stream for running containers. To limit data to one or more specific containers, specify a list of container names or ids separated by a space. You can specify a stopped container but stopped containers do not return any data.

​	`docker stats` 命令返回运行中容器的实时数据流。要将数据限制为一个或多个特定容器，请指定以空格分隔的容器名称或 ID 列表。您可以指定一个已停止的容器，但已停止的容器不会返回任何数据。

If you need more detailed information about a container's resource usage, use the `/containers/(id)/stats` API endpoint.

​	如果您需要有关容器资源使用的更详细信息，请使用 `/containers/(id)/stats` API 端点。

> **Note**
>
> On Linux, the Docker CLI reports memory usage by subtracting cache usage from the total memory usage. The API does not perform such a calculation but rather provides the total memory usage and the amount from the cache so that clients can use the data as needed. The cache usage is defined as the value of `total_inactive_file` field in the `memory.stat` file on cgroup v1 hosts.
>
> ​	在 Linux 上，Docker CLI 通过从总内存使用中减去缓存使用来报告内存使用情况。API 不执行此类计算，而是提供总内存使用量和来自缓存的数量，以便客户端可以根据需要使用这些数据。缓存使用量在 cgroup v1 主机的 `memory.stat` 文件中定义为 `total_inactive_file` 字段的值。
>
> On Docker 19.03 and older, the cache usage was defined as the value of `cache` field. On cgroup v2 hosts, the cache usage is defined as the value of `inactive_file` field.
>
> ​	在 Docker 19.03 及更早版本中，缓存使用量定义为 `cache` 字段的值。在 cgroup v2 主机上，缓存使用量定义为 `inactive_file` 字段的值。

> **Note**
>
> The `PIDS` column contains the number of processes and kernel threads created by that container. Threads is the term used by Linux kernel. Other equivalent terms are "lightweight process" or "kernel task", etc. A large number in the `PIDS` column combined with a small number of processes (as reported by `ps` or `top`) may indicate that something in the container is creating many threads.
>
> ​	`PIDS` 列包含由该容器创建的进程和内核线程的数量。线程是 Linux 内核使用的术语。其他等效术语包括“轻量级进程”或“内核任务”等。在 `PIDS` 列中出现的较大数字与通过 `ps` 或 `top` 报告的较少进程数相结合，可能表明容器中的某些内容正在创建许多线程。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| `-a, --all`                                                  |         | 显示所有容器（默认仅显示正在运行的容器） Show all containers (default shows just running) |
| [`--format`](https://docs.docker.com/reference/cli/docker/container/stats/#format) |         | 使用自定义模板格式化输出：'table'：以表格格式打印输出，带有列标题（默认） 'table TEMPLATE'：使用给定的 Go 模板以表格格式打印输出 'json'：以 JSON 格式打印 'TEMPLATE'：使用给定的 Go 模板打印输出。有关使用模板格式化输出的更多信息，请参阅 https://docs.docker.com/go/formatting/   Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `--no-stream`                                                |         | 禁用流式统计，仅拉取第一次结果 Disable streaming stats and only pull the first result |
| `--no-trunc`                                                 |         | 不截断输出  Do not truncate output                           |

## Examples

Running `docker stats` on all running containers against a Linux daemo

​	在 Linux 守护进程上运行 `docker stats` 命令，监控所有正在运行的容器。



```console
$ docker stats

CONTAINER ID        NAME                                    CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O           PIDS
b95a83497c91        awesome_brattain                        0.28%               5.629MiB / 1.952GiB   0.28%               916B / 0B           147kB / 0B          9
67b2525d8ad1        foobar                                  0.00%               1.727MiB / 1.952GiB   0.09%               2.48kB / 0B         4.11MB / 0B         2
e5c383697914        test-1951.1.kay7x1lh1twk9c0oig50sd5tr   0.00%               196KiB / 1.952GiB     0.01%               71.2kB / 0B         770kB / 0B          1
4bda148efbc0        random.1.vnc8on831idyr42slu578u3cr      0.00%               1.672MiB / 1.952GiB   0.08%               110kB / 0B          578kB / 0B          2
```

If you don't [specify a format string using `--format`](https://docs.docker.com/reference/cli/docker/container/stats/#format), the following columns are shown.

​	如果您没有 [使用 `--format` 指定格式字符串](https://docs.docker.com/reference/cli/docker/container/stats/#format)，则将显示以下列。

| Column name               | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| `CONTAINER ID` and `Name` | 容器的 ID 和名称 the ID and name of the container            |
| `CPU %` and `MEM %`       | 容器使用的主机 CPU 和内存的百分比 the percentage of the host's CPU and memory the container is using |
| `MEM USAGE / LIMIT`       | 容器使用的总内存和它被允许使用的总内存 the total memory the container is using, and the total amount of memory it is allowed to use |
| `NET I/O`                 | 容器通过其网络接口接收和发送的数据量 The amount of data the container has received and sent over its network interface |
| `BLOCK I/O`               | 容器写入和读取主机上块设备的数据量 The amount of data the container has written to and read from block devices on the host |
| `PIDs`                    | 容器创建的进程或线程的数量 the number of processes or threads the container has created |

Running `docker stats` on multiple containers by name and id against a Linux daemon.

​	在 Linux 守护进程上按名称和 ID 运行 `docker stats` 命令，监控多个容器。



```console
$ docker stats awesome_brattain 67b2525d8ad1

CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O           PIDS
b95a83497c91        awesome_brattain    0.28%               5.629MiB / 1.952GiB   0.28%               916B / 0B           147kB / 0B          9
67b2525d8ad1        foobar              0.00%               1.727MiB / 1.952GiB   0.09%               2.48kB / 0B         4.11MB / 0B         2
```

Running `docker stats` on container with name `nginx` and getting output in `json` format.

​	在容器名称为 `nginx` 的容器上运行 `docker stats` 并以 `json` 格式获取输出。



```console
$ docker stats nginx --no-stream --format "{{ json . }}"
{"BlockIO":"0B / 13.3kB","CPUPerc":"0.03%","Container":"nginx","ID":"ed37317fbf42","MemPerc":"0.24%","MemUsage":"2.352MiB / 982.5MiB","Name":"nginx","NetIO":"539kB / 606kB","PIDs":"2"}
```

Running `docker stats` with customized format on all (running and stopped) containers.

​	在所有（正在运行和已停止）容器上运行 `docker stats` 并自定义格式。



```console
$ docker stats --all --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}" fervent_panini 5acfcb1b4fd1 humble_visvesvaraya big_heisenberg

CONTAINER                CPU %               MEM USAGE / LIMIT
fervent_panini           0.00%               56KiB / 15.57GiB
5acfcb1b4fd1             0.07%               32.86MiB / 15.57GiB
humble_visvesvaraya      0.00%               0B / 0B
big_heisenberg           0.00%               0B / 0B
```

`humble_visvesvaraya` and `big_heisenberg` are stopped containers in the above example.

​	在上述示例中，`humble_visvesvaraya` 和 `big_heisenberg` 是已停止的容器。

Running `docker stats` on all running containers against a Windows daemon.

​	在 Windows 守护进程上运行 `docker stats` 命令，监控所有正在运行的容器。



```powershell
PS E:\> docker stats
CONTAINER ID        CPU %               PRIV WORKING SET    NET I/O             BLOCK I/O
09d3bb5b1604        6.61%               38.21 MiB           17.1 kB / 7.73 kB   10.7 MB / 3.57 MB
9db7aa4d986d        9.19%               38.26 MiB           15.2 kB / 7.65 kB   10.6 MB / 3.3 MB
3f214c61ad1d        0.00%               28.64 MiB           64 kB / 6.84 kB     4.42 MB / 6.93 MB
```

Running `docker stats` on multiple containers by name and id against a Windows daemon.

​	在 Windows 守护进程上按名称和 ID 运行 `docker stats` 命令，监控多个容器。



```powershell
PS E:\> docker ps -a
CONTAINER ID        NAME                IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
3f214c61ad1d        awesome_brattain    nanoserver          "cmd"               2 minutes ago       Up 2 minutes                            big_minsky
9db7aa4d986d        mad_wilson          windowsservercore   "cmd"               2 minutes ago       Up 2 minutes                            mad_wilson
09d3bb5b1604        fervent_panini      windowsservercore   "cmd"               2 minutes ago       Up 2 minutes                            affectionate_easley

PS E:\> docker stats 3f214c61ad1d mad_wilson
CONTAINER ID        NAME                CPU %               PRIV WORKING SET    NET I/O             BLOCK I/O
3f214c61ad1d        awesome_brattain    0.00%               46.25 MiB           76.3 kB / 7.92 kB   10.3 MB / 14.7 MB
9db7aa4d986d        mad_wilson          9.59%               40.09 MiB           27.6 kB / 8.81 kB   17 MB / 20.1 MB
```

### 格式化输出 (`--format`) Format the output (`--format`)

The formatting option (`--format`) pretty prints container output using a Go template.

​	格式化选项（`--format`）使用 Go 模板美化容器输出。

Valid placeholders for the Go template are listed below:

​	有效的 Go 模板占位符如下：

| Placeholder  | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| `.Container` | 容器名称或 ID（用户输入） Container name or ID (user input)  |
| `.Name`      | 容器名称 Container name                                      |
| `.ID`        | 容器 ID - Container ID                                       |
| `.CPUPerc`   | CPU 百分比 CPU percentage                                    |
| `.MemUsage`  | 内存使用 Memory usage                                        |
| `.NetIO`     | 网络 I/O - Network IO                                        |
| `.BlockIO`   | Block IO                                                     |
| `.MemPerc`   | 内存百分比（在 Windows 上不可用） Memory percentage (Not available on Windows) |
| `.PIDs`      | PIDs 数量（在 Windows 上不可用） Number of PIDs (Not available on Windows) |

When using the `--format` option, the `stats` command either outputs the data exactly as the template declares or, when using the `table` directive, includes column headers as well.

​	使用 `--format` 选项时，`stats` 命令要么严格按照模板声明输出数据，要么在使用 `table` 指令时，包括列标题。

The following example uses a template without headers and outputs the `Container` and `CPUPerc` entries separated by a colon (`:`) for all images:

​	以下示例使用没有标题的模板，输出所有镜像的 `Container` 和 `CPUPerc` 条目，使用冒号（`:`）分隔：



```console
$ docker stats --format "{{.Container}}: {{.CPUPerc}}"

09d3bb5b1604: 6.61%
9db7aa4d986d: 9.19%
3f214c61ad1d: 0.00%
```

To list all containers statistics with their name, CPU percentage and memory usage in a table format you can use:

​	要以表格格式列出所有容器的统计信息，包括它们的名称、CPU 百分比和内存使用，可以使用：



```console
$ docker stats --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}"

CONTAINER           CPU %               PRIV WORKING SET
1285939c1fd3        0.07%               796 KiB / 64 MiB
9c76f7834ae2        0.07%               2.746 MiB / 64 MiB
d1ea048f04e4        0.03%               4.583 MiB / 64 MiB
```

The default formats as follows:

​	默认格式如下：

On Linux:

​	在 Linux 上：

```
"table {{.ID}}\t{{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}\t{{.MemPerc}}\t{{.NetIO}}\t{{.BlockIO}}\t{{.PIDs}}"
```

On Windows:

​	在 Windows 上：

```
"table {{.ID}}\t{{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}\t{{.NetIO}}\t{{.BlockIO}}"
```

