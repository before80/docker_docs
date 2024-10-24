+++
title = "Install"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install Docker Engine

This section describes how to install Docker Engine on Linux, also known as Docker CE. Docker Engine is also available for Windows, macOS, and Linux, through Docker Desktop. For instructions on how to install Docker Desktop, see:

- [Docker Desktop for Linux]({{< ref "/manuals/DockerDesktop/Install/Linux" >}})
- [Docker Desktop for Mac (macOS)]({{< ref "/manuals/DockerDesktop/Install/Mac" >}})
- [Docker Desktop for Windows]({{< ref "/manuals/DockerDesktop/Install/Windows" >}})

## Supported platforms

| Platform                                                     | x86_64 / amd64 | arm64 / aarch64 | arm (32-bit) | ppc64le | s390x |
| :----------------------------------------------------------- | :------------: | :-------------: | :----------: | :-----: | :---: |
| [CentOS]({{< ref "/manuals/DockerEngine/Install/CentOS" >}})     |       ✅        |        ✅        |              |    ✅    |       |
| [Debian]({{< ref "/manuals/DockerEngine/Install/Debian" >}})     |       ✅        |        ✅        |      ✅       |    ✅    |       |
| [Fedora]({{< ref "/manuals/DockerEngine/Install/Fedora" >}})     |       ✅        |        ✅        |              |    ✅    |       |
| [Raspberry Pi OS (32-bit)]({{< ref "/manuals/DockerEngine/Install/RaspberryPiOS32-bit" >}}) |                |                 |      ✅       |         |       |
| [RHEL]({{< ref "/manuals/DockerEngine/Install/RHEL" >}})         |       🚧        |        🚧        |              |         |   ✅   |
| [SLES]({{< ref "/manuals/DockerEngine/Install/SLESs390x" >}})         |                |                 |              |         |   ✅   |
| [Ubuntu]({{< ref "/manuals/DockerEngine/Install/Ubuntu" >}})     |       ✅        |        ✅        |      ✅       |    ✅    |   ✅   |
| [Binaries]({{< ref "/manuals/DockerEngine/Install/Binaries" >}}) |       ✅        |        ✅        |      ✅       |         |       |

🚧 = Experimental

### Other Linux distros

> **Note**
>
> 
>
> While the following instructions may work, Docker doesn't test or verify installation on distro derivatives.

- If you use Debian derivatives such as "BunsenLabs Linux", "Kali Linux" or "LMDE" (Debian-based Mint) should follow the installation instructions for [Debian]({{< ref "/manuals/DockerEngine/Install/Debian" >}}), substitute the version of your distro for the corresponding Debian release. Refer to the documentation of your distro to find which Debian release corresponds with your derivative version.
- Likewise, if you use Ubuntu derivatives such as "Kubuntu", "Lubuntu" or "Xubuntu" you should follow the installation instructions for [Ubuntu]({{< ref "/manuals/DockerEngine/Install/Ubuntu" >}}), substituting the version of your distro for the corresponding Ubuntu release. Refer to the documentation of your distro to find which Ubuntu release corresponds with your derivative version.
- Some Linux distros provide a package of Docker Engine through their package repositories. These packages are built and maintained by the Linux distro's package maintainers and may have differences in configuration or are built from modified source code. Docker isn't involved in releasing these packages and you should report any bugs or issues involving these packages to your Linux distro's issue tracker.

Docker provides [binaries]({{< ref "/manuals/DockerEngine/Install/Binaries" >}}) for manual installation of Docker Engine. These binaries are statically linked and you can use them on any Linux distro.

## Release channels

Docker Engine has two types of update channels, **stable** and **test**:

- The **stable** channel gives you the latest versions released for general availability.
- The **test** channel gives you pre-release versions that are ready for testing before general availability.

Use the test channel with caution. Pre-release versions include experimental and early-access features that are subject to breaking changes.

## Support

Docker Engine is an open source project, supported by the Moby project maintainers and community members. Docker doesn't provide support for Docker Engine. Docker provides support for Docker products, including Docker Desktop, which uses Docker Engine as one of its components.

For information about the open source project, refer to the [Moby project website](https://mobyproject.org/).

### Upgrade path

Patch releases are always backward compatible with its major and minor version.

### Licensing

Docker Engine is licensed under the Apache License, Version 2.0. See [LICENSE](https://github.com/moby/moby/blob/master/LICENSE) for the full license text.

## Reporting security issues

If you discover a security issue, we request that you bring it to our attention immediately.

DO NOT file a public issue. Instead, submit your report privately to [security@docker.com]({{< ref "/manuals/DockerEngine/Install" >}}).

Security reports are greatly appreciated, and Docker will publicly thank you for it.

## Get started

After setting up Docker, you can learn the basics with [Getting started with Docker]({{< ref "/get-started/Introduction" >}}).
