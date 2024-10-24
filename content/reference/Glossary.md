+++
title = "Glossary"
date = 2024-10-23T14:54:43+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/reference/glossary/](https://docs.docker.com/reference/glossary/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Glossary

## `Compose`

[Compose](https://github.com/docker/compose) is a tool for defining and running complex applications with Docker. With Compose, you define a multi-container application in a single file, then spin your application up in a single command which does everything that needs to be done to get it running. *Also known as Docker Compose*.

## `Docker`

The term Docker can refer to:

- The Docker project as a whole, which is a platform for developers and sysadmins to develop, ship, and run applications.
- The docker daemon process running on the host, which manages images and containers (also called Docker Engine).

## `Docker Business`

Docker Business is a Docker subscription. Docker Business offers centralized management and advanced security features for enterprises that use Docker at scale. It empowers leaders to manage their Docker development environments and accelerate their secure software supply chain initiatives.

## `Docker Desktop`

Docker Desktop is an easy-to-install, lightweight Docker development environment. Docker Desktop is available for [Mac](https://docs.docker.com/reference/glossary/#docker-desktop-for-mac), [Windows](https://docs.docker.com/reference/glossary/#docker-desktop-for-windows), and [Linux](https://docs.docker.com/reference/glossary/#docker-desktop-for-linux), providing developers a consistent experience across platforms. Docker Desktop includes Docker Engine, Docker CLI client, Docker Compose, Docker Content Trust, Kubernetes, and Credential Helper. Docker Desktop works with your choice of development tools and languages and gives you access to a vast library of certified images and templates in Docker Hub. This enables development teams to extend their environment to rapidly auto-build, continuously integrate, and collaborate using a secure repository.

## `Docker Desktop for Linux`

Docker Desktop for Linux is an easy-to-install, lightweight Docker development environment designed specifically for Linux machines. It's the best solution if you want to build, debug, test, package, and ship Dockerized applications on a Linux machine.

## `Docker Desktop for Mac`

Docker Desktop for Mac is an easy-to-install, lightweight Docker development environment designed specifically for the Mac. A native Mac application, Docker Desktop for Mac uses the macOS Hypervisor framework, networking, and filesystem. It's the best solution if you want to build, debug, test, package, and ship Dockerized applications on a Mac.


## `Docker Desktop for Windows`

Docker Desktop for Windows is an easy-to-install, lightweight Docker development environment designed specifically for Windows systems that support WSL 2 and Microsoft Hyper-V. Docker Desktop for Windows uses WSL 2 or Hyper-V for virtualization. Docker Desktop for Windows is the best solution if you want to build, debug, test, package, and ship Dockerized applications from Windows machines.

## `Docker Hub`

[Docker Hub](https://hub.docker.com/) is a centralized resource for working with Docker and its components. It provides the following services:

- A registry to host Docker images
- User authentication
- Automated image builds and workflow tools such as build triggers and web hooks
- Integration with GitHub and Bitbucket
- Security vulnerability scanning.

## `Docker ID`

Your free Docker ID grants you access to Docker Hub repositories and some beta programs. All you need is an email address.

## `Docker Official Images`

The Docker Official Images are a curated set of Docker repositories hosted on [Docker Hub](https://docs.docker.com/reference/glossary/#docker-hub). Docker, Inc. sponsors a dedicated team that is responsible for reviewing and publishing all content in the Docker Official Images. This team works in collaboration with upstream software maintainers, security experts, and the broader Docker community.

## `Docker Open Source Images`
Docker Open Source Images are published and maintained by organizations that are a member of the Docker Open Source Program.

## `Docker Personal`
Docker Personal is a [Docker subscription](https://docs.docker.com/reference/glossary/#docker-subscription). With its focus on the open-source communities, individual developers, education, and small businesses, Docker Personal will continue to allow free use of Docker components - including the Docker CLI, Docker Compose, Docker Engine, Docker Desktop, Docker Hub, Kubernetes, Docker Build and Docker BuildKit, Docker Official Images, Docker Scan, and more.

## `Docker Pro`
Docker Pro is a [Docker subscription](https://docs.docker.com/reference/glossary/#docker-subscription). Docker Pro enables individual developers to get more control of their development environment and provides an integrated and reliable developer experience. It reduces the amount of time developers spend on mundane and repetitive tasks and empowers developers to spend more time creating value for their customers.

## `Docker Team`
Docker Team is a [Docker subscription](https://docs.docker.com/reference/glossary/#docker-subscription). Docker Team offers capabilities for collaboration, productivity, and security across organizations. It enables groups of developers to unlock the full power of collaboration and sharing combined with essential security features and team management capabilities.

## `Docker Trusted Content Program`
The Docker Trusted Content Program verifies content through four programs:

- [Docker Official Images](https://docs.docker.com/reference/glossary/#docker-official-images)
- [Docker Verified Publisher Images](https://docs.docker.com/reference/glossary/#docker-verified-publisher-images)
- [Docker Open Source Images](https://docs.docker.com/reference/glossary/#docker-open-source-images)
- Custom Official Images.

## `Docker Verified Publisher Images`
Docker Verified Publisher Images are confirmed by Docker to be from trusted software publishers that are partners in the Verified Publisher program. Docker Verified Publisher Images are identified by the Verified Publisher badge included on the Docker Hub repositories.

## `Docker subscription`
Docker subscription tiers, sometimes referred to as plans, include [Personal](https://docs.docker.com/reference/glossary/#docker-personal), [Pro](https://docs.docker.com/reference/glossary/#docker-pro), [Team](https://docs.docker.com/reference/glossary/#docker-team), and [Business](https://docs.docker.com/reference/glossary/#docker-business). For more details, see [Docker subscription overview](https://docs.docker.com/subscription/details/).

## `Dockerfile`
A Dockerfile is a text document that contains all the commands you would normally execute manually in order to build a Docker image. Docker can build images automatically by reading the instructions from a Dockerfile.

## `ENTRYPOINT`
In a Dockerfile, an `ENTRYPOINT` is an optional definition for the first part of the command to be run. If you want your Dockerfile to be runnable without specifying additional arguments to the `docker run` command, you must specify either `ENTRYPOINT`, `CMD`, or both. If `ENTRYPOINT` is specified, it is set to a single command. Most official Docker images have an `ENTRYPOINT` of `/bin/sh` or `/bin/bash`. Even if you do not specify `ENTRYPOINT`, you may inherit it from the base image that you specify using the `FROM` keyword in your Dockerfile. To override the `ENTRYPOINT` at runtime, you can use `--entrypoint`. The following example overrides the entrypoint to be `/bin/ls` and sets the `CMD` to `-l /tmp`:
`$ docker run --entrypoint=/bin/ls ubuntu -l /tmp`
`CMD` is appended to the `ENTRYPOINT`. The `CMD` can be any arbitrary string that is valid in terms of the `ENTRYPOINT`, which allows you to pass multiple commands or flags at once. To override the `CMD` at runtime, just add it after the container name or ID. In the following example, the `CMD` is overridden to be `/bin/ls -l /tmp`:
`$ docker run ubuntu /bin/ls -l /tmp`
In practice, `ENTRYPOINT` is not often overridden. However, specifying the `ENTRYPOINT` can make your images more flexible and easier to reuse.

## `SSH`
SSH (secure shell) is a secure protocol for accessing remote machines and applications. It provides authentication and encrypts data communication over insecure networks such as the Internet. SSH uses public/private key pairs to authenticate logins.

## `Union file system`
Union file systems implement a [union mount](https://en.wikipedia.org/wiki/Union_mount) and operate by creating layers. Docker uses union file systems in conjunction with [copy-on-write](https://docs.docker.com/reference/glossary/#copy-on-write) techniques to provide the building blocks for containers, making them very lightweight and fast. For more on Docker and union file systems, see [Docker and OverlayFS in practice]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers/OverlayFSstoragedriver" >}}). Example implementations of union file systems are [UnionFS](https://en.wikipedia.org/wiki/UnionFS) and [OverlayFS](https://en.wikipedia.org/wiki/OverlayFS).

## `amd64`

AMD64 is AMD's 64-bit extension of Intel's x86 architecture, and is also referred to as x86_64 (or x86-64).

## `arm64`

ARM64 is the 64-bit extension of the ARM CPU architecture. arm64 architecture is used in Apple silicon machines.

## `Base image`
A base image is an image you designate in a `FROM` directive in a Dockerfile. It defines the starting point for your build. Dockerfile instructions create additional layers on top of the base image. A Dockerfile with the `FROM scratch` directive uses an empty base image.

## `btrfs`
btrfs (B-tree file system) is a Linux [filesystem](https://docs.docker.com/reference/glossary/#filesystem) that Docker supports as a storage backend. It is a [copy-on-write](https://en.wikipedia.org/wiki/Copy-on-write) filesystem.

## `Build`
Build is the process of building Docker images using a [Dockerfile](https://docs.docker.com/reference/glossary/#dockerfile). The build uses a Dockerfile and a "context". The context is the set of files in the directory in which the image is built.

## `Cgroups`
Cgroups is a Linux kernel feature that limits, accounts for, and isolates the resource usage (CPU, memory, disk I/O, network, etc.) of a collection of processes. Docker relies on cgroups to control and isolate resource limits. *Also known as control groups*.

## `Cluster`
A cluster is a group of machines that work together to run workloads and provide high availability.

## `Container`
A container is a runtime instance of a [docker image](https://docs.docker.com/reference/glossary/#image). A Docker container consists of:

- A Docker image
- An execution environment
- A standard set of instructions
  The concept is borrowed from shipping containers, which define a standard to ship goods globally. Docker defines a standard to ship software.

## `Container image`
Docker images are the basis of containers. An image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime. An image typically contains a union of layered filesystems stacked on top of each other.

## `Copy-on-write`

Docker uses a [copy-on-write](https://docs.docker.com/engine/storage/drivers/#the-copy-on-write-cow-strategy) technique and a [union file system](https://docs.docker.com/reference/glossary/#union-file-system) for both images and containers to optimize resources and speed performance. Multiple copies of an entity share the same instance and each one makes only specific changes to its unique layer. Multiple containers can share access to the same image, and make container-specific changes on a writable layer which is deleted when the container is removed. This speeds up container start times and performance. Images are essentially layers of filesystems typically predicated on a base image under a writable layer, and built up with layers of differences from the base image. This minimizes the footprint of the image and enables shared development. For more about copy-on-write in the context of Docker, see [Understand images, containers, and storage drivers]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers" >}}).

## `Filesystem`
A file system is the method an operating system uses to name files and assign them locations for efficient storage and retrieval.
Examples:

- Linux: overlay2, extfs, btrfs, zfs
- Windows: NTFS
- macOS: APFS.

## `Image`
Docker images are the basis of [containers](https://docs.docker.com/reference/glossary/#container). An image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime. An image typically contains a union of layered filesystems stacked on top of each other. An image does not have state and it never changes.



## `Invitee`
People who have been invited to join an [organization](https://docs.docker.com/reference/glossary/#organization), but have not yet accepted their invitation.

## `Layer`
In an image, a layer is a modification to the image, represented by an instruction in the Dockerfile. Layers are applied in sequence to the base image to create the final image. When an image is updated or rebuilt, only layers that change need to be updated, and unchanged layers are cached locally. This is part of why Docker images are so fast and lightweight. The sizes of each layer add up to equal the size of the final image.

## `Libcontainer`
Libcontainer provides a native Go implementation for creating containers with namespaces, cgroups, capabilities, and filesystem access controls. It allows you to manage the lifecycle of the container performing additional operations after the container is created.

## `Libnetwork`
Libnetwork provides a native Go implementation for creating and managing container network namespaces and other network resources. It manages the networking lifecycle of the container performing additional operations after the container is created.

## `Member`
The people who have received and accepted invitations to join an [organization](https://docs.docker.com/reference/glossary/#organization). Member can also refer to members of a [team](https://docs.docker.com/reference/glossary/#team) within an organization.

## `Namespace`
A [Linux namespace](https://man7.org/linux/man-pages/man7/namespaces.7.html) is a Linux kernel feature that isolates and virtualizes system resources. Processes which are restricted to a namespace can only interact with resources or processes that are part of the same namespace. Namespaces are an important part of Docker's isolation model. Namespaces exist for each type of resource, including `net` (networking), `mnt` (storage), `pid` (processes), `uts` (hostname control), and `user` (UID mapping). For more information about namespaces, see [Docker run reference]({{< ref "/manuals/DockerEngine/Containers/Runningcontainers" >}}) and [Isolate containers with a user namespace]({{< ref "/manuals/DockerEngine/Security/Isolatecontainerswithausernamespace" >}}).

## `Node`
A [node]({{< ref "/manuals/DockerEngine/Swarmmode/Howswarmworks/Hownodeswork" >}}) is a physical or virtual machine running an instance of the Docker Engine in [swarm mode](https://docs.docker.com/reference/glossary/#swarm-mode). Manager nodes perform swarm management and orchestration duties. By default, manager nodes are also worker nodes. Worker nodes execute tasks.

## `Organization`
An organization is a collection of teams and repositories that can be managed together. Docker users become members of an organization when they are assigned to at least one team in the organization.

## `Organization name`
The organization name, sometimes referred to as the organization namespace or the org ID, is the unique identifier of a Docker organization.

## `Overlay network driver`
The overlay network driver provides out-of-the-box multi-host network connectivity for Docker containers in a cluster.

## `Overlay storage driver`
OverlayFS is a [filesystem](https://docs.docker.com/reference/glossary/#filesystem) service for Linux which implements a [union mount](https://en.wikipedia.org/wiki/Union_mount) for other file systems. It is supported by the Docker daemon as a storage driver.

## `Persistent storage`
Persistent storage or volume storage provides a way for a user to add a persistent layer to the running container's file system. This persistent layer could live on the container host or an external device. The lifecycle of this persistent layer is not connected to the lifecycle of the container, allowing a user to retain state.

## `Registry`
A registry is a hosted service containing [repositories](https://docs.docker.com/reference/glossary/#repository) of [images](https://docs.docker.com/reference/glossary/#image) which responds to the Registry API. The default registry can be accessed using a browser at [Docker Hub](https://docs.docker.com/reference/glossary/#docker-hub) or using the `docker search` command.

## `Repository`
A repository is a set of Docker images. A repository can be shared by pushing it to a [registry](https://docs.docker.com/reference/glossary/#registry) server. The different images in the repository can be labeled using [tags](https://docs.docker.com/reference/glossary/#tag). Here is an example of the shared [nginx repository](https://hub.docker.com/_/nginx/) and its [tags](https://hub.docker.com/r/library/nginx/tags/).

## `Seats`
The number of seats refers to the number of planned members within an [organization](https://docs.docker.com/reference/glossary/#organization).

## `Service`
A [service]({{< ref "/manuals/DockerEngine/Swarmmode/Howswarmworks/Howserviceswork" >}}) is the definition of how you want to run your application containers in a swarm. At the most basic level, a service defines which container image to run in the swarm and which commands to run in the container. For orchestration purposes, the service defines the "desired state," meaning how many containers to run as tasks and constraints for deploying the containers. Frequently a service is a microservice within the context of some larger application. Examples of services might include an HTTP server, a database, or any other type of executable program that you wish to run in a distributed environment.

## `Service account`
A service account is a Docker ID used for automated management of container images or containerized applications. Service accounts are typically used in automated workflows and do not share Docker IDs with the members in a Docker Team or Docker Business subscription plan.

## `Service discovery`
Swarm mode [container discovery](https://docs.docker.com/engine/network/drivers/overlay/#container-discovery) is a DNS component internal to the swarm that automatically assigns each service on an overlay network in the swarm a VIP and DNS entry. Containers on the network share DNS mappings for the service through gossip, so any container on the network can access the service through its service name. You don’t need to expose service-specific ports to make the service available to other services on the same overlay network. The swarm’s internal load balancer automatically distributes requests to the service VIP among the active tasks.

## `Swarm`
A [swarm]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) is a cluster of one or more Docker Engines running in [swarm mode](https://docs.docker.com/reference/glossary/#swarm-mode).

## `Swarm mode`
[Swarm mode]({{< ref "/manuals/DockerEngine/Swarmmode" >}}) refers to cluster management and orchestration features embedded in Docker Engine. When you initialize a new swarm (cluster) or join nodes to a swarm, the Docker Engine runs in swarm mode.

## `Tag`
A tag is a label applied to a Docker image in a [repository](https://docs.docker.com/reference/glossary/#repository). Tags are how various images in a repository are distinguished from each other.

## `Task`
A [task](https://docs.docker.com/engine/swarm/how-swarm-mode-works/services/#tasks-and-scheduling) is the atomic unit of scheduling within a swarm. A task carries a Docker container and the commands to run inside the container. Manager nodes assign tasks to worker nodes according to the number of replicas set in the service scale.

## `Team`
A team is a group of Docker users that belong to an [organization](https://docs.docker.com/reference/glossary/#organization). An organization can have multiple teams.

## `Virtual machine`
A virtual machine is a program that emulates a complete computer and imitates dedicated hardware. It shares physical hardware resources with other users but isolates the operating system. The end user has the same experience on a Virtual Machine as they would have on dedicated hardware. Compared to containers, a virtual machine is heavier to run, provides more isolation, gets its own set of resources, and does minimal sharing. *Also known as VM*.

## `Volume`
A volume is a specially-designated directory within one or more containers that bypasses the Union File System. Volumes are designed to persist data, independent of the container's life cycle. Docker, therefore, never automatically deletes volumes when you remove a container, nor will it "garbage collect" volumes that are no longer referenced by a container. *Also known as: data volume*. There are three types of volumes:

- A **host volume** lives on the Docker host's filesystem and can be accessed from within the container.
- A **named volume** is a volume which Docker manages where on disk the volume is created, but it is given a name.
- An **anonymous volume** is similar to a named volume, however, it can be difficult to refer to the same volume over time when it is an anonymous volume. Docker handles where the files are stored.

## `x86_64`

x86_64 (or x86-64) refers to a 64-bit instruction set invented by AMD as an extension of Intel's x86 architecture. AMD calls its x86_64 architecture, AMD64, and Intel calls its implementation, Intel 64.
