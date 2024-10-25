+++
title = "构建镜像"
date = 2024-10-23T14:54:35+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/get-started/docker-concepts/building-images/](https://docs.docker.com/get-started/docker-concepts/building-images/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# 构建镜像 Building images

Building container images is both technical and an art. You want to keep the image small and focused to increase your security posture, but also need to balance potential tradeoffs, such as caching impacts. In this series, you’ll deep dive into the secrets of images, how they are built and best practices.

​	构建容器镜像既是技术也是一门艺术。为了增强安全性，您希望使镜像保持小巧和专注，但也需要平衡潜在的权衡，比如缓存的影响。在本系列中，您将深入了解镜像的奥秘、它们的构建方式以及最佳实践。

**Skill level**Beginner

**Time to complete**25 minutes

**Prerequisites**None

**技能水平** 初级

**完成时间** 25分钟

**前提条件** 无

## 关于本系列 About this series

Learn how to build production-ready images that are lean and efficient Docker images, essential for minimizing overhead and enhancing deployment in production environments.

​	学习如何构建生产级的精简高效的 Docker 镜像，这对最小化开销和提升生产环境中的部署至关重要。

## 您将学到的内容 What you'll learn

- Understanding image layers 了解镜像层
- Writing a Dockerfile 编写 Dockerfile
- Build, tag and publish an image 构建、标记和发布镜像
- Using the build cache 使用构建缓存
- Multi-stage builds 
- 多阶段构建

## 模块 Modules

Have you ever wondered how images work? This guide will help you to understand image layers - the fundamental building blocks of container images. You'll gain a comprehensive understanding of how layers are created, stacked, and utilized to ensure efficient and optimized containers.

​	您是否想知道镜像是如何工作的？本指南将帮助您理解镜像层——容器镜像的基础构件。您将全面了解如何创建、堆叠和利用层以确保高效、优化的容器。

[Start]({{< ref "/get-started/Dockerconcepts/Buildingimages/Understandingtheimagelayers" >}})

Mastering Dockerfile practices is vital for leveraging container technology effectively, enhancing application reliability and supporting DevOps and CI/CD methodologies. In this guide, you’ll learn how to write a Dockerfile, how to define a base image and setup instructions, including software installation and copying necessary files.

​	掌握 Dockerfile 的编写技巧对于有效利用容器技术、提高应用程序可靠性并支持 DevOps 和 CI/CD 方法至关重要。在本指南中，您将学习如何编写 Dockerfile，定义基础镜像和设置指令，包括安装软件和复制必要文件。

[Start]({{< ref "/get-started/Dockerconcepts/Buildingimages/WritingaDockerfile" >}})

Building, tagging, and publishing Docker images are key steps in the containerization workflow. In this guide, you’ll learn how to create Docker images, how to tag those images with a unique identifier, and how to publish your image to a public registry.

​	构建、标记和发布 Docker 镜像是容器化工作流程中的关键步骤。在本指南中，您将学习如何创建 Docker 镜像、如何使用唯一标识符对这些镜像进行标记，以及如何将镜像发布到公共注册表中。

[Start]({{< ref "/get-started/Dockerconcepts/Buildingimages/Buildtagandpublishanimage" >}})

Using the build cache effectively allows you to achieve faster builds by reusing results from previous builds and skipping unnecessary steps. To maximize cache usage and avoid resource-intensive and time-consuming rebuilds, it's crucial to understand how cache invalidation works. In this guide, you’ll learn how to use the Docker build cache efficiently for streamlined Docker image development and continuous integration workflows.

​	有效使用构建缓存可以通过重用之前构建的结果并跳过不必要的步骤来加快构建速度。要最大限度地使用缓存并避免资源密集型的重建，了解缓存失效的工作方式至关重要。在本指南中，您将学习如何高效地使用 Docker 构建缓存，以简化 Docker 镜像开发和持续集成工作流程。

[Start]({{< ref "/get-started/Dockerconcepts/Buildingimages/Usingthebuildcache" >}})

By separating the build environment from the final runtime environment, you can significantly reduce the image size and attack surface. In this guide, you'll unlock the power of multi-stage builds to create lean and efficient Docker images, essential for minimizing overhead and enhancing deployment in production environments.

​	通过将构建环境与最终运行环境分离，您可以显著减小镜像大小并减少攻击面。在本指南中，您将学习如何利用多阶段构建来创建精简高效的 Docker 镜像，这对于最小化开销和提升生产环境中的部署至关重要。

[Start]({{< ref "/get-started/Dockerconcepts/Buildingimages/Multi-stagebuilds" >}})
