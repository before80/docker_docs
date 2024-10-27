+++
title = "Bind mounts"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/storage/bind-mounts/](https://docs.docker.com/engine/storage/bind-mounts/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Bind mounts - 绑定挂载

Bind mounts have been around since the early days of Docker. Bind mounts have limited functionality compared to [volumes]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}). When you use a bind mount, a file or directory on the host machine is mounted into a container. The file or directory is referenced by its absolute path on the host machine. By contrast, when you use a volume, a new directory is created within Docker's storage directory on the host machine, and Docker manages that directory's contents.

​	自 Docker 初期以来，绑定挂载就已经存在。相比[卷]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})，绑定挂载的功能较为有限。当使用绑定挂载时，主机上的文件或目录会被挂载到容器中，通过主机上的绝对路径进行引用。而使用卷时，会在主机的 Docker 存储目录内创建一个新目录，并由 Docker 管理该目录的内容。

The file or directory does not need to exist on the Docker host already. It is created on demand if it does not yet exist. Bind mounts are very performant, but they rely on the host machine's filesystem having a specific directory structure available. If you are developing new Docker applications, consider using [named volumes]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}) instead. You can't use Docker CLI commands to directly manage bind mounts.

​	文件或目录不需要在 Docker 主机上已经存在；如果不存在，则会在需要时按需创建。绑定挂载性能非常高，但依赖于主机文件系统中可用的特定目录结构。如果正在开发新的 Docker 应用程序，建议使用[命名卷]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})。绑定挂载无法通过 Docker CLI 命令直接管理。

![Bind mounts on the Docker host](Bindmounts_img/types-of-mounts-bind.webp)

> **Tip**
>
> 
>
> Working with large repositories or monorepos, or with virtual file systems that are no longer scaling with your codebase? Check out [Synchronized file shares]({{< ref "/manuals/DockerDesktop/Synchronizedfileshares" >}}). It provides fast and flexible host-to-VM file sharing by enhancing bind mount performance through the use of synchronized filesystem caches.
>
> ​	处理大型代码库、单一代码库或无法扩展到代码库的虚拟文件系统？请查看[同步文件共享]({{< ref "/manuals/DockerDesktop/Synchronizedfileshares" >}})。它通过使用同步文件系统缓存来增强绑定挂载性能，实现快速且灵活的主机到虚拟机文件共享。

## Choose the `-v` or `--mount` flag

In general, `--mount` is more explicit and verbose. The biggest difference is that the `-v` syntax combines all the options together in one field, while the `--mount` syntax separates them. Here is a comparison of the syntax for each flag.

​	通常，`--mount` 更为显式和详细。主要区别在于 `-v` 语法将所有选项合并在一个字段中，而 `--mount` 语法将它们分开。以下是两种标志的语法对比。

> Tip
>
> New users should use the `--mount` syntax. Experienced users may be more familiar with the `-v` or `--volume` syntax, but are encouraged to use `--mount`, because research has shown it to be easier to use.
>
> ​	新用户应使用 `--mount` 语法。尽管有经验的用户可能更熟悉 `-v` 或 `--volume` 语法，但建议使用 `--mount`，因为研究表明它更易于使用。

- `-v` or `--volume`: Consists of three fields, separated by colon characters (`:`). The fields must be in the correct order, and the meaning of each field is not immediately obvious. `-v` 或 `--volume`：由三个字段组成，以冒号 (`:`) 分隔。字段顺序必须正确，每个字段的含义不易理解。
  - In the case of bind mounts, the first field is the path to the file or directory on the **host machine**.
    - 对于绑定挂载，第一个字段是**主机**上的文件或目录路径。
  - The second field is the path where the file or directory is mounted in the container.
    - 第二个字段是文件或目录在容器中的挂载路径。
  - The third field is optional, and is a comma-separated list of options, such as `ro`, `z`, and `Z`. These options are discussed below.
    - 第三个字段是可选的，是一个逗号分隔的选项列表，例如 `ro`、`z` 和 `Z`。这些选项将在下文讨论。
