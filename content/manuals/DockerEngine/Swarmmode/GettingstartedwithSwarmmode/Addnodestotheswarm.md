+++
title = "Add nodes to the swarm"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/engine/swarm/swarm-tutorial/add-nodes/](https://docs.docker.com/engine/swarm/swarm-tutorial/add-nodes/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Add nodes to the swarm

Once you've [created a swarm]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Createaswarm" >}}) with a manager node, you're ready to add worker nodes.

1. Open a terminal and ssh into the machine where you want to run a worker node. This tutorial uses the name `worker1`.

2. Run the command produced by the `docker swarm init` output from the [Create a swarm]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Createaswarm" >}}) tutorial step to create a worker node joined to the existing swarm:

   

   ```console
   $ docker swarm join \
     --token  SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
     192.168.99.100:2377
   
   This node joined a swarm as a worker.
   ```

   If you don't have the command available, you can run the following command on a manager node to retrieve the join command for a worker:

   

   ```console
   $ docker swarm join-token worker
   
   To add a worker to this swarm, run the following command:
   
       docker swarm join \
       --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
       192.168.99.100:2377
   ```

3. Open a terminal and ssh into the machine where you want to run a second worker node. This tutorial uses the name `worker2`.

4. Run the command produced by the `docker swarm init` output from the [Create a swarm]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Createaswarm" >}}) tutorial step to create a second worker node joined to the existing swarm:

   

   ```console
   $ docker swarm join \
     --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
     192.168.99.100:2377
   
   This node joined a swarm as a worker.
   ```

5. Open a terminal and ssh into the machine where the manager node runs and run the `docker node ls` command to see the worker nodes:

   

   ```console
   $ docker node ls
   ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
   03g1y59jwfg7cf99w4lt0f662    worker2   Ready   Active
   9j68exjopxe7wfl6yuxml7a7j    worker1   Ready   Active
   dxn1zf6l61qsb1josjja83ngz *  manager1  Ready   Active        Leader
   ```

   The `MANAGER` column identifies the manager nodes in the swarm. The empty status in this column for `worker1` and `worker2` identifies them as worker nodes.

   Swarm management commands like `docker node ls` only work on manager nodes.

## What's next?

Now your swarm consists of a manager and two worker nodes. Next, you'll deploy a service.

[Deploy a service]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode/Deployaservicetotheswarm" >}})
