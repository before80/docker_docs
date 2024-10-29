+++
title = "Logs and metrics"
date = 2024-10-23T14:54:40+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/logging/](https://docs.docker.com/engine/logging/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# View container logs - 查看容器日志

The `docker logs` command shows information logged by a running container. The `docker service logs` command shows information logged by all containers participating in a service. The information that's logged and the format of the log depends almost entirely on the container's endpoint command.

By default, `docker logs` or `docker service logs` shows the command's output just as it would appear if you ran the command interactively in a terminal. Unix and Linux commands typically open three I/O streams when they run, called `STDIN`, `STDOUT`, and `STDERR`. `STDIN` is the command's input stream, which may include input from the keyboard or input from another command. `STDOUT` is usually a command's normal output, and `STDERR` is typically used to output error messages. By default, `docker logs` shows the command's `STDOUT` and `STDERR`. To read more about I/O and Linux, see the [Linux Documentation Project article on I/O redirection](https://tldp.org/LDP/abs/html/io-redirection.html).

​	默认情况下，`docker logs` 或 `docker service logs` 会显示命令的输出，就像在终端中以交互方式运行命令时显示的一样。Unix 和 Linux 命令通常在运行时打开三个 I/O 流，称为 `STDIN`、`STDOUT` 和 `STDERR`。`STDIN` 是命令的输入流，可能包含来自键盘的输入或来自其他命令的输入。`STDOUT` 通常是命令的正常输出，而 `STDERR` 通常用于输出错误消息。默认情况下，`docker logs` 显示命令的 `STDOUT` 和 `STDERR`。要了解更多关于 I/O 和 Linux 的信息，请参阅 [Linux Documentation Project 关于 I/O 重定向的文章](https://tldp.org/LDP/abs/html/io-redirection.html)。

In some cases, `docker logs` may not show useful information unless you take additional steps.

​	在某些情况下，除非采取额外措施，否则 `docker logs` 可能不会显示有用的信息。

- If you use a [logging driver]({{< ref "/manuals/DockerEngine/Logsandmetrics/Configureloggingdrivers" >}}) which sends logs to a file, an external host, a database, or another logging back-end, and have ["dual logging"]({{< ref "/manuals/DockerEngine/Logsandmetrics/Usedockerlogswithremoteloggingdrivers" >}}) disabled, `docker logs` may not show useful information.
  - 如果您使用了 [日志驱动程序]({{< ref "/manuals/DockerEngine/Logsandmetrics/Configureloggingdrivers" >}})，将日志发送到文件、外部主机、数据库或其他日志后端，并且禁用了 ["双重日志记录"]({{< ref "/manuals/DockerEngine/Logsandmetrics/Usedockerlogswithremoteloggingdrivers" >}})，则 `docker logs` 可能不会显示有用的信息。

- If your image runs a non-interactive process such as a web server or a database, that application may send its output to log files instead of `STDOUT` and `STDERR`.
  - 如果您的镜像运行的是非交互式进程，例如 Web 服务器或数据库，那么该应用程序可能会将其输出发送到日志文件，而不是 `STDOUT` 和 `STDERR`。


In the first case, your logs are processed in other ways and you may choose not to use `docker logs`. In the second case, the official `nginx` image shows one workaround, and the official Apache `httpd` image shows another.

​	在第一种情况下，日志已通过其他方式处理，您可以选择不使用 `docker logs`。在第二种情况下，官方的 `nginx` 镜像展示了一种解决方法，而官方的 Apache `httpd` 镜像展示了另一种方法。

The official `nginx` image creates a symbolic link from `/var/log/nginx/access.log` to `/dev/stdout`, and creates another symbolic link from `/var/log/nginx/error.log` to `/dev/stderr`, overwriting the log files and causing logs to be sent to the relevant special device instead. See the [Dockerfile](https://github.com/nginxinc/docker-nginx/blob/8921999083def7ba43a06fabd5f80e4406651353/mainline/jessie/Dockerfile#L21-L23).

​	官方的 `nginx` 镜像创建了从 `/var/log/nginx/access.log` 到 `/dev/stdout` 的符号链接，并从 `/var/log/nginx/error.log` 到 `/dev/stderr` 创建了另一个符号链接，覆盖了日志文件，从而将日志发送到相关的特殊设备。请参见 [Dockerfile](https://github.com/nginxinc/docker-nginx/blob/8921999083def7ba43a06fabd5f80e4406651353/mainline/jessie/Dockerfile#L21-L23)。

The official `httpd` driver changes the `httpd` application's configuration to write its normal output directly to `/proc/self/fd/1` (which is `STDOUT`) and its errors to `/proc/self/fd/2` (which is `STDERR`). See the [Dockerfile](https://github.com/docker-library/httpd/blob/b13054c7de5c74bbaa6d595dbe38969e6d4f860c/2.2/Dockerfile#L72-L75).

​	官方的 `httpd` 驱动程序更改了 `httpd` 应用程序的配置，将其正常输出直接写入 `/proc/self/fd/1`（即 `STDOUT`），并将错误写入 `/proc/self/fd/2`（即 `STDERR`）。请参见 [Dockerfile](https://github.com/docker-library/httpd/blob/b13054c7de5c74bbaa6d595dbe38969e6d4f860c/2.2/Dockerfile#L72-L75)。

## 接下来 Next steps

- Configure [logging drivers]({{< ref "/manuals/DockerEngine/Logsandmetrics/Configureloggingdrivers" >}}).
  - 配置 [日志驱动程序]({{< ref "/manuals/DockerEngine/Logsandmetrics/Configureloggingdrivers" >}})。
- Write a [Dockerfile]({{< ref "/reference/Dockerfilereference" >}}).
  - 编写 [Dockerfile]({{< ref "/reference/Dockerfilereference" >}})。
