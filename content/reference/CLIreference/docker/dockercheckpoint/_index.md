+++
title = "docker checkpoint"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/checkpoint/](https://docs.docker.com/reference/cli/docker/checkpoint/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker checkpoint

| Description | Manage checkpoints  |
| :---------- | ------------------- |
| Usage       | `docker checkpoint` |

**Experimental**

**This command is experimental.**

Experimental features are intended for testing and feedback as their functionality or design may change between releases without warning or can be removed entirely in a future release.

## Description

Checkpoint and Restore is an experimental feature that allows you to freeze a running container by specifying a checkpoint, which turns the container state into a collection of files on disk. Later, the container can be restored from the point it was frozen.

​	检查点和恢复是一个实验性功能，允许您通过指定检查点来冻结正在运行的容器，这会将容器状态转换为磁盘上的一组文件。随后，可以从冻结的点恢复该容器。

This is accomplished using a tool called [CRIU](https://criu.org/), which is an external dependency of this feature. A good overview of the history of checkpoint and restore in Docker is available in this [Kubernetes blog post](https://kubernetes.io/blog/2015/07/how-did-quake-demo-from-dockercon-work/).

​	这一功能是通过名为 [CRIU](https://criu.org/) 的工具实现的，它是此功能的外部依赖。有关 Docker 中检查点和恢复历史的良好概述可以参考这篇 [Kubernetes 博客文章](https://kubernetes.io/blog/2015/07/how-did-quake-demo-from-dockercon-work/)。

### 安装 CRIU - Installing CRIU

If you use a Debian system, you can add the CRIU PPA and install with `apt-get` [from the CRIU launchpad](https://launchpad.net/~criu/+archive/ubuntu/ppa).

​	如果您使用 Debian 系统，可以添加 CRIU PPA 并通过 `apt-get` [从 CRIU 启动平台](https://launchpad.net/~criu/+archive/ubuntu/ppa) 安装。

Alternatively, you can [build CRIU from source](https://criu.org/Installation).

​	另外，您可以选择 [从源代码构建 CRIU](https://criu.org/Installation)。

You need at least version 2.0 of CRIU to run checkpoint and restore in Docker.

​	您需要至少 2.0 版本的 CRIU 才能在 Docker 中运行检查点和恢复功能。

### 检查点和恢复的用例 Use cases for checkpoint and restore

This feature is currently focused on single-host use cases for checkpoint and restore. Here are a few:

​	该功能目前专注于单主机的检查点和恢复用例。以下是几个例子：

- Restarting the host machine without stopping/starting containers
  - 在不停止/启动容器的情况下重启主机

- Speeding up the start time of slow start applications
  - 加快启动慢应用程序的时间

- "Rewinding" processes to an earlier point in time
  - 将进程“倒回”到早期的时间点

- "Forensic debugging" of running processes
  - 对运行中的进程进行“取证调试”


Another primary use case of checkpoint and restore outside of Docker is the live migration of a server from one machine to another. This is possible with the current implementation, but not currently a priority (and so the workflow is not optimized for the task).

​	检查点和恢复在 Docker 之外的另一个主要用例是将服务器从一台机器实时迁移到另一台。当前实现是可能的，但并不是优先考虑的任务（因此该工作流程并未针对该任务进行优化）。

### 使用检查点和恢复 Using checkpoint and restore

A new top level command `docker checkpoint` is introduced, with three subcommands:

​	引入了一个新的顶级命令 `docker checkpoint`，包含三个子命令：

- `docker checkpoint create` (creates a new checkpoint)
  - `docker checkpoint create`（创建新的检查点）

- `docker checkpoint ls` (lists existing checkpoints)
  - `docker checkpoint ls`（列出现有检查点）

- `docker checkpoint rm` (deletes an existing checkpoint)
  - `docker checkpoint rm`（删除现有检查点）


Additionally, a `--checkpoint` flag is added to the `docker container start` command.

​	此外，`docker container start` 命令中添加了一个 `--checkpoint` 标志。

The options for `docker checkpoint create`:

​	`docker checkpoint create` 的选项：



```console
Usage:  docker checkpoint create [OPTIONS] CONTAINER CHECKPOINT

Create a checkpoint from a running container 从正在运行的容器创建一个检查点
 
  --leave-running=false    Leave the container running after checkpoint 在检查点后保持容器运行
  --checkpoint-dir         Use a custom checkpoint storage directory 使用自定义检查点存储目录
```

And to restore a container:

​	要恢复容器：

```console
Usage:  docker start --checkpoint CHECKPOINT_ID [OTHER OPTIONS] CONTAINER
```

Example of using checkpoint and restore on a container:

​	在容器上使用检查点和恢复的示例：

```console
$ docker run --security-opt=seccomp:unconfined --name cr -d busybox /bin/sh -c 'i=0; while true; do echo $i; i=$(expr $i + 1); sleep 1; done'
abc0123

$ docker checkpoint create cr checkpoint1

# <later>
$ docker start --checkpoint checkpoint1 cr
abc0123
```

This process just logs an incrementing counter to stdout. If you run `docker logs` in-between running/checkpoint/restoring, you should see that the counter increases while the process is running, stops while it's frozen, and resumes from the point it left off once you restore.

​	这个过程只是将递增的计数器记录到标准输出。如果您在运行/检查点/恢复之间运行 `docker logs`，您应该会看到计数器在进程运行时增加，在被冻结时停止，并在恢复后从离开的点继续。

### 已知限制 Known limitations

`seccomp` is only supported by CRIU in very up-to-date kernels.

​	`seccomp` 仅在非常新版本的内核中由 CRIU 支持。

External terminals (i.e. `docker run -t ..`) aren't supported. If you try to create a checkpoint for a container with an external terminal, it fails:

​	外部终端（即 `docker run -t ..`）不被支持。如果您尝试为带有外部终端的容器创建检查点，则会失败：



```console
$ docker checkpoint create cr checkpoint1
Error response from daemon: Cannot checkpoint container c1: rpc error: code = 2 desc = exit status 1: "criu failed: type NOTIFY errno 0\nlog file: /var/lib/docker/containers/eb62ebdbf237ce1a8736d2ae3c7d88601fc0a50235b0ba767b559a1f3c5a600b/checkpoints/checkpoint1/criu.work/dump.log\n"

$ cat /var/lib/docker/containers/eb62ebdbf237ce1a8736d2ae3c7d88601fc0a50235b0ba767b559a1f3c5a600b/checkpoints/checkpoint1/criu.work/dump.log
Error (mount.c:740): mnt: 126:./dev/console doesn't have a proper root mount
```

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker checkpoint create`]({{< ref "/reference/CLIreference/docker/dockercheckpoint/dockercheckpointcreate" >}}) | 从正在运行的容器创建检查点 Create a checkpoint from a running container |
| [`docker checkpoint ls`]({{< ref "/reference/CLIreference/docker/dockercheckpoint/dockercheckpointls" >}}) | 列出容器的检查点 List checkpoints for a container            |
| [`docker checkpoint rm`]({{< ref "/reference/CLIreference/docker/dockercheckpoint/dockercheckpointrm" >}}) | 删除一个检查点 Remove a checkpoint                           |
