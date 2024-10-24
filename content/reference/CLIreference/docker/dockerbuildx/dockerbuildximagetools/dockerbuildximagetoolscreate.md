+++
title = "docker buildx imagetools create"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx imagetools create

| Description | Create a new image based on source images                    |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker buildx imagetools create [OPTIONS] [SOURCE] [SOURCE...]` |

## [Description](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#description)

Create a new manifest list based on source manifests. The source manifests can be manifest lists or single platform distribution manifests and must already exist in the registry where the new manifest is created.

If only one source is specified and that source is a manifest list or image index, create performs a carbon copy. If one source is specified and that source is *not* a list or index, the output will be a manifest list, however you can disable this behavior with `--prefer-index=false` which attempts to preserve the source manifest format in the output.

## [Options](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#options)

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`--annotation`](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#annotation) |         | Add annotation to the image                                  |
| [`--append`](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#append) |         | Append to existing manifest                                  |
| [`--dry-run`](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#dry-run) |         | Show final image instead of pushing                          |
| [`-f, --file`](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#file) |         | Read source descriptor from file                             |
| `--prefer-index`                                             | `true`  | When only a single source is specified, prefer outputting an image index or manifest list instead of performing a carbon copy |
| `--progress`                                                 | `auto`  | Set type of progress output (`auto`, `plain`, `tty`, `rawjson`). Use plain to show container output |
| [`-t, --tag`](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#tag) |         | Set reference for new image                                  |

## [Examples](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#examples)

### [Add annotations to an image (--annotation)](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#annotation)

The `--annotation` flag lets you add annotations the image index, manifest, and descriptors when creating a new image.

The following command creates a `foo/bar:latest` image with the `org.opencontainers.image.authors` annotation on the image index.



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
> - `index:`
> - `manifest-descriptor:`
>
> It doesn't support annotating manifests or OCI layouts.

For more information about annotations, see [Annotations](https://docs.docker.com/build/building/annotations/).

### [Append new sources to an existing manifest list (--append)](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#append)

Use the `--append` flag to append the new sources to an existing manifest list in the destination.

### [Override the configured builder instance (--builder)](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#builder)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).

### [Show final image instead of pushing (--dry-run)](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#dry-run)

Use the `--dry-run` flag to not push the image, just show it.

### [Read source descriptor from a file (-f, --file)](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#file)



```text
-f FILE or --file FILE
```

Reads source from files. A source can be a manifest digest, manifest reference, or a JSON of OCI descriptor object.

In order to define annotations or additional platform properties like `os.version` and `os.features` you need to add them in the OCI descriptor object encoded in JSON.



```console
$ docker buildx imagetools inspect --raw alpine | jq '.manifests[0] | .platform."os.version"="10.1"' > descr.json
$ docker buildx imagetools create -f descr.json myuser/image
```

The descriptor in the file is merged with existing descriptor in the registry if it exists.

The supported fields for the descriptor are defined in [OCI spec](https://github.com/opencontainers/image-spec/blob/master/descriptor.md#properties) .

### [Set reference for new image (-t, --tag)](https://docs.docker.com/reference/cli/docker/buildx/imagetools/create/#tag)



```text
-t IMAGE or --tag IMAGE
```

Use the `-t` or `--tag` flag to set the name of the image to be created.



```console
$ docker buildx imagetools create --dry-run alpine@sha256:5c40b3c27b9f13c873fefb2139765c56ce97fd50230f1f2d5c91e55dec171907 sha256:c4ba6347b0e4258ce6a6de2401619316f982b7bcc529f73d2a410d0097730204
$ docker buildx imagetools create -t tonistiigi/myapp -f image1 -f image2
```
