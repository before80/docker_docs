+++
title = "Mac"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/desktop/faqs/macfaqs/](https://docs.docker.com/desktop/faqs/macfaqs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# FAQs for Docker Desktop for Mac - Docker Desktop for Mac 常见问题解答

### 为什么我总是收到通知，说某个应用程序更改了我的 Docker Desktop 配置？ Why do I keep getting a notification telling me an application has changed my Desktop configurations?

You receive this notification because the Configuration integrity check feature has detected that a third-party application has altered your Docker Desktop configuration. This usually happens due to incorrect or missing symlinks. The notification ensures you are aware of these changes so you can review and repair any potential issues to maintain system reliability.

​	您收到此通知是因为配置完整性检查功能检测到第三方应用更改了 Docker Desktop 配置。通常是由于符号链接不正确或缺失导致的。此通知确保您知晓这些更改，以便查看并修复潜在问题，从而保持系统的可靠性。

Opening the notification presents a pop-up window which provides detailed information about the detected integrity issues.

​	点击通知将显示弹出窗口，提供有关检测到的完整性问题的详细信息。

If you choose to ignore the notification, it will be shown again only at the next Docker Desktop startup. If you choose to repair your configuration, you won't be prompted again.

​	如果您选择忽略该通知，它将仅在下次启动 Docker Desktop 时再次显示。如果您选择修复配置，则不会再次提示。

If you want to switch off Configuration integrity check notifications, navigate to Docker Desktop's settings and in the **General** tab, clear the **Automatically check configuration** setting.

​	要关闭配置完整性检查通知，请转到 Docker Desktop 设置，在 **General** 选项卡中，取消选择 **Automatically check configuration** 设置。

If you have feedback on how to further improve the Configuration integrity check feature, [fill out the feedback form](https://docs.google.com/forms/d/e/1FAIpQLSeD_Odqc__4ihRXDtH_ba52QJuaKZ00qGnNa_tM72MmH32CZw/viewform).

​	如果您有关于如何改进配置完整性检查功能的反馈，请填写[反馈表单](https://docs.google.com/forms/d/e/1FAIpQLSeD_Odqc__4ihRXDtH_ba52QJuaKZ00qGnNa_tM72MmH32CZw/viewform)。

### 什么是 HyperKit？ What is HyperKit?

HyperKit is a hypervisor built on top of the Hypervisor.framework in macOS. It runs entirely in userspace and has no other dependencies.

​	HyperKit 是建立在 macOS 中 Hypervisor.framework 之上的一个虚拟机管理程序，完全在用户空间中运行，没有其他依赖项。

We use HyperKit to eliminate the need for other VM products, such as Oracle VirtualBox or VMWare Fusion.

​	我们使用 HyperKit 来消除对其他虚拟机产品（如 Oracle VirtualBox 或 VMWare Fusion）的需求。

### HyperKit 有什么好处？ What is the benefit of HyperKit?

HyperKit is thinner than VirtualBox and VMWare fusion, and the version included is customized for Docker workloads on Mac.

​	HyperKit 比 VirtualBox 和 VMWare Fusion 更轻量化，而且包含的版本专为 Mac 上的 Docker 工作负载而定制。

### 为什么退出应用后 `com.docker.vmnetd` 仍在运行？ Why is com.docker.vmnetd still running after I quit the app?

The privileged helper process `com.docker.vmnetd` is started by `launchd` and runs in the background. The process does not consume any resources unless `Docker.app` connects to it, so it's safe to ignore.

​	特权辅助进程 `com.docker.vmnetd` 是由 `launchd` 启动的，并在后台运行。该进程不会消耗资源，除非 `Docker.app` 连接到它，因此可以忽略它的存在。

### Docker Desktop 在哪里存储 Linux 容器和镜像？Where does Docker Desktop store Linux containers and images?

Docker Desktop stores Linux containers and images in a single, large "disk image" file in the Mac filesystem. This is different from Docker on Linux, which usually stores containers and images in the `/var/lib/docker` directory.

​	Docker Desktop 在 Mac 文件系统中将 Linux 容器和镜像存储在一个大的 “磁盘镜像” 文件中。这与 Linux 上的 Docker 不同，通常存储在主机文件系统的 `/var/lib/docker` 目录中。

#### 磁盘镜像文件在哪里？Where is the disk image file?

To locate the disk image file, select **Settings** from the Docker Dashboard then **Advanced** from the **Resources** tab.

​	要找到磁盘镜像文件，请从 Docker Dashboard 中选择 **Settings**，然后在 **Resources** 标签下选择 **Advanced**。

The **Advanced** tab displays the location of the disk image. It also displays the maximum size of the disk image and the actual space the disk image is consuming. Note that other tools might display space usage of the file in terms of the maximum file size, and not the actual file size.

​	**Advanced** 标签显示磁盘镜像的位置，还显示磁盘镜像的最大大小和实际占用的空间。请注意，其他工具可能会显示文件的最大文件大小，而不是实际大小。

#### 如果文件太大怎么办？ What if the file is too big?

If the disk image file is too big, you can:

​	如果磁盘镜像文件太大，您可以：

- Move it to a bigger drive
  - 将它移动到更大的驱动器

- Delete unnecessary containers and images
  - 删除不必要的容器和镜像

- Reduce the maximum allowable size of the file
  - 减小文件的最大允许大小

##### 如何将文件移动到更大的驱动器？How do I move the file to a bigger drive?

To move the disk image file to a different location:

​	要将磁盘镜像文件移动到不同的位置：

1. Select **Settings** then **Advanced** from the **Resources** tab. 在 Docker Dashboard 中选择 **Settings**，然后在 **Resources** 标签下选择 **Advanced**。
2. In the **Disk image location** section, select **Browse** and choose a new location for the disk image. 在 **Disk image location** 部分中，选择 **Browse**，然后选择磁盘镜像的新位置。
3. Select **Apply & Restart** for the changes to take effect. 选择 **Apply & Restart** 以使更改生效。

> **Important**
>
> Do not move the file directly in Finder as this can cause Docker Desktop to lose track of the file.
>
> ​	请勿直接在 Finder 中移动文件，因为这可能导致 Docker Desktop 无法找到文件。

##### 如何删除不必要的容器和镜像？ How do I delete unnecessary containers and images?

Check whether you have any unnecessary containers and images. If your client and daemon API are running version 1.25 or later (use the `docker version` command on the client to check your client and daemon API versions), you can see the detailed space usage information by running:

​	检查是否有不必要的容器和镜像。如果客户端和守护程序 API 版本为 1.25 或更高版本（使用客户端上的 `docker version` 命令检查客户端和守护程序的 API 版本），您可以通过运行以下命令查看详细的空间使用信息：



```console
$ docker system df -v
```

Alternatively, to list images, run:

​	或者，列出镜像：



```console
$ docker image ls
```

To list containers, run:

​	列出容器：



```console
$ docker container ls -a
```

If there are lots of redundant objects, run the command:

​	如果存在许多冗余对象，可以运行以下命令：



```console
$ docker system prune
```

This command removes all stopped containers, unused networks, dangling images, and build cache.

​	该命令将移除所有已停止的容器、未使用的网络、悬空的镜像和构建缓存。

It might take a few minutes to reclaim space on the host depending on the format of the disk image file. If the file is named:

​	根据磁盘镜像文件的格式，可能需要几分钟才能在主机上回收空间。如果文件名为：

- `Docker.raw`, space on the host is reclaimed within a few seconds.
  - `Docker.raw`，则主机上的空间应在几秒内回收。

- `Docker.qcow2`, space is freed by a background process after a few minutes.
  - `Docker.qcow2`，后台进程将在几分钟后释放空间。


Space is only freed when images are deleted. Space is not freed automatically when files are deleted inside running containers. To trigger a space reclamation at any point, run the command:

​	仅在删除镜像时才会释放空间。当删除运行中容器中的文件时，不会自动释放空间。要在任何时间点触发空间回收，可以运行以下命令：



```console
$ docker run --privileged --pid=host docker/desktop-reclaim-space
```

Note that many tools report the maximum file size, not the actual file size. To query the actual size of the file on the host from a terminal, run:

​	请注意，许多工具报告的是最大文件大小，而不是实际文件大小。要从终端查询主机上文件的实际大小，请运行以下命令：



```console
$ cd ~/Library/Containers/com.docker.docker/Data/vms/0/data
$ ls -klsh Docker.raw
2333548 -rw-r--r--@ 1 username  staff    64G Dec 13 17:42 Docker.raw
```

In this example, the actual size of the disk is `2333548` KB, whereas the maximum size of the disk is `64` GB.

​	在此示例中，磁盘的实际大小为 `2333548` KB，而磁盘的最大大小为 `64` GB。

##### 如何减少文件的最大大小？ How do I reduce the maximum size of the file?

To reduce the maximum size of the disk image file:

​	要减少磁盘镜像文件的最大大小：

1. Select **Settings** then **Advanced** from the **Resources** tab. 在 Docker Dashboard 中选择 **Settings**，然后在 **Resources** 标签下选择 **Advanced**。
2. The **Disk image size** section contains a slider that allows you to change the maximum size of the disk image. Adjust the slider to set a lower limit. **Disk image size** 部分包含一个滑块，可以让您更改磁盘镜像的最大大小。调整滑块以设置较低的限制。
3. Select **Apply & Restart**. 选择 **Apply & Restart**。

When you reduce the maximum size, the current disk image file is deleted, and therefore, all containers and images are lost.

​	当您减少最大大小时，当前的磁盘镜像文件将被删除，因此，所有容器和镜像将丢失。

### 如何添加 TLS 证书？ How do I add TLS certificates?

You can add trusted Certificate Authorities (CAs) (used to verify registry server certificates) and client certificates (used to authenticate to registries) to your Docker daemon.

​	您可以向 Docker 守护程序添加受信任的证书颁发机构 (CAs)（用于验证注册服务器证书）和客户端证书（用于认证到注册表）。

#### 添加自定义 CA 证书（服务器端） Add custom CA certificates (server side)

All trusted CAs (root or intermediate) are supported. Docker Desktop creates a certificate bundle of all user-trusted CAs based on the Mac Keychain, and appends it to Moby trusted certificates. So if an enterprise SSL certificate is trusted by the user on the host, it is trusted by Docker Desktop.

​	支持所有受信任的 CA（根或中间证书）。Docker Desktop 基于 Mac 钥匙串创建了所有用户受信任 CA 的证书包，并将其附加到 Moby 受信任的证书。因此，如果企业 SSL 证书在主机上被用户信任，它也会被 Docker Desktop 信任。

To manually add a custom, self-signed certificate, start by adding the certificate to the macOS keychain, which is picked up by Docker Desktop. Here is an example:

​	要手动添加自定义自签名证书，请首先将证书添加到 macOS 钥匙串，Docker Desktop 将会检测到。以下是一个示例：



```console
$ sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain ca.crt
```

Or, if you prefer to add the certificate to your own local keychain only (rather than for all users), run this command instead:

​	或者，如果您希望仅将证书添加到您自己的本地钥匙串（而不是所有用户），可以运行以下命令：



```console
$ security add-trusted-cert -d -r trustRoot -k ~/Library/Keychains/login.keychain ca.crt
```

See also, [Directory structures for certificates](https://docs.docker.com/desktop/faqs/macfaqs/#directory-structures-for-certificates).

​	另请参见，[证书的目录结构](https://docs.docker.com/desktop/faqs/macfaqs/#directory-structures-for-certificates)。

> **Note**
>
> 
>
> You need to restart Docker Desktop after making any changes to the keychain or to the `~/.docker/certs.d` directory in order for the changes to take effect.
>
> ​	您需要在更改钥匙串或 `~/.docker/certs.d` 目录后重启 Docker Desktop 以使更改生效。

For a complete explanation of how to do this, see the blog post [Adding Self-signed Registry Certs to Docker & Docker Desktop for Mac](https://blog.container-solutions.com/adding-self-signed-registry-certs-docker-mac).

​	有关详细的操作说明，请参阅博客文章 [为 Docker & Docker Desktop for Mac 添加自签名注册证书](https://blog.container-solutions.com/adding-self-signed-registry-certs-docker-mac)。

#### 添加客户端证书 Add client certificates

You can put your client certificates in `~/.docker/certs.d/<MyRegistry>:<Port>/client.cert` and `~/.docker/certs.d/<MyRegistry>:<Port>/client.key`.

​	您可以将客户端证书放在 `~/.docker/certs.d/<MyRegistry>:<Port>/client.cert` 和 `~/.docker/certs.d/<MyRegistry>:<Port>/client.key`。

When the Docker Desktop application starts, it copies the `~/.docker/certs.d` folder on your Mac to the `/etc/docker/certs.d` directory on Moby (the Docker Desktop `xhyve` virtual machine).

​	Docker Desktop 应用程序启动时，会将您 Mac 上的 `~/.docker/certs.d` 文件夹复制到 Moby（Docker Desktop 的 `xhyve` 虚拟机）上的 `/etc/docker/certs.d` 目录中。

> **Note**
>
> 
>
> - You need to restart Docker Desktop after making any changes to the keychain or to the `~/.docker/certs.d` directory in order for the changes to take effect.
>   - 您需要在更改钥匙串或 `~/.docker/certs.d` 目录后重启 Docker Desktop 以使更改生效。
> - The registry cannot be listed as an *insecure registry*. Docker Desktop ignores certificates listed under insecure registries, and does not send client certificates. Commands like `docker run` that attempt to pull from the registry produce error messages on the command line, as well as on the registry.
>   - 注册表不能被列为 *不安全注册表*。Docker Desktop 忽略不安全注册表下的证书，不发送客户端证书。尝试从该注册表拉取的命令（如 `docker run`）会在命令行和注册表中显示错误消息。

#### 证书的目录结构 Directory structures for certificates

If you have this directory structure, you do not need to manually add the CA certificate to your Mac OS system login:

​	如果您具有以下目录结构，则无需手动将 CA 证书添加到您的 macOS 系统登录中：



```text
/Users/<user>/.docker/certs.d/
└── <MyRegistry>:<Port>
   ├── ca.crt
   ├── client.cert
   └── client.key
```

The following further illustrates and explains a configuration with custom certificates:

​	以下示例进一步说明并解释了带有自定义证书的配置：



```text
/etc/docker/certs.d/        <-- Certificate directory
└── localhost:5000          <-- Hostname:port
   ├── client.cert          <-- Client certificate
   ├── client.key           <-- Client key
   └── ca.crt               <-- Certificate authority that signed
                                the registry certificate
```

You can also have this directory structure, as long as the CA certificate is also in your keychain.

​	您也可以有以下目录结构，只要 CA 证书也在您的钥匙串中：



```text
/Users/<user>/.docker/certs.d/
└── <MyRegistry>:<Port>
    ├── client.cert
    └── client.key
```

To learn more about how to install a CA root certificate for the registry and how to set the client TLS certificate for verification, see [Verify repository client with certificates]({{< ref "/manuals/DockerEngine/Security/Verifyrepositoryclientwithcertificates" >}}) in the Docker Engine topics.

​	有关如何为注册表安装 CA 根证书以及如何设置客户端 TLS 证书以进行验证的详细信息，请参见 Docker Engine 主题中的 [使用证书验证存储库客户端]({{< ref "/manuals/DockerEngine/Security/Verifyrepositoryclientwithcertificates" >}})。
