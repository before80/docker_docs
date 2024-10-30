+++
title = "在 Compose 中使用生命周期钩子"
date = 2024-10-30T21:50:00+08:00
weight = 2
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/how-tos/lifecycle/](https://docs.docker.com/compose/how-tos/lifecycle/)
>
> 收录该文档的时间：`2024-10-30T21:50:00+08:00`

# Using lifecycle hooks with Compose - 在 Compose 中使用生命周期钩子

## 服务生命周期钩子 Services lifecycle hooks

When Docker Compose runs a container, it uses two elements, [ENTRYPOINT and COMMAND](https://github.com/manuals//engine/containers/run.md#default-command-and-options), to manage what happens when the container starts and stops.

​	当 Docker Compose 运行容器时，它使用 [ENTRYPOINT 和 COMMAND](https://github.com/manuals//engine/containers/run.md#default-command-and-options) 两个元素来管理容器启动和停止时的行为。

However, it can sometimes be easier to handle these tasks separately with lifecycle hooks - commands that run right after the container starts or just before it stops.

​	然而，有时使用生命周期钩子——即在容器启动后或停止前立即运行的命令——来分别处理这些任务可能更为简单。

Lifecycle hooks are particularly useful because they can have special privileges (like running as the root user), even when the container itself runs with lower privileges for security. This means that certain tasks requiring higher permissions can be done without compromising the overall security of the container.

​	生命周期钩子特别有用，因为它们可以拥有特殊权限（例如以 root 用户身份运行），即使容器本身出于安全原因在较低权限下运行。这意味着某些需要更高权限的任务可以在不影响容器总体安全的情况下完成。

### Post-start hooks

Post-start hooks are commands that run after the container has started, but there's no set time for when exactly they will execute. The hook execution timing is not assured during the execution of the container's `entrypoint`.

​	启动后钩子是在容器启动后执行的命令，但没有规定它们具体会在何时执行。在容器的 `entrypoint` 执行过程中，钩子的执行时间不确定。

In the example provided:

​	在提供的示例中：

- The hook is used to change the ownership of a volume to a non-root user (because volumes are created with root ownership by default).
  - 钩子用于将卷的所有权更改为非 root 用户（因为卷默认是由 root 拥有的）。

- After the container starts, the `chown` command changes the ownership of the `/data` directory to user `1001`.
  - 容器启动后，`chown` 命令将 `/data` 目录的所有权更改为用户 `1001`。




```yaml
services:
  app:
    image: backend
    user: 1001
    volumes:
      - data:/data    
    post_start:
      - command: chown -R /data 1001:1001
        user: root

volumes:
  data: {} # a Docker volume is created with root ownership 创建的 Docker 卷默认由 root 拥有
```

### Pre-stop hooks

Pre-stop hooks are commands that run before the container is stopped by a specific command (like `docker compose down` or stopping it manually with `Ctrl+C`). These hooks won't run if the container stops by itself or gets killed suddenly.

​	停止前钩子是在特定命令（如 `docker compose down` 或手动用 `Ctrl+C` 停止）触发停止前执行的命令。如果容器自行停止或突然被杀死，则这些钩子不会执行。

In the following example, before the container stops, the `./data_flush.sh` script is run to perform any necessary cleanup.

​	在以下示例中，容器停止之前，执行 `./data_flush.sh` 脚本以进行任何必要的清理工作。

```yaml
services:
  app:
    image: backend
    pre_stop:
      - command: ./data_flush.sh
```