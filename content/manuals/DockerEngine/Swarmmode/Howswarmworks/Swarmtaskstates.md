+++
title = "Swarm 任务状态"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/how-swarm-mode-works/swarm-task-states/](https://docs.docker.com/engine/swarm/how-swarm-mode-works/swarm-task-states/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Swarm task states - Swarm 任务状态

Docker lets you create services, which can start tasks. A service is a description of a desired state, and a task does the work. Work is scheduled on swarm nodes in this sequence:

​	Docker 允许你创建服务，这些服务可以启动任务。服务是期望状态的描述，而任务负责执行具体工作。工作在 Swarm 节点上按以下顺序调度：

1. Create a service by using `docker service create`. 使用 `docker service create` 创建服务。
2. The request goes to a Docker manager node. 请求发送到一个 Docker 管理节点。
3. The Docker manager node schedules the service to run on particular nodes. Docker 管理节点将服务调度到特定的节点上运行。
4. Each service can start multiple tasks.  每个服务可以启动多个任务。
5. Each task has a life cycle, with states like `NEW`, `PENDING`, and `COMPLETE`. 每个任务都有生命周期状态，如 `NEW`、`PENDING` 和 `COMPLETE`。

Tasks are execution units that run once to completion. When a task stops, it isn't executed again, but a new task may take its place.

​	任务是一次性执行的单元，运行完成后不再执行，但可能会有新的任务替代它。

Tasks advance through a number of states until they complete or fail. Tasks are initialized in the `NEW` state. The task progresses forward through a number of states, and its state doesn't go backward. For example, a task never goes from `COMPLETE` to `RUNNING`.

​	任务在多个状态间进行转换，直到完成或失败。任务初始状态为 `NEW`，状态只会向前推进，不会回退。例如，任务不会从 `COMPLETE` 回到 `RUNNING` 状态。

Tasks go through the states in the following order:

​	任务在多个状态间进行转换，直到完成或失败。任务初始状态为 `NEW`，状态只会向前推进，不会回退。例如，任务不会从 `COMPLETE` 回到 `RUNNING` 状态。

| Task state  | Description                                                  |
| ----------- | ------------------------------------------------------------ |
| `NEW`       | 任务已初始化。The task was initialized.                      |
| `PENDING`   | 已为任务分配资源。Resources for the task were allocated.     |
| `ASSIGNED`  | Docker 已将任务分配给节点。Docker assigned the task to nodes. |
| `ACCEPTED`  | 工作节点接受了任务。如果工作节点拒绝任务，状态将变为 `REJECTED`。The task was accepted by a worker node. If a worker node rejects the task, the state changes to `REJECTED`. |
| `READY`     | 工作节点准备好启动任务。The worker node is ready to start the task |
| `PREPARING` | Docker 正在准备任务。Docker is preparing the task.           |
| `STARTING`  | Docker 正在启动任务。Docker is starting the task.            |
| `RUNNING`   | 任务正在执行。The task is executing.                         |
| `COMPLETE`  | 任务正常退出，没有错误代码。The task exited without an error code. |
| `FAILED`    | 任务因错误代码而退出。The task exited with an error code.    |
| `SHUTDOWN`  | Docker 请求任务关闭。Docker requested the task to shut down. |
| `REJECTED`  | 工作节点拒绝了任务。The worker node rejected the task.       |
| `ORPHANED`  | 节点因长时间停机而被标记为孤立。The node was down for too long. |
| `REMOVE`    | 任务未终止，但关联服务已被删除或缩减规模。The task is not terminal but the associated service was removed or scaled down. |

## 查看任务状态 View task state

Run `docker service ps <service-name>` to get the state of a task. The `CURRENT STATE` field shows the task's state and how long it's been there.

​	运行 `docker service ps <service-name>` 以获取任务的状态。`CURRENT STATE` 字段显示任务的状态以及该状态的持续时间。

```console
$ docker service ps webserver
ID             NAME              IMAGE    NODE        DESIRED STATE  CURRENT STATE            ERROR                              PORTS
owsz0yp6z375   webserver.1       nginx    UbuntuVM    Running        Running 44 seconds ago
j91iahr8s74p    \_ webserver.1   nginx    UbuntuVM    Shutdown       Failed 50 seconds ago    "No such container: webserver.…"
7dyaszg13mw2    \_ webserver.1   nginx    UbuntuVM    Shutdown       Failed 5 hours ago       "No such container: webserver.…"
```

## Where to go next

- [Learn about swarm tasks 了解 Swarm 任务](https://github.com/docker/swarmkit/blob/master/design/task_model.md)
