+++
title = "docker plugin enable"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/plugin/enable/](https://docs.docker.com/reference/cli/docker/plugin/enable/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker plugin enable

| Description | Enable a plugin                         |
| :---------- | --------------------------------------- |
| Usage       | `docker plugin enable [OPTIONS] PLUGIN` |

## Description

Enables a plugin. The plugin must be installed before it can be enabled, see [`docker plugin install`]({{< ref "/reference/CLIreference/docker/dockerplugin/dockerplugininstall" >}}).

​	启用插件。插件必须已安装后才能启用，详见 [`docker plugin install`]({{< ref "/reference/CLIreference/docker/dockerplugin/dockerplugininstall" >}})。

## Options

| Option      | Default | Description                                                |
| ----------- | ------- | ---------------------------------------------------------- |
| `--timeout` | `30`    | HTTP 客户端超时时间（秒） HTTP client timeout (in seconds) |

## Examples

The following example shows that the `sample-volume-plugin` plugin is installed, but disabled:

​	以下示例显示 `sample-volume-plugin` 插件已安装，但处于禁用状态：



```console
$ docker plugin ls

ID            NAME                                    DESCRIPTION                ENABLED
69553ca1d123  tiborvass/sample-volume-plugin:latest   A test plugin for Docker   false
```

To enable the plugin, use the following command:

​	要启用该插件，请使用以下命令：

```console
$ docker plugin enable tiborvass/sample-volume-plugin

tiborvass/sample-volume-plugin

$ docker plugin ls

ID            NAME                                    DESCRIPTION                ENABLED
69553ca1d123  tiborvass/sample-volume-plugin:latest   A test plugin for Docker   true
```
