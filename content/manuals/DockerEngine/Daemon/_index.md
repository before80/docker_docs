+++
title = "Daemon"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/engine/daemon/](https://docs.docker.com/engine/daemon/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker daemon configuration overview - Docker 守护进程配置概述

This page shows you how to customize the Docker daemon, `dockerd`.

​	本页展示了如何自定义 Docker 守护进程 `dockerd`。

> **Note**
>
> 
>
> This page is for users who've installed Docker Engine manually. If you're using Docker Desktop, refer to the [settings page]({{< ref "/manuals/DockerDesktop/Changesettings#docker-engine">}}).
>
> ​	本页适用于手动安装 Docker Engine 的用户。如果您使用的是 Docker Desktop，请参阅 [设置页面]({{< ref "/manuals/DockerDesktop/Changesettings#docker-engine">}})。

## 配置 Docker 守护进程 Configure the Docker daemon

There are two ways to configure the Docker daemon:

​	配置 Docker 守护进程有两种方法：

- Use a JSON configuration file. This is the preferred option, since it keeps all configurations in a single place.
  - 使用 JSON 配置文件。此选项是首选，因为它将所有配置集中在一个地方。

- Use flags when starting `dockerd`.
  - 在启动 `dockerd` 时使用标志（flags）。


You can use both of these options together as long as you don't specify the same option both as a flag and in the JSON file. If that happens, the Docker daemon won't start and prints an error message.

​	您可以同时使用这两种选项，只要不同时在标志和 JSON 文件中指定相同的选项。如果发生这种情况，Docker 守护进程将无法启动，并打印一条错误消息。

### 配置文件 Configuration file

The following table shows the location where the Docker daemon expects to find the configuration file by default, depending on your system and how you're running the daemon.

​	以下表格显示了 Docker 守护进程在默认情况下期望找到配置文件的位置，具体取决于您的系统和运行守护进程的方式。

| OS and configuration              | File location 文件位置                     |
| --------------------------------- | ------------------------------------------ |
| Linux, regular setup 常规设置     | `/etc/docker/daemon.json`                  |
| Linux, rootless mode 非 root 模式 | `~/.config/docker/daemon.json`             |
| Windows                           | `C:\ProgramData\docker\config\daemon.json` |

For rootless mode, the daemon respects the `XDG_CONFIG_HOME` variable. If set, the expected file location is `$XDG_CONFIG_HOME/docker/daemon.json`.

​	对于非 root 模式，守护进程遵循 `XDG_CONFIG_HOME` 变量。如果已设置，文件位置应为 `$XDG_CONFIG_HOME/docker/daemon.json`。

You can also explicitly specify the location of the configuration file on startup, using the `dockerd --config-file` flag.

​	您还可以在启动时使用 `dockerd --config-file` 标志显式指定配置文件的位置。

Learn about the available configuration options in the [dockerd reference docs](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file)

​	了解可用的配置选项，请参阅 [dockerd 参考文档](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file)。

### 使用标志进行配置 Configuration using flags

You can also start the Docker daemon manually and configure it using flags. This can be useful for troubleshooting problems.

​	您还可以手动启动 Docker 守护进程并使用标志进行配置。这对于解决问题时非常有用。

Here's an example of how to manually start the Docker daemon, using the same configurations as shown in the previous JSON configuration:

​	以下是如何手动启动 Docker 守护进程的示例，使用与之前 JSON 配置中显示的相同配置：



```console
$ dockerd --debug \
  --tls=true \
  --tlscert=/var/docker/server.pem \
  --tlskey=/var/docker/serverkey.pem \
  --host tcp://192.168.59.3:2376
```

Learn about the available configuration options in the [dockerd reference docs]({{< ref "/reference/CLIreference/dockerd" >}}), or by running:

​	了解可用的配置选项，请参阅 [dockerd 参考文档]({{< ref "/reference/CLIreference/dockerd" >}})，或者运行：



```console
$ dockerd --help
```

## 守护进程数据目录 Daemon data directory

The Docker daemon persists all data in a single directory. This tracks everything related to Docker, including containers, images, volumes, service definition, and secrets.

​	Docker 守护进程将所有数据持久化在一个目录中。此目录跟踪与 Docker 相关的所有内容，包括容器、镜像、卷、服务定义和密钥。

By default this directory is:

​	默认情况下，该目录为：

- `/var/lib/docker` on Linux.
- `C:\ProgramData\docker` on Windows.

You can configure the Docker daemon to use a different directory, using the `data-root` configuration option. For example:

​	您可以使用 `data-root` 配置选项将 Docker 守护进程配置为使用其他目录。例如：



```json
{
  "data-root": "/mnt/docker-data"
}
```

Since the state of a Docker daemon is kept on this directory, make sure you use a dedicated directory for each daemon. If two daemons share the same directory, for example, an NFS share, you are going to experience errors that are difficult to troubleshoot.

​	由于 Docker 守护进程的状态保存在该目录中，确保为每个守护进程使用一个独立的目录。如果两个守护进程共享相同的目录，例如 NFS 共享，您可能会遇到难以排查的错误。

## 接下来 Next steps

Many specific configuration options are discussed throughout the Docker documentation. Some places to go next include:

​	Docker 文档中讨论了许多具体的配置选项。下一步的一些参考包括：

- [Automatically start containers 自动启动容器]({{< ref "/manuals/DockerEngine/Containers/Startcontainersautomatically" >}})

- [Limit a container's resources 限制容器的资源]({{< ref "/manuals/DockerEngine/Containers/Resourceconstraints" >}})
- [Configure storage drivers 配置存储驱动程序]({{< ref "/manuals/DockerEngine/Storage/Storagedrivers/Selectastoragedriver" >}})
- [Container security 容器安全性]({{< ref "/manuals/DockerEngine/Security" >}})
- [Configure the Docker daemon to use a proxy 配置 Docker 守护进程使用代理]({{< ref "/manuals/DockerEngine/Daemon/Daemonproxyconfiguration" >}})
