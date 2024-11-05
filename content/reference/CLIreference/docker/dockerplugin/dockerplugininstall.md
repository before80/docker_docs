+++
title = "docker plugin install"
date = 2024-10-23T14:54:43+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/plugin/install/](https://docs.docker.com/reference/cli/docker/plugin/install/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker plugin install

| Description | Install a plugin                                        |
| :---------- | ------------------------------------------------------- |
| Usage       | `docker plugin install [OPTIONS] PLUGIN [KEY=VALUE...]` |

## Description

Installs and enables a plugin. Docker looks first for the plugin on your Docker host. If the plugin does not exist locally, then the plugin is pulled from the registry. Note that the minimum required registry version to distribute plugins is 2.3.0.

​	安装并启用插件。Docker 首先会在本地主机上查找插件，如果本地不存在该插件，则会从注册表中拉取该插件。请注意，分发插件所需的最低注册表版本是 2.3.0。

## Options

| Option                    | Default | Description                                                  |
| ------------------------- | ------- | ------------------------------------------------------------ |
| `--alias`                 |         | 插件的本地名称 Local name for plugin                         |
| `--disable`               |         | 在安装时不启用该插件 Do not enable the plugin on install     |
| `--disable-content-trust` | `true`  | 跳过镜像验证 Skip image verification                         |
| `--grant-all-permissions` |         | 授予运行插件所需的所有权限 Grant all permissions necessary to run the plugin |

## Examples

The following example installs `vieus/sshfs` plugin and [sets]({{< ref "/reference/CLIreference/docker/dockerplugin/dockerpluginset" >}}) its `DEBUG` environment variable to `1`. To install, `pull` the plugin from Docker Hub and prompt the user to accept the list of privileges that the plugin needs, set the plugin's parameters and enable the plugin.

​	以下示例安装 `vieus/sshfs` 插件并[设置]({{< ref "/reference/CLIreference/docker/dockerplugin/dockerpluginset" >}})其 `DEBUG` 环境变量为 `1`。要安装该插件，从 Docker Hub 拉取插件，并提示用户接受插件所需的权限列表，设置插件参数并启用该插件。



```console
$ docker plugin install vieux/sshfs DEBUG=1

Plugin "vieux/sshfs" is requesting the following privileges:
 - network: [host]
 - device: [/dev/fuse]
 - capabilities: [CAP_SYS_ADMIN]
Do you grant the above permissions? [y/N] y
vieux/sshfs
```

After the plugin is installed, it appears in the list of plugins:

​	安装插件后，它将显示在插件列表中：

```console
$ docker plugin ls

ID             NAME                  DESCRIPTION                ENABLED
69553ca1d123   vieux/sshfs:latest    sshFS plugin for Docker    true
```
