+++
title = "Windows"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/desktop/faqs/windowsfaqs/](https://docs.docker.com/desktop/faqs/windowsfaqs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# FAQs for Docker Desktop for Windows - Windows 版 Docker Desktop 常见问题解答

### 我可以与 Docker Desktop 一起使用 VirtualBox 吗？ Can I use VirtualBox alongside Docker Desktop?

Yes, you can run VirtualBox along with Docker Desktop if you have enabled the [Windows Hypervisor Platform](https://docs.microsoft.com/en-us/virtualization/api/) feature on your machine.

​	可以，如果您已在计算机上启用了 [Windows Hypervisor Platform](https://docs.microsoft.com/en-us/virtualization/api/) 功能，则可以与 Docker Desktop 一起运行 VirtualBox。

### 为什么需要 Windows 10 或 Windows 11？ Why is Windows 10 or Windows 11 required?

Docker Desktop uses the Windows Hyper-V features. While older Windows versions have Hyper-V, their Hyper-V implementations lack features critical for Docker Desktop to work.

​	Docker Desktop 使用 Windows 的 Hyper-V 功能。虽然旧版 Windows 也有 Hyper-V，但其实现缺少 Docker Desktop 运行所需的关键功能。

### 我可以在 Windows Server 上运行 Docker Desktop 吗？ Can I run Docker Desktop on Windows Server?

No, running Docker Desktop on Windows Server is not supported.

​	不支持在 Windows Server 上运行 Docker Desktop。

### 我可以更改共享卷的权限以满足容器的特定部署要求吗？ Can I change permissions on shared volumes for container-specific deployment requirements?

Docker Desktop does not enable you to control (`chmod`) the Unix-style permissions on [shared volumes](https://docs.docker.com/desktop/settings/#file-sharing) for deployed containers, but rather sets permissions to a default value of [0777](https://chmodcommand.com/chmod-0777/) (`read`, `write`, `execute` permissions for `user` and for `group`) which is not configurable.

​	Docker Desktop 不支持对部署的容器设置 [共享卷](https://docs.docker.com/desktop/settings/#file-sharing) 的 Unix 样式权限（chmod），默认权限值为 [0777](https://chmodcommand.com/chmod-0777/)（`用户` 和 `组` 均可 `读`、`写` 和 `执行`），不可配置。

For workarounds and to learn more, see [Permissions errors on data directories for shared volumes](https://docs.docker.com/desktop/troubleshoot/topics/#permissions-errors-on-data-directories-for-shared-volumes).

​	有关解决方法和详细信息，请参阅[共享卷数据目录的权限错误](https://docs.docker.com/desktop/troubleshoot/topics/#permissions-errors-on-data-directories-for-shared-volumes)。

### 符号链接在 Windows 上如何工作？How do symlinks work on Windows?

Docker Desktop supports two types of symlinks: Windows native symlinks and symlinks created inside a container.

​	Docker Desktop 支持两种类型的符号链接：Windows 原生符号链接和容器内创建的符号链接。

The Windows native symlinks are visible within the containers as symlinks, whereas symlinks created inside a container are represented as [mfsymlinks](https://wiki.samba.org/index.php/UNIX_Extensions#Minshall.2BFrench_symlinks). These are regular Windows files with a special metadata. Therefore the symlinks created inside a container appear as symlinks inside the container, but not on the host.

​	Windows 原生符号链接在容器内可见为符号链接，而容器内创建的符号链接显示为 [mfsymlinks](https://wiki.samba.org/index.php/UNIX_Extensions#Minshall.2BFrench_symlinks)。这些是带有特殊元数据的常规 Windows 文件，因此在容器内创建的符号链接在容器内显示为符号链接，但在主机上不显示。

### 与 Kubernetes 和 WSL 2 共享文件 File sharing with Kubernetes and WSL 2

Docker Desktop mounts the Windows host filesystem under `/run/desktop` inside the container running Kubernetes. See the [Stack Overflow post](https://stackoverflow.com/questions/67746843/clear-persistent-volume-from-a-kubernetes-cluster-running-on-docker-desktop/69273405#69273) for an example of how to configure a Kubernetes Persistent Volume to represent directories on the host.

​	Docker Desktop 将 Windows 主机文件系统挂载在运行 Kubernetes 的容器内的 `/run/desktop` 下。有关如何配置 Kubernetes Persistent Volume 来表示主机目录的示例，请参阅 [Stack Overflow 帖子](https://stackoverflow.com/questions/67746843/clear-persistent-volume-from-a-kubernetes-cluster-running-on-docker-desktop/69273405#69273)。

### 如何添加自定义 CA 证书？ How do I add custom CA certificates?

You can add trusted Certificate Authorities (CAs) to your Docker daemon to verify registry server certificates, and client certificates, to authenticate to registries.

​	您可以将受信任的证书颁发机构 (CA) 添加到您的 Docker 守护程序中以验证注册表服务器证书，以及客户端证书以认证到注册表。

Docker Desktop supports all trusted Certificate Authorities (CAs) (root or intermediate). Docker recognizes certs stored under Trust Root Certification Authorities or Intermediate Certification Authorities.

​	Docker Desktop 支持所有受信任的证书颁发机构 (CA)（根或中间）。Docker 会识别存储在信任根证书颁发机构或中间证书颁发机构下的证书。

Docker Desktop creates a certificate bundle of all user-trusted CAs based on the Windows certificate store, and appends it to Moby trusted certificates. Therefore, if an enterprise SSL certificate is trusted by the user on the host, it is trusted by Docker Desktop.

​	Docker Desktop 基于 Windows 证书存储创建所有用户受信任 CA 的证书包，并将其附加到 Moby 受信任的证书。因此，如果主机上的用户信任企业 SSL 证书，它也会被 Docker Desktop 信任。

To learn more about how to install a CA root certificate for the registry, see [Verify repository client with certificates]({{< ref "/manuals/DockerEngine/Security/Verifyrepositoryclientwithcertificates" >}}) in the Docker Engine topics.

​	有关如何为注册表安装 CA 根证书的更多信息，请参阅 Docker Engine 主题中的 [使用证书验证存储库客户端]({{< ref "/manuals/DockerEngine/Security/Verifyrepositoryclientwithcertificates" >}})。

### 如何添加客户端证书？ How do I add client certificates?

You can add your client certificates in `~/.docker/certs.d/<MyRegistry><Port>/client.cert` and `~/.docker/certs.d/<MyRegistry><Port>/client.key`. You do not need to push your certificates with `git` commands.

​	您可以将客户端证书放在 `~/.docker/certs.d/<MyRegistry><Port>/client.cert` 和 `~/.docker/certs.d/<MyRegistry><Port>/client.key`。不需要通过 `git` 命令推送证书。

When the Docker Desktop application starts, it copies the `~/.docker/certs.d` folder on your Windows system to the `/etc/docker/certs.d` directory on Moby (the Docker Desktop virtual machine running on Hyper-V).

​	Docker Desktop 应用程序启动时，会将 Windows 系统上的 `~/.docker/certs.d` 文件夹复制到 Moby（在 Hyper-V 上运行的 Docker Desktop 虚拟机）中的 `/etc/docker/certs.d` 目录中。

You need to restart Docker Desktop after making any changes to the keychain or to the `~/.docker/certs.d` directory in order for the changes to take effect.

​	在更改钥匙串或 `~/.docker/certs.d` 目录后，您需要重启 Docker Desktop 以使更改生效。

The registry cannot be listed as an insecure registry (see [Docker Daemon](https://docs.docker.com/desktop/settings/#docker-engine)). Docker Desktop ignores certificates listed under insecure registries, and does not send client certificates. Commands like `docker run` that attempt to pull from the registry produce error messages on the command line, as well as on the registry.

​	注册表不能被列为不安全注册表（请参阅 [Docker 守护程序](https://docs.docker.com/desktop/settings/#docker-engine)）。Docker Desktop 忽略不安全注册表下的证书，不发送客户端证书。尝试从注册表拉取的命令（如 `docker run`）会在命令行和注册表中显示错误消息。

To learn more about how to set the client TLS certificate for verification, see [Verify repository client with certificates]({{< ref "/manuals/DockerEngine/Security/Verifyrepositoryclientwithcertificates" >}}) in the Docker Engine topics.

​	有关如何设置客户端 TLS 证书以进行验证的更多信息，请参阅 Docker Engine 主题中的 [使用证书验证存储库客户端]({{< ref "/manuals/DockerEngine/Security/Verifyrepositoryclientwithcertificates" >}})。
