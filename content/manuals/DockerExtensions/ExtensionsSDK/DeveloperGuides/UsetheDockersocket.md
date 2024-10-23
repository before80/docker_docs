+++
title = "Use the Docker socket"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/extensions/extensions-sdk/guides/use-docker-socket-from-backend/](https://docs.docker.com/extensions/extensions-sdk/guides/use-docker-socket-from-backend/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use the Docker socket from the extension backend

Extensions can invoke Docker commands directly from the frontend with the SDK.

In some cases, it is useful to also interact with Docker Engine from the backend.

Extension backend containers can mount the Docker socket and use it to interact with Docker Engine from the extension backend logic. Learn more about the [Docker Engine socket](https://docs.docker.com/reference/cli/dockerd/#examples)

However, when mounting the Docker socket from an extension container that lives in the Desktop virtual machine, you want to mount the Docker socket from inside the VM, and not mount `/var/run/docker.sock` from the host filesystem (using the Docker socket from the host can lead to permission issues in containers).

In order to do so, you can use `/var/run/docker.sock.raw`. Docker Desktop mounts the socket that lives in the Desktop VM, and not from the host.



```yaml
services:
  myExtension:
    image: ${DESKTOP_PLUGIN_IMAGE}
    volumes:
      - /var/run/docker.sock.raw:/var/run/docker.sock
```
