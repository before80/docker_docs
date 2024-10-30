+++
title = "插件配置版本 1 的插件 V2"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/extend/config/](https://docs.docker.com/engine/extend/config/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Plugin Config Version 1 of Plugin V2 - 插件配置版本 1 的插件 V2

This document outlines the format of the V0 plugin configuration.

​	本文档概述了 V0 插件配置的格式。

Plugin configs describe the various constituents of a Docker engine plugin. Plugin configs can be serialized to JSON format with the following media types:

​	插件配置描述了 Docker 引擎插件的各种组成部分。插件配置可以使用以下媒体类型序列化为 JSON 格式：

| Config Type | Media Type 媒体类型                     |
| ----------- | --------------------------------------- |
| config      | `application/vnd.docker.plugin.v1+json` |

## 配置字段描述 Config Field Descriptions

Config provides the base accessible fields for working with V0 plugin format in the registry.

​	Config 提供了用于在注册表中使用 V0 插件格式的基本可访问字段。

- `description` string

  Description of the plugin 插件的描述

- `documentation` string

  Link to the documentation about the plugin 关于插件的文档链接

- `interface` PluginInterface

  Interface implemented by the plugins, struct consisting of the following fields: 插件实现的接口，结构包含以下字段：

  - `types` string array

    Types indicate what interface(s) the plugin currently implements.

    ​	Types 表示插件当前实现的接口类型。

    Supported types:

    - `docker.volumedriver/1.0`
    - `docker.networkdriver/1.0`
    - `docker.ipamdriver/1.0`
    - `docker.authz/1.0`
    - `docker.logdriver/1.0`
    - `docker.metricscollector/1.0`

  - `socket` string

    Socket is the name of the socket the engine should use to communicate with the plugins. the socket will be created in `/run/docker/plugins`. Socket 是引擎应使用的与插件通信的套接字名称，该套接字将创建在 `/run/docker/plugins` 中。

- `entrypoint` string array

  Entrypoint of the plugin, see [`ENTRYPOINT`](https://docs.docker.com/reference/dockerfile/#entrypoint)

  ​	插件的入口点，参见 [`ENTRYPOINT`](https://docs.docker.com/reference/dockerfile/#entrypoint)

- `workdir` string

  Working directory of the plugin, see [`WORKDIR`](https://docs.docker.com/reference/dockerfile/#workdir)

  ​	插件的工作目录，参见 [`WORKDIR`](https://docs.docker.com/reference/dockerfile/#workdir)

- `network` PluginNetwork

  Network of the plugin, struct consisting of the following fields:

  ​	插件的网络，结构包含以下字段：

  - `type` string

    Network type. 网络类型。

    Supported types:

    - `bridge`
    - `host`
    - `none`

- `mounts` PluginMount array

  Mount of the plugin, struct consisting of the following fields. See [`MOUNTS`](https://github.com/opencontainers/runtime-spec/blob/master/config.md#mounts).

  ​	插件的挂载，结构包含以下字段。参见 [`MOUNTS`](https://github.com/opencontainers/runtime-spec/blob/master/config.md#mounts)。

  - `name` string

    Name of the mount. 挂载的名称。

  - `description` string

    Description of the mount. 挂载的描述。

  - `source` string

    Source of the mount. 挂载的源路径。

  - `destination` string

    Destination of the mount. 挂载的目标路径。

  - `type` string

    Mount type. 挂载类型。

  - `options` string array

    Options of the mount. 挂载选项。

- `ipchost` Boolean

  Access to host ipc namespace.

  ​	访问主机 IPC 命名空间。

- `pidhost` Boolean

  Access to host PID namespace.

  ​	访问主机 PID 命名空间。

- `propagatedMount` string

  Path to be mounted as rshared, so that mounts under that path are visible to Docker. This is useful for volume plugins. This path will be bind-mounted outside of the plugin rootfs so it's contents are preserved on upgrade.

  ​	要作为 rshared 挂载的路径，使该路径下的挂载在 Docker 中可见。这对于卷插件非常有用。此路径将绑定挂载在插件根文件系统外部，以便在升级时保留其内容。

- `env` PluginEnv array

  Environment variables of the plugin, struct consisting of the following fields:

  ​	插件的环境变量，结构包含以下字段：

  - `name` string

    Name of the environment variable.

    ​	环境变量名称。

  - `description` string

    Description of the environment variable.

    ​	环境变量的描述。

  - `value` string

    Value of the environment variable.
    
    ​	环境变量的值。

- `args` PluginArgs

  Arguments of the plugin, struct consisting of the following fields:

  ​	插件的参数，结构包含以下字段：

  - `name` string

    Name of the arguments.

    ​	参数的名称。

  - `description` string

    Description of the arguments.

    ​	参数的描述。

  - `value` string array

    Values of the arguments.
    
    ​	参数的值。

- `linux` PluginLinux

  - `capabilities` string array

    Capabilities of the plugin (Linux only), see list [`here`](https://github.com/opencontainers/runc/blob/master/libcontainer/SPEC.md#security)

    ​	插件的功能（仅限 Linux），参见 [`here`](https://github.com/opencontainers/runc/blob/master/libcontainer/SPEC.md#security)

  - `allowAllDevices` Boolean

    If `/dev` is bind mounted from the host, and allowAllDevices is set to true, the plugin will have `rwm` access to all devices on the host.

    ​	如果 `/dev` 从主机绑定挂载，并且 allowAllDevices 设置为 true，则插件将对主机上的所有设备具有 `rwm` 访问权限。

  - `devices` PluginDevice array

    Device of the plugin, (Linux only), struct consisting of the following fields. See [`DEVICES`](https://github.com/opencontainers/runtime-spec/blob/master/config-linux.md#devices).

    ​	插件的设备（仅限 Linux），结构包含以下字段。参见 [`DEVICES`](https://github.com/opencontainers/runtime-spec/blob/master/config-linux.md#devices)。

    - `name` string

      Name of the device.

      ​	设备的名称。
  
    - `description` string
  
      Description of the device.
    
      ​	设备的描述。
    
    - `path` string
    
      Path of the device.
      
      ​	设备的路径。

## 示例配置 Example Config

The following example shows the 'tiborvass/sample-volume-plugin' plugin config.

​	以下示例显示了 'tiborvass/sample-volume-plugin' 插件配置。

```json
{
  "Args": {
    "Description": "",
    "Name": "",
    "Settable": null,
    "Value": null
  },
  "Description": "A sample volume plugin for Docker",
  "Documentation": "https://docs.docker.com/engine/extend/plugins/",
  "Entrypoint": [
    "/usr/bin/sample-volume-plugin",
    "/data"
  ],
  "Env": [
    {
      "Description": "",
      "Name": "DEBUG",
      "Settable": [
        "value"
      ],
      "Value": "0"
    }
  ],
  "Interface": {
    "Socket": "plugin.sock",
    "Types": [
      "docker.volumedriver/1.0"
    ]
  },
  "Linux": {
    "Capabilities": null,
    "AllowAllDevices": false,
    "Devices": null
  },
  "Mounts": null,
  "Network": {
    "Type": ""
  },
  "PropagatedMount": "/data",
  "User": {},
  "Workdir": ""
}
```
