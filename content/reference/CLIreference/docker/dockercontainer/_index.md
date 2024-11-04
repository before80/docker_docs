+++
title = "docker container"
date = 2024-10-23T14:54:43+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/container/](https://docs.docker.com/reference/cli/docker/container/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker container

| Description | Manage containers  |
| :---------- | ------------------ |
| Usage       | `docker container` |

## Description

Manage containers.

​	管理容器。

## Subcommands

| Command                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`docker container attach`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerattach" >}}) | 将本地标准输入、输出和错误流附加到正在运行的容器 Attach local standard input, output, and error streams to a running container |
| [`docker container commit`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainercommit" >}}) | 从容器的更改创建一个新镜像 Create a new image from a container's changes |
| [`docker container cp`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainercp" >}}) | 在容器和本地文件系统之间复制文件/文件夹 Copy files/folders between a container and the local filesystem |
| [`docker container create`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainercreate" >}}) | 创建一个新容器 Create a new container                        |
| [`docker container diff`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerdiff" >}}) | 检查容器文件系统上文件或目录的更改 Inspect changes to files or directories on a container's filesystem |
| [`docker container export`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerexport" >}}) | 将容器的文件系统导出为 tar 归档 Export a container's filesystem as a tar archive |
| [`docker container inspect`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerinspect" >}}) | 显示一个或多个容器的详细信息 Display detailed information on one or more containers |
| [`docker container kill`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerkill" >}}) | 杀死一个或多个正在运行的容器 Kill one or more running containers |
| [`docker container logs`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerlogs" >}}) | 获取容器的日志 Fetch the logs of a container                 |
| [`docker container pause`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerpause" >}}) | 暂停一个或多个容器内的所有进程 Pause all processes within one or more containers |
| [`docker container port`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerport" >}}) | 列出容器的端口映射或特定映射 List port mappings or a specific mapping for the container |
| [`docker container prune`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerprune" >}}) | 删除所有已停止的容器 Remove all stopped containers           |
| [`docker container rename`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerrename" >}}) | 重命名一个容器 Rename a container                            |
| [`docker container restart`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerrestart" >}}) | 重启一个或多个容器 Restart one or more containers            |
| [`docker container rm`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerrm" >}}) | 删除一个或多个容器 Remove one or more containers             |
| [`docker container start`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerstart" >}}) | 启动一个或多个已停止的容器 Start one or more stopped containers |
| [`docker container stats`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerstats" >}}) | 显示容器资源使用统计的实时流 Display a live stream of container(s) resource usage statistics |
| [`docker container stop`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerstop" >}}) | 停止一个或多个正在运行的容器 Stop one or more running containers |
| [`docker container top`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainertop" >}}) | 显示容器的运行进程 Display the running processes of a container |
| [`docker container unpause`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerunpause" >}}) | 恢复一个或多个容器内的所有进程 Unpause all processes within one or more containers |
| [`docker container update`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerupdate" >}}) | 更新一个或多个容器的配置 Update configuration of one or more containers |
| [`docker container wait`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerwait" >}}) | 阻塞直到一个或多个容器停止，然后打印它们的退出代码 Block until one or more containers stop, then print their exit codes |
| [`docker container exec`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerexec" >}}) | 在运行中的容器内执行命令 Execute a command in a running container |
| [`docker container ls`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerps" >}}) | 列出容器 List containers                                     |
| [`docker container run`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerrun" >}}) | 从镜像创建并运行一个新容器 Create and run a new container from an image |

