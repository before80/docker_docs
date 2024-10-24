+++
title = "Swarm mode"
date = 2024-10-23T14:54:40+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/engine/swarm/](https://docs.docker.com/engine/swarm/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Swarm mode

> **Note**
>
> 
>
> Swarm mode is an advanced feature for managing a cluster of Docker daemons.
>
> Use Swarm mode if you intend to use Swarm as a production runtime environment.
>
> If you're not planning on deploying with Swarm, use [Docker Compose]({{< ref "/manuals/DockerCompose" >}}) instead. If you're developing for a Kubernetes deployment, consider using the [integrated Kubernetes feature]({{< ref "/manuals/DockerDesktop/DeployonKuberneteswithDockerDesktop" >}}) in Docker Desktop.

Current versions of Docker include Swarm mode for natively managing a cluster of Docker Engines called a swarm. Use the Docker CLI to create a swarm, deploy application services to a swarm, and manage swarm behavior.

Docker Swarm mode is built into the Docker Engine. Do not confuse Docker Swarm mode with [Docker Classic Swarm](https://github.com/docker/classicswarm) which is no longer actively developed.

## Feature highlights

### Cluster management integrated with Docker Engine

Use the Docker Engine CLI to create a swarm of Docker Engines where you can deploy application services. You don't need additional orchestration software to create or manage a swarm.

### Decentralized design

Instead of handling differentiation between node roles at deployment time, the Docker Engine handles any specialization at runtime. You can deploy both kinds of nodes, managers and workers, using the Docker Engine. This means you can build an entire swarm from a single disk image.

### Declarative service model

Docker Engine uses a declarative approach to let you define the desired state of the various services in your application stack. For example, you might describe an application comprised of a web front end service with message queueing services and a database backend.

### Scaling

For each service, you can declare the number of tasks you want to run. When you scale up or down, the swarm manager automatically adapts by adding or removing tasks to maintain the desired state.

### Desired state reconciliation

The swarm manager node constantly monitors the cluster state and reconciles any differences between the actual state and your expressed desired state. For example, if you set up a service to run 10 replicas of a container, and a worker machine hosting two of those replicas crashes, the manager creates two new replicas to replace the replicas that crashed. The swarm manager assigns the new replicas to workers that are running and available.

### Multi-host networking

You can specify an overlay network for your services. The swarm manager automatically assigns addresses to the containers on the overlay network when it initializes or updates the application.

### Service discovery

Swarm manager nodes assign each service in the swarm a unique DNS name and load balance running containers. You can query every container running in the swarm through a DNS server embedded in the swarm.

### Load balancing

You can expose the ports for services to an external load balancer. Internally, the swarm lets you specify how to distribute service containers between nodes.

### Secure by default

Each node in the swarm enforces TLS mutual authentication and encryption to secure communications between itself and all other nodes. You have the option to use self-signed root certificates or certificates from a custom root CA.

### Rolling updates

At rollout time you can apply service updates to nodes incrementally. The swarm manager lets you control the delay between service deployment to different sets of nodes. If anything goes wrong, you can roll back to a previous version of the service.

## What's next?

- Learn Swarm mode [key concepts]({{< ref "/manuals/DockerEngine/Swarmmode/Swarmmodekeyconcepts" >}}).
- Get started with the [Swarm mode tutorial]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode" >}}).
- Explore Swarm mode CLI commands
  - [swarm init]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarminit" >}})
  - [swarm join]({{< ref "/reference/CLIreference/docker/dockerswarm/dockerswarmjoin" >}})
  - [service create]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}})
  - [service inspect]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceinspect" >}})
  - [service ls]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicels" >}})
  - [service rm]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicerm" >}})
  - [service scale]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicescale" >}})
  - [service ps]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceps" >}})
  - [service update]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceupdate" >}})
