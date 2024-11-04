+++
title = "docker images"
date = 2024-10-23T14:54:43+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/image/ls/](https://docs.docker.com/reference/cli/docker/image/ls/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker image ls

| Description | List images                                    |
| :---------- | ---------------------------------------------- |
| Usage       | `docker image ls [OPTIONS] [REPOSITORY[:TAG]]` |
| Aliases     | `docker image list``docker images`             |

## Description

The default `docker images` will show all top level images, their repository and tags, and their size.

​	默认的 `docker images` 将显示所有顶层镜像、它们的仓库和标签，以及它们的大小。

Docker images have intermediate layers that increase reusability, decrease disk usage, and speed up `docker build` by allowing each step to be cached. These intermediate layers are not shown by default.

​	Docker 镜像具有中间层，这些中间层增加了重用性、减少了磁盘使用，并通过允许每一步缓存来加速 `docker build`。这些中间层默认不显示。

The `SIZE` is the cumulative space taken up by the image and all its parent images. This is also the disk space used by the contents of the Tar file created when you `docker save` an image.

​	`SIZE` 是镜像及其所有父镜像所占用的累计空间。这也是当你使用 `docker save` 保存一个镜像时，创建的 Tar 文件的内容所占用的磁盘空间。

An image will be listed more than once if it has multiple repository names or tags. This single image (identifiable by its matching `IMAGE ID`) uses up the `SIZE` listed only once.

​	如果一个镜像具有多个仓库名称或标签，它将被列出多次。这个单一镜像（通过其匹配的 `IMAGE ID` 进行识别）只使用一次列出的 `SIZE`。



## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| `-a, --all`                                                  |         | 显示所有镜像（默认隐藏中间镜像） Show all images (default hides intermediate images) |
| [`--digests`](https://docs.docker.com/reference/cli/docker/image/ls/#digests) |         | 显示摘要 Show digests                                        |
| [`-f, --filter`](https://docs.docker.com/reference/cli/docker/image/ls/#filter) |         | 根据提供的条件过滤输出 Filter output based on conditions provided |
| [`--format`](https://docs.docker.com/reference/cli/docker/image/ls/#format) |         | 使用自定义模板格式化输出：'table'：以表格格式打印输出并带有列标题（默认） 'table TEMPLATE'：使用给定的 Go 模板以表格格式打印输出 'json'：以 JSON 格式打印 'TEMPLATE'：使用给定的 Go 模板打印输出。有关使用模板格式化输出的更多信息，请参阅 https://docs.docker.com/go/formatting/ Format output using a custom template: 'table': Print output in table format with column headers (default) 'table TEMPLATE': Print output in table format using the given Go template 'json': Print in JSON format 'TEMPLATE': Print output using the given Go template. Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| [`--no-trunc`](https://docs.docker.com/reference/cli/docker/image/ls/#no-trunc) |         | xxxxxxxxxx5 1$ docker rmi localhost:5000/test/busybox@sha256:cbbf2f9a99b47fc460d422812b6a5adff7dfee951d8fa2e4a98caa0382cfbdbf2Untagged: localhost:5000/test/busybox@sha256:cbbf2f9a99b47fc460d422812b6a5adff7dfee951d8fa2e4a98caa0382cfbdbf3Deleted: 4986bf8c15363d1c5d15512d5266f8777bfba4974ac56e3270e7760f6f0a81254Deleted: ea13149945cb6b1e746bf28032f02e9b5a793523481a0a18645fc77ad53c4ea25Deleted: df7546f9f060a2268024c8a230d8639878585defcc1bc6f79d2728a13957871bconsole |
| `-q, --quiet`                                                |         | 仅显示镜像 ID - Only show image IDs                          |
| `--tree`                                                     |         | API 1.47+ 实验性（CLI）将多平台镜像列为树（实验性） API 1.47+ experimental (CLI) List multi-platform images as a tree (EXPERIMENTAL) |

## Examples

### 列出最近创建的镜像  List the most recently created images



```console
$ docker images

REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
<none>                    <none>              77af4d6b9913        19 hours ago        1.089 GB
committ                   latest              b6fa739cedf5        19 hours ago        1.089 GB
<none>                    <none>              78a85c484f71        19 hours ago        1.089 GB
docker                    latest              30557a29d5ab        20 hours ago        1.089 GB
<none>                    <none>              5ed6274db6ce        24 hours ago        1.089 GB
postgres                  9                   746b819f315e        4 days ago          213.4 MB
postgres                  9.3                 746b819f315e        4 days ago          213.4 MB
postgres                  9.3.5               746b819f315e        4 days ago          213.4 MB
postgres                  latest              746b819f315e        4 days ago          213.4 MB
```

### 按名称和标签列出镜像 List images by name and tag

The `docker images` command takes an optional `[REPOSITORY[:TAG]]` argument that restricts the list to images that match the argument. If you specify `REPOSITORY`but no `TAG`, the `docker images` command lists all images in the given repository.

​	`docker images` 命令接受一个可选的 `[REPOSITORY[:TAG]]` 参数，该参数限制列出匹配该参数的镜像。如果指定了 `REPOSITORY` 但没有 `TAG`，则 `docker images` 命令将列出给定仓库中的所有镜像。

For example, to list all images in the `java` repository, run the following command:

​	例如，要列出 `java` 仓库中的所有镜像，请运行以下命令：



```console
$ docker images java

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
java                8                   308e519aac60        6 days ago          824.5 MB
java                7                   493d82594c15        3 months ago        656.3 MB
java                latest              2711b1d6f3aa        5 months ago        603.9 MB
```

The `[REPOSITORY[:TAG]]` value must be an exact match. This means that, for example, `docker images jav` does not match the image `java`.

​	`[REPOSITORY[:TAG]]` 值必须完全匹配。这意味着，例如，`docker images jav` 不匹配镜像 `java`。

If both `REPOSITORY` and `TAG` are provided, only images matching that repository and tag are listed. To find all local images in the `java` repository with tag `8` you can use:

​	如果同时提供了 `REPOSITORY` 和 `TAG`，则仅列出与该仓库和标签匹配的镜像。要查找 `java` 仓库中带有标签 `8` 的所有本地镜像，可以使用：



```console
$ docker images java:8

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
java                8                   308e519aac60        6 days ago          824.5 MB
```

If nothing matches `REPOSITORY[:TAG]`, the list is empty.

​	如果没有匹配 `REPOSITORY[:TAG]`，则列表为空。



```console
$ docker images java:0

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
```

### 列出完整长度的镜像 ID (`--no-trunc`) List the full length image IDs (`--no-trunc`)



```console
$ docker images --no-trunc

REPOSITORY                    TAG                 IMAGE ID                                                                  CREATED             SIZE
<none>                        <none>              sha256:77af4d6b9913e693e8d0b4b294fa62ade6054e6b2f1ffb617ac955dd63fb0182   19 hours ago        1.089 GB
committest                    latest              sha256:b6fa739cedf5ea12a620a439402b6004d057da800f91c7524b5086a5e4749c9f   19 hours ago        1.089 GB
<none>                        <none>              sha256:78a85c484f71509adeaace20e72e941f6bdd2b25b4c75da8693efd9f61a37921   19 hours ago        1.089 GB
docker                        latest              sha256:30557a29d5abc51e5f1d5b472e79b7e296f595abcf19fe6b9199dbbc809c6ff4   20 hours ago        1.089 GB
<none>                        <none>              sha256:0124422dd9f9cf7ef15c0617cda3931ee68346455441d66ab8bdc5b05e9fdce5   20 hours ago        1.089 GB
<none>                        <none>              sha256:18ad6fad340262ac2a636efd98a6d1f0ea775ae3d45240d3418466495a19a81b   22 hours ago        1.082 GB
<none>                        <none>              sha256:f9f1e26352f0a3ba6a0ff68167559f64f3e21ff7ada60366e2d44a04befd1d3a   23 hours ago        1.089 GB
tryout                        latest              sha256:2629d1fa0b81b222fca63371ca16cbf6a0772d07759ff80e8d1369b926940074   23 hours ago        131.5 MB
<none>                        <none>              sha256:5ed6274db6ceb2397844896966ea239290555e74ef307030ebb01ff91b1914df   24 hours ago        1.089 GB
```

### 列出镜像摘要 (`--digests`) List image digests (`--digests`)

Images that use the v2 or later format have a content-addressable identifier called a `digest`. As long as the input used to generate the image is unchanged, the digest value is predictable. To list image digest values, use the `--digests` flag:

​	使用 v2 或更高版本格式的镜像具有称为 `digest` 的内容可寻址标识符。只要用于生成镜像的输入保持不变，摘要值就是可预测的。要列出镜像摘要值，请使用 `--digests` 标志：

```console
$ docker images --digests
REPOSITORY                         TAG                 DIGEST                                                                    IMAGE ID            CREATED             SIZE
localhost:5000/test/busybox        <none>              sha256:cbbf2f9a99b47fc460d422812b6a5adff7dfee951d8fa2e4a98caa0382cfbdbf   4986bf8c1536        9 weeks ago         2.43 MB
```

When pushing or pulling to a 2.0 registry, the `push` or `pull` command output includes the image digest. You can `pull` using a digest value. You can also reference by digest in `create`, `run`, and `rmi` commands, as well as the `FROM` image reference in a Dockerfile.

​	在推送或拉取到 2.0 注册表时，`push` 或 `pull` 命令的输出包括镜像摘要。你可以使用摘要值进行 `pull`。你还可以在 `create`、`run` 和 `rmi` 命令中通过摘要进行引用，以及在 Dockerfile 中的 `FROM` 镜像引用。

### Filtering (`--filter`)

The filtering flag (`-f` or `--filter`) format is of "key=value". If there is more than one filter, then pass multiple flags (e.g., `--filter "foo=bar" --filter "bif=baz"`).

​	过滤标志（`-f` 或 `--filter`）的格式为 "key=value"。如果有多个过滤器，则传递多个标志（例如，`--filter "foo=bar" --filter "bif=baz"`）。

The currently supported filters are:

​	当前支持的过滤器有：

- dangling (boolean - true or false)
  - dangling（布尔值 - true 或 false）

- label (`label=<key>` or `label=<key>=<value>`)
  - label（`label=<key>` 或 `label=<key>=<value>`）

- before (`<image-name>[:<tag>]`, `<image id>` or `<image@digest>`) - filter images created before given id or references
  - before (`<image-name>[:<tag>]`, `<image id>` 或 `<image@digest>`) - 过滤在给定 ID 或引用之前创建的镜像

- since (`<image-name>[:<tag>]`, `<image id>` or `<image@digest>`) - filter images created since given id or references
  - since (`<image-name>[:<tag>]`, `<image id>` 或 `<image@digest>`) - 过滤在给定 ID 或引用之后创建的镜像

- reference (pattern of an image reference) - filter images whose reference matches the specified pattern
  - reference (镜像引用的模式) - 过滤引用与指定模式匹配的镜像


#### 显示未标记的镜像（dangling） Show untagged images (dangling)



```console
$ docker images --filter "dangling=true"

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              8abc22fbb042        4 weeks ago         0 B
<none>              <none>              48e5f45168b9        4 weeks ago         2.489 MB
<none>              <none>              bf747efa0e2f        4 weeks ago         0 B
<none>              <none>              980fe10e5736        12 weeks ago        101.4 MB
<none>              <none>              dea752e4e117        12 weeks ago        101.4 MB
<none>              <none>              511136ea3c5a        8 months ago        0 B
```

This will display untagged images that are the leaves of the images tree (not intermediary layers). These images occur when a new build of an image takes the `repo:tag` away from the image ID, leaving it as `<none>:<none>` or untagged. A warning will be issued if trying to remove an image when a container is presently using it. By having this flag it allows for batch cleanup.

​	这将显示作为镜像树叶的未标记镜像（而不是中间层）。当一个新构建的镜像将 `repo:tag` 从镜像 ID 中移除时，这些镜像就会出现，留下 `<none>:<none>` 或未标记。如果尝试删除一个正在被容器使用的镜像，将会发出警告。使用这个标志可以进行批量清理。

You can use this in conjunction with `docker rmi`:

​	你可以将其与 `docker rmi` 一起使用：

```console
$ docker rmi $(docker images -f "dangling=true" -q)

8abc22fbb042
48e5f45168b9
bf747efa0e2f
980fe10e5736
dea752e4e117
511136ea3c5a
```

Docker warns you if any containers exist that are using these untagged images.

​	Docker 会警告你如果有任何容器正在使用这些未标记的镜像。

#### 显示具有给定标签的镜像 Show images with a given label

The `label` filter matches images based on the presence of a `label` alone or a `label` and a value.

​	`label` 过滤器根据仅存在一个 `label` 或者一个 `label` 和一个值来匹配镜像。

The following filter matches images with the `com.example.version` label regardless of its value.

​	以下过滤器匹配具有 `com.example.version` 标签的镜像，无论其值是什么。



```console
$ docker images --filter "label=com.example.version"

REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
match-me-1          latest              eeae25ada2aa        About a minute ago   188.3 MB
match-me-2          latest              dea752e4e117        About a minute ago   188.3 MB
```

The following filter matches images with the `com.example.version` label with the `1.0` value.

​	以下过滤器匹配具有 `com.example.version` 标签且值为 `1.0` 的镜像。

```console
$ docker images --filter "label=com.example.version=1.0"

REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
match-me            latest              511136ea3c5a        About a minute ago   188.3 MB
```

In this example, with the `0.1` value, it returns an empty set because no matches were found.

​	在这个例子中，使用 `0.1` 值时返回一个空集合，因为没有找到匹配。

```console
$ docker images --filter "label=com.example.version=0.1"
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
```

#### 按时间过滤镜像 Filter images by time

The `before` filter shows only images created before the image with a given ID or reference. For example, having these images:

​	`before` 过滤器仅显示在具有给定 ID 或引用的镜像之前创建的镜像。例如，具有这些镜像：

```console
$ docker images

REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
image1              latest              eeae25ada2aa        4 minutes ago        188.3 MB
image2              latest              dea752e4e117        9 minutes ago        188.3 MB
image3              latest              511136ea3c5a        25 minutes ago       188.3 MB
```

Filtering with `before` would give:

​	使用 `before` 过滤将返回：

```console
$ docker images --filter "before=image1"

REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
image2              latest              dea752e4e117        9 minutes ago        188.3 MB
image3              latest              511136ea3c5a        25 minutes ago       188.3 MB
```

Filtering with `since` would give:

​	使用 `since` 过滤将返回：

```console
$ docker images --filter "since=image3"
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
image1              latest              eeae25ada2aa        4 minutes ago        188.3 MB
image2              latest              dea752e4e117        9 minutes ago        188.3 MB
```

#### 按引用过滤镜像 Filter images by reference

The `reference` filter shows only images whose reference matches the specified pattern.

​	`reference` 过滤器仅显示引用与指定模式匹配的镜像。

```console
$ docker images

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
busybox             latest              e02e811dd08f        5 weeks ago         1.09 MB
busybox             uclibc              e02e811dd08f        5 weeks ago         1.09 MB
busybox             musl                733eb3059dce        5 weeks ago         1.21 MB
busybox             glibc               21c16b6787c6        5 weeks ago         4.19 MB
```

Filtering with `reference` would give:

​	使用 `reference` 过滤将返回：

```console
$ docker images --filter=reference='busy*:*libc'

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
busybox             uclibc              e02e811dd08f        5 weeks ago         1.09 MB
busybox             glibc               21c16b6787c6        5 weeks ago         4.19 MB
```

Filtering with multiple `reference` would give, either match A or B:

​	使用多个 `reference` 过滤将返回，匹配 A 或 B：

```console
$ docker images --filter=reference='busy*:uclibc' --filter=reference='busy*:glibc'

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
busybox             uclibc              e02e811dd08f        5 weeks ago         1.09 MB
busybox             glibc               21c16b6787c6        5 weeks ago         4.19 MB
```

### 格式化输出（`--format`） Format the output (--format)

The formatting option (`--format`) will pretty print container output using a Go template.

​	格式化选项（`--format`）将使用 Go 模板美观打印容器输出。

Valid placeholders for the Go template are listed below:

​	有效的 Go 模板占位符如下：

| Placeholder     | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| `.ID`           | 镜像 ID Image ID                                             |
| `.Repository`   | 镜像仓库 Image repository                                    |
| `.Tag`          | 镜像标签 Image tag                                           |
| `.Digest`       | 镜像摘要 Image digest                                        |
| `.CreatedSince` | 自镜像创建以来的经过时间 Elapsed time since the image was created |
| `.CreatedAt`    | 镜像创建时间 Time when the image was created                 |
| `.Size`         | 镜像磁盘大小 Image disk size                                 |

When using the `--format` option, the `image` command will either output the data exactly as the template declares or, when using the `table` directive, will include column headers as well.

​	使用 `--format` 选项时，`image` 命令将输出数据，或者当使用 `table` 指令时，将包括列标题。

The following example uses a template without headers and outputs the `ID` and `Repository` entries separated by a colon (`:`) for all images:

​	以下示例使用没有标题的模板，输出 `ID` 和 `Repository` 条目，以冒号（`:`）分隔所有镜像：



```console
$ docker images --format "{{.ID}}: {{.Repository}}"

77af4d6b9913: <none>
b6fa739cedf5: committ
78a85c484f71: <none>
30557a29d5ab: docker
5ed6274db6ce: <none>
746b819f315e: postgres
746b819f315e: postgres
746b819f315e: postgres
746b819f315e: postgres
```

To list all images with their repository and tag in a table format you can use:

​	要以表格格式列出所有镜像及其仓库和标签，可以使用：

```console
$ docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"

IMAGE ID            REPOSITORY                TAG
77af4d6b9913        <none>                    <none>
b6fa739cedf5        committ                   latest
78a85c484f71        <none>                    <none>
30557a29d5ab        docker                    latest
5ed6274db6ce        <none>                    <none>
746b819f315e        postgres                  9
746b819f315e        postgres                  9.3
746b819f315e        postgres                  9.3.5
746b819f315e        postgres                  latest
```

To list all images in JSON format, use the `json` directive:

​	要以 JSON 格式列出所有镜像，请使用 `json` 指令：

```console
$ docker images --format json
{"Containers":"N/A","CreatedAt":"2021-03-04 03:24:42 +0100 CET","CreatedSince":"5 days ago","Digest":"\u003cnone\u003e","ID":"4dd97cefde62","Repository":"ubuntu","SharedSize":"N/A","Size":"72.9MB","Tag":"latest","UniqueSize":"N/A","VirtualSize":"72.9MB"}
{"Containers":"N/A","CreatedAt":"2021-02-17 22:19:54 +0100 CET","CreatedSince":"2 weeks ago","Digest":"\u003cnone\u003e","ID":"28f6e2705743","Repository":"alpine","SharedSize":"N/A","Size":"5.61MB","Tag":"latest","UniqueSize":"N/A","VirtualSize":"5.613MB"}
```

