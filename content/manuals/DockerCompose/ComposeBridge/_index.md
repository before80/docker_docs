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

# Overview of Compose Bridge - Compose Bridge 概述

**Experimental 实验性功能 **

Compose Bridge is an [Experimental](https://docs.docker.com/release-lifecycle/#experimental) product.

​	Compose Bridge 是一个[实验性](https://docs.docker.com/release-lifecycle/#experimental)产品。

Compose Bridge lets you transform your Compose configuration file into configuration files for different platforms, primarily focusing on Kubernetes. The default transformation generates Kubernetes manifests and a Kustomize overlay which are designed for deployment on Docker Desktop with Kubernetes enabled.

​	Compose Bridge 允许您将 Compose 配置文件转换为不同平台的配置文件，主要针对 Kubernetes。默认的转换会生成 Kubernetes 清单和一个 Kustomize 覆盖文件，适用于启用了 Kubernetes 的 Docker Desktop 部署。

It's a flexible tool that lets you either take advantage of the [default transformation]({{< ref "/manuals/DockerCompose/ComposeBridge/Usage" >}}) or [create a custom transformation]({{< ref "/manuals/DockerCompose/ComposeBridge/Customize" >}}) to suit specific project needs and requirements.

​	这是一个灵活的工具，您可以利用[默认转换]({{< ref "/manuals/DockerCompose/ComposeBridge/Usage" >}})或者[创建自定义转换]({{< ref "/manuals/DockerCompose/ComposeBridge/Customize" >}})，以满足特定项目需求。

Compose Bridge significantly simplifies the transition from Docker Compose to Kubernetes, making it easier for you to leverage the power of Kubernetes while maintaining the simplicity and efficiency of Docker Compose.

​	Compose Bridge 大大简化了从 Docker Compose 迁移到 Kubernetes 的过程，使您在保持 Docker Compose 简单高效的同时，更轻松地利用 Kubernetes 的强大功能。

## 工作原理 How it works

Compose Bridge uses transformations to let you convert a Compose model into another form.

​	Compose Bridge 使用转换来让您将 Compose 模型转化为另一种格式。

A transformation is packaged as a Docker image that receives the fully resolved Compose model as `/in/compose.yaml` and can produce any target format file under `/out`.

​	转换以 Docker 镜像的形式打包，接收完全解析的 Compose 模型（路径为 `/in/compose.yaml`），并可以生成任何目标格式文件到 `/out` 下。

Compose Bridge provides its own transformation for Kubernetes using Go templates, so that it is easy to extend for customization by replacing or appending your own templates.

​	Compose Bridge 提供了一个基于 Go 模板的 Kubernetes 转换，以便您可以通过替换或添加自定义模板轻松扩展。

For more detailed information on how these transformations work and how you can customize them for your projects, see [Customize]({{< ref "/manuals/DockerCompose/ComposeBridge/Customize" >}}).

​	有关这些转换的详细工作原理以及如何为您的项目进行自定义，请参阅[自定义]({{< ref "/manuals/DockerCompose/ComposeBridge/Customize" >}})。

## Setup

To get started with Compose Bridge, you need to:

​	要开始使用 Compose Bridge，请完成以下步骤：

1. Download and install Docker Desktop version 4.33 and later. 下载并安装 Docker Desktop 4.33 或更高版本。
2. Sign in to your Docker account. 登录到您的 Docker 帐户。
3. Navigate to the **Features in development** tab in **Settings**. 在 **设置** 中导航到 **开发中功能** 选项卡。
4. From the **Experimental features** tab, select **Enable Compose Bridge**. 在 **实验性功能** 选项卡中，选择 **启用 Compose Bridge**。

## 反馈 Feedback

To give feedback, report bugs, or receive support, email `desktop-preview@docker.com`. There is also a dedicated Slack channel. To join, simply send an email to the provided address.

​	如需提供反馈、报告错误或获取支持，请发送邮件至 `desktop-preview@docker.com`。此外，还有一个专用的 Slack 频道。要加入该频道，请发送电子邮件至上述地址。

## What's next?

- [Use Compose Bridge 使用 Compose Bridge]({{< ref "/manuals/DockerCompose/ComposeBridge/Usage" >}})
- [Explore how you can customize Compose Bridge 探索如何自定义 Compose Bridge]({{< ref "/manuals/DockerCompose/ComposeBridge/Customize" >}})
- [Explore the advanced integration 探索高级集成]({{< ref "/manuals/DockerCompose/ComposeBridge/Advanced" >}})
