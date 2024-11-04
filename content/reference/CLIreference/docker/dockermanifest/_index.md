+++
title = "docker manifest"
date = 2024-10-23T14:54:43+08:00
weight = 140
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/manifest/](https://docs.docker.com/reference/cli/docker/manifest/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker manifest

| Description | Manage Docker image manifests and manifest lists |
| :---------- | ------------------------------------------------ |
| Usage       | `docker manifest COMMAND`                        |

**Experimental**

**This command is experimental.**

Experimental features are intended for testing and feedback as their functionality or design may change between releases without warning or can be removed entirely in a future release.

## Description

The `docker manifest` command by itself performs no action. In order to operate on a manifest or manifest list, one of the subcommands must be used.

​	`docker manifest` 命令本身不执行任何操作。若要操作一个清单或清单列表，必须使用其子命令。

A single manifest is information about an image, such as layers, size, and digest. The `docker manifest` command also gives you additional information, such as the OS and architecture an image was built for.

​	单个清单是关于镜像的信息，例如层、大小和摘要。`docker manifest` 命令还提供额外的信息，如镜像构建的操作系统和架构。

A manifest list is a list of image layers that is created by specifying one or more (ideally more than one) image names. It can then be used in the same way as an image name in `docker pull` and `docker run` commands, for example.

​	清单列表是一个镜像层列表，通过指定一个或多个（理想情况下是多个）镜像名称来创建。它可以在 `docker pull` 和 `docker run` 命令中像镜像名称一样使用。

Ideally a manifest list is created from images that are identical in function for different os/arch combinations. For this reason, manifest lists are often referred to as "multi-arch images". However, a user could create a manifest list that points to two images -- one for Windows on AMD64, and one for Darwin on AMD64.

​	理想情况下，清单列表是由功能相同的、针对不同操作系统/架构组合的镜像组成的。因此，清单列表通常称为“多架构镜像”。然而，用户也可以创建一个清单列表指向两个镜像——例如一个用于 AMD64 的 Windows，另一个用于 AMD64 的 Darwin。

### manifest inspect



```console
$ docker manifest inspect --help

Usage:  docker manifest inspect [OPTIONS] [MANIFEST_LIST] MANIFEST

Display an image manifest, or manifest list 显示镜像清单或清单列表

Options:
      --help       Print usage 显示用法
      --insecure   Allow communication with an insecure registry 允许与不安全的注册表通信
  -v, --verbose    Output additional info including layers and platform 输出额外的信息，包括层和平台
```

### manifest create



```console
Usage:  docker manifest create MANIFEST_LIST MANIFEST [MANIFEST...]

Create a local manifest list for annotating and pushing to a registry
创建用于标注和推送到注册表的本地清单列表

Options:
  -a, --amend      Amend an existing manifest list 修改现有的清单列表
      --insecure   Allow communication with an insecure registry 允许与不安全的注册表通信
      --help       Print usage 显示用法
```

### manifest annotate



```console
Usage:  docker manifest annotate [OPTIONS] MANIFEST_LIST MANIFEST

Add additional information to a local image manifest
	为本地镜像清单添加附加信息

Options:
      --arch string               Set architecture 设置架构
      --help                      Print usage 显示用法
      --os string                 Set operating system 设置操作系统
      --os-version string         Set operating system version 设置操作系统版本
      --os-features stringSlice   Set operating system feature 设置操作系统特性
      --variant string            Set architecture variant 设置架构变体
```

### manifest push



```console
Usage:  docker manifest push [OPTIONS] MANIFEST_LIST

Push a manifest list to a repository
将清单列表推送到存储库

Options:
      --help       Print usage 显示用法
      --insecure   Allow push to an insecure registry 允许推送到不安全的注册表
  -p, --purge      Remove the local manifest list after push 推送后删除本地清单列表
```

### 使用不安全的注册表 Working with insecure registries

The manifest command interacts solely with a registry. Because of this, it has no way to query the engine for the list of allowed insecure registries. To allow the CLI to interact with an insecure registry, some `docker manifest` commands have an `--insecure` flag. For each transaction, such as a `create`, which queries a registry, the `--insecure` flag must be specified. This flag tells the CLI that this registry call may ignore security concerns like missing or self-signed certificates. Likewise, on a `manifest push` to an insecure registry, the `--insecure` flag must be specified. If this is not used with an insecure registry, the manifest command fails to find a registry that meets the default requirements.

​	manifest 命令只与注册表交互。由于此原因，它无法从引擎查询允许的不安全注册表列表。为了允许 CLI 与不安全的注册表交互，某些 `docker manifest` 命令具有 `--insecure` 标志。对于每个事务，例如 `create` 之类查询注册表的操作，必须指定 `--insecure` 标志。此标志告诉 CLI 可以忽略缺少或自签名证书等安全问题。同样，在将清单推送到不安全的注册表时，也必须指定 `--insecure` 标志。如果未在不安全注册表上使用此标志，manifest 命令将无法找到满足默认要求的注册表。

## Examples

### 检查镜像的清单对象 Inspect an image's manifest object



```console
$ docker manifest inspect hello-world
{
        "schemaVersion": 2,
        "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
        "config": {
                "mediaType": "application/vnd.docker.container.image.v1+json",
                "size": 1520,
                "digest": "sha256:1815c82652c03bfd8644afda26fb184f2ed891d921b20a0703b46768f9755c57"
        },
        "layers": [
                {
                        "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
                        "size": 972,
                        "digest": "sha256:b04784fba78d739b526e27edc02a5a8cd07b1052e9283f5fc155828f4b614c28"
                }
        ]
}
```

### 检查镜像的清单并获取 os/arch 信息 Inspect an image's manifest and get the os/arch info

The `docker manifest inspect` command takes an optional `--verbose` flag that gives you the image's name (Ref), as well as the architecture and OS (Platform).

​	`docker manifest inspect` 命令接受一个可选的 `--verbose` 标志，提供镜像名称（Ref）、架构和操作系统（Platform）。

Just as with other Docker commands that take image names, you can refer to an image with or without a tag, or by digest (e.g. `hello-world@sha256:f3b3b28a45160805bb16542c9531888519430e9e6d6ffc09d72261b0d26ff74f`).

​	与其他 Docker 命令类似，可以使用不带标签的镜像名称、标签或摘要（例如 `hello-world@sha256:f3b3b28a45160805bb16542c9531888519430e9e6d6ffc09d72261b0d26ff74f`）来引用镜像。

Here is an example of inspecting an image's manifest with the `--verbose` flag:

​	以下示例显示了使用 `--verbose` 标志检查镜像清单：

```console
$ docker manifest inspect --verbose hello-world
{
        "Ref": "docker.io/library/hello-world:latest",
        "Digest": "sha256:f3b3b28a45160805bb16542c9531888519430e9e6d6ffc09d72261b0d26ff74f",
        "SchemaV2Manifest": {
                "schemaVersion": 2,
                "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
                "config": {
                        "mediaType": "application/vnd.docker.container.image.v1+json",
                        "size": 1520,
                        "digest": "sha256:1815c82652c03bfd8644afda26fb184f2ed891d921b20a0703b46768f9755c57"
                },
                "layers": [
                        {
                                "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
                                "size": 972,
                                "digest": "sha256:b04784fba78d739b526e27edc02a5a8cd07b1052e9283f5fc155828f4b614c28"
                        }
                ]
        },
        "Platform": {
                "architecture": "amd64",
                "os": "linux"
        }
}
```

### 创建并推送清单列表 Create and push a manifest list

To create a manifest list, you first `create` the manifest list locally by specifying the constituent images you would like to have included in your manifest list. Keep in mind that this is pushed to a registry, so if you want to push to a registry other than the docker registry, you need to create your manifest list with the registry name or IP and port. This is similar to tagging an image and pushing it to a foreign registry.

​	要创建清单列表，首先通过指定希望包含的镜像来本地 `create` 清单列表。此列表会推送到注册表，因此如果希望推送到 Docker 注册表以外的注册表，需要在创建清单列表时指定注册表名称或 IP 和端口。这与标记镜像并将其推送到外部注册表类似。

After you have created your local copy of the manifest list, you may optionally `annotate` it. Annotations allowed are the architecture and operating system (overriding the image's current values), os features, and an architecture variant.

​	创建清单列表的本地副本后，可以选择 `annotate` 它。可添加的注释包括架构和操作系统（覆盖镜像的当前值）、操作系统特性和架构变体。

Finally, you need to `push` your manifest list to the desired registry. Below are descriptions of these three commands, and an example putting them all together.

​	最后，需要将清单列表 `push` 到所需注册表。以下是这些命令的描述和完整示例。



```console
$ docker manifest create 45.55.81.106:5000/coolapp:v1 \
    45.55.81.106:5000/coolapp-ppc64le-linux:v1 \
    45.55.81.106:5000/coolapp-arm-linux:v1 \
    45.55.81.106:5000/coolapp-amd64-linux:v1 \
    45.55.81.106:5000/coolapp-amd64-windows:v1

Created manifest list 45.55.81.106:5000/coolapp:v1
```



```console
$ docker manifest annotate 45.55.81.106:5000/coolapp:v1 45.55.81.106:5000/coolapp-arm-linux --arch arm
```



```console
$ docker manifest push 45.55.81.106:5000/coolapp:v1
Pushed manifest 45.55.81.106:5000/coolapp@sha256:9701edc932223a66e49dd6c894a11db8c2cf4eccd1414f1ec105a623bf16b426 with digest: sha256:f67dcc5fc786f04f0743abfe0ee5dae9bd8caf8efa6c8144f7f2a43889dc513b
Pushed manifest 45.55.81.106:5000/coolapp@sha256:f3b3b28a45160805bb16542c9531888519430e9e6d6ffc09d72261b0d26ff74f with digest: sha256:b64ca0b60356a30971f098c92200b1271257f100a55b351e6bbe985638352f3a
Pushed manifest 45.55.81.106:5000/coolapp@sha256:39dc41c658cf25f33681a41310372f02728925a54aac3598310bfb1770615fc9 with digest: sha256:df436846483aff62bad830b730a0d3b77731bcf98ba5e470a8bbb8e9e346e4e8
Pushed manifest 45.55.81.106:5000/coolapp@sha256:f91b1145cd4ac800b28122313ae9e88ac340bb3f1e3a4cd3e59a3648650f3275 with digest: sha256:5bb8e50aa2edd408bdf3ddf61efb7338ff34a07b762992c9432f1c02fc0e5e62
sha256:050b213d49d7673ba35014f21454c573dcbec75254a08f4a7c34f66a47c06aba
```

### 检查清单列表 Inspect a manifest list



```console
$ docker manifest inspect coolapp:v1
{
   "schemaVersion": 2,
   "mediaType": "application/vnd.docker.distribution.manifest.list.v2+json",
   "manifests": [
      {
         "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
         "size": 424,
         "digest": "sha256:f67dcc5fc786f04f0743abfe0ee5dae9bd8caf8efa6c8144f7f2a43889dc513b",
         "platform": {
            "architecture": "arm",
            "os": "linux"
         }
      },
      {
         "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
         "size": 424,
         "digest": "sha256:b64ca0b60356a30971f098c92200b1271257f100a55b351e6bbe985638352f3a",
         "platform": {
            "architecture": "amd64",
            "os": "linux"
         }
      },
      {
         "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
         "size": 425,
         "digest": "sha256:df436846483aff62bad830b730a0d3b77731bcf98ba5e470a8bbb8e9e346e4e8",
         "platform": {
            "architecture": "ppc64le",
            "os": "linux"
         }
      },
      {
         "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
         "size": 425,
         "digest": "sha256:5bb8e50aa2edd408bdf3ddf61efb7338ff34a07b762992c9432f1c02fc0e5e62",
         "platform": {
            "architecture": "s390x",
            "os": "linux"
         }
      }
   ]
}
```

### 推送到不安全的注册表 Push to an insecure registry

Here is an example of creating and pushing a manifest list using a known insecure registry.

​	以下示例展示了如何在已知不安全的注册表中创建并推送清单列表。

```console
$ docker manifest create --insecure myprivateregistry.mycompany.com/repo/image:1.0 \
    myprivateregistry.mycompany.com/repo/image-linux-ppc64le:1.0 \
    myprivateregistry.mycompany.com/repo/image-linux-s390x:1.0 \
    myprivateregistry.mycompany.com/repo/image-linux-arm:1.0 \
    myprivateregistry.mycompany.com/repo/image-linux-armhf:1.0 \
    myprivateregistry.mycompany.com/repo/image-windows-amd64:1.0 \
    myprivateregistry.mycompany.com/repo/image-linux-amd64:1.0

$ docker manifest push --insecure myprivateregistry.mycompany.com/repo/image:tag
```

> **Note**
>
> The `--insecure` flag is not required to annotate a manifest list, since annotations are to a locally-stored copy of a manifest list. You may also skip the `--insecure` flag if you are performing a `docker manifest inspect` on a locally-stored manifest list. Be sure to keep in mind that locally-stored manifest lists are never used by the engine on a `docker pull`.
>
> ​	在标注清单列表时不需要 `--insecure` 标志，因为标注只影响本地存储的清单列表。在本地存储的清单列表上执行 `docker manifest inspect` 时也可以跳过 `--insecure` 标志。请注意，本地存储的清单列表不会在 `docker pull` 中被引擎使用。

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker manifest annotate`]({{< ref "/reference/CLIreference/docker/dockermanifest/dockermanifestannotate" >}}) | 为本地镜像清单添加附加信息 Add additional information to a local image manifest |
| [`docker manifest create`]({{< ref "/reference/CLIreference/docker/dockermanifest/dockermanifestcreate" >}}) | 创建本地清单列表，用于标注并推送到注册表 Create a local manifest list for annotating and pushing to a registry |
| [`docker manifest inspect`]({{< ref "/reference/CLIreference/docker/dockermanifest/dockermanifestinspect" >}}) | 显示镜像清单或清单列表 Display an image manifest, or manifest list |
| [`docker manifest push`]({{< ref "/reference/CLIreference/docker/dockermanifest/dockermanifestpush" >}}) | 将清单列表推送到存储库 Push a manifest list to a repository  |
| [`docker manifest rm`]({{< ref "/reference/CLIreference/docker/dockermanifest/dockermanifestrm" >}}) | 从本地存储中删除一个或多个清单列表 Delete one or more manifest lists from local storage |
