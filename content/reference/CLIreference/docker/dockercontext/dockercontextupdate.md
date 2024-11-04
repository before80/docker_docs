+++
title = "docker context update"
date = 2024-10-23T14:54:43+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/context/update/](https://docs.docker.com/reference/cli/docker/context/update/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker context update

| Description | Update a context                          |
| :---------- | ----------------------------------------- |
| Usage       | `docker context update [OPTIONS] CONTEXT` |

## Description

Updates an existing `context`. See [context create]({{< ref "/reference/CLIreference/docker/dockercontext/dockercontextcreate" >}}).

​	更新现有的 `context`。参见 context create。

## Options

| Option          | Default | Description                              |
| --------------- | ------- | ---------------------------------------- |
| `--description` |         | 上下文的描述 Description of the context  |
| `--docker`      |         | 设置 docker 端点 set the docker endpoint |

## Examples

### 更新现有上下文 Update an existing context



```console
$ docker context update \
    --description "some description" \
    --docker "host=tcp://myserver:2376,ca=~/ca-file,cert=~/cert-file,key=~/key-file" \
    my-context
```
