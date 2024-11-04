+++
title = "docker network ls"
date = 2024-10-23T14:54:43+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/network/ls/](https://docs.docker.com/reference/cli/docker/network/ls/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker network ls

| Description | List networks                 |
| :---------- | ----------------------------- |
| Usage       | `docker network ls [OPTIONS]` |
| Aliases     | `docker network list`         |

## Description

Lists all the networks the Engine `daemon` knows about. This includes the networks that span across multiple hosts in a cluster.

​	列出引擎 `daemon` 所知的所有网络。这包括跨集群中多个主机的网络。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --filter`](https://docs.docker.com/reference/cli/docker/network/ls/#filter) |         | 提供过滤器值（例如 `driver=bridge`） Provide filter values (e.g. `driver=bridge`) |
| [`--format`](https://docs.docker.com/reference/cli/docker/network/ls/#format) |         | 使用自定义模板格式化输出: 'table': 以带有列标题的表格格式打印输出（默认） 'table TEMPLATE': 使用给定的 Go 模板格式化输出 'json': 以 JSON 格式打印输出 'TEMPLATE': 使用给定的 Go 模板格式化输出。详细信息请参阅 https://docs.docker.com/go/formatting/  Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `--no-trunc`                                                 |         | 不截断输出 Do not truncate the output                        |
| `-q, --quiet`                                                |         | 仅显示网络 ID Only display network IDs                       |

## Examples

### 列出所有网络 List all networks



```console
$ docker network ls
NETWORK ID          NAME                DRIVER          SCOPE
7fca4eb8c647        bridge              bridge          local
9f904ee27bf5        none                null            local
cf03ee007fb4        host                host            local
78b03ee04fc4        multi-host          overlay         swarm
```

Use the `--no-trunc` option to display the full network id:

​	使用 `--no-trunc` 选项显示完整的网络 ID：

```console
$ docker network ls --no-trunc
NETWORK ID                                                         NAME                DRIVER           SCOPE
18a2866682b85619a026c81b98a5e375bd33e1b0936a26cc497c283d27bae9b3   none                null             local
c288470c46f6c8949c5f7e5099b5b7947b07eabe8d9a27d79a9cbf111adcbf47   host                host             local
7b369448dccbf865d397c8d2be0cda7cf7edc6b0945f77d2529912ae917a0185   bridge              bridge           local
95e74588f40db048e86320c6526440c504650a1ff3e9f7d60a497c4d2163e5bd   foo                 bridge           local
63d1ff1f77b07ca51070a8c227e962238358bd310bde1529cf62e6c307ade161   dev                 bridge           local
```

### Filtering (`--filter`)

The filtering flag (`-f` or `--filter`) format is a `key=value` pair. If there is more than one filter, then pass multiple flags (e.g. `--filter "foo=bar" --filter "bif=baz"`). Multiple filter flags are combined as an `OR` filter. For example, `-f type=custom -f type=builtin` returns both `custom` and `builtin` networks.

​	过滤标志（`-f` 或 `--filter`）格式为 `key=value` 对。如果有多个过滤条件，则传递多个标志（例如 `--filter "foo=bar" --filter "bif=baz"`）。多个过滤标志作为 `OR` 过滤器组合。例如，`-f type=custom -f type=builtin` 返回 `custom` 和 `builtin` 两种类型的网络。

The currently supported filters are:

​	当前支持的过滤器有：

- driver
- id (network's id) （网络 ID）
- label (`label=<key>` or `label=<key>=<value>`)
- name (network's name)
- scope (`swarm|global|local`)
- type (`custom|builtin`)

#### Driver

The `driver` filter matches networks based on their driver.

​	`driver` 过滤器根据网络的驱动程序匹配网络。

The following example matches networks with the `bridge` driver:

​	以下示例匹配 `bridge` 驱动程序的网络：

```console
$ docker network ls --filter driver=bridge
NETWORK ID          NAME                DRIVER            SCOPE
db9db329f835        test1               bridge            local
f6e212da9dfd        test2               bridge            local
```

#### ID

The `id` filter matches on all or part of a network's ID.

​	`id` 过滤器匹配网络 ID 的全部或部分。

The following filter matches all networks with an ID containing the `63d1ff1f77b0...` string.

​	以下过滤条件匹配 ID 包含 `63d1ff1f77b0...` 的所有网络。

```console
$ docker network ls --filter id=63d1ff1f77b07ca51070a8c227e962238358bd310bde1529cf62e6c307ade161
NETWORK ID          NAME                DRIVER           SCOPE
63d1ff1f77b0        dev                 bridge           local
```

You can also filter for a substring in an ID as this shows:

​	还可以过滤 ID 的子字符串，如下所示：

```console
$ docker network ls --filter id=95e74588f40d
NETWORK ID          NAME                DRIVER          SCOPE
95e74588f40d        foo                 bridge          local

$ docker network ls --filter id=95e
NETWORK ID          NAME                DRIVER          SCOPE
95e74588f40d        foo                 bridge          local
```

#### Label

The `label` filter matches networks based on the presence of a `label` alone or a `label` and a value.

​	`label` 过滤器根据标签或标签和值匹配网络。

The following filter matches networks with the `usage` label regardless of its value.

​	以下过滤条件匹配具有 `usage` 标签的网络，无论其值是什么。



```console
$ docker network ls -f "label=usage"
NETWORK ID          NAME                DRIVER         SCOPE
db9db329f835        test1               bridge         local
f6e212da9dfd        test2               bridge         local
```

The following filter matches networks with the `usage` label with the `prod` value.

​	以下过滤条件匹配 `usage` 标签的值为 `prod` 的网络。



```console
$ docker network ls -f "label=usage=prod"
NETWORK ID          NAME                DRIVER        SCOPE
f6e212da9dfd        test2               bridge        local
```

#### Name

The `name` filter matches on all or part of a network's name.

​	`name` 过滤器匹配网络名称的全部或部分。

The following filter matches all networks with a name containing the `foobar` string.

​	以下过滤条件匹配名称包含 `foobar` 的所有网络。



```console
$ docker network ls --filter name=foobar
NETWORK ID          NAME                DRIVER       SCOPE
06e7eef0a170        foobar              bridge       local
```

You can also filter for a substring in a name as this shows:

​	还可以过滤名称的子字符串，如下所示：



```console
$ docker network ls --filter name=foo
NETWORK ID          NAME                DRIVER       SCOPE
95e74588f40d        foo                 bridge       local
06e7eef0a170        foobar              bridge       local
```

#### Scope

The `scope` filter matches networks based on their scope.

​	`scope` 过滤器根据范围匹配网络。

The following example matches networks with the `swarm` scope:

​	以下示例匹配 `swarm` 范围的网络：



```console
$ docker network ls --filter scope=swarm
NETWORK ID          NAME                DRIVER              SCOPE
xbtm0v4f1lfh        ingress             overlay             swarm
ic6r88twuu92        swarmnet            overlay             swarm
```

The following example matches networks with the `local` scope:

​	以下示例匹配 `local` 范围的网络：



```console
$ docker network ls --filter scope=local
NETWORK ID          NAME                DRIVER              SCOPE
e85227439ac7        bridge              bridge              local
0ca0e19443ed        host                host                local
ca13cc149a36        localnet            bridge              local
f9e115d2de35        none                null                local
```

#### Type

The `type` filter supports two values; `builtin` displays predefined networks (`bridge`, `none`, `host`), whereas `custom` displays user defined networks.

​	`type` 过滤器支持两个值；`builtin` 显示预定义的网络（`bridge`, `none`, `host`），而 `custom` 显示用户定义的网络。

The following filter matches all user defined networks:

​	以下过滤条件匹配所有用户定义的网络：



```console
$ docker network ls --filter type=custom
NETWORK ID          NAME                DRIVER       SCOPE
95e74588f40d        foo                 bridge       local
63d1ff1f77b0        dev                 bridge       local
```

By having this flag it allows for batch cleanup. For example, use this filter to delete all user defined networks:

​	使用此标志可实现批量清理。例如，使用此过滤器删除所有用户定义的网络：



```console
$ docker network rm `docker network ls --filter type=custom -q`
```

A warning will be issued when trying to remove a network that has containers attached.

​	尝试删除连接有容器的网络时将发出警告。

### Format the output (`--format`)

The formatting options (`--format`) pretty-prints networks output using a Go template.

​	格式化选项（`--format`）使用 Go 模板美化网络输出。

Valid placeholders for the Go template are listed below:

​	Go 模板的有效占位符如下所示：

| Placeholder  | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| `.ID`        | Network ID                                                   |
| `.Name`      | Network name                                                 |
| `.Driver`    | Network driver                                               |
| `.Scope`     | Network scope (local, global)                                |
| `.IPv6`      | 是否在网络上启用了 IPv6 Whether IPv6 is enabled on the network or not. |
| `.Internal`  | 网络是否为内部网络 Whether the network is internal or not.   |
| `.Labels`    | 网络分配的所有标签 All labels assigned to the network.       |
| `.Label`     | 此网络中特定标签的值。例如 `{{.Label "project.version"}}` Value of a specific label for this network. For example `{{.Label "project.version"}}` |
| `.CreatedAt` | 网络的创建时间 Time when the network was created             |

When using the `--format` option, the `network ls` command will either output the data exactly as the template declares or, when using the `table` directive, includes column headers as well.

​	使用 `--format` 选项时，`network ls` 命令将按模板直接输出数据，或者在使用 `table` 指令时包含列标题。

The following example uses a template without headers and outputs the `ID` and `Driver` entries separated by a colon (`:`) for all networks:

​	以下示例使用不带标题的模板输出所有网络的 `ID` 和 `Driver` 条目，并用冒号（`:`）分隔：



```console
$ docker network ls --format "{{.ID}}: {{.Driver}}"
afaaab448eb2: bridge
d1584f8dc718: host
391df270dc66: null
```

To list all networks in JSON format, use the `json` directive:

​	要以 JSON 格式列出所有网络，使用 `json` 指令：



```console
$ docker network ls --format json
{"CreatedAt":"2021-03-09 21:41:29.798999529 +0000 UTC","Driver":"bridge","ID":"f33ba176dd8e","IPv6":"false","Internal":"false","Labels":"","Name":"bridge","Scope":"local"}
{"CreatedAt":"2021-03-09 21:41:29.772806592 +0000 UTC","Driver":"host","ID":"caf47bb3ac70","IPv6":"false","Internal":"false","Labels":"","Name":"host","Scope":"local"}
{"CreatedAt":"2021-03-09 21:41:29.752212603 +0000 UTC","Driver":"null","ID":"9d096c122066","IPv6":"false","Internal":"false","Labels":"","Name":"none","Scope":"local"}
```

