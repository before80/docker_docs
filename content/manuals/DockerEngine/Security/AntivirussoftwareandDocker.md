+++
title = "防病毒软件与 Docker"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/security/antivirus/](https://docs.docker.com/engine/security/antivirus/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Antivirus software and Docker - 防病毒软件与 Docker

When antivirus software scans files used by Docker, these files may be locked in a way that causes Docker commands to hang.

​	当防病毒软件扫描 Docker 使用的文件时，这些文件可能会被锁定，从而导致 Docker 命令挂起。

One way to reduce these problems is to add the Docker data directory (`/var/lib/docker` on Linux, `%ProgramData%\docker` on Windows Server, or `$HOME/Library/Containers/com.docker.docker/` on Mac) to the antivirus's exclusion list. However, this comes with the trade-off that viruses or malware in Docker images, writable layers of containers, or volumes are not detected. If you do choose to exclude Docker's data directory from background virus scanning, you may want to schedule a recurring task that stops Docker, scans the data directory, and restarts Docker.

​	减少此类问题的一种方法是将 Docker 数据目录（在 Linux 上为 `/var/lib/docker`，在 Windows Server 上为 `%ProgramData%\docker`，在 Mac 上为 `$HOME/Library/Containers/com.docker.docker/`）添加到防病毒软件的排除列表中。然而，这样做的代价是 Docker 镜像、容器的可写层或卷中的病毒或恶意软件可能不会被检测到。如果您决定将 Docker 的数据目录排除在后台病毒扫描之外，您可以考虑安排一个定期任务，该任务会停止 Docker，扫描数据目录，然后重启 Docker。

# 
