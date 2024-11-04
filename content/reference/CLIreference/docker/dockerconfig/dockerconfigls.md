+++
title = "docker config ls"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/config/ls/](https://docs.docker.com/reference/cli/docker/config/ls/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker config ls

| Description | List configs                 |
| :---------- | ---------------------------- |
| Usage       | `docker config ls [OPTIONS]` |
| Aliases     | `docker config list`         |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令与 Swarm 调度器配合使用。

## Description

Run this command on a manager node to list the configs in the Swarm.

​	在管理节点上运行此命令以列出 Swarm 中的配置。

For detailed information about using configs, refer to [store configuration data using Docker Configs]({{< ref "/manuals/DockerEngine/Swarmmode/StoreconfigurationdatausingDockerConfigs" >}}).

​	有关使用配置的详细信息，请参考 [使用 Docker 配置存储配置数据]({{< ref "/manuals/DockerEngine/Swarmmode/StoreconfigurationdatausingDockerConfigs" >}})。

> **Note**
>
> This is a cluster management command, and must be executed on a Swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理节点和工作节点，请参考文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --filter`](https://docs.docker.com/reference/cli/docker/config/ls/#filter) |         | 根据提供的条件过滤输出 Filter output based on conditions provided |
| [`--format`](https://docs.docker.com/reference/cli/docker/config/ls/#format) |         | 使用自定义模板格式化输出：'table'：以表格格式打印输出，带列标题（默认），'table TEMPLATE'：使用给定的 Go 模板打印表格格式的输出，'json'：以 JSON 格式打印，'TEMPLATE'：使用给定的 Go 模板打印输出。有关使用模板格式化输出的更多信息，请参考 https://docs.docker.com/go/formatting/ Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `-q, --quiet`                                                |         | 仅显示 ID - Only display IDs                                 |

## Examples



```console
$ docker config ls

ID                          NAME                        CREATED             UPDATED
6697bflskwj1998km1gnnjr38   q5s5570vtvnimefos1fyeo2u2   6 weeks ago         6 weeks ago
9u9hk4br2ej0wgngkga6rp4hq   my_config                   5 weeks ago         5 weeks ago
mem02h8n73mybpgqjf0kfi1n0   test_config                 3 seconds ago       3 seconds ago
```

### 过滤（`-f`, `--filter`） Filtering (`-f`, `--filter`)

The filtering flag (`-f` or `--filter`) format is a `key=value` pair. If there is more than one filter, then pass multiple flags (e.g., `--filter "foo=bar" --filter "bif=baz"`)

​	过滤标志（`-f` 或 `--filter`）格式为 `key=value` 对。如果有多个过滤条件，则传递多个标志（例如，`--filter "foo=bar" --filter "bif=baz"`）。

The currently supported filters are:

​	当前支持的过滤条件包括：

- [id](https://docs.docker.com/reference/cli/docker/config/ls/#id) (config's ID)
  - [id](https://docs.docker.com/reference/cli/docker/config/ls/#id)（配置的 ID）

- [label](https://docs.docker.com/reference/cli/docker/config/ls/#label) (`label=<key>` or `label=<key>=<value>`)
  - [label](https://docs.docker.com/reference/cli/docker/config/ls/#label)（`label=<key>` 或 `label=<key>=<value>`）

- [name](https://docs.docker.com/reference/cli/docker/config/ls/#name) (config's name)
  - [name](https://docs.docker.com/reference/cli/docker/config/ls/#name)（配置的名称）


#### id

The `id` filter matches all or prefix of a config's id.

​	`id` 过滤条件匹配配置 ID 的所有或前缀。



```console
$ docker config ls -f "id=6697bflskwj1998km1gnnjr38"

ID                          NAME                        CREATED             UPDATED
6697bflskwj1998km1gnnjr38   q5s5570vtvnimefos1fyeo2u2   6 weeks ago         6 weeks ago
```

#### label

The `label` filter matches configs based on the presence of a `label` alone or a `label` and a value.

​	`label` 过滤条件根据标签的存在匹配配置，单独或与值一起匹配。

The following filter matches all configs with a `project` label regardless of its value:

​	以下过滤器匹配所有带有 `project` 标签的配置，不论其值如何：



```console
$ docker config ls --filter label=project

ID                          NAME                        CREATED             UPDATED
mem02h8n73mybpgqjf0kfi1n0   test_config                 About an hour ago   About an hour ago
```

The following filter matches only services with the `project` label with the `project-a` value.

​	以下过滤器仅匹配值为 `project-a` 的 `project` 标签的服务。



```console
$ docker service ls --filter label=project=test

ID                          NAME                        CREATED             UPDATED
mem02h8n73mybpgqjf0kfi1n0   test_config                 About an hour ago   About an hour ago
```

#### name

The `name` filter matches on all or prefix of a config's name.

​	`name` 过滤条件匹配配置名称的所有或前缀。

The following filter matches config with a name containing a prefix of `test`.

​	以下过滤器匹配名称包含前缀 `test` 的配置。



```console
$ docker config ls --filter name=test_config

ID                          NAME                        CREATED             UPDATED
mem02h8n73mybpgqjf0kfi1n0   test_config                 About an hour ago   About an hour ago
```

### 格式化输出（`--format`） Format the output (`--format`)

The formatting option (`--format`) pretty prints configs output using a Go template.

​	格式化选项（`--format`）使用 Go 模板美观地打印配置输出。

Valid placeholders for the Go templa are listed below:

​	Go 模板的有效占位符如下所示：

| Placeholder  | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| `.ID`        | 配置 ID - Config ID                                          |
| `.Name`      | 配置名称 Config name                                         |
| `.CreatedAt` | 配置创建时间 Time when the config was created                |
| `.UpdatedAt` | 配置更新时间 Time when the config was updated                |
| `.Labels`    | 分配给配置的所有标签 All labels assigned to the config       |
| `.Label`     | 此配置的特定标签的值。例如 `{{.Label "my-label"}}` Value of a specific label for this config. For example `{{.Label "my-label"}}` |

When using the `--format` option, the `config ls` command will either output the data exactly as the template declares or, when using the `table` directive, will include column headers as well.

​	使用 `--format` 选项时，`config ls` 命令将根据模板声明输出数据，或者在使用 `table` 指令时，将包含列标题。

The following example uses a template without headers and outputs the `ID` and `Name` entries separated by a colon (`:`) for all images:

​	以下示例使用不带标题的模板，并输出 `ID` 和 `Name` 条目，以冒号（`:`）分隔所有配置：



```console
$ docker config ls --format "{{.ID}}: {{.Name}}"

77af4d6b9913: config-1
b6fa739cedf5: config-2
78a85c484f71: config-3
```

To list all configs with their name and created date in a table format you can use:

​	要以表格格式列出所有配置的名称和创建日期，可以使用：



```console
$ docker config ls --format "table {{.ID}}\t{{.Name}}\t{{.CreatedAt}}"

ID                  NAME                      CREATED
77af4d6b9913        config-1                  5 minutes ago
b6fa739cedf5        config-2                  3 hours ago
78a85c484f71        config-3                  10 days ago
```
