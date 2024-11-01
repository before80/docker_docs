+++
title = "了解 Windows 上 Docker Desktop 的权限要求"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/install/windows-permission-requirements/](https://docs.docker.com/desktop/install/windows-permission-requirements/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Understand permission requirements for Windows - 了解 Windows 上 Docker Desktop 的权限要求

This page contains information about the permission requirements for running and installing Docker Desktop on Windows, the functionality of the privileged helper process `com.docker.service` and the reasoning behind this approach.

​	此页面包含有关在 Windows 上运行和安装 Docker Desktop 所需权限的信息，介绍了特权辅助进程 `com.docker.service` 的功能及其设计初衷。

It also provides clarity on running containers as `root` as opposed to having `Administrator` access on the host and the privileges of the Windows Docker engine and Windows containers.

​	页面还说明了容器以 `root` 运行与主机上的 `Administrator` 访问权限的不同，以及 Windows Docker 引擎和 Windows 容器的特权。

## 权限要求 Permission requirements

While Docker Desktop on Windows can be run without having `Administrator` privileges, it does require them during installation. On installation you receive a UAC prompt which allows a privileged helper service to be installed. After that, Docker Desktop can be run without administrator privileges, provided you are members of the `docker-users` group. If you performed the installation, you are automatically added to this group, but other users must be added manually. This allows the administrator to control who has access to Docker Desktop.

​	尽管在 Windows 上运行 Docker Desktop 不需要 `Administrator` 权限，但安装时确实需要。在安装过程中，您会收到 UAC 提示，允许安装特权辅助服务。之后，Docker Desktop 可以在没有管理员权限的情况下运行，只要您是 `docker-users` 组的成员。如果您执行了安装，系统会自动将您添加到该组中，但其他用户必须手动添加。这样管理员可以控制谁可以访问 Docker Desktop。

The reason for this approach is that Docker Desktop needs to perform a limited set of privileged operations which are conducted by the privileged helper process `com.docker.service`. This approach allows, following the principle of least privilege, `Administrator` access to be used only for the operations for which it is absolutely necessary, while still being able to use Docker Desktop as an unprivileged user.

​	此设计的原因是 Docker Desktop 需要执行一组有限的特权操作，由特权辅助进程 `com.docker.service` 执行。这种方法遵循最小特权原则，仅对绝对必要的操作使用 `Administrator` 访问权限，同时仍然允许作为非特权用户使用 Docker Desktop。

## 特权辅助进程 Privileged helper

The privileged helper `com.docker.service` is a Windows service which runs in the background with `SYSTEM` privileges. It listens on the named pipe `//./pipe/dockerBackendV2`. The developer runs the Docker Desktop application, which connects to the named pipe and sends commands to the service. This named pipe is protected, and only users that are part of the `docker-users` group can have access to it.

​	特权辅助进程 `com.docker.service` 是一个在后台以 `SYSTEM` 权限运行的 Windows 服务。它监听名为 `//./pipe/dockerBackendV2` 的命名管道。开发人员运行 Docker Desktop 应用程序，该应用程序连接到命名管道并向服务发送命令。此命名管道受到保护，只有属于 `docker-users` 组的用户才能访问它。

The service performs the following functionalities:

​	服务执行以下功能：

- Ensuring that `kubernetes.docker.internal` is defined in the Win32 hosts file. Defining the DNS name `kubernetes.docker.internal` allows Docker to share Kubernetes contexts with containers.
  - 确保在 Win32 hosts 文件中定义 `kubernetes.docker.internal`。定义 DNS 名称 `kubernetes.docker.internal` 允许 Docker 将 Kubernetes 上下文共享给容器。

- Ensuring that `host.docker.internal` and `gateway.docker.internal` are defined in the Win32 hosts file. They point to the host local IP address and allow an application to resolve the host IP using the same name from either the host itself or a container.
  - 确保在 Win32 hosts 文件中定义 `host.docker.internal` 和 `gateway.docker.internal`，它们指向主机的本地 IP 地址，允许应用程序在主机或容器中使用相同的名称解析主机 IP。

- Securely caching the Registry Access Management policy which is read-only for the developer.
  - 安全地缓存只读的注册表访问管理策略。

- Creating the Hyper-V VM `"DockerDesktopVM"` and managing its lifecycle - starting, stopping and destroying it. The VM name is hard coded in the service code so the service cannot be used for creating or manipulating any other VMs.
  - 创建 Hyper-V 虚拟机 `"DockerDesktopVM"` 并管理其生命周期（启动、停止和销毁）。虚拟机名称在服务代码中硬编码，因此服务不能用于创建或操作其他虚拟机。

- Moving the VHDX file or folder.
  - 移动 VHDX 文件或文件夹。

- Starting and stopping the Windows Docker engine and querying whether it's running.
  - 启动和停止 Windows Docker 引擎，并查询其是否正在运行。

- Deleting all Windows containers data files.
  - 删除所有 Windows 容器数据文件。

- Checking if Hyper-V is enabled.
  - 检查是否启用了 Hyper-V。

- Checking if the bootloader activates Hyper-V.
  - 检查启动引导程序是否激活了 Hyper-V。

- Checking if required Windows features are both installed and enabled.
  - 检查是否安装并启用了所需的 Windows 功能。

- Conducting healthchecks and retrieving the version of the service itself.
  - 进行健康检查并检索服务本身的版本。


The service start mode depends on which container engine is selected, and, for WSL, on whether it is needed to maintain `host.docker.internal` and `gateway.docker.internal` in the Win32 hosts file. This is controlled by a setting under `Use the WSL 2 based engine` in the settings page. When this is set, WSL engine behaves the same as Hyper-V. So:

​	服务启动模式取决于所选的容器引擎，并且对于 WSL 引擎，还取决于是否需要在 Win32 hosts 文件中维护 `host.docker.internal` 和 `gateway.docker.internal`。此设置可通过设置页面中的“使用基于 WSL 2 的引擎”选项进行控制。当此选项启用时，WSL 引擎与 Hyper-V 行为相同。因此：

- With Windows containers, or Hyper-v Linux containers, the service is started when the system boots and runs all the time, even when Docker Desktop isn't running. This is required so you can launch Docker Desktop without admin privileges.
  - 对于 Windows 容器或 Hyper-V Linux 容器，系统启动时服务会自动启动，并在 Docker Desktop 不运行时仍保持运行。这样可以在没有管理员权限的情况下启动 Docker Desktop。

- With WSL2 Linux containers, the service isn't necessary and therefore doesn't run automatically when the system boots. When you switch to Windows containers or Hyper-V Linux containers, or choose to maintain `host.docker.internal` and `gateway.docker.internal` in the Win32 hosts file, a UAC prompt is displayed which asks you to accept the privileged operation to start the service. If accepted, the service is started and set to start automatically upon the next Windows boot.
  - 对于 WSL2 Linux 容器，不需要服务，因此系统启动时不会自动运行。如果您切换到 Windows 容器或 Hyper-V Linux 容器，或选择在 Win32 hosts 文件中维护 `host.docker.internal` 和 `gateway.docker.internal`，则会显示 UAC 提示，要求您接受启动服务的特权操作。如果接受，服务会启动并设置为在下次 Windows 启动时自动启动。

## 在 Linux 虚拟机中以 root 运行的容器 Containers running as root within the Linux VM

The Linux Docker daemon and containers run in a minimal, special-purpose Linux VM managed by Docker. It is immutable so you can’t extend it or change the installed software. This means that although containers run by default as `root`, this doesn't allow altering the VM and doesn't grant `Administrator` access to the Windows host machine. The Linux VM serves as a security boundary and limits what resources from the host can be accessed. File sharing uses a user-space crafted file server and any directories from the host bind mounted into Docker containers still retain their original permissions. It doesn't give you access to any files that it doesn’t already have access to.

​	Linux Docker 守护进程和容器在由 Docker 管理的专用、最小化 Linux 虚拟机中运行。该虚拟机是不可变的，因此无法扩展或更改已安装的软件。这意味着虽然容器默认以 `root` 运行，但这不会允许修改虚拟机，也不会授予 Windows 主机的 `Administrator` 访问权限。Linux 虚拟机作为安全边界，限制了对主机资源的访问。文件共享使用用户空间的文件服务器，绑定挂载到 Docker 容器中的主机目录仍保留其原始权限，且不授予对未授权文件的访问权限。

## 增强的容器隔离 Enhanced Container Isolation

In addition, Docker Desktop supports [Enhanced Container Isolation mode]({{< ref "/manuals/Security/Foradmins/HardenedDockerDesktop/EnhancedContainerIsolation" >}}) (ECI), available to Business customers only, which further secures containers without impacting developer workflows.

​	此外，Docker Desktop 支持[增强的容器隔离模式]({{< ref "/manuals/Security/Foradmins/HardenedDockerDesktop/EnhancedContainerIsolation" >}})（ECI），仅对企业客户提供，该模式进一步保护容器，同时不会影响开发者的工作流。

ECI automatically runs all containers within a Linux user-namespace, such that root in the container is mapped to an unprivileged user inside the Docker Desktop VM. ECI uses this and other advanced techniques to further secure containers within the Docker Desktop Linux VM, such that they are further isolated from the Docker daemon and other services running inside the VM.

​	ECI 自动在 Linux 用户命名空间中运行所有容器，使得容器内的 root 用户被映射为 Docker Desktop 虚拟机内的非特权用户。ECI 利用此及其他高级技术进一步保护 Docker Desktop Linux 虚拟机中的容器，确保它们与 Docker 守护进程及虚拟机内其他服务的进一步隔离。

## Windows 容器 Windows Containers

Unlike the Linux Docker engine and containers which run in a VM, Windows containers are an operating system feature, and run directly on the Windows host with `Administrator` privileges. For organizations who don't want their developers to run Windows containers, a `–no-windows-containers` installer flag is available from version 4.11 to disable their use.

​	与在虚拟机中运行的 Linux Docker 引擎和容器不同，Windows 容器是操作系统的一个特性，直接在 Windows 主机上以 `Administrator` 权限运行。对于不希望开发人员运行 Windows 容器的组织，可以从 4.11 版本开始使用 `–no-windows-containers` 安装标志来禁用其使用。

## 网络 Networking

For network connectivity, Docker Desktop uses a user-space process (`vpnkit`), which inherits constraints like firewall rules, VPN, HTTP proxy properties etc. from the user that launched it.

​	对于网络连接，Docker Desktop 使用一个用户空间进程（`vpnkit`），该进程继承启动它的用户的约束条件，例如防火墙规则、VPN、HTTP 代理属性等。
