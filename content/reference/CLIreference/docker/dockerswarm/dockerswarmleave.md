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

## Description

When you run this command on a worker, that worker leaves the swarm.

You can use the `--force` option on a manager to remove it from the swarm. However, this does not reconfigure the swarm to ensure that there are enough managers to maintain a quorum in the swarm. The safe way to remove a manager from a swarm is to demote it to a worker and then direct it to leave the quorum without using `--force`. Only use `--force` in situations where the swarm will no longer be used after the manager leaves, such as in a single-node swarm.

## Options

| Option        | Default | Description                                           |
| ------------- | ------- | ----------------------------------------------------- |
| `-f, --force` |         | Force this node to leave the swarm, ignoring warnings |

## Examples

Consider the following swarm, as seen from the manager:



```console
$ docker node ls

ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
7ln70fl22uw2dvjn2ft53m3q5    worker2   Ready   Active
dkp8vy1dq1kxleu9g4u78tlag    worker1   Ready   Active
dvfxp4zseq4s0rih1selh0d20 *  manager1  Ready   Active        Leader
```

To remove `worker2`, issue the following command from `worker2` itself:



```console
$ docker swarm leave

Node left the default swarm.
```

The node will still appear in the node list, and marked as `down`. It no longer affects swarm operation, but a long list of `down` nodes can clutter the node list. To remove an inactive node from the list, use the [`node rm`]({{< ref "/reference/CLIreference/docker/dockernode/dockernoderm" >}}) command.
