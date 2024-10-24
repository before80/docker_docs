+++
title = "Install"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Overview of installing Docker Compose

This page contains summary information about the available options for installing Docker Compose.

## Installation scenarios

### Scenario one: Install Docker Desktop

The easiest and recommended way to get Docker Compose is to install Docker Desktop. Docker Desktop includes Docker Compose along with Docker Engine and Docker CLI which are Compose prerequisites.

Docker Desktop is available on:

- [Linux]({{< ref "/manuals/DockerDesktop/Install/Linux" >}})
- [Mac]({{< ref "/manuals/DockerDesktop/Install/Mac" >}})
- [Windows]({{< ref "/manuals/DockerDesktop/Install/Windows" >}})

If you have already installed Docker Desktop, you can check which version of Compose you have by selecting **About Docker Desktop** from the Docker menu ![whale menu](_index_img/whale-x.svg) .

> **Note**
>
> 
>
> After Docker Compose V1 was removed in Docker Desktop version [4.23.0](https://docs.docker.com/desktop/release-notes/#4230) as it had reached end-of-life, the `docker-compose` command now points directly to the Docker Compose V2 binary, running in standalone mode. If you rely on Docker Desktop auto-update, the symlink might be broken and command unavailable, as the update doesn't ask for administrator password.
>
> This only affects Mac users. To fix this, either recreate the symlink:
>
> 
>
> ```console
> $ sudo rm /usr/local/bin/docker-compose
> $ sudo ln -s /Applications/Docker.app/Contents/Resources/cli-plugins/docker-compose /usr/local/bin/docker-compose
> ```
>
> Or enable [Automatically check configuration]({{< ref "/manuals/DockerDesktop/Changesettings" >}}) which will detect and fix it for you.

### Scenario two: Install the Compose plugin

If you already have Docker Engine and Docker CLI installed, you can install the Compose plugin from the command line, by either:

- [Using Docker's repository](https://docs.docker.com/compose/install/linux/#install-using-the-repository)
- [Downloading and installing manually](https://docs.docker.com/compose/install/linux/#install-the-plugin-manually)

> **Important**
>
> 
>
> This is only available on Linux

### Scenario three: Install the Compose standalone

You can [install the Compose standalone]({{< ref "/manuals/DockerCompose/Install/InstallComposestandalone" >}}) on Linux or on Windows Server.

> **Warning**
>
> 
>
> This install scenario is not recommended and is only supported for backward compatibility purposes.
