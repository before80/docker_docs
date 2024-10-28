+++
title = "Live restore"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/engine/daemon/live-restore/](https://docs.docker.com/engine/daemon/live-restore/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Live restore - 实时恢复

By default, when the Docker daemon terminates, it shuts down running containers. You can configure the daemon so that containers remain running if the daemon becomes unavailable. This functionality is called *live restore*. The live restore option helps reduce container downtime due to daemon crashes, planned outages, or upgrades.

​	默认情况下，当 Docker 守护进程终止时，会关闭正在运行的容器。您可以配置守护进程，使得当守护进程不可用时容器依然保持运行。此功能称为 *实时恢复*。启用实时恢复选项有助于减少因守护进程崩溃、计划停机或升级造成的容器停机时间。

> **Note**
>
> 
>
> Live restore isn't supported for Windows containers, but it does work for Linux containers running on Docker Desktop for Windows.
>
> ​	实时恢复不支持 Windows 容器，但适用于在 Docker Desktop for Windows 上运行的 Linux 容器。

## 启用实时恢复 Enable live restore

There are two ways to enable the live restore setting to keep containers alive when the daemon becomes unavailable. **Only do one of the following**.

​	有两种方法可以启用实时恢复设置，以确保当守护进程不可用时容器仍保持运行。**仅执行以下选项之一**。

- Add the configuration to the daemon configuration file. On Linux, this defaults to `/etc/docker/daemon.json`. On Docker Desktop for Mac or Docker Desktop for Windows, select the Docker icon from the task bar, then click **Settings** -> **Docker Engine**. 将配置添加到守护进程配置文件中。在 Linux 系统上，该文件默认位于 `/etc/docker/daemon.json`。在 Docker Desktop for Mac 或 Docker Desktop for Windows 中，从任务栏中选择 Docker 图标，然后点击 **Settings** -> **Docker Engine**。

  - Use the following JSON to enable `live-restore`.

    使用以下 JSON 启用 `live-restore`。

    ```json
    {
      "live-restore": true
    }
    ```

  - Restart the Docker daemon. On Linux, you can avoid a restart (and avoid any downtime for your containers) by reloading the Docker daemon. If you use `systemd`, then use the command `systemctl reload docker`. Otherwise, send a `SIGHUP` signal to the `dockerd` process. 重启 Docker 守护进程。在 Linux 上，您可以通过重新加载 Docker 守护进程来避免重启（并避免容器的任何停机）。如果使用 `systemd`，则执行命令 `systemctl reload docker`。否则，向 `dockerd` 进程发送 `SIGHUP` 信号。

- If you prefer, you can start the `dockerd` process manually with the `--live-restore` flag. This approach isn't recommended because it doesn't set up the environment that `systemd` or another process manager would use when starting the Docker process. This can cause unexpected behavior. 如果您更喜欢，可以通过 `--live-restore` 标志手动启动 `dockerd` 进程。不过不建议使用此方法，因为它不会设置 `systemd` 或其他进程管理器启动 Docker 时所需的环境配置，这可能会导致意外行为。

## 升级期间的实时恢复 Live restore during upgrades

Live restore allows you to keep containers running across Docker daemon updates, but is only supported when installing patch releases (`YY.MM.x`), not for major (`YY.MM`) daemon upgrades.

​	实时恢复允许您在 Docker 守护进程更新期间保持容器运行，但仅支持在安装补丁版本（`YY.MM.x`）时使用，不支持主要版本（`YY.MM`）的升级。

If you skip releases during an upgrade, the daemon may not restore its connection to the containers. If the daemon can't restore the connection, it can't manage the running containers and you must stop them manually.

​	如果在升级过程中跳过某些版本，守护进程可能无法恢复与容器的连接。如果守护进程无法恢复连接，它将无法管理正在运行的容器，您必须手动停止它们。

## 重启时的实时恢复 Live restore upon restart

The live restore option only works to restore containers if the daemon options, such as bridge IP addresses and graph driver, didn't change. If any of these daemon-level configuration options have changed, the live restore may not work and you may need to manually stop the containers.

​	实时恢复选项仅在守护进程的配置选项（例如桥接 IP 地址和图形驱动程序）没有更改的情况下才能恢复容器。如果更改了这些守护进程级别的配置选项，则实时恢复可能无法生效，您可能需要手动停止容器。

## 实时恢复对正在运行的容器的影响 Impact of live restore on running containers

If the daemon is down for a long time, running containers may fill up the FIFO log the daemon normally reads. A full log blocks containers from logging more data. The default buffer size is 64K. If the buffers fill, you must restart the Docker daemon to flush them.

​	如果守护进程长时间停机，正在运行的容器可能会填满守护进程通常读取的 FIFO 日志。日志满后，容器将无法继续记录数据。默认缓冲区大小为 64K。如果缓冲区已满，您必须重启 Docker 守护进程以清空它们。

On Linux, you can modify the kernel's buffer size by changing `/proc/sys/fs/pipe-max-size`. You can't modify the buffer size on Docker Desktop for Mac or Docker Desktop for Windows.

​	在 Linux 上，可以通过更改 `/proc/sys/fs/pipe-max-size` 来修改内核的缓冲区大小。无法在 Docker Desktop for Mac 或 Docker Desktop for Windows 上修改缓冲区大小。

## 实时恢复与 Swarm 模式 Live restore and Swarm mode

The live restore option only pertains to standalone containers, and not to Swarm services. Swarm services are managed by Swarm managers. If Swarm managers are not available, Swarm services continue to run on worker nodes but can't be managed until enough Swarm managers are available to maintain a quorum.

​	实时恢复选项仅适用于独立容器，不适用于 Swarm 服务。Swarm 服务由 Swarm 管理器管理。如果 Swarm 管理器不可用，Swarm 服务将在工作节点上继续运行，但在足够的 Swarm 管理器可用以维持法定人数之前无法进行管理。
