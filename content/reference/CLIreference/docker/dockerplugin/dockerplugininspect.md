+++
title = "docker plugin inspect"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/plugin/inspect/](https://docs.docker.com/reference/cli/docker/plugin/inspect/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker plugin inspect

| Description | Display detailed information on one or more plugins  |
| :---------- | ---------------------------------------------------- |
| Usage       | `docker plugin inspect [OPTIONS] PLUGIN [PLUGIN...]` |

## Description

Returns information about a plugin. By default, this command renders all results in a JSON array.

​	返回有关插件的信息。默认情况下，该命令以 JSON 数组形式呈现所有结果。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-f, --format`](https://docs.docker.com/reference/cli/docker/plugin/inspect/#format) |         | 使用自定义模板格式化输出：'json'：以 JSON 格式输出 'TEMPLATE'：使用给定的 Go 模板格式化输出。有关使用模板格式化输出的更多信息，请参阅 https://docs.docker.com/go/formatting/  Format output using a custom template: 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |

## Examples

### Inspect a plugin

The following example inspects the `tiborvass/sample-volume-plugin` plugin:

​	以下示例检查 `tiborvass/sample-volume-plugin` 插件：



```console
$ docker plugin inspect tiborvass/sample-volume-plugin:latest
```

Output is in JSON format (output below is formatted for readability):

​	输出为 JSON 格式（以下输出为可读性而格式化）：

```json
{
  "Id": "8c74c978c434745c3ade82f1bc0acf38d04990eaf494fa507c16d9f1daa99c21",
  "Name": "tiborvass/sample-volume-plugin:latest",
  "PluginReference": "tiborvas/sample-volume-plugin:latest",
  "Enabled": true,
  "Config": {
    "Mounts": [
      {
        "Name": "",
        "Description": "",
        "Settable": null,
        "Source": "/data",
        "Destination": "/data",
        "Type": "bind",
        "Options": [
          "shared",
          "rbind"
        ]
      },
      {
        "Name": "",
        "Description": "",
        "Settable": null,
        "Source": null,
        "Destination": "/foobar",
        "Type": "tmpfs",
        "Options": null
      }
    ],
    "Env": [
      "DEBUG=1"
    ],
    "Args": null,
    "Devices": null
  },
  "Manifest": {
    "ManifestVersion": "v0",
    "Description": "A test plugin for Docker",
    "Documentation": "/engine/extend/plugins/",
    "Interface": {
      "Types": [
        "docker.volumedriver/1.0"
      ],
      "Socket": "plugins.sock"
    },
    "Entrypoint": [
      "plugin-sample-volume-plugin",
      "/data"
    ],
    "Workdir": "",
    "User": {
    },
    "Network": {
      "Type": "host"
    },
    "Capabilities": null,
    "Mounts": [
      {
        "Name": "",
        "Description": "",
        "Settable": null,
        "Source": "/data",
        "Destination": "/data",
        "Type": "bind",
        "Options": [
          "shared",
          "rbind"
        ]
      },
      {
        "Name": "",
        "Description": "",
        "Settable": null,
        "Source": null,
        "Destination": "/foobar",
        "Type": "tmpfs",
        "Options": null
      }
    ],
    "Devices": [
      {
        "Name": "device",
        "Description": "a host device to mount",
        "Settable": null,
        "Path": "/dev/cpu_dma_latency"
      }
    ],
    "Env": [
      {
        "Name": "DEBUG",
        "Description": "If set, prints debug messages",
        "Settable": null,
        "Value": "1"
      }
    ],
    "Args": {
      "Name": "args",
      "Description": "command line arguments",
      "Settable": null,
      "Value": [

      ]
    }
  }
}
```

### Format the output (`--format`)



```console
$ docker plugin inspect -f '{{.Id}}' tiborvass/sample-volume-plugin:latest

8c74c978c434745c3ade82f1bc0acf38d04990eaf494fa507c16d9f1daa99c21
```
