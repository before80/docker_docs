+++
title = "Filter commands"
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

# Filter commands

You can use the `--filter` flag to scope your commands. When filtering, the commands only include entries that match the pattern you specify.

## Using filters

The `--filter` flag expects a key-value pair separated by an operator.



```console
$ docker COMMAND --filter "KEY=VALUE"
```

The key represents the field that you want to filter on. The value is the pattern that the specified field must match. The operator can be either equals (`=`) or not equals (`!=`).

For example, the command `docker images --filter reference=alpine` filters the output of the `docker images` command to only print `alpine` images.



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

Refer to the [CLI reference description](https://docs.docker.com/engine/cli/filter/#reference) for each command to learn about the supported filtering capabilities for each command.

## Combining filters

You can combine multiple filters by passing multiple `--filter` flags. The following example shows how to print all images that match `alpine:latest` or `busybox` - a logical `OR`.



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

### Multiple negated filters

Some commands support negated filters on [labels]({{< ref "/manuals/DockerEngine/Manageresources/Dockerobjectlabels" >}}). Negated filters only consider results that don't match the specified patterns. The following command prunes all containers that aren't labeled `foo`.



```console
$ docker container prune --filter "label!=foo"
```

There's a catch in combining multiple negated label filters. Multiple negated filters create a single negative constraint - a logical `AND`. The following command prunes all containers except those labeled both `foo` and `bar`. Containers labeled either `foo` or `bar`, but not both, will be pruned.



```console
$ docker container prune --filter "label!=foo" --filter "label!=bar"
```

## Reference

For more information about filtering commands, refer to the CLI reference description for commands that support the `--filter` flag:

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
