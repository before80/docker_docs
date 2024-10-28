+++
title = "Resource constraints"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/engine/containers/resource_constraints/](https://docs.docker.com/engine/containers/resource_constraints/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Resource constraints - 资源限制

By default, a container has no resource constraints and can use as much of a given resource as the host's kernel scheduler allows. Docker provides ways to control how much memory, or CPU a container can use, setting runtime configuration flags of the `docker run` command. This section provides details on when you should set such limits and the possible implications of setting them.

​	默认情况下，容器没有资源限制，可以使用主机内核调度器允许的资源。Docker 提供了多种方法来控制容器可以使用的内存或 CPU 量，使用 `docker run` 命令的运行时配置标志进行设置。本节提供了设置此类限制时的详细信息以及可能的影响。

Many of these features require your kernel to support Linux capabilities. To check for support, you can use the [`docker info`]({{< ref "/reference/CLIreference/docker/dockersystem/dockerinfo" >}}) command. If a capability is disabled in your kernel, you may see a warning at the end of the output like the following:

​	许多这些功能需要内核支持 Linux 能力。可以使用 `docker info` 命令检查支持情况。如果内核中的某个功能被禁用，输出的末尾可能会出现类似以下的警告：

```console
WARNING: No swap limit support
```

Consult your operating system's documentation for enabling them. See also the [Docker Engine troubleshooting guide](https://docs.docker.com/engine/daemon/troubleshoot/#kernel-cgroup-swap-limit-capabilities) for more information.

​	请查阅操作系统的文档以启用它们。有关详细信息，请参阅 [Docker 引擎故障排除指南](https://docs.docker.com/engine/daemon/troubleshoot/#kernel-cgroup-swap-limit-capabilities)。

## 内存 Memory

### 了解内存不足的风险 Understand the risks of running out of memory

It's important not to allow a running container to consume too much of the host machine's memory. On Linux hosts, if the kernel detects that there isn't enough memory to perform important system functions, it throws an `OOME`, or `Out Of Memory Exception`, and starts killing processes to free up memory. Any process is subject to killing, including Docker and other important applications. This can effectively bring the entire system down if the wrong process is killed.

​	重要的是不要让运行中的容器消耗过多的主机内存。在 Linux 主机上，如果内核检测到内存不足以执行重要的系统功能，会触发 `OOME`（内存不足异常）并开始杀死进程以释放内存。任何进程都有可能被杀死，包括 Docker 和其他重要应用程序。错误的进程被杀死可能导致整个系统瘫痪。

Docker attempts to mitigate these risks by adjusting the OOM priority on the Docker daemon so that it's less likely to be killed than other processes on the system. The OOM priority on containers isn't adjusted. This makes it more likely for an individual container to be killed than for the Docker daemon or other system processes to be killed. You shouldn't try to circumvent these safeguards by manually setting `--oom-score-adj` to an extreme negative number on the daemon or a container, or by setting `--oom-kill-disable` on a container.

​	Docker 通过调整 Docker 守护进程的 OOM 优先级来降低被杀的可能性。容器的 OOM 优先级不会被调整，这使得单个容器比 Docker 守护进程或其他系统进程更容易被杀死。请不要通过将守护进程或容器的 `--oom-score-adj` 手动设置为极负数或在容器上设置 `--oom-kill-disable` 来规避这些保护措施。

For more information about the Linux kernel's OOM management, see [Out of Memory Management](https://www.kernel.org/doc/gorman/html/understand/understand016.html).

​	更多关于 Linux 内核的 OOM 管理信息，请参阅 [内存不足管理](https://www.kernel.org/doc/gorman/html/understand/understand016.html)。

You can mitigate the risk of system instability due to OOME by:

​	可以通过以下方法降低 OOME 导致系统不稳定的风险：

- Perform tests to understand the memory requirements of your application before placing it into production.
  - 在生产环境中部署应用前，测试其内存需求。

- Ensure that your application runs only on hosts with adequate resources.
  - 确保应用程序仅在资源充足的主机上运行。

- Limit the amount of memory your container can use, as described below.
  - 限制容器可以使用的内存量，如下所述。

- Be mindful when configuring swap on your Docker hosts. Swap is slower than memory but can provide a buffer against running out of system memory.
  - 在配置 Docker 主机的 swap 时要小心。swap 比内存慢，但可以提供对系统内存不足的缓冲。

- Consider converting your container to a [service]({{< ref "/manuals/DockerEngine/Swarmmode/Deployservicestoaswarm" >}}), and using service-level constraints and node labels to ensure that the application runs only on hosts with enough memory
  - 考虑将容器转换为 服务，并使用服务级别的约束和节点标签，以确保应用仅在具有足够内存的主机上运行。

### 限制容器对内存的访问 Limit a container's access to memory

Docker can enforce hard or soft memory limits.

​	Docker 可以强制执行硬内存或软内存限制。

- Hard limits lets the container use no more than a fixed amount of memory.
  - 硬限制：允许容器最多使用固定量的内存。

- Soft limits lets the container use as much memory as it needs unless certain conditions are met, such as when the kernel detects low memory or contention on the host machine.
  - 软限制：允许容器根据需要使用尽量多的内存，除非满足特定条件，例如主机检测到低内存或资源竞争。


Some of these options have different effects when used alone or when more than one option is set.

​	这些选项中的一些在单独使用或组合使用时会产生不同的效果。

Most of these options take a positive integer, followed by a suffix of `b`, `k`, `m`, `g`, to indicate bytes, kilobytes, megabytes, or gigabytes.

​	大多数选项接受一个正整数，并带有 `b`、`k`、`m`、`g` 后缀，表示字节、千字节、兆字节或千兆字节。

| Option                 | Description                                                  |
| :--------------------- | :----------------------------------------------------------- |
| `-m` or `--memory=`    | 容器可使用的最大内存量。如果设置此选项，最小值为 `6m`（6 兆字节）。即，值必须至少为 6 兆字节。 The maximum amount of memory the container can use. If you set this option, the minimum allowed value is `6m` (6 megabytes). That is, you must set the value to at least 6 megabytes. |
| `--memory-swap`*       | 容器允许交换到磁盘的内存量。详见 [`--memory-swap` 详情](https://docs.docker.com/engine/containers/resource_constraints/#--memory-swap-details)。 The amount of memory this container is allowed to swap to disk. See [`--memory-swap` details](https://docs.docker.com/engine/containers/resource_constraints/#--memory-swap-details). |
| `--memory-swappiness`  | 默认情况下，主机内核可以将容器使用的匿名页的某个百分比交换出去。可以将 `--memory-swappiness` 设置为 0 到 100 之间的值，以调整此百分比。详见 [`--memory-swappiness` 详情](https://docs.docker.com/engine/containers/resource_constraints/#--memory-swappiness-details)。 By default, the host kernel can swap out a percentage of anonymous pages used by a container. You can set `--memory-swappiness` to a value between 0 and 100, to tune this percentage. See [`--memory-swappiness` details](https://docs.docker.com/engine/containers/resource_constraints/#--memory-swappiness-details). |
| `--memory-reservation` | 允许指定一个小于 `--memory` 的软限制，当 Docker 检测到主机内存不足或竞争时激活。如果使用 `--memory-reservation`，它必须设置得低于 `--memory` 才能生效。由于是软限制，它不保证容器不会超过此限制。 Allows you to specify a soft limit smaller than `--memory` which is activated when Docker detects contention or low memory on the host machine. If you use `--memory-reservation`, it must be set lower than `--memory` for it to take precedence. Because it is a soft limit, it doesn't guarantee that the container doesn't exceed the limit. |
| `--kernel-memory`      | 容器可以使用的最大内核内存量，最小值为 `6m`。因为内核内存无法交换出去，缺少内核内存的容器可能会阻塞主机资源，对主机和其他容器产生影响。详见 [`--kernel-memory` 详情](https://docs.docker.com/engine/containers/resource_constraints/#--kernel-memory-details)。 The maximum amount of kernel memory the container can use. The minimum allowed value is `6m`. Because kernel memory can't be swapped out, a container which is starved of kernel memory may block host machine resources, which can have side effects on the host machine and on other containers. See [`--kernel-memory` details](https://docs.docker.com/engine/containers/resource_constraints/#--kernel-memory-details). |
| `--oom-kill-disable`   | 默认情况下，如果发生内存不足（OOM）错误，内核会杀死容器内的进程。要更改此行为，请使用 `--oom-kill-disable` 选项。仅在设置了 `-m/--memory` 选项的容器上禁用 OOM 杀手。如果未设置 `-m` 标志，主机可能会耗尽内存，内核可能需要杀死主机系统的进程以释放内存。 By default, if an out-of-memory (OOM) error occurs, the kernel kills processes in a container. To change this behavior, use the `--oom-kill-disable` option. Only disable the OOM killer on containers where you have also set the `-m/--memory` option. If the `-m` flag isn't set, the host can run out of memory and the kernel may need to kill the host system's processes to free memory. |

For more information about cgroups and memory in general, see the documentation for [Memory Resource Controller](https://www.kernel.org/doc/Documentation/cgroup-v1/memory.txt).

​	有关 cgroups 和内存的更多信息，请参阅 [内存资源控制器](https://www.kernel.org/doc/Documentation/cgroup-v1/memory.txt) 文档。

### `--memory-swap` details

`--memory-swap` is a modifier flag that only has meaning if `--memory` is also set. Using swap allows the container to write excess memory requirements to disk when the container has exhausted all the RAM that's available to it. There is a performance penalty for applications that swap memory to disk often.

​	`--memory-swap` 是一个修饰标志，仅在同时设置 `--memory` 时有意义。使用 swap 允许容器在耗尽所有可用的 RAM 时，将多余的内存需求写入磁盘。频繁交换内存到磁盘会有性能损失。

Its setting can have complicated effects:

​	设置该标志的效果可能很复杂：

- If `--memory-swap` is set to a positive integer, then both `--memory` and `--memory-swap` must be set. `--memory-swap` represents the total amount of memory and swap that can be used, and `--memory` controls the amount used by non-swap memory. So if `--memory="300m"` and `--memory-swap="1g"`, the container can use 300m of memory and 700m (`1g - 300m`) swap.
  - 如果 `--memory-swap` 设置为正整数，则必须同时设置 `--memory` 和 `--memory-swap`。`--memory-swap` 表示可用的内存和 swap 总量，`--memory` 控制非 swap 内存的用量。例如，如果 `--memory="300m"` 且 `--memory-swap="1g"`，容器可以使用 300m 的内存和 700m 的 swap（`1g - 300m`）。

- If `--memory-swap` is set to `0`, the setting is ignored, and the value is treated as unset.
  - 如果 `--memory-swap` 设置为 `0`，则忽略该设置，将其视为未设置。

- If `--memory-swap` is set to the same value as `--memory`, and `--memory` is set to a positive integer, **the container doesn't have access to swap**. See [Prevent a container from using swap](https://docs.docker.com/engine/containers/resource_constraints/#prevent-a-container-from-using-swap).
  - 如果 `--memory-swap` 和 `--memory` 设置相同，并且 `--memory` 设置为正整数，**容器无法使用 swap**。详见 [禁止容器使用 swap](https://docs.docker.com/engine/containers/resource_constraints/#prevent-a-container-from-using-swap)。

- If `--memory-swap` is unset, and `--memory` is set, the container can use as much swap as the `--memory` setting, if the host container has swap memory configured. For instance, if `--memory="300m"` and `--memory-swap` is not set, the container can use 600m in total of memory and swap.
  - 如果 `--memory-swap` 未设置，且 `--memory` 已设置，则如果主机配置了 swap，容器可以使用与 `--memory` 相同的 swap。例如，如果 `--memory="300m"` 且 `--memory-swap` 未设置，容器总共可以使用 600m 的内存和 swap。

- If `--memory-swap` is explicitly set to `-1`, the container is allowed to use unlimited swap, up to the amount available on the host system.
  - 如果 `--memory-swap` 明确设置为 `-1`，容器可以使用无限的 swap，直到主机系统可用的最大值。

- Inside the container, tools like `free` report the host's available swap, not what's available inside the container. Don't rely on the output of `free` or similar tools to determine whether swap is present.
  - 在容器内部，像 `free` 这样的工具报告的是主机的可用 swap，而不是容器内部的可用 swap。因此，不要依赖 `free` 或类似工具的输出来确定是否存在 swap。

#### 禁止容器使用 swap - Prevent a container from using swap

If `--memory` and `--memory-swap` are set to the same value, this prevents containers from using any swap. This is because `--memory-swap` is the amount of combined memory and swap that can be used, while `--memory` is only the amount of physical memory that can be used.

​	如果 `--memory` 和 `--memory-swap` 设置为相同的值，则容器将无法使用任何 swap。这是因为 `--memory-swap` 是可以使用的总内存和 swap，而 `--memory` 仅表示可用的物理内存。

### `--memory-swappiness` details

- A value of 0 turns off anonymous page swapping.

  - 值为 0 禁用匿名页交换。

- A value of 100 sets all anonymous pages as swappable.

  - 值为 100 允许所有匿名页交换。

- By default, if you don't set `--memory-swappiness`, the value is inherited from the host machine.

  - 默认情况下，如果未设置 `--memory-swappiness`，则使用主机的值。

  

### `--kernel-memory` details

Kernel memory limits are expressed in terms of the overall memory allocated to a container. Consider the following scenarios:

​	内核内存限制是基于容器的总分配内存来设定的。考虑以下几种场景：

- **Unlimited memory, unlimited kernel memory**: This is the default behavior.
  - **无限内存，无限内核内存**：这是默认行为。

- **Unlimited memory, limited kernel memory**: This is appropriate when the amount of memory needed by all cgroups is greater than the amount of memory that actually exists on the host machine. You can configure the kernel memory to never go over what's available on the host machine, and containers which need more memory need to wait for it.
  - **无限内存，有限内核内存**：适用于所有 cgroup 需要的内存大于主机实际存在的内存的情况。可以配置内核内存限制，以确保它不会超过主机可用内存，并让需要更多内存的容器等待。

- **Limited memory, unlimited kernel memory**: The overall memory is limited, but the kernel memory isn't.
  - **有限内存，无限内核内存**：整体内存有限制，但内核内存不受限。

- **Limited memory, limited kernel memory**: Limiting both user and kernel memory can be useful for debugging memory-related problems. If a container is using an unexpected amount of either type of memory, it runs out of memory without affecting other containers or the host machine. Within this setting, if the kernel memory limit is lower than the user memory limit, running out of kernel memory causes the container to experience an OOM error. If the kernel memory limit is higher than the user memory limit, the kernel limit doesn't cause the container to experience an OOM.
  - **有限内存，有限内核内存**：同时限制用户内存和内核内存，对于调试内存相关问题非常有用。如果容器使用的内存类型超出预期，内存会被耗尽而不会影响其他容器或主机系统。在此设置中，如果内核内存限制低于用户内存限制，耗尽内核内存会导致容器遇到 OOM 错误；如果内核内存限制高于用户内存限制，则内核限制不会导致容器遇到 OOM 错误。


When you enable kernel memory limits, the host machine tracks "high water mark" statistics on a per-process basis, so you can track which processes (in this case, containers) are using excess memory. This can be seen per process by viewing `/proc/<PID>/status` on the host machine.

​	启用内核内存限制后，主机会按进程跟踪“高水位线”统计信息，因此可以跟踪哪个进程（在此情况下为容器）在使用过量内存。这可以通过查看主机上的 `/proc/<PID>/status` 文件实现。

## CPU

By default, each container's access to the host machine's CPU cycles is unlimited. You can set various constraints to limit a given container's access to the host machine's CPU cycles. Most users use and configure the [default CFS scheduler](https://docs.docker.com/engine/containers/resource_constraints/#configure-the-default-cfs-scheduler). You can also configure the [real-time scheduler](https://docs.docker.com/engine/containers/resource_constraints/#configure-the-real-time-scheduler).

​	默认情况下，每个容器对主机 CPU 周期的访问不受限制。可以设置各种约束来限制容器对主机 CPU 周期的访问。大多数用户使用并配置 [默认 CFS 调度器](https://docs.docker.com/engine/containers/resource_constraints/#configure-the-default-cfs-scheduler)。您还可以配置 [实时调度器](https://docs.docker.com/engine/containers/resource_constraints/#configure-the-real-time-scheduler)。

### 配置默认 CFS 调度器 Configure the default CFS scheduler

The CFS is the Linux kernel CPU scheduler for normal Linux processes. Several runtime flags let you configure the amount of access to CPU resources your container has. When you use these settings, Docker modifies the settings for the container's cgroup on the host machine.

​	CFS 是 Linux 内核用于常规进程的 CPU 调度器。几个运行时标志可以用来配置容器的 CPU 资源访问量。使用这些设置时，Docker 会修改容器在主机上的 cgroup 设置。

| Option                 | Description                                                  |
| :--------------------- | :----------------------------------------------------------- |
| `--cpus=<value>`       | 指定容器可以使用的 CPU 资源量。例如，如果主机有两个 CPU，并设置 `--cpus="1.5"`，容器最多可使用一个半 CPU。这相当于设置 `--cpu-period="100000"` 和 `--cpu-quota="150000"`。 Specify how much of the available CPU resources a container can use. For instance, if the host machine has two CPUs and you set `--cpus="1.5"`, the container is guaranteed at most one and a half of the CPUs. This is the equivalent of setting `--cpu-period="100000"` and `--cpu-quota="150000"`. |
| `--cpu-period=<value>` | 指定 CPU CFS 调度器周期，与 `--cpu-quota` 配合使用。默认为 100000 微秒（100 毫秒）。大多数情况下无需更改默认设置。对于大多数用例，`--cpus` 是更便捷的替代方案。 Specify the CPU CFS scheduler period, which is used alongside `--cpu-quota`. Defaults to 100000 microseconds (100 milliseconds). Most users don't change this from the default. For most use-cases, `--cpus` is a more convenient alternative. |
| `--cpu-quota=<value>`  | 对容器施加 CPU CFS 配额。在每个 `--cpu-period` 期间容器的 CPU 使用量上限。对于大多数用例，`--cpus` 是更便捷的替代方案。 Impose a CPU CFS quota on the container. The number of microseconds per `--cpu-period` that the container is limited to before throttled. As such acting as the effective ceiling. For most use-cases, `--cpus` is a more convenient alternative. |
| `--cpuset-cpus`        | 限制容器可以使用的特定 CPU 或核心。如果有多个 CPU，可以是逗号分隔的列表或用连字符分隔的范围。 Limit the specific CPUs or cores a container can use. A comma-separated list or hyphen-separated range of CPUs a container can use, if you have more than one CPU. The first CPU is numbered 0. A valid value might be `0-3` (to use the first, second, third, and fourth CPU) or `1,3` (to use the second and fourth CPU). |
| `--cpu-shares`         | 设置一个值（大于或小于默认值 1024）以增加或减少容器的权重，影响其对主机 CPU 周期的使用。仅在 CPU 周期受限时才生效。 Set this flag to a value greater or less than the default of 1024 to increase or reduce the container's weight, and give it access to a greater or lesser proportion of the host machine's CPU cycles. This is only enforced when CPU cycles are constrained. When plenty of CPU cycles are available, all containers use as much CPU as they need. In that way, this is a soft limit. `--cpu-shares` doesn't prevent containers from being scheduled in Swarm mode. It prioritizes container CPU resources for the available CPU cycles. It doesn't guarantee or reserve any specific CPU access. |

If you have 1 CPU, each of the following commands guarantees the container at most 50% of the CPU every second.

​	如果只有 1 个 CPU，以下命令每秒保证容器最多使用 50% 的 CPU：

```console
$ docker run -it --cpus=".5" ubuntu /bin/bash
```

Which is the equivalent to manually specifying `--cpu-period` and `--cpu-quota`;

​	相当于手动指定 `--cpu-period` 和 `--cpu-quota`：

```console
$ docker run -it --cpu-period=100000 --cpu-quota=50000 ubuntu /bin/bash
```

### 配置实时调度器 Configure the real-time scheduler

You can configure your container to use the real-time scheduler, for tasks which can't use the CFS scheduler. You need to [make sure the host machine's kernel is configured correctly](https://docs.docker.com/engine/containers/resource_constraints/#configure-the-host-machines-kernel) before you can [configure the Docker daemon](https://docs.docker.com/engine/containers/resource_constraints/#configure-the-docker-daemon) or [configure individual containers](https://docs.docker.com/engine/containers/resource_constraints/#configure-individual-containers).

​	您可以将容器配置为使用实时调度器，适用于无法使用 CFS 调度器的任务。需要先[确保主机的内核配置正确](https://docs.docker.com/engine/containers/resource_constraints/#configure-the-host-machines-kernel)，然后才能[配置 Docker 守护进程](https://docs.docker.com/engine/containers/resource_constraints/#configure-the-docker-daemon)或[配置单个容器](https://docs.docker.com/engine/containers/resource_constraints/#configure-individual-containers)。

> **Warning**
>
> 
>
> CPU scheduling and prioritization are advanced kernel-level features. Most users don't need to change these values from their defaults. Setting these values incorrectly can cause your host system to become unstable or unusable.
>
> ​	CPU 调度和优先级是高级的内核级特性。大多数用户不需要更改这些默认值。不正确地设置这些值可能会导致主机系统不稳定或无法使用。

#### 配置主机的内核 Configure the host machine's kernel

Verify that `CONFIG_RT_GROUP_SCHED` is enabled in the Linux kernel by running `zcat /proc/config.gz | grep CONFIG_RT_GROUP_SCHED` or by checking for the existence of the file `/sys/fs/cgroup/cpu.rt_runtime_us`. For guidance on configuring the kernel real-time scheduler, consult the documentation for your operating system.

​	通过运行 `zcat /proc/config.gz | grep CONFIG_RT_GROUP_SCHED` 或检查 `/sys/fs/cgroup/cpu.rt_runtime_us` 文件，确认 Linux 内核中是否启用了 `CONFIG_RT_GROUP_SCHED`。有关配置内核实时调度器的指导，请参考操作系统的文档。

#### 配置 Docker 守护进程 Configure the Docker daemon

To run containers using the real-time scheduler, run the Docker daemon with the `--cpu-rt-runtime` flag set to the maximum number of microseconds reserved for real-time tasks per runtime period. For instance, with the default period of 1000000 microseconds (1 second), setting `--cpu-rt-runtime=950000` ensures that containers using the real-time scheduler can run for 950000 microseconds for every 1000000-microsecond period, leaving at least 50000 microseconds available for non-real-time tasks. To make this configuration permanent on systems which use `systemd`, create a systemd unit file for the `docker` service. For an example, see the instruction on how to configure the daemon to use a proxy with a [systemd unit file](https://docs.docker.com/engine/daemon/proxy/#systemd-unit-file).

​	要使用实时调度器运行容器，运行 Docker 守护进程时添加 `--cpu-rt-runtime` 标志，设置每个运行周期保留给实时任务的最大微秒数。例如，默认周期为 1000000 微秒（1 秒），设置 `--cpu-rt-runtime=950000` 可以确保使用实时调度器的容器在每个 1000000 微秒周期内可以运行 950000 微秒，将至少 50000 微秒留给非实时任务。要在使用 `systemd` 的系统上永久配置该设置，可以为 `docker` 服务创建 systemd 单元文件。有关示例，请参见如何使用[systemd 单元文件](https://docs.docker.com/engine/daemon/proxy/#systemd-unit-file)配置守护进程以使用代理的说明。

#### 配置单个容器 Configure individual containers

You can pass several flags to control a container's CPU priority when you start the container using `docker run`. Consult your operating system's documentation or the `ulimit` command for information on appropriate values.

​	启动容器时可以传递多个标志来控制容器的 CPU 优先级。请参考操作系统的文档或 `ulimit` 命令来获取合适的值。

| Option                     | Description 描述                                             |
| :------------------------- | :----------------------------------------------------------- |
| `--cap-add=sys_nice`       | 授予容器 `CAP_SYS_NICE` 能力，允许容器提升进程优先级、设置实时调度策略、设置 CPU 亲和性等。 Grants the container the `CAP_SYS_NICE` capability, which allows the container to raise process `nice` values, set real-time scheduling policies, set CPU affinity, and other operations. |
| `--cpu-rt-runtime=<value>` | 设置容器在 Docker 守护进程的实时调度器周期内的最大实时优先运行微秒数。需要 `--cap-add=sys_nice` 标志。 The maximum number of microseconds the container can run at real-time priority within the Docker daemon's real-time scheduler period. You also need the `--cap-add=sys_nice` flag. |
| `--ulimit rtprio=<value>`  | 容器允许的最大实时优先级。需要 `--cap-add=sys_nice` 标志。 The maximum real-time priority allowed for the container. You also need the `--cap-add=sys_nice` flag. |

The following example command sets each of these three flags on a `debian:jessie` container.

​	以下示例命令在 `debian:jessie` 容器上设置这些标志：

```console
$ docker run -it \
    --cpu-rt-runtime=950000 \
    --ulimit rtprio=99 \
    --cap-add=sys_nice \
    debian:jessie
```

If the kernel or Docker daemon isn't configured correctly, an error occurs.

​	如果内核或 Docker 守护进程配置不正确，则会出现错误。

## GPU

### Access an NVIDIA GPU

#### 前置条件 Prerequisites

Visit the official [NVIDIA drivers page](https://www.nvidia.com/Download/index.aspx) to download and install the proper drivers. Reboot your system once you have done so.

​	访问[NVIDIA 驱动程序页面](https://www.nvidia.com/Download/index.aspx)下载并安装合适的驱动程序，安装后重启系统。

Verify that your GPU is running and accessible.

​	确认 GPU 正在运行且可以访问。

#### Install nvidia-container-toolkit

Follow the official NVIDIA Container Toolkit [installation instructions](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html).

​	按照官方 NVIDIA Container Toolkit 的[安装指南](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)进行安装。

#### 暴露 GPU 供使用 Expose GPUs for use

Include the `--gpus` flag when you start a container to access GPU resources. Specify how many GPUs to use. For example:

​	启动容器时包含 `--gpus` 标志以访问 GPU 资源。指定要使用的 GPU 数量。例如：

```console
$ docker run -it --rm --gpus all ubuntu nvidia-smi
```

Exposes all available GPUs and returns a result akin to the following:

​	暴露所有可用 GPU，并返回类似以下的结果：

```bash
+-------------------------------------------------------------------------------+
| NVIDIA-SMI 384.130            	Driver Version: 384.130               	|
|-------------------------------+----------------------+------------------------+
| GPU  Name 	   Persistence-M| Bus-Id    	Disp.A | Volatile Uncorr. ECC   |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M.   |
|===============================+======================+========================|
|   0  GRID K520       	Off  | 00000000:00:03.0 Off |                  N/A      |
| N/A   36C	P0    39W / 125W |  	0MiB /  4036MiB |      0%  	Default |
+-------------------------------+----------------------+------------------------+
+-------------------------------------------------------------------------------+
| Processes:                                                       GPU Memory   |
|  GPU   	PID   Type   Process name                         	Usage  	|
|===============================================================================|
|  No running processes found                                                   |
+-------------------------------------------------------------------------------+
```

Use the `device` option to specify GPUs. For example:

​	使用 `device` 选项指定 GPU，例如：

```console
$ docker run -it --rm --gpus device=GPU-3a23c669-1f69-c64e-cf85-44e9b07e7a2a ubuntu nvidia-smi
```

Exposes that specific GPU.

​	暴露该特定 GPU。

```console
$ docker run -it --rm --gpus '"device=0,2"' ubuntu nvidia-smi
```

Exposes the first and third GPUs.

​	暴露第一个和第三个 GPU。

> **Note**
>
> 
>
> NVIDIA GPUs can only be accessed by systems running a single engine.
>
> ​	NVIDIA GPU 只能由运行单个引擎的系统访问。

#### 设置 NVIDIA 功能 Set NVIDIA capabilities

You can set capabilities manually. For example, on Ubuntu you can run the following:

​	可以手动设置功能。例如，在 Ubuntu 上可以运行以下命令：



```console
$ docker run --gpus 'all,capabilities=utility' --rm ubuntu nvidia-smi
```

This enables the `utility` driver capability which adds the `nvidia-smi` tool to the container.

​	这会启用 `utility` 驱动功能并将 `nvidia-smi` 工具添加到容器中。

Capabilities as well as other configurations can be set in images via environment variables. More information on valid variables can be found in the [nvidia-container-toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/docker-specialized.html) documentation. These variables can be set in a Dockerfile.

​	功能以及其他配置可以通过环境变量在镜像中设置。更多有效变量的信息可以在 [nvidia-container-toolkit 文档](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/docker-specialized.html)中找到。可以在 Dockerfile 中设置这些变量。

You can also use CUDA images which sets these variables automatically. See the official [CUDA images](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/cuda) NGC catalog page.

​	您还可以使用自动设置这些变量的 CUDA 镜像。参见官方 [CUDA 镜像](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/cuda) NGC 目录页面。
