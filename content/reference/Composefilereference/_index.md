+++
title = "Compose 文件参考"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/reference/compose-file/](https://docs.docker.com/reference/compose-file/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Compose file reference - Compose 文件参考

> **New to Docker Compose? 刚接触 Docker Compose？**
>
> Find more information about the [key features and use cases of Docker Compose]({{< ref "/manuals/DockerCompose/IntroductiontoCompose/WhyuseCompose" >}}) or [try the quickstart guide]({{< ref "/manuals/DockerCompose/Quickstart" >}}).
>
> ​	可以查看有关 Docker Compose 的主要功能和用例 或 尝试快速入门指南。

The Compose Specification is the latest and recommended version of the Compose file format. It helps you define a [Compose file]({{< ref "/manuals/DockerCompose/IntroductiontoCompose/HowComposeworks" >}}) which is used to configure your Docker application’s services, networks, volumes, and more.

​	Compose 规范是 Compose 文件格式的最新和推荐版本。它帮助您定义一个 Compose 文件，用于配置 Docker 应用程序的服务、网络、卷等。

Legacy versions 2.x and 3.x of the Compose file format were merged into the Compose Specification. It is implemented in versions 1.27.0 and above (also known as Compose V2) of the Docker Compose CLI.

​	Compose 文件格式的旧版 2.x 和 3.x 已合并到 Compose 规范中。该规范在 Docker Compose CLI 的 1.27.0 及以上版本（也称为 Compose V2）中实现。

The Compose Specification on Docker Docs is the Docker Compose implementation. If you wish to implement your own version of the Compose Specification, see the [Compose Specification repository](https://github.com/compose-spec/compose-spec).

​	Docker 文档上的 Compose 规范是 Docker Compose 的实现版本。如果您希望实现自己的 Compose 规范版本，请参阅 [Compose 规范库](https://github.com/compose-spec/compose-spec)。

Use the following links to navigate key sections of the Compose Specification.

​	使用以下链接可导航到 Compose 规范的关键部分：



Version and name top-level element 版本和名称顶级元素

​	Understand version and name attributes for Compose.

​	了解 Compose 的版本和名称属性。

Services top-level element 服务顶级元素

Explore all services attributes for Compose.

​	探索 Compose 的所有服务属性。

Networks top-level element 网络顶级元素

Find all networks attributes for Compose.

​	查找 Compose 的所有网络属性。

Volumes top-level element 卷顶级元素

Explore all volumes attributes for Compose.

​	探索 Compose 的所有卷属性。

Configs top-level element 配置顶级元素

Find out about configs in Compose.

​	了解 Compose 中的配置项。

Secrets top-level element 密钥顶级元素

Learn about secrets in Compose.

​	学习 Compose 中的密钥相关内容。
