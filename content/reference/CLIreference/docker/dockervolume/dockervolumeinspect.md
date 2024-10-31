+++
title = "docker volume inspect"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/volume/inspect/](https://docs.docker.com/reference/cli/docker/volume/inspect/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker volume inspect

| Description | Display detailed information on one or more volumes  |
| :---------- | ---------------------------------------------------- |
| Usage       | `docker volume inspect [OPTIONS] VOLUME [VOLUME...]` |

## Description

Returns information about a volume. By default, this command renders all results in a JSON array. You can specify an alternate format to execute a given template for each result. Go's [text/template](https://pkg.go.dev/text/template) package describes all the details of the format.

​	返回有关卷的信息。默认情况下，该命令以 JSON 数组格式输出所有结果。您可以指定替代格式来根据给定模板执行输出。Go 的 [text/template](https://pkg.go.dev/text/template) 包描述了格式的所有详细信息。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --format`](https://docs.docker.com/reference/cli/docker/volume/inspect/#format) |         | 使用自定义模板格式化输出：'json'：以 JSON 格式输出 'TEMPLATE'：使用给定的 Go 模板格式输出。请参阅 https://docs.docker.com/go/formatting/ 了解更多关于模板格式化的信息  Format output using a custom template: 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |

## Examples



```console
$ docker volume create myvolume

myvolume
```

Use the `docker volume inspect` comment to inspect the configuration of the volume:

​	使用 `docker volume inspect` 命令检查卷的配置：



```console
$ docker volume inspect myvolume
```

The output is in JSON format, for example:

​	输出为 JSON 格式，例如：



```json
[
  {
    "CreatedAt": "2020-04-19T11:00:21Z",
    "Driver": "local",
    "Labels": {},
    "Mountpoint": "/var/lib/docker/volumes/8140a838303144125b4f54653b47ede0486282c623c3551fbc7f390cdc3e9cf5/_data",
    "Name": "myvolume",
    "Options": {},
    "Scope": "local"
  }
]
```

### Format the output (--format)

Use the `--format` flag to format the output using a Go template, for example, to prt the `Mountpoint` property:

​	使用 `--format` 标志使用 Go 模板格式化输出，例如打印 `Mountpoint` 属性：



```console
$ docker volume inspect --format '{{ .Mountpoint }}' myvolume

/var/lib/docker/volumes/myvolume/_data
```
