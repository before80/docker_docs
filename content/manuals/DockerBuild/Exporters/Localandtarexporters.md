+++
title = "本地和 tar 导出器"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/exporters/local-tar/](https://docs.docker.com/build/exporters/local-tar/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Local and tar exporters - 本地和 tar 导出器

The `local` and `tar` exporters output the root filesystem of the build result into a local directory. They're useful for producing artifacts that aren't container images.

​	`local` 和 `tar` 导出器将构建结果的根文件系统输出到本地目录。它们适用于生成不是容器镜像的工件。

- `local` exports files and directories.

  - `local` 导出文件和目录。

- `tar` exports the same, but bundles the export into a tarball.

  - `tar` 导出相同内容，但将导出打包到 tarball 中。

  

## 概要 Synopsis

Build a container image using the `local` exporter:

​	使用 `local` 导出器构建容器镜像：



```console
$ docker buildx build --output type=local[,parameters] .
$ docker buildx build --output type=tar[,parameters] .
```

The following table describes the available parameters:

​	下表描述了可用的参数：

| Parameter | Type   | Default | Description                           |
| --------- | ------ | ------- | ------------------------------------- |
| `dest`    | String |         | 复制文件的路径  Path to copy files to |

## Further reading

For more information on the `local` or `tar` exporters, see the [BuildKit README](https://github.com/moby/buildkit/blob/master/README.md#local-directory).

​	有关 `local` 或 `tar` 导出器的更多信息，请参阅 [BuildKit README](https://github.com/moby/buildkit/blob/master/README.md#local-directory)。

