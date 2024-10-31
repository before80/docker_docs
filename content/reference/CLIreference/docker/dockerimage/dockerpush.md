+++
title = "docker push"
date = 2024-10-23T14:54:43+08:00
weight = 100
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/image/push/](https://docs.docker.com/reference/cli/docker/image/push/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker image push

| Description | Upload an image to a registry            |
| :---------- | ---------------------------------------- |
| Usage       | `docker image push [OPTIONS] NAME[:TAG]` |
| Aliases     | `docker push`                            |

## Description

Use `docker image push` to share your images to the [Docker Hub](https://hub.docker.com/) registry or to a self-hosted one.

​	使用 `docker image push` 将你的镜像分享至 [Docker Hub](https://hub.docker.com/) 注册表或自托管的注册表。

Refer to the [`docker image tag`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimagetag" >}}) reference for more information about valid image and tag names.

​	有关有效的镜像和标签名称的更多信息，请参考 [`docker image tag`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimagetag" >}}) 参考文档。

Killing the `docker image push` process, for example by pressing `CTRL-c` while it is running in a terminal, terminates the push operation.

​	终止 `docker image push` 进程，例如在终端运行过程中按下 `CTRL-c`，会终止推送操作。

Progress bars are shown during docker push, which show the uncompressed size. The actual amount of data that's pushed will be compressed before sending, so the uploaded size will not be reflected by the progress bar.

​	在 docker push 过程中会显示进度条，显示的是未压缩的大小。实际上传的数据会在发送前被压缩，因此上传大小不会在进度条中反映出来。

Registry credentials are managed by [docker login]({{< ref "/reference/CLIreference/docker/dockerlogin" >}}).

​	注册表凭证通过 [docker login]({{< ref "/reference/CLIreference/docker/dockerlogin" >}}) 管理。

### 并行上传 Concurrent uploads

By default the Docker daemon will push five layers of an image at a time. If you are on a low bandwidth connection this may cause timeout issues and you may want to lower this via the `--max-concurrent-uploads` daemon option. See the [daemon documentation]({{< ref "/reference/CLIreference/dockerd" >}}) for more details.

​	默认情况下，Docker 守护进程一次会推送镜像的五个层。如果你网络带宽较低，这可能会导致超时问题，可以通过 `--max-concurrent-uploads` 守护进程选项来降低该值。详情请参阅 [守护进程文档]({{< ref "/reference/CLIreference/dockerd" >}})。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| [`-a, --all-tags`](https://docs.docker.com/reference/cli/docker/image/push/#all-tags) |         | 推送本地镜像的所有标签至仓库 Push all tags of an image to the repository |
| `--disable-content-trust`                                    | `true`  | 跳过镜像签名 Skip image signing                              |
| `--platform`                                                 |         | API 1.46+ 推送特定平台的单平台镜像至注册表。不会推送镜像索引，其他清单包括验证信息将不保留。格式：'os[/arch[/variant]]'（例如 linux/amd64） API 1.46+ Push a platform-specific manifest as a single-platform image to the registry. Image index won't be pushed, meaning that other manifests, including attestations won't be preserved. 'os[/arch[/variant]]': Explicit platform (eg. linux/amd64) |
| `-q, --quiet`                                                |         | 禁用详细输出 Suppress verbose output                         |

## Examples

### 将新镜像推送至注册表 Push a new image to a registry

First save the new image by finding the container ID (using [`docker container ls`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerps" >}})) and then committing it to a new image name. Note that only `a-z0-9-_.` are allowed when naming images:

​	首先通过查找容器 ID（使用 [`docker container ls`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerps" >}})）并提交它到新镜像名称来保存新镜像。注意，命名镜像时只能使用 `a-z0-9-_`：



```console
$ docker container commit c16378f943fe rhel-httpd:latest
```

Now, push the image to the registry using the image ID. In this example the registry is on host named `registry-host` and listening on port `5000`. To do this, tag the image with the host name or IP address, and the port of the registry:

​	现在，使用镜像 ID 将镜像推送至注册表。本示例中，注册表在名为 `registry-host` 的主机上并监听端口 `5000`。为此，请使用主机名或 IP 地址和注册表端口为镜像打标签：



```console
$ docker image tag rhel-httpd:latest registry-host:5000/myadmin/rhel-httpd:latest

$ docker image push registry-host:5000/myadmin/rhel-httpd:latest
```

Check that this worked by running:

​	通过运行以下命令检查是否成功：

```console
$ docker image ls
```

You should see both `rhel-httpd` and `registry-host:5000/myadmin/rhel-httpd` listed.

### 推送镜像的所有标签 (`-a`, `--all-tags`) Push all tags of an image (-a, --all-tags)

Use the `-a` (or `--all-tags`) option to push all tags of a local image.

​	使用 `-a`（或 `--all-tags`）选项可以推送本地镜像的所有标签。

The following example creates multiple tags for an image, and pushes all those tags to Docker Hub.

​	以下示例为镜像创建了多个标签，并将所有这些标签推送到 Docker Hub。



```console
$ docker image tag myimage registry-host:5000/myname/myimage:latest
$ docker image tag myimage registry-host:5000/myname/myimage:v1.0.1
$ docker image tag myimage registry-host:5000/myname/myimage:v1.0
$ docker image tag myimage registry-host:5000/myname/myimage:v1
```

The image is now tagged under multiple names:

​	现在镜像有多个标签：

```console
$ docker image ls

REPOSITORY                          TAG        IMAGE ID       CREATED      SIZE
myimage                             latest     6d5fcfe5ff17   2 hours ago  1.22MB
registry-host:5000/myname/myimage   latest     6d5fcfe5ff17   2 hours ago  1.22MB
registry-host:5000/myname/myimage   v1         6d5fcfe5ff17   2 hours ago  1.22MB
registry-host:5000/myname/myimage   v1.0       6d5fcfe5ff17   2 hours ago  1.22MB
registry-host:5000/myname/myimage   v1.0.1     6d5fcfe5ff17   2 hours ago  1.22MB
```

When pushing with the `--all-tags` option, all tags of the `registry-host:5000/myname/myimage` image are pushed:

​	当使用 `--all-tags` 选项推送时，`registry-host:5000/myname/myimage` 镜像的所有标签都会被推送：

```console
$ docker image push --all-tags registry-host:5000/myname/myimage

The push refers to repository [registry-host:5000/myname/myimage]
195be5f8be1d: Pushed
latest: digest: sha256:edafc0a0fb057813850d1ba44014914ca02d671ae247107ca70c94db686e7de6 size: 4527
195be5f8be1d: Layer already exists
v1: digest: sha256:edafc0a0fb057813850d1ba44014914ca02d671ae247107ca70c94db686e7de6 size: 4527
195be5f8be1d: Layer already exists
v1.0: digest: sha256:edafc0a0fb057813850d1ba44014914ca02d671ae247107ca70c94db686e7de6 size: 4527
195be5f8be1d: Layer already exists
v1.0.1: digest: sha256:edafc0a0fb057813850d1ba44014914ca02d671ae247107ca70c94db686e7de6 size: 4527
```
