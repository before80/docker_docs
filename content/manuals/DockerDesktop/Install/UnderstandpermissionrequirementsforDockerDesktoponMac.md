+++
title = "了解 Mac 上 Docker Desktop 的权限要求"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/install/mac-permission-requirements/](https://docs.docker.com/desktop/install/mac-permission-requirements/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Understand permission requirements for Docker Desktop on Mac - 了解 Mac 上 Docker Desktop 的权限要求

This page contains information about the permission requirements for running and installing Docker Desktop on Mac.

​	本页面包含有关在 Mac 上运行和安装 Docker Desktop 的权限要求的信息。

It also provides clarity on running containers as `root` as opposed to having `root` access on the host.

​	同时，阐明了以 `root` 身份运行容器与在主机上获得 `root` 访问权限之间的区别。

## 权限要求 Permission requirements

Docker Desktop for Mac is run as an unprivileged user. However, Docker Desktop requires certain functionalities to perform a limited set of privileged configurations such as:

​	Docker Desktop for Mac 以非特权用户身份运行。然而，Docker Desktop 需要某些功能来执行一小部分特权配置操作，例如：

- [Installing symlinks](https://docs.docker.com/desktop/install/mac-permission-requirements/#installing-symlinks) in`/usr/local/bin`.
  - 在 `/usr/local/bin` 中[安装符号链接](https://docs.docker.com/desktop/install/mac-permission-requirements/#installing-symlinks)。

- [Binding privileged ports](https://docs.docker.com/desktop/install/mac-permission-requirements/#binding-privileged-ports) that are less than 1024. The so-called "privileged ports" are not generally used as a security boundary, however operating systems still prevent unprivileged processes from binding them which breaks commands like `docker run -p 127.0.0.1:80:80 docker/getting-started`.
  - 绑定小于 1024 的[特权端口](https://docs.docker.com/desktop/install/mac-permission-requirements/#binding-privileged-ports)。这些“特权端口”通常不用于安全边界，但操作系统仍然阻止非特权进程绑定它们，这会导致 `docker run -p 127.0.0.1:80:80 docker/getting-started` 等命令失败。

- [Ensuring `localhost` and `kubernetes.docker.internal` are defined](https://docs.docker.com/desktop/install/mac-permission-requirements/#ensuring-localhost-and-kubernetesdockerinternal-are-defined) in `/etc/hosts`. Some old macOS installs don't have `localhost` in `/etc/hosts`, which causes Docker to fail. Defining the DNS name `kubernetes.docker.internal` allows Docker to share Kubernetes contexts with containers.
  - 确保 `/etc/hosts` 中定义了 `localhost` 和 `kubernetes.docker.internal`。某些旧版 macOS 安装中没有 `/etc/hosts` 中的 `localhost`，这会导致 Docker 失败。定义 DNS 名称 `kubernetes.docker.internal` 可使 Docker 在容器中共享 Kubernetes 上下文。

- Securely caching the Registry Access Management policy which is read-only for the developer.
  - 安全地缓存注册表访问管理策略，该策略对开发者来说是只读的。


Depending on which version of Docker Desktop for Mac is used, privileged access is granted either during installation, first run, or only when it's needed.

​	根据使用的 Docker Desktop for Mac 版本，特权访问可能在安装时、首次运行时或仅在需要时授予。

{{< tabpane text=true persist=disabled >}}

{{% tab header="Version 4.18 and later" %}}

From version 4.18 and later, Docker Desktop for Mac provides greater control over functionality that's enabled during installation.

​	从4.18及更高版本开始，Docker Desktop for Mac在安装过程中提供了对启用功能的更多控制。

The first time Docker Desktop for Mac launches, it presents an installation window where you can choose to either use the default settings, which work for most developers and requires you to grant privileged access, or use advanced settings.

​	Docker Desktop for Mac首次启动时会显示一个安装窗口，您可以选择使用默认设置（适用于大多数开发者，并要求您授予特权访问权限）或使用高级设置。

If you work in an environment with elevated security requirements, for instance where local administrative access is prohibited, then you can use the advanced settings to remove the need for granting privileged access. You can configure:

​	如果您在具有较高安全要求的环境中工作，例如禁止本地管理访问，您可以使用高级设置来取消特权访问的需求。您可以配置：

- The location of the Docker CLI tools either in the system or user directory
  - Docker CLI 工具的位置（系统目录或用户目录）

- The default Docker socket
  - 默认 Docker 套接字

- Privileged port mapping
  - 特权端口映射


Depending on which advanced settings you configure, you must enter your password to confirm.

​	根据您配置的高级设置，可能需要输入密码进行确认。

You can change these configurations at a later date from the **Advanced** page in **Settings**.

​	您可以稍后从**设置**中的**高级**页面更改这些配置。

{{% /tab  %}}

{{% tab header="Version 4.15 - 4.17" %}}

Versions 4.15 to 4.17 of Docker Desktop for Mac don't require the privileged process to run permanently. Whenever elevated privileges are needed for a configuration, Docker Desktop prompts you with information on the task it needs to perform. Most configurations are applied once, subsequent runs don't prompt for privileged access anymore. The only time Docker Desktop may start the privileged process is for binding privileged ports that aren't allowed by default on the host OS.

​	4.15到4.17版本的Docker Desktop for Mac不需要永久运行特权进程。每当某个配置需要提升权限时，Docker Desktop会提示您进行操作说明。大多数配置只需应用一次，之后的运行不再提示特权访问。唯一可能启动特权进程的情况是绑定主机操作系统默认不允许的特权端口。

{{% /tab  %}}

{{% tab header="  Versions prior to 4.15" %}}

Versions prior to 4.15 of Docker Desktop for Mac require `root` access to be granted on the first run. The first time that Docker Desktop launches you receive an admin prompt to grant permission for the installation of the `com.docker.vmnetd` privileged helper service. For subsequent runs, `root` privileges aren't required. Following the principle of least privilege, this approach allows `root` access to be used only for the operations for which it's absolutely necessary, while still being able to use Docker Desktop as an unprivileged user. All privileged operations are run using the privileged helper process `com.docker.vmnetd`.

​	4.15之前的Docker Desktop for Mac版本在首次运行时需要授予`root`权限。首次启动Docker Desktop时，您会收到管理员提示，授予安装`com.docker.vmnetd`特权辅助服务的权限。后续运行无需`root`权限。按照最低权限原则，这种方法仅在绝对必要的操作中使用`root`权限，同时仍允许非特权用户使用Docker Desktop。所有特权操作通过特权辅助进程`com.docker.vmnetd`运行。

{{% /tab  %}}

{{< /tabpane >}}



------

### 安装符号链接 Installing symlinks

The Docker binaries are installed by default in `/Applications/Docker.app/Contents/Resources/bin`. Docker Desktop creates symlinks for the binaries in `/usr/local/bin`, which means they're automatically included in `PATH` on most systems.

​	Docker 二进制文件默认安装在 `/Applications/Docker.app/Contents/Resources/bin`。Docker Desktop 在 `/usr/local/bin` 中为这些二进制文件创建符号链接，这意味着它们会自动包含在大多数系统的 `PATH` 中。

{{< tabpane text=true persist=disabled >}}

{{% tab header="Version 4.18 and later" %}}

With version 4.18 and later, you can choose whether to install symlinks either in `/usr/local/bin` or `$HOME/.docker/bin` during installation of Docker Desktop.

​	从4.18版本开始，您可以选择在Docker Desktop安装期间将符号链接安装在`/usr/local/bin`或`$HOME/.docker/bin`中。

If `/usr/local/bin` is chosen, and this location is not writable by unprivileged users, Docker Desktop requires authorization to confirm this choice before the symlinks to Docker binaries are created in `/usr/local/bin`. If `$HOME/.docker/bin` is chosen, authorization is not required, but then you must [manually add `$HOME/.docker/bin`](https://docs.docker.com/desktop/settings/#advanced) to their PATH.

​	如果选择`/usr/local/bin`且该位置对非特权用户不可写，Docker Desktop需要授权以确认该选择，然后在`/usr/local/bin`中创建符号链接。如果选择`$HOME/.docker/bin`，则不需要授权，但您需要[手动添加 `$HOME/.docker/bin`](https://docs.docker.com/desktop/settings/#advanced) 到PATH中。

You are also given the option to enable the installation of the `/var/run/docker.sock` symlink. Creating this symlink ensures various Docker clients relying on the default Docker socket path work without additional changes.

​	此外，您还可以选择启用`/var/run/docker.sock`符号链接的安装。创建此符号链接可以确保依赖默认Docker套接字路径的各种Docker客户端无需其他更改。

As the `/var/run` is mounted as a tmpfs, its content is deleted on restart, symlink to the Docker socket included. To ensure the Docker socket exists after restart, Docker Desktop sets up a `launchd` startup task that creates the symlink by running `ln -s -f /Users/<user>/.docker/run/docker.sock /var/run/docker.sock`. This ensures the you aren't prompted on each startup to create the symlink. If you don't enable this option at installation, the symlink and the startup task is not created and you may have to explicitly set the `DOCKER_HOST` environment variable to `/Users/<user>/.docker/run/docker.sock` in the clients it is using. The Docker CLI relies on the current context to retrieve the socket path, the current context is set to `desktop-linux` on Docker Desktop startup.

​	由于`/var/run`挂载为tmpfs，其内容在重启时会删除，包括指向Docker套接字的符号链接。为确保重启后Docker套接字存在，Docker Desktop设置了一个`launchd`启动任务，通过运行`ln -s -f /Users/<user>/.docker/run/docker.sock /var/run/docker.sock`来创建符号链接。这确保了每次启动时不会提示您创建符号链接。如果您在安装时未启用此选项，则不会创建符号链接和启动任务，您可能需要在使用的客户端中将`DOCKER_HOST`环境变量显式设置为`/Users/<user>/.docker/run/docker.sock`。Docker CLI 依赖当前上下文来检索套接字路径，Docker Desktop启动时当前上下文设置为`desktop-linux`。

{{% /tab  %}}

{{% tab header=" Version 4.17 and earlier" %}}

For versions prior to 4.18, installing symlinks in `/usr/local/bin` is a privileged configuration Docker Desktop performs on the first startup. Docker Desktop checks if symlinks exists and takes the following actions:

​	在4.18之前的版本中，将符号链接安装到 `/usr/local/bin` 是 Docker Desktop 在首次启动时执行的特权配置。Docker Desktop 会检查符号链接是否存在并采取以下措施：

- Creates the symlinks without the admin prompt if `/usr/local/bin` is writable by unprivileged users.
  - 如果 `/usr/local/bin` 对非特权用户可写，则无需管理员提示创建符号链接。

- Triggers an admin prompt for you to authorize the creation of symlinks in `/usr/local/bin`. If you authorizes this, symlinks to Docker binaries are created in `/usr/local/bin`. If you reject the prompt, are not willing to run configurations requiring elevated privileges, or don't have admin rights on your machine, Docker Desktop creates the symlinks in `~/.docker/bin` and edits your shell profile to ensure this location is in your PATH. This requires all open shells to be reloaded. The rejection is recorded for future runs to avoid prompting you again. For any failure to ensure binaries are on your PATH, you may need to manually add to their PATH the `/Applications/Docker.app/Contents/Resources/bin` or use the full path to Docker binaries.
  - 如果需要管理员权限来创建符号链接，则会弹出管理员提示。若您授权，则在 `/usr/local/bin` 中创建 Docker 二进制文件的符号链接。若拒绝提示、不愿运行需要提升权限的配置或没有机器管理员权限，Docker Desktop 会在 `~/.docker/bin` 中创建符号链接并编辑 shell 配置文件，以确保此位置在 PATH 中。需要重新加载所有打开的 shell。拒绝会在后续运行中记录以避免重复提示。如需确保二进制文件在 PATH 中，您可能需要手动添加 `/Applications/Docker.app/Contents/Resources/bin` 或使用 Docker 二进制文件的完整路径。


A particular case is the installation of the `/var/run/docker.sock` symlink. Creating this symlink ensures various Docker clients relying on the default Docker socket path work without additional changes. As the `/var/run` is mounted as a tmpfs, its content is deleted on restart, symlink to Docker socket included. To ensure the Docker socket exists after restart, Docker Desktop sets up a `launchd` startup task that creates a symlink by running `ln -s -f /Users/<user>/.docker/run/docker.sock /var/run/docker.sock`. This ensures that you are not prompted on each startup to create the symlink. If you reject the prompt, the symlink and the startup task are not created and you may have to explicitly set the `DOCKER_HOST` to `/Users/<user>/.docker/run/docker.sock` in the clients it is using. The Docker CLI relies on the current context to retrieve the socket path, the current context is set to `desktop-linux` on Docker Desktop startup.

​	创建 `/var/run/docker.sock` 符号链接的情况较为特殊。此符号链接可确保依赖默认 Docker 套接字路径的各种 Docker 客户端无需额外更改即可工作。由于 `/var/run` 挂载为 tmpfs，其内容在重启时会删除，包括指向 Docker 套接字的符号链接。为确保重启后 Docker 套接字存在，Docker Desktop 设置了一个 `launchd` 启动任务，通过运行 `ln -s -f /Users/<user>/.docker/run/docker.sock /var/run/docker.sock` 创建符号链接。这确保您在每次启动时不会被提示创建符号链接。如果您拒绝提示，则不会创建符号链接和启动任务，您可能需要在使用的客户端中显式将 `DOCKER_HOST` 设置为 `/Users/<user>/.docker/run/docker.sock`。Docker CLI 依赖当前上下文来检索套接字路径，Docker Desktop 启动时当前上下文设置为 `desktop-linux`。

{{% /tab  %}}

{{< /tabpane >}}



### 绑定特权端口 Binding privileged ports

{{< tabpane text=true persist=disabled >}}

{{% tab header="Version 4.18 and later" %}}

With version 4.18 and later you can choose to enable privileged port mapping during installation, or from the **Advanced** page in **Settings** post-installation. Docker Desktop requires authorization to confirm this choice.

​	在4.18及更高版本中，您可以在安装期间或在安装后从**设置**中的**高级**页面中选择启用特权端口映射。Docker Desktop 需要授权以确认此选择。

{{% /tab  %}}

{{% tab header=" Version 4.17 and earlier" %}}

For versions below 4.18 , if you run a container that requires binding privileged ports, Docker Desktop first attempts to bind it directly as an unprivileged process. If the OS prevents this and it fails, Docker Desktop checks if the `com.docker.vmnetd` privileged helper process is running to bind the privileged port through it.

​	在4.18以下版本中，如果您运行的容器需要绑定特权端口，Docker Desktop首先会尝试以非特权进程直接绑定它。如果操作系统阻止了该操作并导致失败，Docker Desktop会检查`com.docker.vmnetd`特权辅助进程是否正在运行，以便通过它绑定特权端口。

If the privileged helper process is not running, Docker Desktop prompts you for authorization to run it under [launchd](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLaunchdJobs.html). This configures the privileged helper to run as in the versions of Docker Desktop prior to 4.15. However, the functionality provided by this privileged helper now only supports port binding and caching the Registry Access Management policy. If you decline the launch of the privileged helper process, binding the privileged port cannot be done and the Docker CLI returns an error:

​	如果特权辅助进程未运行，Docker Desktop 会提示您授权在[launchd](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLaunchdJobs.html)下运行该进程。这将配置特权辅助进程，以便与4.15之前的Docker Desktop版本中的行为一致。然而，此特权辅助进程现在仅支持端口绑定和缓存注册表访问管理策略的功能。如果您拒绝启动特权辅助进程，则无法绑定特权端口，并且Docker CLI会返回错误：



```console
$ docker run -p 127.0.0.1:80:80 docker/getting-started

docker: Error response from daemon: Ports are not available: exposing port
TCP 127.0.0.1:80 -> 0.0.0.0:0: failed to connect to /var/run/com.docker.vmnetd.sock:
is vmnetd running?: dial unix /var/run/com.docker.vmnetd.sock: connect: connection
refused.
ERRO[0003] error waiting for container: context canceled
```

> **Note**
>
> 
>
> The command may fail with the same error if you take too long to authorize the prompt to start the helper process, as it may timeout.
>
> ​	如果您授权启动辅助进程的提示花费的时间过长，命令可能会因超时而失败并出现相同的错误。

{{% /tab  %}}

{{< /tabpane >}}

### 确保 `localhost` 和 `kubernetes.docker.internal` 已定义 Ensuring `localhost` and `kubernetes.docker.internal` are defined

{{< tabpane text=true persist=disabled >}}

{{% tab header="Version 4.18 and later" %}}

With versions 4.18 and later, it is your responsibility to ensure that localhost is resolved to `127.0.0.1` and if Kubernetes is used, that `kubernetes.docker.internal` is resolved to `127.0.0.1`.

​	在4.18及更高版本中，您需要确保 `localhost` 解析为 `127.0.0.1`，如果使用 Kubernetes，还需确保 `kubernetes.docker.internal` 解析为 `127.0.0.1`。

{{% /tab  %}}

{{% tab header=" Version 4.17 and earlier" %}}

On first run, Docker Desktop checks if `localhost` is resolved to `127.0.0.1`. In case the resolution fails, it prompts you to allow adding the mapping to `/etc/hosts`. Similarly, when the Kubernetes cluster is installed, it checks that `kubernetes.docker.internal` is resolved to `127.0.0.1` and prompts you to do so.

​	在首次运行时，Docker Desktop 会检查 `localhost` 是否解析为 `127.0.0.1`。如果解析失败，Docker Desktop 会提示您允许在 `/etc/hosts` 中添加该映射。同样，在安装 Kubernetes 集群时，它会检查 `kubernetes.docker.internal` 是否解析为 `127.0.0.1` 并提示您进行相应配置。

{{% /tab  %}}

{{< /tabpane >}}

------

## 从命令行安装Installing from the command line

In version 4.11 and later of Docker Desktop for Mac, privileged configurations are applied during the installation with the `--user` flag on the [install command](https://docs.docker.com/desktop/install/mac-install/#install-from-the-command-line). In this case, you are not prompted to grant root privileges on the first run of Docker Desktop. Specifically, the `--user` flag:

​	在4.11及更高版本的 Docker Desktop for Mac 中，可以在安装期间使用 `--user` 标志应用特权配置。这样，在 Docker Desktop 首次运行时无需授予 root 权限。具体来说，`--user` 标志会执行以下操作：

- Uninstalls the previous `com.docker.vmnetd` if present
  - 卸载现有的 `com.docker.vmnetd`（如果存在）

- Sets up symlinks
  - 设置符号链接

- Ensures that `localhost` is resolved to `127.0.0.1`
  - 确保 `localhost` 解析为 `127.0.0.1`


The limitation of this approach is that Docker Desktop can only be run by one user-account per machine, namely the one specified in the `-–user` flag.

​	此方法的限制在于，每台机器上只能有一个指定在 `--user` 标志中的用户账户可以运行 Docker Desktop。

## 特权辅助进程 Privileged helper

In the limited situations when the privileged helper is needed, for example binding privileged ports or caching the Registry Access Management policy, the privileged helper is started by `launchd` and runs in the background unless it is disabled at runtime as previously described. The Docker Desktop backend communicates with the privileged helper over the UNIX domain socket `/var/run/com.docker.vmnetd.sock`. The functionalities it performs are:

​	在需要特权辅助进程的有限情况下，例如绑定特权端口或缓存注册表访问管理策略，特权辅助进程会由 `launchd` 启动并在后台运行，除非按照之前描述在运行时禁用它。Docker Desktop 后端通过 UNIX 域套接字 `/var/run/com.docker.vmnetd.sock` 与特权辅助进程进行通信。特权辅助进程提供以下功能：

- Binding privileged ports that are less than 1024.
  - 绑定小于 1024 的特权端口。

- Securely caching the Registry Access Management policy which is read-only for the developer.
  - 安全地缓存只读的注册表访问管理策略。

- Uninstalling the privileged helper.
  - 卸载特权辅助进程。


The removal of the privileged helper process is done in the same way as removing `launchd` processes.

​	可以通过与删除 `launchd` 进程相同的方式移除特权辅助进程：



```console
$ ps aux | grep vmnetd
root             28739   0.0  0.0 34859128    228   ??  Ss    6:03PM   0:00.06 /Library/PrivilegedHelperTools/com.docker.vmnetd
user             32222   0.0  0.0 34122828    808 s000  R+   12:55PM   0:00.00 grep vmnetd

$ sudo launchctl unload -w /Library/LaunchDaemons/com.docker.vmnetd.plist
Password:

$ ps aux | grep vmnetd
user             32242   0.0  0.0 34122828    716 s000  R+   12:55PM   0:00.00 grep vmnetd

$ rm /Library/LaunchDaemons/com.docker.vmnetd.plist

$ rm /Library/PrivilegedHelperTools/com.docker.vmnetd
```

## 在 Linux 虚拟机中以 root 运行的容器 Containers running as root within the Linux VM

With Docker Desktop, the Docker daemon and containers run in a lightweight Linux VM managed by Docker. This means that although containers run by default as `root`, this doesn't grant `root` access to the Mac host machine. The Linux VM serves as a security boundary and limits what resources can be accessed from the host. Any directories from the host bind mounted into Docker containers still retain their original permissions.

​	在 Docker Desktop 中，Docker 守护进程和容器运行在由 Docker 管理的轻量级 Linux 虚拟机中。这意味着，虽然容器默认以 `root` 运行，但这并不会授予 Mac 主机的 `root` 访问权限。Linux 虚拟机作为安全边界，限制了主机资源的访问权限。主机上绑定挂载到 Docker 容器中的目录仍保留其原始权限。

## 增强的容器隔离 Enhanced Container Isolation

In addition, Docker Desktop supports [Enhanced Container Isolation mode]({{< ref "/manuals/Security/Foradmins/HardenedDockerDesktop/EnhancedContainerIsolation" >}}) (ECI), available to Business customers only, which further secures containers without impacting developer workflows.

​	此外，Docker Desktop 支持[增强的容器隔离模式]({{< ref "/manuals/Security/Foradmins/HardenedDockerDesktop/EnhancedContainerIsolation" >}})（ECI），仅对企业客户提供，该模式进一步保护容器，同时不会影响开发者的工作流。

ECI automatically runs all containers within a Linux user-namespace, such that root in the container is mapped to an unprivileged user inside the Docker Desktop VM. ECI uses this and other advanced techniques to further secure containers within the Docker Desktop Linux VM, such that they are further isolated from the Docker daemon and other services running inside the VM.

​	ECI 自动在 Linux 用户命名空间中运行所有容器，使得容器内的 root 用户被映射为 Docker Desktop 虚拟机内的非特权用户。ECI 利用此及其他高级技术进一步保护 Docker Desktop Linux 虚拟机中的容器，确保它们与 Docker 守护进程及虚拟机内其他服务的进一步隔离。
