+++
title = "使用多个 Compose 文件"
date = 2024-10-23T14:54:40+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/how-tos/multiple-compose-files/](https://docs.docker.com/compose/how-tos/multiple-compose-files/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use multiple Compose files - 使用多个 Compose 文件

This section contains information on the ways you can work with multiple Compose files.

​	本节介绍如何使用多个 Compose 文件。

Using multiple Compose files lets you customize a Compose application for different environments or workflows. This is useful for large applications that may use dozens of containers, with ownership distributed across multiple teams. For example, if your organization or team uses a monorepo, each team may have their own “local” Compose file to run a subset of the application. They then need to rely on other teams to provide a reference Compose file that defines the expected way to run their own subset. Complexity moves from the code in to the infrastructure and the configuration file.

​	使用多个 Compose 文件可以针对不同环境或工作流定制 Compose 应用。这对于使用多个容器的大型应用程序尤其有用，并且适合由多个团队负责不同部分的情况。例如，如果您的组织或团队使用单一代码库（monorepo），每个团队可以拥有自己的“本地”Compose 文件来运行应用程序的一个子集。然后，他们需要依赖其他团队提供一个参考 Compose 文件，以定义如何运行他们的子集。这样一来，复杂性从代码转移到了基础设施和配置文件上。

The quickest way to work with multiple Compose files is to [merge]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Merge" >}}) Compose files using the `-f` flag in the command line to list out your desired Compose files. However, [merging rules](https://docs.docker.com/compose/how-tos/multiple-compose-files/merge/#merging-rules) means this can soon get quite complicated.

​	使用多个 Compose 文件的最快方式是使用命令行的 `-f` 标志[合并]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Merge" >}})所需的 Compose 文件。然而，[合并规则](https://docs.docker.com/compose/how-tos/multiple-compose-files/merge/#merging-rules)会导致配置变得复杂。

Docker Compose provides two other options to manage this complexity when working with multiple Compose files. Depending on your project's needs, you can:

​	Docker Compose 提供了两种其他方法来在处理多个 Compose 文件时管理这种复杂性。根据项目需求，您可以：

- [Extend a Compose file]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Extend" >}}) by referring to another Compose file and selecting the bits you want to use in your own application, with the ability to override some attributes.
  - [扩展 Compose 文件]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Extend" >}})，通过引用另一个 Compose 文件并选择要用于自己应用的部分，同时可以覆盖某些属性。
- [Include other Compose files]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Include" >}}) directly in your Compose file.
  - [包含其他 Compose 文件]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Include" >}})直接在您的 Compose 文件中。
