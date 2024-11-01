+++
title = "常见问题的解决方法"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/troubleshoot/workarounds/](https://docs.docker.com/desktop/troubleshoot/workarounds/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Workarounds for common problems - 常见问题的解决方法

### 重启 Reboot

Restart your PC to stop / discard any vestige of the daemon running from the previously installed version.

​	重新启动计算机以停止或丢弃任何来自先前安装版本的守护程序残留。

### Unset `DOCKER_HOST`

The `DOCKER_HOST` environmental variable does not need to be set. If you use bash, use the command `unset ${!DOCKER_*}` to unset it. For other shells, consult the shell's documentation.

​	无需设置 `DOCKER_HOST` 环境变量。如果使用 bash，请使用 `unset ${!DOCKER_*}` 来取消设置。对于其他 shell，请参考相应 shell 的文档。

### 确保 Docker 正在运行以支持 Web 服务器示例 Make sure Docker is running for webserver examples

For the `hello-world-nginx` example and others, Docker Desktop must be running to get to the webserver on `http://localhost/`. Make sure that the Docker whale is showing in the menu bar, and that you run the Docker commands in a shell that is connected to the Docker Desktop Engine. Otherwise, you might start the webserver container but get a "web page not available" error when you go to `docker`.

​	在 `hello-world-nginx` 示例和其他示例中，Docker Desktop 必须在运行状态，才能访问 `http://localhost/` 上的 Web 服务器。确保菜单栏中显示鲸鱼图标，并在与 Docker Desktop 引擎连接的 shell 中运行 Docker 命令。否则，您可能启动了 Web 服务器容器，但在访问 `docker` 时会收到“网页不可用”错误。

### 如何解决 `端口已分配` 错误 How to solve `port already allocated` errors

If you see errors like `Bind for 0.0.0.0:8080 failed: port is already allocated` or `listen tcp:0.0.0.0:8080: bind: address is already in use` ...

​	如果您看到类似 `Bind for 0.0.0.0:8080 failed: port is already allocated` 或 `listen tcp:0.0.0.0:8080: bind: address is already in use` 的错误...

These errors are often caused by some other software on Windows using those ports. To discover the identity of this software, either use the `resmon.exe` GUI and click "Network" and then "Listening Ports" or in a PowerShell use `netstat -aon | find /i "listening "` to discover the PID of the process currently using the port (the PID is the number in the rightmost column). Decide whether to shut the other process down, or to use a different port in your docker app.

​	这些错误通常是由于 Windows 上的其他软件使用了这些端口。要找出这些软件的身份，可以使用 `resmon.exe` GUI，点击“网络”然后点击“监听端口”，或者在 PowerShell 中使用 `netstat -aon | find /i "listening "` 来找到当前占用端口的进程 PID（右侧列中的数字）。决定关闭该进程或在 Docker 应用中使用其他端口。

### 安装了防病毒软件时 Docker Desktop 启动失败 Docker Desktop fails to start when anti-virus software is installed

Some anti-virus software may be incompatible with Hyper-V and Microsoft Windows 10 builds. The conflict typically occurs after a Windows update and manifests as an error response from the Docker daemon and a Docker Desktop start failure.

​	某些防病毒软件可能与 Hyper-V 和 Microsoft Windows 10 版本不兼容。这种冲突通常在 Windows 更新后出现，表现为 Docker 守护程序的错误响应以及 Docker Desktop 启动失败。

For a temporary workaround, uninstall the anti-virus software, or explore other workarounds suggested on Docker Desktop forums.

​	临时解决方案是卸载防病毒软件，或在 Docker Desktop 论坛中探索其他建议的解决方法。
