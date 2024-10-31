+++
title = "指定项目名称"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/compose/how-tos/project-name/](https://docs.docker.com/compose/how-tos/project-name/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Specify a project name - 指定项目名称

In Compose, the default project name is derived from the base name of the project directory. However, you have the flexibility to set a custom project name.

​	在 Compose 中，默认的项目名称源自项目目录的基本名称，但您可以灵活地设置自定义项目名称。

This page offers examples of scenarios where custom project names can be helpful, outlines the various methods to set a project name, and provides the order of precedence for each approach.

​	本页面提供了使用自定义项目名称的示例场景，概述了设置项目名称的多种方法，并按优先级顺序列出每种方法的顺序。

> **Note**
>
> 
>
> The default project directory is the base directory of the Compose file. A custom value can also be set for it using the [`--project-directory` command line option](https://docs.docker.com/reference/cli/docker/compose/#use--p-to-specify-a-project-name).
>
> ​	默认项目目录是 Compose 文件的基本目录。也可以使用 [`--project-directory` 命令行选项](https://docs.docker.com/reference/cli/docker/compose/#use--p-to-specify-a-project-name) 为其设置自定义值。

## 示例使用场景 Example use cases

Compose uses a project name to isolate environments from each other. There are multiple contexts where a project name is useful:

​	Compose 使用项目名称隔离不同的环境。项目名称在以下多个上下文中非常有用：

- On a development host: Create multiple copies of a single environment, useful for running stable copies for each feature branch of a project.
  - 在开发主机上：创建同一环境的多个副本，适用于为项目的每个功能分支运行稳定的副本。

- On a CI server: Prevent interference between builds by setting the project name to a unique build number.
  - 在 CI 服务器上：通过将项目名称设置为唯一的构建编号，防止构建之间的干扰。

- On a shared or development host: Avoid interference between different projects that might share the same service names.
  - 在共享或开发主机上：避免不同项目之间可能共享相同服务名称的干扰。


## 设置项目名称 Set a project name

Project names must contain only lowercase letters, decimal digits, dashes, and underscores, and must begin with a lowercase letter or decimal digit. If the base name of the project directory or current directory violates this constraint, alternative mechanisms are available.

xxxxxxxxxx5 1services:2  app:3    image: backend4    pre_stop:5      - command: ./data_flush.shyaml

The precedence order for each method, from highest to lowest, is as follows:

​	各方法的优先级顺序（从高到低）如下：

1. The `-p` command line flag. `-p` 命令行标志。
2. The [COMPOSE_PROJECT_NAME environment variable]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Pre-definedenvironmentvariables" >}}). [COMPOSE_PROJECT_NAME 环境变量]({{< ref "/manuals/DockerCompose/How-tos/Useenvironmentvariables/Pre-definedenvironmentvariables" >}})。
3. The [top-level `name:` attribute]({{< ref "/reference/Composefilereference/Versionandnametop-levelelements" >}}) in your Compose file. Or the last `name:` if you [specify multiple Compose files]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Merge" >}}) in the command line with the `-f` flag. Compose 文件中的 [顶级 `name:` 属性]({{< ref "/reference/Composefilereference/Versionandnametop-levelelements" >}})。如果在命令行中使用 `-f` 标志 [指定多个 Compose 文件]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Merge" >}})，则取最后一个 `name:`。
4. The base name of the project directory containing your Compose file. Or the base name of the first Compose file if you [specify multiple Compose files]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Merge" >}}) in the command line with the `-f` flag. 包含 Compose 文件的项目目录的基本名称。如果在命令行中使用 `-f` 标志 [指定多个 Compose 文件]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Merge" >}})，则取第一个 Compose 文件的基本名称。
5. The base name of the current directory if no Compose file is specified. 如果未指定 Compose 文件，则取当前目录的基本名称。

## What's next?

- Read up on [working with multiple Compose files]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles" >}}).
  - 阅读 使用多个 Compose 文件 的相关内容。
- Explore some [sample apps]({{< ref "/manuals/DockerCompose/Supportandfeedback/Sampleapps" >}}).
  - 探索一些 示例应用。
