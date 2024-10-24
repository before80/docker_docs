+++
title = "Security"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/extensions/extensions-sdk/architecture/security/](https://docs.docker.com/extensions/extensions-sdk/architecture/security/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Extension security

## Extension capabilities

An extension can have the following optional parts:

- A user interface in HTML or JavaScript, displayed in Docker Desktop Dashboard
- A backend part that runs as a container
- Executables deployed on the host machine.

Extensions are executed with the same permissions as the Docker Desktop user. Extension capabilities include running any Docker commands (including running containers and mounting folders), running extension binaries, and accessing files on your machine that are accessible by the user running Docker Desktop.

The Extensions SDK provides a set of JavaScript APIs to invoke commands or invoke these binaries from the extension UI code. Extensions can also provide a backend part that starts a long-lived running container in the background.

> **Important**
>
> 
>
> Make sure you trust the publisher or author of the extension when you install it, as the extension has the same access rights as the user running Docker Desktop.
