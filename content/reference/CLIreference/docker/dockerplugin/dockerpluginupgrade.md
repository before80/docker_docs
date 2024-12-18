+++
title = "docker plugin upgrade"
date = 2024-10-23T14:54:43+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/plugin/upgrade/](https://docs.docker.com/reference/cli/docker/plugin/upgrade/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker plugin upgrade

| Description | Upgrade an existing plugin                        |
| :---------- | ------------------------------------------------- |
| Usage       | `docker plugin upgrade [OPTIONS] PLUGIN [REMOTE]` |

## Description

Upgrades an existing plugin to the specified remote plugin image. If no remote is specified, Docker will re-pull the current image and use the updated version. All existing references to the plugin will continue to work. The plugin must be disabled before running the upgrade.

​	升级现有插件至指定的远程插件镜像。如果未指定远程，Docker 将重新拉取当前镜像并使用更新版本。插件的所有现有引用将继续生效。在运行升级之前，插件必须禁用。

## Options

| Option                    | Default | Description                                                  |
| ------------------------- | ------- | ------------------------------------------------------------ |
| `--disable-content-trust` | `true`  | 跳过镜像验证 Skip image verification                         |
| `--grant-all-permissions` |         | 授予运行插件所需的所有权限 Grant all permissions necessary to run the plugin |
| `--skip-remote-check`     |         | 不检查指定的远程插件是否与现有插件镜像匹配 Do not check if specified remote plugin matches existing plugin image |

## Examples

The following example installs `vieus/sshfs` plugin, uses it to create and use a volume, then upgrades the plugin.

​	以下示例安装 `vieus/sshfs` 插件，使用该插件创建并使用卷，然后升级该插件。

```console
$ docker plugin install vieux/sshfs DEBUG=1

Plugin "vieux/sshfs:next" is requesting the following privileges:
 - network: [host]
 - device: [/dev/fuse]
 - capabilities: [CAP_SYS_ADMIN]
Do you grant the above permissions? [y/N] y
vieux/sshfs:next

$ docker volume create -d vieux/sshfs:next -o sshcmd=root@1.2.3.4:/tmp/shared -o password=XXX sshvolume

sshvolume

$ docker run -it -v sshvolume:/data alpine sh -c "touch /data/hello"

$ docker plugin disable -f vieux/sshfs:next

viex/sshfs:next

# Here docker volume ls doesn't show 'sshfsvolume', since the plugin is disabled
$ docker volume ls

DRIVER              VOLUME NAME

$ docker plugin upgrade vieux/sshfs:next vieux/sshfs:next

Plugin "vieux/sshfs:next" is requesting the following privileges:
 - network: [host]
 - device: [/dev/fuse]
 - capabilities: [CAP_SYS_ADMIN]
Do you grant the above permissions? [y/N] y
Upgrade plugin vieux/sshfs:next to vieux/sshfs:next

$ docker plugin enable vieux/sshfs:next

viex/sshfs:next

$ docker volume ls

DRIVER              VOLUME NAME
viuex/sshfs:next    sshvolume

$ docker run -it -v sshvolume:/data alpine sh -c "ls /data"

hello
```
