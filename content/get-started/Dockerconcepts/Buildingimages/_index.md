+++
title = "Building images"
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

# Building images

Building container images is both technical and an art. You want to keep the image small and focused to increase your security posture, but also need to balance potential tradeoffs, such as caching impacts. In this series, you’ll deep dive into the secrets of images, how they are built and best practices.

**Skill level**Beginner

**Time to complete**25 minutes

**Prerequisites**None

## About this series

Learn how to build production-ready images that are lean and efficient Docker images, essential for minimizing overhead and enhancing deployment in production environments.

## What you'll learn

- Understanding image layers
- Writing a Dockerfile
- Build, tag and publish an image
- Using the build cache
- Multi-stage builds

## Modules

Have you ever wondered how images work? This guide will help you to understand image layers - the fundamental building blocks of container images. You'll gain a comprehensive understanding of how layers are created, stacked, and utilized to ensure efficient and optimized containers.

[Start]({{< ref "/get-started/Dockerconcepts/Buildingimages/Understandingtheimagelayers" >}})

Mastering Dockerfile practices is vital for leveraging container technology effectively, enhancing application reliability and supporting DevOps and CI/CD methodologies. In this guide, you’ll learn how to write a Dockerfile, how to define a base image and setup instructions, including software installation and copying necessary files.

[Start]({{< ref "/get-started/Dockerconcepts/Buildingimages/WritingaDockerfile" >}})

Building, tagging, and publishing Docker images are key steps in the containerization workflow. In this guide, you’ll learn how to create Docker images, how to tag those images with a unique identifier, and how to publish your image to a public registry.

[Start]({{< ref "/get-started/Dockerconcepts/Buildingimages/Buildtagandpublishanimage" >}})

Using the build cache effectively allows you to achieve faster builds by reusing results from previous builds and skipping unnecessary steps. To maximize cache usage and avoid resource-intensive and time-consuming rebuilds, it's crucial to understand how cache invalidation works. In this guide, you’ll learn how to use the Docker build cache efficiently for streamlined Docker image development and continuous integration workflows.

[Start]({{< ref "/get-started/Dockerconcepts/Buildingimages/Usingthebuildcache" >}})

By separating the build environment from the final runtime environment, you can significantly reduce the image size and attack surface. In this guide, you'll unlock the power of multi-stage builds to create lean and efficient Docker images, essential for minimizing overhead and enhancing deployment in production environments.

[Start]({{< ref "/get-started/Dockerconcepts/Buildingimages/Multi-stagebuilds" >}})
