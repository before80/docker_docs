+++
title = "docker swarm unlock-key"
date = 2024-10-23T14:54:43+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/swarm/unlock-key/](https://docs.docker.com/reference/cli/docker/swarm/unlock-key/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker swarm unlock-key

| Description | Manage the unlock key               |
| :---------- | ----------------------------------- |
| Usage       | `docker swarm unlock-key [OPTIONS]` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 协调器。

## Description

An unlock key is a secret key needed to unlock a manager after its Docker daemon restarts. These keys are only used when the autolock feature is enabled for the swarm.

​	解锁密钥是一种在 Docker 守护进程重新启动后解锁管理节点所需的密钥。这些密钥仅在 Swarm 启用了自动锁定功能时使用。

You can view or rotate the unlock key using `swarm unlock-key`. To view the key, run the `docker swarm unlock-key` command without any arguments:

​	可以使用 `swarm unlock-key` 查看或更换解锁密钥。要查看密钥，运行不带任何参数的 `docker swarm unlock-key` 命令：

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理器和工作节点，请参阅 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Options

| Option        | Default | Description        |
| ------------- | ------- | ------------------ |
| `-q, --quiet` |         | Only display token |
| `--rotate`    |         | Rotate unlock key  |

## Examples



```console
$ docker swarm unlock-key

To unlock a swarm manager after it restarts, run the `docker swarm unlock`
command and provide the following key:

    SWMKEY-1-fySn8TY4w5lKcWcJPIpKufejh9hxx5KYwx6XZigx3Q4

Remember to store this key in a password manager, since without it you
will not be able to restart the manager.
```

Use the `--rotate` flag to rotate the unlock key to a new, randomly-generated key:

​	使用 `--rotate` 标志将解锁密钥旋转为一个新的随机生成的密钥：

```console
$ docker swarm unlock-key --rotate

Successfully rotated manager unlock key.

To unlock a swarm manager after it restarts, run the `docker swarm unlock`
command and provide the following key:

    SWMKEY-1-7c37Cc8654o6p38HnroywCi19pllOnGtbdZEgtKxZu8

Remember to store this key in a password manager, since without it you
will not be able to restart the manager.
```

The `-q` (or `--quiet`) flag only prints the key:

​	`-q`（或 `--quiet`）标志仅输出密钥：

```console
$ docker swarm unlock-key -q

SWMKEY-1-7c37Cc8654o6p38HnroywCi19pllOnGtbdZEgtKxZu8
```

### `--rotate`

This flag rotates the unlock key, replacing it with a new randomly-generated key. The old unlock key will no longer be accepted.

​	此标志旋转解锁密钥，用一个新的随机生成的密钥替换旧密钥。旧的解锁密钥将不再被接受。

### `--quiet`

Only print the unlock key, without instructions.

​	仅输出解锁密钥，不带说明。
