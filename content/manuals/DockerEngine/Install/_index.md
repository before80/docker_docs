+++
title = "Install"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install Docker Engine

This section describes how to install Docker Engine on Linux, also known as Docker CE. Docker Engine is also available for Windows, macOS, and Linux, through Docker Desktop. For instructions on how to install Docker Desktop, see:

- [Docker Desktop for Linux](https://docs.docker.com/desktop/install/linux/)
- [Docker Desktop for Mac (macOS)](https://docs.docker.com/desktop/install/mac-install/)
- [Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/)

## [Supported platforms](https://docs.docker.com/engine/install/#supported-platforms)

| Platform                                                     | x86_64 / amd64 | arm64 / aarch64 | arm (32-bit) | ppc64le | s390x |
| :----------------------------------------------------------- | :------------: | :-------------: | :----------: | :-----: | :---: |
| [CentOS](https://docs.docker.com/engine/install/centos/)     |       ✅        |        ✅        |              |    ✅    |       |
| [Debian](https://docs.docker.com/engine/install/debian/)     |       ✅        |        ✅        |      ✅       |    ✅    |       |
| [Fedora](https://docs.docker.com/engine/install/fedora/)     |       ✅        |        ✅        |              |    ✅    |       |
| [Raspberry Pi OS (32-bit)](https://docs.docker.com/engine/install/raspberry-pi-os/) |                |                 |      ✅       |         |       |
| [RHEL](https://docs.docker.com/engine/install/rhel/)         |       🚧        |        🚧        |              |         |   ✅   |
| [SLES](https://docs.docker.com/engine/install/sles/)         |                |                 |              |         |   ✅   |
| [Ubuntu](https://docs.docker.com/engine/install/ubuntu/)     |       ✅        |        ✅        |      ✅       |    ✅    |   ✅   |
| [Binaries](https://docs.docker.com/engine/install/binaries/) |       ✅        |        ✅        |      ✅       |         |       |

🚧 = Experimental

### [Other Linux distros](https://docs.docker.com/engine/install/#other-linux-distros)

> **Note**
>
> 
>
> While the following instructions may work, Docker doesn't test or verify installation on distro derivatives.

- If you use Debian derivatives such as "BunsenLabs Linux", "Kali Linux" or "LMDE" (Debian-based Mint) should follow the installation instructions for [Debian](https://docs.docker.com/engine/install/debian/), substitute the version of your distro for the corresponding Debian release. Refer to the documentation of your distro to find which Debian release corresponds with your derivative version.
- Likewise, if you use Ubuntu derivatives such as "Kubuntu", "Lubuntu" or "Xubuntu" you should follow the installation instructions for [Ubuntu](https://docs.docker.com/engine/install/ubuntu/), substituting the version of your distro for the corresponding Ubuntu release. Refer to the documentation of your distro to find which Ubuntu release corresponds with your derivative version.
- Some Linux distros provide a package of Docker Engine through their package repositories. These packages are built and maintained by the Linux distro's package maintainers and may have differences in configuration or are built from modified source code. Docker isn't involved in releasing these packages and you should report any bugs or issues involving these packages to your Linux distro's issue tracker.

Docker provides [binaries](https://docs.docker.com/engine/install/binaries/) for manual installation of Docker Engine. These binaries are statically linked and you can use them on any Linux distro.

## [Release channels](https://docs.docker.com/engine/install/#release-channels)

Docker Engine has two types of update channels, **stable** and **test**:

- The **stable** channel gives you the latest versions released for general availability.
- The **test** channel gives you pre-release versions that are ready for testing before general availability.

Use the test channel with caution. Pre-release versions include experimental and early-access features that are subject to breaking changes.

## [Support](https://docs.docker.com/engine/install/#support)

Docker Engine is an open source project, supported by the Moby project maintainers and community members. Docker doesn't provide support for Docker Engine. Docker provides support for Docker products, including Docker Desktop, which uses Docker Engine as one of its components.

For information about the open source project, refer to the [Moby project website](https://mobyproject.org/).

### [Upgrade path](https://docs.docker.com/engine/install/#upgrade-path)

Patch releases are always backward compatible with its major and minor version.

### [Licensing](https://docs.docker.com/engine/install/#licensing)

Docker Engine is licensed under the Apache License, Version 2.0. See [LICENSE](https://github.com/moby/moby/blob/master/LICENSE) for the full license text.

## [Reporting security issues](https://docs.docker.com/engine/install/#reporting-security-issues)

If you discover a security issue, we request that you bring it to our attention immediately.

DO NOT file a public issue. Instead, submit your report privately to [security@docker.com](https://docs.docker.com/engine/install/).

Security reports are greatly appreciated, and Docker will publicly thank you for it.

## [Get started](https://docs.docker.com/engine/install/#get-started)

After setting up Docker, you can learn the basics with [Getting started with Docker](https://docs.docker.com/get-started/introduction/).
