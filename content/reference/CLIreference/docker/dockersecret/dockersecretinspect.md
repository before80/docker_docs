+++
title = "docker secret inspect"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/secret/inspect/](https://docs.docker.com/reference/cli/docker/secret/inspect/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker secret inspect

| Description | Display detailed information on one or more secrets  |
| :---------- | ---------------------------------------------------- |
| Usage       | `docker secret inspect [OPTIONS] SECRET [SECRET...]` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 该命令可在 Swarm 编排器中使用。

## Description

Inspects the specified secret.

​	检查指定的机密。

By default, this renders all results in a JSON array. If a format is specified, the given template will be executed for each result.

​	默认情况下，所有结果会以 JSON 数组格式呈现。如果指定了格式，则会为每个结果执行给定的模板。

Go's [text/template](https://pkg.go.dev/text/template) package describes all the details of the format.

​	Go 的 [text/template](https://pkg.go.dev/text/template) 包描述了格式的所有细节。

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
| [`-f, --format`](https://docs.docker.com/reference/cli/docker/secret/inspect/#format) |         | 使用自定义模板格式化输出：'json': 以 JSON 格式打印 'TEMPLATE': 使用指定的 Go 模板打印输出。有关使用模板格式化输出的更多信息，请参阅 https://docs.docker.com/go/formatting/  Format output using a custom template: 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `--pretty`                                                   |         | 以人性化的格式显示信息 Print the information in a human friendly format |

## Examples

### 通过名称或 ID 检查机密 Inspect a secret by name or ID

You can inspect a secret, either by its name or ID.

​	您可以通过名称或 ID 检查一个机密。

For example, given the following secret:

​	例如，给定以下机密：

```console
$ docker secret ls

ID                          NAME                CREATED             UPDATED
eo7jnzguqgtpdah3cm5srfb97   my_secret           3 minutes ago       3 minutes ago
```



```console
$ docker secret inspect secret.json
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
      "Name": "my_secret",
      "Labels": {
        "env": "dev",
        "rev": "20170324"
      }
    }
  }
]
```

### Format the output (`--format`)

You can use the `--format` option to obtain specific information about a secret. The following example command outputs the creation time of the secret.

​	您可以使用 `--format` 选项获取关于机密的特定信息。以下示例命令输出机密的创建时间。

```console
$ docker secret inspect --format='{{.CreatedAt}}' eo7jnzguqgtpdah3cm5srfb97

2017-03-24 08:15:09.735271783 +0000 UTC
```
