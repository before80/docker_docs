+++
title = "Pause Docker Desktop"
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

# Pause Docker Desktop

When Docker Desktop is paused, the Linux VM running Docker Engine is paused, the current state of all your containers are saved in memory, and all processes are frozen. This reduces the CPU and memory usage and helps you retain a longer battery life on your laptop.

You can manually pause Docker Desktop by selecting the Docker menu ![whale menu](PauseDockerDesktop_img/whale-x.svg) and then **Pause**. To manually resume Docker Desktop, select the **Resume** option in the Docker menu, or run any Docker CLI command.

When you manually pause Docker Desktop, a paused status displays on the Docker menu and on the Docker Dashboard. You can still access the **Settings** and the **Troubleshoot** menu.

> **Tip**
>
> The Resource Saver feature, available in Docker Desktop version 4.24 and later, is enabled by default and provides better CPU and memory savings than the manual Pause feature. See [here]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/ResourceSavermode" >}}) for more info.
