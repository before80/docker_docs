+++
title = "自定义 Dockerfile 语法"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/buildkit/frontend/](https://docs.docker.com/build/buildkit/frontend/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Custom Dockerfile syntax - 自定义 Dockerfile 语法

## Dockerfile 前端 Dockerfile frontend

BuildKit supports loading frontends dynamically from container images. To use an external Dockerfile frontend, the first line of your [Dockerfile]({{< ref "/reference/Dockerfilereference" >}}) needs to set the [`syntax` directive](https://docs.docker.com/reference/dockerfile/#syntax) pointing to the specific image you want to use:

​	BuildKit 支持从容器镜像动态加载前端。要使用外部 Dockerfile 前端，您的 [Dockerfile]({{< ref "/reference/Dockerfilereference" >}}) 的第一行需要设置 [`syntax` 指令](https://docs.docker.com/reference/dockerfile/#syntax)，指向要使用的特定镜像：



```dockerfile
# syntax=[remote image reference]
```

For example:



```dockerfile
# syntax=docker/dockerfile:1
# syntax=docker.io/docker/dockerfile:1
# syntax=example.com/user/repo:tag@sha256:abcdef...
```

You can also use the predefined `BUILDKIT_SYNTAX` build argument to set the frontend image reference on the command line:

​	您还可以使用预定义的 `BUILDKIT_SYNTAX` 构建参数在命令行上设置前端镜像引用：



```console
$ docker build --build-arg BUILDKIT_SYNTAX=docker/dockerfile:1 .
```

This defines the location of the Dockerfile syntax that is used to build the Dockerfile. The BuildKit backend allows seamlessly using external implementations that are distributed as Docker images and execute inside a container sandbox environment.

​	这定义了用于构建 Dockerfile 的语法位置。BuildKit 后端允许无缝地使用作为 Docker 镜像分发的外部实现，并在容器沙盒环境中执行。

Custom Dockerfile implementations allow you to:

​	自定义 Dockerfile 实现允许您：

- Automatically get bug fixes without updating the Docker daemon
  - 自动获取错误修复，而无需更新 Docker 守护进程

- Make sure all users are using the same implementation to build your Dockerfile
  - 确保所有用户使用相同的实现来构建您的 Dockerfile

- Use the latest features without updating the Docker daemon
  - 在不更新 Docker 守护进程的情况下使用最新功能

- Try out new features or third-party features before they are integrated in the Docker daemon
  - 在新功能或第三方功能集成到 Docker 守护进程之前进行试用

- Use [alternative build definitions, or create your own](https://github.com/moby/buildkit#exploring-llb)
  - 使用 [替代构建定义或创建您自己的定义](https://github.com/moby/buildkit#exploring-llb)

- Build your own Dockerfile frontend with custom features
  - 构建具有自定义功能的 Dockerfile 前端


> **Note**
>
> 
>
> BuildKit ships with a built-in Dockerfile frontend, but it's recommended to use an external image to make sure that all users use the same version on the builder and to pick up bug fixes automatically without waiting for a new version of BuildKit or Docker Engine.
>
> ​	BuildKit 自带一个内置的 Dockerfile 前端，但建议使用外部镜像，以确保所有用户在构建器上使用相同的版本，并在不等待新的 BuildKit 或 Docker 引擎版本的情况下自动获取错误修复。

## 官方版本 Official releases

Docker distributes official versions of the images that can be used for building Dockerfiles under `docker/dockerfile` repository on Docker Hub. There are two channels where new images are released: `stable` and `labs`.

​	Docker 在 Docker Hub 的 `docker/dockerfile` 仓库中发布了可以用于构建 Dockerfile 的官方镜像版本。新镜像在 `stable` 和 `labs` 两个渠道发布。

### Stable 渠道 Stable channel

The `stable` channel follows [semantic versioning](https://semver.org/). For example:

​	`stable` 渠道遵循 [语义版本控制](https://semver.org/)。例如：

- `docker/dockerfile:1` - kept updated with the latest `1.x.x` minor *and* patch release.
  - `docker/dockerfile:1` - 始终保持更新到最新的 `1.x.x` 次要和修补程序版本。

- `docker/dockerfile:1.2` - kept updated with the latest `1.2.x` patch release, and stops receiving updates once version `1.3.0` is released.
  - `docker/dockerfile:1.2` - 始终更新到最新的 `1.2.x` 修补程序版本，直到 `1.3.0` 发布。

- `docker/dockerfile:1.2.1` - immutable: never updated.
  - `docker/dockerfile:1.2.1` - 不变，永不更新。


We recommend using `docker/dockerfile:1`, which always points to the latest stable release of the version 1 syntax, and receives both "minor" and "patch" updates for the version 1 release cycle. BuildKit automatically checks for updates of the syntax when performing a build, making sure you are using the most current version.

​	推荐使用 `docker/dockerfile:1`，它总是指向语法版本 1 的最新稳定版本，并在版本 1 发布周期内接收“次要”和“修补”更新。BuildKit 在执行构建时会自动检查语法更新，确保您使用的是最新版本。

If a specific version is used, such as `1.2` or `1.2.1`, the Dockerfile needs to be updated manually to continue receiving bugfixes and new features. Old versions of the Dockerfile remain compatible with the new versions of the builder.

​	如果使用特定版本（如 `1.2` 或 `1.2.1`），需要手动更新 Dockerfile 以继续接收错误修复和新功能。旧版本的 Dockerfile 仍然兼容新版本的构建器。

### Labs 渠道 Labs channel

The `labs` channel provides early access to Dockerfile features that are not yet available in the `stable` channel. `labs` images are released at the same time as stable releases, and follow the same version pattern, but use the `-labs` suffix, for example:

​	`labs` 渠道提供尚未在 `stable` 渠道中提供的 Dockerfile 功能的早期访问。`labs` 镜像与稳定版本同时发布，遵循相同的版本模式，但带有 `-labs` 后缀，例如：

- `docker/dockerfile:labs` - latest release on `labs` channel.
  - `docker/dockerfile:labs` - `labs` 渠道的最新发布。

- `docker/dockerfile:1-labs` - same as `dockerfile:1`, with experimental features enabled.
  - `docker/dockerfile:1-labs` - 与 `dockerfile:1` 相同，但启用了实验功能。

- `docker/dockerfile:1.2-labs` - same as `dockerfile:1.2`, with experimental features enabled.
  - `docker/dockerfile:1.2-labs` - 与 `dockerfile:1.2` 相同，但启用了实验功能。

- `docker/dockerfile:1.2.1-labs` - immutable: never updated. Same as `dockerfile:1.2.1`, with experimental features enabled.
  - `docker/dockerfile:1.2.1-labs` - 不变，永不更新。与 `dockerfile:1.2.1` 相同，但启用了实验功能。


Choose a channel that best fits your needs. If you want to benefit from new features, use the `labs` channel. Images in the `labs` channel contain all the features in the `stable` channel, plus early access features. Stable features in the `labs` channel follow [semantic versioning](https://semver.org/), but early access features don't, and newer releases may not be backwards compatible. Pin the version to avoid having to deal with breaking changes.

​	根据需要选择适合的渠道。如果您希望使用新功能，请使用 `labs` 渠道。`labs` 渠道中的镜像包含 `stable` 渠道中的所有功能，并且提供早期访问的功能。`labs` 渠道中的稳定功能遵循 [语义版本控制](https://semver.org/)，但早期访问的功能不遵循，更新的版本可能不向后兼容。固定版本以避免处理破坏性更改。

## 其他资源 Other resources

For documentation on `labs` features, master builds, and nightly feature releases, refer to the description in [the BuildKit source repository on GitHub](https://github.com/moby/buildkit/blob/master/README.md). For a full list of available images, visit the [`docker/dockerfile` repository on Docker Hub](https://hub.docker.com/r/docker/dockerfile), and the [`docker/dockerfile-upstream` repository on Docker Hub](https://hub.docker.com/r/docker/dockerfile-upstream) for development builds.

​	有关 `labs` 功能、主分支构建和夜间功能发布的文档，请参阅 [GitHub 上的 BuildKit 源代码仓库描述](https://github.com/moby/buildkit/blob/master/README.md)。完整的可用镜像列表，请访问 [Docker Hub 上的 `docker/dockerfile` 仓库](https://hub.docker.com/r/docker/dockerfile) 和 [Docker Hub 上的 `docker/dockerfile-upstream` 仓库](https://hub.docker.com/r/docker/dockerfile-upstream)，用于开发构建。

