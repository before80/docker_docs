+++
title = "docker swarm join-token"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/swarm/join-token/](https://docs.docker.com/reference/cli/docker/swarm/join-token/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker swarm join-token

| Description | Manage join tokens                                   |
| :---------- | ---------------------------------------------------- |
| Usage       | `docker swarm join-token [OPTIONS] (worker|manager)` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 协调器。

## Description

Join tokens are secrets that allow a node to join the swarm. There are two different join tokens available, one for the worker role and one for the manager role. You pass the token using the `--token` flag when you run [swarm join]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmjoin" >}}). Nodes use the join token only when they join the swarm.

​	加入令牌是允许节点加入 Swarm 的密钥。存在两种不同的加入令牌，一种用于工作角色，一种用于管理角色。在运行 [swarm join]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmjoin" >}}) 时，可以通过 `--token` 标志传递该令牌。节点仅在加入 Swarm 时使用加入令牌。

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.
>
> ​	这是一个集群管理命令，必须在 Swarm 管理节点上执行。要了解管理器和工作节点，请参阅 [Swarm 模式部分]({{< ref "/manuals/DockerEngine/Swarmmode" >}})。

## Options

| Option        | Default | Description        |
| ------------- | ------- | ------------------ |
| `-q, --quiet` |         | Only display token |
| `--rotate`    |         | Rotate join token  |

## Examples

You can view or rotate the join tokens using `swarm join-token`.

​	可以使用 `swarm join-token` 查看或旋转加入令牌。

As a convenience, you can pass `worker` or `manager` as an argument to `join-token` to print the full `docker swarm join` command to join a new node to the swarm:

​	为方便起见，可以将 `worker` 或 `manager` 作为参数传递给 `join-token`，以打印完整的 `docker swarm join` 命令，将新节点加入到 Swarm 中：



```console
$ docker swarm join-token worker

To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-3pu6hszjas19xyp7ghgosyx9k8atbfcr8p2is99znpy26u2lkl-1awxwuwd3z9j1z3puu7rcgdbx \
    172.17.0.2:2377

$ docker swarm join-token manager

To add a manager to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-3pu6hszjas19xyp7ghgosyx9k8atbfcr8p2is99znpy26u2lkl-7p73s1dx5in4tatdymyhg9hu2 \
    172.17.0.2:2377
```

Use the `--rotate` flag to generate a new join token for the specified role:

​	使用 `--rotate` 标志为指定角色生成新的加入令牌：

```console
$ docker swarm join-token --rotate worker

Successfully rotated worker join token.

To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-3pu6hszjas19xyp7ghgosyx9k8atbfcr8p2is99znpy26u2lkl-b30ljddcqhef9b9v4rs7mel7t \
    172.17.0.2:2377
```

After using `--rotate`, only the new token will be valid for joining with the specified role.

​	使用 `--rotate` 后，只有新生成的令牌才有效，旧令牌将不再允许指定角色的节点加入。

The `-q` (or `--quiet`) flag only prints the token:

​	`-q`（或 `--quiet`）标志仅打印令牌：

```console
$ docker swarm join-token -q worker

SWMTKN-1-3pu6hszjas19xyp7ghgosyx9k8atbfcr8p2is99znpy26u2lkl-b30ljddcqhef9b9v4rs7mel7t
```

### `--rotate`

Because tokens allow new nodes to join the swarm, you should keep them secret. Be particularly careful with manager tokens since they allow new manager nodes to join the swarm. A rogue manager has the potential to disrupt the operation of your swarm.

​	由于令牌允许新节点加入 Swarm，因此应保密存储令牌。特别要小心管理器令牌，因为它允许新管理器节点加入 Swarm。恶意的管理器可能会破坏 Swarm 的正常运行。

Rotate your swarm's join token if a token gets checked-in to version control, stolen, or a node is compromised. You may also want to periodically rotate the token to ensure any unknown token leaks do not allow a rogue node to join the swarm.

​	如果令牌被提交到版本控制、被盗或节点遭到入侵，请旋转 Swarm 的加入令牌。您还可以定期旋转令牌，以确保任何未知的令牌泄漏不会允许恶意节点加入 Swarm。

To rotate the join token and print the newly generated token, run `docker swarm join-token --rotate` and pass the role: `manager` or `worker`.

​	要旋转加入令牌并打印新生成的令牌，请运行 `docker swarm join-token --rotate` 并传递角色：`manager` 或 `worker`。

Rotating a join-token means that no new nodes will be able to join the swarm using the old token. Rotation does not affect existing nodes in the swarm because the join token is only used for authorizing new nodes joining the swarm.

​	旋转加入令牌意味着不再允许使用旧令牌加入新的节点。旋转不会影响已在 Swarm 中的现有节点，因为加入令牌仅用于授权新节点加入。

### `--quiet`

Only print the token. Do not print a complete command for joining.

​	仅打印令牌。不打印用于加入的完整命令。
