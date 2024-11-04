+++
title = "docker buildx create"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/buildx/create/](https://docs.docker.com/reference/cli/docker/buildx/create/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx create

| Description | Create a new builder instance                       |
| :---------- | --------------------------------------------------- |
| Usage       | `docker buildx create [OPTIONS] [CONTEXT|ENDPOINT]` |

## Description

Create makes a new builder instance pointing to a Docker context or endpoint, where context is the name of a context from `docker context ls` and endpoint is the address for Docker socket (eg. `DOCKER_HOST` value).

​	`create` 会创建一个新的构建器实例，指向 Docker 上下文或端点，其中上下文是从 `docker context ls` 获取的上下文名称，而端点是 Docker 套接字的地址（例如 `DOCKER_HOST` 的值）。

By default, the current Docker configuration is used for determining the context/endpoint value.

​	默认情况下，将使用当前 Docker 配置来确定上下文/端点的值。

Builder instances are isolated environments where builds can be invoked. All Docker contexts also get the default builder instance.

​	构建器实例是独立的环境，可以在其中调用构建操作。所有 Docker 上下文也会获得默认的构建器实例。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`--append`](https://docs.docker.com/reference/cli/docker/buildx/create/#append) |         | 将节点附加到构建器而不是更改它 Append a node to builder instead of changing it |
| `--bootstrap`                                                |         | 创建后引导构建器 Boot builder after creation                 |
| [`--buildkitd-config`](https://docs.docker.com/reference/cli/docker/buildx/create/#buildkitd-config) |         | BuildKit 守护进程配置文件 BuildKit daemon config file        |
| [`--buildkitd-flags`](https://docs.docker.com/reference/cli/docker/buildx/create/#buildkitd-flags) |         | BuildKit 守护进程标志 BuildKit daemon flags                  |
| [`--driver`](https://docs.docker.com/reference/cli/docker/buildx/create/#driver) |         | 使用的驱动程序（可用：`docker-container`，`kubernetes`，`remote`） Driver to use (available: `docker-container`, `kubernetes`, `remote`) |
| [`--driver-opt`](https://docs.docker.com/reference/cli/docker/buildx/create/#driver-opt) |         | 驱动的选项 Options for the driver                            |
| [`--leave`](https://docs.docker.com/reference/cli/docker/buildx/create/#leave) |         | 从构建器中移除节点而不是更改它 Remove a node from builder instead of changing it |
| [`--name`](https://docs.docker.com/reference/cli/docker/buildx/create/#name) |         | 构建器实例名称 Builder instance name                         |
| [`--node`](https://docs.docker.com/reference/cli/docker/buildx/create/#node) |         | 使用给定名称创建/修改节点 Create/modify node with given name |
| [`--platform`](https://docs.docker.com/reference/cli/docker/buildx/create/#platform) |         | 当前节点的固定平台 Fixed platforms for current node          |
| [`--use`](https://docs.docker.com/reference/cli/docker/buildx/create/#use) |         | 设置当前构建器实例 Set the current builder instance          |

## Examples

### 向现有构建器附加一个新节点（`--append`） Append a new node to an existing builder (`--append`)

The `--append` flag changes the action of the command to append a new node to an existing builder specified by `--name`. Buildx will choose an appropriate node for a build based on the platforms it supports.

​	`--append` 标志会将命令的操作更改为向指定 `--name` 的现有构建器附加一个新节点。Buildx 将根据节点支持的平台选择合适的节点进行构建。



```console
$ docker buildx create mycontext1
eager_beaver

$ docker buildx create --name eager_beaver --append mycontext2
eager_beaver
```

### 指定 BuildKit 守护进程的配置文件（`--buildkitd-config`） Specify a configuration file for the BuildKit daemon (`--buildkitd-config`)



```text
--buildkitd-config FILE
```

Specifies the configuration file for the BuildKit daemon to use. The configuration can be overridden by [`--buildkitd-flags`](https://docs.docker.com/reference/cli/docker/buildx/create/#buildkitd-flags). See an [example BuildKit daemon configuration file](https://github.com/moby/buildkit/blob/master/docs/buildkitd.toml.md).

​	指定 BuildKit 守护进程使用的配置文件。此配置可被 [`--buildkitd-flags`](https://docs.docker.com/reference/cli/docker/buildx/create/#buildkitd-flags) 覆盖。参见 [示例 BuildKit 守护进程配置文件](https://github.com/moby/buildkit/blob/master/docs/buildkitd.toml.md)。

If you don't specify a configuration file, Buildx looks for one by default in:

​	如果未指定配置文件，Buildx 将默认从以下路径查找：

- `$BUILDX_CONFIG/buildkitd.default.toml`

- `$DOCKER_CONFIG/buildx/buildkitd.default.toml`
- `~/.docker/buildx/buildkitd.default.toml`

Note that if you create a `docker-container` builder and have specified certificates for registries in the `buildkitd.toml` configuration, the files will be copied into the container under `/etc/buildkit/certs` and configuration will be updated to reflect that.

​	请注意，如果创建了 `docker-container` 构建器并在 `buildkitd.toml` 配置中指定了注册表的证书，这些文件将被复制到容器中的 `/etc/buildkit/certs`，并更新配置以反映这一更改。

### 指定 BuildKit 守护进程的选项（`--buildkitd-flags`） Specify options for the BuildKit daemon (`--buildkitd-flags`)



```text
--buildkitd-flags FLAGS
```

Adds flags when starting the BuildKit daemon. They take precedence over the configuration file specified by [`--buildkitd-config`](https://docs.docker.com/reference/cli/docker/buildx/create/#buildkitd-config). See `buildkitd --help` for the available flags.

​	在启动 BuildKit 守护进程时添加标志。这些标志优先于通过 [`--buildkitd-config`](https://docs.docker.com/reference/cli/docker/buildx/create/#buildkitd-config) 指定的配置文件。有关可用标志，请参见 `buildkitd --help`。



```text
--buildkitd-flags '--debug --debugaddr 0.0.0.0:6666'
```

#### BuildKit 守护进程的网络模式 BuildKit daemon network mode

You can specify the network mode for the BuildKit daemon with either the configuration file specified by [`--buildkitd-config`](https://docs.docker.com/reference/cli/docker/buildx/create/#buildkitd-config) using the `worker.oci.networkMode` option or `--oci-worker-net` flag here. The default value is `auto` and can be one of `bridge`, `cni`, `host`:

​	可以通过 `worker.oci.networkMode` 选项或 `--oci-worker-net` 标志在配置文件中为 BuildKit 守护进程指定网络模式。默认值是 `auto`，可以是 `bridge`、`cni` 或 `host`。



```text
--buildkitd-flags '--oci-worker-net bridge'
```

> **Note**
>
> Network mode "bridge" is supported since BuildKit v0.13 and will become the default in next v0.14.
>
> ​	网络模式 "bridge" 自 BuildKit v0.13 支持，并将在 v0.14 中成为默认值。

### 设置构建器驱动程序（`--driver`） Set the builder driver to use (`--driver`)



```text
--driver DRIVER
```

Sets the builder driver to be used. A driver is a configuration of a BuildKit backend. Buildx supports the following drivers:

​	设置要使用的构建器驱动程序。驱动程序是 BuildKit 后端的配置。Buildx 支持以下驱动：

- `docker` (default)

- `docker-container`
- `kubernetes`
- `remote`

For more information about build drivers, see [here]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}}).

​	有关构建驱动的更多信息，请参见[此处]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}})。

#### `docker` driver

Uses the builder that is built into the Docker daemon. With this driver, the [`--load`](https://docs.docker.com/reference/cli/docker/buildx/build/#load) flag is implied by default on `buildx build`. However, building multi-platform images or exporting cache is not currently supported.

​	使用内置于 Docker 守护进程中的构建器。使用此驱动时，在 `buildx build` 上默认隐含 `--load` 标志，但不支持构建多平台镜像或导出缓存。

#### 

#### `docker-container` driver

Uses a BuildKit container that will be spawned via Docker. With this driver, both building multi-platform images and exporting cache are supported.

​	使用将通过 Docker 启动的 BuildKit 容器。此驱动支持构建多平台镜像和导出缓存。

Unlike `docker` driver, built images will not automatically appear in `docker images` and [`build --load`](https://docs.docker.com/reference/cli/docker/buildx/build/#load) needs to be used to achieve that.

​	与 `docker` 驱动不同，构建的镜像不会自动出现在 `docker images` 中，需使用 [`build --load`](https://docs.docker.com/reference/cli/docker/buildx/build/#load) 来实现。

#### `kubernetes` driver

Uses Kubernetes pods. With this driver, you can spin up pods with defined BuildKit container image to build your images.

​	使用 Kubernetes pod。此驱动允许启动包含已定义 BuildKit 容器镜像的 pod 来构建镜像。

Unlike `docker` driver, built images will not automatically appear in `docker images` and [`build --load`](https://docs.docker.com/reference/cli/docker/buildx/build/#load) needs to be used to achieve that.

​	与 `docker` 驱动不同，构建的镜像不会自动出现在 `docker images` 中，需使用 [`build --load`](https://docs.docker.com/reference/cli/docker/buildx/build/#load) 来实现。

#### `remote` driver

Uses a remote instance of BuildKit daemon over an arbitrary connection. With this driver, you manually create and manage instances of buildkit yourself, and configure buildx to point at it.

​	使用通过任意连接的远程 BuildKit 守护进程实例。此驱动需要手动创建和管理 BuildKit 实例，并配置 buildx 指向它。

Unlike `docker` driver, built images will not automatically appear in `docker images` and [`build --load`](https://docs.docker.com/reference/cli/docker/buildx/build/#load) needs to be used to achieve that.

​	与 `docker` 驱动不同，构建的镜像不会自动出现在 `docker images` 中，需使用 [`build --load`](https://docs.docker.com/reference/cli/docker/buildx/build/#load) 来实现。

### 设置额外的驱动特定选项（`--driver-opt`） Set additional driver-specific options (`--driver-opt`)



```text
--driver-opt OPTIONS
```

Passes additional driver-specific options. For information about available driver options, refer to the detailed documentation for the specific driver:

​	传递额外的驱动特定选项。关于可用的驱动选项信息，请参考特定驱动的详细文档：

- [`docker` driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockerdriver" >}})
- [`docker-container` driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockercontainerbuilddriver" >}})
- [`kubernetes` driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Kubernetesdriver" >}})
- [`remote` driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Remotedriver" >}})

### 从构建器中移除节点（`--leave`）Remove a node from a builder (`--leave`)

The `--leave` flag changes the action of the command to remove a node from a builder. The builder needs to be specified with `--name` and node that is removed is set with `--node`.

​	`--leave` 标志会将命令的操作更改为从构建器中移除节点。构建器需通过 `--name` 指定，并通过 `--node` 设置要移除的节点。



```console
$ docker buildx create --name mybuilder --node mybuilder0 --leave
```

### 指定构建器的名称（`--name`） Specify the name of the builder (`--name`)



```text
--name NAME
```

The `--name` flag specifies the name of the builder to be created or modified. If none is specified, one will be automatically generated.

​	`--name` 标志指定要创建或修改的构建器的名称。如果未指定，将自动生成一个名称。

### 指定节点的名称（`--node`） Specify the name of the node (`--node`)



```text
--node NODE
```

The `--node` flag specifies the name of the node to be created or modified. If you don't specify a name, the node name defaults to the name of the builder it belongs to, with an index number suffix.

​	`--node` 标志指定要创建或修改的节点的名称。如果不指定名称，节点名称默认为它所属的构建器名称，并带有索引编号后缀。

### 设置节点支持的平台（`--platform`） Set the platforms supported by the node (`--platform`)



```text
--platform PLATFORMS
```

The `--platform` flag sets the platforms supported by the node. It expects a comma-separated list of platforms of the form OS/architecture/variant. The node will also automatically detect the platforms it supports, but manual values take priority over the detected ones and can be used when multiple nodes support building for the same platform.

​	`--platform` 标志设置节点支持的平台。它接受以逗号分隔的平台列表，格式为操作系统/架构/变体。节点会自动检测支持的平台，但手动值优先于检测到的值，适用于多个节点支持相同平台的情况。



```console
$ docker buildx create --platform linux/amd64
$ docker buildx create --platform linux/arm64,linux/arm/v7
```

### 自动切换到新创建的构建器（`--use`） Automatically switch to the newly created builder (`--use`)

The `--use` flag automatically switches the current builder to the newly created one. Equivalent to running `docker buildx use $(docker buildx create ...)`.

​	`--use` 标志会自动将当前构建器切换为新创建的构建器。等同于运行 `docker buildx use $(docker buildx create ...)`。
