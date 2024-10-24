+++
title = "docker container rm"
date = 2024-10-23T14:54:43+08:00
weight = 140
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/rm/](https://docs.docker.com/reference/cli/docker/container/rm/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container rm

| Description | Remove one or more containers                            |
| :---------- | -------------------------------------------------------- |
| Usage       | `docker container rm [OPTIONS] CONTAINER [CONTAINER...]` |
| Aliases     | `docker container remove``docker rm`                     |

## Description

Remove one or more containers

## Options

| Option                                                       | Default | Description                                             |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------- |
| [`-f, --force`](https://docs.docker.com/reference/cli/docker/container/rm/#force) |         | Force the removal of a running container (uses SIGKILL) |
| [`-l, --link`](https://docs.docker.com/reference/cli/docker/container/rm/#link) |         | Remove the specified link                               |
| [`-v, --volumes`](https://docs.docker.com/reference/cli/docker/container/rm/#volumes) |         | Remove anonymous volumes associated with the container  |

## Examples

### Remove a container

This removes the container referenced under the link `/redis`.



```console
$ docker rm /redis

/redis
```

### Remove a link specified with `--link` on the default bridge network (--link)

This removes the underlying link between `/webapp` and the `/redis` containers on the default bridge network, removing all network communication between the two containers. This does not apply when `--link` is used with user-specified networks.



```console
$ docker rm --link /webapp/redis

/webapp/redis
```

### Force-remove a running container (--force)

This command force-removes a running container.



```console
$ docker rm --force redis

redis
```

The main process inside the container referenced under the link `redis` will receive `SIGKILL`, then the container will be removed.

### Remove all stopped containers

Use the [`docker container prune`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerprune" >}}) command to remove all stopped containers, or refer to the [`docker system prune`]({{< ref "/reference/CLIreference/docker/dockersystem/dockersystemprune" >}}) command to remove unused containers in addition to other Docker resources, such as (unused) images and networks.

Alternatively, you can use the `docker ps` with the `-q` / `--quiet` option to generate a list of container IDs to remove, and use that list as argument for the `docker rm` command.

Combining commands can be more flexible, but is less portable as it depends on features provided by the shell, and the exact syntax may differ depending on what shell is used. To use this approach on Windows, consider using PowerShell or Bash.

The example below uses `docker ps -q` to print the IDs of all containers that have exited (`--filter status=exited`), and removes those containers with the `docker rm` command:



```console
$ docker rm $(docker ps --filter status=exited -q)
```

Or, using the `xargs` Linux utility:



```console
$ docker ps --filter status=exited -q | xargs docker rm
```

### Remove a container and its volumes (-v, --volumes)



```console
$ docker rm --volumes redis
redis
```

This command removes the container and any volumes associated with it. Note that if a volume was specified with a name, it will not be removed.

### Remove a container and selectively remove volumes



```console
$ docker create -v awesome:/foo -v /bar --name hello redis
hello

$ docker rm -v hello
```

In this example, the volume for `/foo` remains intact, but the volume for `/bar` is removed. The same behavior holds for volumes inherited with `--volumes-from`.
