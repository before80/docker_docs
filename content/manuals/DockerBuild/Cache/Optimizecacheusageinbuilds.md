+++
title = "优化构建中的缓存使用"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/cache/optimize/](https://docs.docker.com/build/cache/optimize/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Optimize cache usage in builds - 优化构建中的缓存使用

When building with Docker, a layer is reused from the build cache if the instruction and the files it depends on hasn't changed since it was previously built. Reusing layers from the cache speeds up the build process because Docker doesn't have to rebuild the layer again.

​	在使用 Docker 构建时，如果指令及其依赖文件自上次构建以来未更改，则会从构建缓存中重用该层。重用缓存层加快了构建过程，因为 Docker 不需要重新构建该层。

Here are a few techniques you can use to optimize build caching and speed up the build process:

​	以下是一些优化构建缓存并加快构建过程的技术：

- [Order your layers](https://docs.docker.com/build/cache/optimize/#order-your-layers): Putting the commands in your Dockerfile into a logical order can help you avoid unnecessary cache invalidation.
  - [排列层顺序](https://docs.docker.com/build/cache/optimize/#order-your-layers)：将 Dockerfile 中的命令按逻辑顺序排列，避免不必要的缓存失效。

- [Keep the context small](https://docs.docker.com/build/cache/optimize/#keep-the-context-small): The context is the set of files and directories that are sent to the builder to process a build instruction. Keeping the context as small reduces the amount of data that needs to be sent to the builder, and reduces the likelihood of cache invalidation.
  - [保持上下文小巧](https://docs.docker.com/build/cache/optimize/#keep-the-context-small)：上下文是发送给构建器以处理构建指令的文件和目录集。保持上下文小巧可减少发送到构建器的数据量，降低缓存失效的可能性。

- [Use bind mounts](https://docs.docker.com/build/cache/optimize/#use-bind-mounts): Bind mounts let you mount a file or directory from the host machine into the build container. Using bind mounts can help you avoid unnecessary layers in the image, which can slow down the build process.
  - [使用绑定挂载](https://docs.docker.com/build/cache/optimize/#use-bind-mounts)：绑定挂载允许将主机上的文件或目录挂载到构建容器中。使用绑定挂载可以避免在镜像中添加不必要的层，减少构建时间。

- [Use cache mounts](https://docs.docker.com/build/cache/optimize/#use-cache-mounts): Cache mounts let you specify a persistent package cache to be used during builds. The persistent cache helps speed up build steps, especially steps that involve installing packages using a package manager. Having a persistent cache for packages means that even if you rebuild a layer, you only download new or changed packages.
  - [使用缓存挂载](https://docs.docker.com/build/cache/optimize/#use-cache-mounts)：缓存挂载允许在构建过程中使用持久的包缓存，特别适用于使用包管理器安装包的步骤。这样，即使重新构建层，也只需下载新包或变更的包。

- [Use an external cache](https://docs.docker.com/build/cache/optimize/#use-an-external-cache): An external cache lets you store build cache at a remote location. The external cache image can be shared between multiple builds, and across different environments.
  - [使用外部缓存](https://docs.docker.com/build/cache/optimize/#use-an-external-cache)：外部缓存允许在远程位置存储构建缓存。外部缓存镜像可以在多个构建和不同环境中共享。

## 排列层顺序 Order your layers

Putting the commands in your Dockerfile into a logical order is a great place to start. Because a change causes a rebuild for steps that follow, try to make expensive steps appear near the beginning of the Dockerfile. Steps that change often should appear near the end of the Dockerfile, to avoid triggering rebuilds of layers that haven't changed.

​	将 Dockerfile 中的命令按逻辑顺序排列是一个很好的起点。因为更改会导致后续步骤重新构建，因此尽量将耗时步骤放在 Dockerfile 的开头。频繁更改的步骤应放在 Dockerfile 的末尾，以避免触发未更改层的重建。

Consider the following example. A Dockerfile snippet that runs a JavaScript build from the source files in the current directory:

​	考虑以下示例，一个从当前目录中的源文件运行 JavaScript 构建的 Dockerfile 片段：



```dockerfile
# syntax=docker/dockerfile:1
FROM node
WORKDIR /app
COPY . .          # Copy over all files in the current directory 复制当前目录中的所有文件
RUN npm install   # Install dependencies 安装依赖
RUN npm build     # Run build 运行构建
```

This Dockerfile is rather inefficient. Updating any file causes a reinstall of all dependencies every time you build the Docker image even if the dependencies didn't change since last time.

​	这个 Dockerfile 比较低效。更新任何文件都会导致每次构建 Docker 镜像时重新安装所有依赖，即使依赖没有更改。

Instead, the `COPY` command can be split in two. First, copy over the package management files (in this case, `package.json` and `yarn.lock`). Then, install the dependencies. Finally, copy over the project source code, which is subject to frequent change.

​	可以将 `COPY` 命令分为两步。首先，复制包管理文件（如 `package.json` 和 `yarn.lock`），然后安装依赖，最后再复制项目源代码，这样频繁的更改不会触发依赖的重建。

```dockerfile
# syntax=docker/dockerfile:1
FROM node
WORKDIR /app
COPY package.json yarn.lock .    # Copy package management files 复制包管理文件
RUN npm install                  # Install dependencies 安装依赖
COPY . .                         # Copy over project files 复制项目文件
RUN npm build                    # Run build 运行构建
```

By installing dependencies in earlier layers of the Dockerfile, there is no need to rebuild those layers when a project file has changed.

​	通过在 Dockerfile 的早期层中安装依赖，当项目文件发生更改时无需重新构建这些层。

## 保持上下文小巧 Keep the context small

The easiest way to make sure your context doesn't include unnecessary files is to create a `.dockerignore` file in the root of your build context. The `.dockerignore` file works similarly to `.gitignore` files, and lets you exclude files and directories from the build context.

​	确保上下文不包含不必要的文件的最简单方法是在构建上下文的根目录创建一个 `.dockerignore` 文件。`.dockerignore` 文件类似于 `.gitignore`，允许你排除不需要的文件和目录。

Here's an example `.dockerignore` file that excludes the `node_modules` directory, all files and directories that start with `tmp`:

​	以下是一个排除了 `node_modules` 目录和所有以 `tmp` 开头的文件或目录的 `.dockerignore` 示例：

.dockerignore



```plaintext
node_modules
tmp*
```

Ignore-rules specified in the `.dockerignore` file apply to the entire build context, including subdirectories. This means it's a rather coarse-grained mechanism, but it's a good way to exclude files and directories that you know you don't need in the build context, such as temporary files, log files, and build artifacts.

​	`.dockerignore` 文件中指定的忽略规则适用于整个构建上下文，包括子目录。这是一种粗粒度的机制，但对于排除不需要的文件和目录非常有效，如临时文件、日志文件和构建产物。

## 使用绑定挂载 Use bind mounts

You might be familiar with bind mounts for when you run containers with `docker run` or Docker Compose. Bind mounts let you mount a file or directory from the host machine into a container.

​	你可能熟悉在使用 `docker run` 或 Docker Compose 运行容器时的绑定挂载。绑定挂载允许将主机上的文件或目录挂载到容器中。



```bash
# bind mount using the -v flag
# 使用 -v 标志进行绑定挂载
docker run -v $(pwd):/path/in/container image-name
# bind mount using the --mount flag
# 使用 --mount 标志进行绑定挂载
docker run --mount=type=bind,src=.,dst=/path/in/container image-name
```

To use bind mounts in a build, you can use the `--mount` flag with the `RUN` instruction in your Dockerfile:

​	在构建中使用绑定挂载，可以在 Dockerfile 的 `RUN` 指令中使用 `--mount` 标志：



```dockerfile
FROM golang:latest
WORKDIR /app
RUN --mount=type=bind,target=. go build -o /app/hello
```

In this example, the current directory is mounted into the build container before the `go build` command gets executed. The source code is available in the build container for the duration of that `RUN` instruction. When the instruction is done executing, the mounted files are not persisted in the final image, or in the build cache. Only the output of the `go build` command remains.

​	在此示例中，当前目录在执行 `go build` 命令之前被挂载到构建容器中。源代码在此 `RUN` 指令的执行期间可用。指令执行完成后，挂载的文件不会保留在最终镜像或构建缓存中，只有 `go build` 命令的输出被保留。

The `COPY` and `ADD` instructions in a Dockerfile lets you copy files from the build context into the build container. Using bind mounts is beneficial for build cache optimization because you're not adding unnecessary layers to the cache. If you have build context that's on the larger side, and it's only used to generate an artifact, you're better off using bind mounts to temporarily mount the source code required to generate the artifact into the build. If you use `COPY` to add the files to the build container, BuildKit will include all of those files in the cache, even if the files aren't used in the final image.

​	Dockerfile 中的 `COPY` 和 `ADD` 指令可以将文件从构建上下文复制到构建容器中。使用绑定挂载对构建缓存优化有好处，因为这样不会将不必要的层添加到缓存中。如果构建上下文较大且仅用于生成构件，使用绑定挂载将生成所需的源代码临时挂载到构建中会更高效。如果使用 `COPY` 将文件添加到构建容器中，即使这些文件未在最终镜像中使用，BuildKit 也会将所有这些文件包含在缓存中。

There are a few things to be aware of when using bind mounts in a build:

​	在构建中使用绑定挂载时需要注意以下几点：

- Bind mounts are read-only by default. If you need to write to the mounted directory, you need to specify the `rw` option. However, even with the `rw` option, the changes are not persisted in the final image or the build cache. The file writes are sustained for the duration of the `RUN` instruction, and are discarded after the instruction is done.
  - 绑定挂载默认是只读的。如果需要对挂载的目录进行写操作，需要指定 `rw` 选项。不过，即使使用 `rw` 选项，更改也不会保存在最终镜像或构建缓存中。文件写入仅在 `RUN` 指令执行期间有效，指令执行完成后这些更改会被丢弃。

- Mounted files are not persisted in the final image. Only the output of the `RUN` instruction is persisted in the final image. If you need to include files from the build context in the final image, you need to use the `COPY` or `ADD` instructions.
  - 挂载的文件不会保存在最终镜像中，只有 `RUN` 指令的输出被保存在最终镜像中。如果需要将构建上下文中的文件包含在最终镜像中，需使用 `COPY` 或 `ADD` 指令。

- If the target directory is not empty, the contents of the target directory are hidden by the mounted files. The original contents are restored after the `RUN` instruction is done.
  - 如果目标目录不为空，目标目录的内容将被挂载的文件隐藏，`RUN` 指令完成后原始内容会恢复。


## 使用缓存挂载 Use cache mounts

Regular cache layers in Docker correspond to an exact match of the instruction and the files it depends on. If the instruction and the files it depends on have changed since the layer was built, the layer is invalidated, and the build process has to rebuild the layer.

​	Docker 中的常规缓存层与指令及其依赖文件的精确匹配相对应。如果指令及其依赖文件自构建该层以来发生了更改，该层将失效，构建过程必须重新构建该层。

Cache mounts are a way to specify a persistent cache location to be used during builds. The cache is cumulative across builds, so you can read and write to the cache multiple times. This persistent caching means that even if you need to rebuild a layer, you only download new or changed packages. Any unchanged packages are reused from the cache mount.

​	缓存挂载是一种在构建期间指定持久缓存位置的方法。缓存在构建之间是累积的，因此可以多次读写缓存。这种持久缓存意味着即使需要重新构建某一层，您也只需下载新的或更改的包。未更改的包将从缓存挂载中重用。

To use cache mounts in a build, you can use the `--mount` flag with the `RUN` instruction in your Dockerfile:

​	在构建中使用缓存挂载，可以在 Dockerfile 中的 `RUN` 指令中使用 `--mount` 标志：



```dockerfile
FROM node:latest
WORKDIR /app
RUN --mount=type=cache,target=/root/.npm npm install
```

In this example, the `npm install` command uses a cache mount for the `/root/.npm` directory, the default location for the npm cache. The cache mount is persisted across builds, so even if you end up rebuilding the layer, you only download new or changed packages. Any changes to the cache are persisted across builds, and the cache is shared between multiple builds.

​	在此示例中，`npm install` 命令为 `/root/.npm` 目录（npm 缓存的默认位置）使用了一个缓存挂载。缓存挂载在构建之间是持久的，因此即使重新构建该层，也只会下载新的或更改的包。任何对缓存的更改都会在构建之间持久存在，并在多个构建之间共享。

How you specify cache mounts depends on the build tool you're using. If you're unsure how to specify cache mounts, refer to the documentation for the build tool you're using. Here are a few examples:

​	指定缓存挂载的方法取决于使用的构建工具。如果不确定如何指定缓存挂载，请参考所用构建工具的文档。以下是一些示例：

{{< tabpane text=true persist=disabled >}}

{{% tab header="Go " %}}

```dockerfile
RUN --mount=type=cache,target=/go/pkg/mod \
    go build -o /app/hello
```



{{% /tab  %}}

{{% tab header="Apt " %}}

```dockerfile
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
  --mount=type=cache,target=/var/lib/apt,sharing=locked \
  apt update && apt-get --no-install-recommends install -y gcc
```

{{% /tab  %}}

{{% tab header="Python " %}}

```dockerfile
RUN --mount=type=cache,target=/root/.cache/pip \
    pip install -r requirements.txt
```

{{% /tab  %}}

{{% tab header="Ruby " %}}

```dockerfile
RUN --mount=type=cache,target=/root/.gem \
    bundle install
```

{{% /tab  %}}

{{% tab header="Rust " %}}

```dockerfile
RUN --mount=type=cache,target=/app/target/ \
    --mount=type=cache,target=/usr/local/cargo/git/db \
    --mount=type=cache,target=/usr/local/cargo/registry/ \
    cargo build
```

{{% /tab  %}}

{{% tab header=".NET" %}}

```dockerfile
RUN --mount=type=cache,target=/root/.nuget/packages \
    dotnet restore
```

{{% /tab  %}}

{{% tab header="PHP" %}}

```dockerfile
RUN --mount=type=cache,target=/tmp/cache \
    composer install
```

{{% /tab  %}}

{{< /tabpane >}}



------



It's important that you read the documentation for the build tool you're using to make sure you're using the correct cache mount options. Package managers have different requirements for how they use the cache, and using the wrong options can lead to unexpected behavior. For example, Apt needs exclusive access to its data, so the caches use the option `sharing=locked` to ensure parallel builds using the same cache mount wait for each other and not access the same cache files at the same time.

​	请务必阅读所用构建工具的文档，以确保使用正确的缓存挂载选项。包管理器对缓存的使用有不同的要求，使用错误的选项可能会导致意外行为。例如，Apt 需要独占访问其数据，因此缓存使用 `sharing=locked` 选项，以确保使用相同缓存挂载的并行构建相互等待，而不是同时访问相同的缓存文件。

## 使用外部缓存 Use an external cache

The default cache storage for builds is internal to the builder (BuildKit instance) you're using. Each builder uses its own cache storage. When you switch between different builders, the cache is not shared between them. Using an external cache lets you define a remote location for pushing and pulling cache data.

​	默认情况下，构建的缓存存储在所用构建器（BuildKit 实例）的内部。每个构建器使用自己的缓存存储。当在不同的构建器之间切换时，缓存不会共享。使用外部缓存可以定义一个远程位置，用于推送和拉取缓存数据。

External caches are especially useful for CI/CD pipelines, where the builders are often ephemeral, and build minutes are precious. Reusing the cache between builds can drastically speed up the build process and reduce cost. You can even make use of the same cache in your local development environment.

​	外部缓存对于 CI/CD 管道尤其有用，因为这些管道中的构建器通常是短暂的，构建时间宝贵。在构建之间重用缓存可以大大加快构建过程并降低成本。您甚至可以在本地开发环境中使用相同的缓存。

To use an external cache, you specify the `--cache-to` and `--cache-from` options with the `docker buildx build` command.

​	要使用外部缓存，可以在 `docker buildx build` 命令中指定 `--cache-to` 和 `--cache-from` 选项。

- `--cache-to` exports the build cache to the specified location.
  - `--cache-to` 将构建缓存导出到指定位置。

- `--cache-from` specifies remote caches for the build to use.
  - `--cache-from` 指定构建所使用的远程缓存。


The following example shows how to set up a GitHub Actions workflow using `docker/build-push-action`, and push the build cache layers to an OCI registry image:

​	以下示例展示了如何使用 `docker/build-push-action` 设置 GitHub Actions 工作流，并将构建缓存层推送到 OCI 注册表镜像：

`.github/workflows/ci.yml`



```yaml
name: ci

on:
  push:

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      
      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      
      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          push: true
          tags: user/app:latest
          cache-from: type=registry,ref=user/app:buildcache
          cache-to: type=registry,ref=user/app:buildcache,mode=max
```

This setup tells BuildKit to look for cache in the `user/app:buildcache` image. And when the build is done, the new build cache is pushed to the same image, overwriting the old cache.

​	此设置指示 BuildKit 在 `user/app:buildcache` 镜像中查找缓存，并在构建完成后将新的构建缓存推送到相同的镜像中，覆盖旧的缓存。

This cache can be used locally as well. To pull the cache in a local build, you can use the `--cache-from` option with the `docker buildx build` command:

​	该缓存也可以在本地使用。要在本地构建中拉取缓存，可以在 `docker buildx build` 命令中使用 `--cache-from` 选项：

```console
$ docker buildx build --cache-from type=registry,ref=user/app:buildcache .
```

## 总结 Summary

Optimizing cache usage in builds can significantly speed up the build process. Keeping the build context small, using bind mounts, cache mounts, and external caches are all techniques you can use to make the most of the build cache and speed up the build process.

​	在构建中优化缓存使用可以显著加快构建过程。保持构建上下文小、使用绑定挂载、缓存挂载和外部缓存，都是可以充分利用构建缓存并加快构建过程的技术。

For more information about the concepts discussed in this guide, see:

​	有关本指南中讨论的概念的更多信息，请参阅：

- [.dockerignore files](https://docs.docker.com/build/concepts/context/#dockerignore-files)
  - .dockerignore 文件
- [Cache invalidation]({{< ref "/manuals/DockerBuild/Cache/Buildcacheinvalidation" >}})
  - 缓存失效
- [Cache mounts](https://docs.docker.com/reference/dockerfile/#run---mounttypecache)
  - 缓存挂载
- [Cache backend types]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends" >}})
  - 缓存后端类型
- [Building best practices]({{< ref "/manuals/DockerBuild/Building/Bestpractices" >}})
  - 构建最佳实践
