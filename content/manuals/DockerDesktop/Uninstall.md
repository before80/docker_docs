+++
title = "卸载"
date = 2024-10-23T14:54:40+08:00
weight = 200
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/uninstall/](https://docs.docker.com/desktop/uninstall/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Uninstall Docker Desktop - 卸载 Docker Desktop

> **Warning**
>
> 
>
> Uninstalling Docker Desktop destroys Docker containers, images, volumes, and other Docker-related data local to the machine, and removes the files generated by the application. To learn how to preserve important data before uninstalling, refer to the [back up and restore data]({{< ref "/manuals/DockerDesktop/HowtobackupandrestoreyourDockerDesktopdata" >}}) section .
>
> ​	卸载 Docker Desktop 会销毁本地机器上的 Docker 容器、镜像、卷和其他与 Docker 相关的数据，并删除应用生成的文件。要了解如何在卸载前保留重要数据，请参阅 [备份和恢复数据]({{< ref "/manuals/DockerDesktop/HowtobackupandrestoreyourDockerDesktopdata" >}}) 部分。

{{< tabpane text=true persist=disabled >}}

{{% tab header="Windows" %}}

o uninstall Docker Desktop from your Windows machine: 在 Windows 机器上卸载 Docker Desktop：

1. From the Windows **Start** menu, select **Settings** > **Apps** > **Apps & features**. 从 Windows **开始**菜单，选择 **设置** > **应用** > **应用和功能**。
2. Select **Docker Desktop** from the **Apps & features** list and then select **Uninstall**. 在 **应用和功能** 列表中选择 **Docker Desktop**，然后选择 **卸载**。
3. Select **Uninstall** to confirm your selection. 选择 **卸载** 以确认您的选择。

You can also uninstall Docker Desktop from the CLI:

​	您也可以从 CLI 卸载 Docker Desktop：

1. Locate the installer:

   找到安装程序：

   ```console
   $ C:\Program Files\Docker\Docker\Docker Desktop Installer.exe
   ```

2. Uninstall Docker Desktop. 卸载 Docker Desktop。

- In PowerShell, run:

  在 PowerShell 中运行：

  ```console
  $ Start-Process 'Docker Desktop Installer.exe' -Wait uninstall
  ```

- In the Command Prompt, run:

  在命令提示符中运行：

  ```console
  $ start /w "" "Docker Desktop Installer.exe" uninstall
  ```

After uninstalling Docker Desktop, there may be some residual files left behind which you can remove manually. These are:

​	卸载 Docker Desktop 后，可能会留下一些残余文件，您可以手动删除这些文件：

```console
C:\ProgramData\Docker
C:\ProgramData\DockerDesktop
C:\Program Files\Docker
C:\Users\<your user name>\AppData\Local\Docker
C:\Users\<your user name>\AppData\Roaming\Docker
C:\Users\<your user name>\AppData\Roaming\Docker Desktop
C:\Users\<your user name>\.docker
```

{{% /tab  %}}

{{% tab header="Mac" %}}

To uninstall Docker Desktop from your Mac:

​	在 Mac 上卸载 Docker Desktop：

1. From the Docker menu, select the **Troubleshoot** icon in the top-right corner of Docker Dashboard and then select **Uninstall**. 在 Docker 菜单中选择 Docker Dashboard 右上角的 **Troubleshoot** 图标，然后选择 **Uninstall**。
2. Select **Uninstall** to confirm your selection. 选择 **Uninstall** 以确认您的选择。

You can also uninstall Docker Desktop from the CLI. Run:

​	您也可以从 CLI 卸载 Docker Desktop，运行：

```console
$ /Applications/Docker.app/Contents/MacOS/uninstall
```

After uninstalling Docker Desktop, there may be some residual files left behind which you can remove:

​	卸载 Docker Desktop 后，可能会留下一些残余文件，您可以删除这些文件：

```console
$ rm -rf ~/Library/Group\ Containers/group.com.docker
$ rm -rf ~/Library/Containers/com.docker.docker
$ rm -rf ~/.docker
```

You can also move the Docker application to the trash.

​	您也可以将 Docker 应用程序移至废纸篓。

{{% /tab  %}}

{{% tab header="Linux" %}}

Docker Desktop is removed from a Linux host using the package manager.

​	在 Linux 主机上使用包管理器卸载 Docker Desktop。

Once Docker Desktop is removed, users must delete the `credsStore` and `currentContext` properties from the `~/.docker/config.json`.

​	卸载 Docker Desktop 后，用户需要从 `~/.docker/config.json` 文件中删除 `credsStore` 和 `currentContext` 属性。

{{% /tab  %}}

{{% tab header="Ubuntu" %}}

To remove Docker Desktop for Ubuntu, run:

​	要从 Ubuntu 卸载 Docker Desktop，运行：

```console
$ sudo apt remove docker-desktop
```

For a complete cleanup, remove configuration and data files at `$HOME/.docker/desktop`, the symlink at `/usr/local/bin/com.docker.cli`, and purge the remaining systemd service files.

​	要进行完全清理，请删除 `$HOME/.docker/desktop` 中的配置和数据文件，删除 `/usr/local/bin/com.docker.cli` 的符号链接，并清除剩余的 systemd 服务文件。

```console
$ rm -r $HOME/.docker/desktop
$ sudo rm /usr/local/bin/com.docker.cli
$ sudo apt purge docker-desktop
```

Remove the `credsStore` and `currentContext` properties from `$HOME/.docker/config.json`. Additionally, you must delete any edited configuration files manually.

​	从 `$HOME/.docker/config.json` 文件中删除 `credsStore` 和 `currentContext` 属性。此外，您需要手动删除任何编辑过的配置文件。

{{% /tab  %}}

{{% tab header="Debian" %}}

To remove Docker Desktop for Debian, run:

​	要从 Debian 卸载 Docker Desktop，运行：

```console
$ sudo apt remove docker-desktop
```

For a complete cleanup, remove configuration and data files at `$HOME/.docker/desktop`, the symlink at `/usr/local/bin/com.docker.cli`, and purge the remaining systemd service files.

​	要进行完全清理，请删除 `$HOME/.docker/desktop` 中的配置和数据文件，删除 `/usr/local/bin/com.docker.cli` 的符号链接，并清除剩余的 systemd 服务文件。

```console
$ rm -r $HOME/.docker/desktop
$ sudo rm /usr/local/bin/com.docker.cli
$ sudo apt purge docker-desktop
```

Remove the `credsStore` and `currentContext` properties from `$HOME/.docker/config.json`. Additionally, you must delete any edited configuration files manually.

​	从 `$HOME/.docker/config.json` 文件中删除 `credsStore` 和 `currentContext` 属性。此外，您需要手动删除任何编辑过的配置文件。

{{% /tab  %}}

{{% tab header="Fedora" %}}

To remove Docker Desktop for Fedora, run:

​	要从 Fedora 卸载 Docker Desktop，运行：

```console
$ sudo dnf remove docker-desktop
```

For a complete cleanup, remove configuration and data files at `$HOME/.docker/desktop`, the symlink at `/usr/local/bin/com.docker.cli`, and purge the remaining systemd service files.

​	要进行完全清理，请删除 `$HOME/.docker/desktop` 中的配置和数据文件，删除 `/usr/local/bin/com.docker.cli` 的符号链接，并清除剩余的 systemd 服务文件。

```console
$ rm -r $HOME/.docker/desktop
$ sudo rm /usr/local/bin/com.docker.cli
```

Remove the `credsStore` and `currentContext` properties from `$HOME/.docker/config.json`. Additionally, you must delete any edited configuration files manually.

​	从 `$HOME/.docker/config.json` 文件中删除 `credsStore` 和 `currentContext` 属性。此外，您需要手动删除任何编辑过的配置文件。

{{% /tab  %}}

{{% tab header="Arch" %}}

To remove Docker Desktop for Arch, run:

​	要从 Arch 卸载 Docker Desktop，运行：

```console
$ sudo pacman -R docker-desktop
```

For a complete cleanup, remove configuration and data files at `$HOME/.docker/desktop`, the symlink at `/usr/local/bin/com.docker.cli`, and purge the remaining systemd service files.

​	要进行完全清理，请删除 `$HOME/.docker/desktop` 中的配置和数据文件，删除 `/usr/local/bin/com.docker.cli` 的符号链接，并清除剩余的 systemd 服务文件。

```console
$ rm -r $HOME/.docker/desktop
$ sudo rm /usr/local/bin/com.docker.cli
$ sudo pacman -Rns docker-desktop
```

Remove the `credsStore` and `currentContext` properties from `$HOME/.docker/config.json`. Additionally, you must delete any edited configuration files manually.

​	从 `$HOME/.docker/config.json` 文件中删除 `credsStore` 和 `currentContext` 属性。此外，您需要手动删除任何编辑过的配置文件。

{{% /tab  %}}

{{< /tabpane >}}



​    
