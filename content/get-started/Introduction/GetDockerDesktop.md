+++
title = "Get Docker Desktop"
date = 2024-10-23T14:54:35+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/get-started/introduction/get-docker-desktop/](https://docs.docker.com/get-started/introduction/get-docker-desktop/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# Get Docker Desktop

{{< youtube "C2bPVhiNU-0">}}

## Explanation

Docker Desktop is the all-in-one package to build images, run containers, and so much more. This guide will walk you through the installation process, enabling you to experience Docker Desktop firsthand.

> **Docker Desktop terms**
>
> Commercial use of Docker Desktop in larger enterprises (more than 250 employees OR more than $10 million USD in annual revenue) requires a [paid subscription](https://www.docker.com/pricing/?_gl=1*1nyypal*_ga*MTYxMTUxMzkzOS4xNjgzNTM0MTcw*_ga_XJWPQMJYHQ*MTcxNjk4MzU4Mi4xMjE2LjEuMTcxNjk4MzkzNS4xNy4wLjA.).

![img](GetDockerDesktop_img/apple_48.svg)

Docker Desktop for Mac

[Download (Apple Silicon)](https://desktop.docker.com/mac/main/arm64/Docker.dmg?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-mac-arm64) | [Download (Intel)](https://desktop.docker.com/mac/main/amd64/Docker.dmg?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-mac-amd64) | [Install instructions]({{< ref "/manuals/DockerDesktop/Install/Mac" >}})

![img](https://docs.docker.com/assets/images/windows_48.svg)

Docker Desktop for Windows

[Download](https://desktop.docker.com/win/main/amd64/Docker Desktop Installer.exe?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-windows) | [Install instructions]({{< ref "/manuals/DockerDesktop/Install/Windows" >}})

![img](GetDockerDesktop_img/linux_48.svg)

Docker Desktop for Linux

[Install instructions]({{< ref "/manuals/DockerDesktop/Install/Linux" >}})

Once it's installed, complete the setup process and you're all set to run a Docker container.

## Try it out

In this hands-on guide, you will see how to run a Docker container using Docker Desktop.

Follow the instructions to run a container using the CLI.

## Run your first container

Open your CLI terminal and start a container by running the `docker run` command:



```console
$ docker run -d -p 8080:80 docker/welcome-to-docker
```

## Access the frontend

For this container, the frontend is accessible on port `8080`. To open the website, visit [http://localhost:8080](http://localhost:8080/) in your browser.

![Screenshot of the landing page of the Nginx web server, coming from the running container](GetDockerDesktop_img/access-the-frontend.webp)

## Manage containers using Docker Desktop

1. Open Docker Desktop and select the **Containers** field on the left sidebar.

2. You can view information about your container including logs, and files, and even access the shell by selecting the **Exec** tab.

   ![Screenshot of exec into the running container in Docker Desktop](GetDockerDesktop_img/exec-into-docker-container.webp)

3. Select the **Inspect** field to obtain detailed information about the container. You can perform various actions such as pause, resume, start or stop containers, or explore the **Logs**, **Bind mounts**, **Exec**, **Files**, and **Stats** tabs.

![Screenshot of inspecting the running container in Docker Desktop](GetDockerDesktop_img/inspecting-container.webp)

Docker Desktop simplifies container management for developers by streamlining the setup, configuration, and compatibility of applications across different environments, thereby addressing the pain points of environment inconsistencies and deployment challenges.

## What's next?

Now that you have Docker Desktop installed and ran your first container, it's time to start developing with containers.

[Develop with containers]({{< ref "/get-started/Introduction/Developwithcontainers" >}})
