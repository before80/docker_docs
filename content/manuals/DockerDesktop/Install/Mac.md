+++
title = "Mac"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/desktop/install/mac-install/](https://docs.docker.com/desktop/install/mac-install/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install Docker Desktop on Mac - 在 Mac 上安装 Docker Desktop

> **Docker Desktop terms - Docker Desktop 条款**
>
> Commercial use of Docker Desktop in larger enterprises (more than 250 employees OR more than $10 million USD in annual revenue) requires a [paid subscription](https://www.docker.com/pricing/).
>
> ​	对于较大的企业（超过 250 名员工或年收入超过 1000 万美元），商用 Docker Desktop 需要 [付费订阅](https://www.docker.com/pricing/)。

This page contains download URLs, information about system requirements, and instructions on how to install Docker Desktop for Mac.

​	此页面包含下载 URL、系统要求信息以及在 Mac 上安装 Docker Desktop 的说明。



[Docker Desktop for Mac with Apple silicon](https://desktop.docker.com/mac/main/arm64/Docker.dmg?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-mac-arm64)

[Docker Desktop for Mac with Intel chip](https://desktop.docker.com/mac/main/amd64/Docker.dmg?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-mac-amd64)



*For checksums, see [Release notes]({{< ref "/manuals/DockerDesktop/Releasenotes" >}}).*

​	有关校验和，请参阅 [发行说明]({{< ref "/manuals/DockerDesktop/Releasenotes" >}})。

## 系统要求 System requirements

{{< tabpane text=true persist=disabled >}}

{{% tab header="Mac with Intel chip" %}}

- A supported version of macOS. 支持的 macOS 版本

  > **Important**
  >
  > 
  >
  > Docker supports Docker Desktop on the most recent versions of macOS. That is, the current release of macOS and the previous two releases. As new major versions of macOS are made generally available, Docker stops supporting the oldest version and supports the newest version of macOS (in addition to the previous two releases).
  >
  > ​	Docker 支持 macOS 的最新版本，即当前发布的 macOS 版本和前两个版本。当新的 macOS 主版本可用时，Docker 将停止支持最旧的版本并支持 macOS 的最新版本（以及之前的两个版本）。

- At least 4 GB of RAM. 至少 4 GB 内存

{{% /tab  %}}

{{% tab header="Mac with Apple silicon" %}}

- A supported version of macOS. 支持的 macOS 版本

  > **Important**
  >
  > 
  >
  > Docker supports Docker Desktop on the most recent versions of macOS. That is, the current release of macOS and the previous two releases. As new major versions of macOS are made generally available, Docker stops supporting the oldest version and supports the newest version of macOS (in addition to the previous two releases).
  >
  > ​	Docker 支持 macOS 的最新版本，即当前发布的 macOS 版本和前两个版本。当新的 macOS 主版本可用时，Docker 将停止支持最旧的版本并支持 macOS 的最新版本（以及之前的两个版本）。

- At least 4 GB of RAM. 至少 4 GB 内存

- For the best experience, it's recommended that you install Rosetta 2. There is no longer a hard requirement to install Rosetta 2, however there are a few optional command line tools that still require Rosetta 2 when using Darwin/AMD64. See [Known issues]({{< ref "/manuals/DockerDesktop/Troubleshootanddiagnose/Knownissues" >}}). To install Rosetta 2 manually from the command line, run the following command:

  为获得最佳体验，建议安装 Rosetta 2。现在不再需要强制安装 Rosetta 2，不过在使用 Darwin/AMD64 时，某些可选的命令行工具仍然需要 Rosetta 2。请参阅 [已知问题]({{< ref "/manuals/DockerDesktop/Troubleshootanddiagnose/Knownissues" >}})。要从命令行手动安装 Rosetta 2，请运行以下命令：

  ```console
  $ softwareupdate --install-rosetta
  ```

{{% /tab  %}}

{{< /tabpane >}}



------

## 在 Mac 上安装和运行 Docker Desktop - Install and run Docker Desktop on Mac

> **Tip**
>
> 
>
> See the [FAQs](https://docs.docker.com/desktop/faqs/general/#how-do-I-run-docker-desktop-without-administrator-privileges) on how to install and run Docker Desktop without needing administrator privileges.
>
> ​	请参阅 [常见问题解答](https://docs.docker.com/desktop/faqs/general/#how-do-I-run-docker-desktop-without-administrator-privileges) 了解如何在不需要管理员权限的情况下安装和运行 Docker Desktop。

### 交互式安装 Install interactively

1. Download the installer using the download buttons at the top of the page, or from the [release notes]({{< ref "/manuals/DockerDesktop/Releasenotes" >}}). 使用页面顶部的下载按钮或从 [发行说明]({{< ref "/manuals/DockerDesktop/Releasenotes" >}}) 下载安装程序。

2. Double-click `Docker.dmg` to open the installer, then drag the Docker icon to the **Applications** folder. By default, Docker Desktop is installed at `/Applications/Docker.app`. 双击 `Docker.dmg` 以打开安装程序，然后将 Docker 图标拖到 **Applications** 文件夹中。默认情况下，Docker Desktop 安装在 `/Applications/Docker.app`。

3. Double-click `Docker.app` in the **Applications** folder to start Docker. 双击 **Applications** 文件夹中的 `Docker.app` 以启动 Docker。

4. The Docker menu displays the Docker Subscription Service Agreement. Docker 菜单会显示 Docker 订阅服务协议。

   Here’s a summary of the key points: 这里是一些关键点：

   - Docker Desktop is free for small businesses (fewer than 250 employees AND less than $10 million in annual revenue), personal use, education, and non-commercial open source projects. 
     - Docker Desktop 对于小企业（员工少于 250 名且年收入少于 1000 万美元）、个人使用、教育和非商业开源项目是免费的。
   - Otherwise, it requires a paid subscription for professional use.
     - 否则，对于专业用途需要付费订阅。
   - Paid subscriptions are also required for government entities.
     - 政府机构也需要付费订阅。
   - Docker Pro, Team, and Business subscriptions include commercial use of Docker Desktop.
     - Docker Pro、Team 和 Business 订阅包括 Docker Desktop 的商用权利。

5. Select **Accept** to continue. 选择 **接受** 以继续。

   Note that Docker Desktop won't run if you do not agree to the terms. You can choose to accept the terms at a later date by opening Docker Desktop.

   ​	请注意，如果不同意条款，Docker Desktop 将无法运行。您可以稍后在 Docker Desktop 中选择接受条款。

   For more information, see [Docker Desktop Subscription Service Agreement](https://www.docker.com/legal/docker-subscription-service-agreement). It is recommended that you also read the [FAQs](https://www.docker.com/pricing/faq).

   ​	有关更多信息，请参阅 [Docker Desktop 订阅服务协议](https://www.docker.com/legal/docker-subscription-service-agreement)。建议您还阅读 [常见问题解答](https://www.docker.com/pricing/faq)。

6. From the installation window, select either: 在安装窗口中，选择以下任一选项：

   - **Use recommended settings (Requires password)**. This lets Docker Desktop automatically set the necessary configuration settings.
     - **使用推荐设置（需要密码）**。这让 Docker Desktop 自动设置必要的配置。
   - **Use advanced settings**. You can then set the location of the Docker CLI tools either in the system or user directory, enable the default Docker socket, and enable privileged port mapping. See [Settings](https://docs.docker.com/desktop/settings/#advanced), for more information and how to set the location of the Docker CLI tools.
     - **使用高级设置**。您可以选择将 Docker CLI 工具放置在系统或用户目录中，启用默认 Docker 套接字，以及启用特权端口映射。有关详细信息，请参阅 [设置](https://docs.docker.com/desktop/settings/#advanced)，了解如何设置 Docker CLI 工具的位置。

7. Select **Finish**. If you have applied any of the previous configurations that require a password in step 6, enter your password to confirm your choice. 选择 **完成**。如果在步骤 6 中应用了需要密码的配置，请输入密码以确认选择。

### 从命令行安装 Install from the command line

After downloading `Docker.dmg` from either the download buttons at the top of the page or from the [release notes]({{< ref "/manuals/DockerDesktop/Releasenotes" >}}), run the following commands in a terminal to install Docker Desktop in the **Applications** folder:

​	在页面顶部的下载按钮或从 [发行说明]({{< ref "/manuals/DockerDesktop/Releasenotes" >}}) 下载 `Docker.dmg` 后，在终端中运行以下命令，将 Docker Desktop 安装到 **Applications** 文件夹中：



```console
$ sudo hdiutil attach Docker.dmg
$ sudo /Volumes/Docker/Docker.app/Contents/MacOS/install
$ sudo hdiutil detach /Volumes/Docker
```

By default, Docker Desktop is installed at `/Applications/Docker.app`. As macOS typically performs security checks the first time an application is used, the `install` command can take several minutes to run.

​	默认情况下，Docker Desktop 安装在 `/Applications/Docker.app`。由于 macOS 通常会在应用程序首次使用时执行安全检查，因此 `install` 命令可能需要几分钟才能完成。

The `install` command accepts the following flags:

​	`install` 命令接受以下标志：

- `--accept-license`: Accepts the [Docker Subscription Service Agreement](https://www.docker.com/legal/docker-subscription-service-agreement) now, rather than requiring it to be accepted when the application is first run.

  - `--accept-license`：立即接受 [Docker 订阅服务协议](https://www.docker.com/legal/docker-subscription-service-agreement)，而无需在首次运行时接受。

- `--allowed-org=<org name>`: Requires the user to sign in and be part of the specified Docker Hub organization when running the application

  - `--allowed-org=<org name>`：要求用户登录并成为指定的 Docker Hub 组织成员后才能运行该应用程序。

- `--user=<username>`: Performs the privileged configurations once during installation. This removes the need for the user to grant root privileges on first run. For more information, see [Privileged helper permission requirements](https://docs.docker.com/desktop/install/mac-permission-requirements/#permission-requirements). To find the username, enter `ls /Users` in the CLI.

  - `--user=<username>`：在安装过程中执行特权配置。这使用户在首次运行时无需授予 root 权限。有关用户名的查找方法，请在 CLI 中输入 `ls /Users`。

- `--admin-settings`: Automatically creates an `admin-settings.json` file which is used by administrators to control certain Docker Desktop settings on client machines within their organization. For more information, see [Settings Management](https://docs.docker.com/security/for-admins/hardened-desktop/settings-management/). `--admin-settings`：自动创建一个 `admin-settings.json` 文件，用于管理员在组织中的客户端设备上控制 Docker Desktop 的某些设置。更多信息请参见 [设置管理](https://docs.docker.com/security/for-admins/hardened-desktop/settings-management/)。

  - It must be used together with the `--allowed-org=<org name>` flag.

    - 必须与 `--allowed-org=<org name>` 标志一起使用。

  - For example:  例如：

    ```console
    --allowed-org=<org name> --admin-settings="{'configurationFileVersion': 2, 'enhancedContainerIsolation': {'value': true, 'locked': false}}"
    ```

    

- `--proxy-http-mode=<mode>`: Sets the HTTP Proxy mode. The two modes are `system` (default) or `manual`.

  - `--proxy-http-mode=<mode>`：设置 HTTP 代理模式。可选的两种模式为 `system`（默认）或 `manual`。

- `--override-proxy-http=<URL>`: Sets the URL of the HTTP proxy that must be used for outgoing HTTP requests. It requires `--proxy-http-mode` to be `manual`.

  - `--override-proxy-http=<URL>`：设置必须用于传出 HTTP 请求的 HTTP 代理 URL。此选项需要 `--proxy-http-mode` 设置为 `manual`。

- `--override-proxy-https=<URL>`: Sets the URL of the HTTP proxy that must be used for outgoing HTTPS requests, requires `--proxy-http-mode` to be `manual`

  - `--override-proxy-https=<URL>`：设置必须用于传出 HTTPS 请求的 HTTPS 代理 URL，此选项需要 `--proxy-http-mode` 设置为 `manual`。

- `--override-proxy-exclude=<hosts/domains>`: Bypasses proxy settings for the hosts and domains. It's a comma-separated list.

  - `--override-proxy-exclude=<hosts/domains>`：为指定的主机和域绕过代理设置，使用逗号分隔的列表。


> **Tip**
>
> 
>
> As an IT administrator, you can use endpoint management (MDM) software to identify the number of Docker Desktop instances and their versions within your environment. This can provide accurate license reporting, help ensure your machines use the latest version of Docker Desktop, and enable you to [enforce sign-in]({{< ref "/manuals/Security/Foradmins/Enforcesign-in" >}}).
>
> ​	作为 IT 管理员，您可以使用终端管理（MDM）软件来识别环境中的 Docker Desktop 实例数量及其版本。这可以提供准确的许可证报告，帮助确保您的机器使用 Docker Desktop 的最新版本，并允许您 [强制登录]({{< ref "/manuals/Security/Foradmins/Enforcesign-in" >}})。
>
> - [Intune](https://learn.microsoft.com/en-us/mem/intune/apps/app-discovered-apps)
> - [Jamf](https://docs.jamf.com/10.25.0/jamf-pro/administrator-guide/Application_Usage.html)
> - [Kandji](https://support.kandji.io/support/solutions/articles/72000559793-view-a-device-application-list)
> - [Kolide](https://www.kolide.com/features/device-inventory/properties/mac-apps)
> - [Workspace One](https://blogs.vmware.com/euc/2022/11/how-to-use-workspace-one-intelligence-to-manage-app-licenses-and-reduce-costs.html)

## Where to go next

- Explore [Docker's core subscriptions](https://www.docker.com/pricing/) to see what Docker can offer you.
  - 探索 [Docker 的核心订阅](https://www.docker.com/pricing/) 以查看 Docker 能为您提供的服务。
- [Get started with Docker]({{< ref "/get-started/Introduction" >}}).
  - [开始使用 Docker]({{< ref "/get-started/Introduction" >}})。
- [Explore Docker Desktop]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop" >}}) and all its features.
  - [探索 Docker Desktop]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop" >}}) 及其所有功能。
- [Troubleshooting]({{< ref "/manuals/DockerDesktop/Troubleshootanddiagnose" >}}) describes common problems, workarounds, how to run and submit diagnostics, and submit issues.
  - [故障排除]({{< ref "/manuals/DockerDesktop/Troubleshootanddiagnose" >}}) 描述了常见问题、解决方法、如何运行和提交诊断以及提交问题。
- [FAQs]({{< ref "/manuals/DockerDesktop/FAQs/General" >}}) provide answers to frequently asked questions.
  - [常见问题解答]({{< ref "/manuals/DockerDesktop/FAQs/General" >}}) 提供常见问题的答案。
- [Release notes]({{< ref "/manuals/DockerDesktop/Releasenotes" >}}) lists component updates, new features, and improvements associated with Docker Desktop releases.
  - [发行说明]({{< ref "/manuals/DockerDesktop/Releasenotes" >}}) 列出了 Docker Desktop 版本相关的组件更新、新功能和改进。
- [Back up and restore data]({{< ref "/manuals/DockerDesktop/HowtobackupandrestoreyourDockerDesktopdata" >}}) provides instructions on backing up and restoring data related to Docker.
  - [备份和恢复数据]({{< ref "/manuals/DockerDesktop/HowtobackupandrestoreyourDockerDesktopdata" >}}) 提供有关备份和恢复与 Docker 相关数据的说明。
