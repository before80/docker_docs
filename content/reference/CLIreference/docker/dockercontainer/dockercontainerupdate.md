+++
title = "docker container update"
date = 2024-10-23T14:54:43+08:00
weight = 200
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/update/](https://docs.docker.com/reference/cli/docker/container/update/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container update

| Description | Update configuration of one or more containers               |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker container update [OPTIONS] CONTAINER [CONTAINER...]` |
| Aliases     | `docker update`                                              |

## Description

The `docker update` command dynamically updates container configuration. You can use this command to prevent containers from consuming too many resources from their Docker host. With a single command, you can place limits on a single container or on many. To specify more than one container, provide space-separated list of container names or IDs.

With the exception of the `--kernel-memory` option, you can specify these options on a running or a stopped container. On kernel version older than 4.6, you can only update `--kernel-memory` on a stopped container or on a running container with kernel memory initialized.

> **Warning**
>
> The `docker update` and `docker container update` commands are not supported for Windows containers.

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| `--blkio-weight`                                             |         | Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0) |
| `--cpu-period`                                               |         | Limit CPU CFS (Completely Fair Scheduler) period             |
| `--cpu-quota`                                                |         | Limit CPU CFS (Completely Fair Scheduler) quota              |
| `--cpu-rt-period`                                            |         | API 1.25+ Limit the CPU real-time period in microseconds     |
| `--cpu-rt-runtime`                                           |         | API 1.25+ Limit the CPU real-time runtime in microseconds    |
| [`-c, --cpu-shares`](https://docs.docker.com/reference/cli/docker/container/update/#cpu-shares) |         | CPU shares (relative weight)                                 |
| `--cpus`                                                     |         | API 1.29+ Number of CPUs                                     |
| `--cpuset-cpus`                                              |         | CPUs in which to allow execution (0-3, 0,1)                  |
| `--cpuset-mems`                                              |         | MEMs in which to allow execution (0-3, 0,1)                  |
| [`-m, --memory`](https://docs.docker.com/reference/cli/docker/container/update/#memory) |         | Memory limit                                                 |
| `--memory-reservation`                                       |         | Memory soft limit                                            |
| `--memory-swap`                                              |         | Swap limit equal to memory plus swap: -1 to enable unlimited swap |
| `--pids-limit`                                               |         | API 1.40+ Tune container pids limit (set -1 for unlimited)   |
| [`--restart`](https://docs.docker.com/reference/cli/docker/container/update/#restart) |         | Restart policy to apply when a container exits               |

## Examples

The following sections illustrate ways to use this command.

### Update a container's cpu-shares (--cpu-shares)

To limit a container's cpu-shares to 512, first identify the container name or ID. You can use `docker ps` to find these values. You can also use the ID returned from the `docker run` command. Then, do the following:



```console
$ docker update --cpu-shares 512 abebf7571666
```

### Update a container with cpu-shares and memory (-m, --memory)

To update multiple resource configurations for multiple containers:



```console
$ docker update --cpu-shares 512 -m 300M abebf7571666 hopeful_morse
```

### Update a container's kernel memory constraints (--kernel-memory)

You can update a container's kernel memory limit using the `--kernel-memory` option. On kernel version older than 4.6, this option can be updated on a running container only if the container was started with `--kernel-memory`. If the container was started without `--kernel-memory` you need to stop the container before updating kernel memory.

> **Note**
>
> The `--kernel-memory` option has been deprecated since Docker 20.10.

For example, if you started a container with this command:



```console
$ docker run -dit --name test --kernel-memory 50M ubuntu bash
```

You can update kernel memory while the container is running:



```console
$ docker update --kernel-memory 80M test
```

If you started a container without kernel memory initialized:



```console
$ docker run -dit --name test2 --memory 300M ubuntu bash
```

Update kernel memory of running container `test2` will fail. You need to stop the container before updating the `--kernel-memory` setting. The next time you start it, the container uses the new value.

Kernel version newer than (include) 4.6 does not have this limitation, you can use `--kernel-memory` the same way as other options.

### Update a container's restart policy (--restart)

You can change a container's restart policy on a running container. The new restart policy takes effect instantly after you run `docker update` on a container.

To update restart policy for one or more containers:



```console
$ docker update --restart=on-failure:3 abebf7571666 hopeful_morse
```

Note that if the container is started with `--rm` flag, you cannot update the restart policy for it. The `AutoRemove` and `RestartPolicy` are mutually exclusive for the container.
