+++
title = "docker compose events"
date = 2024-10-23T14:54:43+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/compose/events/](https://docs.docker.com/reference/cli/docker/compose/events/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker compose events

| Description | Receive real time events from containers       |
| :---------- | ---------------------------------------------- |
| Usage       | `docker compose events [OPTIONS] [SERVICE...]` |

## Description

Stream container events for every container in the project.

​	流式传输项目中每个容器的事件。

With the `--json` flag, a json object is printed one per line with the format:

​	使用 `--json` 标志时，将以每行一个的格式打印 JSON 对象：



```json
{
    "time": "2015-11-20T18:01:03.615550",
    "type": "container",
    "action": "create",
    "id": "213cf7...5fc39a",
    "service": "web",
    "attributes": {
      "name": "application_web_1",
      "image": "alpine:edge"
    }
}
```

The events that can be received using this can be seen [here](https://docs.docker.com/reference/cli/docker/system/events/#object-types).

​	可以使用此功能接收的事件可以在 [这里](https://docs.docker.com/reference/cli/docker/system/events/#object-types) 查看。

## Options

| Option   | Default | Description                                                  |
| -------- | ------- | ------------------------------------------------------------ |
| `--json` |         | 将事件输出为 JSON 对象流 Output events as a stream of json objects |

​	
