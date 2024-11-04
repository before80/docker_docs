+++
title = "docker buildx ls"
date = 2024-10-23T14:54:43+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/buildx/ls/](https://docs.docker.com/reference/cli/docker/buildx/ls/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx ls

| Description | List builder instances |
| :---------- | ---------------------- |
| Usage       | `docker buildx ls`     |

## Description

Lists all builder instances and the nodes for each instance.

​	列出所有构建器实例及每个实例的节点。

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

​	每个构建器都有一个或多个与之关联的节点。当前构建器的名称在 `NAME/NODE` 中用 `*` 标记，而针对目标平台的显式构建节点在 `PLATFORMS` 列中用 `*` 标记。

## Options

| Option                                                       | Default | Description                  |
| ------------------------------------------------------------ | ------- | ---------------------------- |
| [`--format`](https://docs.docker.com/reference/cli/docker/buildx/ls/#format) | `table` | 格式化输出 Format the output |

## Examples

### 格式化输出（`--format`） Format the output (`--format`)

The formatting options (`--format`) pretty-prints builder instances output using a Go template.

​	格式化选项（`--format`）使用 Go 模板对构建器实例的输出进行美化打印。

Valid placeholders for the Go template are listed below:

​	有效的 Go 模板占位符如下：

| Placeholder       | Description                                                  |
| ----------------- | ------------------------------------------------------------ |
| `.Name`           | 构建器或节点名称Builder or node name                         |
| `.DriverEndpoint` | 驱动程序（对于构建器）或端点（对于节点）Driver (for builder) or Endpoint (for node) |
| `.LastActivity`   | 构建器最后活动时间 Builder last activity                     |
| `.Status`         | 构建器或节点状态 Builder or node status                      |
| `.Buildkit`       | 节点的 BuildKit 版本 BuildKit version of the node            |
| `.Platforms`      | 可用的节点平台 Available node's platforms                    |
| `.Error`          | 错误信息 Error                                               |
| `.Builder`        | 构建器对象 Builder object                                    |

When using the `--format` option, the `ls` command will either output the data exactly as the template declares or, when using the `table` directive, includes column headers as well.

​	使用 `--format` 选项时，`ls` 命令将根据模板的声明输出数据，或者在使用 `table` 指令时包含列标题。

The following example uses a template without headers and outputs the `Name` and `DriverEndpoint` entries separated by a colon (`:`):

​	以下示例使用没有标题的模板，输出 `Name` 和 `DriverEndpoint` 条目，以冒号（`:`）分隔：



```console
$ docker buildx ls --format "{{.Name}}: {{.DriverEndpoint}}"
elated_tesla: docker-container
elated_tesla0: unix:///var/run/docker.sock
elated_tesla1: ssh://ubuntu@1.2.3.4
default: docker
default: default
```

The `Builder` placeholder can be used to access the builder object and its fields. For example, the following template outputs the builder's and nodes' names with their respective endpoints:

​	可以使用 `Builder` 占位符访问构建器对象及其字段。例如，以下模板输出构建器及节点的名称和各自的端点：



```console
$ docker buildx ls --format "{{.Builder.Name}}: {{range .Builder.Nodes}}\n  {{.Name}}: {{.Endpoint}}{{end}}"
elated_tesla:
  elated_tesla0: unix:///var/run/docker.sock
  elated_tesla1: ssh://ubuntu@1.2.3.4
default: docker
  default: default
```
