+++
title = "暂停 Docker Desktop"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/use-desktop/pause/](https://docs.docker.com/desktop/use-desktop/pause/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Pause Docker Desktop - 暂停 Docker Desktop

When Docker Desktop is paused, the Linux VM running Docker Engine is paused, the current state of all your containers are saved in memory, and all processes are frozen. This reduces the CPU and memory usage and helps you retain a longer battery life on your laptop.

​	当 Docker Desktop 处于暂停状态时，运行 Docker 引擎的 Linux VM 将暂停，所有容器的当前状态将保存在内存中，所有进程都被冻结。这样可以减少 CPU 和内存使用量，从而延长笔记本电脑的电池续航时间。

You can manually pause Docker Desktop by selecting the Docker menu ![whale menu](PauseDockerDesktop_img/whale-x.svg) and then **Pause**. To manually resume Docker Desktop, select the **Resume** option in the Docker menu, or run any Docker CLI command.

​	您可以手动暂停 Docker Desktop，通过选择 Docker 菜单 ![whale menu](PauseDockerDesktop_img/whale-x.svg)，然后点击 **暂停**。要手动恢复 Docker Desktop，选择 Docker 菜单中的 **恢复** 选项，或运行任何 Docker CLI 命令。

When you manually pause Docker Desktop, a paused status displays on the Docker menu and on the Docker Dashboard. You can still access the **Settings** and the **Troubleshoot** menu.

​	当您手动暂停 Docker Desktop 时，Docker 菜单和 Docker 仪表板上会显示暂停状态。您仍可以访问 **设置** 和 **故障排除** 菜单。

> **Tip**
>
> The Resource Saver feature, available in Docker Desktop version 4.24 and later, is enabled by default and provides better CPU and memory savings than the manual Pause feature. See [here]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/ResourceSavermode" >}}) for more info.
>
> ​	Docker Desktop 版本 4.24 及更高版本中提供的 **资源节约模式** 默认启用，提供比手动暂停更好的 CPU 和内存节约效果。详见[此处]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/ResourceSavermode" >}})。
