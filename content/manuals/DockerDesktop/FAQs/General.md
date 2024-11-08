+++
title = "常见"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/faqs/general/](https://docs.docker.com/desktop/faqs/general/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# General FAQs for Desktop - Docker Desktop 常见问题解答

### 我可以在离线状态下使用 Docker Desktop 吗？ Can I use Docker Desktop offline?

Yes, you can use Docker Desktop offline. However, you cannot access features that require an active internet connection. Additionally, any functionality that requires you to sign in won't work while using Docker Desktop offline or in air-gapped environments. This includes:

​	可以，您可以在离线状态下使用 Docker Desktop。不过，您将无法访问需要活跃互联网连接的功能。此外，任何需要登录的功能在离线或隔离环境中都无法使用，包括：

- The resources in the [Learning Center]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop" >}})
  - [学习中心]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop" >}})中的资源

- Pulling or pushing an image to Docker Hub
  - 从 Docker Hub 拉取或推送镜像

- [Image Access Management]({{< ref "/manuals/Security/Fordevelopers/Accesstokens" >}})
  - [镜像访问管理]({{< ref "/manuals/Security/Fordevelopers/Accesstokens" >}})

- [Static vulnerability scanning]({{< ref "/manuals/DockerHub/Staticvulnerabilityscanning" >}})
  - [静态漏洞扫描]({{< ref "/manuals/DockerHub/Staticvulnerabilityscanning" >}})

- Viewing remote images in the Docker Dashboard
  - 在 Docker Dashboard 中查看远程镜像

- Setting up [Dev Environments]({{< ref "/manuals/DockerDesktop/DevEnvironmentsBeta" >}})
  - 设置 [开发环境]({{< ref "/manuals/DockerDesktop/DevEnvironmentsBeta" >}})

- Docker Build when using [BuildKit](https://docs.docker.com/build/buildkit/#getting-started). You can work around this by disabling BuildKit. Run `DOCKER_BUILDKIT=0 docker build .` to disable BuildKit.
  - 使用 [BuildKit](https://docs.docker.com/build/buildkit/#getting-started) 进行 Docker 构建。您可以通过禁用 BuildKit 来解决此问题。运行 `DOCKER_BUILDKIT=0 docker build .` 以禁用 BuildKit。

- [Kubernetes]({{< ref "/manuals/DockerDesktop/DeployonKuberneteswithDockerDesktop" >}}) (Images are download when you enable Kubernetes for the first time)
  - [Kubernetes]({{< ref "/manuals/DockerDesktop/DeployonKuberneteswithDockerDesktop" >}})（启用 Kubernetes 时首次下载镜像）

- Checking for updates
  - 检查更新

- [In-app diagnostics](https://docs.docker.com/desktop/troubleshoot/#diagnose-from-the-app) (including the [Self-diagnose tool](https://docs.docker.com/desktop/troubleshoot/#diagnose-from-the-app))
  - [应用内诊断](https://docs.docker.com/desktop/troubleshoot/#diagnose-from-the-app)（包括 [自我诊断工具](https://docs.docker.com/desktop/troubleshoot/#diagnose-from-the-app)）

- Sending usage statistics
  - 发送使用统计信息


### 如何连接到远程 Docker Engine API？ How do I connect to the remote Docker Engine API?

To connect to the remote Engine API, you might need to provide the location of the Engine API for Docker clients and development tools.

​	要连接到远程 Engine API，您可能需要为 Docker 客户端和开发工具提供 Engine API 的位置。

Mac and Windows WSL 2 users can connect to the Docker Engine through a Unix socket: `unix:///var/run/docker.sock`.

​	Mac 和 Windows WSL 2 用户可以通过 Unix 套接字连接到 Docker Engine：`unix:///var/run/docker.sock`。

If you are working with applications like [Apache Maven](https://maven.apache.org/) that expect settings for `DOCKER_HOST` and `DOCKER_CERT_PATH` environment variables, specify these to connect to Docker instances through Unix sockets.

​	如果您正在使用类似 [Apache Maven](https://maven.apache.org/) 的应用程序，这些应用程序需要设置 `DOCKER_HOST` 和 `DOCKER_CERT_PATH` 环境变量，则可以指定这些设置，以便通过 Unix 套接字连接到 Docker 实例。

For example:

​	例如：

```console
$ export DOCKER_HOST=unix:///var/run/docker.sock
```

Docker Desktop Windows users can connect to the Docker Engine through a **named pipe**: `npipe:////./pipe/docker_engine`, or **TCP socket** at this URL:  `tcp://localhost:2375`.

​	Docker Desktop Windows 用户可以通过 **命名管道** `npipe:////./pipe/docker_engine` 连接到 Docker Engine，或者通过 **TCP 套接字** 连接，URL 为 `tcp://localhost:2375`。



For details, see [Docker Engine API]({{< ref "/reference/APIreference/DockerEngineAPI" >}}).

​	详情请参阅 [Docker Engine API]({{< ref "/reference/APIreference/DockerEngineAPI" >}})。

### 如何从容器连接到主机上的服务？ How do I connect from a container to a service on the host?

The host has a changing IP address, or none if you have no network access. We recommend that you connect to the special DNS name `host.docker.internal`, which resolves to the internal IP address used by the host.

​	主机可能有一个变化的 IP 地址，或者如果没有网络连接，则没有 IP 地址。我们建议您连接到特殊的 DNS 名称 `host.docker.internal`，它解析为主机使用的内部 IP 地址。

For more information and examples, see [how to connect from a container to a service on the host](https://docs.docker.com/desktop/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host).

​	有关更多信息和示例，请参阅[如何从容器连接到主机上的服务](https://docs.docker.com/desktop/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host)。

### 我可以将 USB 设备传递给容器吗？ Can I pass through a USB device to a container?

It is not possible to pass through a USB device (or a serial port) to a container as it requires support at the hypervisor level.

​	由于这需要在虚拟机管理程序层支持，因此无法将 USB 设备（或串口）传递给容器。

### 如何在没有管理员权限的情况下运行 Docker Desktop？ How do I run Docker Desktop without administrator privileges?

Docker Desktop requires administrator privileges only for installation. Once installed, administrator privileges are not needed to run it. However, for non-admin users to run Docker Desktop, it must be installed using a specific installer flag and meet certain prerequisites, which vary by platform.

​	Docker Desktop 仅在安装时需要管理员权限。安装后不需要管理员权限即可运行它。不过，对于非管理员用户来说，必须使用特定的安装标志安装 Docker Desktop，并且需要满足特定平台的先决条件。

{{< tabpane text=true persist=disabled >}}

{{% tab header="Mac" %}}

To run Docker Desktop on Mac without requiring administrator privileges, install via the command line and pass the `—user=<userid>` installer flag:

​	在 Mac 上安装 Docker Desktop 而无需管理员权限，可以通过命令行安装并传递 `—user=<userid>` 安装标志：

```console
$ /Applications/Docker.app/Contents/MacOS/install --user=<userid>
```

You can then sign in to your machine with the user ID specified, and launch Docker Desktop.

​	然后您可以使用指定的用户 ID 登录机器并启动 Docker Desktop。



> **Note**
>
> 
>
> Before launching Docker Desktop, if a `settings.json` file already exists in the `~/Library/Group Containers/group.com.docker/` directory, you will see a **Finish setting up Docker Desktop** window that prompts for administrator privileges when you select **Finish**. To avoid this, ensure you delete the `settings.json` file left behind from any previous installations before launching the application.
>
> ​	启动 Docker Desktop 之前，如果 `~/Library/Group Containers/group.com.docker/` 目录中已经存在 `settings.json` 文件，那么在选择 **完成** 时会弹出 **完成设置 Docker Desktop** 窗口，并提示需要管理员权限。为避免这种情况，在启动应用程序之前确保删除任何先前安装遗留的 `settings.json` 文件。

{{% /tab  %}}

{{% tab header="Windows" %}}

> **Note**
>
> 
>
> If you are using the WSL 2 backend, first make sure that you meet the [minimum required version]({{< ref "/manuals/DockerDesktop/WSL/Bestpractices" >}}) for WSL 2. Otherwise, update WSL 2 first.
>
> ​	如果您使用 WSL 2 后端，首先确保满足 [WSL 2 的最低版本要求]({{< ref "/manuals/DockerDesktop/WSL/Bestpractices" >}})。否则，请先更新 WSL 2。

To run Docker Desktop on Windows without requiring administrator privileges, install via the command line and pass the `—always-run-service` installer flag.

​	要在 Windows 上运行 Docker Desktop 而无需管理员权限，请通过命令行安装并传递 `—always-run-service` 安装标志。

```console
$ "Docker Desktop Installer.exe" install —always-run-service
```

{{% /tab  %}}

{{< /tabpane >}}
