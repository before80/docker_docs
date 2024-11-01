+++
title = "Linux"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/desktop/faqs/linuxfaqs/](https://docs.docker.com/desktop/faqs/linuxfaqs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# FAQs for Docker Desktop for Linux - Docker Desktop for Linux 常见问题

### 为什么 Docker Desktop for Linux 需要运行虚拟机？ Why does Docker Desktop for Linux run a VM?

Docker Desktop for Linux runs a Virtual Machine (VM) for the following reasons:

​	Docker Desktop for Linux 运行虚拟机（VM）的原因如下：

1. To ensure that Docker Desktop provides a consistent experience across platforms. 确保 Docker Desktop 在所有平台上提供一致的体验。

   During research, the most frequently cited reason for users wanting Docker Desktop for Linux was to ensure a consistent Docker Desktop experience with feature parity across all major operating systems. Utilizing a VM ensures that the Docker Desktop experience for Linux users will closely match that of Windows and macOS.

   ​	研究表明，用户期望 Docker Desktop for Linux 的主要原因是确保 Docker Desktop 在所有主要操作系统上的功能一致。利用虚拟机可以确保 Linux 用户的 Docker Desktop 体验与 Windows 和 macOS 用户一致。

2. To make use of new kernel features. 利用新的内核特性。

   Sometimes we want to make use of new operating system features. Because we control the kernel and the OS inside the VM, we can roll these out to all users immediately, even to users who are intentionally sticking on an LTS version of their machine OS.

   ​	有时我们希望利用新的操作系统特性。因为我们可以控制虚拟机中的内核和操作系统，因此可以立即向所有用户推出这些特性，即使用户选择保留其主机操作系统的长期支持版本。

3. To enhance security. 增强安全性。

   Container image vulnerabilities pose a security risk for the host environment. There is a large number of unofficial images that are not guaranteed to be verified for known vulnerabilities. Malicious users can push images to public registries and use different methods to trick users into pulling and running them. The VM approach mitigates this threat as any malware that gains root privileges is restricted to the VM environment without access to the host.

   ​	镜像中的漏洞对主机环境构成安全风险。许多非官方镜像并未验证已知漏洞。恶意用户可以将镜像上传到公共注册表，并使用不同的方法诱使用户拉取和运行它们。虚拟机方法可以减轻此威胁，因为任何获得 root 权限的恶意软件仅限于虚拟机环境，无法访问主机。

   Why not run rootless Docker? Although this has the benefit of superficially limiting access to the root user so everything looks safer in "top", it allows unprivileged users to gain `CAP_SYS_ADMIN` in their own user namespace and access kernel APIs which are not expecting to be used by unprivileged users, resulting in [vulnerabilities](https://www.openwall.com/lists/oss-security/2022/01/18/7).

   ​	为什么不运行无 root 的 Docker？尽管这在“top”中显示表面上限制了对 root 用户的访问，但它允许非特权用户在自己的用户命名空间中获得 `CAP_SYS_ADMIN` 并访问不打算由非特权用户使用的内核 API，从而导致 [漏洞](https://www.openwall.com/lists/oss-security/2022/01/18/7)。

4. To provide the benefits of feature parity and enhanced security, with minimal impact on performance. 在确保功能一致和增强安全的同时，尽量减少性能影响。

   The VM utilized by Docker Desktop for Linux uses [`VirtioFS`](https://virtio-fs.gitlab.io/), a shared file system that allows virtual machines to access a directory tree located on the host. Our internal benchmarking shows that with the right resource allocation to the VM, near native file system performance can be achieved with VirtioFS.

   ​	Docker Desktop for Linux 使用 [`VirtioFS`](https://virtio-fs.gitlab.io/) 共享文件系统，使虚拟机能够访问主机上的目录树。我们内部基准测试显示，通过适当的资源分配，VirtioFS 可实现接近原生的文件系统性能。
   
   As such, we have adjusted the default memory available to the VM in Docker Desktop for Linux. You can tweak this setting to your specific needs by using the **Memory** slider within the **Settings** > **Resources** tab of Docker Desktop.
   
   ​	因此，我们调整了 Docker Desktop for Linux 中虚拟机的默认可用内存。您可以在 Docker Desktop 的 **设置** > **资源** 选项卡中的 **内存** 滑块中根据需要进行调整。

### 如何启用文件共享？ How do I enable file sharing?

Docker Desktop for Linux uses [VirtioFS](https://virtio-fs.gitlab.io/) as the default (and currently only) mechanism to enable file sharing between the host and Docker Desktop VM. In order not to require elevated privileges, without unnecessarily restricting operations on the shared files, Docker Desktop runs the file sharing service (`virtiofsd`) inside a user namespace (see `user_namespaces(7)`) with UID and GID mapping configured. As a result Docker Desktop relies on the host being configured to enable the current user to use subordinate ID delegation. For this to be true `/etc/subuid` (see `subuid(5)`) and `/etc/subgid` (see `subgid(5)`) must be present. Docker Desktop only supports subordinate ID delegation configured via files. Docker Desktop maps the current user ID and GID to 0 in the containers. It uses the first entry corresponding to the current user in `/etc/subuid` and `/etc/subgid` to set up mappings for IDs above 0 in the containers.

​	Docker Desktop for Linux 使用 [VirtioFS](https://virtio-fs.gitlab.io/) 作为主机与 Docker Desktop 虚拟机之间文件共享的默认（且当前唯一）机制。为了避免不必要的权限提升，同时不限制对共享文件的操作，Docker Desktop 在用户命名空间中运行文件共享服务（`virtiofsd`），并配置了 UID 和 GID 映射。因此 Docker Desktop 依赖主机配置，以便当前用户能够使用下属 ID 委派。这要求 `/etc/subuid`（参见 `subuid(5)`）和 `/etc/subgid`（参见 `subgid(5)`）文件存在。Docker Desktop 仅支持通过文件配置的下属 ID 委派。Docker Desktop 将当前用户 ID 和 GID 映射为容器中的 0，并使用 `/etc/subuid` 和 `/etc/subgid` 中对应当前用户的第一个条目设置 0 以上的 ID 映射。

| ID in container | ID on host                                                   |
| --------------- | ------------------------------------------------------------ |
| 0 (root)        | ID of the user running DD (e.g. 1000)                        |
| 1               | 0 + beginning of ID range specified in `/etc/subuid`/`/etc/subgid` (e.g. 100000) |
| 2               | 1 + beginning of ID range specified in `/etc/subuid`/`/etc/subgid` (e.g. 100001) |
| 3               | 2 + beginning of ID range specified in `/etc/subuid`/`/etc/subgid` (e.g. 100002) |
| ...             | ...                                                          |

If `/etc/subuid` and `/etc/subgid` are missing, they need to be created. Both should contain entries in the form - `<username>:<start of id range>:<id range size>`. For example, to allow the current user to use IDs from 100 000 to 165 535:

​	如果 `/etc/subuid` 和 `/etc/subgid` 缺失，需要创建它们。每个文件应包含格式为 `<username>:<start of id range>:<id range size>` 的条目。例如，允许当前用户使用从 100000 到 165535 的 ID：



```console
$ grep "$USER" /etc/subuid >> /dev/null 2&>1 || (echo "$USER:100000:65536" | sudo tee -a /etc/subuid)
$ grep "$USER" /etc/subgid >> /dev/null 2&>1 || (echo "$USER:100000:65536" | sudo tee -a /etc/subgid)
```

To verify the configs have been created correctly, inspect their contents:

​	要验证配置是否正确创建，请检查其内容：



```console
$ echo $USER
exampleuser
$ cat /etc/subuid
exampleuser:100000:65536
$ cat /etc/subgid
exampleuser:100000:65536
```

In this scenario if a shared file is `chown`ed inside a Docker Desktop container owned by a user with a UID of 1000, it shows up on the host as owned by a user with a UID of 100999. This has the unfortunate side effect of preventing easy access to such a file on the host. The problem is resolved by creating a group with the new GID and adding our user to it, or by setting a recursive ACL (see `setfacl(1)`) for folders shared with the Docker Desktop VM.

​	在这种情况下，如果共享文件在 Docker Desktop 容器中由 UID 为 1000 的用户 `chown` 了，则该文件在主机上显示为 UID 为 100999 的用户所有。此问题可通过创建具有新 GID 的组并将用户添加到该组，或为 Docker Desktop VM 共享的文件夹设置递归 ACL（参见 `setfacl(1)`）来解决。

### Docker Desktop 在 Linux 上存储容器的位置？ Where does Docker Desktop store Linux containers?

Docker Desktop stores Linux containers and images in a single, large "disk image" file in the Linux filesystem. This is different from Docker on Linux, which usually stores containers and images in the `/var/lib/docker` directory on the host's filesystem.

​	Docker Desktop 将 Linux 容器和镜像存储在 Linux 文件系统中的单个“大型磁盘镜像”文件中。这与 Linux 上的 Docker 不同，通常将容器和镜像存储在主机文件系统的 `/var/lib/docker` 目录中。

#### 磁盘镜像文件的位置在哪里？ Where is the disk image file?

To locate the disk image file, select **Settings** from the Docker Dashboard then **Advanced** from the **Resources** tab.

​	要找到磁盘镜像文件的位置，从 Docker Dashboard 选择 **设置**，然后从 **资源** 选项卡选择 **高级**。

The **Advanced** tab displays the location of the disk image. It also displays the maximum size of the disk image and the actual space the disk image is consuming. Note that other tools might display space usage of the file in terms of the maximum file size, and not the actual file size.

​	**高级** 选项卡显示了磁盘镜像的位置、最大磁盘镜像大小以及磁盘镜像实际占用的空间。请注意，其他工具可能会显示文件的最大大小，而非实际大小。

##### 如果文件太大怎么办？ What if the file is too large?

If the disk image file is too large, you can:

​	如果磁盘镜像文件太大，您可以：

- Move it to a bigger drive
  - 将其移动到更大的驱动器

- Delete unnecessary containers and images
  - 删除不必要的容器和镜像

- Reduce the maximum allowable size of the file
  - 减少文件的最大允许大小

##### 如何将文件移动到更大的驱动器？ How do I move the file to a bigger drive?

To move the disk image file to a different location:

​	要将磁盘镜像文件移动到其他位置：

1. Select **Settings** then **Advanced** from the **Resources** tab. 选择 **设置**，然后从 **资源** 选项卡选择 **高级**。
2. In the **Disk image location** section, select **Browse** and choose a new location for the disk image. 在 **磁盘镜像位置** 部分中选择 **浏览** 并选择新的磁盘镜像位置。
3. Select **Apply & Restart** for the changes to take effect. 选择 **应用并重启** 以使更改生效。

Do not move the file directly in Finder as this can cause Docker Desktop to lose track of the file.

​	不要直接在 Finder 中移动文件，这会导致 Docker Desktop 无法找到该文件。

##### 如何删除不必要的容器和镜像？ How do I delete unnecessary containers and images?

Check whether you have any unnecessary containers and images. If your client and daemon API are running version 1.25 or later (use the `docker version` command on the client to check your client and daemon API versions), you can see the detailed space usage information by running:

​	检查是否有不必要的容器和镜像。如果客户端和守护进程 API 运行的是 1.25 或更高版本（使用 `docker version` 命令检查客户端和守护进程 API 版本），您可以通过运行以下命令查看详细的空间使用信息：

```console
$ docker system df -v
```

Alternatively, to list images, run:

​	此外，列出镜像可以使用：

```console
$ docker image ls
```

To list containers, run:

​	列出容器可以使用：

```console
$ docker container ls -a
```

If there are lots of redundant objects, run the command:

​	如果存在许多冗余对象，可以运行以下命令清理：

```console
$ docker system prune
```

This command removes all stopped containers, unused networks, dangling images, and build cache.

​	此命令将移除所有已停止的容器、未使用的网络、悬空的镜像和构建缓存。

It might take a few minutes to reclaim space on the host depending on the format of the disk image file:

​	根据磁盘镜像文件的格式，可能需要几分钟才能在主机上回收空间：

- If the file is named `Docker.raw`: space on the host should be reclaimed within a few seconds.
  - 如果文件名为 `Docker.raw`：主机上的空间应在几秒内回收。

- If the file is named `Docker.qcow2`: space will be freed by a background process after a few minutes.
  - 如果文件名为 `Docker.qcow2`：后台进程将在几分钟后释放空间。


Space is only freed when images are deleted. Space is not freed automatically when files are deleted inside running containers. To trigger a space reclamation at any point, run the command:

​	仅在删除镜像时才会释放空间。当删除运行中容器中的文件时，不会自动释放空间。要在任何时间点触发空间回收，可以运行以下命令：

```console
$ docker run --privileged --pid=host docker/desktop-reclaim-space
```

Note that many tools report the maximum file size, not the actual file size. To query the actual size of the file on the host from a terminal, run:

​	请注意，许多工具报告的是最大文件大小，而不是实际文件大小。要从终端查询主机上文件的实际大小，请运行以下命令：

```console
$ cd ~/.docker/desktop/vms/0/data
$ ls -klsh Docker.raw
2333548 -rw-r--r--@ 1 username  staff    64G Dec 13 17:42 Docker.raw
```

In this example, the actual size of the disk is `2333548` KB, whereas the maximum size of the disk is `64` GB.

​	在此示例中，磁盘的实际大小为 `2333548` KB，而磁盘的最大大小为 `64` GB。

##### 如何减少文件的最大大小？ How do I reduce the maximum size of the file?

To reduce the maximum size of the disk image file:

​	要减少磁盘镜像文件的最大大小：

1. From Docker Dashboard select **Settings** then **Advanced** from the **Resources** tab. 在 Docker Dashboard 中选择 **Settings**，然后在 **Resources** 标签下选择 **Advanced**。
2. The **Disk image size** section contains a slider that allows you to change the maximum size of the disk image. Adjust the slider to set a lower limit. 在 **Disk image size** 部分，有一个滑块可让您更改磁盘镜像的最大大小。调整滑块以设置较低的限制。
3. Select **Apply & Restart**. 选择 **Apply & Restart**。

When you reduce the maximum size, the current disk image file is deleted, and therefore, all containers and images are lost.

​	当您减少最大大小时，当前的磁盘镜像文件将被删除，因此，所有容器和镜像将丢失。
