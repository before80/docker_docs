+++
title = "docker context inspect"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/context/inspect/](https://docs.docker.com/reference/cli/docker/context/inspect/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker context inspect

| Description | Display detailed information on one or more contexts      |
| :---------- | --------------------------------------------------------- |
| Usage       | `docker context inspect [OPTIONS] [CONTEXT] [CONTEXT...]` |

## Description

Inspects one or more contexts.

​	检查一个或多个上下文。

## Options

| Option         | Default | Description                                                  |
| -------------- | ------- | ------------------------------------------------------------ |
| `-f, --format` |         | 使用自定义模板格式化输出：'json'：以 JSON 格式打印输出；'TEMPLATE'：使用指定的 Go 模板格式化输出。有关使用模板格式化输出的详细信息，请参阅 https://docs.docker.com/go/formatting/。  Format output using a custom template: 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |

## Examples

### 按名称检查上下文 Inspect a context by name



```console
$ docker context inspect "local+aks"

[
  {
    "Name": "local+aks",
    "Metadata": {
      "Description": "Local Docker Engine",
      "StackOrchestrator": "swarm"
    },
    "Endpoints": {
      "docker": {
        "Host": "npipe:////./pipe/docker_engine",
        "SkipTLSVerify": false
      }
    },
    "TLSMaterial": {},
    "Storage": {
      "MetadataPath": "C:\\Users\\simon\\.docker\\contexts\\meta\\cb6d08c0a1bfa5fe6f012e61a442788c00bed93f509141daff05f620fc54ddee",
      "TLSPath": "C:\\Users\\simon\\.docker\\contexts\\tls\\cb6d08c0a1bfa5fe6f012e61a442788c00bed93f509141daff05f620fc54ddee"
    }
  }
]
```
