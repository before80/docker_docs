+++
title = "自动启动容器"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/containers/start-containers-automatically/](https://docs.docker.com/engine/containers/start-containers-automatically/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Start containers automatically - 自动启动容器

Docker provides [restart policies](https://docs.docker.com/engine/containers/run/#restart-policies---restart) to control whether your containers start automatically when they exit, or when Docker restarts. Restart policies start linked containers in the correct order. Docker recommends that you use restart policies, and avoid using process managers to start containers.

​	Docker 提供了 [重启策略](https://docs.docker.com/engine/containers/run/#restart-policies---restart)，用于控制容器在退出或 Docker 重启时是否自动启动。重启策略确保关联的容器按正确的顺序启动。Docker 建议使用重启策略，而不是通过进程管理器启动容器。

Restart policies are different from the `--live-restore` flag of the `dockerd` command. Using `--live-restore` lets you to keep your containers running during a Docker upgrade, though networking and user input are interrupted.

​	重启策略与 `dockerd` 命令中的 `--live-restore` 标志不同。使用 `--live-restore` 可以让容器在 Docker 升级时保持运行，但会中断网络连接和用户输入。

## 使用重启策略 Use a restart policy

To configure the restart policy for a container, use the `--restart` flag when using the `docker run` command. The value of the `--restart` flag can be any of the following:

​	要配置容器的重启策略，可在 `docker run` 命令中使用 `--restart` 标志。`--restart` 标志的值可以是以下任意值：

| Flag                       | Description                                                  |
| :------------------------- | :----------------------------------------------------------- |
| `no`                       | 不自动重启容器。（默认） Don't automatically restart the container. (Default) |
| `on-failure[:max-retries]` | 当容器因错误退出（返回非零退出代码）时重启。可选地，使用 `:max-retries` 限制 Docker 守护进程尝试重启容器的次数。`on-failure` 策略仅在容器因错误退出时重启，不会在守护进程重启时重启容器。 Restart the container if it exits due to an error, which manifests as a non-zero exit code. Optionally, limit the number of times the Docker daemon attempts to restart the container using the `:max-retries` option. The `on-failure` policy only prompts a restart if the container exits with a failure. It doesn't restart the container if the daemon restarts. |
| `always`                   | 每当容器停止时自动重启。如果手动停止容器，则只有在 Docker 守护进程重启或手动重启容器时才会重启。（参见 [重启策略详细信息](https://docs.docker.com/engine/containers/start-containers-automatically/#restart-policy-details) 中的第二点） Always restart the container if it stops. If it's manually stopped, it's restarted only when Docker daemon restarts or the container itself is manually restarted. (See the second bullet listed in [restart policy details](https://docs.docker.com/engine/containers/start-containers-automatically/#restart-policy-details)) |
| `unless-stopped`           | 类似于 `always`，但当容器被手动或其他方式停止时，即使 Docker 守护进程重启也不会重新启动容器。 Similar to `always`, except that when the container is stopped (manually or otherwise), it isn't restarted even after Docker daemon restarts. |

The following command starts a Redis container and configures it to always restart, unless the container is explicitly stopped, or the daemon restarts.

​	以下命令启动一个 Redis 容器，并配置它在手动停止或守护进程重启时除外，总是自动重启：



```console
$ docker run -d --restart unless-stopped redis
```

The following command changes the restart policy for an already running container named `redis`.

​	以下命令更改已在运行的容器 `redis` 的重启策略：



```console
$ docker update --restart unless-stopped redis
```

The following command ensures all running containers restart.

​	以下命令确保所有正在运行的容器重启：



```console
$ docker update --restart unless-stopped $(docker ps -q)
```

### 重启策略详细信息 Restart policy details

Keep the following in mind when using restart policies:

​	使用重启策略时请注意以下几点：

- A restart policy only takes effect after a container starts successfully. In this case, starting successfully means that the container is up for at least 10 seconds and Docker has started monitoring it. This prevents a container which doesn't start at all from going into a restart loop.
  - 重启策略仅在容器成功启动后生效。在这种情况下，成功启动指容器至少运行 10 秒并被 Docker 监控到。这可防止无法启动的容器进入重启循环。

- If you manually stop a container, the restart policy is ignored until the Docker daemon restarts or the container is manually restarted. This prevents a restart loop.
  - 如果手动停止容器，则在 Docker 守护进程重启或手动重启容器之前，重启策略将被忽略。这可防止重启循环。

- Restart policies only apply to containers. To configure restart policies for Swarm services, see [flags related to service restart]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}}).
  - 重启策略仅适用于容器。要为 Swarm 服务配置重启策略，请参阅 [服务重启的相关标志]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}})。

