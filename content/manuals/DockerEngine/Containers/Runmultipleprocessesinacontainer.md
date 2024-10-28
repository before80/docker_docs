+++
title = "在容器中运行多个进程"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/containers/multi-service_container/](https://docs.docker.com/engine/containers/multi-service_container/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Run multiple processes in a container - 在容器中运行多个进程

A container's main running process is the `ENTRYPOINT` and/or `CMD` at the end of the `Dockerfile`. It's best practice to separate areas of concern by using one service per container. That service may fork into multiple processes (for example, Apache web server starts multiple worker processes). It's ok to have multiple processes, but to get the most benefit out of Docker, avoid one container being responsible for multiple aspects of your overall application. You can connect multiple containers using user-defined networks and shared volumes.

​	容器的主要运行进程由 `Dockerfile` 末尾的 `ENTRYPOINT` 和/或 `CMD` 指定。最佳实践是将关注点分离，每个容器只运行一个服务。该服务可以派生多个进程（例如，Apache Web 服务器会启动多个工作进程）。拥有多个进程是可以的，但为了最大化 Docker 的优势，建议避免让一个容器负责应用的多个部分。可以使用用户定义的网络和共享卷来连接多个容器。

The container's main process is responsible for managing all processes that it starts. In some cases, the main process isn't well-designed, and doesn't handle "reaping" (stopping) child processes gracefully when the container exits. If your process falls into this category, you can use the `--init` option when you run the container. The `--init` flag inserts a tiny init-process into the container as the main process, and handles reaping of all processes when the container exits. Handling such processes this way is superior to using a full-fledged init process such as `sysvinit` or `systemd` to handle process lifecycle within your container.

​	容器的主要进程负责管理它启动的所有进程。在某些情况下，主要进程的设计不够完善，无法在容器退出时优雅地处理子进程的终止（“收割”）。如果您的进程属于这种情况，可以在运行容器时使用 `--init` 选项。`--init` 标志会在容器中插入一个微型 init 进程作为主要进程，并在容器退出时负责收割所有进程。使用这种方式处理进程比使用完整的 init 进程（如 `sysvinit` 或 `systemd`）来管理容器内的进程生命周期更为优越。

If you need to run more than one service within a container, you can achieve this in a few different ways.

​	如果需要在一个容器中运行多个服务，可以通过以下几种方式来实现。

## 使用包装脚本 Use a wrapper script

Put all of your commands in a wrapper script, complete with testing and debugging information. Run the wrapper script as your `CMD`. The following is a naive example. First, the wrapper script:

​	将所有命令放入一个包装脚本中，包含测试和调试信息。将包装脚本作为 `CMD` 运行。以下是一个简单的示例。首先是包装脚本：



```bash
#!/bin/bash

# Start the first process
# 启动第一个进程
./my_first_process &

# Start the second process
# 启动第二个进程
./my_second_process &

# Wait for any process to exit
# 等待任意进程退出
wait -n

# Exit with status of process that exited first
# 使用第一个退出的进程的状态码退出
exit $?
```

Next, the Dockerfile:

​	接下来是 Dockerfile：

```dockerfile
# syntax=docker/dockerfile:1
FROM ubuntu:latest
COPY my_first_process my_first_process
COPY my_second_process my_second_process
COPY my_wrapper_script.sh my_wrapper_script.sh
CMD ./my_wrapper_script.sh
```

## 使用 Bash 作业控制 Use Bash job controls

If you have one main process that needs to start first and stay running but you temporarily need to run some other processes (perhaps to interact with the main process) then you can use bash's job control. First, the wrapper script:

​	如果有一个需要先启动并保持运行的主要进程，但需要临时启动其他进程（例如为了与主要进程交互），可以使用 Bash 的作业控制。首先是包装脚本：

```bash
#!/bin/bash

# turn on bash's job control
# 开启 Bash 作业控制
set -m

# Start the primary process and put it in the background
# 启动主进程并将其放到后台
./my_main_process &

# Start the helper process
# 启动辅助进程
./my_helper_process

# the my_helper_process might need to know how to wait on the
# primary process to start before it does its work and returns
# 辅助进程可能需要等待主进程启动后再开始工作并返回

# now we bring the primary process back into the foreground
# and leave it there
# 将主进程重新放到前台并保持在那里
fg %1
```

​	接下来的 Dockerfile：

```dockerfile
# syntax=docker/dockerfile:1
FROM ubuntu:latest
COPY my_main_process my_main_process
COPY my_helper_process my_helper_process
COPY my_wrapper_script.sh my_wrapper_script.sh
CMD ./my_wrapper_script.sh
```

## 使用进程管理器 Use a process manager

Use a process manager like `supervisord`. This is more involved than the other options, as it requires you to bundle `supervisord` and its configuration into your image (or base your image on one that includes `supervisord`), along with the different applications it manages. Then you start `supervisord`, which manages your processes for you.

​	可以使用 `supervisord` 等进程管理器。这种方法比其他选项更复杂，因为需要将 `supervisord` 及其配置文件打包到镜像中（或者基于包含 `supervisord` 的基础镜像），以及它管理的不同应用程序。然后启动 `supervisord`，由它来管理各个进程。

The following Dockerfile example shows this approach. The example assumes that these files exist at the root of the build context:

​	以下 Dockerfile 示例展示了这种方法。该示例假设这些文件存在于构建上下文的根目录中：

- `supervisord.conf`

- `my_first_process`
- `my_second_process`



```dockerfile
# syntax=docker/dockerfile:1
FROM ubuntu:latest
RUN apt-get update && apt-get install -y supervisor
RUN mkdir -p /var/log/supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY my_first_process my_first_process
COPY my_second_process my_second_process
CMD ["/usr/bin/supervisord"]
```

If you want to make sure both processes output their `stdout` and `stderr` to the container logs, you can add the following to the `supervisord.conf` file:

​	如果希望确保两个进程的 `stdout` 和 `stderr` 输出到容器日志，可以在 `supervisord.conf` 文件中添加以下内容：

```ini
[supervisord]
nodaemon=true
logfile=/dev/null
logfile_maxbytes=0

[program:app]
stdout_logfile=/dev/fd/1
stdout_logfile_maxbytes=0
redirect_stderr=true
```
