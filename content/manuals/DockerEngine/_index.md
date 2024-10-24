+++
title = "Docker Engine"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/engine/](https://docs.docker.com/engine/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker Engine overview

Docker Engine is an open source containerization technology for building and containerizing your applications. Docker Engine acts as a client-server application with:

- A server with a long-running daemon process [`dockerd`]({{< ref "/reference/CLIreference/dockerd" >}}).
- APIs which specify interfaces that programs can use to talk to and instruct the Docker daemon.
- A command line interface (CLI) client [`docker`]({{< ref "/reference/CLIreference/docker" >}}).

The CLI uses [Docker APIs]({{< ref "/reference/APIreference/DockerEngineAPI" >}}) to control or interact with the Docker daemon through scripting or direct CLI commands. Many other Docker applications use the underlying API and CLI. The daemon creates and manages Docker objects, such as images, containers, networks, and volumes.

For more details, see [Docker Architecture](https://docs.docker.com/get-started/docker-overview/#docker-architecture).



Install Docker Engine

Learn how to install the open source Docker Engine for your distribution.



Storage

Use persistent data with Docker containers.



Networking

Manage network connections between containers.



Container logs

Learn how to view and read container logs.



Prune

Tidy up unused resources.



Configure the daemon

Delve into the configuration options of the Docker daemon.



Rootless mode

Run Docker without root privileges.



Deprecated features

Find out what features of Docker Engine you should stop using.



Release notes

Read the release notes for the latest version.

## Licensing

The Docker Engine is licensed under the Apache License, Version 2.0. See [LICENSE](https://github.com/moby/moby/blob/master/LICENSE) for the full license text.

However, for commercial use of Docker Engine obtained via Docker Desktop within larger enterprises (exceeding 250 employees OR with annual revenue surpassing $10 million USD), a [paid subscription](https://www.docker.com/pricing/) is required.
