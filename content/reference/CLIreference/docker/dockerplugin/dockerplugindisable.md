+++
title = "docker plugin disable"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/plugin/disable/](https://docs.docker.com/reference/cli/docker/plugin/disable/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker plugin disable

| Description | Disable a plugin                         |
| :---------- | ---------------------------------------- |
| Usage       | `docker plugin disable [OPTIONS] PLUGIN` |

## Description

Disables a plugin. The plugin must be installed before it can be disabled, see [`docker plugin install`]({{< ref "/reference/CLIreference/docker/dockerplugin/dockerplugininstall" >}}). Without the `-f` option, a plugin that has references (e.g., volumes, networks) cannot be disabled.

​	禁用插件。插件必须已安装后才能禁用，详见 [`docker plugin install`]({{< ref "/reference/CLIreference/docker/dockerplugin/dockerplugininstall" >}})。没有 `-f` 选项的情况下，具有引用（如卷、网络）的插件无法禁用。

## Options

| Option        | Default | Description                                                |
| ------------- | ------- | ---------------------------------------------------------- |
| `-f, --force` |         | 强制禁用一个活动插件 Force the disable of an active plugin |

## Examples

The following example shows that the `sample-volume-plugin` plugin is installed and enabled:

​	以下示例显示了 `sample-volume-plugin` 插件已安装并启用：



```console
$ docker plugin ls

ID            NAME                                    DESCRIPTION                ENABLED
69553ca1d123  tiborvass/sample-volume-plugin:latest   A test plugin for Docker   true
```

To disable the plugin, use the following command:

​	要禁用该插件，请使用以下命令：



```console
$ docker plugin disable tiborvass/sample-volume-plugin

tiborvass/sample-volume-plugin

$ docker plugin ls

ID            NAME                                    DESCRIPTION                ENABLED
69553ca1d123  tiborvass/sample-volume-plugin:latest   A test plugin for Docker   false
```
