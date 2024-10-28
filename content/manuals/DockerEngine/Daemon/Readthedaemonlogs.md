+++
title = "查看守护进程日志"
date = 2024-10-23T14:54:40+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/daemon/logs/](https://docs.docker.com/engine/daemon/logs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Read the daemon logs - 查看守护进程日志

The daemon logs may help you diagnose problems. The logs may be saved in one of a few locations, depending on the operating system configuration and the logging subsystem used:

​	守护进程日志可以帮助您诊断问题。日志的位置可能因操作系统配置和使用的日志记录子系统而异：

| Operating system                   | Location 位置                                                |
| :--------------------------------- | :----------------------------------------------------------- |
| Linux                              | 使用命令 `journalctl -xu docker.service`（或根据您的 Linux 发行版读取 `/var/log/syslog` 或 `/var/log/messages`） - Use the command `journalctl -xu docker.service` (or read `/var/log/syslog` or `/var/log/messages`, depending on your Linux Distribution) |
| macOS (`dockerd` logs)             | `~/Library/Containers/com.docker.docker/Data/log/vm/dockerd.log` |
| macOS (`containerd` logs)          | `~/Library/Containers/com.docker.docker/Data/log/vm/containerd.log` |
| Windows (WSL2) (`dockerd` logs)    | `%LOCALAPPDATA%\Docker\log\vm\dockerd.log`                   |
| Windows (WSL2) (`containerd` logs) | `%LOCALAPPDATA%\Docker\log\vm\containerd.log`                |
| Windows (Windows containers)       | 日志在 Windows 事件日志中 Logs are in the Windows Event Log  |

To view the `dockerd` logs on macOS, open a terminal Window, and use the `tail` command with the `-f` flag to "follow" the logs. Logs will be printed until you terminate the command using `CTRL+c`:

​	在 macOS 上查看 `dockerd` 日志，打开终端窗口，并使用 `tail` 命令和 `-f` 标志“跟随”日志。日志将持续打印，直到您使用 `CTRL+c` 终止命令：

```console
$ tail -f ~/Library/Containers/com.docker.docker/Data/log/vm/dockerd.log
2021-07-28T10:21:21Z dockerd time="2021-07-28T10:21:21.497642089Z" level=debug msg="attach: stdout: begin"
2021-07-28T10:21:21Z dockerd time="2021-07-28T10:21:21.497714291Z" level=debug msg="attach: stderr: begin"
2021-07-28T10:21:21Z dockerd time="2021-07-28T10:21:21.499798390Z" level=debug msg="Calling POST /v1.41/containers/35fc5ec0ffe1ad492d0a4fbf51fd6286a087b89d4dd66367fa3b7aec70b46a40/wait?condition=removed"
2021-07-28T10:21:21Z dockerd time="2021-07-28T10:21:21.518403686Z" level=debug msg="Calling GET /v1.41/containers/35fc5ec0ffe1ad492d0a4fbf51fd6286a087b89d4dd66367fa3b7aec70b46a40/json"
2021-07-28T10:21:21Z dockerd time="2021-07-28T10:21:21.527074928Z" level=debug msg="Calling POST /v1.41/containers/35fc5ec0ffe1ad492d0a4fbf51fd6286a087b89d4dd66367fa3b7aec70b46a40/start"
2021-07-28T10:21:21Z dockerd time="2021-07-28T10:21:21.528203579Z" level=debug msg="container mounted via layerStore: &{/var/lib/docker/overlay2/6e76ffecede030507fcaa576404e141e5f87fc4d7e1760e9ce5b52acb24
...
^C
```

## 启用调试 Enable debugging

There are two ways to enable debugging. The recommended approach is to set the `debug` key to `true` in the `daemon.json` file. This method works for every Docker platform.

​	启用调试有两种方法。推荐的方法是将 `debug` 键设置为 `true`，并将其添加到 `daemon.json` 文件中。这种方法适用于所有 Docker 平台。

1. Edit the `daemon.json` file, which is usually located in `/etc/docker/`. You may need to create this file, if it doesn't yet exist. On macOS or Windows, don't edit the file directly. Instead, edit the file through the Docker Desktop settings. 编辑通常位于 `/etc/docker/` 下的 `daemon.json` 文件。如果文件不存在，您可能需要创建它。在 macOS 或 Windows 上，不要直接编辑文件，而是通过 Docker Desktop 设置进行编辑。

2. If the file is empty, add the following:

   如果文件为空，添加以下内容：

   ```json
   {
     "debug": true
   }
   ```

   If the file already contains JSON, just add the key `"debug": true`, being careful to add a comma to the end of the line if it's not the last line before the closing bracket. Also verify that if the `log-level` key is set, it's set to either `info` or `debug`. `info` is the default, and possible values are `debug`, `info`, `warn`, `error`, `fatal`.

   ​	如果文件中已有 JSON 数据，只需添加 `"debug": true` 键，注意若该行不是最后一行，需要在行末添加逗号。还请确保 `log-level` 键（如果已设置）为 `info` 或 `debug`。`info` 是默认值，可选值包括 `debug`、`info`、`warn`、`error`、`fatal`。

3. Send a `HUP` signal to the daemon to cause it to reload its configuration. On Linux hosts, use the following command.

   向守护进程发送 `HUP` 信号，使其重新加载配置。在 Linux 主机上，使用以下命令：

   ```console
   $ sudo kill -SIGHUP $(pidof dockerd)
   ```

   On Windows hosts, restart Docker.
   
   ​	在 Windows 主机上，重启 Docker。

Instead of following this procedure, you can also stop the Docker daemon and restart it manually with the debug flag `-D`. However, this may result in Docker restarting with a different environment than the one the hosts' startup scripts create, and this may make debugging more difficult.

​	您也可以停止 Docker 守护进程并使用调试标志 `-D` 手动重新启动它，但这样可能导致 Docker 在不同的环境下重启，从而可能使调试更困难。

## 强制记录栈跟踪 Force a stack trace to be logged

If the daemon is unresponsive, you can force a full stack trace to be logged by sending a `SIGUSR1` signal to the daemon.

​	如果守护进程无响应，您可以通过发送 `SIGUSR1` 信号强制记录完整的堆栈跟踪。

- **Linux**:

  

  ```console
  $ sudo kill -SIGUSR1 $(pidof dockerd)
  ```

- **Windows Server**:

  Download [docker-signal](https://github.com/moby/docker-signal). 下载 [docker-signal](https://github.com/moby/docker-signal)。

  Get the process ID of dockerd `Get-Process dockerd`. 获取 dockerd 的进程 ID：`Get-Process dockerd`。

  Run the executable with the flag `--pid=<PID of daemon>`. 使用标志 `--pid=<PID of daemon>` 运行可执行文件。

This forces a stack trace to be logged but doesn't stop the daemon. Daemon logs show the stack trace or the path to a file containing the stack trace if it was logged to a file.

​	这会强制记录堆栈跟踪，但不会停止守护进程。守护进程日志将显示堆栈跟踪或包含堆栈跟踪的文件路径。

The daemon continues operating after handling the `SIGUSR1` signal and dumping the stack traces to the log. The stack traces can be used to determine the state of all goroutines and threads within the daemon.

​	在处理 `SIGUSR1` 信号并将堆栈跟踪输出到日志后，守护进程将继续运行。堆栈跟踪可用于确定守护进程中所有 goroutine 和线程的状态。

## 查看栈跟踪 View stack traces

The Docker daemon log can be viewed by using one of the following methods:

​	可以使用以下方法查看 Docker 守护进程日志：

- By running `journalctl -u docker.service` on Linux systems using `systemctl`
  - 在使用 `systemctl` 的 Linux 系统上运行 `journalctl -u docker.service`

- `/var/log/messages`, `/var/log/daemon.log`, or `/var/log/docker.log` on older Linux systems
  - 在旧版 Linux 系统上查看 `/var/log/messages`、`/var/log/daemon.log` 或 `/var/log/docker.log`


> **Note**
>
> 
>
> It isn't possible to manually generate a stack trace on Docker Desktop for Mac or Docker Desktop for Windows. However, you can click the Docker taskbar icon and choose **Troubleshoot** to send information to Docker if you run into issues.
>
> ​	在 Docker Desktop for Mac 或 Docker Desktop for Windows 上，无法手动生成堆栈跟踪。但是，如果遇到问题，您可以点击 Docker 任务栏图标并选择 **Troubleshoot** 以向 Docker 发送信息。

Look in the Docker logs for a message like the following:

​	在 Docker 日志中查找如下消息：

```none
...goroutine stacks written to /var/run/docker/goroutine-stacks-2017-06-02T193336z.log
```

The locations where Docker saves these stack traces and dumps depends on your operating system and configuration. You can sometimes get useful diagnostic information straight from the stack traces and dumps. Otherwise, you can provide this information to Docker for help diagnosing the problem.

​	Docker 将堆栈跟踪和转储文件保存的位置取决于您的操作系统和配置。您可以直接从堆栈跟踪和转储文件中获取有用的诊断信息，或者将这些信息提供给 Docker 以帮助诊断问题。
