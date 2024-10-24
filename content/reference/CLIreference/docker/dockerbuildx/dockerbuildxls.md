+++
title = "docker buildx ls"
date = 2024-10-23T14:54:43+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/buildx/ls/](https://docs.docker.com/reference/cli/docker/buildx/ls/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx ls

| Description | List builder instances |
| :---------- | ---------------------- |
| Usage       | `docker buildx ls`     |

## [Description](https://docs.docker.com/reference/cli/docker/buildx/ls/#description)

Lists all builder instances and the nodes for each instance.



```console
$ docker buildx ls
NAME/NODE           DRIVER/ENDPOINT                   STATUS    BUILDKIT   PLATFORMS
elated_tesla*       docker-container
 \_ elated_tesla0    \_ unix:///var/run/docker.sock   running   v0.10.3    linux/amd64
 \_ elated_tesla1    \_ ssh://ubuntu@1.2.3.4          running   v0.10.3    linux/arm64*, linux/arm/v7, linux/arm/v6
default             docker
 \_ default          \_ default                       running   v0.8.2     linux/amd64
```

Each builder has one or more nodes associated with it. The current builder's name is marked with a `*` in `NAME/NODE` and explicit node to build against for the target platform marked with a `*` in the `PLATFORMS` column.

## [Options](https://docs.docker.com/reference/cli/docker/buildx/ls/#options)

| Option                                                       | Default | Description       |
| ------------------------------------------------------------ | ------- | ----------------- |
| [`--format`](https://docs.docker.com/reference/cli/docker/buildx/ls/#format) | `table` | Format the output |

## [Examples](https://docs.docker.com/reference/cli/docker/buildx/ls/#examples)

### [Format the output (--format)](https://docs.docker.com/reference/cli/docker/buildx/ls/#format)

The formatting options (`--format`) pretty-prints builder instances output using a Go template.

Valid placeholders for the Go template are listed below:

| Placeholder       | Description                                 |
| ----------------- | ------------------------------------------- |
| `.Name`           | Builder or node name                        |
| `.DriverEndpoint` | Driver (for builder) or Endpoint (for node) |
| `.LastActivity`   | Builder last activity                       |
| `.Status`         | Builder or node status                      |
| `.Buildkit`       | BuildKit version of the node                |
| `.Platforms`      | Available node's platforms                  |
| `.Error`          | Error                                       |
| `.Builder`        | Builder object                              |

When using the `--format` option, the `ls` command will either output the data exactly as the template declares or, when using the `table` directive, includes column headers as well.

The following example uses a template without headers and outputs the `Name` and `DriverEndpoint` entries separated by a colon (`:`):



```console
$ docker buildx ls --format "{{.Name}}: {{.DriverEndpoint}}"
elated_tesla: docker-container
elated_tesla0: unix:///var/run/docker.sock
elated_tesla1: ssh://ubuntu@1.2.3.4
default: docker
default: default
```

The `Builder` placeholder can be used to access the builder object and its fields. For example, the following template outputs the builder's and nodes' names with their respective endpoints:



```console
$ docker buildx ls --format "{{.Builder.Name}}: {{range .Builder.Nodes}}\n  {{.Name}}: {{.Endpoint}}{{end}}"
elated_tesla:
  elated_tesla0: unix:///var/run/docker.sock
  elated_tesla1: ssh://ubuntu@1.2.3.4
default: docker
  default: default
```
