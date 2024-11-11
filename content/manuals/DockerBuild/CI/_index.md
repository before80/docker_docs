+++
title = "CI"
date = 2024-10-23T14:54:40+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/build/ci/](https://docs.docker.com/build/ci/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Continuous integration with Docker - 使用 Docker 实现持续集成

Continuous Integration (CI) is the part of the development process where you're looking to get your code changes merged with the main branch of the project. At this point, development teams run tests and builds to vet that the code changes don't cause any unwanted or unexpected behaviors.

​	持续集成（CI）是开发过程的一部分，此时您希望将代码更改合并到项目的主分支。此阶段，开发团队会运行测试和构建，以确保代码更改不会引发任何不想要的或意外的行为。

![Git branches about to get merged](_index_img/continuous-integration.svg)

There are several uses for Docker at this stage of development, even if you don't end up packaging your application as a container image.

​	在这个开发阶段，即使不将应用程序打包为容器镜像，也有多种方式可以使用 Docker。

## Docker 作为构建环境 Docker as a build environment

Containers are reproducible, isolated environments that yield predictable results. Building and testing your application in a Docker container makes it easier to prevent unexpected behaviors from occurring. Using a Dockerfile, you define the exact requirements for the build environment, including programming runtimes, operating system, binaries, and more.

​	容器是可复现的、隔离的环境，可以产生可预测的结果。在 Docker 容器中构建和测试应用程序，可以更轻松地防止意外行为的发生。通过 Dockerfile，您可以定义构建环境的确切需求，包括编程运行时、操作系统、二进制文件等。

Using Docker to manage your build environment also eases maintenance. For example, updating to a new version of a programming runtime can be as simple as changing a tag or digest in a Dockerfile. No need to SSH into a pet VM to manually reinstall a newer version and update the related configuration files.

​	使用 Docker 管理构建环境还可以简化维护。例如，更新到新版本的编程运行时只需更改 Dockerfile 中的标签或摘要，无需 SSH 登录到虚拟机手动重新安装较新版本并更新相关配置文件。

Additionally, just as you expect third-party open source packages to be secure, the same should go for your build environment. You can scan and index a builder image, just like you would for any other containerized application.

​	此外，正如您期望第三方开源软件包是安全的一样，构建环境也应如此。您可以像对待其他容器化应用程序一样扫描和索引构建镜像。

The following links provide instructions for how you can get started using Docker for building your applications in CI:

​	以下链接提供了如何在 CI 中使用 Docker 构建应用程序的说明：

- [GitHub Actions](https://docs.github.com/en/actions/creating-actions/creating-a-docker-container-action)
- [GitLab](https://docs.gitlab.com/runner/executors/docker.html)
- [Circle CI](https://circleci.com/docs/using-docker/)
- [Render](https://render.com/docs/docker)

### Docker in Docker

You can also use a Dockerized build environment to build container images using Docker. That is, your build environment runs inside a container which itself is equipped to run Docker builds. This method is referred to as "Docker in Docker".

​	您还可以使用 Docker 化的构建环境来构建容器镜像。这意味着您的构建环境运行在一个能够执行 Docker 构建的容器中。这种方法称为 "Docker in Docker"。

Docker provides an official [Docker image](https://hub.docker.com/_/docker) that you can use for this purpose.

​	Docker 提供了一个官方的 [Docker 镜像](https://hub.docker.com/_/docker)，您可以用于此目的。

## What's next

Docker maintains a set of official GitHub Actions that you can use to build, annotate, and push container images on the GitHub Actions platform. See [Introduction to GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions" >}}) to learn more and get started.

​	Docker 维护了一组官方的 GitHub Actions，您可以在 GitHub Actions 平台上使用它们来构建、注释和推送容器镜像。请参见 [GitHub Actions 入门]({{< ref "/manuals/DockerBuild/CI/GitHubActions" >}}) 以了解更多信息并开始使用。
