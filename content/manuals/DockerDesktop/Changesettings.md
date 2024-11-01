+++
title = "更改设置"
date = 2024-10-23T14:54:40+08:00
weight = 140
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/settings/](https://docs.docker.com/desktop/settings/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Change your Docker Desktop settings - 更改 Docker Desktop 设置

To navigate to **Settings** either:

​	要导航到 **Settings** 设置页面，您可以：

- Select the Docker menu ![whale menu](Changesettings_img/whale-x.svg) and then **Settings**
  - 从 Docker 菜单中![whale menu](Changesettings_img/whale-x.svg)选择 **Settings** 设置

- Select the **Settings** icon from the Docker Dashboard.
  - 从 Docker Dashboard 中选择 **Settings** 图标


You can also locate the `settings.json` file at:

​	您还可以找到 `settings.json` 文件的位置：

- Mac: `~/Library/Group Containers/group.com.docker/settings.json`
- Windows: `C:\Users\[USERNAME]\AppData\Roaming\Docker\settings.json`
- Linux: `~/.docker/desktop/settings.json`

## 常规 General

On the **General** tab, you can configure when to start Docker and specify other settings:

​	在 **General** 选项卡上，您可以配置 Docker 启动时机和其他设置：

- **Start Docker Desktop when you sign in to your computer**. Select to automatically start Docker Desktop when you sign in to your machine.
  - **在登录计算机时启动 Docker Desktop**。选择此项可在您登录时自动启动 Docker Desktop。

- **Open Docker Dashboard when Docker Desktop starts**. Select to automatically open the dashboard when starting Docker Desktop.

  - **Docker Desktop 启动时打开 Docker Dashboard**。选择此项可在 Docker Desktop 启动时自动打开仪表板。

- **Choose theme for Docker Desktop**. Choose whether you want to apply a **Light** or **Dark** theme to Docker Desktop. Alternatively you can set Docker Desktop to **Use system settings**.

  - **选择 Docker Desktop 主题**。可以选择 **Light** 或 **Dark** 主题，或根据系统设置。

- **Choose container terminal**. Determines which terminal is launched when opening the terminal from a container. If you choose the integrated terminal, you can run commands in a running container straight from the Docker Dashboard. For more information, see [Explore containers]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/Containers" >}}).

  - **选择容器终端**。决定从 Docker Dashboard 打开终端时启动哪个终端。如果选择集成终端，可以直接在 Docker Dashboard 中在运行的容器中执行命令。详情参见 [Explore containers]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/Containers" >}})。

- **Enable Docker terminal**. Interact with your host machine and execute commands directly from Docker Desktop.

  - **启用 Docker 终端**。从 Docker Desktop 直接与主机机器交互并执行命令。

- **Enable Docker Debug by default**. Check this option to use Docker Debug by default when accessing the integrated terminal. For more information, see [Explore containers](https://docs.docker.com/desktop/use-desktop/container/#integrated-terminal).

  - **默认启用 Docker Debug**。选择此项在访问集成终端时默认使用 Docker Debug。更多信息请参见 [Explore containers](https://docs.docker.com/desktop/use-desktop/container/#integrated-terminal)。

- Mac only **Include VM in Time Machine backups**. Select to back up the Docker Desktop virtual machine. This option is turned off by default.

  - 仅限 Mac **将 VM 包含在 Time Machine 备份中**。选择此项可备份 Docker Desktop 虚拟机。默认关闭。

- Windows only **Expose daemon on tcp://localhost:2375 without TLS**. Check this option to enable legacy clients to connect to the Docker daemon. You must use this option with caution as exposing the daemon without TLS can result in remote code execution attacks.

  - 仅限 Windows **在 tcp://localhost:2375 上暴露守护进程而不使用 TLS**。启用该选项后可使旧客户端连接 Docker 守护进程。请谨慎使用，因为不启用 TLS 可能导致远程代码执行攻击。

- Windows only **Use the WSL 2 based engine**. WSL 2 provides better performance than the Hyper-V backend. For more information, see [Docker Desktop WSL 2 backend]({{< ref "/manuals/DockerDesktop/WSL" >}}).

  - 仅限 Windows **使用基于 WSL 2 的引擎**。WSL 2 比 Hyper-V 后端性能更佳。详情参见 [Docker Desktop WSL 2 后端]({{< ref "/manuals/DockerDesktop/WSL" >}})。

- Windows only **Add the `\*.docker.internal` names to the host's `/etc/hosts` file (Password required)**. Lets you resolve `*.docker.internal` DNS names from both the host and your containers.

  - 仅限 Windows **将 `\*.docker.internal` 名称添加到主机的 `/etc/hosts` 文件中（需要密码）**。使您可以从主机和容器中解析 `*.docker.internal` DNS 名称。

- **Use containerd for pulling and storing images**. Turns on the containerd image store. This brings new features like faster container startup performance by lazy-pulling images, and the ability to run Wasm applications with Docker. For more information, see [containerd image store]({{< ref "/manuals/DockerDesktop/containerdimagestore" >}}).

  - **使用 containerd 拉取和存储镜像**。启用 containerd 镜像存储，提供诸如通过延迟拉取镜像提升容器启动速度和使用 Docker 运行 Wasm 应用的新功能。详情参见 [containerd 镜像存储]({{< ref "/manuals/DockerDesktop/containerdimagestore" >}})。

- Mac only **Use Virtualization framework**. Select to allow Docker Desktop to use the `virtualization.framework` instead of the `hypervisor.framework`.

  - 仅限 Mac **使用虚拟化框架**。允许 Docker Desktop 使用 `virtualization.framework` 而非 `hypervisor.framework`。


  > **Tip**
  >
  > 
  >
  > Turn this setting on to make Docker Desktop run faster.
  >
  > **提示** 打开此设置可使 Docker Desktop 运行更快。

- Mac only **Choose file sharing implementation for your containers**. Choose whether you want to share files using **VirtioFS**, **gRPC FUSE**, or **osxfs (Legacy)**. VirtioFS is only available for macOS versions 12.5 and above, and is turned on by default.

  - 仅限 Mac **为您的容器选择文件共享实现**。选择使用 **VirtioFS**、**gRPC FUSE** 或 **osxfs（旧版）** 来共享文件。VirtioFS 仅适用于 macOS 版本 12.5 及以上，默认开启。


  > **Tip**
  >
  > Use VirtioFS for speedy file sharing. VirtioFS has reduced the time taken to complete filesystem operations by [up to 98%](https://github.com/docker/roadmap/issues/7#issuecomment-1044452206)
  >
  > 使用 VirtioFS 可加快文件共享。VirtioFS 将文件系统操作时间减少了[最多 98%](https://github.com/docker/roadmap/issues/7#issuecomment-1044452206)。

- Mac only **Use Rosetta for x86_64/amd64 emulation on Apple Silicon**. Turns on Rosetta to accelerate x86/AMD64 binary emulation on Apple Silicon. This option is only available if you have turned on **Virtualization framework** in the **General** settings tab. You must also be on macOS 13 or later.

  - 仅限 Mac **在 Apple Silicon 上使用 Rosetta 进行 x86_64/amd64 仿真**。启用此选项在 Apple Silicon 上加速 x86/AMD64 二进制仿真。仅在 General 设置标签中开启 **虚拟化框架** 时可用，且需要 macOS 13 或更高版本。

- **Send usage statistics**. Select so Docker Desktop sends diagnostics, crash reports, and usage data. This information helps Docker improve and troubleshoot the application. Clear the checkbox to opt out. Docker may periodically prompt you for more information.

  - **发送使用统计数据**。启用此项可使 Docker Desktop 发送诊断、崩溃报告和使用数据，以帮助 Docker 改善和排除应用问题。取消勾选即可选择退出。Docker 可能会定期请求您提供更多信息。

- **Use Enhanced Container Isolation**. Select to enhance security by preventing containers from breaching the Linux VM. For more information, see [Enhanced Container Isolation]({{< ref "/manuals/Security/Foradmins/HardenedDockerDesktop/EnhancedContainerIsolation" >}}).

  - **使用增强的容器隔离**。选择此项可通过防止容器越过 Linux VM 增强安全性。详情参见 [增强的容器隔离]({{< ref "/manuals/Security/Foradmins/HardenedDockerDesktop/EnhancedContainerIsolation" >}})。


  > **Note**
  >
  > 
  >
  > This setting is only available if you are signed in to Docker Desktop and have a Docker Business subscription.
  >
  > 此设置仅在您登录 Docker Desktop 并拥有 Docker Business 订阅时可用。

- **Show CLI hints**. Displays CLI hints and tips when running Docker commands in the CLI. This is turned on by default. To turn CLI hints on or off from the CLI, set `DOCKER_CLI_HINTS` to `true` or `false` respectively.

  - **显示 CLI 提示**。默认情况下，当在 CLI 中运行 Docker 命令时显示 CLI 提示和提示信息。要从 CLI 打开或关闭 CLI 提示，可将 `DOCKER_CLI_HINTS` 设置为 `true` 或 `false`。

- **SBOM Indexing**. When this option is enabled, inspecting an image in Docker Desktop shows a **Start analysis** button that, when selected, analyzes the image with Docker Scout.

  - **SBOM 索引**。启用此选项后，Docker Desktop 中的镜像检查将显示 **开始分析** 按钮，单击此按钮可使用 Docker Scout 分析镜像。

- **Enable background SBOM indexing**. When this option is enabled, Docker Scout automatically analyzes images that you build or pull.

  - **启用后台 SBOM 索引**。启用后，Docker Scout 将自动分析您构建或拉取的镜像。

- Mac only **Automatically check configuration**. Regularly checks your configuration to ensure no unexpected changes have been made by another application. 仅限 Mac **自动检查配置**。定期检查配置以确保未被其他应用意外更改。

  Docker Desktop checks if your setup, configured during installation, has been altered by external apps like Orbstack. Docker Desktop checks:

  ​	Docker Desktop 检查安装期间配置是否被外部应用（如 Orbstack）更改。Docker Desktop 检查以下内容：

  - The symlinks of Docker binaries to `/usr/local/bin`. Docker 二进制文件的符号链接指向 `/usr/local/bin`。
  - The symlink of the default Docker socket. Additionally, Docker Desktop ensures that the context is switched to `desktop-linux` on startup. 默认 Docker 套接字的符号链接。此外，Docker Desktop 确保启动时上下文切换到 `desktop-linux`。

  You are notified if changes are found and are able to restore the configuration directly from the notification. For more information, see the [FAQs](https://docs.docker.com/desktop/faqs/macfaqs/#why-do-i-keep-getting-a-notification-telling-me-an-application-has-changed-my-desktop-configurations).

  ​	如有更改，将会收到通知，并可直接从通知恢复配置。详情请参见 [FAQs](https://docs.docker.com/desktop/faqs/macfaqs/#why-do-i-keep-getting-a-notification-telling-me-an-application-has-changed-my-desktop-configurations)。

## 资源 Resources

The **Resources** tab allows you to configure CPU, memory, disk, proxies, network, and other resources.

​	在 **Resources** 选项卡中，您可以配置 CPU、内存、磁盘、代理、网络和其他资源。

### Advanced

> **Note**
>
> 
>
> On Windows, the **Resource allocation** options in the **Advanced** tab are only available in Hyper-V mode, because Windows manages the resources in WSL 2 mode and Windows container mode. In WSL 2 mode, you can configure limits on the memory, CPU, and swap size allocated to the [WSL 2 utility VM](https://docs.microsoft.com/en-us/windows/wsl/wsl-config#configure-global-options-with-wslconfig).
>
> ​	在 Windows 上，**高级** 选项卡中的 **资源分配** 选项仅在 Hyper-V 模式下可用，因为 Windows 在 WSL 2 模式和 Windows 容器模式下管理资源。在 WSL 2 模式下，可以为 [WSL 2 工具虚拟机](https://docs.microsoft.com/en-us/windows/wsl/wsl-config#configure-global-options-with-wslconfig)配置内存、CPU 和交换大小的限制。

On the **Advanced** tab, you can limit resources available to the Docker Linux VM.

​	在 **高级** 选项卡中，您可以限制 Docker Linux 虚拟机的可用资源。

Advanced settings are:

​	高级设置如下：

- **CPU limit**. Specify the maximum number of CPUs to be used by Docker Desktop. By default, Docker Desktop is set to use all the processors available on the host machine.

  - **CPU 限制**。指定 Docker Desktop 使用的最大 CPU 数量。默认情况下，Docker Desktop 设置为使用主机机器上可用的所有处理器。

- **Memory limit**. By default, Docker Desktop is set to use up to 50% of your host's memory. To increase the RAM, set this to a higher number; to decrease it, lower the number.

  - **内存限制**。默认情况下，Docker Desktop 设置为使用主机内存的 50%。要增加 RAM，设置为更高的数值；要减少 RAM，设置为较低的数值。

- **Swap**. Configure swap file size as needed. The default is 1 GB.

  - **交换空间**。根据需要配置交换文件大小，默认值为 1 GB。

- **Virtual disk limit**. Specify the maximum size of the disk image.

  - **虚拟磁盘限制**。指定磁盘映像的最大大小。

- **Disk image location**. Specify the location of the Linux volume where containers and images are stored.

  - **磁盘映像位置**。指定存储容器和镜像的 Linux 卷的位置。

  You can also move the disk image to a different location. If you attempt to move a disk image to a location that already has one, you are asked if you want to use the existing image or replace it.

  ​	您可以将磁盘镜像移动到其他位置。如果您尝试将磁盘镜像移动到已有镜像的位置，系统会询问您是使用现有镜像还是替换它。


> **Tip**
>
> 
>
> If you feel Docker Desktop starting to get slow or you're running multi-container workloads, increase the memory and disk image space allocation
>
> ​	如果您发现 Docker Desktop 开始变慢，或者正在运行多个容器工作负载，请增加内存和磁盘镜像空间的分配。

- **Resource Saver**. Enable or disable [Resource Saver mode]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/ResourceSavermode" >}}), which significantly reduces CPU and memory utilization on the host by automatically turning off the Linux VM when Docker Desktop is idle (i.e., no containers are running).

  - **资源节省模式**。启用或禁用 [资源节省模式]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/ResourceSavermode" >}})，它会在 Docker Desktop 空闲时（即没有容器运行）自动关闭 Linux 虚拟机，从而显著减少主机的 CPU 和内存使用量。

  You can also configure the Resource Saver timeout which indicates how long should Docker Desktop be idle before Resource Saver mode kicks in. Default is 5 minutes.
  
  ​	您还可以配置资源节省的超时时间，以指示 Docker Desktop 需要空闲多久才能激活资源节省模式。默认值为 5 分钟。
  
  > **Note**
  >
  > 
  >
  > Exit from Resource Saver mode occurs automatically when containers run. Exit may take a few seconds (~3 to 10 secs) as Docker Desktop restarts the Linux VM.
  >
  > ​	资源节省模式将在容器运行时自动退出。退出可能需要几秒钟（约 3 到 10 秒），因为 Docker Desktop 将重新启动 Linux 虚拟机。

### 文件共享 File sharing

> **Note**
>
> 
>
> On Windows, the **File sharing** tab is only available in Hyper-V mode because the files are automatically shared in WSL 2 mode and Windows container mode.
>
> ​	在 Windows 上，**文件共享** 选项卡仅在 Hyper-V 模式下可用，因为文件在 WSL 2 模式和 Windows 容器模式下会自动共享。

Use File sharing to allow local directories on your machine to be shared with Linux containers. This is especially useful for editing source code in an IDE on the host while running and testing the code in a container.

​	使用文件共享可以让您计算机上的本地目录与 Linux 容器共享。这对于在主机上的 IDE 中编辑源代码并在容器中运行和测试代码特别有用。

#### 同步文件共享 Synchronized file shares

Synchronized file shares is an alternative file sharing mechanism that provides fast and flexible host-to-VM file sharing, enhancing bind mount performance through the use of synchronized filesystem caches. Available with Pro, Team, and Business subscriptions.

​	同步文件共享是一种替代的文件共享机制，提供快速且灵活的主机到虚拟机文件共享，通过同步的文件系统缓存来提升绑定挂载的性能。适用于 Pro、Team 和 Business 订阅。

To learn more, see [Synchronized file share]({{< ref "/manuals/DockerDesktop/Synchronizedfileshares" >}}).

​	要了解更多信息，请参阅 [同步文件共享]({{< ref "/manuals/DockerDesktop/Synchronizedfileshares" >}})。

#### 虚拟文件共享 Virtual file shares

By default the `/Users`, `/Volumes`, `/private`, `/tmp` and `/var/folders` directory are shared. If your project is outside this directory then it must be added to the list, otherwise you may get `Mounts denied` or `cannot start service` errors at runtime.

​	默认情况下，共享 `/Users`、`/Volumes`、`/private`、`/tmp` 和 `/var/folders` 目录。如果您的项目位于此目录之外，则必须将其添加到列表中，否则可能会在运行时出现 `Mounts denied` 或 `cannot start service` 错误。

File share settings are:

​	文件共享设置包括：

- **Add a Directory**. Select `+` and navigate to the directory you want to add.
  - **添加目录**。选择 `+` 并导航到要添加的目录。

- **Remove a Directory**. Select `-` next to the directory you want to remove
  - **删除目录**。选择 `-` 以删除要移除的目录。

- **Apply & Restart** makes the directory available to containers using Docker's bind mount (`-v`) feature.
  - **应用并重启** 使目录可以通过 Docker 的绑定挂载（`-v`）功能供容器使用。


> **Tip**
>
> 
>
> - Share only the directories that you need with the container. File sharing introduces overhead as any changes to the files on the host need to be notified to the Linux VM. Sharing too many files can lead to high CPU load and slow filesystem performance.
>   - 仅共享您需要与容器共享的目录。文件共享会带来额外开销，因为主机上的文件发生更改时需要通知 Linux 虚拟机。共享过多文件可能导致高 CPU 负载和文件系统性能下降。
>
> - Shared folders are designed to allow application code to be edited on the host while being executed in containers. For non-code items such as cache directories or databases, the performance will be much better if they are stored in the Linux VM, using a [data volume]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}) (named volume) or [data container]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}).
>   - 共享文件夹旨在允许应用代码在主机上编辑并在容器中执行。对于缓存目录或数据库等非代码项，存储在 Linux 虚拟机中，使用 [数据卷]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})（命名卷）或 [数据容器]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}) 的性能会更好。
>
> - If you share the whole of your home directory into a container, MacOS may prompt you to give Docker access to personal areas of your home directory such as your Reminders or Downloads.
>   - 如果您将整个主目录共享到容器中，MacOS 可能会提示您授权 Docker 访问主目录中的个人区域，例如提醒或下载。
>
> - By default, Mac file systems are case-insensitive while Linux is case-sensitive. On Linux, it is possible to create two separate files: `test` and `Test`, while on Mac these filenames would actually refer to the same underlying file. This can lead to problems where an app works correctly on a developer's machine (where the file contents are shared) but fails when run in Linux in production (where the file contents are distinct). To avoid this, Docker Desktop insists that all shared files are accessed as their original case. Therefore, if a file is created called `test`, it must be opened as `test`. Attempts to open `Test` will fail with the error "No such file or directory". Similarly, once a file called `test` is created, attempts to create a second file called `Test` will fail.
>   - 默认情况下，Mac 文件系统区分大小写，而 Linux 是区分大小写的。在 Linux 上，可能会创建两个单独的文件：`test` 和 `Test`，而在 Mac 上，这两个文件名实际上引用相同的底层文件。这可能导致应用在开发人员计算机上（文件内容被共享）工作正常，但在 Linux 的生产环境中（文件内容不同）失败。为避免此情况，Docker Desktop 强制要求所有共享文件按其原始大小写访问。因此，如果创建了一个名为 `test` 的文件，则必须按 `test` 访问它。尝试打开 `Test` 会失败并显示错误“没有此类文件或目录”。同样，一旦创建了一个名为 `test` 的文件，尝试创建另一个名为 `Test` 的文件也会失败。
>
>
> For more information, see [Volume mounting requires file sharing for any project directories outside of `/Users`]({{< ref "/manuals/DockerDesktop/Troubleshootanddiagnose/Commontopics" >}})
>
> ​	更多信息，请参见 [项目目录外的 `/Users` 文件共享]({{< ref "/manuals/DockerDesktop/Troubleshootanddiagnose/Commontopics" >}})。

#### 按需共享文件夹 Shared folders on demand

On Windows, you can share a folder "on demand" the first time a particular folder is used by a container.

​	在 Windows 上，您可以在容器首次使用特定文件夹时“按需”共享该文件夹。

If you run a Docker command from a shell with a volume mount (as shown in the example below) or kick off a Compose file that includes volume mounts, you get a popup asking if you want to share the specified folder.

​	如果从 shell 中运行包含卷挂载的 Docker 命令（如下示例所示）或启动包含卷挂载的 Compose 文件，系统会弹出一个窗口，询问您是否要共享指定的文件夹。

You can select to **Share it**, in which case it is added to your Docker Desktop Shared Folders list and available to containers. Alternatively, you can opt not to share it by selecting **Cancel**.

​	您可以选择 **共享**，这会将其添加到 Docker Desktop 共享文件夹列表中并提供给容器使用。或者，您可以选择 **取消** 来不共享它。

![Shared folder on demand](Changesettings_img/shared-folder-on-demand.png)

### 代理 Proxies

Docker Desktop supports the use of HTTP/HTTPS and [SOCKS5 proxies](https://docs.docker.com/desktop/networking/#socks5-proxy-support).

​	Docker Desktop 支持 HTTP/HTTPS 和 [SOCKS5 代理](https://docs.docker.com/desktop/networking/#socks5-proxy-support)。

HTTP/HTTPS proxies can be used when:

​	在以下情况下可以使用 HTTP/HTTPS 代理：

- Signing in to Docker
  - 登录 Docker

- Pulling or pushing images
  - 拉取或推送镜像

- Fetching artifacts during image builds
  - 在镜像构建过程中获取工件

- Containers interact with the external network
  - 容器与外部网络交互

- Scanning images
  - 扫描镜像


If the host uses a HTTP/HTTPS proxy configuration (static or via Proxy Auto-Configuration (PAC)), Docker Desktop reads this configuration and automatically uses these settings for signing in to Docker, for pulling and pushing images, and for container Internet access. If the proxy requires authorization then Docker Desktop dynamically asks the developer for a username and password. All passwords are stored securely in the OS credential store. Note that only the `Basic` proxy authentication method is supported so we recommend using an `https://` URL for your HTTP/HTTPS proxies to protect passwords while in transit on the network. Docker Desktop supports TLS 1.3 when communicating with proxies.

​	如果主机使用 HTTP/HTTPS 代理配置（静态或通过代理自动配置（PAC）），Docker Desktop 会读取此配置并自动使用这些设置登录 Docker、拉取和推送镜像以及进行容器互联网访问。如果代理需要授权，则 Docker Desktop 会动态请求开发者输入用户名和密码。所有密码均安全地存储在操作系统的凭据存储中。注意，Docker Desktop 仅支持 `Basic` 代理身份验证方法，因此建议使用 `https://` URL 的 HTTP/HTTPS 代理，以保护传输中的密码。Docker Desktop 在与代理通信时支持 TLS 1.3。

To set a different proxy for Docker Desktop, turn on **Manual proxy configuration** and enter a single upstream proxy URL of the form `http://proxy:port` or `https://proxy:port`.

​	要为 Docker Desktop 设置不同的代理，请启用 **手动代理配置** 并输入格式为 `http://proxy:port` 或 `https://proxy:port` 的上游代理 URL。

To prevent developers from accidentally changing the proxy settings, see [Settings Management](https://docs.docker.com/security/for-admins/hardened-desktop/settings-management/#what-features-can-i-configure-with-settings-management).

​	要防止开发者意外更改代理设置，请参见 [设置管理](https://docs.docker.com/security/for-admins/hardened-desktop/settings-management/#what-features-can-i-configure-with-settings-management)。

The HTTPS proxy settings used for scanning images are set using the `HTTPS_PROXY` environment variable.

​	用于扫描镜像的 HTTPS 代理设置通过 `HTTPS_PROXY` 环境变量设置。

> **Note**
>
> 
>
> If you are using a PAC file hosted on a web server, make sure to add the MIME type `application/x-ns-proxy-autoconfig` for the `.pac` file extension on the server or website. Without this configuration, the PAC file may not be parsed correctly.
>
> ​	如果您使用的 PAC 文件托管在网络服务器上，请确保为 `.pac` 文件扩展名添加 MIME 类型 `application/x-ns-proxy-autoconfig`。否则，PAC 文件可能无法正确解析。

> **Important**
>
> 
>
> You do not need to separately configure proxy settings for the Docker CLI or Docker daemon.
>
> ​	您无需单独配置 Docker CLI 或 Docker 守护进程的代理设置。

#### 代理身份验证 Proxy authentication

##### 基本身份验证 Basic authentication

If your proxy uses Basic authentication, Docker Desktop prompts developers for a username and password and caches the credentials. All passwords are stored securely in the OS credential store. It will request re-authentication if that cache is removed.

​	如果您的代理使用基本身份验证，Docker Desktop 会提示开发者输入用户名和密码，并缓存这些凭据。所有密码均安全地存储在操作系统的凭据存储中。如果缓存被移除，系统将要求重新认证。

It's recommended that you use an `https://` URL for HTTP/HTTPS proxies to protect passwords during network transit. Docker Desktop also supports TLS 1.3 for communication with proxies.

​	建议您为 HTTP/HTTPS 代理使用 `https://` URL，以在网络传输过程中保护密码。Docker Desktop 还支持代理通信的 TLS 1.3。

##### Kerberos 和 NTLM 身份验证 Kerberos and NTLM authentication

> **Note**
>
> 
>
> Available for Docker Business subscribers with Docker Desktop for Windows version 4.30 and later.
>
> ​	Docker Business 订阅用户可以在 Docker Desktop for Windows 版本 4.30 及以上版本中使用。

Developers are no longer interrupted by prompts for proxy credentials as authentication is centralized. This also reduces the risk of account lockouts due to incorrect sign in attempts.

​	开发者不再会被身份验证的提示中断，因为身份验证已集中管理。这也降低了因登录尝试错误导致帐户锁定的风险。

If your proxy offers multiple authentication schemes in 407 (Proxy Authentication Required) response, Docker Desktop by default selects the Basic authentication scheme.

​	如果代理在 407（需要代理身份验证）响应中提供了多种身份验证方案，Docker Desktop 默认选择基本身份验证方案。

For Docker Desktop version 4.30 to 4.31:

​	对于 Docker Desktop 版本 4.30 到 4.31：

To enable Kerberos or NTLM proxy authentication, no additional configuration is needed beyond specifying the proxy IP address and port.

​	要启用 Kerberos 或 NTLM 代理身份验证，除指定代理 IP 地址和端口外，不需要额外配置。

For Docker Desktop version 4.32 and later:

​	对于 Docker Desktop 版本 4.32 及更高版本：

To enable Kerberos or NTLM proxy authentication you must pass the `--proxy-enable-kerberosntlm` installer flag during installation via the command line, and ensure your proxy server is properly configured for Kerberos or NTLM authentication.

​	要启用 Kerberos 或 NTLM 代理身份验证，必须在安装过程中通过命令行传递 `--proxy-enable-kerberosntlm` 安装程序标志，并确保您的代理服务器已正确配置为支持 Kerberos 或 NTLM 身份验证。

### 网络 Network

> **Note**
>
> 
>
> On Windows, the **Network** tab isn't available in the Windows container mode because Windows manages networking.
>
> ​	在 Windows 容器模式下，**网络** 选项卡不可用，因为网络由 Windows 管理。

Docker Desktop uses a private IPv4 network for internal services such as a DNS server and an HTTP proxy. In case Docker Desktop's choice of subnet clashes with IPs in your environment, you can specify a custom subnet using the **Network** setting.

​	Docker Desktop 使用专用的 IPv4 网络来提供 DNS 服务器和 HTTP 代理等内部服务。如果 Docker Desktop 选择的子网与您环境中的 IP 冲突，您可以使用 **网络** 设置指定自定义子网。

On Mac, you can also select the **Use kernel networking for UDP** setting. This lets you use a more efficient kernel networking path for UDP. This may not be compatible with your VPN software.

​	在 Mac 上，您还可以选择 **使用内核网络来支持 UDP** 设置。此选项允许您为 UDP 使用更高效的内核网络路径。该设置可能与您的 VPN 软件不兼容。

### WSL 集成 WSL Integration

On Windows in WSL 2 mode, you can configure which WSL 2 distributions will have the Docker WSL integration.

​	在 Windows 的 WSL 2 模式下，您可以配置哪些 WSL 2 发行版将启用 Docker WSL 集成。

By default, the integration is enabled on your default WSL distribution. To change your default WSL distro, run `wsl --set-default <distro name>`. (For example, to set Ubuntu as your default WSL distro, run `wsl --set-default ubuntu`).

​	默认情况下，集成在您的默认 WSL 发行版上启用。要更改默认的 WSL 发行版，请运行 `wsl --set-default <distro name>`。（例如，要将 Ubuntu 设置为默认 WSL 发行版，运行 `wsl --set-default ubuntu`。）

You can also select any additional distributions you would like to enable the WSL 2 integration on.

​	您还可以选择希望启用 WSL 2 集成的其他发行版。

For more details on configuring Docker Desktop to use WSL 2, see [Docker Desktop WSL 2 backend]({{< ref "/manuals/DockerDesktop/WSL" >}}).

​	有关配置 Docker Desktop 以使用 WSL 2 的更多详细信息，请参阅 [Docker Desktop WSL 2 后端]({{< ref "/manuals/DockerDesktop/WSL" >}})。

## Docker Engine

The **Docker Engine** tab allows you to configure the Docker daemon used to run containers with Docker Desktop.

​	**Docker 引擎** 选项卡允许您配置 Docker Desktop 用于运行容器的 Docker 守护进程。

You configure the daemon using a JSON configuration file. Here's what the file might look like:

​	您可以使用 JSON 配置文件来配置守护进程。文件示例如下：



```json
{
  "builder": {
    "gc": {
      "defaultKeepStorage": "20GB",
      "enabled": true
    }
  },
  "experimental": false
}
```

You can find this file at `$HOME/.docker/daemon.json`. To change the configuration, either edit the JSON configuration directly from the dashboard in Docker Desktop, or open and edit the file using your favorite text editor.

​	您可以在 `$HOME/.docker/daemon.json` 中找到此文件。要更改配置，可以直接在 Docker Desktop 的仪表板中编辑 JSON 配置，或使用您喜欢的文本编辑器打开并编辑文件。

To see the full list of possible configuration options, see the [dockerd command reference]({{< ref "/reference/CLIreference/dockerd" >}}).

​	要查看可能的配置选项列表，请参阅 [dockerd 命令参考]({{< ref "/reference/CLIreference/dockerd" >}})。

Select **Apply & Restart** to save your settings and restart Docker Desktop.

​	选择 **应用并重启** 以保存设置并重启 Docker Desktop。

## 构建器 Builders

If you have turned on the [Docker Desktop Builds view]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/Builds" >}}), you can use the **Builders** tab to inspect and manage builders in the Docker Desktop settings.

​	如果您已启用 [Docker Desktop 构建视图]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/Builds" >}})，可以使用 **构建器** 选项卡在 Docker Desktop 设置中检查和管理构建器。

### 检查 Inspect

To inspect builders, find the builder that you want to inspect and select the expand icon. You can only inspect active builders.

​	要检查构建器，找到您要检查的构建器并选择展开图标。仅可检查活动构建器。

Inspecting an active builder shows:

​	检查活动构建器显示：

- BuildKit version

- Status
- Driver type
- Supported capabilities and platforms 支持的功能和平台
- Disk usage
- Endpoint address

### 选择不同的构建器 Select a different builder

The **Selected builder** section displays the selected builder. To select a different builder:

​	**选定的构建器** 部分显示当前选定的构建器。要选择不同的构建器：

1. Find the builder that you want to use under **Available builders** 在 **可用构建器** 下找到您要使用的构建器
2. Open the drop-down menu next to the builder's name. 打开构建器名称旁边的下拉菜单。
3. Select **Use** to switch to this builder. 选择 **使用** 切换到此构建器。

Your build commands now use the selected builder by default.

​	您的构建命令现在默认使用选定的构建器。

### 创建构建器 Create a builder

To create a builder, use the Docker CLI. See [Create a new builder](https://docs.docker.com/build/builders/manage/#create-a-new-builder)

​	要创建构建器，请使用 Docker CLI。请参阅 [创建新构建器](https://docs.docker.com/build/builders/manage/#create-a-new-builder)。

### 删除构建器 Remove a builder

You can remove a builder if:

​	如果满足以下条件，您可以删除构建器：

- The builder isn't your [selected builder](https://docs.docker.com/build/builders/#selected-builder)

  - 构建器不是您的 [选定构建器](https://docs.docker.com/build/builders/#selected-builder)

- The builder isn't [associated with a Docker context](https://docs.docker.com/build/builders/#default-builder).

  - 构建器未与 [Docker 上下文](https://docs.docker.com/build/builders/#default-builder) 关联。

  To remove builders associated with a Docker context, remove the context using the `docker context rm` command.

  ​	要删除与 Docker 上下文关联的构建器，请使用 `docker context rm` 命令删除上下文。


To remove a builder:

​	要删除构建器：

1. Find the builder that you want to remove under **Available builders **在 **可用构建器** 下找到您要删除的构建器
2. Open the drop-down menu. 打开下拉菜单。
3. Select **Remove** to remove this builder. 选择 **删除** 以删除此构建器。

If the builder uses the `docker-container` or `kubernetes` driver, the build cache is also removed, along with the builder.

​	如果构建器使用 `docker-container` 或 `kubernetes` 驱动程序，则会同时删除构建缓存及构建器。

### 停止和启动构建器 Stop and start a builder

Builders that use the [`docker-container` driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockercontainerbuilddriver" >}}) run the BuildKit daemon in a container. You can start and stop the BuildKit container using the drop-down menu.

​	使用 [`docker-container` 驱动程序]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockercontainerbuilddriver" >}}) 的构建器在容器中运行 BuildKit 守护进程。您可以使用下拉菜单启动和停止 BuildKit 容器。

Running a build automatically starts the container if it's stopped.

​	如果容器已停止，运行构建将自动启动它。

You can only start and stop builders using the `docker-container` driver.

​	您只能使用 `docker-container` 驱动程序启动和停止构建器。

## Kubernetes

> **Note**
>
> 
>
> On Windows the **Kubernetes** tab is not available in Windows container mode.
>
> ​	在 Windows 容器模式下，**Kubernetes** 选项卡不可用。

Docker Desktop includes a standalone Kubernetes server, so that you can test deploying your Docker workloads on Kubernetes. To turn on Kubernetes support and install a standalone instance of Kubernetes running as a Docker container, select **Enable Kubernetes**.

​	Docker Desktop 包含一个独立的 Kubernetes 服务器，以便您测试在 Kubernetes 上部署 Docker 工作负载。要启用 Kubernetes 支持并安装以 Docker 容器运行的独立 Kubernetes 实例，请选择 **启用 Kubernetes**。

Select **Show system containers (advanced)** to view internal containers when using Docker commands.

​	选择 **显示系统容器（高级）** 以在使用 Docker 命令时查看内部容器。

Select **Reset Kubernetes cluster** to delete all stacks and Kubernetes resources.

​	选择 **重置 Kubernetes 集群** 以删除所有栈和 Kubernetes 资源。

For more information about using the Kubernetes integration with Docker Desktop, see [Deploy on Kubernetes]({{< ref "/manuals/DockerDesktop/DeployonKuberneteswithDockerDesktop" >}}).

​	有关使用 Docker Desktop 的 Kubernetes 集成的更多信息，请参阅 [在 Kubernetes 上部署]({{< ref "/manuals/DockerDesktop/DeployonKuberneteswithDockerDesktop" >}})。

## 软件更新 Software Updates

The **Software Updates** tab notifies you of any updates available to Docker Desktop. When there's a new update, you can choose to download the update right away, or select the **Release Notes** option to learn what's included in the updated version.

​	**软件更新** 选项卡会通知您 Docker Desktop 的可用更新。出现新更新时，您可以立即选择下载更新，或选择 **发行说明** 了解更新版本中包含的内容。

Turn off the check for updates by clearing the **Automatically check for updates** check box. This disables notifications in the Docker menu and the notification badge that appears on the Docker Dashboard. To check for updates manually, select the **Check for updates** option in the Docker menu.

​	通过清除 **自动检查更新** 复选框来关闭自动检查更新。这将禁用 Docker 菜单中的通知和 Docker Dashboard 上显示的通知徽章。要手动检查更新，请在 Docker 菜单中选择 **检查更新** 选项。

To allow Docker Desktop to automatically download new updates in the background, select **Always download updates**. This downloads newer versions of Docker Desktop when an update becomes available. After downloading the update, select **Apply and Restart** to install the update. You can do this either through the Docker menu or in the **Updates** section in the Docker Dashboard.

​	要允许 Docker Desktop 在后台自动下载新更新，请选择 **始终下载更新**。这会在更新可用时下载 Docker Desktop 的新版本。下载更新后，通过 Docker 菜单或在 Docker Dashboard 的 **更新** 部分选择 **应用并重启** 以安装更新。

## 扩展 Extensions

Use the **Extensions** tab to:

​	使用 **扩展** 选项卡：

- **Enable Docker Extensions**
  - **启用 Docker 扩展**

- **Allow only extensions distributed through the Docker Marketplace**
  - **仅允许通过 Docker Marketplace 分发的扩展**

- **Show Docker Extensions system containers**
  - **显示 Docker 扩展系统容器**


For more information about Docker extensions, see [Extensions]({{< ref "/manuals/DockerExtensions" >}}).

​	有关 Docker 扩展的更多信息，请参阅 [扩展]({{< ref "/manuals/DockerExtensions" >}})。

## 开发中的功能 Features in development

On the **Feature control** tab you can control your settings for **Beta features** and **Experimental features**.

​	在 **功能控制** 选项卡上，您可以控制 **Beta 功能** 和 **实验功能** 的设置。

You can also sign up to the [Developer Preview program](https://www.docker.com/community/get-involved/developer-preview/) from the **Features in development** tab.

​	您还可以在 **开发中的功能** 选项卡中注册 [开发者预览计划](https://www.docker.com/community/get-involved/developer-preview/)。

### Beta 功能 Beta features

Beta features provide access to future product functionality. These features are intended for testing and feedback only as they may change between releases without warning or remove them entirely from a future release. Beta features must not be used in production environments. Docker doesn't offer support for beta features.

​	Beta 功能提供未来产品功能的访问权限。这些功能仅用于测试和反馈，因为它们可能在版本间发生更改，甚至可能在未来版本中完全移除。Beta 功能不应在生产环境中使用。Docker 不提供 Beta 功能的支持。

### 实验功能 Experimental features

Experimental features provide early access to future product functionality. These features are intended for testing and feedback only as they may change between releases without warning or can be removed entirely from a future release. Experimental features must not be used in production environments. Docker does not offer support for experimental features.

​	实验功能提供未来产品功能的早期访问权限。这些功能仅用于测试和反馈，因为它们可能在版本间发生更改，甚至可能在未来版本中完全移除。实验功能不应在生产环境中使用。Docker 不提供实验功能的支持。

For a list of current experimental features in the Docker CLI, see [Docker CLI Experimental features](https://github.com/docker/cli/blob/master/experimental/README.md).

​	有关 Docker CLI 中当前实验功能的列表，请参阅 [Docker CLI 实验功能](https://github.com/docker/cli/blob/master/experimental/README.md)。

## 通知 Notifications

Use the **Notifications** tab to turn on or turn off notifications for the following events:

​	使用 **通知** 选项卡可以打开或关闭以下事件的通知：

- **Status updates on tasks and processes**
  - **任务和进程状态更新**

- **Docker announcements**
  - **Docker 公告**

- **Docker surveys**
  - **Docker 调查**


By default, all notifications are turned on. You'll always receive error notifications and notifications about new Docker Desktop releases and updates.

​	默认情况下，所有通知均已开启。您将始终收到有关 Docker Desktop 发布和更新的错误通知和通知。

Notifications momentarily appear in the lower-right of the Docker Dashboard and then move to the **Notifications** drawer. To open the **Notifications** drawer, select ![notifications](Changesettings_img/notifications.svg) .

​	通知会短暂显示在 Docker Dashboard 的右下角，然后移动到 **通知** 抽屉。要打开 **通知** 抽屉，请选择 ![notifications](Changesettings_img/notifications.svg)。

## 高级 Advanced

On Mac, you can reconfigure your initial installation settings on the **Advanced** tab:

​	在 Mac 上，您可以在 **高级** 选项卡中重新配置初始安装设置：

- **Choose how to configure the installation of Docker's CLI tools**. **选择 Docker CLI 工具的安装配置方式**。

  - **System**: Docker CLI tools are installed in the system directory under `/usr/local/bin`

  - User: Docker CLI tools are installed in the user directory under  `$HOME/.docker/bin`. You must then add `$HOME/.docker/bin` to your PATH. To add `$HOME/.docker/bin` to your path: 用户：Docker CLI 工具安装在用户目录 `$HOME/.docker/bin` 下。您需要将 `$HOME/.docker/bin` 添加到 PATH 中。要将 `$HOME/.docker/bin` 添加到您的路径中：

    1. Open your shell configuration file. This is `~/.bashrc` if you're using a bash shell, or `~/.zshrc` if you're using a zsh shell. 打开您的 shell 配置文件。如果您使用的是 bash shell，则为 `~/.bashrc`；如果是 zsh shell，则为 `~/.zshrc`。

    2. Copy and paste the following:

       复制并粘贴以下内容：
    
       ```console
     $ export PATH=$PATH:~/.docker/bin
       ```

    3. Save and the close the file. Restart your shell to apply the changes to the PATH variable. 保存并关闭文件。重启 shell 以应用 PATH 变量的更改。

- **Enable default Docker socket (Requires password)**. Creates `/var/run/docker.sock` which some third party clients may use to communicate with Docker Desktop. For more information, see [permission requirements for macOS](https://docs.docker.com/desktop/install/mac-permission-requirements/#installing-symlinks).

  - **启用默认 Docker 套接字（需要密码）**：创建 `/var/run/docker.sock`，某些第三方客户端可能使用它与 Docker Desktop 通信。更多信息请参见 [macOS 的权限要求](https://docs.docker.com/desktop/install/mac-permission-requirements/#installing-symlinks)。

- **Enable privileged port mapping (Requires password)**. Starts the privileged helper process which binds the ports that are between 1 and 1024. For more information, see [permission requirements for macOS](https://docs.docker.com/desktop/install/mac-permission-requirements/#binding-privileged-ports).

  - **启用特权端口映射（需要密码）**：启动特权助手进程，该进程绑定 1 到 1024 之间的端口。更多信息请参见 [macOS 的权限要求](https://docs.docker.com/desktop/install/mac-permission-requirements/#binding-privileged-ports)。

  For more information on each configuration and use case, see [Permission requirements]({{< ref "/manuals/DockerDesktop/Install/UnderstandpermissionrequirementsforDockerDesktoponMac" >}}).

  ​	有关每个配置和使用场景的更多信息，请参见 [权限要求]({{< ref "/manuals/DockerDesktop/Install/UnderstandpermissionrequirementsforDockerDesktoponMac" >}})。