### 重启前台容器 Restarting foreground containers

When you run a container in the foreground, stopping a container causes the attached CLI to exit as well, regardless of the restart policy of the container. This behavior is illustrated in the following example.

​	当在前台运行容器时，停止容器也会导致已连接的 CLI 退出，无论容器的重启策略是什么。以下示例展示了这种行为。

1. Create a Dockerfile that prints the numbers 1 to 5 and then exits.

   创建一个 Dockerfile，打印数字 1 到 5，然后退出。

   ```dockerfile
   FROM busybox:latest
   COPY --chmod=755 <<"EOF" /start.sh
   echo "Starting..."
   for i in $(seq 1 5); do
     echo "$i"
     sleep 1
   done
   echo "Exiting..."
   exit 1
   EOF
   ENTRYPOINT /start.sh
   ```

2. Build an image from the Dockerfile.

   从 Dockerfile 构建镜像。

   ```console
   $ docker build -t startstop .
   ```

3. Run a container from the image, specifying `always` for its restart policy.  从镜像运行容器，并指定 `always` 作为重启策略。

   The container prints the numbers 1..5 to stdout, and then exits. This causes the attached CLI to exit as well.

   容器将数字 1 到 5 输出到标准输出，然后退出。这会导致已连接的 CLI 也退出。

   ```console
   $ docker run --restart always startstop
   Starting...
   1
   2
   3
   4
   5
   Exiting...
   $
   ```

4. Running `docker ps` shows that is still running or restarting, thanks to the restart policy. The CLI session has already exited, however. It doesn't survive the initial container exit.

   运行 `docker ps` 显示容器仍在运行或重启，这得益于重启策略。然而，CLI 会话已经退出，无法在初始容器退出后保持连接。

   ```console
   $ docker ps
   CONTAINER ID   IMAGE       COMMAND                  CREATED         STATUS         PORTS     NAMES
   081991b35afe   startstop   "/bin/sh -c /start.sh"   9 seconds ago   Up 4 seconds             gallant_easley
   ```

5. You can re-attach your terminal to the container between restarts, using the `docker container attach` command. It's detached again the next time the container exits.

   在重启期间，可以使用 `docker container attach` 命令重新连接到容器。下次容器退出时，连接会再次断开。

   ```console
   $ docker container attach 081991b35afe
   4
   5
   Exiting...
   $
   ```

## 使用进程管理器 Use a process manager

If restart policies don't suit your needs, such as when processes outside Docker depend on Docker containers, you can use a process manager such as [systemd](https://systemd.io/) or [supervisor](http://supervisord.org/) instead.

​	如果重启策略无法满足需求，例如在 Docker 容器外的进程依赖于 Docker 容器，可以使用 [systemd](https://systemd.io/) 或 [supervisor](http://supervisord.org/) 等进程管理器。

> **Warning**
>
> 
>
> Don't combine Docker restart policies with host-level process managers, as this creates conflicts.
>
> ​	不要将 Docker 重启策略与主机级进程管理器结合使用，否则会产生冲突。

To use a process manager, configure it to start your container or service using the same `docker start` or `docker service` command you would normally use to start the container manually. Consult the documentation for the specific process manager for more details.

​	使用进程管理器时，将其配置为使用与手动启动容器相同的 `docker start` 或 `docker service` 命令启动容器或服务。具体细节请参考特定进程管理器的文档。

### 在容器内使用进程管理器 Using a process manager inside containers

Process managers can also run within the container to check whether a process is running and starts/restart it if not.

​	进程管理器也可在容器内运行，以检查进程是否在运行，如果没有则启动/重启。

> **Warning**
>
> 
>
> These aren't Docker-aware, and only monitor operating system processes within the container. Docker doesn't recommend this approach, because it's platform-dependent and may differ between versions of a given Linux distribution.
>
> ​	这些管理器无法识别 Docker，仅监控容器内的操作系统进程。Docker 不推荐此方法，因为它依赖平台，且在不同 Linux 发行版之间可能有所差异。

