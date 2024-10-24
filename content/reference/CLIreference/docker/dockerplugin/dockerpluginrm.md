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

## Options

| Option        | Default | Description                           |
| ------------- | ------- | ------------------------------------- |
| `-f, --force` |         | Force the removal of an active plugin |

## Examples

The following example disables and removes the `sample-volume-plugin:latest` plugin:



```console
$ docker plugin disable tiborvass/sample-volume-plugin

tiborvass/sample-volume-plugin

$ docker plugin rm tiborvass/sample-volume-plugin:latest

tiborvass/sample-volume-plugin
```
