+++
title = "docker context"
date = 2024-10-23T14:54:43+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/context/](https://docs.docker.com/reference/cli/docker/context/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker context

| Description | Manage contexts  |
| :---------- | ---------------- |
| Usage       | `docker context` |

## Description

Manage contexts.

​	管理上下文。

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker context create`]({{< ref "/reference/CLIreference/docker/dockercontext/dockercontextcreate" >}}) | 创建一个上下文 Create a context                              |
| [`docker context export`]({{< ref "/reference/CLIreference/docker/dockercontext/dockercontextexport" >}}) | 将上下文导出为 tar 存档文件或标准输出中的 tar 流 Export a context to a tar archive FILE or a tar stream on STDOUT. |
| [`docker context import`]({{< ref "/reference/CLIreference/docker/dockercontext/dockercontextimport" >}}) | 从 tar 或 zip 文件中导入一个上下文 Import a context from a tar or zip file |
| [`docker context inspect`]({{< ref "/reference/CLIreference/docker/dockercontext/dockercontextinspect" >}}) | 显示一个或多个上下文的详细信息 Display detailed information on one or more contexts |
| [`docker context ls`]({{< ref "/reference/CLIreference/docker/dockercontext/dockercontextls" >}}) | 列出所有上下文 List contexts                                 |
| [`docker context rm`]({{< ref "/reference/CLIreference/docker/dockercontext/dockercontextrm" >}}) | 删除一个或多个上下文 Remove one or more contexts             |
| [`docker context show`]({{< ref "/reference/CLIreference/docker/dockercontext/dockercontextshow" >}}) | 打印当前上下文的名称 Print the name of the current context   |
| [`docker context update`]({{< ref "/reference/CLIreference/docker/dockercontext/dockercontextupdate" >}}) | 更新一个上下文 Update a context                              |
| [`docker context use`]({{< ref "/reference/CLIreference/docker/dockercontext/dockercontextuse" >}}) | 设置当前的 docker 上下文 Set the current docker context      |
