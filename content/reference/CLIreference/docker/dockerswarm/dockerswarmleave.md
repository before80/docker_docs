+++
title = "docker swarm leave"
date = 2024-10-23T14:54:43+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/swarm/leave/](https://docs.docker.com/reference/cli/docker/swarm/leave/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker swarm leave

| Description | Leave the swarm                |
| :---------- | ------------------------------ |
| Usage       | `docker swarm leave [OPTIONS]` |

Swarm This command works with the Swarm orchestrator.

​	Swarm 此命令适用于 Swarm 协调器。

## Description

When you run this command on a worker, that worker leaves the swarm.

​	当您在工作节点上运行此命令时，该工作节点将离开 Swarm。

You can use the `--force` option on a manager to remove it from the swarm. However, this does not reconfigure the swarm to ensure that there are enough managers to maintain a quorum in the swarm. The safe way to remove a manager from a swarm is to demote it to a worker and then direct it to leave the quorum without using `--force`. Only use `--force` in situations where the swarm will no longer be used after the manager leaves, such as in a single-node swarm.

​	在管理节点上，可以使用 `--force` 选项将其从 Swarm 中移除。然而，这不会重新配置 Swarm 以确保有足够的管理节点来维持 Swarm 的仲裁。安全的移除管理节点方法是将其降级为工作节点，然后让它在不使用 `--force` 的情况下离开仲裁。仅在管理节点离开后 Swarm 不再使用的情况下使用 `--force`，例如在单节点 Swarm 中。

## Options

| Option        | Default | Description                                                  |
| ------------- | ------- | ------------------------------------------------------------ |
| `-f, --force` |         | 强制该节点离开 Swarm，忽略警告 Force this node to leave the swarm, ignoring warnings |

## Examples

Consider the following swarm, as seen from the manager:

​	以下是从管理节点查看的 Swarm 状态：



```console
$ docker node ls

ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
7ln70fl22uw2dvjn2ft53m3q5    worker2   Ready   Active
dkp8vy1dq1kxleu9g4u78tlag    worker1   Ready   Active
dvfxp4zseq4s0rih1selh0d20 *  manager1  Ready   Active        Leader
```

To remove `worker2`, issue the following command from `worker2` itself:

​	要移除 `worker2`，请从 `worker2` 本身执行以下命令：



```console
$ docker swarm leave

Node left the default swarm.
```

The node will still appear in the node list, and marked as `down`. It no longer affects swarm operation, but a long list of `down` nodes can clutter the node list. To remove an inactive node from the list, use the [`node rm`]({{< ref "/reference/CLIreference/docker/dockernode/dockernoderm" >}}) command.

​	该节点仍然会显示在节点列表中，并标记为 `down`。它不再影响 Swarm 的运行，但长时间的 `down` 节点列表会使节点列表变得冗长。要从列表中移除不活动节点，请使用 [`node rm`]({{< ref "/reference/CLIreference/docker/dockernode/dockernoderm" >}}) 命令。
