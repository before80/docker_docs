+++
title = "docker secret ls"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/secret/ls/](https://docs.docker.com/reference/cli/docker/secret/ls/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker secret ls

| Description | List secrets                 |
| :---------- | ---------------------------- |
| Usage       | `docker secret ls [OPTIONS]` |
| Aliases     | `docker secret list`         |

Swarm This command works with the Swarm orchestrator.

​	Swarm 该命令可在 Swarm 编排器中使用。

## Description

Run this command on a manager node to list the secrets in the swarm.

​	在管理节点上运行该命令以列出 Swarm 中的机密。

For detailed information about using secrets, refer to [manage sensitive data with Docker secrets]({{< ref "/manuals/DockerEngine/Swarmmode/ManagesensitivedatawithDockersecrets" >}}).

​	有关使用机密的详细信息，请参阅 [使用 Docker 机密管理敏感数据]({{< ref "/manuals/DockerEngine/Swarmmode/ManagesensitivedatawithDockersecrets" >}})。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。关于管理节点和工作节点的更多信息，请参阅文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --filter`](https://docs.docker.com/reference/cli/docker/secret/ls/#filter) |         | 根据提供的条件过滤输出 Filter output based on conditions provided |
| [`--format`](https://docs.docker.com/reference/cli/docker/secret/ls/#format) |         | 使用自定义模板格式化输出：'table': 以表格格式打印输出并带有列标题（默认） 'table TEMPLATE': 使用指定的 Go 模板以表格格式打印输出 'json': 以 JSON 格式打印输出 'TEMPLATE': 使用指定的 Go 模板打印输出。有关使用模板格式化输出的更多信息，请参阅 https://docs.docker.com/go/formatting/  Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `-q, --quiet`                                                |         | 仅显示 ID  Only display IDs                                  |

## Examples



```console
$ docker secret ls

ID                          NAME                        CREATED             UPDATED
6697bflskwj1998km1gnnjr38   q5s5570vtvnimefos1fyeo2u2   6 weeks ago         6 weeks ago
9u9hk4br2ej0wgngkga6rp4hq   my_secret                   5 weeks ago         5 weeks ago
mem02h8n73mybpgqjf0kfi1n0   test_secret                 3 seconds ago       3 seconds ago
```

### Filtering (--filter)

The filtering flag (`-f` or `--filter`) format is a `key=value` pair. If there is more than one filter, then pass multiple flags (e.g., `--filter "foo=bar" --filter "bif=baz"`).

​	过滤标志（`-f` 或 `--filter`）格式为 `key=value`。如果有多个过滤器，则传递多个标志（例如，`--filter "foo=bar" --filter "bif=baz"`）。

The currently supported filters are:

​	当前支持的过滤器包括：

- [id](https://docs.docker.com/reference/cli/docker/secret/ls/#id) (secret's ID)
- [label](https://docs.docker.com/reference/cli/docker/secret/ls/#label) (`label=<key>` or `label=<key>=<value>`)
- [name](https://docs.docker.com/reference/cli/docker/secret/ls/#name) (secret's name)

#### id

The `id` filter matches all or prefix of a secret's id.

​	`id` 过滤器匹配机密 ID 的全部或前缀。

```console
$ docker secret ls -f "id=6697bflskwj1998km1gnnjr38"

ID                          NAME                        CREATED             UPDATED
6697bflskwj1998km1gnnjr38   q5s5570vtvnimefos1fyeo2u2   6 weeks ago         6 weeks ago
```

#### label

The `label` filter matches secrets based on the presence of a `label` alone or a `label` and a value.

​	`label` 过滤器基于标签或标签和值匹配机密。

The following filter matches all secrets with a `project` label regardless of its value:

​	以下过滤器匹配所有具有 `project` 标签的机密，无论其值。



```console
$ docker secret ls --filter label=project

ID                          NAME                        CREATED             UPDATED
mem02h8n73mybpgqjf0kfi1n0   test_secret                 About an hour ago   About an hour ago
```

The following filter matches only services with the `project` label with the `project-a` value.

​	以下过滤器仅匹配带有 `project` 标签且值为 `project-a` 的服务。

```console
$ docker service ls --filter label=project=test

ID                          NAME                        CREATED             UPDATED
mem02h8n73mybpgqjf0kfi1n0   test_secret                 About an hour ago   About an hour ago
```

#### name

The `name` filter matches on all or prefix of a secret's name.

​	`name` 过滤器匹配机密名称的全部或前缀。

The following filter matches secret with a name containing a prefix of `test`.

​	以下过滤器匹配名称前缀为 `test` 的机密。



```console
$ docker secret ls --filter name=test_secret

ID                          NAME                        CREATED             UPDATED
mem02h8n73mybpgqjf0kfi1n0   test_secret                 About an hour ago   About an hour ago
```

### Format the output (`--format`)

The formatting option (`--format`) pretty prints secrets output using a Go template.

​	格式化选项（`--format`）使用 Go 模板美化机密输出。

Valid placeholders for the Go template are listed below:

​	Go 模板的有效占位符如下：

| Placeholder  | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| `.ID`        | 机密 ID  Secret ID                                           |
| `.Name`      | 机密名称  Secret name                                        |
| `.CreatedAt` | 机密创建时间  Time when the secret was created               |
| `.UpdatedAt` | 机密更新时间  Time when the secret was updated               |
| `.Labels`    | 机密的所有标签  All labels assigned to the secret            |
| `.Label`     | 机密中特定标签的值。例如 `{{.Label "secret.ssh.key"}}`  Value of a specific label for this secret. For example `{{.Label "secret.ssh.key"}}` |

When using the `--format` option, the `secret ls` command will either output the data exactly as the template declares or, when using the `table` directive, will include column headers as well.

​	使用 `--format` 选项时，`secret ls` 命令将按模板声明输出数据，或者在使用 `table` 指令时包含列标题。

The following example uses a template without headers and outputs the `ID` and `Name` entries separated by a colon (`:`) for all images:

​	以下示例使用无标题模板，并为所有镜像按 `ID` 和 `Name` 条目以冒号（`:`）分隔输出：



```console
$ docker secret ls --format "{{.ID}}: {{.Name}}"

77af4d6b9913: secret-1
b6fa739cedf5: secret-2
78a85c484f71: secret-3
```

To list all secrets with their name and created date in a table format you can use:

​	要以表格格式列出所有机密的名称和创建日期，可以使用：



```console
$ docker secret ls --format "table {{.ID}}\t{{.Name}}\t{{.CreatedAt}}"

ID                  NAME                      CREATED
77af4d6b9913        secret-1                  5 minutes ago
b6fa739cedf5        secret-2                  3 hours ago
78a85c484f71        secret-3                  10 days ago
```

To list all secrets in JSON format, use the `json` directive:

​	要以 JSON 格式列出所有机密，请使用 `json` 指令：

```console
$ docker secret ls --format json
{"CreatedAt":"28 seconds ago","Driver":"","ID":"4y7hvwrt1u8e9uxh5ygqj7mzc","Labels":"","Name":"mysecret","UpdatedAt":"28 seconds ago"}
```
