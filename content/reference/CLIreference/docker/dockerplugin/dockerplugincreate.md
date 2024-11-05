+++
title = "docker plugin create"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/plugin/create/](https://docs.docker.com/reference/cli/docker/plugin/create/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker plugin create

| Description | Create a plugin from a rootfs and configuration. Plugin data directory must contain config.json and rootfs directory. |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker plugin create [OPTIONS] PLUGIN PLUGIN-DATA-DIR`      |

## Description

Creates a plugin. Before creating the plugin, prepare the plugin's root filesystem as well as the [config.json]({{< ref "/manuals/DockerEngine/DockerEngineplugins/PluginConfigVersion1ofPluginV2" >}}).

​	创建插件。在创建插件之前，准备好插件的根文件系统以及 [config.json]({{< ref "/manuals/DockerEngine/DockerEngineplugins/PluginConfigVersion1ofPluginV2" >}})。

## Options

| Option       | Default | Description                                          |
| ------------ | ------- | ---------------------------------------------------- |
| `--compress` |         | 使用 gzip 压缩上下文 Compress the context using gzip |

## Examples

The following example shows how to create a sample `plugin`.

​	以下示例展示了如何创建一个示例 `plugin`。



```console
$ ls -ls /home/pluginDir

total 4
4 -rw-r--r--  1 root root 431 Nov  7 01:40 config.json
0 drwxr-xr-x 19 root root 420 Nov  7 01:40 rootfs

$ docker plugin create plugin /home/pluginDir

plugin

$ docker plugin ls

ID              NAME            DESCRIPTION                  ENABLED
672d8144ec02    plugin:latest   A sample plugin for Docker   false
```

The plugin can subsequently be enabled for local use or pushed to the public registry.

​	插件随后可以启用以供本地使用或推送到公共注册表。
