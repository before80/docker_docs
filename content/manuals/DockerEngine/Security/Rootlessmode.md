+++
title = "Rootless 模式"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/security/rootless/](https://docs.docker.com/engine/security/rootless/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Rootless mode - Rootless 模式

Rootless mode allows running the Docker daemon and containers as a non-root user to mitigate potential vulnerabilities in the daemon and the container runtime.

​	Rootless 模式允许以非 root 用户身份运行 Docker 守护进程和容器，以减轻守护进程和容器运行时的潜在漏洞风险。

Rootless mode does not require root privileges even during the installation of the Docker daemon, as long as the [prerequisites](https://docs.docker.com/engine/security/rootless/#prerequisites) are met.

​	Rootless 模式在 Docker 守护进程的安装过程中也不需要 root 权限，只要满足 [先决条件](https://docs.docker.com/engine/security/rootless/#prerequisites)。

## 工作原理 How it works

Rootless mode executes the Docker daemon and containers inside a user namespace. This is very similar to [`userns-remap` mode]({{< ref "/manuals/DockerEngine/Security/Isolatecontainerswithausernamespace" >}}), except that with `userns-remap` mode, the daemon itself is running with root privileges, whereas in rootless mode, both the daemon and the container are running without root privileges.

​	Rootless 模式在用户命名空间内执行 Docker 守护进程和容器。这与 [`userns-remap` 模式]({{< ref "/manuals/DockerEngine/Security/Isolatecontainerswithausernamespace" >}})非常相似，但不同之处在于 `userns-remap` 模式下守护进程本身以 root 权限运行，而在 Rootless 模式下，守护进程和容器均无 root 权限运行。

Rootless mode does not use binaries with `SETUID` bits or file capabilities, except `newuidmap` and `newgidmap`, which are needed to allow multiple UIDs/GIDs to be used in the user namespace.

​	Rootless 模式不使用带有 `SETUID` 位或文件功能的二进制文件，除了 `newuidmap` 和 `newgidmap`，它们用于在用户命名空间中允许使用多个 UID/GID。

## 前置条件 Prerequisites

- You must install `newuidmap` and `newgidmap` on the host. These commands are provided by the `uidmap` package on most distros.
  - 必须在主机上安装 `newuidmap` 和 `newgidmap`。在大多数发行版中，这些命令由 `uidmap` 包提供。

- `/etc/subuid` and `/etc/subgid` should contain at least 65,536 subordinate UIDs/GIDs for the user. In the following example, the user `testuser` has 65,536 subordinate UIDs/GIDs (231072-296607).
  - `/etc/subuid` 和 `/etc/subgid` 应为用户配置至少 65,536 个下属的 UID/GID。以下示例中，用户 `testuser` 拥有 65,536 个下属 UID/GID（231072-296607）。



```console
$ id -u
1001
$ whoami
testuser
$ grep ^$(whoami): /etc/subuid
testuser:231072:65536
$ grep ^$(whoami): /etc/subgid
testuser:231072:65536
```

### 特定发行版的提示 Distribution-specific hint

> **Tip**
>
> 
>
> We recommend that you use the Ubuntu kernel.
>
> ​	推荐使用 Ubuntu 内核。

{{< tabpane text=true persist=disabled >}}

{{% tab header="Ubuntu" %}}

- Install `dbus-user-session` package if not installed. Run `sudo apt-get install -y dbus-user-session` and relogin. 如果尚未安装 `dbus-user-session` 包，请运行 `sudo apt-get install -y dbus-user-session` 并重新登录。

- Install `uidmap` package if not installed. Run `sudo apt-get install -y uidmap`. 如果尚未安装 `uidmap` 包，请运行 `sudo apt-get install -y uidmap`。

- If running in a terminal where the user was not directly logged into, you will need to install `systemd-container` with `sudo apt-get install -y systemd-container`, then switch to TheUser with the command `sudo machinectl shell TheUser@`. 如果在未直接登录的终端中运行，需安装 `systemd-container`，运行 `sudo apt-get install -y systemd-container`，然后使用命令 `sudo machinectl shell TheUser@` 切换至用户 TheUser。

- `overlay2` storage driver is enabled by default ( [Ubuntu-specific kernel patch](https://kernel.ubuntu.com/git/ubuntu/ubuntu-bionic.git/commit/fs/overlayfs?id=3b7da90f28fe1ed4b79ef2d994c81efbc58f1144)). 默认启用 `overlay2` 存储驱动（[Ubuntu 专用内核补丁](https://kernel.ubuntu.com/git/ubuntu/ubuntu-bionic.git/commit/fs/overlayfs?id=3b7da90f28fe1ed4b79ef2d994c81efbc58f1144)）。

- Ubuntu 24.04 and later enables restricted unprivileged user namespaces by default, which prevents unprivileged processes in creating user namespaces unless an AppArmor profile is configured to allow programs to use unprivileged user namespaces. Ubuntu 24.04 及更高版本默认启用限制未授权的用户命名空间，除非为程序配置了 AppArmor 配置文件，否则不允许非特权进程创建用户命名空间。

  If you install `docker-ce-rootless-extras` using the deb package (`apt-get install docker-ce-rootless-extras`), then the AppArmor profile for `rootlesskit` is already bundled with the `apparmor` deb package. With this installation method, you don't need to add any manual the AppArmor configuration. If you install the rootless extras using the [installation script](https://get.docker.com/rootless), however, you must add an AppArmor profile for `rootlesskit` manually: 如果使用 deb 包 (`apt-get install docker-ce-rootless-extras`) 安装 `docker-ce-rootless-extras`，则 `rootlesskit` 的 AppArmor 配置文件已包含在 `apparmor` deb 包中。使用此安装方法无需手动添加 AppArmor 配置文件。但如果使用[安装脚本](https://get.docker.com/rootless) 安装 rootless extras，则必须手动为 `rootlesskit` 添加 AppArmor 配置文件：

  1. Create and install the currently logged-in user's AppArmor profile:

     创建并安装当前登录用户的 AppArmor 配置文件：

     ```console
     $ filename=$(echo $HOME/bin/rootlesskit | sed -e s@^/@@ -e s@/@.@g)
     $ cat <<EOF > ~/${filename}
     abi <abi/4.0>,
     include <tunables/global>
     
     "$HOME/bin/rootlesskit" flags=(unconfined) {
       userns,
     
       include if exists <local/${filename}>
     }
     EOF
     $ sudo mv ~/${filename} /etc/apparmor.d/${filename}
     ```

  2. Restart AppArmor.

     重启 AppArmor。

     ```console
     $ systemctl restart apparmor.service
     ```

{{% /tab  %}}

{{% tab header="Debian GNU/Linux" %}}

- Install `dbus-user-session` package if not installed. Run `sudo apt-get install -y dbus-user-session` and relogin.

  - 如果尚未安装 `dbus-user-session` 包，请运行 `sudo apt-get install -y dbus-user-session` 并重新登录。

- For Debian 10, add `kernel.unprivileged_userns_clone=1` to `/etc/sysctl.conf` (or `/etc/sysctl.d`) and run `sudo sysctl --system`. This step is not required on Debian 11.

  - 对于 Debian 10，添加 `kernel.unprivileged_userns_clone=1` 到 `/etc/sysctl.conf`（或 `/etc/sysctl.d`），并运行 `sudo sysctl --system`。在 Debian 11 上不需要此步骤。

- For Debian 11, installing `fuse-overlayfs` is recommended. Run `sudo apt-get install -y fuse-overlayfs`. This step is not required on Debian 12.

  - 对于 Debian 11，推荐安装 `fuse-overlayfs`，运行 `sudo apt-get install -y fuse-overlayfs`。在 Debian 12 上不需要此步骤。

- Rootless docker requires version of `slirp4netns` greater than `v0.4.0` (when `vpnkit` is not installed). Check you have this with

  Rootless Docker 需要 `slirp4netns` 版本大于 `v0.4.0`（在未安装 `vpnkit` 时）。检查版本：

  ```console
  $ slirp4netns --version
  ```

  If you do not have this download and install with `sudo apt-get install -y slirp4netns` or download the latest [release](https://github.com/rootless-containers/slirp4netns/releases).

  ​	如果没有该版本，使用 `sudo apt-get install -y slirp4netns` 下载并安装，或从[最新发布](https://github.com/rootless-containers/slirp4netns/releases)中下载。

{{% /tab  %}}

{{% tab header="Arch Linux " %}}

- Installing `fuse-overlayfs` is recommended. Run `sudo pacman -S fuse-overlayfs`.
  - 推荐安装 `fuse-overlayfs`，运行 `sudo pacman -S fuse-overlayfs`。

- Add `kernel.unprivileged_userns_clone=1` to `/etc/sysctl.conf` (or `/etc/sysctl.d`) and run `sudo sysctl --system`
  - 添加 `kernel.unprivileged_userns_clone=1` 到 `/etc/sysctl.conf`（或 `/etc/sysctl.d`），并运行 `sudo sysctl --system`。


{{% /tab  %}}

{{% tab header="openSUSE and SLES " %}}

- For openSUSE 15 and SLES 15, Installing `fuse-overlayfs` is recommended. Run `sudo zypper install -y fuse-overlayfs`. This step is not required on openSUSE Tumbleweed.
  - 对于 openSUSE 15 和 SLES 15，推荐安装 `fuse-overlayfs`。运行 `sudo zypper install -y fuse-overlayfs`。在 openSUSE Tumbleweed 上不需要此步骤。

- `sudo modprobe ip_tables iptable_mangle iptable_nat iptable_filter` is required. This might be required on other distros as well depending on the configuration.
  - 运行 `sudo modprobe ip_tables iptable_mangle iptable_nat iptable_filter`。其他发行版上根据配置也可能需要此步骤。

- Known to work on openSUSE 15 and SLES 15.
  - 已知在 openSUSE 15 和 SLES 15 上工作。


{{% /tab  %}}

{{% tab header="CentOS, RHEL, and Fedora" %}}

For RHEL 8 and similar distributions, installing `fuse-overlayfs` is recommended. Run `sudo dnf install -y fuse-overlayfs`. This step is not required on RHEL 9 and similar distributions.

​	对于 RHEL 8 及类似发行版，推荐安装 `fuse-overlayfs`，运行 `sudo dnf install -y fuse-overlayfs`。在 RHEL 9 及类似发行版上不需要此步骤。

You might need `sudo dnf install -y iptables`.

​	可能需要运行 `sudo dnf install -y iptables`。

{{% /tab  %}}

{{< /tabpane >}}

------

## 已知限制 Known limitations

- Only the following storage drivers are supported:  仅支持以下存储驱动程序：
  - `overlay2` (only if running with kernel 5.11 or later, or Ubuntu-flavored kernel)
    - `overlay2`（仅在内核 5.11 或更新版本，或 Ubuntu 专用内核中运行时支持）
  - `fuse-overlayfs` (only if running with kernel 4.18 or later, and `fuse-overlayfs` is installed)
    - `fuse-overlayfs`（仅在内核 4.18 或更新版本且安装了 `fuse-overlayfs` 时支持）
  - `btrfs` (only if running with kernel 4.18 or later, or `~/.local/share/docker` is mounted with `user_subvol_rm_allowed` mount option)
    - `btrfs`（仅在内核 4.18 或更新版本，或 `~/.local/share/docker` 挂载为 `user_subvol_rm_allowed` 选项时支持）
  - `vfs`
- Cgroup is supported only when running with cgroup v2 and systemd. See [Limiting resources](https://docs.docker.com/engine/security/rootless/#limiting-resources).

  - 仅在运行 cgroup v2 和 systemd 时支持 Cgroup。参见 [资源限制](https://docs.docker.com/engine/security/rootless/#limiting-resources)。

- Following features are not supported: 不支持以下功能：
  - AppArmor
  - Checkpoint
  - Overlay network
  - Exposing SCTP ports
- To use the `ping` command, see [Routing ping packets](https://docs.docker.com/engine/security/rootless/#routing-ping-packets).

  - 要使用 `ping` 命令，请参见 [路由 ping 数据包](https://docs.docker.com/engine/security/rootless/#routing-ping-packets)。

- To expose privileged TCP/UDP ports (< 1024), see [Exposing privileged ports](https://docs.docker.com/engine/security/rootless/#exposing-privileged-ports).

  - 要暴露特权 TCP/UDP 端口（< 1024），请参见 [暴露特权端口](https://docs.docker.com/engine/security/rootless/#exposing-privileged-ports)。

- `IPAddress` shown in `docker inspect` is namespaced inside RootlessKit's network namespace. This means the IP address is not reachable from the host without `nsenter`-ing into the network namespace.

  - `docker inspect` 中显示的 `IPAddress` 在 RootlessKit 的网络命名空间内进行了命名空间隔离。这意味着该 IP 地址无法从主机访问，除非通过 `nsenter` 进入网络命名空间。

- Host network (`docker run --net=host`) is also namespaced inside RootlessKit.

  - 主机网络（`docker run --net=host`）也在 RootlessKit 内进行了命名空间隔离。

- NFS mounts as the docker "data-root" is not supported. This limitation is not specific to rootless mode.

  - 不支持将 NFS 挂载为 Docker "data-root"。此限制并非 Rootless 模式特有。

  

## Install

> **Note**
>
> 
>
> If the system-wide Docker daemon is already running, consider disabling it:
>
> ​	如果系统级 Docker 守护进程已经运行，建议将其禁用：
>
> ```console
> $ sudo systemctl disable --now docker.service docker.socket
> $ sudo rm /var/run/docker.sock
> ```
>
> Should you choose not to shut down the `docker` service and socket, you will need to use the `--force` parameter in the next section. There are no known issues, but until you shutdown and disable you're still running rootful Docker.
>
> ​	如果不禁用 `docker` 服务和 socket，则在下一节中需要使用 `--force` 参数。目前没有已知问题，但在禁用之前，仍在运行 root 模式的 Docker。



{{< tabpane text=true persist=disabled >}}

{{% tab header="With packages (RPM/DEB)" %}}

If you installed Docker 20.10 or later with [RPM/DEB packages]({{< ref "/manuals/DockerEngine/Install" >}}), you should have `dockerd-rootless-setuptool.sh` in `/usr/bin`.

​	如果您使用 RPM/DEB 包 安装了 Docker 20.10 或更高版本，系统中应该有 `/usr/bin` 目录下的 `dockerd-rootless-setuptool.sh` 文件。

Run `dockerd-rootless-setuptool.sh install` as a non-root user to set up the daemon:

​	作为非 root 用户运行 `dockerd-rootless-setuptool.sh install` 以设置守护进程：



```console
$ dockerd-rootless-setuptool.sh install
[INFO] Creating /home/testuser/.config/systemd/user/docker.service
...
[INFO] Installed docker.service successfully.
[INFO] To control docker.service, run: `systemctl --user (start|stop|restart) docker.service`
[INFO] To run docker.service on system startup, run: `sudo loginctl enable-linger testuser`

[INFO] Make sure the following environment variables are set (or add them to ~/.bashrc):

export PATH=/usr/bin:$PATH
export DOCKER_HOST=unix:///run/user/1000/docker.sock
```

If `dockerd-rootless-setuptool.sh` is not present, you may need to install the `docker-ce-rootless-extras` package manually, e.g.,

​	如果 `dockerd-rootless-setuptool.sh` 不存在，您可能需要手动安装 `docker-ce-rootless-extras` 包，例如：



```console
$ sudo apt-get install -y docker-ce-rootless-extras
```

{{% /tab  %}}

{{% tab header=" Without packages" %}}

If you do not have permission to run package managers like `apt-get` and `dnf`, consider using the installation script available at https://get.docker.com/rootless. Since static packages are not available for `s390x`, hence it is not supported for `s390x`.

​	如果您没有权限运行诸如 `apt-get` 和 `dnf` 的包管理器，可以考虑使用 https://get.docker.com/rootless 上的安装脚本。由于静态包不支持 `s390x`，因此 `s390x` 不支持该脚本。

```console
$ curl -fsSL https://get.docker.com/rootless | sh
...
[INFO] Creating /home/testuser/.config/systemd/user/docker.service
...
[INFO] Installed docker.service successfully.
[INFO] To control docker.service, run: `systemctl --user (start|stop|restart) docker.service`
[INFO] To run docker.service on system startup, run: `sudo loginctl enable-linger testuser`

[INFO] Make sure the following environment variables are set (or add them to ~/.bashrc):

export PATH=/home/testuser/bin:$PATH
export DOCKER_HOST=unix:///run/user/1000/docker.sock
```

The binaries will be installed at `~/bin`.

​	二进制文件将安装在 `~/bin` 目录下。

{{% /tab  %}}

{{< /tabpane >}}



------

See [Troubleshooting](https://docs.docker.com/engine/security/rootless/#troubleshooting) if you faced an error.

​	遇到错误时请参见[疑难解答](https://docs.docker.com/engine/security/rootless/#troubleshooting)。

## Uninstall

To remove the systemd service of the Docker daemon, run `dockerd-rootless-setuptool.sh uninstall`:

​	要移除 Docker 守护进程的 systemd 服务，运行 `dockerd-rootless-setuptool.sh uninstall`：



```console
$ dockerd-rootless-setuptool.sh uninstall
+ systemctl --user stop docker.service
+ systemctl --user disable docker.service
Removed /home/testuser/.config/systemd/user/default.target.wants/docker.service.
[INFO] Uninstalled docker.service
[INFO] This uninstallation tool does NOT remove Docker binaries and data.
[INFO] To remove data, run: `/usr/bin/rootlesskit rm -rf /home/testuser/.local/share/docker`
```

Unset environment variables PATH and DOCKER_HOST if you have added them to `~/.bashrc`.

​	如果您在 `~/.bashrc` 中添加了环境变量 PATH 和 DOCKER_HOST，请将其取消设置。

To remove the data directory, run `rootlesskit rm -rf ~/.local/share/docker`.

​	要删除数据目录，运行 `rootlesskit rm -rf ~/.local/share/docker`。

To remove the binaries, remove `docker-ce-rootless-extras` package if you installed Docker with package managers. If you installed Docker with https://get.docker.com/rootless ( [Install without packages](https://docs.docker.com/engine/security/rootless/#install)), remove the binary files under `~/bin`:

​	如果您通过包管理器安装了 Docker，删除二进制文件需要卸载 `docker-ce-rootless-extras` 包。如果您通过 https://get.docker.com/rootless ( [无包安装](https://docs.docker.com/engine/security/rootless/#install) ) 安装 Docker，移除 `~/bin` 目录下的二进制文件：



```console
$ cd ~/bin
$ rm -f containerd containerd-shim containerd-shim-runc-v2 ctr docker docker-init docker-proxy dockerd dockerd-rootless-setuptool.sh dockerd-rootless.sh rootlesskit rootlesskit-docker-proxy runc vpnkit
```

## Usage

### Daemon

{{< tabpane text=true persist=disabled >}}

{{% tab header="With systemd (Highly recommended)" %}}

The systemd unit file is installed as `~/.config/systemd/user/docker.service`.

​	systemd 单元文件已安装为 `~/.config/systemd/user/docker.service`。

Use `systemctl --user` to manage the lifecycle of the daemo

​	使用 `systemctl --user` 管理守护进程的生命周期：

```console
$ systemctl --user start docker
```

To launch the daemon on system startup, enable the systemd service and lingering:

​	要在系统启动时启动守护进程，启用 systemd 服务和持久化：



```console
$ systemctl --user enable docker
$ sudo loginctl enable-linger $(whoami)
```

Starting Rootless Docker as a systemd-wide service (`/etc/systemd/system/docker.service`) is not supported, even with the `User=` directive.

​	不支持以 systemd 范围服务 (`/etc/systemd/system/docker.service`) 的方式启动 Rootless Docker，即使使用了 `User=` 指令。

{{% /tab  %}}

{{% tab header=" Without systemd" %}}

To run the daemon directly without systemd, you need to run `dockerd-rootless.sh` instead of `dockerd`.

​	在没有 systemd 的情况下直接运行守护进程，您需要运行 `dockerd-rootless.sh` 而不是 `dockerd`。

The following environment variables must be set:

​	必须设置以下环境变量：

- `$HOME`: the home directory 
  - `$HOME`：主目录
- `$XDG_RUNTIME_DIR`: an ephemeral directory that is only accessible by the expected user, e,g, `~/.docker/run`. The directory should be removed on every host shutdown. The directory can be on tmpfs, however, should not be under `/tmp`. Locating this directory under `/tmp` might be vulnerable to TOCTOU attack.
  - `$XDG_RUNTIME_DIR`：仅对预期用户可访问的临时目录，例如 `~/.docker/run`。应在每次主机关机时删除该目录。该目录可以放置在 tmpfs 中，但不应位于 `/tmp` 下，因为放在 `/tmp` 下可能容易受到 TOCTOU 攻击。

{{% /tab  %}}

{{< /tabpane >}}

------

Remarks about directory paths:

​	关于目录路径的说明：

- The socket path is set to `$XDG_RUNTIME_DIR/docker.sock` by default. `$XDG_RUNTIME_DIR` is typically set to `/run/user/$UID`.
  - 默认情况下，socket 路径设置为 `$XDG_RUNTIME_DIR/docker.sock`。`$XDG_RUNTIME_DIR` 通常设置为 `/run/user/$UID`。

- The data dir is set to `~/.local/share/docker` by default. The data dir should not be on NFS.

  - 数据目录默认为 `~/.local/share/docker`。数据目录不应位于 NFS 上。

- The daemon config dir is set to `~/.config/docker` by default. This directory is different from `~/.docker` that is used by the client.

  - 守护进程配置目录默认设置为 `~/.config/docker`，与客户端使用的 `~/.docker` 不同。

  

### Client

You need to specify either the socket path or the CLI context explicitly.

​	您需要明确指定 socket 路径或 CLI 上下文。

To specify the socket path using `$DOCKER_HOST`:

​	使用 `$DOCKER_HOST` 指定 socket 路径：



```console
$ export DOCKER_HOST=unix://$XDG_RUNTIME_DIR/docker.sock
$ docker run -d -p 8080:80 nginx
```

To specify the CLI context using `docker context`:

​	使用 `docker context` 指定 CLI 上下文：

```console
$ docker context use rootless
rootless
Current context is now "rootless"
$ docker run -d -p 8080:80 nginx
```

## 最佳实践 Best practices

### Rootless Docker in Docker

To run Rootless Docker inside "rootful" Docker, use the `docker:<version>-dind-rootless` image instead of `docker:<version>-dind`.

​	要在“有 root 权限”的 Docker 中运行 Rootless Docker，请使用 `docker:<版本>-dind-rootless` 镜像而不是 `docker:<版本>-dind`。

```console
$ docker run -d --name dind-rootless --privileged docker:25.0-dind-rootless
```

The `docker:<version>-dind-rootless` image runs as a non-root user (UID 1000). However, `--privileged` is required for disabling seccomp, AppArmor, and mount masks.

​	`docker:<版本>-dind-rootless` 镜像以非 root 用户（UID 1000）身份运行。但是，需要 `--privileged` 来禁用 seccomp、AppArmor 和挂载掩码。

### 通过 TCP 暴露 Docker API socket - Expose Docker API socket through TCP

To expose the Docker API socket through TCP, you need to launch `dockerd-rootless.sh` with `DOCKERD_ROOTLESS_ROOTLESSKIT_FLAGS="-p 0.0.0.0:2376:2376/tcp"`.

​	要通过 TCP 暴露 Docker API socket，您需要以 `DOCKERD_ROOTLESS_ROOTLESSKIT_FLAGS="-p 0.0.0.0:2376:2376/tcp"` 启动 `dockerd-rootless.sh`。



```console
$ DOCKERD_ROOTLESS_ROOTLESSKIT_FLAGS="-p 0.0.0.0:2376:2376/tcp" \
  dockerd-rootless.sh \
  -H tcp://0.0.0.0:2376 \
  --tlsverify --tlscacert=ca.pem --tlscert=cert.pem --tlskey=key.pem
```

### 通过 SSH 暴露 Docker API socket - Expose Docker API socket through SSH

To expose the Docker API socket through SSH, you need to make sure `$DOCKER_HOST` is set on the remote host.

​	要通过 SSH 暴露 Docker API socket，您需要确保在远程主机上设置了 `$DOCKER_HOST`。

​	

```console
$ ssh -l REMOTEUSER REMOTEHOST 'echo $DOCKER_HOST'
unix:///run/user/1001/docker.sock
$ docker -H ssh://REMOTEUSER@REMOTEHOST run ...
```

### 路由 ping 包 Routing ping packets

On some distributions, `ping` does not work by default.

​	在某些发行版中，默认情况下 `ping` 不工作。

Add `net.ipv4.ping_group_range = 0 2147483647` to `/etc/sysctl.conf` (or `/etc/sysctl.d`) and run `sudo sysctl --system` to allow using `ping`.

​	将 `net.ipv4.ping_group_range = 0 2147483647` 添加到 `/etc/sysctl.conf`（或 `/etc/sysctl.d`），并运行 `sudo sysctl --system` 以允许使用 `ping`。

### 暴露特权端口 Exposing privileged ports

To expose privileged ports (< 1024), set `CAP_NET_BIND_SERVICE` on `rootlesskit` binary and restart the daemon.

​	要暴露特权端口（小于 1024），请在 `rootlesskit` 二进制文件上设置 `CAP_NET_BIND_SERVICE` 并重新启动守护进程。



```console
$ sudo setcap cap_net_bind_service=ep $(which rootlesskit)
$ systemctl --user restart docker
```

Or add `net.ipv4.ip_unprivileged_port_start=0` to `/etc/sysctl.conf` (or `/etc/sysctl.d`) and run `sudo sysctl --system`.

​	或者将 `net.ipv4.ip_unprivileged_port_start=0` 添加到 `/etc/sysctl.conf`（或 `/etc/sysctl.d`），并运行 `sudo sysctl --system`。

### 限制资源 Limiting resources

Limiting resources with cgroup-related `docker run` flags such as `--cpus`, `--memory`, `--pids-limit` is supported only when running with cgroup v2 and systemd. See [Changing cgroup version]({{< ref "/manuals/DockerEngine/Containers/Runtimemetrics" >}}) to enable cgroup v2.

​	使用 cgroup 相关的 `docker run` 参数（如 `--cpus`、`--memory`、`--pids-limit`）来限制资源仅在使用 cgroup v2 和 systemd 时受支持。参见 [更改 cgroup 版本](https://docs.docker.com/engine/security/rootless/#changing-cgroup-version) 启用 cgroup v2。

If `docker info` shows `none` as `Cgroup Driver`, the conditions are not satisfied. When these conditions are not satisfied, rootless mode ignores the cgroup-related `docker run` flags. See [Limiting resources without cgroup](https://docs.docker.com/engine/security/rootless/#limiting-resources-without-cgroup) for workarounds.

​	如果 `docker info` 显示 `Cgroup Driver` 为 `none`，则不满足条件。在此情况下，rootless 模式会忽略 cgroup 相关的 `docker run` 参数。参见[无 cgroup 限制资源](https://docs.docker.com/engine/security/rootless/#limiting-resources-without-cgroup) 以获取替代方案。

If `docker info` shows `systemd` as `Cgroup Driver`, the conditions are satisfied. However, typically, only `memory` and `pids` controllers are delegated to non-root users by default.

​	如果 `docker info` 显示 `Cgroup Driver` 为 `systemd`，则条件满足。然而，默认情况下仅将 `memory` 和 `pids` 控制器委派给非 root 用户。



```console
$ cat /sys/fs/cgroup/user.slice/user-$(id -u).slice/user@$(id -u).service/cgroup.controllers
memory pids
```

To allow delegation of all controllers, you need to change the systemd configuration as follows:

​	要允许委派所有控制器，您需要按如下方式更改 systemd 配置：



```console
# mkdir -p /etc/systemd/system/user@.service.d
# cat > /etc/systemd/system/user@.service.d/delegate.conf << EOF
[Service]
Delegate=cpu cpuset io memory pids
EOF
# systemctl daemon-reload
```

> **Note**
>
> 
>
> Delegating `cpuset` requires systemd 244 or later.
>
> ​	委派 `cpuset` 需要 systemd 244 或更高版本。

#### 无 cgroup 的资源限制 Limiting resources without cgroup

Even when cgroup is not available, you can still use the traditional `ulimit` and [`cpulimit`](https://github.com/opsengine/cpulimit), though they work in process-granularity rather than in container-granularity, and can be arbitrarily disabled by the container process.

​	即使 cgroup 不可用，您仍可以使用传统的 `ulimit` 和 [`cpulimit`](https://github.com/opsengine/cpulimit)，不过它们以进程为粒度，而非容器粒度，并且可以被容器进程任意禁用。

For example:

- To limit CPU usage to 0.5 cores (similar to `docker run --cpus 0.5`): `docker run <IMAGE> cpulimit --limit=50 --include-children <COMMAND>`
  - 将 CPU 使用限制为 0.5 核（类似 `docker run --cpus 0.5`）：`docker run <IMAGE> cpulimit --limit=50 --include-children <COMMAND>`

- To limit max VSZ to 64MiB (similar to `docker run --memory 64m`): `docker run <IMAGE> sh -c "ulimit -v 65536; <COMMAND>"`
  - 将最大 VSZ 限制为 64MiB（类似 `docker run --memory 64m`）：`docker run <IMAGE> sh -c "ulimit -v 65536; <COMMAND>"`

- To limit max number of processes to 100 per namespaced UID 2000 (similar to `docker run --pids-limit=100`): `docker run --user 2000 --ulimit nproc=100 <IMAGE> <COMMAND>`
  - 将最大进程数限制为每命名空间 UID 2000 的 100 个（类似 `docker run --pids-limit=100`）：`docker run --user 2000 --ulimit nproc=100 <IMAGE> <COMMAND>`

## 疑难解答 Troubleshooting

### 当系统存在 systemd 时无法安装 Unable to install with systemd when systemd is present on the system



```console
$ dockerd-rootless-setuptool.sh install
[INFO] systemd not detected, dockerd-rootless.sh needs to be started manually:
...
```

`rootlesskit` cannot detect systemd properly if you switch to your user via `sudo su`. For users which cannot be logged-in, you must use the `machinectl` command which is part of the `systemd-container` package. After installing `systemd-container` switch to `myuser` with the following command:

​	如果通过 `sudo su` 切换到用户，`rootlesskit` 无法正确检测到 systemd。对于无法登录的用户，必须使用 `systemd-container` 包中的 `machinectl` 命令。安装 `systemd-container` 后，使用以下命令切换至 `myuser`：



```console
$ sudo machinectl shell myuser@
```

Where `myuser@` is your desired username and @ signifies this machine.

​	其中 `myuser@` 是您期望的用户名，@ 表示这台机器。

### 启动 Docker 守护进程时的错误 Errors when starting the Docker daemon

**[rootlesskit] 错误：无法启动子进程：fork/exec /proc/self/exe：操作不允许 [rootlesskit:parent] error: failed to start the child: fork/exec /proc/self/exe: operation not permitted**

This error occurs mostly when the value of `/proc/sys/kernel/unprivileged_userns_clone` is set to 0:

​	此错误通常发生在 `/proc/sys/kernel/unprivileged_userns_clone` 的值为 0 时：

```console
$ cat /proc/sys/kernel/unprivileged_userns_clone
0
```

To fix this issue, add `kernel.unprivileged_userns_clone=1` to `/etc/sysctl.conf` (or `/etc/sysctl.d`) and run `sudo sysctl --system`.

​	解决此问题，添加 `kernel.unprivileged_userns_clone=1` 到 `/etc/sysctl.conf`（或 `/etc/sysctl.d`），并运行 `sudo sysctl --system`。

**[rootlesskit] 错误：无法启动子进程：fork/exec /proc/self/exe：设备上无空间 [rootlesskit:parent] error: failed to start the child: fork/exec /proc/self/exe: no space left on device**

This error occurs mostly when the value of `/proc/sys/user/max_user_namespaces` is too small:

​	此错误通常在 `/proc/sys/user/max_user_namespaces` 的值太小时发生：



```console
$ cat /proc/sys/user/max_user_namespaces
0
```

To fix this issue, add `user.max_user_namespaces=28633` to `/etc/sysctl.conf` (or `/etc/sysctl.d`) and run `sudo sysctl --system`.

​	解决此问题，添加 `user.max_user_namespaces=28633` 到 `/etc/sysctl.conf`（或 `/etc/sysctl.d`），并运行 `sudo sysctl --system`。

**[rootlesskit] 错误：无法设置 UID/GID 映射：无法计算 uid/gid 映射：找不到用户 1001 ("testuser") 的子 UID 范围 [rootlesskit:parent] error: failed to setup UID/GID map: failed to compute uid/gid map: No subuid ranges found for user 1001 ("testuser")**

This error occurs when `/etc/subuid` and `/etc/subgid` are not configured. See [Prerequisites](https://docs.docker.com/engine/security/rootless/#prerequisites).

​	当 `/etc/subuid` 和 `/etc/subgid` 未配置时出现此错误。参见[先决条件](https://docs.docker.com/engine/security/rootless/#prerequisites)。

**无法获取 XDG_RUNTIME_DIR - could not get XDG_RUNTIME_DIR**

This error occurs when `$XDG_RUNTIME_DIR` is not set.

​	当 `$XDG_RUNTIME_DIR` 未设置时出现此错误。

On a non-systemd host, you need to create a directory and then set the path:

​	在非 systemd 主机上，需要创建一个目录并设置路径：



```console
$ export XDG_RUNTIME_DIR=$HOME/.docker/xrd
$ rm -rf $XDG_RUNTIME_DIR
$ mkdir -p $XDG_RUNTIME_DIR
$ dockerd-rootless.sh
```

> **Note**
>
> 
>
> You must remove the directory every time you log out.
>
> ​	每次注销后必须删除此目录。

On a systemd host, log into the host using `pam_systemd` (see below). The value is automatically set to `/run/user/$UID` and cleaned up on every logout.

​	在 systemd 主机上，通过 `pam_systemd` 登录主机（见下文）。该值会自动设置为 `/run/user/$UID`，并在每次注销时清理。

**`systemctl --user` 失败，显示 "Failed to connect to bus: No such file or directory" `systemctl --user` fails with "Failed to connect to bus: No such file or directory"**

This error occurs mostly when you switch from the root user to an non-root user with `sudo`:

​	此错误通常在通过 `sudo` 从 root 用户切换到非 root 用户时发生：



```console
# sudo -iu testuser
$ systemctl --user start docker
Failed to connect to bus: No such file or directory
```

Instead of `sudo -iu <USERNAME>`, you need to log in using `pam_systemd`. For example:

​	替代方法是使用 `pam_systemd` 登录。例如：

- Log in through the graphic console 通过图形界面登录

- `ssh <USERNAME>@localhost`
- `machinectl shell <USERNAME>@`

**守护进程未自动启动 The daemon does not start up automatically**

You need `sudo loginctl enable-linger $(whoami)` to enable the daemon to start up automatically. See [Usage](https://docs.docker.com/engine/security/rootless/#usage).

​	您需要使用 `sudo loginctl enable-linger $(whoami)` 使守护进程自动启动。参见[使用方法](https://docs.docker.com/engine/security/rootless/#usage)。

**iptables 失败：iptables -t nat -N DOCKER: Fatal: can't open lock file /run/xtables.lock: Permission denied - iptables failed: iptables -t nat -N DOCKER: Fatal: can't open lock file /run/xtables.lock: Permission denied**

This error may happen with an older version of Docker when SELinux is enabled on the host.

​	当主机启用了 SELinux 且 Docker 版本较旧时，可能会发生此错误。

The issue has been fixed in Docker 20.10.8. A known workaround for older version of Docker is to run the following commands to disable SELinux for `iptables`:

​	在 Docker 20.10.8 中已修复该问题。针对旧版 Docker 的已知解决方案是运行以下命令以为 `iptables` 禁用 SELinux：



```console
$ sudo dnf install -y policycoreutils-python-utils && sudo semanage permissive -a iptables_t
```

### `docker pull` errors

**docker: failed to register layer: Error processing tar file(exit status 1): lchown <FILE>: invalid argument**

This error occurs when the number of available entries in `/etc/subuid` or `/etc/subgid` is not sufficient. The number of entries required vary across images. However, 65,536 entries are sufficient for most images. See [Prerequisites](https://docs.docker.com/engine/security/rootless/#prerequisites).

​	此错误发生在 `/etc/subuid` 或 `/etc/subgid` 中可用条目数量不足时。所需的条目数量因镜像而异，但大多数镜像 65,536 条目就足够了。请参见[先决条件](https://docs.docker.com/engine/security/rootless/#prerequisites)。

**docker: failed to register layer: ApplyLayer exit status 1 stdout: stderr: lchown <FILE>: operation not permitted**

This error occurs mostly when `~/.local/share/docker` is located on NFS.

​	此错误通常发生在 `~/.local/share/docker` 位于 NFS 上时。

A workaround is to specify non-NFS `data-root` directory in `~/.config/docker/daemon.json` as follows:

​	一种解决方法是在 `~/.config/docker/daemon.json` 中指定非 NFS 的 `data-root` 目录，如下所示：



```json
{"data-root":"/somewhere-out-of-nfs"}
```

### `docker run` errors

**docker: Error response from daemon: OCI runtime create failed: ...: read unix @->/run/systemd/private: read: connection reset by peer: unknown.**

This error occurs on cgroup v2 hosts mostly when the dbus daemon is not running for the user.

​	此错误通常发生在 cgroup v2 主机上，并且 dbus 守护进程未为用户运行时。



```console
$ systemctl --user is-active dbus
inactive

$ docker run hello-world
docker: Error response from daemon: OCI runtime create failed: container_linux.go:380: starting container process caused: process_linux.go:385: applying cgroup configuration for process caused: error while starting unit "docker
-931c15729b5a968ce803784d04c7421f791d87e5ca1891f34387bb9f694c488e.scope" with properties [{Name:Description Value:"libcontainer container 931c15729b5a968ce803784d04c7421f791d87e5ca1891f34387bb9f694c488e"} {Name:Slice Value:"use
r.slice"} {Name:PIDs Value:@au [4529]} {Name:Delegate Value:true} {Name:MemoryAccounting Value:true} {Name:CPUAccounting Value:true} {Name:IOAccounting Value:true} {Name:TasksAccounting Value:true} {Name:DefaultDependencies Val
ue:false}]: read unix @->/run/systemd/private: read: connection reset by peer: unknown.
```

To fix the issue, run `sudo apt-get install -y dbus-user-session` or `sudo dnf install -y dbus-daemon`, and then relogin.

​	为解决此问题，运行 `sudo apt-get install -y dbus-user-session` 或 `sudo dnf install -y dbus-daemon`，然后重新登录。

If the error still occurs, try running `systemctl --user enable --now dbus` (without sudo).

​	如果错误仍然存在，请尝试运行 `systemctl --user enable --now dbus`（不使用 sudo）。

**`--cpus`, `--memory`, and `--pids-limit` are ignored**

This is an expected behavior on cgroup v1 mode. To use these flags, the host needs to be configured for enabling cgroup v2. For more information, see [Limiting resources](https://docs.docker.com/engine/security/rootless/#limiting-resources).

​	这是 cgroup v1 模式下的预期行为。要使用这些标志，主机需要配置以启用 cgroup v2。有关更多信息，请参见[限制资源](https://docs.docker.com/engine/security/rootless/#limiting-resources)。

### Networking errors

This section provides troubleshooting tips for networking in rootless mode.

​	本节提供了在 rootless 模式下进行网络故障排除的提示。

Networking in rootless mode is supported via network and port drivers in RootlessKit. Network performance and characteristics depend on the combination of network and port driver you use. If you're experiencing unexpected behavior or performance related to networking, review the following table which shows the configurations supported by RootlessKit, and how they compare:

​	在 rootless 模式下通过 RootlessKit 中的网络和端口驱动程序支持网络。网络性能和特性取决于所使用的网络和端口驱动程序组合。如果遇到与网络相关的意外行为或性能问题，请查看以下支持的 RootlessKit 配置表以及它们的比较：

| Network driver | Port driver    | Net throughput 网络吞吐量 | Port throughput 端口吞吐量 | Source IP propagation 源 IP 传播 | No SUID | Note                                                         |
| -------------- | -------------- | ------------------------- | -------------------------- | -------------------------------- | ------- | ------------------------------------------------------------ |
| `slirp4netns`  | `builtin`      | Slow                      | Fast ✅                     | ❌                                | ✅       | 典型设置的默认配置Default in a typical setup                 |
| `vpnkit`       | `builtin`      | Slow                      | Fast ✅                     | ❌                                | ✅       | 当未安装 `slirp4netns` 时的默认配置Default when `slirp4netns` isn't installed |
| `slirp4netns`  | `slirp4netns`  | Slow                      | Slow                       | ✅                                | ✅       |                                                              |
| `pasta`        | `implicit`     | Slow                      | Fast ✅                     | ✅                                | ✅       | 实验性功能；需要 2023_12_04 或更高版本的 pastaExperimental; Needs pasta version 2023_12_04 or later |
| `lxc-user-nic` | `builtin`      | Fast ✅                    | Fast ✅                     | ❌                                | ❌       | 实验性功能Experimental                                       |
| `bypass4netns` | `bypass4netns` | Fast ✅                    | Fast ✅                     | ✅                                | ✅       | **注意**：尚未集成到 RootlessKit 中，因为需要自定义 seccomp 配置文件**Note:** Not integrated to RootlessKit as it needs a custom seccomp profile |

For information about troubleshooting specific networking issues, see:

​	有关特定网络问题的故障排除信息，请参见：

- [`docker run -p` fails with `cannot expose privileged port`](https://docs.docker.com/engine/security/rootless/#docker-run--p-fails-with-cannot-expose-privileged-port)

- [Ping doesn't work](https://docs.docker.com/engine/security/rootless/#ping-doesnt-work)
- [`IPAddress` shown in `docker inspect` is unreachable - `docker inspect` 中显示的 `IPAddress` 不可达](https://docs.docker.com/engine/security/rootless/#ipaddress-shown-in-docker-inspect-is-unreachable)
- [`--net=host` doesn't listen ports on the host network namespace `--net=host` 不在主机网络命名空间中监听端口](https://docs.docker.com/engine/security/rootless/#--nethost-doesnt-listen-ports-on-the-host-network-namespace)
- [Newtork is slow 网络速度慢](https://docs.docker.com/engine/security/rootless/#network-is-slow)
- [`docker run -p` does not propagate source IP addresses](https://docs.docker.com/engine/security/rootless/#docker-run--p-does-not-propagate-source-ip-addresses)

#### `docker run -p` fails with `cannot expose privileged port`

`docker run -p` fails with this error when a privileged port (< 1024) is specified as the host port.

​	当主机端口指定为特权端口（< 1024）时，`docker run -p` 会出现此错误。



```console
$ docker run -p 80:80 nginx:alpine
docker: Error response from daemon: driver failed programming external connectivity on endpoint focused_swanson (9e2e139a9d8fc92b37c36edfa6214a6e986fa2028c0cc359812f685173fa6df7): Error starting userland proxy: error while calling PortManager.AddPort(): cannot expose privileged port 80, you might need to add "net.ipv4.ip_unprivileged_port_start=0" (currently 1024) to /etc/sysctl.conf, or set CAP_NET_BIND_SERVICE on rootlesskit binary, or choose a larger port number (>= 1024): listen tcp 0.0.0.0:80: bind: permission denied.
```

When you experience this error, consider using an unprivileged port instead. For example, 8080 instead of 80.

​	遇到此错误时，建议使用非特权端口。例如，将端口 80 改为 8080：



```console
$ docker run -p 8080:80 nginx:alpine
```

To allow exposing privileged ports, see [Exposing privileged ports](https://docs.docker.com/engine/security/rootless/#exposing-privileged-ports).

​	有关暴露特权端口的更多信息，请参见[暴露特权端口](https://docs.docker.com/engine/security/rootless/#exposing-privileged-ports)。

#### Ping doesn't work

Ping does not work when `/proc/sys/net/ipv4/ping_group_range` is set to `1 0`:

​	当 `/proc/sys/net/ipv4/ping_group_range` 设置为 `1 0` 时，Ping 不工作：



```console
$ cat /proc/sys/net/ipv4/ping_group_range
1       0
```

For details, see [Routing ping packets](https://docs.docker.com/engine/security/rootless/#routing-ping-packets).

​	有关详细信息，请参见[路由 ping 包](https://docs.docker.com/engine/security/rootless/#routing-ping-packets)。

#### `IPAddress` shown in `docker inspect` is unreachable

This is an expected behavior, as the daemon is namespaced inside RootlessKit's network namespace. Use `docker run -p` instead.

​	这是预期的行为，因为守护进程命名空间在 RootlessKit 的网络命名空间内。请使用 `docker run -p`。

#### `--net=host` doesn't listen ports on the host network namespace

This is an expected behavior, as the daemon is namespaced inside RootlessKit's network namespace. Use `docker run -p` instead.

​	这是预期的行为，因为守护进程命名空间在 RootlessKit 的网络命名空间内。请使用 `docker run -p`。

#### 网络速度慢 Network is slow

Docker with rootless mode uses [slirp4netns](https://github.com/rootless-containers/slirp4netns) as the default network stack if slirp4netns v0.4.0 or later is installed. If slirp4netns is not installed, Docker falls back to [VPNKit](https://github.com/moby/vpnkit). Installing slirp4netns may improve the network throughput.

​	在 rootless 模式下，如果已安装 `slirp4netns` v0.4.0 或更高版本，Docker 使用 [slirp4netns](https://github.com/rootless-containers/slirp4netns) 作为默认网络栈。如果未安装 `slirp4netns`，Docker 会回退到 [VPNKit](https://github.com/moby/vpnkit)。安装 `slirp4netns` 可能会提高网络吞吐量。

For more information about network drivers for RootlessKit, see [RootlessKit documentation](https://github.com/rootless-containers/rootlesskit/blob/v2.0.0/docs/network.md).

​	有关 RootlessKit 网络驱动的更多信息，请参见[RootlessKit 文档](https://github.com/rootless-containers/rootlesskit/blob/v2.0.0/docs/network.md)。

Also, changing MTU value may improve the throughput. The MTU value can be specified by creating `~/.config/systemd/user/docker.service.d/override.conf` with the following content:

​	此外，修改 MTU 值可能会提高吞吐量。可以通过创建 `~/.config/systemd/user/docker.service.d/override.conf` 并添加以下内容来指定 MTU 值：



```systemd
[Service]
Environment="DOCKERD_ROOTLESS_ROOTLESSKIT_MTU=INTEGER"
```

And then restart the daemon:

​	然后重启守护进程：



```console
$ systemctl --user daemon-reload
$ systemctl --user restart docker
```

#### `docker run -p` does not propagate source IP addresses

This is because Docker in rootless mode uses RootlessKit's `builtin` port driver by default, which doesn't support source IP propagation. To enable source IP propagation, you can:

​	在 rootless 模式下，Docker 默认使用 RootlessKit 的 `builtin` 端口驱动，不支持源 IP 传播。要启用源 IP 传播，可以：

- Use the `slirp4netns` RootlessKit port driver
  - 使用 `slirp4netns` 作为 RootlessKit 的端口驱动

- Use the `pasta` RootlessKit network driver, with the `implicit` port driver
  - 使用 `pasta` 作为 RootlessKit 的网络驱动，并使用 `implicit` 端口驱动


The `pasta` network driver is experimental, but provides improved throughput performance compared to the `slirp4netns` port driver. The `pasta` driver requires Docker Engine version 25.0 or later.

​	`pasta` 网络驱动是实验性的，但相比 `slirp4netns` 端口驱动提供了更好的吞吐性能。`pasta` 驱动需要 Docker Engine 版本 25.0 或更高。

To change the RootlessKit networking configuration:

​	要更改 RootlessKit 网络配置：

1. Create a file at `~/.config/systemd/user/docker.service.d/override.conf`. 在 `~/.config/systemd/user/docker.service.d/override.conf` 创建文件。

2. Add the following contents, depending on which configuration you would like to use: 根据所需配置添加以下内容：

   - `slirp4netns`

     

     ```systemd
     [Service]
     Environment="DOCKERD_ROOTLESS_ROOTLESSKIT_NET=slirp4netns"
     Environment="DOCKERD_ROOTLESS_ROOTLESSKIT_PORT_DRIVER=slirp4netns"
     ```

   - `pasta` network driver with `implicit` port driver

     `pasta` 网络驱动和 `implicit` 端口驱动

     ```systemd
     [Service]
     Environment="DOCKERD_ROOTLESS_ROOTLESSKIT_NET=pasta"
     Environment="DOCKERD_ROOTLESS_ROOTLESSKIT_PORT_DRIVER=implicit"
     ```

3. Restart the daemon:

   重启守护进程：

   ```console
   $ systemctl --user daemon-reload
   $ systemctl --user restart docker
   ```

For more information about networking options for RootlessKit, see:

​	有关 RootlessKit 网络选项的更多信息，请参见：

- [Network drivers](https://github.com/rootless-containers/rootlesskit/blob/v2.0.0/docs/network.md)

- [Port drivers](https://github.com/rootless-containers/rootlesskit/blob/v2.0.0/docs/port.md)

### 调试提示 Tips for debugging

**Entering into `dockerd` namespaces**

The `dockerd-rootless.sh` script executes `dockerd` in its own user, mount, and network namespaces.

​	`dockerd-rootless.sh` 脚本在其自身的用户、挂载和网络命名空间中执行 `dockerd`。

For debugging, you can enter the namespaces by running `nsenter -U --preserve-credentials -n -m -t $(cat $XDG_RUNTIME_DIR/docker.pid)`.

​	为调试，可以通过运行 `nsenter -U --preserve-credentials -n -m -t $(cat $XDG_RUNTIME_DIR/docker.pid)` 进入命名空间。
