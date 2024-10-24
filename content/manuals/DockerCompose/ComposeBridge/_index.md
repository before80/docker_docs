+++
title = "Compose Bridge"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/compose/bridge/](https://docs.docker.com/compose/bridge/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Overview of Compose Bridge

**Experimental**

Compose Bridge is an [Experimental](https://docs.docker.com/release-lifecycle/#experimental) product.

Compose Bridge lets you transform your Compose configuration file into configuration files for different platforms, primarily focusing on Kubernetes. The default transformation generates Kubernetes manifests and a Kustomize overlay which are designed for deployment on Docker Desktop with Kubernetes enabled.

It's a flexible tool that lets you either take advantage of the [default transformation]({{< ref "/manuals/DockerCompose/ComposeBridge/Usage" >}}) or [create a custom transformation]({{< ref "/manuals/DockerCompose/ComposeBridge/Customize" >}}) to suit specific project needs and requirements.

Compose Bridge significantly simplifies the transition from Docker Compose to Kubernetes, making it easier for you to leverage the power of Kubernetes while maintaining the simplicity and efficiency of Docker Compose.

## How it works

Compose Bridge uses transformations to let you convert a Compose model into another form.

A transformation is packaged as a Docker image that receives the fully resolved Compose model as `/in/compose.yaml` and can produce any target format file under `/out`.

Compose Bridge provides its own transformation for Kubernetes using Go templates, so that it is easy to extend for customization by replacing or appending your own templates.

For more detailed information on how these transformations work and how you can customize them for your projects, see [Customize]({{< ref "/manuals/DockerCompose/ComposeBridge/Customize" >}}).

## Setup

To get started with Compose Bridge, you need to:

1. Download and install Docker Desktop version 4.33 and later.
2. Sign in to your Docker account.
3. Navigate to the **Features in development** tab in **Settings**.
4. From the **Experimental features** tab, select **Enable Compose Bridge**.

## Feedback

To give feedback, report bugs, or receive support, email `desktop-preview@docker.com`. There is also a dedicated Slack channel. To join, simply send an email to the provided address.

## What's next?

- [Use Compose Bridge]({{< ref "/manuals/DockerCompose/ComposeBridge/Usage" >}})
- [Explore how you can customize Compose Bridge]({{< ref "/manuals/DockerCompose/ComposeBridge/Customize" >}})
- [Explore the advanced integration]({{< ref "/manuals/DockerCompose/ComposeBridge/Advanced" >}})
