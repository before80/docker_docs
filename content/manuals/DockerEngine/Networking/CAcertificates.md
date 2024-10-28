+++
title = "CA certificates"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/engine/network/ca-certs/](https://docs.docker.com/engine/network/ca-certs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use CA certificates with Docker

> **Caution 警告**
>
> Best practices should be followed when using Man-in-the-Middle (MITM) CA certificates in production containers. If compromised, attackers could intercept sensitive data, spoof a trusted service, or perform man-in-the-middle attacks. Consult your security team before you proceed.
>
> ​	在生产容器中使用中间人 (MITM) CA 证书时应遵循最佳实践。若被攻破，攻击者可能会拦截敏感数据、伪造受信任的服务或执行中间人攻击。在继续之前，请咨询您的安全团队。

If your company uses a proxy that inspects HTTPS traffic, you might need to add the required root certificates to your host machine and your Docker containers or images. This is because Docker and its containers, when pulling images or making network requests, need to trust the proxy’s certificates.

​	如果您的公司使用检查 HTTPS 流量的代理，则可能需要将所需的根证书添加到主机和 Docker 容器或镜像中。这是因为 Docker 和它的容器在拉取镜像或进行网络请求时，需要信任代理的证书。

On the host, adding the root certificate ensures that any Docker commands (like `docker pull`) work without issues. For containers, you'll need to add the root certificate to the container's trust store either during the build process or at runtime. This ensures that applications running inside the containers can communicate through the proxy without encountering security warnings or connection failures.

​	在主机上添加根证书可以确保任何 Docker 命令（如 `docker pull`）都可以正常工作。对于容器，需要在构建过程中或运行时将根证书添加到容器的信任存储区中，从而确保容器内运行的应用程序可以通过代理进行通信，而不会遇到安全警告或连接失败。

## 将 CA 证书添加到主机 Add CA certificate to the host

The following sections describe how to install CA certificates on your macOS or Windows host. For Linux, refer to the documentation for your distribution.

​	以下部分描述如何在 macOS 或 Windows 主机上安装 CA 证书。对于 Linux，请参考您所用发行版的文档。

### macOS

1. Download the CA certificate for your MITM proxy software. 下载用于 MITM 代理软件的 CA 证书。
2. Open the **Keychain Access** app. 打开 **钥匙串访问** 应用。
3. In Keychain Access, select **System**, then switch to the **Certificates** tab. 在钥匙串访问中，选择 **系统**，然后切换到 **证书** 选项卡。
4. Drag-and-drop the downloaded certificate into the list of certificates. Enter your password if prompted. 将下载的证书拖放到证书列表中。如有提示，请输入密码。
5. Find the newly added certificate, double-click it, and expand the **Trust** section. 找到新添加的证书，双击它并展开 **信任** 部分。
6. Set **Always Trust** for the certificate. Enter your password if prompted. 设置证书的 **始终信任**。如有提示，请输入密码。
7. Start Docker Desktop and verify that `docker pull` works, assuming Docker Desktop is configured to use the MITM proxy. 启动 Docker Desktop 并验证 `docker pull` 是否正常工作（假设 Docker Desktop 配置为使用 MITM 代理）。

### Windows

Choose whether you want to install the certificate using the Microsoft Management Console (MMC) or your web browser.

​	选择是使用 Microsoft 管理控制台 (MMC) 还是 Web 浏览器安装证书。

{{< tabpane text=true persist=disabled >}}

{{% tab header="MMC" %}}

1. Download CA certificate for the MITM proxy software. 下载用于 MITM 代理软件的 CA 证书。
2. Open the Microsoft Management Console (`mmc.exe`). 打开 Microsoft 管理控制台 (`mmc.exe`)。
3. Add the **Certificates Snap-In** in the MMC. 在 MMC 中添加 **证书管理单元**：
   1. Select **File** → **Add/Remove Snap-in**, and then select **Certificates** → **Add >**. 选择 **文件** → **添加/删除管理单元**，然后选择 **证书** → **添加 >**。
   2. Select **Computer Account** and then **Next**. 选择 **计算机账户**，然后点击 **下一步**。
   3. Select **Local computer** and then select **Finish**. 选择 **本地计算机**，然后点击 **完成**。
4. Import the CA certificate: 导入 CA 证书：
   1. From the MMC, expand **Certificates (Local Computer)**. 在 MMC 中，展开 **证书（本地计算机）**。
   2. Expand the **Trusted Root Certification Authorities** section. 展开 **受信任的根证书颁发机构** 部分。
   3. Right-click **Certificates** and select **All Tasks** and **Import…**. 右键点击 **证书**，选择 **所有任务** 和 **导入…**。
   4. Follow the prompts to import your CA certificate. 按照提示导入您的 CA 证书。
5. Select **Finish** and then **Close**. 选择 **完成**，然后点击 **关闭**。
6. Start Docker Desktop and verify that `docker pull` succeeds (assuming Docker Desktop is already configured to use the MITM proxy server). 启动 Docker Desktop 并验证 `docker pull` 是否成功（假设 Docker Desktop 已配置为使用 MITM 代理服务器）。

> **Note**
>
> Depending on the SDK and/or runtime/framework in use, further steps may be required beyond adding the CA certificate to the operating system's trust store.
>
> ​	根据使用的 SDK 和/或运行时/框架，可能需要执行其他步骤，除了将 CA 证书添加到操作系统的信任存储区之外。

{{% /tab  %}}

{{% tab header="Web browser" %}}

1. Download the CA certificate for your MITM proxy software. 下载用于 MITM 代理软件的 CA 证书。
2. Open your web browser, go to **Settings** and open **Manage certificates **打开您的网络浏览器，进入 **设置** 并打开 **管理证书**。
3. Select the **Trusted Root Certification Authorities** tab. 选择 **受信任的根证书颁发机构** 选项卡。
4. Select **Import**, then browse for the downloaded CA certificate. 选择 **导入**，然后浏览到下载的 CA 证书。
5. Select **Open**, then choose **Place all certificates in the following store**. 选择 **打开**，然后选择 **将所有证书放入以下存储**。
6. Ensure **Trusted Root Certification Authorities** is selected and select **Next**. 确保选择了 **受信任的根证书颁发机构**，然后点击 **下一步**。
7. Select **Finish** and then **Close**. 选择 **完成**，然后点击 **关闭**。
8. Start Docker Desktop and verify that `docker pull` succeeds (assuming Docker Desktop is already configured to use the MITM proxy server). 启动 Docker Desktop 并验证 `docker pull` 是否成功（假设 Docker Desktop 已配置为使用 MITM 代理服务器）。

{{% /tab  %}}

{{< /tabpane >}}

## 将 CA 证书添加到 Linux 镜像和容器 Add CA certificates to Linux images and containers

If you need to run containerized workloads that rely on internal or custom certificates, such as in environments with corporate proxies or secure services, you must ensure that the containers trust these certificates. Without adding the necessary CA certificates, applications inside your containers may encounter failed requests or security warnings when attempting to connect to HTTPS endpoints.

​	如果您需要运行依赖内部或自定义证书的容器化工作负载（如公司代理或安全服务），则必须确保容器信任这些证书。否则，容器内的应用程序在尝试连接到 HTTPS 端点时可能会遇到请求失败或安全警告。

By [adding CA certificates to images](https://docs.docker.com/engine/network/ca-certs/#add-certificates-to-images) at build time, you ensure that any containers started from the image will trust the specified certificates. This is particularly important for applications that require seamless access to internal APIs, databases, or other services during production.

​	通过在构建时[将 CA 证书添加到镜像](https://docs.docker.com/engine/network/ca-certs/#add-certificates-to-images)，可以确保从该镜像启动的容器信任指定的证书。这对于需要无缝访问内部 API、数据库或其他生产服务的应用程序尤为重要。

In cases where rebuilding the image isn't feasible, you can instead [add certificates to containers](https://docs.docker.com/engine/network/ca-certs/#add-certificates-to-containers) directly. However, certificates added at runtime won’t persist if the container is destroyed or recreated, so this method is typically used for temporary fixes or testing scenarios.

​	如果无法重新构建镜像，可以直接[将证书添加到容器](https://docs.docker.com/engine/network/ca-certs/#add-certificates-to-containers)。不过，运行时添加的证书不会在容器销毁或重新创建后保留，因此此方法通常用于临时修复或测试场景。

## 向镜像添加证书 Add certificates to images

> **Note**
>
> The following commands are for an Ubuntu base image. If your build uses a different Linux distribution, use equivalent commands for package management (`apt-get`, `update-ca-certificates`, and so on).
>
> ​	以下命令适用于基于 Ubuntu 的镜像。如果您的构建使用不同的 Linux 发行版，请使用等效的包管理命令（如 `apt-get`、`update-ca-certificates` 等）。

To add ca certificate to a container image when you're building it, add the following instructions to your Dockerfile.

​	在构建容器镜像时添加 CA 证书，请将以下指令添加到 Dockerfile 中。

```dockerfile
# Install the ca-certificate package
	# 安装 ca-certificate 包
RUN apt-get update && apt-get install -y ca-certificates
# Copy the CA certificate from the context to the build container
	# 将 CA 证书从上下文复制到构建容器
COPY your_certificate.crt /usr/local/share/ca-certificates/
# Update the CA certificates in the container
	# 更新容器中的 CA 证书
RUN update-ca-certificates
```

### 向容器添加证书 Add certificates to containers

> **Note**
>
> The following commands are for an Ubuntu-based container. If your container uses a different Linux distribution, use equivalent commands for package management (`apt-get`, `update-ca-certificates`, and so on).
>
> ​	以下命令适用于基于 Ubuntu 的容器。如果您的容器使用不同的 Linux 发行版，请使用等效的包管理命令（如 `apt-get`、`update-ca-certificates` 等）。

To add a CA certificate to a running Linux container:

​	向运行中的 Linux 容器添加 CA 证书：

1. Download the CA certificate for your MITM proxy software. 下载用于 MITM 代理软件的 CA 证书。

2. If the certificate is in a format other than `.crt`, convert it to `.crt` format: 如果证书格式不是 `.crt`，请将其转换为 `.crt` 格式：

   Example command

   示例命令：

   ```console
   $ openssl x509 -in cacert.der -inform DER -out myca.crt
   ```

3. Copy the certificate into the running container:

   将证书复制到运行中的容器中：

   ```console
   $ docker cp myca.crt <containerid>:/tmp
   ```

4. Attach to the container:

   连接到容器：

   ```console
   $ docker exec -it <containerid> sh
   ```

5. Ensure the `ca-certificates` package is installed (required for updating certificates):

   确保已安装 `ca-certificates` 包（用于更新证书）：

   ```console
   # apt-get update && apt-get install -y ca-certificates
   ```

6. Copy the certificate to the correct location for CA certificates:

   将证书复制到 CA 证书的正确位置：

   ```console
   # cp /tmp/myca.crt /usr/local/share/ca-certificates/root_cert.crt
   ```

7. Update the CA certificates:

   更新 CA 证书：

   ```console
   # update-ca-certificates
   ```

   Example output

   示例输出：

   ```plaintext
   Updating certificates in /etc/ssl/certs...
   rehash: warning: skipping ca-certificates.crt, it does not contain exactly one certificate or CRL
   1 added, 0 removed; done.
   ```

8. Verify that the container can communicate via the MITM proxy:

   验证容器可以通过 MITM 代理进行通信：

   ```console
   # curl https://example.com
   ```

   Example output

   示例输出：

   ```plaintext
   <!doctype html>
   <html>
   <head>
       <title>Example Domain</title>
   ...
   ```

