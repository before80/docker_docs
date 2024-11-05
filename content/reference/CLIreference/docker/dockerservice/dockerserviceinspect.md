+++
title = "docker service inspect"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/service/inspect/](https://docs.docker.com/reference/cli/docker/service/inspect/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker service inspect

| Description | Display detailed information on one or more services    |
| :---------- | ------------------------------------------------------- |
| Usage       | `docker service inspect [OPTIONS] SERVICE [SERVICE...]` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 该命令适用于 Swarm 编排器。

## Description

Inspects the specified service.

​	检查指定的服务。

By default, this renders all results in a JSON array. If a format is specified, the given template will be executed for each result.

​	默认情况下，此操作会将所有结果以 JSON 数组格式呈现。如果指定格式，则会针对每个结果执行给定的模板。

Go's [text/template](https://pkg.go.dev/text/template) package describes all the details of the format.

​	Go 的 [text/template](https://pkg.go.dev/text/template) 包描述了所有的格式详情。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理节点和工作节点，请参阅文档中的 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --format`](https://docs.docker.com/reference/cli/docker/service/inspect/#format) |         | 使用自定义模板格式化输出：'json': 以 JSON 格式输出 'TEMPLATE': 使用给定的 Go 模板输出。有关模板格式化的更多信息，请参阅 https://docs.docker.com/go/formatting/  Format output using a custom template: 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| [`--pretty`](https://docs.docker.com/reference/cli/docker/service/inspect/#pretty) |         | 以人性化格式打印信息  Print the information in a human friendly format |

## Examples

### 通过名称或 ID 检查服务 Inspect a service by name or ID

You can inspect a service, either by its *name*, or *ID*

​	您可以通过服务的 *名称* 或 *ID* 来检查服务。

For example, given the following service;

​	例如，以下是一个服务：

```console
$ docker service ls
ID            NAME   MODE        REPLICAS  IMAGE
dmu1ept4cxcf  redis  replicated  3/3       redis:3.0.6
```

Both `docker service inspect redis`, and `docker service inspect dmu1ept4cxcf` produce the same result:

​	`docker service inspect redis` 和 `docker service inspect dmu1ept4cxcf` 的结果相同：

```console
$ docker service inspect redis
```

The output is in JSON format, for example:

​	输出为 JSON 格式，例如：

```json
[
  {
    "ID": "dmu1ept4cxcfe8k8lhtux3ro3",
    "Version": {
      "Index": 12
    },
    "CreatedAt": "2016-06-17T18:44:02.558012087Z",
    "UpdatedAt": "2016-06-17T18:44:02.558012087Z",
    "Spec": {
      "Name": "redis",
      "TaskTemplate": {
        "ContainerSpec": {
          "Image": "redis:3.0.6"
        },
        "Resources": {
          "Limits": {},
          "Reservations": {}
        },
        "RestartPolicy": {
          "Condition": "any",
          "MaxAttempts": 0
        },
        "Placement": {}
      },
      "Mode": {
        "Replicated": {
          "Replicas": 1
        }
      },
      "UpdateConfig": {},
      "EndpointSpec": {
        "Mode": "vip"
      }
    },
    "Endpoint": {
      "Spec": {}
    }
  }
]
```



```console
$ docker service inspect dmu1ept4cxcf

[
  {
    "ID": "dmu1ept4cxcfe8k8lhtux3ro3",
    "Version": {
      "Index": 12
    },
    ...
  }
]
```

### Formatting (`--pretty`)

You can print the inspect output in a human-readable format instead of the default JSON output, by using the `--pretty` option:

​	您可以使用 `--pretty` 选项以人类可读的格式而不是默认的 JSON 输出打印检查结果：



```console
$ docker service inspect --pretty frontend

ID:     c8wgl7q4ndfd52ni6qftkvnnp
Name:   frontend
Labels:
 - org.example.projectname=demo-app
Service Mode:   REPLICATED
 Replicas:      5
Placement:
UpdateConfig:
 Parallelism:   0
 On failure:    pause
 Max failure ratio: 0
ContainerSpec:
 Image:     nginx:alpine
Resources:
Networks:   net1
Endpoint Mode:  vip
Ports:
 PublishedPort = 4443
  Protocol = tcp
  TargetPort = 443
  PublishMode = ingress
```

You can also use `--format pretty` for the same effect.

​	您也可以使用 `--format pretty` 达到相同效果。

### Format the output (`--format`)

You can use the --format option to obtain specific information about a The `--format` option can be used to obtain specific information about a service. For example, the following command outputs the number of replicas of the "redis" service.

​	可以使用 `--format` 选项获取有关服务的特定信息。例如，以下命令输出 “redis” 服务的副本数量。



```console
$ docker service inspect --format='{{.Spec.Mode.Replicated.Replicas}}' redis

10
```
