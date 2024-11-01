+++
title = "已知问题"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/troubleshoot/known-issues/](https://docs.docker.com/desktop/troubleshoot/known-issues/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Known issues - 已知问题

{{< tabpane text=true persist=disabled >}}

{{% tab header="For all platforms " %}}

- IPv6 is not yet supported on Docker Desktop.
  - Docker Desktop 目前尚不支持 IPv6。


{{% /tab  %}}

{{% tab header="For Mac with Intel chip" %}}

- The Mac Activity Monitor reports that Docker is using twice the amount of memory it's actually using. This is due to a bug in MacOS. We have written [a detailed report](https://docs.google.com/document/d/17ZiQC1Tp9iH320K-uqVLyiJmk4DHJ3c4zgQetJiKYQM/edit?usp=sharing) on this.

  - Mac 活动监视器显示 Docker 使用的内存是实际使用量的两倍。这是由于 macOS 的一个错误引起的。我们已撰写了一份[详细报告](https://docs.google.com/document/d/17ZiQC1Tp9iH320K-uqVLyiJmk4DHJ3c4zgQetJiKYQM/edit?usp=sharing)。

- Force-ejecting the `.dmg` after running `Docker.app` from it can cause the whale icon to become unresponsive, Docker tasks to show as not responding in the Activity Monitor, and for some processes to consume a large amount of CPU resources. Reboot and restart Docker to resolve these issues.

  - 从 `.dmg` 中运行 `Docker.app` 后强制弹出该文件可能导致鲸鱼图标无响应，活动监视器中显示 Docker 任务未响应，部分进程消耗大量 CPU 资源。重启并重新启动 Docker 可以解决这些问题。

- Docker doesn't auto-start after sign in even when it's enabled in **Settings**. This is related to a set of issues with Docker helper, registration, and versioning.

  - 即使在 **设置** 中启用了 Docker 自启动功能，Docker 也不会在登录后自动启动。这与 Docker 助手、注册和版本控制的问题有关。

- Docker Desktop uses the `HyperKit` hypervisor ( https://github.com/docker/hyperkit) in macOS 10.10 Yosemite and higher. If you are developing with tools that have conflicts with `HyperKit`, such as [Intel Hardware Accelerated Execution Manager (HAXM)](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/), the current workaround is not to run them at the same time. You can pause `HyperKit` by quitting Docker Desktop temporarily while you work with HAXM. This allows you to continue work with the other tools and prevent `HyperKit` from interfering.

  - Docker Desktop 在 macOS 10.10 Yosemite 及更高版本中使用 `HyperKit` 虚拟机管理程序 (https://github.com/docker/hyperkit)。如果您使用的开发工具与 `HyperKit` 存在冲突，如 [Intel Hardware Accelerated Execution Manager (HAXM)](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/)，目前的解决方法是避免同时运行它们。您可以在使用 HAXM 时暂时退出 Docker Desktop 以暂停 `HyperKit`，从而防止冲突。

- If you are working with applications like [Apache Maven](https://maven.apache.org/) that expect settings for `DOCKER_HOST` and `DOCKER_CERT_PATH` environment variables, specify these to connect to Docker instances through Unix sockets. For example:

  如果您使用的应用程序（如 [Apache Maven](https://maven.apache.org/)）期望设置 `DOCKER_HOST` 和 `DOCKER_CERT_PATH` 环境变量，请通过 Unix 套接字来指定这些变量以连接到 Docker 实例。例如：

  ```console
  $ export DOCKER_HOST=unix:///var/run/docker.sock
  ```

- There are a number of issues with the performance of directories bind-mounted into containers. In particular, writes of small blocks, and traversals of large directories are currently slow. Additionally, containers that perform large numbers of directory operations, such as repeated scans of large directory trees, may suffer from poor performance. Applications that behave in this way include: 将目录绑定到容器内的性能存在一些问题，特别是小块写入和大目录遍历操作目前较慢。此外，执行大量目录操作的容器（如反复扫描大型目录树）可能会出现性能下降。这类应用程序包括：

  - `rake`
  - `ember build`
  - Symfony
  - Magento
  - Zend Framework
  - PHP applications that use [Composer](https://getcomposer.org/) to install dependencies in a `vendor` folder - 使用 [Composer](https://getcomposer.org/) 在 `vendor` 文件夹中安装依赖项的 PHP 应用程序

  As a workaround for this behavior, you can put vendor or third-party library directories in Docker volumes, perform temporary file system operations outside of bind mounts, and use third-party tools like Unison or `rsync` to synchronize between container directories and bind-mounted directories. We are actively working on performance improvements using a number of different techniques. To learn more, see the [topic on our roadmap](https://github.com/docker/roadmap/issues/7).

  ​	为解决这些问题，可以将供应商或第三方库目录放入 Docker 卷中，在绑定挂载之外执行临时文件系统操作，并使用 Unison 或 `rsync` 等第三方工具在容器目录和绑定挂载目录之间进行同步。我们正积极采用多种技术进行性能改进。详细了解，请参阅我们的[路线图主题](https://github.com/docker/roadmap/issues/7)。

{{% /tab  %}}

{{% tab header="For Mac with Apple silicon" %}}

On Apple silicon in native `arm64` containers, older versions of `libssl` such as `debian:buster`, `ubuntu:20.04`, and `centos:8` will segfault when connected to some TLS servers, for example, `curl https://dl.yarnpkg.com`. The bug is fixed in newer versions of `libssl` in `debian:bullseye`, `ubuntu:21.04`, and `fedora:35`.

​	在搭载 Apple Silicon 的原生 `arm64` 容器中，较旧版本的 `libssl`（如 `debian:buster`、`ubuntu:20.04` 和 `centos:8`）在连接到某些 TLS 服务器时会发生段错误，例如 `curl https://dl.yarnpkg.com`。该错误已在较新版本的 `libssl` 中修复，如 `debian:bullseye`、`ubuntu:21.04` 和 `fedora:35`。

Some command line tools do not work when Rosetta 2 is not installed.

​	一些命令行工具在未安装 Rosetta 2 时无法工作。

- The old version 1.x of `docker-compose`. Use Compose V2 instead - type `docker compose`.
  - 旧版本的 `docker-compose` 1.x。请改用 Compose V2，即键入 `docker compose`。

- The `docker-credential-ecr-login` credential helper.
  - `docker-credential-ecr-login` 凭证助手。


Some images do not support the ARM64 architecture. You can add `--platform linux/amd64` to run (or build) an Intel image using emulation.

​	一些镜像不支持 ARM64 架构。您可以添加 `--platform linux/amd64` 以使用仿真运行（或构建）Intel 镜像。

However, attempts to run Intel-based containers on Apple silicon machines under emulation can crash as qemu sometimes fails to run the container. In addition, filesystem change notification APIs (`inotify`) do not work under qemu emulation. Even when the containers do run correctly under emulation, they will be slower and use more memory than the native equivalent.

​	然而，尝试在 Apple Silicon 机器上以仿真方式运行基于 Intel 的容器时，qemu 有时会无法运行容器，导致崩溃。此外，文件系统变更通知 API（`inotify`）在 qemu 仿真下无法正常工作。即使容器能够在仿真下正常运行，速度也会较慢且占用更多内存。

In summary, running Intel-based containers on Arm-based machines should be regarded as "best effort" only. We recommend running arm64 containers on Apple silicon machines whenever possible, and encouraging container authors to produce arm64, or multi-arch, versions of their containers. This issue should become less common over time, as more and more images are rebuilt [supporting multiple architectures](https://www.docker.com/blog/multi-arch-build-and-images-the-simple-way/).

​	总而言之，在基于 ARM 的机器上运行基于 Intel 的容器应仅作为“尽力而为”的方案。我们建议在 Apple Silicon 机器上运行 arm64 容器，并鼓励容器作者生成 arm64 或多架构版本。随着越来越多的镜像[支持多架构](https://www.docker.com/blog/multi-arch-build-and-images-the-simple-way/)，此问题将逐渐减少。

`ping` from inside a container to the Internet does not work as expected. To test the network, use `curl` or `wget`. See [docker/for-mac#5322](https://github.com/docker/for-mac/issues/5322#issuecomment-809392861).

​	容器内对互联网执行 `ping` 不会如预期般工作。要测试网络连接，请使用 `curl` 或 `wget`。请参阅 [docker/for-mac#5322](https://github.com/docker/for-mac/issues/5322#issuecomment-809392861)。

Users may occasionally experience data drop when a TCP stream is half-closed.

​	用户可能偶尔会在半关闭的 TCP 流中遇到数据丢失的情况。

{{% /tab  %}}

{{< /tabpane >}}
