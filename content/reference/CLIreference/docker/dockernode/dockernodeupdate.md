+++
title = "docker node update"
date = 2024-10-23T14:54:43+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/node/update/](https://docs.docker.com/reference/cli/docker/node/update/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker node update

| Description | Update a node                       |
| :---------- | ----------------------------------- |
| Usage       | `docker node update [OPTIONS] NODE` |

Swarm This command works with the Swarm orchestrator.

## Description

Update metadata about a node, such as its availability, labels, or roles.

> **Note**
>
> This is a cluster management command, and must be executed on a swarm manager node. To learn about managers and workers, refer to the [Swarm mode section]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) in the documentation.

## Options

| Option                                                       | Default | Description                                           |
| ------------------------------------------------------------ | ------- | ----------------------------------------------------- |
| `--availability`                                             |         | Availability of the node (`active`, `pause`, `drain`) |
| [`--label-add`](https://docs.docker.com/reference/cli/docker/node/update/#label-add) |         | Add or update a node label (`key=value`)              |
| `--label-rm`                                                 |         | Remove a node label if exists                         |
| `--role`                                                     |         | Role of the node (`worker`, `manager`)                |

## Examples

### Add label metadata to a node (--label-add)

Add metadata to a swarm node using node labels. You can specify a node label as a key with an empty value:



```bash
$ docker node update --label-add foo worker1
```

To add multiple labels to a node, pass the `--label-add` flag for each label:



```console
$ docker node update --label-add foo --label-add bar worker1
```

When you [create a service]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}}), you can use node labels as a constraint. A constraint limits the nodes where the scheduler deploys tasks for a service.

For example, to add a `type` label to identify nodes where the scheduler should deploy message queue service tasks:



```bash
$ docker node update --label-add type=queue worker1
```

The labels you set for nodes using `docker node update` apply only to the node entity within the swarm. Do not confuse them with the docker daemon labels for [dockerd]({{< ref "/reference/CLIreference/dockerd" >}}).

For more information about labels, refer to [apply custom metadata](https://docs.docker.com/engine/userguide/labels-custom-metadata/).
