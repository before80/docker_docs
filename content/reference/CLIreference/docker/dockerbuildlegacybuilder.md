+++
title = "docker build (legacy builder)"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/reference/cli/docker/build-legacy/](https://docs.docker.com/reference/cli/docker/build-legacy/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker build (legacy builder)

| Description | Build an image from a Dockerfile                         |
| :---------- | -------------------------------------------------------- |
| Usage       | `docker image build [OPTIONS] PATH | URL | -`            |
| Aliases     | `docker image build``docker build``docker builder build` |

## Description

> **Note**
>
> This page refers to the **legacy implementation** of `docker build`, using the legacy (pre-BuildKit) build backend. This configuration is only relevant if you're building Windows containers.
>
> ​	本页面描述了 **旧版 `docker build` 的实现**，使用的是旧版（BuildKit 之前的）构建后端。此配置仅在构建 Windows 容器时相关。
>
> For information about the default `docker build`, using Buildx, see [`docker buildx build`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxbuild">}}).
>
> ​	关于默认的 `docker build`（使用 Buildx），请参阅 [`docker buildx build`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxbuild">}})。

When building with legacy builder, images are created from a Dockerfile by running a sequence of [commits]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainercommit">}}). This process is inefficient and slow compared to using BuildKit, which is why this build strategy is deprecated for all use cases except for building Windows containers. It's still useful for building Windows containers because BuildKit doesn't yet have full feature parity for Windows.

​	在使用旧版构建器时，通过运行一系列 [提交操作]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainercommit">}})从 Dockerfile 创建镜像。相比使用 BuildKit，这种方式效率低且速度慢，因此除了用于构建 Windows 容器外，该构建策略已被弃用。对于 Windows 容器，这种构建方式仍然有用，因为 BuildKit 尚未完全支持 Windows 的所有功能。

Builds invoked with `docker build` use Buildx (and BuildKit) by default, unless:

​	除非符合以下条件，否则通过 `docker build` 命令启动的构建默认使用 Buildx（和 BuildKit）：

- You're running Docker Engine in Windows container mode
  - Docker 引擎在 Windows 容器模式下运行

- You explicitly opt out of using BuildKit by setting the environment variable `DOCKER_BUILDKIT=0`.
  - 通过设置环境变量 `DOCKER_BUILDKIT=0` 明确选择不使用 BuildKit


The descriptions on this page only covers information that's exclusive to the legacy builder, and cases where behavior in the legacy builder deviates from behavior in BuildKit. For information about features and flags that are common between the legacy builder and BuildKit, such as `--tag` and `--target`, refer to the documentation for [`docker buildx build`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxbuild" >}}).

​	本页面仅涵盖旧版构建器独有的信息，以及在旧版构建器与 BuildKit 的行为有所不同的情况。关于旧版构建器和 BuildKit 共有的功能和选项（如 `--tag` 和 `--target`），请参考 [`docker buildx build`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxbuild" >}}) 的文档。

### 使用旧版构建器的构建上下文 Build context with the legacy builder

The build context is the positional argument you pass when invoking the build command. In the following example, the context is `.`, meaning current the working directory.

​	构建上下文是调用构建命令时传递的定位参数。在以下示例中，上下文是 `.`，表示当前工作目录。



```console
$ docker build .
```

When using the legacy builder, the build context is sent over to the daemon in its entirety. With BuildKit, only the files you use in your builds are transmitted. The legacy builder doesn't calculate which files it needs beforehand. This means that for builds with a large context, context transfer can take a long time, even if you're only using a subset of the files included in the context.

​	在使用旧版构建器时，构建上下文将完整传送到守护进程。使用 BuildKit 时，只有构建中用到的文件会被传送。旧版构建器不会预先计算所需文件，因此对于较大的构建上下文，传输上下文可能会花费较长时间，即使实际构建只使用了其中的一小部分文件。

When using the legacy builder, it's therefore extra important that you carefully consider what files you include in the context you specify. Use a [`.dockerignore`](https://docs.docker.com/build/concepts/context/#dockerignore-files) file to exclude files and directories that you don't require in your build from being sent as part of the build context.

​	因此，在使用旧版构建器时，尤其需要仔细选择要包含在上下文中的文件。使用 [`.dockerignore`](https://docs.docker.com/build/concepts/context/#dockerignore-files) 文件来排除不需要的文件和目录，以免它们在构建上下文传送时被包含。

#### 访问构建上下文外的路径 Accessing paths outside the build context

The legacy builder will error out if you try to access files outside of the build context using relative paths in your Dockerfile.

​	如果在 Dockerfile 中使用相对路径访问构建上下文外的文件，旧版构建器将会报错。



```dockerfile
FROM alpine
COPY ../../some-dir .
```



```console
$ docker build .
...
Step 2/2 : COPY ../../some-dir .
COPY failed: forbidden path outside the build context: ../../some-dir ()
```

BuildKit on the other hand strips leading relative paths that traverse outside of the build context. Re-using the previous example, the path `COPY ../../some-dir .` evaluates to `COPY some-dir .` with BuildKit.

​	相比之下，BuildKit 会去除在构建上下文外的相对路径。以之前的示例为例，路径 `COPY ../../some-dir .` 在 BuildKit 中会被解析为 `COPY some-dir .`。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`--add-host`](https://docs.docker.com/reference/cli/docker/buildx/build/#add-host) |         | 添加自定义主机到 IP 的映射 (`host:ip`) Add a custom host-to-IP mapping (`host:ip`) |
| [`--build-arg`](https://docs.docker.com/reference/cli/docker/buildx/build/#build-arg) |         | 设置构建时的变量 Set build-time variables                    |
| `--cache-from`                                               |         | 作为缓存源的镜像 Images to consider as cache sources         |
| [`--cgroup-parent`](https://docs.docker.com/reference/cli/docker/buildx/build/#cgroup-parent) |         | 设置构建过程中 `RUN` 指令的父 cgroup Set the parent cgroup for the `RUN` instructions during build |
| `--compress`                                                 |         | 使用 gzip 压缩构建上下文 Compress the build context using gzip |
| `--cpu-period`                                               |         | 限制 CPU 完全公平调度（CFS）周期 Limit the CPU CFS (Completely Fair Scheduler) period |
| `--cpu-quota`                                                |         | 限制 CPU 完全公平调度（CFS）配额 Limit the CPU CFS (Completely Fair Scheduler) quota |
| `-c, --cpu-shares`                                           |         | CPU 共享（相对权重） CPU shares (relative weight)            |
| `--cpuset-cpus`                                              |         | 允许执行的 CPU（0-3, 0,1） CPUs in which to allow execution (0-3, 0,1) |
| `--cpuset-mems`                                              |         | 允许执行的 MEM（0-3, 0,1） MEMs in which to allow execution (0-3, 0,1) |
| `--disable-content-trust`                                    | `true`  | 跳过镜像验证 Skip image verification                         |
| [`-f, --file`](https://docs.docker.com/reference/cli/docker/buildx/build/#file) |         | Dockerfile 的名称（默认为 `PATH/Dockerfile`） Name of the Dockerfile (Default is `PATH/Dockerfile`) |
| `--force-rm`                                                 |         | 始终删除中间容器 Always remove intermediate containers       |
| `--iidfile`                                                  |         | 将镜像 ID 写入文件 Write the image ID to the file            |
| [`--isolation`](https://docs.docker.com/reference/cli/docker/build-legacy/#isolation) |         | 容器隔离技术 Container isolation technology                  |
| `--label`                                                    |         | 为镜像设置元数据 Set metadata for an image                   |
| `-m, --memory`                                               |         | 内存限制 Memory limit                                        |
| `--memory-swap`                                              |         | 交换内存限制，等于内存加交换内存：-1 表示无限制 Swap limit equal to memory plus swap: -1 to enable unlimited swap |
| [`--network`](https://docs.docker.com/reference/cli/docker/buildx/build/#network) |         | API 1.25+ 设置 `RUN` 指令的网络模式 API 1.25+ Set the networking mode for the RUN instructions during build |
| `--no-cache`                                                 |         | 构建镜像时不使用缓存 Do not use cache when building the image |
| `--platform`                                                 |         | API 1.38+ 设置平台（如果服务器支持多平台） API 1.38+ Set platform if server is multi-platform capable |
| `--pull`                                                     |         | 始终尝试拉取镜像的最新版本 Always attempt to pull a newer version of the image |
| `-q, --quiet`                                                |         | 抑制构建输出并在成功时仅显示镜像 ID Suppress the build output and print image ID on success |
| `--rm`                                                       | `true`  | 构建成功后删除中间容器 Remove intermediate containers after a successful build |
| [`--security-opt`](https://docs.docker.com/reference/cli/docker/build-legacy/#security-opt) |         | 安全选项 Security options                                    |
| `--shm-size`                                                 |         | `/dev/shm` 的大小 Size of `/dev/shm`                         |
| [`--squash`](https://docs.docker.com/reference/cli/docker/build-legacy/#squash) |         | API 1.25+ 实验性功能（守护进程）将新构建的层合并为一个新层 API 1.25+ experimental (daemon) Squash newly built layers into a single new layer |
| [`-t, --tag`](https://docs.docker.com/reference/cli/docker/buildx/build/#tag) |         | 镜像的名称和可选标签，格式为 `name:tag` Name and optionally a tag in the `name:tag` format |
| [`--target`](https://docs.docker.com/reference/cli/docker/buildx/build/#target) |         | 设置构建目标阶段 Set the target build stage to build.        |
| `--ulimit`                                                   |         | ulimit 选项 Ulimit options                                   |

## Examples

### 指定容器的隔离技术（`--isolation`） Specify isolation technology for container (`--isolation`)

This option is useful in situations where you are running Docker containers on Windows. The `--isolation=<value>` option sets a container's isolation technology. On Linux, the only supported is the `default` option which uses Linux namespaces. On Microsoft Windows, you can specify these values:

​	该选项在运行 Docker 容器于 Windows 时很有用。`--isolation=<value>` 选项设置容器的隔离技术。在 Linux 上，唯一支持的选项是 `default`，使用 Linux 命名空间。在 Windows 上，可以指定以下值：

| Value     | Description                                                  |
| --------- | ------------------------------------------------------------ |
| `default` | 使用 Docker 守护进程的 `--exec-opt` 指定的值。若未指定隔离技术，Windows 默认使用 `process` Use the value specified by the Docker daemon's `--exec-opt` . If the `daemon` does not specify an isolation technology, Microsoft Windows uses `process` as its default value. |
| `process` | 仅使用命名空间隔离 Namespace isolation only.                 |
| `hyperv`  | 基于 Hyper-V 的分区隔离 Hyper-V hypervisor partition-based isolation. |

Specifying the `--isolation` flag without a value is the same as setting `--isolation="default"`.

​	不指定值时，`--isolation` 相当于 `--isolation="default"`。

### 可选的安全选项（`--security-opt`） Optional security options (`--security-opt`)

This flag is only supported on a daemon running on Windows, and only supports the `credentialspec` option. The `credentialspec` must be in the format `file://spec.txt` or `registry://keyname`.

​	此标志仅在 Windows 上运行的守护进程上支持，并仅支持 `credentialspec` 选项。`credentialspec` 必须为 `file://spec.txt` 或 `registry://keyname` 格式。

### 合并镜像的层（`--squash`）（实验性） Squash an image's layers (`--squash`) (experimental)

#### Overview

> **Note**
>
> The `--squash` option is an experimental feature, and should not be considered stable.
>
> ​	`--squash` 选项为实验性功能，可能不稳定。

Once the image is built, this flag squashes the new layers into a new image with a single new layer. Squashing doesn't destroy any existing image, rather it creates a new image with the content of the squashed layers. This effectively makes it look like all `Dockerfile` commands were created with a single layer. The `--squash` flag preserves the build cache.

​	镜像构建完成后，此标志将新层合并为具有单层的新镜像。合并不会破坏现有镜像，而是创建一个新镜像，包含合并的层内容，效果类似于 `Dockerfile` 中的所有命令都创建在单层内。`--squash` 标志保留构建缓存。

Squashing layers can be beneficial if your Dockerfile produces multiple layers modifying the same files. For example, files created in one step and removed in another step. For other use-cases, squashing images may actually have a negative impact on performance. When pulling an image consisting of multiple layers, the daemon can pull layers in parallel and allows sharing layers between images (saving space).

​	合并层对于修改相同文件的多层 `Dockerfile` 可能有用。例如，一步创建文件，另一层删除它们。对于其他情况，合并层可能会对性能产生负面影响。包含多个层的镜像在拉取时可以并行处理，并在镜像间共享层（节省空间）。

For most use cases, multi-stage builds are a better alternative, as they give more fine-grained control over your build, and can take advantage of future optimizations in the builder. Refer to the [Multi-stage builds]({{< ref "/manuals/DockerBuild/Building/Multi-stage" >}}) section for more information.

​	大多数情况下，多阶段构建是更好的选择，因为它提供了更细致的控制，并可利用构建器的未来优化。有关更多信息，请参阅 [多阶段构建]({{< ref "/manuals/DockerBuild/Building/Multi-stage" >}}) 部分。

#### 已知限制 Known limitations

The `--squash` option has a number of known limitations:

​	`--squash` 选项有一些已知限制：

- When squashing layers, the resulting image can't take advantage of layer sharing with other images, and may use significantly more space. Sharing the base image is still supported.

  - 合并层后，结果镜像无法与其他镜像共享层，可能占用更多空间。基础镜像的共享仍然支持。

- When using this option you may see significantly more space used due to storing two copies of the image, one for the build cache with all the cache layers intact, and one for the squashed version.

  - 使用此选项时，可能会占用大量空间，因为会存储构建缓存和合并版本的两个镜像副本。

- While squashing layers may produce smaller images, it may have a negative impact on performance, as a single layer takes longer to extract, and you can't parallelize downloading a single layer.

  - 合并层可能生成更小的镜像，但可能对性能产生负面影响，因为单层提取时间更长，无法并行下载单层。

- When attempting to squash an image that doesn't make changes to the filesystem (for example, the Dockerfile only contains `ENV` instructions), the squash step will fail (see [issue #33823](https://github.com/moby/moby/issues/33823)).

  - 合并不修改文件系统的镜像（例如 `Dockerfile` 中仅包含 `ENV` 指令）时，合并步骤会失败（参见 [问题 #33823](https://github.com/moby/moby/issues/33823)）。

  

#### Prerequisites

The example on this page is using experimental mode in Docker 23.03.

​	本示例在 Docker 23.03 的实验模式下使用。

You can enable experimental mode by using the `--experimental` flag when starting the Docker daemon or setting `experimental: true` in the `daemon.json` configuration file.

​	可以在启动 Docker 守护进程时使用 `--experimental` 标志启用实验模式，或在 `daemon.json` 配置文件中设置 `experimental: true`。

By default, experimental mode is disabled. To see the current configuration of the Docker daemon, use the `docker version` command and check the `Experimental` line in the `Engine` section:

​	默认情况下，实验模式是禁用的。要查看 Docker 守护进程的当前配置，请使用 `docker version` 命令并检查 `Engine` 部分中的 `Experimental` 行：



```console
Client: Docker Engine - Community
 Version:           23.0.3
 API version:       1.42
 Go version:        go1.19.7
 Git commit:        3e7cbfd
 Built:             Tue Apr  4 22:05:41 2023
 OS/Arch:           darwin/amd64
 Context:           default

Server: Docker Engine - Community
 Engine:
  Version:          23.0.3
  API version:      1.42 (minimum version 1.12)
  Go version:       go1.19.7
  Git commit:       59118bf
  Built:            Tue Apr  4 22:05:41 2023
  OS/Arch:          linux/amd64
  Experimental:     true
 [...]
```

#### 使用 `--squash` 标志构建镜像 Build an image with the `--squash` flag

The following is an example of a build with the `--squash` flag. Below is the `Dockerfile`:

​	以下是带有 `--squash` 标志的构建示例。下面是 `Dockerfile`：



```dockerfile
FROM busybox
RUN echo hello > /hello
RUN echo world >> /hello
RUN touch remove_me /remove_me
ENV HELLO=world
RUN rm /remove_me
```

Next, build an image named `test` using the `--squash` flag.

​	接下来，使用 `--squash` 标志构建名为 `test` 的镜像。



```console
$ docker build --squash -t test .
```

After the build completes, the history looks like the below. The history could show that a layer's name is `<missing>`, and there is a new layer with COMMENT `merge`.

​	构建完成后，历史记录如下。历史记录可能会显示某个层的名称为 `<missing>`，并且有一个新的层带有 `merge` 的注释。



```console
$ docker history test

IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
4e10cb5b4cac        3 seconds ago                                                       12 B                merge sha256:88a7b0112a41826885df0e7072698006ee8f621c6ab99fca7fe9151d7b599702 to sha256:47bcc53f74dc94b1920f0b34f6036096526296767650f223433fe65c35f149eb
<missing>           5 minutes ago       /bin/sh -c rm /remove_me                        0 B
<missing>           5 minutes ago       /bin/sh -c #(nop) ENV HELLO=world               0 B
<missing>           5 minutes ago       /bin/sh -c touch remove_me /remove_me           0 B
<missing>           5 minutes ago       /bin/sh -c echo world >> /hello                 0 B
<missing>           6 minutes ago       /bin/sh -c echo hello > /hello                  0 B
<missing>           7 weeks ago         /bin/sh -c #(nop) CMD ["sh"]                    0 B
<missing>           7 weeks ago         /bin/sh -c #(nop) ADD file:47ca6e777c36a4cfff   1.113 MB
```

Test the image, check for `/remove_me` being gone, make sure `hello\nworld` is in `/hello`, make sure the `HELLO` environment variable's value is `world`.

​	测试镜像，确认 `/remove_me` 被移除，确保 `/hello` 包含 `hello\nworld`，并确认环境变量 `HELLO` 的值为 `world`。
