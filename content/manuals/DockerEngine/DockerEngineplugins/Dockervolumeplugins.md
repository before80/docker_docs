+++
title = "Docker 卷插件"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/extend/plugins_volume/](https://docs.docker.com/engine/extend/plugins_volume/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker volume plugins - Docker 卷插件

Docker Engine volume plugins enable Engine deployments to be integrated with external storage systems such as Amazon EBS, and enable data volumes to persist beyond the lifetime of a single Docker host. See the [plugin documentation]({{< ref "/manuals/DockerEngine/DockerEngineplugins/UseDockerEngineplugins" >}}) for more information.

​	Docker 引擎卷插件使引擎部署能够与外部存储系统（如 Amazon EBS）集成，并使数据卷在单个 Docker 主机的生命周期之外保持持久性。更多信息请参阅 [插件文档]({{< ref "/manuals/DockerEngine/DockerEngineplugins/UseDockerEngineplugins" >}})。

## 变更日志 Changelog

### 1.13.0

- If used as part of the v2 plugin architecture, mountpoints that are part of paths returned by the plugin must be mounted under the directory specified by `PropagatedMount` in the plugin configuration ( [#26398](https://github.com/docker/docker/pull/26398))
  - 如果作为 v2 插件架构的一部分使用，插件返回路径中的挂载点必须位于插件配置中 `PropagatedMount` 指定的目录下 ([#26398](https://github.com/docker/docker/pull/26398))


### 1.12.0

- Add `Status` field to `VolumeDriver.Get` response ( [#21006](https://github.com/docker/docker/pull/21006#))

  - 在 `VolumeDriver.Get` 响应中添加 `Status` 字段 ([#21006](https://github.com/docker/docker/pull/21006#))

- Add `VolumeDriver.Capabilities` to get capabilities of the volume driver ( [#22077](https://github.com/docker/docker/pull/22077))

  - 添加 `VolumeDriver.Capabilities` 以获取卷驱动程序的功能 ([#22077](https://github.com/docker/docker/pull/22077))

  

### 1.10.0

- Add `VolumeDriver.Get` which gets the details about the volume ( [#16534](https://github.com/docker/docker/pull/16534))
  - 添加 `VolumeDriver.Get` 获取卷的详细信息 ([#16534](https://github.com/docker/docker/pull/16534))

- Add `VolumeDriver.List` which lists all volumes owned by the driver ( [#16534](https://github.com/docker/docker/pull/16534))
  - 添加 `VolumeDriver.List` 列出驱动程序拥有的所有卷 ([#16534](https://github.com/docker/docker/pull/16534))


### 1.8.0

- Initial support for volume driver plugins ( [#14659](https://github.com/docker/docker/pull/14659))
  - 增加对卷驱动程序插件的初步支持 ([#14659](https://github.com/docker/docker/pull/14659))


## Command-line changes

To give a container access to a volume, use the `--volume` and `--volume-driver` flags on the `docker container run` command. The `--volume` (or `-v`) flag accepts a volume name and path on the host, and the `--volume-driver` flag accepts a driver type.

​	使用 `docker container run` 命令的 `--volume` 和 `--volume-driver` 标志为容器提供卷访问。`--volume`（或 `-v`）标志接受卷名称和主机上的路径，`--volume-driver` 标志接受驱动程序类型。

```console
$ docker volume create --driver=flocker volumename

$ docker container run -it --volume volumename:/data busybox sh
```

### `--volume`

The `--volume` (or `-v`) flag takes a value that is in the format `<volume_name>:<mountpoint>`. The two parts of the value are separated by a colon (`:`) character.

​	`--volume`（或 `-v`）标志的值格式为 `<volume_name>:<mountpoint>`，两部分由冒号 (`:`) 分隔。

- The volume name is a human-readable name for the volume, and cannot begin with a `/` character. It is referred to as `volume_name` in the rest of this topic.
  - 卷名称是卷的可读名称，不能以 `/` 字符开头。在本主题的其余部分中称为 `volume_name`。

- The `Mountpoint` is the path on the host (v1) or in the plugin (v2) where the volume has been made available.
  - `Mountpoint` 是在主机 (v1) 或插件 (v2) 中该卷被挂载的位置路径。


### `volumedriver`

Specifying a `volumedriver` in conjunction with a `volumename` allows you to use plugins such as [Flocker](https://github.com/ScatterHQ/flocker) to manage volumes external to a single host, such as those on EBS.

​	将 `volumedriver` 与 `volumename` 一起指定，可以使用诸如 [Flocker](https://github.com/ScatterHQ/flocker) 之类的插件来管理跨单个主机的外部卷，例如 EBS 上的卷。

## 创建 VolumeDriver - Create a VolumeDriver

The container creation endpoint (`/containers/create`) accepts a `VolumeDriver` field of type `string` allowing to specify the name of the driver. If not specified, it defaults to `"local"` (the default driver for local volumes).

​	容器创建端点 (`/containers/create`) 接受一个 `VolumeDriver` 字段，该字段类型为 `string`，用于指定驱动程序的名称。如果未指定，则默认为 `"local"`（本地卷的默认驱动程序）。

## 卷插件协议 Volume plugin protocol

If a plugin registers itself as a `VolumeDriver` when activated, it must provide the Docker Daemon with writeable paths on the host filesystem. The Docker daemon provides these paths to containers to consume. The Docker daemon makes the volumes available by bind-mounting the provided paths into the containers.

​	如果插件在激活时注册为 `VolumeDriver`，则必须为 Docker 守护进程提供可写的主机文件系统路径。Docker 守护进程将这些路径提供给容器使用，并通过绑定挂载这些路径到容器内。

> **Note**
>
> Volume plugins should *not* write data to the `/var/lib/docker/` directory, including `/var/lib/docker/volumes`. The `/var/lib/docker/` directory is reserved for Docker.
>
> ​	卷插件不应向 `/var/lib/docker/` 目录写入数据，包括 `/var/lib/docker/volumes`。该目录保留供 Docker 使用。

### `/VolumeDriver.Create`

Request:

​	请求：

```json
{
    "Name": "volume_name",
    "Opts": {}
}
```

Instruct the plugin that the user wants to create a volume, given a user specified volume name. The plugin does not need to actually manifest the volume on the filesystem yet (until `Mount` is called). `Opts` is a map of driver specific options passed through from the user request.

​	指示插件创建一个卷，卷名称由用户指定。插件不需要立即在文件系统上体现该卷（直到调用 `Mount` 时才需要）。`Opts` 是一个包含用户请求传递的特定驱动选项的映射。

Response:

​	响应：

```json
{
    "Err": ""
}
```

Respond with a string error if an error occurred.

​	如果发生错误，则返回错误字符串。

### `/VolumeDriver.Remove`

Request:

​	请求：

```json
{
    "Name": "volume_name"
}
```

Delete the specified volume from disk. This request is issued when a user invokes `docker rm -v` to remove volumes associated with a container.

​	从磁盘中删除指定的卷。此请求在用户调用 `docker rm -v` 以删除与容器关联的卷时发出。

Response:

​	响应：

```json
{
    "Err": ""
}
```

Respond with a string error if an error occurred.

​	如果发生错误，则返回错误字符串。

### `/VolumeDriver.Mount`

Request:

​	请求：

```json
{
    "Name": "volume_name",
    "ID": "b87d7442095999a92b65b3d9691e697b61713829cc0ffd1bb72e4ccd51aa4d6c"
}
```

Docker requires the plugin to provide a volume, given a user specified volume name. `Mount` is called once per container start. If the same `volume_name` is requested more than once, the plugin may need to keep track of each new mount request and provision at the first mount request and deprovision at the last corresponding unmount request.

​	Docker 需要插件提供一个指定的卷名。`Mount` 在每次容器启动时调用一次。如果同一个 `volume_name` 多次请求，插件可能需要跟踪每次新的挂载请求，在第一次挂载请求时配置资源并在最后一个相应的卸载请求时取消配置。

`ID` is a unique ID for the caller that is requesting the mount.

​	`ID` 是请求挂载的调用方的唯一标识符。

Response:

​	响应：

- v1

  

  ```json
  {
      "Mountpoint": "/path/to/directory/on/host",
      "Err": ""
  }
  ```

- v2

  

  ```json
  {
      "Mountpoint": "/path/under/PropagatedMount",
      "Err": ""
  }
  ```

`Mountpoint` is the path on the host (v1) or in the plugin (v2) where the volume has been made available.

​	`Mountpoint` 是卷在主机 (v1) 或插件 (v2) 中被挂载的位置路径。

`Err` is either empty or contains an error string.

​	`Err` 要么为空，要么包含错误字符串。

### `/VolumeDriver.Path`

Request:

​	请求：

```json
{
    "Name": "volume_name"
}
```

Request the path to the volume with the given `volume_name`.

​	请求给定 `volume_name` 的卷路径。

Response:

​	响应：

- v1

  

  ```json
  {
      "Mountpoint": "/path/to/directory/on/host",
      "Err": ""
  }
  ```

- v2

  

  ```json
  {
      "Mountpoint": "/path/under/PropagatedMount",
      "Err": ""
  }
  ```

Respond with the path on the host (v1) or inside the plugin (v2) where the volume has been made available, and/or a string error if an error occurred.

​	响应主机 (v1) 或插件 (v2) 中该卷的路径，以及错误字符串（如果发生错误）。`Mountpoint` 是可选的，但如果未提供，插件可能会在稍后查询该路径。

### 

`Mountpoint` is optional. However, the plugin may be queried again later if one is not provided.

### `/VolumeDriver.Unmount`

Request:

​	请求：

```json
{
    "Name": "volume_name",
    "ID": "b87d7442095999a92b65b3d9691e697b61713829cc0ffd1bb72e4ccd51aa4d6c"
}
```

Docker is no longer using the named volume. `Unmount` is called once per container stop. Plugin may deduce that it is safe to deprovision the volume at this point.

​	Docker 不再使用指定的卷。每次容器停止时都会调用 `Unmount`。插件可以推断在此时取消配置该卷是安全的。

`ID` is a unique ID for the caller that is requesting the mount.

​	`ID` 是请求挂载的调用方的唯一标识符。

Response:

​	响应：

```json
{
    "Err": ""
}
```

Respond with a string error if an error occurred.

​	如果发生错误，则返回错误字符串。

### `/VolumeDriver.Get`

Request:

​	请求：

```json
{
    "Name": "volume_name"
}
```

Get info about `volume_name`.

​	获取 `volume_name` 的信息。

Response:

​	响应：

- v1

  

  ```json
  {
    "Volume": {
      "Name": "volume_name",
      "Mountpoint": "/path/to/directory/on/host",
      "Status": {}
    },
    "Err": ""
  }
  ```

- v2

  

  ```json
  {
    "Volume": {
      "Name": "volume_name",
      "Mountpoint": "/path/under/PropagatedMount",
      "Status": {}
    },
    "Err": ""
  }
  ```

Respond with a string error if an error occurred. `Mountpoint` and `Status` are optional.

### /VolumeDriver.List

Request:

​	请求：

```json
{}
```

Get the list of volumes registered with the plugin.

​	获取已注册的插件卷列表。

Response:

​	响应：

- v1

  

  ```json
  {
    "Volumes": [
      {
        "Name": "volume_name",
        "Mountpoint": "/path/to/directory/on/host"
      }
    ],
    "Err": ""
  }
  ```

- v2

  

  ```json
  {
    "Volumes": [
      {
        "Name": "volume_name",
        "Mountpoint": "/path/under/PropagatedMount"
      }
    ],
    "Err": ""
  }
  ```

Respond with a string error if an error occurred. `Mountpoint` is optional.

​	如果发生错误，则返回错误字符串。`Mountpoint` 是可选的。

### /VolumeDriver.Capabilities

Request:

​	请求：

```json
{}
```

Get the list of capabilities the driver supports.

​	获取驱动支持的能力列表。

The driver is not required to implement `Capabilities`. If it is not implemented, the default values are used.

​	驱动程序不是必须实现 `Capabilities`。如果未实现，将使用默认值。

Response:

​	响应：

```json
{
  "Capabilities": {
    "Scope": "global"
  }
}
```

Supported scopes are `global` and `local`. Any other value in `Scope` will be ignored, and `local` is used. `Scope` allows cluster managers to handle the volume in different ways. For instance, a scope of `global`, signals to the cluster manager that it only needs to create the volume once instead of on each Docker host. More capabilities may be added in the future.

​	支持的范围为 `global` 和 `local`。任何其他 `Scope` 值将被忽略，使用 `local`。`Scope` 允许集群管理器以不同的方式处理该卷。例如，`global` 范围表示集群管理器只需创建一次该卷，而不必在每个 Docker 主机上创建。未来可能会添加更多能力。
