+++
title = "docker plugin rm"
date = 2024-10-23T14:54:43+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/plugin/rm/](https://docs.docker.com/reference/cli/docker/plugin/rm/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker plugin rm

| Description | Remove one or more plugins                      |
| :---------- | ----------------------------------------------- |
| Usage       | `docker plugin rm [OPTIONS] PLUGIN [PLUGIN...]` |
| Aliases     | `docker plugin remove`                          |

## Description

Removes a plugin. You cannot remove a plugin if it is enabled, you must disable a plugin using the [`docker plugin disable`]({{< ref "/reference/CLIreference/docker/dockerplugin/dockerplugindisable" >}}) before removing it, or use `--force`. Use of `--force` is not recommended, since it can affect functioning of running containers using the plugin.

​	移除插件。如果插件已启用，则无法移除，必须先使用 [`docker plugin disable`]({{< ref "/reference/CLIreference/docker/dockerplugin/dockerplugindisable" >}}) 禁用插件，然后再移除它，或者使用 `--force`。不推荐使用 `--force`，因为这可能影响正在使用该插件的运行容器。

## Options

| Option        | Default | Description                                                |
| ------------- | ------- | ---------------------------------------------------------- |
| `-f, --force` |         | 强制移除一个活动插件 Force the removal of an active plugin |

## Examples

The following example disables and removes the `sample-volume-plugin:latest` plugin:

​	以下示例禁用并移除 `sample-volume-plugin:latest` 插件：

```console
$ docker plugin disable tiborvass/sample-volume-plugin

tiborvass/sample-volume-plugin

$ docker plugin rm tiborvass/sample-volume-plugin:latest

tiborvass/sample-volume-plugin
```
