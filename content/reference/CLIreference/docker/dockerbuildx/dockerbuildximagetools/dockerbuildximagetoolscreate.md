+++
title = "docker buildx imagetools create"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx imagetools create

| Description | Create a new image based on source images                    |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker buildx imagetools create [OPTIONS] [SOURCE] [SOURCE...]` |

## Description

Create a new manifest list based on source manifests. The source manifests can be manifest lists or single platform distribution manifests and must already exist in the registry where the new manifest is created.

​	创建一个新的清单列表，基于源清单。源清单可以是清单列表或单一平台分发清单，必须已经存在于新清单创建的注册表中。

If only one source is specified and that source is a manifest list or image index, create performs a carbon copy. If one source is specified and that source is *not* a list or index, the output will be a manifest list, however you can disable this behavior with `--prefer-index=false` which attempts to preserve the source manifest format in the output.

​	如果只指定一个源且该源是清单列表或镜像索引，则执行完全复制。如果指定了一个源且该源**不是**列表或索引，则输出将是清单列表，不过您可以通过 `--prefer-index=false` 禁用此行为，以尝试在输出中保留源清单的格式。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`--annotation`](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#annotation) |         | 向镜像添加注释 Add annotation to the image                   |
| [`--append`](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#append) |         | 追加到现有清单 Append to existing manifest                   |
| [`--dry-run`](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#dry-run) |         | 显示最终镜像而不推送 Show final image instead of pushing     |
| [`-f, --file`](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#file) |         | 从文件读取源描述符 Read source descriptor from file          |
| `--prefer-index`                                             | `true`  | 当仅指定一个源时，优先输出镜像索引或清单列表，而不是执行完全复制 When only a single source is specified, prefer outputting an image index or manifest list instead of performing a carbon copy |
| `--progress`                                                 | `auto`  | 设置进度输出的类型（`auto`, `plain`, `tty`, `rawjson`）。使用 plain 显示容器输出 Set type of progress output (`auto`, `plain`, `tty`, `rawjson`). Use plain to show container output |
| [`-t, --tag`](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#tag) |         | 设置新镜像的引用 Set reference for new image                 |

## Examples

### 向镜像添加注释 (`--annotation`) Add annotations to an image (`--annotation`)

The `--annotation` flag lets you add annotations the image index, manifest, and descriptors when creating a new image.

​	`--annotation` 标志允许您在创建新镜像时向镜像索引、清单和描述符添加注释。

The following command creates a `foo/bar:latest` image with the `org.opencontainers.image.authors` annotation on the image index.

​	以下命令创建了一个带有 `org.opencontainers.image.authors` 注释的 `foo/bar:latest` 镜像。



```console
$ docker buildx imagetools create \
  --annotation "index:org.opencontainers.image.authors=dvdksn" \
  --tag foo/bar:latest \
  foo/bar:alpha foo/bar:beta foo/bar:gamma
```

> **Note**
>
> The `imagetools create` command supports adding annotations to the image index and descriptor, using the following type prefixes:
>
> ​	`imagetools create` 命令支持在镜像索引和描述符上添加注释，使用以下类型前缀：
>
> - `index:`
>
> - `manifest-descriptor:`
>
> It doesn't support annotating manifests or OCI layouts.
>
> ​	不支持对清单或 OCI 布局进行注释。

For more information about annotations, see [Annotations](https://docs.docker.com/build/building/annotations/).

​	有关注释的更多信息，请参见 [Annotations](https://docs.docker.com/build/building/annotations/)。

### 向现有清单列表追加新源 (`--append`) Append new sources to an existing manifest list (`--append`)

Use the `--append` flag to append the new sources to an existing manifest list in the destination.

​	使用 `--append` 标志向目标中的现有清单列表追加新源。

### 重写配置的构建器实例 (`--builder`) Override the configured builder instance (`--builder`)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).

​	与 [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder) 相同。

### 显示最终镜像而不推送 (`--dry-run`) Show final image instead of pushing (`--dry-run`)

Use the `--dry-run` flag to not push the image, just show it.

​	使用 `--dry-run` 标志可以不推送镜像，仅显示镜像。

### 从文件读取源描述符 (`-f`, `--file`) Read source descriptor from a file (`-f`, `--file`)



```text
-f FILE or --file FILE
```

Reads source from files. A source can be a manifest digest, manifest reference, or a JSON of OCI descriptor object.

​	从文件读取源。源可以是清单摘要、清单引用或 OCI 描述符对象的 JSON。

In order to define annotations or additional platform properties like `os.version` and `os.features` you need to add them in the OCI descriptor object encoded in JSON.

​	为了定义注释或其他平台属性，如 `os.version` 和 `os.features`，您需要在 JSON 编码的 OCI 描述符对象中添加它们。



```console
$ docker buildx imagetools inspect --raw alpine | jq '.manifests[0] | .platform."os.version"="10.1"' > descr.json
$ docker buildx imagetools create -f descr.json myuser/image
```

The descriptor in the file is merged with existing descriptor in the registry if it exists.

​	文件中的描述符将与注册表中现有的描述符合并（如果存在）。

The supported fields for the descriptor are defined in [OCI spec](https://github.com/opencontainers/image-spec/blob/master/descriptor.md#properties) .

​	支持的描述符字段在 [OCI 规范](https://github.com/opencontainers/image-spec/blob/master/descriptor.md#properties) 中定义。

### 设置新镜像的引用 (`-t, --tag`) Set reference for new image (-t, --tag)



```text
-t IMAGE or --tag IMAGE
```

Use the `-t` or `--tag` flag to set the name of the image to be created.

​	使用 `-t` 或 `--tag` 标志设置要创建的镜像的名称。

```console
$ docker buildx imagetools create --dry-run alpine@sha256:5c40b3c27b9f13c873fefb2139765c56ce97fd50230f1f2d5c91e55dec171907 sha256:c4ba6347b0e4258ce6a6de2401619316f982b7bcc529f73d2a410d0097730204
$ docker buildx imagetools create -t tonistiigi/myapp -f image1 -f image2
```
