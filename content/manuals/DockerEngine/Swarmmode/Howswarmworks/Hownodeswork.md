+++
title = "How nodes work"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/](https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# How nodes work

Swarm mode lets you create a cluster of one or more Docker Engines called a swarm. A swarm consists of one or more nodes: physical or virtual machines running Docker Engine.

There are two types of nodes: [managers](https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/#manager-nodes) and [workers](https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/#worker-nodes).

![Swarm mode cluster](Hownodeswork_img/swarm-diagram.webp)

If you haven't already, read through the [Swarm mode overview]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) and [key concepts]({{< ref "/manuals/DockerEngine/Swarmmode/Swarmmodekeyconcepts" >}}).

## Manager nodes

Manager nodes handle cluster management tasks:

- Maintaining cluster state
- Scheduling services
- Serving Swarm mode [HTTP API endpoints]({{< ref "/reference/APIreference/DockerEngineAPI" >}})

Using a [Raft](https://raft.github.io/raft.pdf) implementation, the managers maintain a consistent internal state of the entire swarm and all the services running on it. For testing purposes it is OK to run a swarm with a single manager. If the manager in a single-manager swarm fails, your services continue to run, but you need to create a new cluster to recover.

To take advantage of Swarm mode's fault-tolerance features, we recommend you implement an odd number of nodes according to your organization's high-availability requirements. When you have multiple managers you can recover from the failure of a manager node without downtime.

- A three-manager swarm tolerates a maximum loss of one manager.

- A five-manager swarm tolerates a maximum simultaneous loss of two manager nodes.

- An odd number `N` of manager nodes in the cluster tolerates the loss of at most `(N-1)/2` managers. Docker recommends a maximum of seven manager nodes for a swarm.

  > **Important**
  >
  > Adding more managers does NOT mean increased scalability or higher performance. In general, the opposite is true.

## Worker nodes

Worker nodes are also instances of Docker Engine whose sole purpose is to execute containers. Worker nodes don't participate in the Raft distributed state, make scheduling decisions, or serve the swarm mode HTTP API.

You can create a swarm of one manager node, but you cannot have a worker node without at least one manager node. By default, all managers are also workers. In a single manager node cluster, you can run commands like `docker service create` and the scheduler places all tasks on the local engine.

To prevent the scheduler from placing tasks on a manager node in a multi-node swarm, set the availability for the manager node to `Drain`. The scheduler gracefully stops tasks on nodes in `Drain` mode and schedules the tasks on an `Active` node. The scheduler does not assign new tasks to nodes with `Drain` availability.

Refer to the [`docker node update`]({{< ref "/reference/CLIreference/docker/dockernode/dockernodeupdate" >}}) command line reference to see how to change node availability.

## Change roles

You can promote a worker node to be a manager by running `docker node promote`. For example, you may want to promote a worker node when you take a manager node offline for maintenance. See [node promote]({{< ref "/reference/CLIreference/docker/dockernode/dockernodepromote" >}}).

You can also demote a manager node to a worker node. See [node demote]({{< ref "/reference/CLIreference/docker/dockernode/dockernodedemote" >}}).

## Learn more

- Read about how Swarm mode [services]({{< ref "/manuals/DockerEngine/Swarmmode/Howswarmworks/Howserviceswork" >}}) work.
- Learn how [PKI]({{< ref "/manuals/DockerEngine/Swarmmode/Howswarmworks/ManageswarmsecuritywithpublickeyinfrastructurePKI" >}}) works in Swarm mode.
