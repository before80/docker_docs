+++
title = "Docker 容器构建驱动"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/builders/drivers/docker-container/](https://docs.docker.com/build/builders/drivers/docker-container/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker container build driver -  Docker 容器构建驱动

The Docker container driver allows creation of a managed and customizable BuildKit environment in a dedicated Docker container.

​	Docker 容器驱动允许在专用 Docker 容器中创建一个可管理且可定制的 BuildKit 环境。

Using the Docker container driver has a couple of advantages over the default Docker driver. For example:

​	与默认的 Docker 驱动相比，使用 Docker 容器驱动有以下优势：

- Specify custom BuildKit versions to use.
  - 可以指定自定义的 BuildKit 版本。

- Build multi-arch images, see [QEMU](https://docs.docker.com/build/builders/drivers/docker-container/#qemu)
  - 构建多架构镜像，参见 [QEMU](https://docs.docker.com/build/builders/drivers/docker-container/#qemu)

- Advanced options for [cache import and export]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends" >}})
  - 提供高级选项用于[缓存导入和导出]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends" >}})


## 概述 Synopsis

Run the following command to create a new builder, named `container`, that uses the Docker container driver:

​	运行以下命令，使用 Docker 容器驱动创建一个名为 `container` 的新构建器：

```console
$ docker buildx create \
  --name container \
  --driver=docker-container \
  --driver-opt=[key=value,...]
container
```

The following table describes the available driver-specific options that you can pass to `--driver-opt`:

​	以下表格描述了可以传递给 `--driver-opt` 的特定驱动选项：

| Parameter        | Type    | Default          | Description                                                  |
| ---------------- | ------- | ---------------- | ------------------------------------------------------------ |
| `image`          | String  |                  | 设置容器使用的 BuildKit 镜像。 Sets the BuildKit image to use for the container. |
| `memory`         | String  |                  | 设置容器可以使用的内存量。 Sets the amount of memory the container can use. |
| `memory-swap`    | String  |                  | 设置容器的内存交换限制。 Sets the memory swap limit for the container. |
| `cpu-quota`      | String  |                  | 对容器施加 CPU CFS 配额。 Imposes a CPU CFS quota on the container. |
| `cpu-period`     | String  |                  | 设置容器的 CPU CFS 调度周期。 Sets the CPU CFS scheduler period for the container. |
| `cpu-shares`     | String  |                  | 配置容器的 CPU 共享（相对权重）。 Configures CPU shares (relative weight) of the container. |
| `cpuset-cpus`    | String  |                  | 限制容器可以使用的 CPU 核心。 Limits the set of CPU cores the container can use. |
| `cpuset-mems`    | String  |                  | 限制容器可以使用的 CPU 内存节点。 Limits the set of CPU memory nodes the container can use. |
| `default-load`   | Boolean | `false`          | 自动将镜像加载到 Docker 引擎镜像存储中。 Automatically load images to the Docker Engine image store. |
| `network`        | String  |                  | 设置容器的网络模式。 Sets the network mode for the container. |
| `cgroup-parent`  | String  | `/docker/buildx` | 如果 Docker 使用 "cgroupfs" 驱动，则设置容器的 cgroup 父级。 Sets the cgroup parent of the container if Docker is using the "cgroupfs" driver. |
| `restart-policy` | String  | `unless-stopped` | 设置容器的[重启策略](https://docs.docker.com/engine/containers/start-containers-automatically/#use-a-restart-policy)。 Sets the container's [restart policy](https://docs.docker.com/engine/containers/start-containers-automatically/#use-a-restart-policy). |
| `env.<key>`      | String  |                  | 在容器中将环境变量 `key` 设置为指定的 `value`。 Sets the environment variable `key` to the specified `value` in the container. |

Before you configure the resource limits for the container, read about [configuring runtime resource constraints for containers]({{< ref "/manuals/DockerEngine/Containers/Resourceconstraints" >}}).

​	在配置容器的资源限制之前，请阅读[配置容器的运行时资源约束]({{< ref "/manuals/DockerEngine/Containers/Resourceconstraints" >}})。

## Usage

When you run a build, Buildx pulls the specified `image` (by default, [`moby/buildkit`](https://hub.docker.com/r/moby/buildkit)). When the container has started, Buildx submits the build submitted to the containerized build server.

​	当您运行构建时，Buildx 会拉取指定的 `image`（默认为 [`moby/buildkit`](https://hub.docker.com/r/moby/buildkit)）。容器启动后，Buildx 会将构建任务提交到容器化的构建服务器。





```console
$ docker buildx build -t <image> --builder=container .
WARNING: No output specified with docker-container driver. Build result will only remain in the build cache. To push result image into registry use --push or to load image into docker use --load
#1 [internal] booting buildkit
#1 pulling image moby/buildkit:buildx-stable-1
#1 pulling image moby/buildkit:buildx-stable-1 1.9s done
#1 creating container buildx_buildkit_container0
#1 creating container buildx_buildkit_container0 0.5s done
#1 DONE 2.4s
...
```

## 缓存持久化 Cache persistence

The `docker-container` driver supports cache persistence, as it stores all the BuildKit state and related cache into a dedicated Docker volume.

​	`docker-container` 驱动支持缓存持久化，因为它将所有 BuildKit 状态和相关缓存存储到专用的 Docker 卷中。

To persist the `docker-container` driver's cache, even after recreating the driver using `docker buildx rm` and `docker buildx create`, you can destroy the builder using the `--keep-state` flag:

​	即使在使用 `docker buildx rm` 和 `docker buildx create` 重新创建驱动后，也可以通过 `--keep-state` 标志来保持 `docker-container` 驱动的缓存：

For example, to create a builder named `container` and then remove it while persisting state:

​	例如，创建一个名为 `container` 的构建器，并在保留状态的情况下将其移除：

```console
# setup a builder
# 设置构建器
$ docker buildx create --name=container --driver=docker-container --use --bootstrap
container
$ docker buildx ls
NAME/NODE       DRIVER/ENDPOINT              STATUS   BUILDKIT PLATFORMS
container *     docker-container
  container0    desktop-linux                running  v0.10.5  linux/amd64
$ docker volume ls
DRIVER    VOLUME NAME
local     buildx_buildkit_container0_state

# remove the builder while persisting state
# 在保留状态的情况下移除构建器
$ docker buildx rm --keep-state container
$ docker volume ls
DRIVER    VOLUME NAME
local     buildx_buildkit_container0_state

# the newly created driver with the same name will have all the state of the previous one!
# 重新创建同名的驱动器将保留上一个状态！
$ docker buildx create --name=container --driver=docker-container --use --bootstrap
container
```

## QEMU

The `docker-container` driver supports using [QEMU](https://www.qemu.org/) (user mode) to build non-native platforms. Use the `--platform` flag to specify which architectures that you want to build for.

​	`docker-container` 驱动支持使用 [QEMU](https://www.qemu.org/)（用户模式）构建非原生平台。使用 `--platform` 标志指定要构建的架构。

For example, to build a Linux image for `amd64` and `arm64`:

​	例如，构建一个适用于 `amd64` 和 `arm64` 的 Linux 镜像：

```console
$ docker buildx build \
  --builder=container \
  --platform=linux/amd64,linux/arm64 \
  -t <registry>/<image> \
  --push .
```

> **Note**
>
> 
>
> Emulation with QEMU can be much slower than native builds, especially for compute-heavy tasks like compilation and compression or decompression.
>
> ​	使用 QEMU 进行仿真比原生构建要慢得多，尤其是对于编译和压缩/解压缩等计算密集型任务。

## 自定义网络 Custom network

You can customize the network that the builder container uses. This is useful if you need to use a specific network for your builds.

​	您可以自定义构建器容器使用的网络。这在构建需要特定网络时非常有用。

For example, let's [create a network]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkcreate" >}}) named `foonet`:

​	例如，创建一个名为 `foonet` 的[网络]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkcreate" >}})：

```console
$ docker network create foonet
```

Now create a [`docker-container` builder]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxcreate" >}}) that will use this network:

​	现在创建一个使用该网络的 [`docker-container` 构建器]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxcreate" >}})：





```console
$ docker buildx create --use \
  --name mybuilder \
  --driver docker-container \
  --driver-opt "network=foonet"
```

Boot and [inspect `mybuilder`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxinspect" >}}):

​	启动并[检查 `mybuilder`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxinspect" >}})：





```console
$ docker buildx inspect --bootstrap
```

[Inspect the builder container]({{< ref "/reference/CLIreference/docker/dockerinspect" >}}) and see what network is being used:

​	[检查构建器容器]({{< ref "/reference/CLIreference/docker/dockerinspect" >}})以查看使用的网络：





```console
$ docker inspect buildx_buildkit_mybuilder0 --format={{.NetworkSettings.Networks}}
map[foonet:0xc00018c0c0]
```

## 进一步阅读 Further reading

For more information on the Docker container driver, see the [buildx reference](https://docs.docker.com/reference/cli/docker/buildx/create/#driver).

​	有关 Docker 容器驱动的更多信息，请参阅 [buildx 参考](https://docs.docker.com/reference/cli/docker/buildx/create/#driver)。
