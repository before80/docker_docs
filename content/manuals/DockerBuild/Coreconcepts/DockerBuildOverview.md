+++
title = "Docker Build 概述"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/concepts/overview/](https://docs.docker.com/build/concepts/overview/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Overview of Docker Build - Docker Build 概述

Docker Build is one of Docker Engine's most used features. Whenever you are creating an image you are using Docker Build. Build is a key part of your software development life cycle allowing you to package and bundle your code and ship it anywhere.

​	Docker Build 是 Docker 引擎最常用的功能之一。每当您创建镜像时，实际上是在使用 Docker Build。Build 是软件开发生命周期中的一个关键部分，它使您能够打包和封装代码，并将其发布到任何地方。

Docker Build is more than a command for building images, and it's not only about packaging your code. It's a whole ecosystem of tools and features that support not only common workflow tasks but also provides support for more complex and advanced scenarios.

​	Docker Build 不仅仅是一个构建镜像的命令，也不仅仅是打包代码，它是一个支持常见工作流程任务的工具和功能的生态系统，同时还支持更复杂和高级的场景。



打包您的软件 Packaging your software

Build and package your application to run it anywhere: locally or in the cloud.

​	构建并打包您的应用程序，使其可以在本地或云端运行。



多阶段构建 Multi-stage builds

Keep your images small and secure with minimal dependencies.

​	通过最少的依赖项使您的镜像更小、更安全。



多平台镜像 Multi-platform images

Build, push, pull, and run images seamlessly on different computer architectures.

​	在不同的计算架构上无缝构建、推送、拉取和运行镜像。



BuildKit

Explore BuildKit, the open source build engine.

​	探索 BuildKit，这是一种开源的构建引擎。



构建驱动 Build drivers

Configure where and how you run your builds.

​	配置构建运行的位置和方式。



Exporters

Export any artifact you like, not just Docker images.

​	导出您想要的任何产物，而不仅仅是 Docker 镜像。

Build caching

Avoid unnecessary repetitions of costly operations, such as package installs.

​	避免重复进行成本高昂的操作，例如安装软件包。



Bake

Orchestrate your builds with Bake.

​	使用 Bake 编排您的构建。
