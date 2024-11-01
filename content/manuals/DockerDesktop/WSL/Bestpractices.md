+++
title = "最佳实践"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/wsl/best-practices/](https://docs.docker.com/desktop/wsl/best-practices/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Best practices - 最佳实践

- Always use the latest version of WSL. At a minimum you must use WSL version 1.1.3.0., otherwise Docker Desktop may not work as expected. Testing, development, and documentation is based on the newest kernel versions. Older versions of WSL can cause: 始终使用最新版本的 WSL。最低要求为 WSL 版本 1.1.3.0，否则 Docker Desktop 可能无法正常工作。测试、开发和文档均基于最新的内核版本。旧版本的 WSL 可能会导致：
  - Docker Desktop to hang periodically or when upgrading
    - Docker Desktop 定期挂起或在升级时挂起
  - Deployment via SCCM to fail
    - 通过 SCCM 部署失败
  - The `vmmem.exe` to consume all memory
    - `vmmem.exe` 占用所有内存
  - Network filter policies to be applied globally, not to specific objects
    - 网络过滤策略全局应用，而非针对特定对象
  - GPU failures with containers
    - 容器中的 GPU 故障
- To get the best out of the file system performance when bind-mounting files, it's recommended that you store source code and other data that is bind-mounted into Linux containers. For instance, use `docker run -v <host-path>:<container-path>` in the Linux file system, rather than the Windows file system. You can also refer to the [recommendation](https://learn.microsoft.com/en-us/windows/wsl/compare-versions) from Microsoft. 为了在绑定挂载文件时获得最佳文件系统性能，建议将源代码和其他数据存储在绑定挂载到 Linux 容器中的 Linux 文件系统中。例如，使用 `docker run -v <host-path>:<container-path>` 绑定到 Linux 文件系统，而不是 Windows 文件系统。也可以参考 [微软的建议](https://learn.microsoft.com/en-us/windows/wsl/compare-versions)。
  - Linux containers only receive file change events, “inotify events”, if the original files are stored in the Linux filesystem. For example, some web development workflows rely on inotify events for automatic reloading when files have changed.
    - 仅当原始文件存储在 Linux 文件系统中时，Linux 容器才会接收到文件更改事件（“inotify 事件”）。例如，一些 Web 开发流程依赖于 inotify 事件在文件更改时自动重新加载。
  - Performance is much higher when files are bind-mounted from the Linux filesystem, rather than remoted from the Windows host. Therefore avoid `docker run -v /mnt/c/users:/users,` where `/mnt/c` is mounted from Windows.
    - 文件从 Linux 文件系统绑定挂载时的性能远高于从 Windows 主机远程挂载。因此请避免使用 `docker run -v /mnt/c/users:/users`，其中 `/mnt/c` 是从 Windows 挂载的。
  - Instead, from a Linux shell use a command like `docker run -v ~/my-project:/sources <my-image>` where `~` is expanded by the Linux shell to `$HOME`.
    - 相反，从 Linux shell 使用类似 `docker run -v ~/my-project:/sources <my-image>` 的命令，其中 `~` 由 Linux shell 展开为 `$HOME`。
- If you have concerns about the size of the `docker-desktop-data` distribution, take a look at the [WSL tooling built into Windows](https://learn.microsoft.com/en-us/windows/wsl/disk-space).  如果您担心 `docker-desktop-data` 分发的大小，可以查看[Windows 内置的 WSL 工具](https://learn.microsoft.com/en-us/windows/wsl/disk-space)。
  - Installations of Docker Desktop version 4.30 and later no longer rely on the `docker-desktop-data` distribution; instead Docker Desktop creates and manages its own virtual hard disk (VHDX) for storage. (note, however, that Docker Desktop keeps using the `docker-desktop-data` distribution if it was already created by an earlier version of the software).
    - Docker Desktop 4.30 及更高版本不再依赖 `docker-desktop-data` 分发；相反，Docker Desktop 创建并管理其自己的虚拟硬盘 (VHDX) 进行存储。（请注意，如果较早版本的软件已经创建了 `docker-desktop-data` 分发，Docker Desktop 将继续使用它。）
  - Starting from version 4.34 and later, Docker Desktop automatically manages the size of the managed VHDX and returns unused space to the operating system.
    - 从 4.34 版本起，Docker Desktop 自动管理受控 VHDX 的大小，并将未使用的空间返回给操作系统。
- If you have concerns about CPU or memory usage, you can configure limits on the memory, CPU, and swap size allocated to the [WSL 2 utility VM](https://learn.microsoft.com/en-us/windows/wsl/wsl-config#global-configuration-options-with-wslconfig).
  - 如果您担心 CPU 或内存使用量，可以对分配给 [WSL 2 实用程序 VM](https://learn.microsoft.com/en-us/windows/wsl/wsl-config#global-configuration-options-with-wslconfig) 的内存、CPU 和交换分区大小设置限制。
