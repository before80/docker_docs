+++
title = "docker volume ls"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/volume/ls/](https://docs.docker.com/reference/cli/docker/volume/ls/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker volume ls

| Description | List volumes                 |
| :---------- | ---------------------------- |
| Usage       | `docker volume ls [OPTIONS]` |
| Aliases     | `docker volume list`         |

## Description

List all the volumes known to Docker. You can filter using the `-f` or `--filter` flag. Refer to the [filtering](https://docs.docker.com/reference/cli/docker/volume/ls/#filter) section for more information about available filter options.

​	列出 Docker 所识别的所有卷。您可以使用 `-f` 或 `--filter` 标志进行过滤。有关可用过滤选项的更多信息，请参阅 [过滤](https://docs.docker.com/reference/cli/docker/volume/ls/#filter) 部分。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| `--cluster`                                                  |         | API 1.42+ Swarm 显示仅集群卷，并使用集群卷列表格式 API 1.42+ Swarm Display only cluster volumes, and use cluster volume list formatting |
| [`-f, --filter`](https://docs.docker.com/reference/cli/docker/volume/ls/#filter) |         | Provide filter values (e.g. `dangling=true`)                 |
| [`--format`](https://docs.docker.com/reference/cli/docker/volume/ls/#format) |         | 使用自定义模板格式化输出：'table'：以表格格式输出，包含列标题（默认）'table TEMPLATE'：使用指定的 Go 模板以表格格式输出 'json'：以 JSON 格式输出 'TEMPLATE'：使用给定的 Go 模板格式输出。请参阅 https://docs.docker.com/go/formatting/ 了解更多模板格式信息 Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `-q, --quiet`                                                |         | 仅显示卷名称 Only display volume names                       |

## Examples

### 创建一个卷 Create a volume



```console
$ docker volume create rosemary

rosemary

$ docker volume create tyler

tyler

$ docker volume ls

DRIVER              VOLUME NAME
local               rosemary
local               tyler
```

### Filtering (--filter)

The filtering flag (`-f` or `--filter`) format is of "key=value". If there is more than one filter, then pass multiple flags (e.g., `--filter "foo=bar" --filter "bif=baz"`)

​	过滤标志 (`-f` 或 `--filter`) 的格式为 "key=value"。如果有多个过滤器，请传递多个标志（例如 `--filter "foo=bar" --filter "bif=baz"`）

The currently supported filters are:

​	当前支持的过滤器包括：

- dangling (Boolean - true or false, 0 or 1)

  - dangling（布尔值 - true 或 false，0 或 1）

- driver (a volume driver's name)

  - driver（卷驱动程序的名称）

- label (`label=<key>` or `label=<key>=<value>`)

  - label (`label=<key>` 或 `label=<key>=<value>`)

- name (a volume's name)

  - name（卷的名称）

  

#### dangling

The `dangling` filter matches on all volumes not referenced by any containers

​	`dangling` 过滤器匹配所有未被任何容器引用的卷。



```console
$ docker run -d  -v tyler:/tmpwork  busybox

f86a7dd02898067079c99ceacd810149060a70528eff3754d0b0f1a93bd0af18
$ docker volume ls -f dangling=true
DRIVER              VOLUME NAME
local               rosemary
```

#### driver

The `driver` filter matches volumes based on their driver.

​	`driver` 过滤器根据驱动程序匹配卷。

The following example matches volumes that are created with the `local` driver:

​	以下示例匹配使用 `local` 驱动程序创建的卷：



```console
$ docker volume ls -f driver=local

DRIVER              VOLUME NAME
local               rosemary
local               tyler
```

#### label

The `label` filter matches volumes based on the presence of a `label` alone or a `label` and a value.

​	`label` 过滤器基于标签的存在或标签和值匹配卷。

First, create some volumes to illustrate this;

​	首先，创建一些卷以进行演示：



```console
$ docker volume create the-doctor --label is-timelord=yes

the-doctor
$ docker volume create daleks --label is-timelord=no

daleks
```

The following example filter matches volumes with the `is-timelord` label regardless of its value.

​	以下示例过滤器匹配带有 `is-timelord` 标签的卷，无论其值如何。



```console
$ docker volume ls --filter label=is-timelord

DRIVER              VOLUME NAME
local               daleks
local               the-doctor
```

As the above example demonstrates, both volumes with `is-timelord=yes`, and `is-melord=no` are returned.

​	如上例所示，标签值为 `is-timelord=yes` 和 `is-timelord=no` 的卷均被返回。

Filtering on both `key` *and* `value` of the label, produces the expected result:

​	基于标签的 `key` *和* `value` 过滤可获得预期结果：



```console
$ docker volume ls --filter label=is-timelord=yes

DRIVER              VOLUME NAME
local               the-doctor
```

Specifying multiple label filter produces an "and" search; all conditions should be met;

​	指定多个标签过滤器会产生“与”搜索；所有条件必须匹配：



```console
$ docker volume ls --filter label=is-timelord=yes --filter label=is-timelord=no

DRIVER              VOLUME NAME
```

#### name

The `name` filter matches on all or part of a volume's name.

​	`name` 过滤器匹配所有或部分卷名称。

The following filter matches all volumes with a name containing the `rose` string.

​	以下过滤器匹配名称包含 `rose` 字符串的所有卷。



```console
$ docker volume ls -f name=rose

DRIVER              VOLUME NAME
local               rosemary
```

### Format the output (--format)

The formatting options (`--format`) pretty-prints volumes output using a Go template.

​	格式化选项（`--format`）使用 Go 模板美化卷输出。

Valid placeholders for the Go tplate are listed below:

​	Go 模板的有效占位符如下：

| Placeholder   | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| `.Name`       | 卷名称 Volume name                                           |
| `.Driver`     | 卷驱动程序 Volume driver                                     |
| `.Scope`      | 卷范围（本地、全局） Volume scope (local, global)            |
| `.Mountpoint` | 主机上卷的挂载点The mount point of the volume on the host    |
| `.Labels`     | 分配给卷的所有标签All labels assigned to the volume          |
| `.Label`      | 此卷的特定标签的值，例如 `{{.Label "project.version"}}`Value of a specific label for this volume. For example `{{.Label "project.version"}}` |

When using the `--format` option, the `volume ls` command will either output the data exactly as the template declares or, when using the `table` directive, includes column headers as well.

​	使用 `--format` 选项时，`volume ls` 命令将输出符合模板声明的数据，或者在使用 `table` 指令时，包含列标题。

The following example uses a template without headers and outputs the `Name` and `Driver` entries separated by a colon (`:`) for all volumes:

​	以下示例使用不带标题的模板，输出所有卷的 `Name` 和 `Driver` 条目，以冒号（`:`）分隔：



```console
$ docker volume ls --format "{{.Name}}: {{.Driver}}"

vol1: local
vol2: local
vol3: local
```

To list all volumes in JSON format, use the `json` directive:

​	要以 JSON 格式列出所有卷，请使用 `json` 指令：

```console
$ docker volume ls --format json
{"Driver":"local","Labels":"","Links":"N/A","Mountpoint":"/var/lib/docker/volumes/docker-cli-dev-cache/_data","Name":"docker-cli-dev-cache","Scope":"local","Size":"N/A"}
```

