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

## Description

Inspects the specified service.

By default, this renders all results in a JSON array. If a format is specified, the given template will be executed for each result.

Go's [text/template](https://pkg.go.dev/text/template) package describes all the details of the format.

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --format`](https://docs.docker.com/reference/cli/docker/service/inspect/#format) |         | Format output using a custom template: 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| [`--pretty`](https://docs.docker.com/reference/cli/docker/service/inspect/#pretty) |         | Print the information in a human friendly format             |

## Examples

### Inspect a service by name or ID

You can inspect a service, either by its *name*, or *ID*

For example, given the following service;



```console
$ docker service ls
ID            NAME   MODE        REPLICAS  IMAGE
dmu1ept4cxcf  redis  replicated  3/3       redis:3.0.6
```

Both `docker service inspect redis`, and `docker service inspect dmu1ept4cxcf` produce the same result:



```console
$ docker service inspect redis
```

The output is in JSON format, for example:



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

### Formatting (--pretty)

You can print the inspect output in a human-readable format instead of the default JSON output, by using the `--pretty` option:



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

### Format the output (--format)

You can use the --format option to obtain specific information about a The `--format` option can be used to obtain specific information about a service. For example, the following command outputs the number of replicas of the "redis" service.



```console
$ docker service inspect --format='{{.Spec.Mode.Replicated.Replicas}}' redis

10
```
