+++
title = "Antivirus software and Docker"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/engine/security/antivirus/](https://docs.docker.com/engine/security/antivirus/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Antivirus software and Docker

When antivirus software scans files used by Docker, these files may be locked in a way that causes Docker commands to hang.

One way to reduce these problems is to add the Docker data directory (`/var/lib/docker` on Linux, `%ProgramData%\docker` on Windows Server, or `$HOME/Library/Containers/com.docker.docker/` on Mac) to the antivirus's exclusion list. However, this comes with the trade-off that viruses or malware in Docker images, writable layers of containers, or volumes are not detected. If you do choose to exclude Docker's data directory from background virus scanning, you may want to schedule a recurring task that stops Docker, scans the data directory, and restarts Docker.