- `--mount`: Consists of multiple key-value pairs, separated by commas and each consisting of a `<key>=<value>` tuple. The `--mount` syntax is more verbose than `-v` or `--volume`, but the order of the keys is not significant, and the value of the flag is easier to understand.  `--mount`：由多个键值对组成，逗号分隔，每个由 `<key>=<value>` 元组组成。`--mount` 语法比 `-v` 或 `--volume` 更为详细，但键的顺序无关紧要，标志的值也更易于理解。
  - The `type` of the mount, which can be `bind`, `volume`, or `tmpfs`. This topic discusses bind mounts, so the type is always `bind`.
    - 挂载的 `type`，可以是 `bind`、`volume` 或 `tmpfs`。本文讨论绑定挂载，因此类型始终为 `bind`。
  - The `source` of the mount. For bind mounts, this is the path to the file or directory on the Docker daemon host. May be specified as `source` or `src`.
    - 挂载的 `source`。对于绑定挂载，这是 Docker 守护进程主机上文件或目录的路径。可以指定为 `source` 或 `src`。
  - The `destination` takes as its value the path where the file or directory is mounted in the container. May be specified as `destination`, `dst`, or `target`.
    - `destination` 指定文件或目录在容器中的挂载路径。可以指定为 `destination`、`dst` 或 `target`。
  - The `readonly` option, if present, causes the bind mount to be [mounted into the container as read-only](https://docs.docker.com/engine/storage/bind-mounts/#use-a-read-only-bind-mount).
    - `readonly` 选项（如果存在），使绑定挂载[作为只读挂载到容器中](https://docs.docker.com/engine/storage/bind-mounts/#use-a-read-only-bind-mount)。
  - The `bind-propagation` option, if present, changes the [bind propagation](https://docs.docker.com/engine/storage/bind-mounts/#configure-bind-propagation). May be one of `rprivate`, `private`, `rshared`, `shared`, `rslave`, `slave`.
    - `bind-propagation` 选项（如果存在），更改[绑定传播](https://docs.docker.com/engine/storage/bind-mounts/#configure-bind-propagation)，可以是 `rprivate`、`private`、`rshared`、`shared`、`rslave`、`slave` 中的一个。
  - The `--mount` flag does not support `z` or `Z` options for modifying selinux labels.
    - `--mount` 标志不支持 `z` 或 `Z` 选项修改 selinux 标签。

The examples below show both the `--mount` and `-v` syntax where possible, and `--mount` is presented first.

​	以下示例在可能的情况下展示 `--mount` 和 `-v` 语法，`--mount` 语法优先显示。

### Differences between `-v` and `--mount` behavior

Because the `-v` and `--volume` flags have been a part of Docker for a long time, their behavior cannot be changed. This means that there is one behavior that is different between `-v` and `--mount`.

​	由于 `-v` 和 `--volume` 标志已在 Docker 中使用较长时间，其行为无法更改。这意味着它们之间有一个不同的行为。

If you use `-v` or `--volume` to bind-mount a file or directory that does not yet exist on the Docker host, `-v` creates the endpoint for you. It is always created as a directory.

​	如果使用 `-v` 或 `--volume` 绑定挂载一个在 Docker 主机上尚不存在的文件或目录，`-v` 会为您创建该终端点，且始终创建为目录。

If you use `--mount` to bind-mount a file or directory that does not yet exist on the Docker host, Docker does not automatically create it for you, but generates an error.

​	如果使用 `--mount` 绑定挂载一个在 Docker 主机上尚不存在的文件或目录，Docker 不会自动创建它，而是会生成一个错误。

## 使用绑定挂载启动容器 Start a container with a bind mount

Consider a case where you have a directory `source` and that when you build the source code, the artifacts are saved into another directory, `source/target/`. You want the artifacts to be available to the container at `/app/`, and you want the container to get access to a new build each time you build the source on your development host. Use the following command to bind-mount the `target/` directory into your container at `/app/`. Run the command from within the `source` directory. The `$(pwd)` sub-command expands to the current working directory on Linux or macOS hosts. If you're on Windows, see also [Path conversions on Windows]({{< ref "/manuals/DockerDesktop/Troubleshootanddiagnose/Commontopics" >}}).

​	假设有一个 `source` 目录，当构建源代码时，生成的构建工件保存在 `source/target/` 目录中。您希望构建工件在容器中的 `/app/` 可用，并希望容器每次访问新构建时，能够访问开发主机上的源代码。使用以下命令将 `target/` 目录绑定挂载到容器中的 `/app/`。从 `source` 目录中运行该命令。`$(pwd)` 子命令会在 Linux 或 macOS 主机上扩展为当前工作目录。如果您在 Windows 上运行，请参考[Windows 路径转换]({{< ref "/manuals/DockerDesktop/Troubleshootanddiagnose/Commontopics" >}})。

The `--mount` and `-v` examples below produce the same result. You can't run them both unless you remove the `devtest` container after running the first one.

​	`--mount` 和 `-v` 示例的结果相同。运行第一个后需删除 `devtest` 容器才能运行它们两个。

{{< tabpane text=true persist=disabled >}}

{{% tab header="`--mount`" %}}

```console
 docker run -d \
  -it \
  --name devtest \
  --mount type=bind,source="$(pwd)"/target,target=/app \
  nginx:latest
```

{{% /tab  %}}

{{% tab header="`-v`" %}}

```console
 docker run -d \
  -it \
  --name devtest \
  -v "$(pwd)"/target:/app \
  nginx:latest
```

{{% /tab  %}}

{{< /tabpane >}}



------

Use `docker inspect devtest` to verify that the bind mount was created correctly. Look for the `Mounts` section:

​	使用 `docker inspect devtest` 验证绑定挂载是否创建正确。查看 `Mounts` 部分：



```json
"Mounts": [
    {
        "Type": "bind",
        "Source": "/tmp/source/target",
        "Destination": "/app",
        "Mode": "",
        "RW": true,
        "Propagation": "rprivate"
    }
],
```

This shows that the mount is a `bind` mount, it shows the correct source and destination, it shows that the mount is read-write, and that the propagation is set to `rprivate`.

​	这表明挂载是一个绑定挂载，显示了正确的源和目标，挂载为可读写，且传播设置为 `rprivate`。

Stop the container:

​	停止容器：

```console
$ docker container stop devtest

$ docker container rm devtest
```

### 挂载到容器的非空目录 Mount into a non-empty directory on the container

If you bind-mount a directory into a non-empty directory on the container, the directory's existing contents are obscured by the bind mount. This can be beneficial, such as when you want to test a new version of your application without building a new image. However, it can also be surprising and this behavior differs from that of [docker volumes]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}).

​	如果将目录绑定挂载到容器中的非空目录，该目录的现有内容会被绑定挂载遮蔽。这在测试应用程序的新版本而不构建新镜像时非常有用，但也可能带来意外，这种行为不同于[卷]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})。

This example is contrived to be extreme, but replaces the contents of the container's `/usr/` directory with the `/tmp/` directory on the host machine. In most cases, this would result in a non-functioning container.

​	以下例子将容器的 `/usr/` 目录内容替换为主机的 `/tmp/` 目录内容。大多数情况下，这会导致容器无法正常工作。

The `--mount` and `-v` examples have the same end result.

​	`--mount` 和 `-v` 示例的结果相同。

{{< tabpane text=true persist=disabled >}}

{{% tab header="`--mount`" %}}

```console
 docker run -d \
  -it \
  --name broken-container \
  --mount type=bind,source=/tmp,target=/usr \
  nginx:latest

docker: Error response from daemon: oci runtime error: container_linux.go:262:
starting container process caused "exec: \"nginx\": executable file not found in $PATH".
```

{{% /tab  %}}

{{% tab header="`-v`" %}}

```console
 docker run -d \
  -it \
  --name broken-container \
  -v /tmp:/usr \
  nginx:latest

docker: Error response from daemon: oci runtime error: container_linux.go:262:
starting container process caused "exec: \"nginx\": executable file not found in $PATH".
```

{{% /tab  %}}

{{< /tabpane >}}



------

The container is created but does not start. Remove it:

​	容器创建成功但无法启动。将其删除：

```console
$ docker container rm broken-container
```

## 使用只读绑定挂载 Use a read-only bind mount

For some development applications, the container needs to write into the bind mount, so changes are propagated back to the Docker host. At other times, the container only needs read access.

​	在某些开发应用中，容器需要写入绑定挂载，以便更改被传播回 Docker 主机。其他情况下，容器只需要读取访问权限。

This example modifies the one above but mounts the directory as a read-only bind mount, by adding `ro` to the (empty by default) list of options, after the mount point within the container. Where multiple options are present, separate them by commas.

​	此示例修改上例，将目录挂载为只读绑定挂载，通过在容器内的挂载点后添加 `ro`（默认情况下为空），以实现只读。多个选项时，用逗号分隔。

The `--mount` and `-v` examples have the same result.

​	`--mount` 和 `-v` 示例的结果相同。

{{< tabpane text=true persist=disabled >}}

{{% tab header="`--mount`" %}}

```console
 docker run -d \
  -it \
  --name devtest \
  --mount type=bind,source="$(pwd)"/target,target=/app,readonly \
  nginx:latest
```

{{% /tab  %}}

{{% tab header="`-v`" %}}

```console
 docker run -d \
  -it \
  --name devtest \
  -v "$(pwd)"/target:/app:ro \
  nginx:latest
```

{{% /tab  %}}

{{< /tabpane >}}



------

Use `docker inspect devtest` to verify that the bind mount was created correctly. Look for the `Mounts` section:

​	使用 `docker inspect devtest` 验证绑定挂载创建是否正确。查看 `Mounts` 部分：



```json
"Mounts": [
    {
        "Type": "bind",
        "Source": "/tmp/source/target",
        "Destination": "/app",
        "Mode": "ro",
        "RW": false,
        "Propagation": "rprivate"
    }
],
```

Stop the container:

​	停止容器：

```console
$ docker container stop devtest

$ docker container rm devtest
```

## 递归挂载 Recursive mounts

When you bind mount a path that itself contains mounts, those submounts are also included in the bind mount by default. This behavior is configurable, using the `bind-recursive` option for `--mount`. This option is only supported with the `--mount` flag, not with `-v` or `--volume`.

​	当绑定挂载一个路径且该路径包含其他挂载时，这些子挂载默认也会包含在绑定挂载中。此行为可通过 `bind-recursive` 选项配置，仅支持 `--mount` 标志，不支持 `-v` 或 `--volume`。

If the bind mount is read-only, the Docker Engine makes a best-effort attempt at making the submounts read-only as well. This is referred to as recursive read-only mounts. Recursive read-only mounts require Linux kernel version 5.12 or later. If you're running an older kernel version, submounts are automatically mounted as read-write by default. Attempting to set submounts to be read-only on a kernel version earlier than 5.12, using the `bind-recursive=readonly` option, results in an error.

​	若绑定挂载为只读，Docker 引擎会尽力尝试将子挂载设置为只读。递归只读挂载需要 Linux 内核版本 5.12 或更高版本。如果运行的内核版本较低，子挂载默认会自动设置为可读写。尝试在 5.12 以下内核版本上使用 `bind-recursive=readonly` 会导致错误。

Supported values for the `bind-recursive` option are:

​	有关 `bind-recursive` 选项的支持值如下：

| Value               | Description                                                  |
| :------------------ | :----------------------------------------------------------- |
| `enabled` (default) | Read-only mounts are made recursively read-only if kernel is v5.12 or later. Otherwise, submounts are read-write. 只读挂载在内核版本 5.12 或更高版本下递归只读。否则子挂载为可读写。 |
| `disabled`          | Submounts are ignored (not included in the bind mount). 忽略子挂载（不包含在绑定挂载中）。 |
| `writable`          | Submounts are read-write. 子挂载为可读写。                   |
| `readonly`          | Submounts are read-only. Requires kernel v5.12 or later. 子挂载为只读。需内核版本 5.12 或更高。 |

## 配置绑定传播 Configure bind propagation

Bind propagation defaults to `rprivate` for both bind mounts and volumes. It is only configurable for bind mounts, and only on Linux host machines. Bind propagation is an advanced topic and many users never need to configure it.

​	绑定传播在绑定挂载和卷中默认设置为 `rprivate`。该设置仅可在绑定挂载中配置，并且只能在 Linux 主机上使用。绑定传播是一个高级主题，大多数用户无需配置它。

Bind propagation refers to whether or not mounts created within a given bind-mount can be propagated to replicas of that mount. Consider a mount point `/mnt`, which is also mounted on `/tmp`. The propagation settings control whether a mount on `/tmp/a` would also be available on `/mnt/a`. Each propagation setting has a recursive counterpoint. In the case of recursion, consider that `/tmp/a` is also mounted as `/foo`. The propagation settings control whether `/mnt/a` and/or `/tmp/a` would exist.

​	绑定传播指的是在给定的绑定挂载中创建的挂载点是否可以传播到该挂载的副本。例如，假设一个挂载点 `/mnt`，它也被挂载在 `/tmp` 上。传播设置控制是否 `/tmp/a` 上的挂载也可以在 `/mnt/a` 上使用。每个传播设置都有一个递归的对应点。在递归的情况下，可以假设 `/tmp/a` 也被挂载在 `/foo` 上，传播设置控制 `/mnt/a` 和/或 `/tmp/a` 是否存在。

> **Warning**
>
> 
>
> Mount propagation doesn't work with Docker Desktop.
>
> ​	Docker Desktop 不支持挂载传播。

| Propagation setting | Description                                                  |
| :------------------ | :----------------------------------------------------------- |
| `shared`            | Sub-mounts of the original mount are exposed to replica mounts, and sub-mounts of replica mounts are also propagated to the original mount. 原始挂载的子挂载会暴露给副本挂载，副本挂载的子挂载也会传播到原始挂载。 |
| `slave`             | similar to a shared mount, but only in one direction. If the original mount exposes a sub-mount, the replica mount can see it. However, if the replica mount exposes a sub-mount, the original mount cannot see it. 类似于共享挂载，但仅单向传播。原始挂载暴露子挂载时，副本挂载可见；副本挂载暴露子挂载时，原始挂载不可见。 |
| `private`           | The mount is private. Sub-mounts within it are not exposed to replica mounts, and sub-mounts of replica mounts are not exposed to the original mount. 挂载是私有的，子挂载不会暴露给副本挂载，副本挂载的子挂载也不会暴露给原始挂载。 |
| `rshared`           | The same as shared, but the propagation also extends to and from mount points nested within any of the original or replica mount points. 与共享相同，但传播也扩展到原始或副本挂载点内嵌的任何挂载点。 |
| `rslave`            | The same as slave, but the propagation also extends to and from mount points nested within any of the original or replica mount points. 与从属相同，但传播也扩展到原始或副本挂载点内嵌的任何挂载点。 |
| `rprivate`          | The default. The same as private, meaning that no mount points anywhere within the original or replica mount points propagate in either direction. 默认值。与私有相同，意味着原始或副本挂载点内的任何挂载点都不会向任何方向传播。 |

Before you can set bind propagation on a mount point, the host filesystem needs to already support bind propagation.

​	在设置挂载点上的绑定传播之前，主机文件系统需要已支持绑定传播。

For more information about bind propagation, see the [Linux kernel documentation for shared subtree](https://www.kernel.org/doc/Documentation/filesystems/sharedsubtree.txt).

​	有关绑定传播的更多信息，请参阅 [Linux 内核文档 - 共享子树](https://www.kernel.org/doc/Documentation/filesystems/sharedsubtree.txt)。

The following example mounts the `target/` directory into the container twice, and the second mount sets both the `ro` option and the `rslave` bind propagation option.

​	以下示例将 `target/` 目录两次挂载到容器中，第二次挂载设置了 `ro` 和 `rslave` 绑定传播选项。

The `--mount` and `-v` examples have the same result.

​	`--mount` 和 `-v` 示例效果相同。

{{< tabpane text=true persist=disabled >}}

{{% tab header="`--mount`" %}}

```console
 docker run -d \
  -it \
  --name devtest \
  --mount type=bind,source="$(pwd)"/target,target=/app \
  --mount type=bind,source="$(pwd)"/target,target=/app2,readonly,bind-propagation=rslave \
  nginx:latest
```

{{% /tab  %}}

{{% tab header="`-v`" %}}

```console
 docker run -d \
  -it \
  --name devtest \
  -v "$(pwd)"/target:/app \
  -v "$(pwd)"/target:/app2:ro,rslave \
  nginx:latest
```

{{% /tab  %}}

{{< /tabpane >}}



------

Now if you create `/app/foo/`, `/app2/foo/` also exists.

​	现在，如果在 `/app/foo/` 创建文件夹，则 `/app2/foo/` 也存在。

## 配置 selinux 标签 Configure the selinux label

If you use `selinux` you can add the `z` or `Z` options to modify the selinux label of the host file or directory being mounted into the container. This affects the file or directory on the host machine itself and can have consequences outside of the scope of Docker.

​	如果使用 `selinux`，可以添加 `z` 或 `Z` 选项来修改挂载到容器中的主机文件或目录的 selinux 标签。这会影响主机上的文件或目录，可能会带来 Docker 之外的后果。

- The `z` option indicates that the bind mount content is shared among multiple containers.
  - `z` 选项表示绑定挂载内容在多个容器间共享。

- The `Z` option indicates that the bind mount content is private and unshared.
  - `Z` 选项表示绑定挂载内容是私有的且不共享。


Use extreme caution with these options. Bind-mounting a system directory such as `/home` or `/usr` with the `Z` option renders your host machine inoperable and you may need to relabel the host machine files by hand.

​	在使用这些选项时要极其谨慎。例如，将系统目录（如 `/home` 或 `/usr`）使用 `Z` 选项绑定挂载，可能导致主机无法操作，并可能需要手动重新标记主机文件。

> **Important**
>
> 
>
> When using bind mounts with services, selinux labels (`:Z` and `:z`), as well as `:ro` are ignored. See [moby/moby #32579](https://github.com/moby/moby/issues/32579) for details.
>
> ​	在服务中使用绑定挂载时，selinux 标签（`:Z` 和 `:z`）以及 `:ro` 会被忽略。详情见 [moby/moby #32579](https://github.com/moby/moby/issues/32579)。

This example sets the `z` option to specify that multiple containers can share the bind mount's contents:

​	此示例设置 `z` 选项，指定多个容器可共享绑定挂载的内容：

It is not possible to modify the selinux label using the `--mount` flag.

​	无法通过 `--mount` 标志修改 selinux 标签。



```console
$ docker run -d \
  -it \
  --name devtest \
  -v "$(pwd)"/target:/app:z \
  nginx:latest
```

## 在 Docker Compose 中使用绑定挂载 Use a bind mount with compose

A single Docker Compose service with a bind mount looks like this:

​	一个带绑定挂载的单一 Docker Compose 服务示例如下：

```yaml
services:
  frontend:
    image: node:lts
    volumes:
      - type: bind
        source: ./static
        target: /opt/app/static
volumes:
  myapp:
```

For more information about using volumes of the `bind` type with Compose, see [Compose reference on volumes](https://docs.docker.com/reference/compose-file/services/#volumes). and [Compose reference on volume configuration](https://docs.docker.com/reference/compose-file/services/#volumes).

​	有关在 Compose 中使用 `bind` 类型卷的更多信息，请参阅 [Compose 卷参考](https://docs.docker.com/reference/compose-file/services/#volumes) 和 [卷配置参考](https://docs.docker.com/reference/compose-file/services/#volumes)。

## 接下来 Next steps

- Learn about [volumes]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}).
  - 了解有关 [卷]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}) 的信息。
- Learn about [tmpfs mounts]({{< ref "/manuals/DockerEngine/Storage/tmpfsmounts" >}}).
  - 了解有关 [tmpfs 挂载]({{< ref "/manuals/DockerEngine/Storage/tmpfsmounts" >}}) 的信息。
- Learn about [storage drivers]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers" >}}).
  - 了解有关 [存储驱动程序]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers" >}}) 的信息。
