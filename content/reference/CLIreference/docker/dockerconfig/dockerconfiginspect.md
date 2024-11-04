+++
title = "docker config inspect"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/config/inspect/](https://docs.docker.com/reference/cli/docker/config/inspect/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker config inspect

| Description | Display detailed information on one or more configs  |
| :---------- | ---------------------------------------------------- |
| Usage       | `docker config inspect [OPTIONS] CONFIG [CONFIG...]` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令与 Swarm 调度器配合使用。

## Description

Inspects the specified config.

​	检查指定的配置。

By default, this renders all results in a JSON array. If a format is specified, the given template will be executed for each result.

​	默认情况下，所有结果以 JSON 数组格式呈现。如果指定了格式，将为每个结果执行给定模板。

Go's [text/template](https://pkg.go.dev/text/template) package describes all the details of the format.

​	Go 的 [text/template](https://pkg.go.dev/text/template) 包描述了格式的所有细节。

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
| [`-f, --format`](https://docs.docker.com/reference/cli/docker/config/inspect/#format) |         | 使用自定义模板格式化输出：'json'：以 JSON 格式打印，'TEMPLATE'：使用给定的 Go 模板打印输出。有关使用模板格式化输出的更多信息，请参考 https://docs.docker.com/go/formatting/  Format output using a custom template: 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `--pretty`                                                   |         | 以人类友好的格式打印信息 Print the information in a human friendly format |

## Examples

### 通过名称或 ID 检查配置 Inspect a config by name or ID

You can inspect a config, either by its *name*, or *ID*

​	您可以通过配置的 *名称* 或 *ID* 来检查配置。

For example, given the following config:

​	例如，给定以下配置：

```console
$ docker config ls

ID                          NAME                CREATED             UPDATED
eo7jnzguqgtpdah3cm5srfb97   my_config           3 minutes ago       3 minutes ago
```



```console
$ docker config inspect config.json
```

The output is in JSON format, for example:

​	输出为 JSON 格式，例如：

```json
[
  {
    "ID": "eo7jnzguqgtpdah3cm5srfb97",
    "Version": {
      "Index": 17
    },
    "CreatedAt": "2017-03-24T08:15:09.735271783Z",
    "UpdatedAt": "2017-03-24T08:15:09.735271783Z",
    "Spec": {
      "Name": "my_config",
      "Labels": {
        "env": "dev",
        "rev": "20170324"
      },
      "Data": "aGVsbG8K"
    }
  }
]
```

### 格式化输出（`--format`） Format the output (`--format`)

You can use the --format option to obtain specific information about a config. The following example command outputs the creation time of the config.

​	您可以使用 `--format` 选项获取关于配置的特定信息。以下示例命令输出配置的创建时间。

```console
$ docker config inspect --format='{{.CreatedAt}}' eo7jnzguqgtpdah3cm5srfb97

2017-03-24 08:15:09.735271783 +0000 UTC
```
