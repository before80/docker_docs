+++
title = "docker buildx build"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/buildx/build/](https://docs.docker.com/reference/cli/docker/buildx/build/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx build

| Description | Start a build                                                |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker buildx build [OPTIONS] PATH | URL | -`               |
| Aliases     | `docker build``docker builder build``docker image build``docker buildx b` |

## Description

The `docker buildx build` command starts a build using BuildKit.

​	`docker buildx build` 命令使用 BuildKit 启动构建。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`--add-host`](https://docs.docker.com/reference/cli/docker/buildx/build/#add-host) |         | 添加自定义的主机到 IP 映射（格式：`host:ip`） Add a custom host-to-IP mapping (format: `host:ip`) |
| [`--allow`](https://docs.docker.com/reference/cli/docker/buildx/build/#allow) |         | 允许额外的特权（例如，`network.host`, `security.insecure`） Allow extra privileged entitlement (e.g., `network.host`, `security.insecure`) |
| [`--annotation`](https://docs.docker.com/reference/cli/docker/buildx/build/#annotation) |         | 向镜像添加注解 Add annotation to the image                   |
| [`--attest`](https://docs.docker.com/reference/cli/docker/buildx/build/#attest) |         | 声明参数（格式：`type=sbom,generator=image`） Attestation parameters (format: `type=sbom,generator=image`) |
| [`--build-arg`](https://docs.docker.com/reference/cli/docker/buildx/build/#build-arg) |         | 设置构建时的变量 Set build-time variables                    |
| [`--build-context`](https://docs.docker.com/reference/cli/docker/buildx/build/#build-context) |         | 额外的构建上下文（例如，`name=path`） Additional build contexts (e.g., name=path) |
| [`--cache-from`](https://docs.docker.com/reference/cli/docker/buildx/build/#cache-from) |         | 外部缓存源（例如，`user/app:cache`，`type=local,src=path/to/dir`） External cache sources (e.g., `user/app:cache`, `type=local,src=path/to/dir`) |
| [`--cache-to`](https://docs.docker.com/reference/cli/docker/buildx/build/#cache-to) |         | 缓存导出目标（例如，`user/app:cache`，`type=local,dest=path/to/dir`） Cache export destinations (e.g., `user/app:cache`, `type=local,dest=path/to/dir`) |
| [`--call`](https://docs.docker.com/reference/cli/docker/buildx/build/#call) | `build` | 设置用于评估构建的方法（`check`，`outline`，`targets`）Set method for evaluating build (`check`, `outline`, `targets`) |
| [`--cgroup-parent`](https://docs.docker.com/reference/cli/docker/buildx/build/#cgroup-parent) |         | 为构建期间的 `RUN` 指令设置父 cgroup - Set the parent cgroup for the `RUN` instructions during build |
| xxxxxxxxxx6 1$ docker buildx ls --format "{{.Builder.Name}}: {{range .Builder.Nodes}}\n  {{.Name}}: {{.Endpoint}}{{end}}"2elated_tesla:3  elated_tesla0: unix:///var/run/docker.sock4  elated_tesla1: ssh://ubuntu@1.2.3.45default: docker6  default: defaultconsole |         | `--call=check` 的简写 Shorthand for `--call=check`           |
| `--detach`                                                   |         | 实验性（CLI）从构建中分离（仅支持在 Linux 上运行） experimental (CLI) Detach buildx server (supported only on linux) |
| [`-f, --file`](https://docs.docker.com/reference/cli/docker/buildx/build/#file) |         | Dockerfile 文件名称（默认：`PATH/Dockerfile`） Name of the Dockerfile (default: `PATH/Dockerfile`) |
| `--iidfile`                                                  |         | 将镜像 ID 写入文件 Write the image ID to a file              |
| `--label`                                                    |         | 为镜像设置元数据 Set metadata for an image                   |
| [`--load`](https://docs.docker.com/reference/cli/docker/buildx/build/#load) |         | `--output=type=docker` 的简写 Shorthand for `--output=type=docker` |
| [`--metadata-file`](https://docs.docker.com/reference/cli/docker/buildx/build/#metadata-file) |         | 将构建结果的元数据写入文件 Write build result metadata to a file |
| [`--network`](https://docs.docker.com/reference/cli/docker/buildx/build/#network) |         | 设置构建期间 `RUN` 指令的网络模式 Set the networking mode for the `RUN` instructions during build |
| `--no-cache`                                                 |         | 构建镜像时不使用缓存 Do not use cache when building the image |
| [`--no-cache-filter`](https://docs.docker.com/reference/cli/docker/buildx/build/#no-cache-filter) |         | 不缓存指定的阶段 Do not cache specified stages               |
| [`-o, --output`](https://docs.docker.com/reference/cli/docker/buildx/build/#output) |         | 输出目标（格式：`type=local,dest=path`） Output destination (format: `type=local,dest=path`) |
| [`--platform`](https://docs.docker.com/reference/cli/docker/buildx/build/#platform) |         | 设置构建的目标平台 Set target platform for build             |
| [`--progress`](https://docs.docker.com/reference/cli/docker/buildx/build/#progress) | `auto`  | 设置进度输出类型（`auto`，`plain`，`tty`，`rawjson`） Set type of progress output (`auto`, `plain`, `tty`, `rawjson`). Use plain to show container output |
| [`--provenance`](https://docs.docker.com/reference/cli/docker/buildx/build/#provenance) |         | `--attest=type=provenance` 的简写 Shorthand for `--attest=type=provenance` |
| `--pull`                                                     |         | 始终尝试拉取所有引用的镜像 Always attempt to pull all referenced images |
| [`--push`](https://docs.docker.com/reference/cli/docker/buildx/build/#push) |         | `--output=type=registry` 的简写 Shorthand for `--output=type=registry` |
| `-q, --quiet`                                                |         | 抑制构建输出，成功时仅打印镜像 ID Suppress the build output and print image ID on success |
| `--root`                                                     |         | 实验性（CLI）指定要连接的服务器的根目录 experimental (CLI) Specify root directory of server to connect |
| [`--sbom`](https://docs.docker.com/reference/cli/docker/buildx/build/#sbom) |         | `--attest=type=sbom` 的简写 Shorthand for `--attest=type=sbom` |
| [`--secret`](https://docs.docker.com/reference/cli/docker/buildx/build/#secret) |         | 向构建暴露的秘密（格式：`id=mysecret[,src=/local/secret]`） Secret to expose to the build (format: `id=mysecret[,src=/local/secret]`) |
| `--server-config`                                            |         | 实验性（CLI）指定 buildx 服务器配置文件（仅在启动新服务器时使用） experimental (CLI) Specify buildx server config file (used only when launching new server) |
| [`--shm-size`](https://docs.docker.com/reference/cli/docker/buildx/build/#shm-size) |         | 构建容器的共享内存大小 Shared memory size for build containers |
| [`--ssh`](https://docs.docker.com/reference/cli/docker/buildx/build/#ssh) |         | 要向构建暴露的 SSH agent 套接字或密钥（格式：`default SSH agent socket or keys to expose to the build (format: `default |
| [`-t, --tag`](https://docs.docker.com/reference/cli/docker/buildx/build/#tag) |         | 名称和可选标签（格式：`name:tag`） Name and optionally a tag (format: `name:tag`) |
| [`--target`](https://docs.docker.com/reference/cli/docker/buildx/build/#target) |         | 设置构建阶段的目标 Set the target build stage to build       |
| [`--ulimit`](https://docs.docker.com/reference/cli/docker/buildx/build/#ulimit) |         | Ulimit 选项 Ulimit options                                   |

## Examples

### 向容器主机文件添加条目（`--add-host`）Add entries to container hosts file (`--add-host`)

You can add other hosts into a build container's `/etc/hosts` file by using one or more `--add-host` flags. This example adds static addresses for hosts named `my-hostname` and `my_hostname_v6`:

​	可以使用一个或多个 `--add-host` 标志将其他主机添加到构建容器的 `/etc/hosts` 文件中。以下示例为名为 `my-hostname` 和 `my_hostname_v6` 的主机添加静态地址：

```console
$ docker buildx build --add-host my_hostname=8.8.8.8 --add-host my_hostname_v6=2001:4860:4860::8888 .
```

If you need your build to connect to services running on the host, you can use the special `host-gateway` value for `--add-host`. In the following example, build containers resolve `host.docker.internal` to the host's gateway IP.

​	如果需要构建连接到主机上运行的服务，可以为 `--add-host` 使用特殊的 `host-gateway` 值。在以下示例中，构建容器将 `host.docker.internal` 解析为主机的网关 IP。

```console
$ docker buildx build --add-host host.docker.internal=host-gateway .
```

You can wrap an IPv6 address in square brackets. `=` and `:` are both valid separators. Both formats in the following example are valid:

​	可以用方括号括起 IPv6 地址。`=` 和 `:` 都是有效的分隔符。以下示例中的两种格式都是有效的：

```console
$ docker buildx build --add-host my-hostname:10.180.0.1 --add-host my-hostname_v6=[2001:4860:4860::8888] .
```

### 创建注解（`--annotation`） Create annotations (`--annotation`)



```text
--annotation="key=value"
--annotation="[type:]key=value"
```

Add OCI annotations to the image index, manifest, or descriptor. The following example adds the `foo=bar` annotation to the image manifests:

​	向镜像索引、清单或描述符添加 OCI 注解。以下示例将 `foo=bar` 注解添加到镜像清单中：

```console
$ docker buildx build -t TAG --annotation "foo=bar" --push .
```

You can optionally add a type prefix to specify the level of the annotation. By default, the image manifest is annotated. The following example adds the `foo=bar` annotation the image index instead of the manifests:

​	可以选择添加类型前缀来指定注解的级别。默认情况下，镜像清单被注解。以下示例将 `foo=bar` 注解添加到镜像索引，而不是清单：

```console
$ docker buildx build -t TAG --annotation "index:foo=bar" --push .
```

You can specify multiple types, separated by a comma (,) to add the annotation to multiple image components. The following example adds the `foo=bar` annotation to image index, descriptors, manifests:

​	可以用逗号（,）分隔多个类型，向多个镜像组件添加注解。以下示例将 `foo=bar` 注解添加到镜像索引、描述符、清单中：

```console
$ docker buildx build -t TAG --annotation "index,manifest,manifest-descriptor:foo=bar" --push .
```

You can also specify a platform qualifier in square brackets (`[os/arch]`) in the type prefix, to apply the annotation to a subset of manifests with the matching platform. The following example adds the `foo=bar` annotation only to the manifest with the `linux/amd64` platform:

​	还可以在类型前缀中使用方括号（`[os/arch]`）指定平台限定符，以便将注解应用于具有匹配平台的清单子集。以下示例仅将 `foo=bar` 注解添加到具有 `linux/amd64` 平台的清单：

```console
$ docker buildx build -t TAG --annotation "manifest[linux/amd64]:foo=bar" --push .
```

Wildcards are not supported in the platform qualifier; you can't specify a type prefix like `manifest[linux/*]` to add annotations only to manifests which has `linux` as the OS platform.

​	平台限定符中不支持通配符；不能指定类型前缀 `manifest[linux/*]` 仅向操作系统平台为 `linux` 的清单添加注解。

For more information about annotations, see [Annotations](https://docs.docker.com/build/building/annotations/).

​	有关注解的更多信息，请参见 [Annotations](https://docs.docker.com/build/building/annotations/)。

### 创建声明（`--attest`） Create attestations (`--attest`)



```text
--attest=type=sbom,...
--attest=type=provenance,...
```

Create [image attestations]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations" >}}). BuildKit currently supports:

​	创建[镜像声明]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations" >}})。BuildKit 当前支持以下类型：

- `sbom` - Software Bill of Materials. `sbom` - 软件物料清单 (SBOM)

  Use `--attest=type=sbom` to generate an SBOM for an image at build-time. Alternatively, you can use the [`--sbom` shorthand](https://docs.docker.com/reference/cli/docker/buildx/build/#sbom).

  ​	使用 `--attest=type=sbom` 在构建时为镜像生成 SBOM。可以使用 [`--sbom` 简写](https://docs.docker.com/reference/cli/docker/buildx/build/#sbom)。

  For more information, see [here]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations/SBOMattestations" >}}).

  ​	详情请参见 [此处]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations/SBOMattestations" >}})。

- `provenance` - SLSA Provenance `provenance` - SLSA 溯源

  Use `--attest=type=provenance` to generate provenance for an image at build-time. Alternatively, you can use the [`--provenance` shorthand](https://docs.docker.com/reference/cli/docker/buildx/build/#provenance).

  ​	使用 `--attest=type=provenance` 在构建时为镜像生成溯源声明。可以使用 [`--provenance` 简写](https://docs.docker.com/reference/cli/docker/buildx/build/#provenance)。

  By default, a minimal provenance attestation will be created for the build result, which will only be attached for images pushed to registries.
  
  ​	默认情况下，将为构建结果创建最小溯源声明，该声明仅会附加到推送到注册表的镜像。
  
  For more information, see [here]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations/Provenanceattestations" >}}).
  
  ​	详情请参见 [此处]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations/Provenanceattestations" >}})。

### 允许额外特权（`--allow`） Allow extra privileged entitlement (`--allow`)



```text
--allow=ENTITLEMENT
```

Allow extra privileged entitlement. List of entitlements:

​	允许额外特权授权。授权列表包括：

- `network.host` - Allows executions with host networking.
  - `network.host` - 允许使用主机网络。

- `security.insecure` - Allows executions without sandbox. See [related Dockerfile extensions](https://docs.docker.com/reference/dockerfile/#run---security).
  - `security.insecure` - 允许在没有沙箱的情况下执行。参见 [相关 Dockerfile 扩展](https://docs.docker.com/reference/dockerfile/#run---security)。


For entitlements to be enabled, the BuildKit daemon also needs to allow them with `--allow-insecure-entitlement` (see [`create --buildkitd-flags`](https://docs.docker.com/reference/cli/docker/buildx/create/#buildkitd-flags)).

​	启用特权需要 BuildKit 守护进程使用 `--allow-insecure-entitlement` 选项（请参见 [`create --buildkitd-flags`](https://docs.docker.com/reference/cli/docker/buildx/create/#buildkitd-flags)）。

```console
$ docker buildx create --use --name insecure-builder --buildkitd-flags '--allow-insecure-entitlement security.insecure'
$ docker buildx build --allow security.insecure .
```

### 设置构建时变量（`--build-arg`） Set build-time variables (`--build-arg`)

You can use `ENV` instructions in a Dockerfile to define variable values. These values persist in the built image. Often persistence isn't what you want. Users want to specify variables differently depending on which host they build an image on.

​	可以在 Dockerfile 中使用 `ENV` 指令定义变量值。这些值会在构建的镜像中保留。但在某些情况下，用户可能想根据构建的主机动态设置变量值。

A good example is `http_proxy` or source versions for pulling intermediate files. The `ARG` instruction lets Dockerfile authors define values that users can set at build-time using the `--build-arg` flag:

​	例如，可以使用 `ARG` 指令定义构建时变量，用户可以通过 `--build-arg` 标志在构建时指定这些变量：

```console
$ docker buildx build --build-arg HTTP_PROXY=http://10.20.30.2:1234 --build-arg FTP_PROXY=http://40.50.60.5:4567 .
```

This flag allows you to pass the build-time variables that are accessed like regular environment variables in the `RUN` instruction of the Dockerfile. These values don't persist in the intermediate or final images like `ENV` values do. You must add `--build-arg` for each build argument.

​	此标志允许您传递构建时变量，这些变量在 Dockerfile 的 `RUN` 指令中像常规环境变量一样被访问。这些值不会像 `ENV` 值那样在中间或最终镜像中保留。您必须为每个构建参数添加 `--build-arg`。

Using this flag doesn't alter the output you see when the build process echoes the`ARG` lines from the Dockerfile.

​	使用此标志不会更改构建过程回显 Dockerfile 中 `ARG` 行时的输出。

For detailed information on using `ARG` and `ENV` instructions, see the [Dockerfile reference]({{< ref "/reference/Dockerfilereference" >}}).

​	有关使用 `ARG` 和 `ENV` 指令的详细信息，请参见 Dockerfile 参考。

You can also use the `--build-arg` flag without a value, in which case the daemon propagates the value from the local environment into the Docker container it's building:

​	您还可以不提供值使用 `--build-arg` 标志，此时守护进程会将本地环境中的值传递到构建中的 Docker 容器：

```console
$ export HTTP_PROXY=http://10.20.30.2:1234
$ docker buildx build --build-arg HTTP_PROXY .
```

This example is similar to how `docker run -e` works. Refer to the [`docker run` documentation](https://docs.docker.com/reference/cli/docker/container/run/#env) for more information.

​	该示例类似于 `docker run -e` 的工作方式。有关更多信息，请参阅 [`docker run` 文档](https://docs.docker.com/reference/cli/docker/container/run/#env)。

There are also useful built-in build arguments, such as:

​	Docker 还提供了一些内置的构建参数，例如：

- `BUILDKIT_CONTEXT_KEEP_GIT_DIR=<bool>`: trigger git context to keep the `.git` directory
  - `BUILDKIT_CONTEXT_KEEP_GIT_DIR=<bool>`：触发 Git 上下文保留 `.git` 目录。

- `BUILDKIT_INLINE_CACHE=<bool>`: inline cache metadata to image config or not
  - `BUILDKIT_INLINE_CACHE=<bool>`：是否将缓存元数据内联到镜像配置。

- `BUILDKIT_MULTI_PLATFORM=<bool>`: opt into deterministic output regardless of multi-platform output or not
  - `BUILDKIT_MULTI_PLATFORM=<bool>`：是否选择多平台输出的一致性。




```console
$ docker buildx build --build-arg BUILDKIT_MULTI_PLATFORM=1 .
```

Learn more about the built-in build arguments in the [Dockerfile reference docs](https://docs.docker.com/reference/dockerfile/#buildkit-built-in-build-args).

​	在 [Dockerfile 参考文档](https://docs.docker.com/reference/dockerfile/#buildkit-built-in-build-args) 中了解更多关于内置构建参数的信息。

### 额外的构建上下文 (`--build-context`) Additional build contexts (`--build-context`)



```text
--build-context=name=VALUE
```

Define additional build context with specified contents. In Dockerfile the context can be accessed when `FROM name` or `--from=name` is used. When Dockerfile defines a stage with the same name it is overwritten.

​	定义包含指定内容的额外构建上下文。在 Dockerfile 中，当使用 `FROM name` 或 `--from=name` 时，可以访问上下文。当 Dockerfile 中定义了同名的阶段时，该阶段将被覆盖。

The value can be a local source directory, [local OCI layout compliant directory](https://github.com/opencontainers/image-spec/blob/main/image-layout.md), container image (with docker-image:// prefix), Git or HTTP URL.

​	值可以是本地源目录、[本地符合 OCI 布局的目录](https://github.com/opencontainers/image-spec/blob/main/image-layout.md)、容器镜像（带 `docker-image://` 前缀）、Git 或 HTTP URL。

Replace `alpine:latest` with a pinned one:

​	例如，将 `alpine:latest` 替换为固定版本：

```console
$ docker buildx build --build-context alpine=docker-image://alpine@sha256:0123456789 .
```

Expose a secondary local source directory:

​	暴露次级本地源目录：

```console
$ docker buildx build --build-context project=path/to/project/source .
# docker buildx build --build-context project=https://github.com/myuser/project.git .
```



```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
COPY --from=project myfile /
```

#### 使用符合 OCI 布局的目录作为构建上下文 Use an OCI layout directory as build context

Source an image from a local [OCI layout compliant directory](https://github.com/opencontainers/image-spec/blob/main/image-layout.md), either by tag, or by digest:

​	从本地 [符合 OCI 布局的目录](https://github.com/opencontainers/image-spec/blob/main/image-layout.md)中引用镜像，可以通过标签或摘要指定：

```console
$ docker buildx build --build-context foo=oci-layout:///path/to/local/layout:<tag>
$ docker buildx build --build-context foo=oci-layout:///path/to/local/layout@sha256:<digest>
```



```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
RUN apk add git
COPY --from=foo myfile /

FROM foo
```

The OCI layout directory must be compliant with the [OCI layout specification](https://github.com/opencontainers/image-spec/blob/main/image-layout.md). You can reference an image in the layout using either tags, or the exact digest.

​	OCI 布局目录必须符合 [OCI 布局规范](https://github.com/opencontainers/image-spec/blob/main/image-layout.md)。您可以通过标签或确切的摘要在布局中引用镜像。

### 覆盖已配置的构建器实例 (`--builder`) Override the configured builder instance (`--builder`)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).

​	与 [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder) 相同。

### 使用外部缓存源进行构建 (`--cache-from`) Use an external cache source for a build (`--cache-from`)



```text
--cache-from=[NAME|type=TYPE[,KEY=VALUE]]
```

Use an external cache source for a build. Supported types are `registry`, `local`, `gha` and `s3`.

​	使用外部缓存源进行构建。支持的类型有 `registry`、`local`、`gha` 和 `s3`。

- [`registry` source](https://github.com/moby/buildkit#registry-push-image-and-cache-separately) can import cache from a cache manifest or (special) image configuration on the registry.
  - [`registry` 源](https://github.com/moby/buildkit#registry-push-image-and-cache-separately)：从注册表中的缓存清单或特殊镜像配置中导入缓存。

- [`local` source](https://github.com/moby/buildkit#local-directory-1) can import cache from local files previously exported with `--cache-to`.
  - [`local` 源](https://github.com/moby/buildkit#local-directory-1)：从使用 `--cache-to` 导出的本地文件中导入缓存。

- [`gha` source](https://github.com/moby/buildkit#github-actions-cache-experimental) can import cache from a previously exported cache with `--cache-to` in your GitHub repository
  - [`gha` 源](https://github.com/moby/buildkit#github-actions-cache-experimental)：从 GitHub 仓库中导入先前导出的缓存。

- [`s3` source](https://github.com/moby/buildkit#s3-cache-experimental) can import cache from a previously exported cache with `--cache-to` in your S3 bucket
  - [`s3` 源](https://github.com/moby/buildkit#s3-cache-experimental)：从 S3 存储桶中导入先前导出的缓存。


If no type is specified, `registry` exporter is used with a specified reference.

​	如果未指定类型，使用带指定引用的 `registry` 导出器。

`docker` driver currently only supports importing build cache from the registry.

​	当前 `docker` 驱动仅支持从注册表中导入构建缓存。



```console
$ docker buildx build --cache-from=user/app:cache .
$ docker buildx build --cache-from=user/app .
$ docker buildx build --cache-from=type=registry,ref=user/app .
$ docker buildx build --cache-from=type=local,src=path/to/cache .
$ docker buildx build --cache-from=type=gha .
$ docker buildx build --cache-from=type=s3,region=eu-west-1,bucket=mybucket .
```

More info about cache exporters and available attributes: https://github.com/moby/buildkit#export-cache

​	有关缓存导出器和可用属性的更多信息，请参见：https://github.com/moby/buildkit#export-cache

### 调用前端方法 (`--call`) Invoke a frontend method (`--call`)



```text
--call=[build|check|outline|targets]
```

BuildKit frontends can support alternative modes of executions for builds, using frontend methods. Frontend methods are a way to change or extend the behavior of a build invocation, which lets you, for example, inspect, validate, or generate alternative outputs from a build.

​	BuildKit 前端支持使用前端方法的不同构建执行模式。前端方法可以更改或扩展构建调用的行为，允许您检查、验证或生成构建的替代输出。

The `--call` flag for `docker buildx build` lets you specify the frontend method that you want to execute. If this flag is unspecified, it defaults to executing the build and evaluating [build checks]({{< ref "/reference/Buildchecks" >}}).

​	`docker buildx build` 的 `--call` 标志允许您指定要执行的前端方法。如果未指定该标志，默认为执行构建并评估 [构建检查]({{< ref "/reference/Buildchecks" >}})。

For Dockerfiles, the available methods are:

​	对于 Dockerfile，可用方法如下：

| Command                | Description                                                  |
| ---------------------- | ------------------------------------------------------------ |
| `build` (default)      | 执行构建并评估当前构建目标的构建检查。 Execute the build and evaluate build checks for the current build target. |
| `check`                | 评估整个 Dockerfile 或选定目标的构建检查，而不执行构建。 Evaluate build checks for the either the entire Dockerfile or the selected target, without executing a build. |
| `outline`              | 显示可以为目标设置的构建参数及其默认值。 Show the build arguments that you can set for a target, and their default values. |
| `targets`              | 列出 Dockerfile 中的所有构建目标。 List all the build targets in the Dockerfile. |
| `subrequests.describe` | 列出当前前端支持的所有前端方法。 List all the frontend methods that the current frontend supports. |

Note that other frontends may implement these or other methods. To see the list of available methods for the frontend you're using, use `--call=subrequests.describe`.

​	其他前端可能实现这些或其他方法。要查看当前使用的前端的可用方法列表，请使用 `--call=subrequests.describe`。

```console
$ docker buildx build -q --call=subrequests.describe .

NAME                 VERSION DESCRIPTION
outline              1.0.0   List all parameters current build target supports 列出当前构建目标支持的所有参数
targets              1.0.0   List all targets current build supports 列出当前构建支持的所有目标
subrequests.describe 1.0.0   List available subrequest types 列出可用的请求类型
```

#### Descriptions

The [`--call=targets`](https://docs.docker.com/reference/cli/docker/buildx/build/#call-targets) and [`--call=outline`](https://docs.docker.com/reference/cli/docker/buildx/build/#call-outline) methods include descriptions for build targets and arguments, if available. Descriptions are generated from comments in the Dockerfile. A comment on the line before a `FROM` instruction becomes the description of a build target, and a comment before an `ARG` instruction the description of a build argument. The comment must lead with the name of the stage or argument, for example:

​	[`--call=targets`](https://docs.docker.com/reference/cli/docker/buildx/build/#call-targets) 和 [`--call=outline`](https://docs.docker.com/reference/cli/docker/buildx/build/#call-outline) 方法（如可用）会包含构建目标和参数的描述。这些描述从 Dockerfile 中的注释生成。`FROM` 指令之前的注释将成为构建目标的描述，`ARG` 指令之前的注释为构建参数的描述。例如：

```dockerfile
# syntax=docker/dockerfile:1

# GO_VERSION sets the Go version for the build
# GO_VERSION 设置构建的 Go 版本
ARG GO_VERSION=1.22

# base-builder is the base stage for building the project
# base-builder 是用于构建项目的基础阶段
FROM golang:${GO_VERSION} AS base-builder
```

When you run `docker buildx build --call=outline`, the output includes the descriptions, as follows:

​	运行 `docker buildx build --call=outline` 时，输出会包含描述，如下所示：

```console
$ docker buildx build -q --call=outline .

TARGET:      base-builder
DESCRIPTION: is the base stage for building the project 是用于构建项目的基础阶段

BUILD ARG    VALUE   DESCRIPTION
GO_VERSION   1.22    sets the Go version for the build 设置构建的 Go 版本
```

For more examples on how to write Dockerfile docstrings, check out [the Dockerfile for Docker docs](https://github.com/docker/docs/blob/main/Dockerfile).

#### Call: check (`--check`)

The `check` method evaluates build checks without executing the build. The `--check` flag is a convenient shorthand for `--call=check`. Use the `check` method to validate the build configuration before starting the build.

​	`check` 方法评估构建检查而不执行构建。`--check` 标志是 `--call=check` 的简便写法。使用 `check` 方法在开始构建之前验证构建配置。

```console
$ docker buildx build -q --check https://github.com/docker/docs.git

WARNING: InvalidBaseImagePlatform
Base image wjdp/htmltest:v0.17.0 was pulled with platform "linux/amd64", expected "linux/arm64" for current build
Dockerfile:43
--------------------
  41 |         "#content/desktop/previous-versions/*.md"
  42 |
  43 | >>> FROM wjdp/htmltest:v${HTMLTEST_VERSION} AS test
  44 |     WORKDIR /test
  45 |     COPY --from=build /out ./public
--------------------
```

Using `--check` without specifying a target evaluates the entire Dockerfile. If you want to evaluate a specific target, use the `--target` flag.

​	使用 `--check` 而不指定目标将评估整个 Dockerfile。要评估特定目标，请使用 `--target` 标志。



#### Call: outline

The `outline` method prints the name of the specified target (or the default target, if `--target` isn't specified), and the build arguments that the target consumes, along with their default values, if set.

​	`outline` 方法会打印指定目标的名称（如果未指定 `--target`，则为默认目标），以及目标使用的构建参数及其默认值（如果已设置）。

The following example shows the default target `release` and its build arguments:

​	以下示例显示了默认目标 `release` 及其构建参数：

```console
$ docker buildx build -q --call=outline https://github.com/docker/docs.git

TARGET:      release
DESCRIPTION: is an empty scratch image with only compiled assets 是一个仅包含已编译资产的空白镜像

BUILD ARG          VALUE     DESCRIPTION
GO_VERSION         1.22      sets the Go version for the base stage 设置基础阶段的 Go 版本
HUGO_VERSION       0.127.0
HUGO_ENV                     sets the hugo.Environment (production, development, preview)
DOCS_URL                     sets the base URL for the site
PAGEFIND_VERSION   1.1.0
```

This means that the `release` target is configurable using these build arguments:

​	这意味着 `release` 目标可以使用以下构建参数进行配置：

```console
$ docker buildx build \
  --build-arg GO_VERSION=1.22 \
  --build-arg HUGO_VERSION=0.127.0 \
  --build-arg HUGO_ENV=production \
  --build-arg DOCS_URL=https://example.com \
  --build-arg PAGEFIND_VERSION=1.1.0 \
  --target release https://github.com/docker/docs.git
```

#### Call: targets

The `targets` method lists all the build targets in the Dockerfile. These are the stages that you can build using the `--target` flag. It also indicates the default target, which is the target that will be built when you don't specify a target.

​	`targets` 方法列出 Dockerfile 中的所有构建目标。这些是可以使用 `--target` 标志构建的阶段。它还指示默认目标，即在未指定目标时构建的目标。



```console
$ docker buildx build -q --call=targets https://github.com/docker/docs.git

TARGET            DESCRIPTION
base              is the base stage with build dependencies 是包含构建依赖的基础阶段
node              installs Node.js dependencies 安装 Node.js 依赖
hugo              downloads and extracts the Hugo binary 下载并提取 Hugo 二进制文件
build-base        is the base stage for building the site 是用于构建站点的基础阶段
dev               is for local development with Docker Compose 用于 Docker Compose 的本地开发
build             creates production builds with Hugo 使用 Hugo 创建生产构建
lint              lints markdown files 校验 markdown 文件
test              validates HTML output and checks for broken links 验证 HTML 输出并检查断链
update-modules    downloads and vendors Hugo modules 下载并供应 Hugo 模块
vendor            is an empty stage with only vendored Hugo modules 是仅包含供应模块的空阶段
build-upstream    builds an upstream project with a replacement module 使用替代模块构建上游项目
validate-upstream validates HTML output for upstream builds 验证上游构建的 HTML 输出
unused-media      checks for unused graphics and other media 检查未使用的图形和其他媒体
pagefind          installs the Pagefind runtime 安装 Pagefind 运行时
index             generates a Pagefind index 生成 Pagefind 索引
test-go-redirects checks that the /go/ redirects are valid 检查 `/go/` 重定向的有效性
release (default) is an empty scratch image with only compiled assets 是仅包含已编译资产的空白镜像
```

### 导出构建缓存到外部缓存目标 (`--cache-to`) Export build cache to an external cache destination (`--cache-to`)



```text
--cache-to=[NAME|type=TYPE[,KEY=VALUE]]
```

Export build cache to an external cache destination. Supported types are `registry`, `local`, `inline`, `gha` and `s3`.

​	将构建缓存导出到外部缓存目标。支持的类型有 `registry`、`local`、`inline`、`gha` 和 `s3`。

- [`registry` type](https://github.com/moby/buildkit#registry-push-image-and-cache-separately) exports build cache to a cache manifest in the registry.
  - [`registry` 类型](https://github.com/moby/buildkit#registry-push-image-and-cache-separately)：将构建缓存导出到注册表中的缓存清单。

- [`local` type](https://github.com/moby/buildkit#local-directory-1) exports cache to a local directory on the client.
  - [`local` 类型](https://github.com/moby/buildkit#local-directory-1)：将缓存导出到客户端上的本地目录。

- [`inline` type](https://github.com/moby/buildkit#inline-push-image-and-cache-together) writes the cache metadata into the image configuration.
  - [`inline` 类型](https://github.com/moby/buildkit#inline-push-image-and-cache-together)：将缓存元数据写入镜像配置。

- [`gha` type](https://github.com/moby/buildkit#github-actions-cache-experimental) exports cache through the [GitHub Actions Cache service API](https://github.com/tonistiigi/go-actions-cache/blob/master/api.md#authentication).
  - [`gha` 类型](https://github.com/moby/buildkit#github-actions-cache-experimental)：通过 [GitHub Actions 缓存服务 API](https://github.com/tonistiigi/go-actions-cache/blob/master/api.md#authentication) 导出缓存。

- [`s3` type](https://github.com/moby/buildkit#s3-cache-experimental) exports cache to a S3 bucket.
  - [`s3` 类型](https://github.com/moby/buildkit#s3-cache-experimental)：将缓存导出到 S3 存储桶。


The `docker` driver only supports cache exports using the `inline` and `local` cache backends.

​	`docker` 驱动程序仅支持使用 `inline` 和 `local` 缓存后端导出缓存。

Attribute key:

​	属性键：

- `mode` - Specifies how many layers are exported with the cache. `min` on only exports layers already in the final build stage, `max` exports layers for all stages. Metadata is always exported for the whole build.
  - `mode` - 指定导出缓存的层数。`min` 仅导出最终构建阶段中已存在的层，`max` 导出所有阶段的层。元数据总是导出整个构建过程的。




```console
$ docker buildx build --cache-to=user/app:cache .
$ docker buildx build --cache-to=type=inline .
$ docker buildx build --cache-to=type=registry,ref=user/app .
$ docker buildx build --cache-to=type=local,dest=path/to/cache .
$ docker buildx build --cache-to=type=gha .
$ docker buildx build --cache-to=type=s3,region=eu-west-1,bucket=mybucket .
```

More info about cache exporters and available attributes: https://github.com/moby/buildkit#export-cache

​	更多关于缓存导出器及可用属性的信息，请参见：https://github.com/moby/buildkit#export-cache

### 使用自定义父 cgroup (`--cgroup-parent`) Use a custom parent cgroup (`--cgroup-parent`)

When you run `docker buildx build` with the `--cgroup-parent` option, the daemon runs the containers used in the build with the [corresponding `docker run` flag](https://docs.docker.com/reference/cli/docker/container/run/#cgroup-parent).

​	当你运行 `docker buildx build` 并使用 `--cgroup-parent` 选项时，守护进程会根据[对应的 `docker run` 标志](https://docs.docker.com/reference/cli/docker/container/run/#cgroup-parent)来运行构建使用的容器。

### 指定 Dockerfile (`-f`, `--file`) Specify a Dockerfile (`-f`, `--file`)



```console
$ docker buildx build -f <filepath> .
```

Specifies the filepath of the Dockerfile to use. If unspecified, a file named `Dockerfile` at the root of the build context is used by default.

​	指定要使用的 Dockerfile 文件路径。如果未指定，则默认使用构建上下文根目录下名为 `Dockerfile` 的文件。

To read a Dockerfile from stdin, you can use `-` as the argument for `--file`.

​	要从标准输入读取 Dockerfile，可以使用 `-` 作为 `--file` 的参数。

```console
$ cat Dockerfile | docker buildx build -f - .
```

### 将单平台构建结果加载到 `docker images` (`--load`) Load the single-platform build result to `docker images` (`--load`)

Shorthand for [`--output=type=docker`](https://docs.docker.com/reference/cli/docker/buildx/build/#docker). Will automatically load the single-platform build result to `docker images`.

​	这是 [`--output=type=docker`](https://docs.docker.com/reference/cli/docker/buildx/build/#docker) 的简写。将自动加载单平台构建结果到 `docker images`。

### 将构建结果元数据写入文件 (`--metadata-file`) Write build result metadata to a file (`--metadata-file`)

To output build metadata such as the image digest, pass the `--metadata-file` flag. The metadata will be written as a JSON object to the specified file. The directory of the specified file must already exist and be writable.

​	要输出构建元数据（如镜像摘要），可以使用 `--metadata-file` 标志。元数据将以 JSON 对象形式写入指定的文件中。该文件所在的目录必须已存在并且可写。

```console
$ docker buildx build --load --metadata-file metadata.json .
$ cat metadata.json
```



```json
{
  "buildx.build.provenance": {},
  "buildx.build.ref": "mybuilder/mybuilder0/0fjb6ubs52xx3vygf6fgdl611",
  "buildx.build.warnings": {},
  "containerimage.config.digest": "sha256:2937f66a9722f7f4a2df583de2f8cb97fc9196059a410e7f00072fc918930e66",
  "containerimage.descriptor": {
    "annotations": {
      "config.digest": "sha256:2937f66a9722f7f4a2df583de2f8cb97fc9196059a410e7f00072fc918930e66",
      "org.opencontainers.image.created": "2022-02-08T21:28:03Z"
    },
    "digest": "sha256:19ffeab6f8bc9293ac2c3fdf94ebe28396254c993aea0b5a542cfb02e0883fa3",
    "mediaType": "application/vnd.oci.image.manifest.v1+json",
    "size": 506
  },
  "containerimage.digest": "sha256:19ffeab6f8bc9293ac2c3fdf94ebe28396254c993aea0b5a542cfb02e0883fa3"
}
```

> **Note**
>
> Build record [provenance](https://docs.docker.com/build/metadata/attestations/slsa-provenance/#provenance-attestation-example) (`buildx.build.provenance`) includes minimal provenance by default. Set the `BUILDX_METADATA_PROVENANCE` environment variable to customize this behavior:
>
> ​	构建记录 [provenance](https://docs.docker.com/build/metadata/attestations/slsa-provenance/#provenance-attestation-example) (`buildx.build.provenance`) 默认包含最小的溯源。可以通过设置环境变量 `BUILDX_METADATA_PROVENANCE` 来自定义此行为：
>
> - `min` sets minimal provenance (default).
>   - `min` 设置最小溯源（默认）。
> - `max` sets full provenance.
>   - `max` 设置完整溯源。
> - `disabled`, `false` or `0` doesn't set any provenance.
>   - `disabled`、`false` 或 `0` 不设置溯源。

> **Note**
>
> Build warnings (`buildx.build.warnings`) are not included by default. Set the `BUILDX_METADATA_WARNINGS` environment variable to `1` or `true` to include them.
>
> ​	构建警告 (`buildx.build.warnings`) 默认不包含。将环境变量 `BUILDX_METADATA_WARNINGS` 设置为 `1` 或 `true` 可以包含这些警告。

### 设置构建期间 RUN 指令的网络模式 (`--network`) Set the networking mode for the RUN instructions during build (`--network`)

Available options for the networking mode are:

​	可用的网络模式选项有：

- `default` (default): Run in the default network.
  - `default`（默认）：在默认网络中运行。

- `none`: Run with no network access.
  - `none`：无网络访问。

- `host`: Run in the host’s network environment.
  - `host`：在主机的网络环境中运行。


Find more details in the [Dockerfile reference](https://docs.docker.com/reference/dockerfile/#run---network).

​	更多细节请参阅 [Dockerfile 参考](https://docs.docker.com/reference/dockerfile/#run---network)。

### 忽略特定阶段的构建缓存 (`--no-cache-filter`) Ignore build cache for specific stages (`--no-cache-filter`)

The `--no-cache-filter` lets you specify one or more stages of a multi-stage Dockerfile for which build cache should be ignored. To specify multiple stages, use a comma-separated syntax:

​	`--no-cache-filter` 允许你指定多阶段 Dockerfile 的一个或多个阶段，在这些阶段中将忽略构建缓存。要指定多个阶段，可以使用逗号分隔的语法：

```console
$ docker buildx build --no-cache-filter stage1,stage2,stage3 .
```

For example, the following Dockerfile contains four stages:

​	例如，以下 Dockerfile 包含四个阶段：

- `base`
- `install`
- `test`
- `release`



```dockerfile
# syntax=docker/dockerfile:1

FROM oven/bun:1 as base
WORKDIR /app

FROM base AS install
WORKDIR /temp/dev
RUN --mount=type=bind,source=package.json,target=package.json \
    --mount=type=bind,source=bun.lockb,target=bun.lockb \
    bun install --frozen-lockfile

FROM base AS test
COPY --from=install /temp/dev/node_modules node_modules
COPY . .
RUN bun test

FROM base AS release
ENV NODE_ENV=production
COPY --from=install /temp/dev/node_modules node_modules
COPY . .
ENTRYPOINT ["bun", "run", "index.js"]
```

To ignore the cache for the `install` stage:

​	要忽略 `install` 阶段的缓存：

```console
$ docker buildx build --no-cache-filter install .
```

To ignore the cache the `install` and `release` stages:

​	要忽略 `install` 和 `release` 阶段的缓存：

```console
$ docker buildx build --no-cache-filter install,release .
```

The arguments for the `--no-cache-filter` flag must be names of stages.

​	`--no-cache-filter` 标志的参数必须是阶段的名称。

### 设置构建结果的导出操作 (`-o`, `--output`) Set the export action for the build result (`-o`, `--output`)



```text
-o, --output=[PATH,-,type=TYPE[,KEY=VALUE]
```

Sets the export action for the build result. The default output, when using the `docker` [build driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}}), is a container image exported to the local image store. The `--output` flag makes this step configurable allows export of results directly to the client's filesystem, an OCI image tarball, a registry, and more.

​	设置构建结果的导出操作。当使用 `docker` [构建驱动]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}})时，默认输出是导出到本地镜像存储的容器镜像。`--output` 标志使得该步骤可以配置为直接导出结果到客户端文件系统、OCI 镜像 tarball、注册表等。

Buildx with `docker` driver only supports the local, tarball, and image [exporters]({{< ref "/manuals/DockerBuild/Exporters" >}}). The `docker-container` driver supports all exporters.

​	使用 `docker` 驱动的 Buildx 仅支持本地、tarball 和镜像[导出器]({{< ref "/manuals/DockerBuild/Exporters" >}})。`docker-container` 驱动支持所有导出器。

If you only specify a filepath as the argument to `--output`, Buildx uses the local exporter. If the value is `-`, Buildx uses the `tar` exporter and writes the output to stdout.

​	如果只指定文件路径作为 `--output` 参数，则 Buildx 使用本地导出器。如果值为 `-`，则 Buildx 使用 `tar` 导出器并将输出写入标准输出。

```console
$ docker buildx build -o . .
$ docker buildx build -o outdir .
$ docker buildx build -o - . > out.tar
$ docker buildx build -o type=docker .
$ docker buildx build -o type=docker,dest=- . > myimage.tar
$ docker buildx build -t tonistiigi/foo -o type=registry
```

You can export multiple outputs by repeating the flag.

​	通过重复标志可以导出多个输出。

Supported exported types are:

​	支持的导出类型有：

- [`local`](https://docs.docker.com/reference/cli/docker/buildx/build/#local)
- [`tar`](https://docs.docker.com/reference/cli/docker/buildx/build/#tar)
- [`oci`](https://docs.docker.com/reference/cli/docker/buildx/build/#oci)
- [`docker`](https://docs.docker.com/reference/cli/docker/buildx/build/#docker)
- [`image`](https://docs.docker.com/reference/cli/docker/buildx/build/#image)
- [`registry`](https://docs.docker.com/reference/cli/docker/buildx/build/#registry)

#### `local`

The `local` export type writes all result files to a directory on the client. The new files will be owned by the current user. On multi-platform builds, all results will be put in subdirectories by their platform.

​	`local` 导出类型将所有结果文件写入客户端上的目录。新文件将归当前用户所有。在多平台构建中，所有结果将按平台存放在子目录中。

Attribute key:

​	属性键：

- `dest` - destination directory where files will be written
  - `dest` - 文件写入的目标目录


For more information, see [Local and tar exporters]({{< ref "/manuals/DockerBuild/Exporters/Localandtarexporters" >}}).

​	更多信息，请参阅 [Local 和 tar 导出器]({{< ref "/manuals/DockerBuild/Exporters/Localandtarexporters" >}})。

#### `tar`

The `tar` export type writes all result files as a single tarball on the client. On multi-platform builds all results will be put in subdirectories by their platform.

​	`tar` 导出类型将所有结果文件写为一个 tarball。在多平台构建中，所有结果将按平台存放在子目录中。

Attribute key:

​	属性键：

- `dest` - destination path where tarball will be written. “-” writes to stdout.
  - `dest` - tarball 将写入的目标路径。“-” 表示写入标准输出。


For more information, see [Local and tar exporters]({{< ref "/manuals/DockerBuild/Exporters/Localandtarexporters" >}}).

​	更多信息，请参阅 [Local 和 tar 导出器]({{< ref "/manuals/DockerBuild/Exporters/Localandtarexporters" >}})。

#### `oci`

The `oci` export type writes the result image or manifest list as an [OCI image layout](https://github.com/opencontainers/image-spec/blob/v1.0.1/image-layout.md) tarball on the client.

​	`oci` 导出类型将结果镜像或清单列表写为 [OCI 镜像布局](https://github.com/opencontainers/image-spec/blob/v1.0.1/image-layout.md) 的 tarball。

Attribute key:

​	属性键：

- `dest` - destination path where tarball will be written. “-” writes to stdout.
  - `dest` - tarball 写入的目标路径。“-” 表示写入标准输出。


For more information, see [OCI and Docker exporters]({{< ref "/manuals/DockerBuild/Exporters/OCIandDockerexporters" >}}).

​	更多信息，请参阅 [OCI 和 Docker 导出器]({{< ref "/manuals/DockerBuild/Exporters/OCIandDockerexporters" >}})。

#### `docker`

The `docker` export type writes the single-platform result image as a [Docker image specification](https://github.com/docker/docker/blob/v20.10.2/image/spec/v1.2.md) tarball on the client. Tarballs created by this exporter are also OCI compatible.

​	`docker` 导出类型将单平台结果镜像写为 [Docker 镜像规范](https://github.com/docker/docker/blob/v20.10.2/image/spec/v1.2.md) 的 tarball。此导出器创建的 tarball 也兼容 OCI。

The default image store in Docker Engine doesn't support loading multi-platform images. You can enable the containerd image store, or push multi-platform images is to directly push to a registry, see [`registry`](https://docs.docker.com/reference/cli/docker/buildx/build/#registry).

​	Docker Engine 中的默认镜像存储不支持加载多平台镜像。可以启用 containerd 镜像存储，或直接推送多平台镜像至注册表，参见 [`registry`](https://docs.docker.com/reference/cli/docker/buildx/build/#registry)。

Attribute keys:

​	属性键：

- `dest` - destination path where tarball will be written. If not specified, the tar will be loaded automatically to the local image store.
  - `dest` - tarball 写入的目标路径。如果未指定，tar 将自动加载到本地镜像存储。

- `context` - name for the Docker context where to import the result
  - `context` - 导入结果的 Docker 上下文名称


For more information, see [OCI and Docker exporters]({{< ref "/manuals/DockerBuild/Exporters/OCIandDockerexporters" >}}).

​	更多信息，请参阅 [OCI 和 Docker 导出器]({{< ref "/manuals/DockerBuild/Exporters/OCIandDockerexporters" >}})。

#### `image`

The `image` exporter writes the build result as an image or a manifest list. When using `docker` driver the image will appear in `docker images`. Optionally, image can be automatically pushed to a registry by specifying attributes.

​	`image` 导出器将构建结果写为镜像或清单列表。当使用 `docker` 驱动时，镜像将出现在 `docker images` 中。可以选择自动将镜像推送至注册表。

Attribute keys:

​	属性键：

- `name` - name (references) for the new image.
  - `name` - 新镜像的名称（引用）。

- `push` - Boolean to automatically push the image.
  - `push` - 布尔值，是否自动推送镜像。


For more information, see [Image and registry exporters]({{< ref "/manuals/DockerBuild/Exporters/Imageandregistryexporters" >}}).

​	更多信息，请参阅 [Image 和 registry 导出器]({{< ref "/manuals/DockerBuild/Exporters/Imageandregistryexporters" >}})。

#### `registry`

The `registry` exporter is a shortcut for `type=image,push=true`.

​	`registry` 导出器是 `type=image,push=true` 的快捷方式。

For more information, see [Image and registry exporters]({{< ref "/manuals/DockerBuild/Exporters/Imageandregistryexporters" >}}).

​	更多信息，请参阅 [Image 和 registry 导出器]({{< ref "/manuals/DockerBuild/Exporters/Imageandregistryexporters" >}})。

### 设置构建的目标平台 (`--platform`) Set the target platforms for the build (`--platform`)



```text
--platform=value[,value]
```

Set the target platform for the build. All `FROM` commands inside the Dockerfile without their own `--platform` flag will pull base images for this platform and this value will also be the platform of the resulting image.

​	设置构建的目标平台。Dockerfile 中没有自身 `--platform` 标志的所有 `FROM` 命令都将会为该平台拉取基础镜像，该值也将是结果镜像的平台。

The default value is the platform of the BuildKit daemon where the build runs. The value takes the form of `os/arch` or `os/arch/variant`. For example, `linux/amd64` or `linux/arm/v7`. Additionally, the `--platform` flag also supports a special `local` value, which tells BuildKit to use the platform of the BuildKit client that invokes the build.

​	默认值是构建运行所在 BuildKit 守护进程的平台。格式为 `os/arch` 或 `os/arch/variant`。例如，`linux/amd64` 或 `linux/arm/v7`。此外，`--platform` 标志还支持一个特殊的 `local` 值，指示 BuildKit 使用调用构建的 BuildKit 客户端平台。

When using `docker-container` driver with `buildx`, this flag can accept multiple values as an input separated by a comma. With multiple values the result will be built for all of the specified platforms and joined together into a single manifest list.

​	使用 `docker-container` 驱动和 `buildx` 时，此标志可以接受用逗号分隔的多个值作为输入。对于多个值，结果将为指定平台构建，并合并成单个清单列表。

If the `Dockerfile` needs to invoke the `RUN` command, the builder needs runtime support for the specified platform. In a clean setup, you can only execute `RUN` commands for your system architecture. If your kernel supports [`binfmt_misc`](https://en.wikipedia.org/wiki/Binfmt_misc) launchers for secondary architectures, buildx will pick them up automatically. Docker Desktop releases come with `binfmt_misc` automatically configured for `arm64` and `arm` architectures. You can see what runtime platforms your current builder instance supports by running `docker buildx inspect --bootstrap`.

​	如果 Dockerfile 需要调用 `RUN` 命令，构建器需为指定平台提供运行时支持。在一个干净的设置中，你只能为系统架构执行 `RUN` 命令。如果你的内核支持用于次要架构的 [`binfmt_misc`](https://en.wikipedia.org/wiki/Binfmt_misc) 启动器，buildx 会自动检测它们。Docker Desktop 版本自带 `binfmt_misc`，自动配置了 `arm64` 和 `arm` 架构。你可以运行 `docker buildx inspect --bootstrap` 来查看当前构建器实例支持的平台。

Inside a `Dockerfile`, you can access the current platform value through `TARGETPLATFORM` build argument. Refer to the [Dockerfile reference](https://docs.docker.com/reference/dockerfile/#automatic-platform-args-in-the-global-scope) for the full description of automatic platform argument variants .

​	在 Dockerfile 中，你可以通过 `TARGETPLATFORM` 构建参数访问当前平台值。详细描述请参见 [Dockerfile 参考](https://docs.docker.com/reference/dockerfile/#automatic-platform-args-in-the-global-scope)。

You can find the formatting definition for the platform specifier in the [containerd source code](https://github.com/containerd/containerd/blob/v1.4.3/platforms/platforms.go#L63).

​	平台标识符的格式定义可在 [containerd 源代码](https://github.com/containerd/containerd/blob/v1.4.3/platforms/platforms.go#L63) 中找到。

```console
$ docker buildx build --platform=linux/arm64 .
$ docker buildx build --platform=linux/amd64,linux/arm64,linux/arm/v7 .
$ docker buildx build --platform=darwin .
```

### 设置进度输出类型 (`--progress`) Set type of progress output (`--progress`)



```text
--progress=VALUE
```

Set type of progress output (`auto`, `plain`, `tty`, `rawjson`). Use `plain` to show container output (default `auto`).

​	设置进度输出类型（`auto`、`plain`、`tty`、`rawjson`）。使用 `plain` 显示容器输出（默认 `auto`）。

> **Note**
>
> You can also use the `BUILDKIT_PROGRESS` environment variable to set its value.
>
> ​	你也可以使用环境变量 `BUILDKIT_PROGRESS` 来设置其值。

The following example uses `plain` output during the build:

​	以下示例在构建期间使用 `plain` 输出：

```console
$ docker buildx build --load --progress=plain .

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile: 227B 0.0s done
#1 DONE 0.1s

#2 [internal] load .dockerignore
#2 transferring context: 129B 0.0s done
#2 DONE 0.0s
...
```

> **Note**
>
> Check also the [`BUILDKIT_COLORS`](https://docs.docker.com/build/building/variables/#buildkit_colors) environment variable for modifying the colors of the terminal output.
>
> ​	请检查环境变量 [`BUILDKIT_COLORS`](https://docs.docker.com/build/building/variables/#buildkit_colors) 以修改终端输出的颜色。

The `rawjson` output marshals the solve status events from BuildKit to JSON lines. This mode is designed to be read by an external program.

​	`rawjson` 输出将 BuildKit 的解决状态事件序列化为 JSON 行。这种模式设计为供外部程序读取。

### Create provenance attestations (`--provenance`)

Shorthand for [`--attest=type=provenance`](https://docs.docker.com/reference/cli/docker/buildx/build/#attest), used to configure provenance attestations for the build result. For example, `--provenance=mode=max` can be used as an abbreviation for `--attest=type=provenance,mode=max`.

​	这是 [`--attest=type=provenance`](https://docs.docker.com/reference/cli/docker/buildx/build/#attest) 的简写，用于配置构建结果的溯源声明。例如，可以使用 `--provenance=mode=max` 作为 `--attest=type=provenance,mode=max` 的简写。

Additionally, `--provenance` can be used with Boolean values to enable or disable provenance attestations. For example, `--provenance=false` disables all provenance attestations, while `--provenance=true` enables all provenance attestations.

​	此外，`--provenance` 可以与布尔值一起使用，以启用或禁用溯源声明。例如，`--provenance=false` 禁用所有溯源声明，而 `--provenance=true` 启用所有溯源声明。

By default, a minimal provenance attestation will be created for the build result. Note that the default image store in Docker Engine doesn't support attestations. Provenance attestations only persist for images pushed directly to a registry if you use the default image store. Alternatively, you can switch to using the containerd image store.

​	默认情况下，构建结果将创建一个最小溯源声明。注意，Docker Engine 中的默认镜像存储不支持声明。溯源声明仅在你使用默认镜像存储时直接推送到注册表的镜像中持久保存。或者，你可以切换到使用 containerd 镜像存储。

For more information about provenance attestations, see [here]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations/Provenanceattestations" >}}).

​	有关溯源声明的更多信息，请参见[此处]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations/Provenanceattestations" >}})。

### 将构建结果推送到注册表 (`--push`) Push the build result to a registry (`--push`)

Shorthand for [`--output=type=registry`](https://docs.docker.com/reference/cli/docker/buildx/build/#registry). Will automatically push the build result to registry.

​	这是 [`--output=type=registry`](https://docs.docker.com/reference/cli/docker/buildx/build/#registry) 的简写。将自动把构建结果推送到注册表。

### 创建 SBOM 声明 (`--sbom`) Create SBOM attestations (`--sbom`)

Shorthand for [`--attest=type=sbom`](https://docs.docker.com/reference/cli/docker/buildx/build/#attest), used to configure SBOM attestations for the build result. For example, `--sbom=generator=<user>/<generator-image>` can be used as an abbreviation for `--attest=type=sbom,generator=<user>/<generator-image>`.

​	这是 [`--attest=type=sbom`](https://docs.docker.com/reference/cli/docker/buildx/build/#attest) 的简写，用于配置构建结果的 SBOM 声明。例如，可以使用 `--sbom=generator=<user>/<generator-image>` 作为 `--attest=type=sbom,generator=<user>/<generator-image>` 的简写。

Additionally, `--sbom` can be used with Boolean values to enable or disable SBOM attestations. For example, `--sbom=false` disables all SBOM attestations.

​	此外，`--sbom` 可以与布尔值一起使用，以启用或禁用 SBOM 声明。例如，`--sbom=false` 禁用所有 SBOM 声明。

Note that the default image store in Docker Engine doesn't support attestations. Provenance attestations only persist for images pushed directly to a registry if you use the default image store. Alternatively, you can switch to using the containerd image store.

​	注意，Docker Engine 中的默认镜像存储不支持声明。溯源声明仅在你使用默认镜像存储时直接推送到注册表的镜像中持久保存。或者，你可以切换到使用 containerd 镜像存储。

For more information, see [here]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations/SBOMattestations" >}}).

​	有关更多信息，请参见[此处]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations/SBOMattestations" >}})。

### 向构建公开的秘密 (`--secret`) Secret to expose to the build (`--secret`)



```text
--secret=[type=TYPE[,KEY=VALUE]
```

Exposes secrets (authentication credentials, tokens) to the build. A secret can be mounted into the build using a `RUN --mount=type=secret` mount in the [Dockerfile](https://docs.docker.com/reference/dockerfile/#run---mounttypesecret). For more information about how to use build secrets, see [Build secrets]({{< ref "/manuals/DockerBuild/Building/Secrets" >}}).

​	将秘密（如身份验证凭证、令牌）暴露给构建。可以使用 Dockerfile 中的 `RUN --mount=type=secret` 挂载构建秘密。有关如何使用构建秘密的更多信息，请参阅 [构建秘密]({{< ref "/manuals/DockerBuild/Building/Secrets" >}})。

Supported types are:

​	支持的类型有：

- [`file`](https://docs.docker.com/reference/cli/docker/buildx/build/#file)
- [`env`](https://docs.docker.com/reference/cli/docker/buildx/build/#env)

Buildx attempts to detect the `type` automatically if unset.

​	如果未设置，Buildx 会自动检测 `type`。

#### `file`

Attribute keys:

​	属性键：

- `id` - ID of the secret. Defaults to base name of the `src` path.
  - `id` - 秘密的 ID。默认值为 `src` 路径的基本名称。

- `src`, `source` - Secret filename. `id` used if unset.
  - `src`、`source` - 秘密文件名。若未设置 `id`，将使用默认值。



```dockerfile
# syntax=docker/dockerfile:1
FROM python:3
RUN pip install awscli
RUN --mount=type=secret,id=aws,target=/root/.aws/credentials \
  aws s3 cp s3://... ...
```



```console
$ docker buildx build --secret id=aws,src=$HOME/.aws/credentials .
```

#### `env`

Attribute keys:

​	属性键：

- `id` - ID of the secret. Defaults to `env` name.
  - `id` - 秘密的 ID。默认为 `env` 名称。

- `env` - Secret environment variable. `id` used if unset, otherwise will look for `src`, `source` if `id` unset.
  - `env` - 秘密环境变量。若未设置 `id`，将使用默认值，否则将查找 `src`、`source`（若 `id` 未设置）。




```dockerfile
# syntax=docker/dockerfile:1
FROM node:alpine
RUN --mount=type=bind,target=. \
  --mount=type=secret,id=SECRET_TOKEN,env=SECRET_TOKEN \
  yarn run test
```



```console
$ SECRET_TOKEN=token docker buildx build --secret id=SECRET_TOKEN .
```

### 构建容器的共享内存大小 (`--shm-size`) Shared memory size for build containers (`--shm-size`)

Sets the size of the shared memory allocated for build containers when using `RUN` instructions.

​	设置使用 `RUN` 指令时分配给构建容器的共享内存大小。

The format is `<number><unit>`. `number` must be greater than `0`. Unit is optional and can be `b` (bytes), `k` (kilobytes), `m` (megabytes), or `g` (gigabytes). If you omit the unit, the system uses bytes.

​	格式为 `<number><unit>`。`number` 必须大于 `0`。单位是可选的，可以是 `b`（字节）、`k`（千字节）、`m`（兆字节）或 `g`（千兆字节）。如果省略单位，系统将使用字节。

> **Note**
>
> In most cases, it is recommended to let the builder automatically determine the appropriate configurations. Manual adjustments should only be considered when specific performance tuning is required for complex build scenarios.
>
> ​	在大多数情况下，建议让构建器自动确定适当的配置。仅在需要为复杂的构建场景进行特定性能调整时考虑手动调整。

### 向构建公开 SSH 代理套接字或密钥 (`--ssh`) SSH agent socket or keys to expose to the build (`--ssh`)



```text
--ssh=default|<id>[=<socket>|<key>[,<key>]]
```

This can be useful when some commands in your Dockerfile need specific SSH authentication (e.g., cloning a private repository).

​	当你的 Dockerfile 中的某些命令需要特定的 SSH 认证（例如克隆私有仓库）时，这可能会很有用。

`--ssh` exposes SSH agent socket or keys to the build and can be used with the [`RUN --mount=type=ssh` mount](https://docs.docker.com/reference/dockerfile/#run---mounttypessh).

​	`--ssh` 将 SSH 代理套接字或密钥暴露给构建，可与 [`RUN --mount=type=ssh`](https://docs.docker.com/reference/dockerfile/#run---mounttypessh) 挂载一起使用。

Example to access Gitlab using an SSH agent socket:

​	使用 SSH 代理套接字访问 Gitlab 的示例：



```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
RUN apk add --no-cache openssh-client
RUN mkdir -p -m 0700 ~/.ssh && ssh-keyscan gitlab.com >> ~/.ssh/known_hosts
RUN --mount=type=ssh ssh -q -T git@gitlab.com 2>&1 | tee /hello
# "Welcome to GitLab, @GITLAB_USERNAME_ASSOCIATED_WITH_SSHKEY" should be printed here
# with the type of build progress is defined as `plain`.
# 这里应打印 "Welcome to GitLab, @GITLAB_USERNAME_ASSOCIATED_WITH_SSHKEY"
# 构建进度类型定义为 `plain`。
```



```console
$ eval $(ssh-agent)
$ ssh-add ~/.ssh/id_rsa
(Input your passphrase here)
$ docker buildx build --ssh default=$SSH_AUTH_SOCK .
```

### 为镜像打标签 (`-t`, `--tag`) Tag an image (`-t`, `--tag`)



```console
$ docker buildx build -t docker/apache:2.0 .
```

This examples builds in the same way as the previous example, but it then tags the resulting image. The repository name will be `docker/apache` and the tag `2.0`.

​	这个示例与上一个示例的构建方式相同，但会为生成的镜像打标签。仓库名为 `docker/apache`，标签为 `2.0`。

[Read more about valid tags]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimagetag" >}}).

​	[了解更多有效标签]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimagetag" >}})。

You can apply multiple tags to an image. For example, you can apply the `latest` tag to a newly built image and add another tag that references a specific version.

​	可以为镜像应用多个标签。例如，可以为新构建的镜像应用 `latest` 标签，并添加另一个特定版本的标签。

For example, to tag an image both as `docker/fedora-jboss:latest` and `docker/fedora-jboss:v2.1`, use the following:

​	例如，要将镜像标记为 `docker/fedora-jboss:latest` 和 `docker/fedora-jboss:v2.1`，使用以下命令：



```console
$ docker buildx build -t docker/fedora-jboss:latest -t docker/fedora-jboss:v2.1 .
```

### 指定目标构建阶段 (`--target`) Specifying target build stage (`--target`)

When building a Dockerfile with multiple build stages, use the `--target` option to specify an intermediate build stage by name as a final stage for the resulting image. The builder skips commands after the target stage.

​	构建包含多个阶段的 Dockerfile 时，使用 `--target` 选项指定一个中间阶段作为生成镜像的最终阶段。构建器会跳过目标阶段之后的命令。

```dockerfile
FROM debian AS build-env
# ...

FROM alpine AS production-env
# ...
```



```console
$ docker buildx build -t mybuildimage --target build-env .
```

### 设置 ulimits (`--ulimit`) Set ulimits (`--ulimit`)

`--ulimit` overrides the default ulimits of build's containers when using `RUN` instructions and are specified with a soft and hard limit as such: `<type>=<soft limit>[:<hard limit>]`, for example:

​	`--ulimit` 覆盖构建容器使用 `RUN` 指令时的默认 ulimits，指定格式为 `<type>=<soft limit>[:<hard limit>]`，例如：

```console
$ docker buildx build --ulimit nofile=1024:1024 .
```

> **Note**
>
> If you don't provide a `hard limit`, the `soft limit` is used for both values. If no `ulimits` are set, they're inherited from the default `ulimits` set on the daemon.
>
> ​	如果未提供 `hard limit`，则使用 `soft limit` 作为两个值。如果未设置 `ulimits`，则继承守护进程上设置的默认 `ulimits`。

> **Note**
>
> In most cases, it is recommended to let the builder automatically determine the appropriate configurations. Manual adjustments should only be considered when specific performance tuning is required for complex build scenarios.
>
> ​	在大多数情况下，建议让构建器自动确定适当的配置。仅在需要为复杂的构建场景进行特定性能调整时考虑手动调整。
