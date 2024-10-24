+++
title = "General"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/desktop/faqs/general/](https://docs.docker.com/desktop/faqs/general/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# General FAQs for Desktop

### [Can I use Docker Desktop offline?](https://docs.docker.com/desktop/faqs/general/#can-i-use-docker-desktop-offline)

Yes, you can use Docker Desktop offline. However, you cannot access features that require an active internet connection. Additionally, any functionality that requires you to sign in won't work while using Docker Desktop offline or in air-gapped environments. This includes:

- The resources in the [Learning Center](https://docs.docker.com/desktop/use-desktop/)
- Pulling or pushing an image to Docker Hub
- [Image Access Management](https://docs.docker.com/security/for-developers/access-tokens/)
- [Static vulnerability scanning](https://docs.docker.com/docker-hub/vulnerability-scanning/)
- Viewing remote images in the Docker Dashboard
- Setting up [Dev Environments](https://docs.docker.com/desktop/dev-environments/)
- Docker Build when using [BuildKit](https://docs.docker.com/build/buildkit/#getting-started). You can work around this by disabling BuildKit. Run `DOCKER_BUILDKIT=0 docker build .` to disable BuildKit.
- [Kubernetes](https://docs.docker.com/desktop/kubernetes/) (Images are download when you enable Kubernetes for the first time)
- Checking for updates
- [In-app diagnostics](https://docs.docker.com/desktop/troubleshoot/#diagnose-from-the-app) (including the [Self-diagnose tool](https://docs.docker.com/desktop/troubleshoot/#diagnose-from-the-app))
- Sending usage statistics

### [How do I connect to the remote Docker Engine API?](https://docs.docker.com/desktop/faqs/general/#how-do-i-connect-to-the-remote-docker-engine-api)

To connect to the remote Engine API, you might need to provide the location of the Engine API for Docker clients and development tools.

Mac and Windows WSL 2 users can connect to the Docker Engine through a Unix socket: `unix:///var/run/docker.sock`.

If you are working with applications like [Apache Maven](https://maven.apache.org/) that expect settings for `DOCKER_HOST` and `DOCKER_CERT_PATH` environment variables, specify these to connect to Docker instances through Unix sockets.

For example:



```console
$ export DOCKER_HOST=unix:///var/run/docker.sock
```

Docker Desktop Windows users can connect to the Docker Engine through a **named pipe**: `npipe:////./pipe/docker_engine`, or **TCP socket** at this URL: `tcp://localhost:2375`.

For details, see [Docker Engine API](https://docs.docker.com/reference/api/engine/).

### [How do I connect from a container to a service on the host?](https://docs.docker.com/desktop/faqs/general/#how-do-i-connect-from-a-container-to-a-service-on-the-host)

The host has a changing IP address, or none if you have no network access. We recommend that you connect to the special DNS name `host.docker.internal`, which resolves to the internal IP address used by the host.

For more information and examples, see [how to connect from a container to a service on the host](https://docs.docker.com/desktop/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host).

### [Can I pass through a USB device to a container?](https://docs.docker.com/desktop/faqs/general/#can-i-pass-through-a-usb-device-to-a-container)

It is not possible to pass through a USB device (or a serial port) to a container as it requires support at the hypervisor level.

### [How do I run Docker Desktop without administrator privileges?](https://docs.docker.com/desktop/faqs/general/#how-do-i-run-docker-desktop-without-administrator-privileges)

Docker Desktop requires administrator privileges only for installation. Once installed, administrator privileges are not needed to run it. However, for non-admin users to run Docker Desktop, it must be installed using a specific installer flag and meet certain prerequisites, which vary by platform.

Mac Windows

------

To run Docker Desktop on Mac without requiring administrator privileges, install via the command line and pass the `—user=<userid>` installer flag:



```console
$ /Applications/Docker.app/Contents/MacOS/install --user=<userid>
```

You can then sign in to your machine with the user ID specified, and launch Docker Desktop.

> **Note**
>
> 
>
> Before launching Docker Desktop, if a `settings.json` file already exists in the `~/Library/Group Containers/group.com.docker/` directory, you will see a **Finish setting up Docker Desktop** window that prompts for administrator privileges when you select **Finish**. To avoid this, ensure you delete the `settings.json` file left behind from any previous installations before launching the application.
