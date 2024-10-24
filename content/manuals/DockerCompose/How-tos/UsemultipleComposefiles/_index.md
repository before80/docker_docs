+++
title = "Use multiple Compose files"
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

# Use multiple Compose files

This section contains information on the ways you can work with multiple Compose files.

Using multiple Compose files lets you customize a Compose application for different environments or workflows. This is useful for large applications that may use dozens of containers, with ownership distributed across multiple teams. For example, if your organization or team uses a monorepo, each team may have their own “local” Compose file to run a subset of the application. They then need to rely on other teams to provide a reference Compose file that defines the expected way to run their own subset. Complexity moves from the code in to the infrastructure and the configuration file.

The quickest way to work with multiple Compose files is to [merge]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Merge" >}}) Compose files using the `-f` flag in the command line to list out your desired Compose files. However, [merging rules](https://docs.docker.com/compose/how-tos/multiple-compose-files/merge/#merging-rules) means this can soon get quite complicated.

Docker Compose provides two other options to manage this complexity when working with multiple Compose files. Depending on your project's needs, you can:

- [Extend a Compose file]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Extend" >}}) by referring to another Compose file and selecting the bits you want to use in your own application, with the ability to override some attributes.
- [Include other Compose files]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Include" >}}) directly in your Compose file.
