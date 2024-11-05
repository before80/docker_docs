+++
title = "docker plugin ls"
date = 2024-10-23T14:54:43+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/plugin/ls/](https://docs.docker.com/reference/cli/docker/plugin/ls/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker plugin ls

| Description | List plugins                 |
| :---------- | ---------------------------- |
| Usage       | `docker plugin ls [OPTIONS]` |
| Aliases     | `docker plugin list`         |

## Description

Lists all the plugins that are currently installed. You can install plugins using the [`docker plugin install`]({{< ref "/reference/CLIreference/docker/dockerplugin/dockerplugininstall" >}}) command. You can also filter using the `-f` or `--filter` flag. Refer to the [filtering](https://docs.docker.com/reference/cli/docker/plugin/ls/#filter) section for more information about available filter options.

​	列出当前安装的所有插件。可以使用 [`docker plugin install`]({{< ref "/reference/CLIreference/docker/dockerplugin/dockerplugininstall" >}}) 命令安装插件。您还可以使用 `-f` 或 `--filter` 标志进行筛选。有关可用筛选选项的更多信息，请参阅[筛选](https://docs.docker.com/reference/cli/docker/plugin/ls/#filter)部分。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --filter`](https://docs.docker.com/reference/cli/docker/plugin/ls/#filter) |         | 提供筛选值（例如 `enabled=true`） Provide filter values (e.g. `enabled=true`) |
| [`--format`](https://docs.docker.com/reference/cli/docker/plugin/ls/#format) |         | 使用自定义模板格式化输出：'table'：以表格格式输出并带有列标题（默认） 'table TEMPLATE'：以指定的 Go 模板格式输出表格 'json'：以 JSON 格式输出 'TEMPLATE'：使用给定的 Go 模板格式输出。更多信息请参阅 https://docs.docker.com/go/formatting/   Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `--no-trunc`                                                 |         | 不截断输出  Don't truncate output                            |
| `-q, --quiet`                                                |         | 仅显示插件 ID  Only display plugin IDs                       |

## Examples



```console
$ docker plugin ls

ID            NAME                                    DESCRIPTION                ENABLED
69553ca1d123  tiborvass/sample-volume-plugin:latest   A test plugin for Docker   true
```

### Filtering (`--filter`)

The filtering flag (`-f` or `--filter`) format is of "key=value". If there is more than one filter, then pass multiple flags (e.g., `--filter "foo=bar" --filter "bif=baz"`).

​	筛选标志（`-f` 或 `--filter`）格式为“key=value”。如果有多个筛选条件，则传递多个标志（例如 `--filter "foo=bar" --filter "bif=baz"`）。

The currently supported filters are:

​	当前支持的筛选器包括：

- enabled (boolean - true or false, 0 or 1)

  - enabled（布尔值 - true 或 false, 0 或 1）

- capability (string - currently `volumedriver`, `networkdriver`, `ipamdriver`, `logdriver`, `metricscollector`, or `authz`)

  - capability（字符串 - 当前支持 `volumedriver`、`networkdriver`、`ipamdriver`、`logdriver`、`metricscollector` 或 `authz`）

  

#### enabled

The `enabled` filter matches on plugins enabled or disabled.

​	`enabled` 筛选器用于匹配启用或禁用的插件。

#### capability

The `capability` filter matches on plugin capabilities. One plugin might have multiple capabilities. Currently `volumedriver`, `networkdriver`, `ipamdriver`, `logdriver`, `metricscollector`, and `authz` are supported capabilities.

​	`capability` 筛选器用于匹配插件的功能。一个插件可能具有多种功能。当前支持的功能包括 `volumedriver`、`networkdriver`、`ipamdriver`、`logdriver`、`metricscollector` 和 `authz`。



```console
$ docker plugin install --disable vieux/sshfs

Installed plugin vieux/sshfs

$ docker plugin ls --filter enabled=true

ID                  NAME                DESCRIPTION         ENABLED
```

### Format the output (`--format`)

The formatting options (`--format`) pretty-prints plugins output using a Go template.

​	格式化选项（`--format`）使用 Go 模板对插件输出进行漂亮打印。

Valid placeholders for the Go template are listed below:

​	以下是 Go 模板中有效的占位符：

| Placeholder        | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| `.ID`              | 插件 ID  Plugin ID                                           |
| `.Name`            | 插件名称和标签  Plugin name and tag                          |
| `.Description`     | 插件描述 Plugin description                                  |
| `.Enabled`         | 插件是否已启用 Whether plugin is enabled or not              |
| `.PluginReference` | 用于推送/拉取注册表的引用 The reference used to push/pull from a registry |

When using the `--format` option, the `plugin ls` command will either output the data exactly as the template declares or, when using the `table` directive, includes column headers as well.

​	使用 `--format` 选项时，`plugin ls` 命令将以模板定义的格式精确输出数据，或者在使用 `table` 指令时包含列标题。

The following example uses a template without headers and outputs the `ID` and `Name` entries separated by a colon (`:`) for all plugins:

​	以下示例使用不带标题的模板，并为所有插件输出 `ID` 和 `Name` 条目，以冒号（`:`）分隔：



```console
$ docker plugin ls --format "{{.ID}}: {{.Name}}"

4be01827a72e: vieux/sshfs:latest
```

To list all plugins in JSON format, use the `json` directive:

​	要以 JSON 格式列出所有插件，请使用 `json` 指令：

```console
$ docker plugin ls --format json
{"Description":"sshFS plugin for Docker","Enabled":false,"ID":"856d89febb1c","Name":"vieux/sshfs:latest","PluginReference":"docker.io/vieux/sshfs:latest"}
```

