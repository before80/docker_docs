+++
title = "Linux"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/desktop/install/linux/](https://docs.docker.com/desktop/install/linux/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install Docker Desktop on Linux

> **Docker Desktop terms**
>
> Commercial use of Docker Desktop in larger enterprises (more than 250 employees OR more than $10 million USD in annual revenue) requires a [paid subscription](https://www.docker.com/pricing/).

This page contains information about general system requirements, supported platforms, and instructions on how to install Docker Desktop for Linux.

> **Important**
>
> 
>
> Docker Desktop on Linux runs a Virtual Machine (VM) which creates and uses a custom docker context, `desktop-linux`, on startup.
>
> This means images and containers deployed on the Linux Docker Engine (before installation) are not available in Docker Desktop for Linux.

## [Supported platforms](https://docs.docker.com/desktop/install/linux/#supported-platforms)

Docker provides `.deb` and `.rpm` packages from the following Linux distributions and architectures:

| Platform                                                     | x86_64 / amd64 |
| :----------------------------------------------------------- | :------------: |
| [Ubuntu](https://docs.docker.com/desktop/install/linux/ubuntu/) |       ✅        |
| [Debian](https://docs.docker.com/desktop/install/linux/debian/) |       ✅        |
| [Red Hat Enterprise Linux (RHEL)](https://docs.docker.com/desktop/install/linux/rhel/) |       ✅        |
| [Fedora](https://docs.docker.com/desktop/install/linux/fedora/) |       ✅        |

An experimental package is available for [Arch](https://docs.docker.com/desktop/install/linux/archlinux/)-based distributions. Docker has not tested or verified the installation.

Docker supports Docker Desktop on the current LTS release of the aforementioned distributions and the most recent version. As new versions are made available, Docker stops supporting the oldest version and supports the newest version.

## [General system requirements](https://docs.docker.com/desktop/install/linux/#general-system-requirements)

To install Docker Desktop successfully, your Linux host must meet the following general requirements:

- 64-bit kernel and CPU support for virtualization.
- KVM virtualization support. Follow the [KVM virtualization support instructions](https://docs.docker.com/desktop/install/linux/#kvm-virtualization-support) to check if the KVM kernel modules are enabled and how to provide access to the KVM device.
- QEMU must be version 5.2 or later. We recommend upgrading to the latest version.
- systemd init system.
- Gnome, KDE, or MATE Desktop environment.
  - For many Linux distros, the Gnome environment does not support tray icons. To add support for tray icons, you need to install a Gnome extension. For example, [AppIndicator](https://extensions.gnome.org/extension/615/appindicator-support/).
- At least 4 GB of RAM.
- Enable configuring ID mapping in user namespaces, see [File sharing](https://docs.docker.com/desktop/faqs/linuxfaqs/#how-do-i-enable-file-sharing).
- Recommended: [Initialize `pass`](https://docs.docker.com/desktop/get-started/#credentials-management-for-linux-users) for credentials management.

Docker Desktop for Linux runs a Virtual Machine (VM). For more information on why, see [Why Docker Desktop for Linux runs a VM](https://docs.docker.com/desktop/faqs/linuxfaqs/#why-does-docker-desktop-for-linux-run-a-vm).

> **Note**
>
> 
>
> Docker does not provide support for running Docker Desktop for Linux in nested virtualization scenarios. We recommend that you run Docker Desktop for Linux natively on supported distributions.

### [KVM virtualization support](https://docs.docker.com/desktop/install/linux/#kvm-virtualization-support)

Docker Desktop runs a VM that requires [KVM support](https://www.linux-kvm.org/).

The `kvm` module should load automatically if the host has virtualization support. To load the module manually, run:



```console
$ modprobe kvm
```

Depending on the processor of the host machine, the corresponding module must be loaded:



```console
$ modprobe kvm_intel  # Intel processors

$ modprobe kvm_amd    # AMD processors
```

If the above commands fail, you can view the diagnostics by running:



```console
$ kvm-ok
```

To check if the KVM modules are enabled, run:



```console
$ lsmod | grep kvm
kvm_amd               167936  0
ccp                   126976  1 kvm_amd
kvm                  1089536  1 kvm_amd
irqbypass              16384  1 kvm
```

#### [Set up KVM device user permissions](https://docs.docker.com/desktop/install/linux/#set-up-kvm-device-user-permissions)

To check ownership of `/dev/kvm`, run :



```console
$ ls -al /dev/kvm
```

Add your user to the kvm group in order to access the kvm device:



```console
$ sudo usermod -aG kvm $USER
```

Sign out and sign back in so that your group membership is re-evaluated.

## [Where to go next](https://docs.docker.com/desktop/install/linux/#where-to-go-next)

- Install Docker Desktop for Linux for your specific Linux distribution:
  - [Install on Ubuntu](https://docs.docker.com/desktop/install/linux/ubuntu/)
  - [Install on Debian](https://docs.docker.com/desktop/install/linux/debian/)
  - [Install on Red Hat Enterprise Linux (RHEL)](https://docs.docker.com/desktop/install/linux/rhel/)
  - [Install on Fedora](https://docs.docker.com/desktop/install/linux/fedora/)
  - [Install on Arch](https://docs.docker.com/desktop/install/linux/archlinux/)
