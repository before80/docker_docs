+++
title = "Linux"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/desktop/install/linux/](https://docs.docker.com/desktop/install/linux/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install Docker Desktop on Linux - 安装 Docker Desktop 在 Linux 上

> **Docker Desktop terms Docker Desktop 条款**
>
> Commercial use of Docker Desktop in larger enterprises (more than 250 employees OR more than $10 million USD in annual revenue) requires a [paid subscription](https://www.docker.com/pricing/).
>
> ​	在大型企业中商业使用 Docker Desktop（超过 250 名员工或年收入超过 1000 万美元）需要[付费订阅](https://www.docker.com/pricing/)。

This page contains information about general system requirements, supported platforms, and instructions on how to install Docker Desktop for Linux.

​	本页包含有关系统要求、支持平台以及如何在 Linux 上安装 Docker Desktop 的说明。

> **Important**
>
> 
>
> Docker Desktop on Linux runs a Virtual Machine (VM) which creates and uses a custom docker context, `desktop-linux`, on startup.
>
> ​	Linux 上的 Docker Desktop 运行一个虚拟机 (VM)，该虚拟机会在启动时创建并使用自定义的 docker 上下文 `desktop-linux`。
>
> This means images and containers deployed on the Linux Docker Engine (before installation) are not available in Docker Desktop for Linux.
>
> ​	这意味着在安装 Docker Desktop for Linux 之前，在 Linux Docker 引擎上部署的镜像和容器在 Docker Desktop for Linux 中不可用。

## 支持的平台 Supported platforms

Docker provides `.deb` and `.rpm` packages from the following Linux distributions and architectures:

​	Docker 提供适用于以下 Linux 发行版和架构的 `.deb` 和 `.rpm` 包：

| Platform                                                     | x86_64 / amd64 |
| :----------------------------------------------------------- | :------------: |
| [Ubuntu]({{< ref "/manuals/DockerDesktop/Install/Linux/Ubuntu" >}}) |       ✅        |
| [Debian]({{< ref "/manuals/DockerDesktop/Install/Linux/Debian" >}}) |       ✅        |
| [Red Hat Enterprise Linux (RHEL)](https://docs.docker.com/desktop/install/linux/rhel/) |       ✅        |
| [Fedora]({{< ref "/manuals/DockerDesktop/Install/Linux/Fedora" >}}) |       ✅        |

An experimental package is available for [Arch]({{< ref "/manuals/DockerDesktop/Install/Linux/Arch" >}})-based distributions. Docker has not tested or verified the installation.

​	对于基于 [Arch]({{< ref "/manuals/DockerDesktop/Install/Linux/Arch" >}}) 的发行版，提供了实验性安装包。Docker 尚未对其进行测试或验证。

Docker supports Docker Desktop on the current LTS release of the aforementioned distributions and the most recent version. As new versions are made available, Docker stops supporting the oldest version and supports the newest version.

​	Docker 支持上述发行版的当前 LTS 版本和最新版本。当发布新版本时，Docker 停止支持最旧的版本并支持最新的版本。

## General system requirements

To install Docker Desktop successfully, your Linux host must meet the following general requirements:

​	要成功安装 Docker Desktop，您的 Linux 主机必须满足以下一般要求：

- 64-bit kernel and CPU support for virtualization.
  - 64 位内核和 CPU 支持虚拟化。

- KVM virtualization support. Follow the [KVM virtualization support instructions]({{< ref "/manuals/DockerDesktop/Install/Linux#kvm-virtualization-support">}}) to check if the KVM kernel modules are enabled and how to provide access to the KVM device.
  - KVM 虚拟化支持。请参阅 [KVM 虚拟化支持说明]({{< ref "/manuals/DockerDesktop/Install/Linux#kvm-virtualization-support">}}) 以检查是否启用了 KVM 内核模块以及如何提供对 KVM 设备的访问。

- QEMU must be version 5.2 or later. We recommend upgrading to the latest version.
  - QEMU 版本必须为 5.2 或更高版本。建议升级到最新版本。

- systemd init system.
  - 使用 systemd 初始化系统。

- Gnome, KDE, or MATE Desktop environment. Gnome、KDE 或 MATE 桌面环境。
  - For many Linux distros, the Gnome environment does not support tray icons. To add support for tray icons, you need to install a Gnome extension. For example, [AppIndicator](https://extensions.gnome.org/extension/615/appindicator-support/).
    - 对于许多 Linux 发行版，Gnome 环境不支持托盘图标。要添加托盘图标支持，您需要安装 Gnome 扩展，例如 [AppIndicator](https://extensions.gnome.org/extension/615/appindicator-support/)。
- At least 4 GB of RAM.
  - 至少 4 GB 的内存。

- Enable configuring ID mapping in user namespaces, see [File sharing](https://docs.docker.com/desktop/faqs/linuxfaqs/#how-do-i-enable-file-sharing).
  - 启用在用户命名空间中配置 ID 映射的功能，详见[文件共享](https://docs.docker.com/desktop/faqs/linuxfaqs/#how-do-i-enable-file-sharing)。

- Recommended: [Initialize `pass`](https://docs.docker.com/desktop/get-started/#credentials-management-for-linux-users) for credentials management.
  - 推荐：为凭证管理[初始化 `pass`](https://docs.docker.com/desktop/get-started/#credentials-management-for-linux-users)。


Docker Desktop for Linux runs a Virtual Machine (VM). For more information on why, see [Why Docker Desktop for Linux runs a VM](https://docs.docker.com/desktop/faqs/linuxfaqs/#why-does-docker-desktop-for-linux-run-a-vm).

​	Linux 上的 Docker Desktop 运行一个虚拟机 (VM)。了解更多信息，请参阅[为何 Linux 上的 Docker Desktop 运行 VM](https://docs.docker.com/desktop/faqs/linuxfaqs/#why-does-docker-desktop-for-linux-run-a-vm)。

> **Note**
>
> 
>
> Docker does not provide support for running Docker Desktop for Linux in nested virtualization scenarios. We recommend that you run Docker Desktop for Linux natively on supported distributions.
>
> ​	Docker 不支持在嵌套虚拟化环境中运行 Docker Desktop for Linux。我们建议您在受支持的发行版上本地运行 Docker Desktop for Linux。

### KVM virtualization support

Docker Desktop runs a VM that requires [KVM support](https://www.linux-kvm.org/).

​	Docker Desktop 运行需要 [KVM 支持](https://www.linux-kvm.org/) 的 VM。

The `kvm` module should load automatically if the host has virtualization support. To load the module manually, run:

​	如果主机支持虚拟化，则 `kvm` 模块应自动加载。要手动加载该模块，请运行：

```console
$ modprobe kvm
```

Depending on the processor of the host machine, the corresponding module must be loaded:

​	根据主机机器的处理器，必须加载相应的模块：

```console
$ modprobe kvm_intel  # Intel processors

$ modprobe kvm_amd    # AMD processors
```

If the above commands fail, you can view the diagnostics by running:

​	如果上述命令失败，您可以通过运行以下命令查看诊断信息：

```console
$ kvm-ok
```

To check if the KVM modules are enabled, run:

​	要检查是否启用了 KVM 模块，请运行：

```console
$ lsmod | grep kvm
kvm_amd               167936  0
ccp                   126976  1 kvm_amd
kvm                  1089536  1 kvm_amd
irqbypass              16384  1 kvm
```

#### 设置 KVM 设备用户权限 Set up KVM device user permissions

To check ownership of `/dev/kvm`, run :

​	要检查 `/dev/kvm` 的所有权，请运行：

```console
$ ls -al /dev/kvm
```

Add your user to the kvm group in order to access the kvm device:

​	将您的用户添加到 kvm 组以访问 kvm 设备：

```console
$ sudo usermod -aG kvm $USER
```

Sign out and sign back in so that your group membership is re-evaluated.

​	退出并重新登录，以便重新评估您的组成员身份。

## Where to go next

- Install Docker Desktop for Linux for your specific Linux distribution: 为您的特定 Linux 发行版安装 Docker Desktop：
  - [Install on Ubuntu]({{< ref "/manuals/DockerDesktop/Install/Linux/Ubuntu" >}})
  - [Install on Debian]({{< ref "/manuals/DockerDesktop/Install/Linux/Debian" >}})
  - [Install on Red Hat Enterprise Linux (RHEL)](https://docs.docker.com/desktop/install/linux/rhel/)
  - [Install on Fedora]({{< ref "/manuals/DockerDesktop/Install/Linux/Fedora" >}})
  - [Install on Arch]({{< ref "/manuals/DockerDesktop/Install/Linux/Arch" >}})
