+++
title = "构建缓存失效"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/cache/invalidation/](https://docs.docker.com/build/cache/invalidation/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Build cache invalidation - 构建缓存失效

When building an image, Docker steps through the instructions in your Dockerfile, executing each in the order specified. For each instruction, the [builder]({{< ref "/manuals/DockerBuild/Builders" >}}) checks whether it can reuse the instruction from the build cache.

​	在构建镜像时，Docker 会按照 Dockerfile 中指定的顺序依次执行每条指令。对于每条指令，[构建器]({{< ref "/manuals/DockerBuild/Builders" >}})会检查是否可以从构建缓存中重用该指令。

## 通用规则 General rules

The basic rules of build cache invalidation are as follows:

​	构建缓存失效的基本规则如下：

- The builder begins by checking if the base image is already cached. Each subsequent instruction is compared against the cached layers. If no cached layer matches the instruction exactly, the cache is invalidated.

  - 构建器首先检查基础镜像是否已被缓存。每个后续指令都与缓存的层进行比较。如果没有缓存层完全匹配该指令，则缓存失效。

- In most cases, comparing the Dockerfile instruction with the corresponding cached layer is sufficient. However, some instructions require additional checks and explanations.

  - 通常，将 Dockerfile 指令与对应的缓存层进行比较就足够了。然而，有些指令需要额外的检查和说明。

- For the `ADD` and `COPY` instructions, and for `RUN` instructions with bind mounts (`RUN --mount=type=bind`), the builder calculates a cache checksum from file metadata to determine whether cache is valid. During cache lookup, cache is invalidated if the file metadata has changed for any of the files involved.

  - 对于 `ADD` 和 `COPY` 指令，以及带绑定挂载的 `RUN` 指令（`RUN --mount=type=bind`），构建器会通过文件元数据计算缓存校验和，以确定缓存是否有效。在缓存查找过程中，如果任何相关文件的元数据发生更改，缓存就会失效。


  The modification time of a file (`mtime`) is not taken into account when calculating the cache checksum. If only the `mtime` of the copied files have changed, the cache is not invalidated.

  ​	在计算缓存校验和时，不会考虑文件的修改时间（`mtime`）。如果仅复制文件的 `mtime` 发生了更改，则缓存不会失效。

- Aside from the `ADD` and `COPY` commands, cache checking doesn't look at the files in the container to determine a cache match. For example, when processing a `RUN apt-get -y update` command the files updated in the container aren't examined to determine if a cache hit exists. In that case just the command string itself is used to find a match.

  - 除了 `ADD` 和 `COPY` 命令之外，缓存检查不会查看容器中的文件来确定缓存匹配。例如，在处理 `RUN apt-get -y update` 命令时，不会检查容器中更新的文件以确定是否存在缓存匹配。在这种情况下，仅使用命令字符串本身来查找匹配项。


Once the cache is invalidated, all subsequent Dockerfile commands generate new images and the cache isn't used.

​	一旦缓存失效，所有后续的 Dockerfile 命令将生成新镜像，并且不再使用缓存。

If your build contains several layers and you want to ensure the build cache is reusable, order the instructions from less frequently changed to more frequently changed where possible.

​	如果构建包含多个层，并且希望确保构建缓存可以重复使用，请尽可能将更不频繁更改的指令放在前面。

## RUN 指令 RUN instructions

The cache for `RUN` instructions isn't invalidated automatically between builds. Suppose you have a step in your Dockerfile to install `curl`:

​	`RUN` 指令的缓存不会在构建之间自动失效。假设在 Dockerfile 中有一个安装 `curl` 的步骤：



```dockerfile
FROM alpine:3.20 AS install
RUN apk add curl
```

This doesn't mean that the version of `curl` in your image is always up-to-date. Rebuilding the image one week later will still get you the same packages as before. To force a re-execution of the `RUN` instruction, you can:

​	这并不意味着镜像中的 `curl` 版本总是最新的。即使在一周后重新构建镜像，仍然会得到相同的包。要强制重新执行 `RUN` 指令，可以：

- Make sure that a layer before it has changed
  - 确保它之前的层已更改

- Clear the build cache ahead of the build using [`docker builder prune`]({{< ref "/reference/CLIreference/docker/dockerbuilder/dockerbuilderprune" >}})
  - 在构建之前使用 [`docker builder prune`]({{< ref "/reference/CLIreference/docker/dockerbuilder/dockerbuilderprune" >}}) 清除构建缓存

- Use the `--no-cache` or `--no-cache-filter` options
  - 使用 `--no-cache` 或 `--no-cache-filter` 选项


The `--no-cache-filter` option lets you specify a specific build stage to invalidate the cache for:

​	`--no-cache-filter` 选项允许指定特定的构建阶段来使缓存失效：

```console
$ docker build --no-cache-filter install .
```

## 构建密钥 Build secrets

The contents of build secrets are not part of the build cache. Changing the value of a secret doesn't result in cache invalidation.

​	构建密钥的内容不属于构建缓存的一部分。更改密钥的值不会导致缓存失效。

If you want to force cache invalidation after changing a secret value, you can pass a build argument with an arbitrary value that you also change when changing the secret. Build arguments do result in cache invalidation.

​	如果希望在更改密钥后强制缓存失效，可以传递一个带有任意值的构建参数，并在更改密钥时也更改该参数。构建参数会导致缓存失效。

```dockerfile
FROM alpine
ARG CACHEBUST
RUN --mount=type=secret,id=TOKEN,env=TOKEN \
    some-command ...
```



```console
$ TOKEN="tkn_pat123456" docker build --secret id=TOKEN --build-arg CACHEBUST=1 .
```

Properties of secrets such as IDs and mount paths do participate in the cache checksum, and result in cache invalidation if changed.

​	密钥的属性（如 ID 和挂载路径）会参与缓存校验和，如果更改，则会导致缓存失效。
