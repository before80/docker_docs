+++
title = "过滤命令"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/cli/filter/](https://docs.docker.com/engine/cli/filter/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Filter commands - 过滤命令

You can use the `--filter` flag to scope your commands. When filtering, the commands only include entries that match the pattern you specify.

​	您可以使用 `--filter` 标志来限制命令的范围。使用过滤器时，命令只会包含匹配指定模式的条目。



## 使用过滤器 Using filters

The `--filter` flag expects a key-value pair separated by an operator.

​	`--filter` 标志需要一个用运算符分隔的键值对。



```console
$ docker COMMAND --filter "KEY=VALUE"
```

The key represents the field that you want to filter on. The value is the pattern that the specified field must match. The operator can be either equals (`=`) or not equals (`!=`).

​	键表示要过滤的字段，值是该字段必须匹配的模式。运算符可以是等于（`=`）或不等于（`!=`）。

For example, the command `docker images --filter reference=alpine` filters the output of the `docker images` command to only print `alpine` images.

​	例如，命令 `docker images --filter reference=alpine` 只会输出 `docker images` 命令中与 `alpine` 匹配的镜像。

# 



```console
$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
ubuntu       20.04     33a5cc25d22c   36 minutes ago   101MB
ubuntu       18.04     152dc042452c   36 minutes ago   88.1MB
alpine       3.16      a8cbb8c69ee7   40 minutes ago   8.67MB
alpine       latest    7144f7bab3d4   40 minutes ago   11.7MB
busybox      uclibc    3e516f71d880   48 minutes ago   2.4MB
busybox      glibc     7338d0c72c65   48 minutes ago   6.09MB
$ docker images --filter reference=alpine
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
alpine       3.16      a8cbb8c69ee7   40 minutes ago   8.67MB
alpine       latest    7144f7bab3d4   40 minutes ago   11.7MB
```

The available fields (`reference` in this case) depend on the command you run. Some filters expect an exact match. Others handle partial matches. Some filters let you use regular expressions.

​	可用字段（此例中为 `reference`）取决于您运行的命令。一些过滤器要求精确匹配，其他则可以进行部分匹配，有些过滤器甚至允许使用正则表达式。

Refer to the [CLI reference description](https://docs.docker.com/engine/cli/filter/#reference) for each command to learn about the supported filtering capabilities for each command.

​	请参阅每个命令的 [CLI 参考描述](https://docs.docker.com/engine/cli/filter/#reference)，了解每个命令支持的过滤功能。

## 组合过滤器 Combining filters

You can combine multiple filters by passing multiple `--filter` flags. The following example shows how to print all images that match `alpine:latest` or `busybox` - a logical `OR`.

​	您可以通过传递多个 `--filter` 标志来组合多个过滤器。以下示例展示了如何打印所有与 `alpine:latest` 或 `busybox` 匹配的镜像，这相当于逻辑 `OR`。



```console
$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       20.04     33a5cc25d22c   2 hours ago   101MB
ubuntu       18.04     152dc042452c   2 hours ago   88.1MB
alpine       3.16      a8cbb8c69ee7   2 hours ago   8.67MB
alpine       latest    7144f7bab3d4   2 hours ago   11.7MB
busybox      uclibc    3e516f71d880   2 hours ago   2.4MB
busybox      glibc     7338d0c72c65   2 hours ago   6.09MB
$ docker images --filter reference=alpine:latest --filter=reference=busybox
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
alpine       latest    7144f7bab3d4   2 hours ago   11.7MB
busybox      uclibc    3e516f71d880   2 hours ago   2.4MB
busybox      glibc     7338d0c72c65   2 hours ago   6.09MB
```

### 多个否定过滤器 Multiple negated filters

Some commands support negated filters on [labels]({{< ref "/manuals/DockerEngine/Manageresources/Dockerobjectlabels" >}}). Negated filters only consider results that don't match the specified patterns. The following command prunes all containers that aren't labeled `foo`.

​	一些命令支持对标签的否定过滤。否定过滤器只考虑不匹配指定模式的结果。以下命令会删除所有没有标签 `foo` 的容器。



```console
$ docker container prune --filter "label!=foo"
```

There's a catch in combining multiple negated label filters. Multiple negated filters create a single negative constraint - a logical `AND`. The following command prunes all containers except those labeled both `foo` and `bar`. Containers labeled either `foo` or `bar`, but not both, will be pruned.

​	组合多个否定标签过滤器时，会生成一个逻辑 `AND` 的单一否定约束。以下命令会删除所有没有同时标记 `foo` 和 `bar` 的容器。只有标签 `foo` 或 `bar`（但不是两者皆有）的容器将被删除。



```console
$ docker container prune --filter "label!=foo" --filter "label!=bar"
```

## 参考 Reference

For more information about filtering commands, refer to the CLI reference description for commands that support the `--filter` flag:

​	有关过滤命令的更多信息，请参阅支持 `--filter` 标志的命令的 CLI 参考描述。

- [`docker config ls`]({{< ref "/reference/CLIreference/docker/dockerconfig/dockerconfigls" >}})
- [`docker container prune`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerprune" >}})
- [`docker image prune`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimageprune" >}})
- [`docker image ls`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimages" >}})
- [`docker network ls`]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkls" >}})
- [`docker network prune`]({{< ref "/reference/CLIreference/docker/dockernetwork/dockernetworkprune" >}})
- [`docker node ls`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodels" >}})
- [`docker node ps`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodeps" >}})
- [`docker plugin ls`]({{< ref "/reference/CLIreference/docker/dockerplugin/dockerpluginls" >}})
- [`docker container ls`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerps" >}})
- [`docker search`]({{< ref "/reference/CLIreference/docker/dockersearch" >}})
- [`docker secret ls`]({{< ref "/reference/CLIreference/docker/dockersecret/dockersecretls" >}})
- [`docker service ls`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicels" >}})
- [`docker service ps`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceps" >}})
- [`docker stack ps`]({{< ref "/reference/CLIreference/docker/dockerstack/dockerstackps" >}})
- [`docker system prune`]({{< ref "/reference/CLIreference/docker/dockersystem/dockersystemprune" >}})
- [`docker volume ls`]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumels" >}})
- [`docker volume prune`]({{< ref "/reference/CLIreference/docker/dockervolume/dockervolumeprune" >}})
