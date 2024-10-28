+++
title = "Docker 统计信息"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/containers/runmetrics/](https://docs.docker.com/engine/containers/runmetrics/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Runtime metrics - 运行时指标

### 

## Docker stats

You can use the `docker stats` command to live stream a container's runtime metrics. The command supports CPU, memory usage, memory limit, and network IO metrics.

​	可以使用 `docker stats` 命令实时流式传输容器的运行时指标。该命令支持 CPU、内存使用率、内存限制和网络 IO 指标。

The following is a sample output from the `docker stats` command

​	以下是 `docker stats` 命令的示例输出：



```console
$ docker stats redis1 redis2

CONTAINER           CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O
redis1              0.07%               796 KB / 64 MB        1.21%               788 B / 648 B       3.568 MB / 512 KB
redis2              0.07%               2.746 MB / 64 MB      4.29%               1.266 KB / 648 B    12.4 MB / 0 B
```

The [`docker stats`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainerstats" >}}) reference page has more details about the `docker stats` command.

​	`docker stats` 参考页面有关于 `docker stats` 命令的更多详细信息。



### 控制组 Control groups

Linux Containers rely on [control groups](https://www.kernel.org/doc/Documentation/cgroup-v1/cgroups.txt) which not only track groups of processes, but also expose metrics about CPU, memory, and block I/O usage. You can access those metrics and obtain network usage metrics as well. This is relevant for "pure" LXC containers, as well as for Docker containers.

​	Linux 容器依赖于 [控制组（control groups）](https://www.kernel.org/doc/Documentation/cgroup-v1/cgroups.txt)，它不仅可以跟踪进程组，还可以公开 CPU、内存和块 IO 使用情况的指标。可以通过控制组访问这些指标，并获得网络使用情况指标。这对“纯” LXC 容器以及 Docker 容器都适用。

Control groups are exposed through a pseudo-filesystem. In modern distros, you should find this filesystem under `/sys/fs/cgroup`. Under that directory, you see multiple sub-directories, called `devices`, `freezer`, `blkio`, and so on. Each sub-directory actually corresponds to a different cgroup hierarchy.

​	控制组通过伪文件系统暴露。在现代发行版中，可以在 `/sys/fs/cgroup` 下找到该文件系统。在该目录下，可以看到多个子目录，如 `devices`、`freezer`、`blkio` 等。每个子目录实际上对应不同的 cgroup 层次结构。

On older systems, the control groups might be mounted on `/cgroup`, without distinct hierarchies. In that case, instead of seeing the sub-directories, you see a bunch of files in that directory, and possibly some directories corresponding to existing containers.

​	在旧系统中，控制组可能挂载在 `/cgroup` 上，且没有区分层次结构。在这种情况下，可能不会看到子目录，而是看到该目录中的一堆文件，以及可能对应现有容器的某些目录。

To figure out where your control groups are mounted, you can run:

​	要确定控制组挂载的位置，可以运行：

```console
$ grep cgroup /proc/mounts
```

### 枚举控制组 Enumerate cgroups

The file layout of cgroups is significantly different between v1 and v2.

​	cgroup 的文件布局在 v1 和 v2 之间显著不同。

If `/sys/fs/cgroup/cgroup.controllers` is present on your system, you are using v2, otherwise you are using v1. Refer to the subsection that corresponds to your cgroup version.

​	如果系统上存在 `/sys/fs/cgroup/cgroup.controllers` 文件，则使用的是 v2，否则使用的是 v1。请参阅与您的 cgroup 版本相对应的小节。

cgroup v2 is used by default on the following distributions:

​	以下发行版默认使用 cgroup v2：

- Fedora (since 31)

- Debian GNU/Linux (since 11)
- Ubuntu (since 21.10)

#### cgroup v1

You can look into `/proc/cgroups` to see the different control group subsystems known to the system, the hierarchy they belong to, and how many groups they contain.

​	可以查看 `/proc/cgroups` 以了解系统已知的不同控制组子系统、它们所属的层次结构以及它们包含的组数。

You can also look at `/proc/<pid>/cgroup` to see which control groups a process belongs to. The control group is shown as a path relative to the root of the hierarchy mountpoint. `/` means the process hasn't been assigned to a group, while `/lxc/pumpkin` indicates that the process is a member of a container named `pumpkin`.

​	还可以查看 `/proc/<pid>/cgroup` 以查看进程所属的控制组。控制组显示为相对于层次结构挂载点根目录的路径。`/` 表示该进程未分配到组，而 `/lxc/pumpkin` 表示该进程是名为 `pumpkin` 的容器的成员。

#### cgroup v2

On cgroup v2 hosts, the content of `/proc/cgroups` isn't meaningful. See `/sys/fs/cgroup/cgroup.controllers` to the available controllers.

​	在使用 cgroup v2 的主机上，`/proc/cgroups` 的内容没有意义。可查看 `/sys/fs/cgroup/cgroup.controllers` 以获取可用控制器。

### Changing cgroup version

Changing cgroup version requires rebooting the entire system.

​	更改 cgroup 版本需要重新启动整个系统。

On systemd-based systems, cgroup v2 can be enabled by adding `systemd.unified_cgroup_hierarchy=1` to the kernel command line. To revert the cgroup version to v1, you need to set `systemd.unified_cgroup_hierarchy=0` instead.

​	在基于 systemd 的系统上，可以通过在内核命令行中添加 `systemd.unified_cgroup_hierarchy=1` 来启用 cgroup v2。要将 cgroup 版本恢复为 v1，则需要设置 `systemd.unified_cgroup_hierarchy=0`。

If `grubby` command is available on your system (e.g. on Fedora), the command line can be modified as follows:

​	如果系统上有 `grubby` 命令（例如在 Fedora 上），可以如下修改命令行：



```console
$ sudo grubby --update-kernel=ALL --args="systemd.unified_cgroup_hierarchy=1"
```

If `grubby` command isn't available, edit the `GRUB_CMDLINE_LINUX` line in `/etc/default/grub` and run `sudo update-grub`.

​	如果系统没有 `grubby` 命令，请编辑 `/etc/default/grub` 中的 `GRUB_CMDLINE_LINUX` 行并运行 `sudo update-grub`。

### 在 cgroup v2 上运行 Docker - Running Docker on cgroup v2

Docker supports cgroup v2 since Docker 20.10. Running Docker on cgroup v2 also requires the following conditions to be satisfied:

​	Docker 自 20.10 版本起支持 cgroup v2。运行 Docker 在 cgroup v2 上还需要满足以下条件：

- containerd: v1.4 or later
  - containerd：v1.4 或更高版本

- runc: v1.0.0-rc91 or later
  - runc：v1.0.0-rc91 或更高版本

- Kernel: v4.15 or later (v5.2 or later is recommended)
  - 内核：v4.15 或更高版本（推荐 v5.2 或更高版本）


Note that the cgroup v2 mode behaves slightly different from the cgroup v1 mode:

​	请注意，cgroup v2 模式的行为与 cgroup v1 模式略有不同：

- 
  - The default cgroup driver (`dockerd --exec-opt native.cgroupdriver`) is `systemd` on v2, `cgroupfs` on v1.
    - 默认的 cgroup 驱动程序（`dockerd --exec-opt native.cgroupdriver`）在 v2 上为 `systemd`，在 v1 上为 `cgroupfs`。

- The default cgroup namespace mode (`docker run --cgroupns`) is `private` on v2, `host` on v1.
  - 默认的 cgroup 命名空间模式（`docker run --cgroupns`）在 v2 上为 `private`，在 v1 上为 `host`。

- The `docker run` flags `--oom-kill-disable` and `--kernel-memory` are discarded on v2.
  - `docker run` 标志 `--oom-kill-disable` 和 `--kernel-memory` 在 v2 上被忽略。

### 查找给定容器的 cgroup - Find the cgroup for a given container

For each container, one cgroup is created in each hierarchy. On older systems with older versions of the LXC userland tools, the name of the cgroup is the name of the container. With more recent versions of the LXC tools, the cgroup is `lxc/<container_name>.`

​	对于每个容器，在每个层次结构中都会创建一个 cgroup。在使用较旧的 LXC 用户工具的系统上，cgroup 的名称就是容器的名称。对于使用较新版本 LXC 工具的系统，cgroup 是 `lxc/<container_name>`。

For Docker containers using cgroups, the container name is the full ID or long ID of the container. If a container shows up as ae836c95b4c3 in `docker ps`, its long ID might be something like `ae836c95b4c3c9e9179e0e91015512da89fdec91612f63cebae57df9a5444c79`. You can look it up with `docker inspect` or `docker ps --no-trunc`.

​	对于使用 cgroup 的 Docker 容器，容器名称是完整的 ID 或长 ID。如果一个容器在 `docker ps` 中显示为 ae836c95b4c3，其长 ID 可能类似于 `ae836c95b4c3c9e9179e0e91015512da89fdec91612f63cebae57df9a5444c79`。可以通过 `docker inspect` 或 `docker ps --no-trunc` 查找它。

Putting everything together to look at the memory metrics for a Docker container, take a look at the following paths:

​	要查看 Docker 容器的内存指标，可以查看以下路径：

- `/sys/fs/cgroup/memory/docker/<longid>/` on cgroup v1, `cgroupfs` driver
- `/sys/fs/cgroup/memory/system.slice/docker-<longid>.scope/` on cgroup v1, `systemd` driver
- `/sys/fs/cgroup/docker/<longid>/` on cgroup v2, `cgroupfs` driver
- `/sys/fs/cgroup/system.slice/docker-<longid>.scope/` on cgroup v2, `systemd` driver

### 来自控制组的指标：内存、CPU、块 I/O - Metrics from cgroups: memory, CPU, block I/O

> **Note**
>
> 
>
> This section isn't yet updated for cgroup v2. For further information about cgroup v2, refer to [the kernel documentation](https://www.kernel.org/doc/html/latest/admin-guide/cgroup-v2.html).
>
> ​	本节尚未针对 cgroup v2 进行更新。有关 cgroup v2 的更多信息，请参阅 [内核文档](https://www.kernel.org/doc/html/latest/admin-guide/cgroup-v2.html)。

For each subsystem (memory, CPU, and block I/O), one or more pseudo-files exist and contain statistics.

#### Memory metrics: `memory.stat`

Memory metrics are found in the `memory` cgroup. The memory control group adds a little overhead, because it does very fine-grained accounting of the memory usage on your host. Therefore, many distros chose to not enable it by default. Generally, to enable it, all you have to do is to add some kernel command-line parameters: `cgroup_enable=memory swapaccount=1`.

​	内存指标在 `memory` cgroup 中找到。内存控制组会增加一些开销，因为它对主机的内存使用情况进行了非常细粒度的计算。因此，许多发行版选择默认不启用它。通常，启用它只需添加一些内核命令行参数：`cgroup_enable=memory swapaccount=1`。

The metrics are in the pseudo-file `memory.stat`. Here is what it looks like:

​	指标位于伪文件 `memory.stat` 中。其内容如下所示：

```
cache 11492564992
rss 1930993664
mapped_file 306728960
pgpgin 406632648
pgpgout 403355412
swap 0
pgfault 728281223
pgmajfault 1724
inactive_anon 46608384
active_anon 1884520448
inactive_file 7003344896
active_file 4489052160
unevictable 32768
hierarchical_memory_limit 9223372036854775807
hierarchical_memsw_limit 9223372036854775807
total_cache 11492564992
total_rss 1930993664
total_mapped_file 306728960
total_pgpgin 406632648
total_pgpgout 403355412
total_swap 0
total_pgfault 728281223
total_pgmajfault 1724
total_inactive_anon 46608384
total_active_anon 1884520448
total_inactive_file 7003344896
total_active_file 4489052160
total_unevictable 32768
```

The first half (without the `total_` prefix) contains statistics relevant to the processes within the cgroup, excluding sub-cgroups. The second half (with the `total_` prefix) includes sub-cgroups as well.

​	前半部分（没有 `total_` 前缀）包含与 cgroup 内的进程相关的统计数据，不包括子 cgroup。后半部分（带有 `total_` 前缀）则包含了子 cgroup 的统计数据。

Some metrics are "gauges", or values that can increase or decrease. For instance, `swap` is the amount of swap space used by the members of the cgroup. Some others are "counters", or values that can only go up, because they represent occurrences of a specific event. For instance, `pgfault` indicates the number of page faults since the creation of the cgroup.

​	一些指标是“量表”类型，值可以增加或减少。例如，`swap` 表示该 cgroup 成员使用的交换空间量。其他指标是“计数器”，只能上升，因为它们表示特定事件的发生次数。例如，`pgfault` 表示自创建该 cgroup 以来发生的页面错误数量。

- `cache`

  The amount of memory used by the processes of this control group that can be associated precisely with a block on a block device. When you read from and write to files on disk, this amount increases. This is the case if you use "conventional" I/O (`open`, `read`, `write` syscalls) as well as mapped files (with `mmap`). It also accounts for the memory used by `tmpfs` mounts, though the reasons are unclear.

  ​	此 cgroup 中进程使用的、可以准确关联到块设备上的一个块的内存量。当从磁盘读取或写入文件时，这个值会增加。无论是使用常规 I/O（`open`、`read`、`write` 系统调用）还是映射文件（通过 `mmap`），都会发生这种情况。它还包括 `tmpfs` 挂载使用的内存，但原因尚不清楚。

- `rss`

  The amount of memory that doesn't correspond to anything on disk: stacks, heaps, and anonymous memory maps.

  ​	表示不对应于磁盘上任何数据的内存量：堆栈、堆以及匿名内存映射。

- `mapped_file`

  Indicates the amount of memory mapped by the processes in the control group. It doesn't give you information about how much memory is used; it rather tells you how it's used.

  ​	表示该 cgroup 中的进程映射的内存量。它并未显示实际使用了多少内存，而是展示了内存的使用方式。

- `pgfault`, `pgmajfault`

  Indicate the number of times that a process of the cgroup triggered a "page fault" and a "major fault", respectively. A page fault happens when a process accesses a part of its virtual memory space which is nonexistent or protected. The former can happen if the process is buggy and tries to access an invalid address (it is sent a `SIGSEGV` signal, typically killing it with the famous `Segmentation fault` message). The latter can happen when the process reads from a memory zone which has been swapped out, or which corresponds to a mapped file: in that case, the kernel loads the page from disk, and let the CPU complete the memory access. It can also happen when the process writes to a copy-on-write memory zone: likewise, the kernel preempts the process, duplicate the memory page, and resume the write operation on the process's own copy of the page. "Major" faults happen when the kernel actually needs to read the data from disk. When it just duplicates an existing page, or allocate an empty page, it's a regular (or "minor") fault.

  ​	分别指示该 cgroup 内的进程触发“页面错误”和“重大错误”的次数。页面错误发生在进程访问不存在或受保护的虚拟内存空间部分时。前者可能由于进程存在漏洞，访问了无效地址（通常发送 `SIGSEGV` 信号，从而以著名的“段错误”信息终止进程）。后者可能发生在进程读取已交换出的内存区域或对应于映射文件时，此时内核从磁盘加载页面，并让 CPU 完成内存访问。当进程对写时复制的内存区域进行写操作时，也可能发生这种情况：内核会暂时中断进程，复制内存页面，并在进程自己的页面副本上恢复写操作。重大错误发生在内核实际需要从磁盘读取数据时。当只是复制现有页面或分配一个空页面时，则是常规（或称为“次要”）错误。

- `swap`

  The amount of swap currently used by the processes in this cgroup.

  ​	表示当前该 cgroup 内进程使用的交换空间量。

- `active_anon`, `inactive_anon`

  The amount of anonymous memory that has been identified has respectively *active* and *inactive* by the kernel. "Anonymous" memory is the memory that is *not* linked to disk pages. In other words, that's the equivalent of the rss counter described above. In fact, the very definition of the rss counter is `active_anon` + `inactive_anon` - `tmpfs` (where tmpfs is the amount of memory used up by `tmpfs` filesystems mounted by this control group). Now, what's the difference between "active" and "inactive"? Pages are initially "active"; and at regular intervals, the kernel sweeps over the memory, and tags some pages as "inactive". Whenever they're accessed again, they're immediately re-tagged "active". When the kernel is almost out of memory, and time comes to swap out to disk, the kernel swaps "inactive" pages.

  ​	表示内核分别标记为 *active*（活动）和 *inactive*（非活动）的匿名内存量。“匿名”内存是与磁盘页面没有关联的内存。换句话说，这相当于上面描述的 rss 计数器。实际上，rss 计数器的定义是 `active_anon` + `inactive_anon` - `tmpfs`（其中 tmpfs 是该 cgroup 挂载的 `tmpfs` 文件系统使用的内存量）。那么，“活动”和“非活动”有什么区别呢？页面最初为“活动”状态；内核会定期扫描内存，将一些页面标记为“非活动”。一旦页面再次被访问，它们会立即重新标记为“活动”。当内存几乎耗尽时，内核会交换“非活动”页面。

- `active_file`, `inactive_file`

  Cache memory, with *active* and *inactive* similar to the *anon* memory above. The exact formula is `cache` = `active_file` + `inactive_file` + `tmpfs`. The exact rules used by the kernel to move memory pages between active and inactive sets are different from the ones used for anonymous memory, but the general principle is the same. When the kernel needs to reclaim memory, it's cheaper to reclaim a clean (=non modified) page from this pool, since it can be reclaimed immediately (while anonymous pages and dirty/modified pages need to be written to disk first).

  ​	缓存内存，*active* 和 *inactive* 类似于上述 *anon* 内存。具体公式为 `cache` = `active_file` + `inactive_file` + `tmpfs`。内核将页面在活动和非活动集合之间移动的规则与匿名内存不同，但原理相同。当内核需要回收内存时，从这个池中回收未修改的页面更为便宜（而匿名页面和脏/已修改的页面则需要先写入磁盘）。

- `unevictable`

  The amount of memory that cannot be reclaimed; generally, it accounts for memory that has been "locked" with `mlock`. It's often used by crypto frameworks to make sure that secret keys and other sensitive material never gets swapped out to disk.

  ​	无法回收的内存量；通常指被 `mlock` 锁定的内存。此类内存通常用于加密框架，以确保密钥和其他敏感材料永远不会被交换到磁盘上。

- `memory_limit`, `memsw_limit`

  These aren't really metrics, but a reminder of the limits applied to this cgroup. The first one indicates the maximum amount of physical memory that can be used by the processes of this control group; the second one indicates the maximum amount of RAM+swap.
  
  ​	这些并非真正的指标，而是对应用于该 cgroup 的限制的提醒。第一个表示此控制组内进程可用的最大物理内存量；第二个表示最大 RAM + 交换内存量。

Accounting for memory in the page cache is very complex. If two processes in different control groups both read the same file (ultimately relying on the same blocks on disk), the corresponding memory charge is split between the control groups. It's nice, but it also means that when a cgroup is terminated, it could increase the memory usage of another cgroup, because they're not splitting the cost anymore for those memory pages.

​	页面缓存中的内存核算非常复杂。如果不同控制组中的两个进程都读取相同的文件（最终依赖于磁盘上的相同块），则相应的内存消耗将在控制组之间分摊。这种机制很好，但也意味着当某个 cgroup 被终止时，可能会增加另一个 cgroup 的内存使用量，因为这些页面不再共同分担成本。

### CPU metrics: `cpuacct.stat`

Now that we've covered memory metrics, everything else is simple in comparison. CPU metrics are in the `cpuacct` controller.

​	在介绍了内存指标之后，CPU 指标相对简单得多。CPU 指标位于 `cpuacct` 控制器中。

For each container, a pseudo-file `cpuacct.stat` contains the CPU usage accumulated by the processes of the container, broken down into `user` and `system` time. The distinction is:

​	对于每个容器，`cpuacct.stat` 伪文件包含了容器进程累积的 CPU 使用情况，分为 `user` 和 `system` 时间。二者的区别如下：

- `user` time is the amount of time a process has direct control of the CPU, executing process code.
  - `user` 时间：进程直接控制 CPU 执行进程代码的时间。

- `system` time is the time the kernel is executing system calls on behalf of the process.
  - `system` 时间：内核为进程执行系统调用的时间。


Those times are expressed in ticks of 1/100th of a second, also called "user jiffies". There are `USER_HZ` *"jiffies"* per second, and on x86 systems, `USER_HZ` is 100. Historically, this mapped exactly to the number of scheduler "ticks" per second, but higher frequency scheduling and [tickless kernels](https://lwn.net/Articles/549580/) have made the number of ticks irrelevant.

​	这些时间以 1/100 秒的刻度（称为“用户 jiffies”）表示。每秒有 `USER_HZ` 个 "jiffies"，在 x86 系统上，`USER_HZ` 为 100。历史上，这个数值正好对应于每秒的调度器“时钟滴答数”，但由于高频调度和[无时钟滴答内核](https://lwn.net/Articles/549580/)的出现，滴答数的意义变得不再重要。

#### Block I/O metrics

Block I/O is accounted in the `blkio` controller. Different metrics are scattered across different files. While you can find in-depth details in the [blkio-controller](https://www.kernel.org/doc/Documentation/cgroup-v1/blkio-controller.txt) file in the kernel documentation, here is a short list of the most relevant ones:

​	块 I/O 在 `blkio` 控制器中进行核算。不同的指标分散在不同的文件中。可以在内核文档中的 [blkio-controller](https://www.kernel.org/doc/Documentation/cgroup-v1/blkio-controller.txt) 文件中找到详细信息，以下是一些最相关的指标：

- `blkio.sectors`

- Contains the number of 512-bytes sectors read and written by the processes member of the cgroup, device by device. Reads and writes are merged in a single counter.

  - 包含该 cgroup 内成员进程每个设备的 512 字节扇区读写次数。读取和写入合并为一个计数器。

- `blkio.io_service_bytes`

  Indicates the number of bytes read and written by the cgroup. It has 4 counters per device, because for each device, it differentiates between synchronous vs. asynchronous I/O, and reads vs. writes.

  ​	表示该 cgroup 读取和写入的字节数。每个设备有 4 个计数器，区分同步与异步 I/O 以及读写操作。

- `blkio.io_serviced`

  The number of I/O operations performed, regardless of their size. It also has 4 counters per device.

  ​	表示执行的 I/O 操作数量，不论大小。每个设备也有 4 个计数器。

- `blkio.io_queued`

  Indicates the number of I/O operations currently queued for this cgroup. In other words, if the cgroup isn't doing any I/O, this is zero. The opposite is not true. In other words, if there is no I/O queued, it doesn't mean that the cgroup is idle (I/O-wise). It could be doing purely synchronous reads on an otherwise quiescent device, which can therefore handle them immediately, without queuing. Also, while it's helpful to figure out which cgroup is putting stress on the I/O subsystem, keep in mind that it's a relative quantity. Even if a process group doesn't perform more I/O, its queue size can increase just because the device load increases because of other devices.

  ​	

  - 表示当前为该 cgroup 排队的 I/O 操作数量。如果该 cgroup 没有执行任何 I/O 操作，此值为零。反之，若没有 I/O 排队，也并不意味着该 cgroup 处于 I/O 空闲状态。它可能在安静设备上执行纯同步读取操作，能立即处理而无需排队。即使进程组不执行更多 I/O，随着设备负载的增加，队列大小也可能增加。

### 网络指标 Network metrics

Network metrics aren't exposed directly by control groups. There is a good explanation for that: network interfaces exist within the context of *network namespaces*. The kernel could probably accumulate metrics about packets and bytes sent and received by a group of processes, but those metrics wouldn't be very useful. You want per-interface metrics (because traffic happening on the local `lo` interface doesn't really count). But since processes in a single cgroup can belong to multiple network namespaces, those metrics would be harder to interpret: multiple network namespaces means multiple `lo` interfaces, potentially multiple `eth0` interfaces, etc.; so this is why there is no easy way to gather network metrics with control groups.

​	控制组不直接暴露网络指标，这有充分的原因：网络接口存在于 *网络命名空间* 的上下文中。内核可能会累积一组进程发送和接收的数据包和字节数的指标，但这些指标意义不大。通常需要每个接口的指标（因为本地 `lo` 接口上的流量不计入）。由于单个 cgroup 中的进程可以属于多个网络命名空间，这些指标的解释会更困难：多个网络命名空间意味着多个 `lo` 接口，可能还有多个 `eth0` 接口等。因此，控制组无法轻松收集网络指标。

Instead you can gather network metrics from other sources.

​	可以从其他来源收集网络指标。

#### iptables

iptables (or rather, the netfilter framework for which iptables is just an interface) can do some serious accounting.

​	iptables（或称 netfilter 框架）可以执行一些复杂的流量统计工作。

For instance, you can setup a rule to account for the outbound HTTP traffic on a web server:

​	例如，可以设置规则来统计 Web 服务器上的 HTTP 出站流量：



```console
$ iptables -I OUTPUT -p tcp --sport 80
```

There is no `-j` or `-g` flag, so the rule just counts matched packets and goes to the following rule.

​	没有 `-j` 或 `-g` 标志，因此该规则仅统计匹配的包并进入下一条规则。

Later, you can check the values of the counters, with:

​	稍后可以通过以下命令检查计数器值：



```console
$ iptables -nxvL OUTPUT
```

Technically, `-n` isn't required, but it prevents iptables from doing DNS reverse lookups, which are probably useless in this scenario.

​	技术上讲，`-n` 不是必需的，但它阻止 iptables 执行 DNS 反向查找，在这种场景下可能无用。

Counters include packets and bytes. If you want to setup metrics for container traffic like this, you could execute a `for` loop to add two `iptables` rules per container IP address (one in each direction), in the `FORWARD` chain. This only meters traffic going through the NAT layer; you also need to add traffic going through the userland proxy.

​	计数器包括数据包和字节数。要为容器流量设置此类指标，可以执行 `for` 循环，在 `FORWARD` 链中为每个容器 IP 地址添加两个 `iptables` 规则（每个方向一个）。这仅计量通过 NAT 层的流量，还需添加通过用户态代理的流量。

Then, you need to check those counters on a regular basis. If you happen to use `collectd`, there is a [nice plugin](https://collectd.org/wiki/index.php/Table_of_Plugins) to automate iptables counters collection.

​	然后需要定期检查这些计数器。如果使用 `collectd`，有一个[不错的插件](https://collectd.org/wiki/index.php/Table_of_Plugins)可以自动收集 iptables 计数器。

#### 接口级别计数器 Interface-level counters

Since each container has a virtual Ethernet interface, you might want to check directly the TX and RX counters of this interface. Each container is associated to a virtual Ethernet interface in your host, with a name like `vethKk8Zqi`. Figuring out which interface corresponds to which container is, unfortunately, difficult.

​	每个容器都有一个虚拟以太网接口，因此可以直接检查该接口的 TX 和 RX 计数器。每个容器在主机上与一个虚拟以太网接口关联，接口名如 `vethKk8Zqi`。识别哪个接口对应哪个容器较为困难。

But for now, the best way is to check the metrics *from within the containers*. To accomplish this, you can run an executable from the host environment within the network namespace of a container using **ip-netns magic**.

​	目前，最好的方法是*从容器内部*检查指标。可以通过 **ip-netns magic** 命令在容器的网络命名空间内从主机环境中运行可执行文件。

The `ip-netns exec` command allows you to execute any program (present in the host system) within any network namespace visible to the current process. This means that your host can enter the network namespace of your containers, but your containers can't access the host or other peer containers. Containers can interact with their sub-containers, though.

​	`ip-netns exec` 命令允许在当前进程可见的任何网络命名空间内执行任何程序（在主机系统中存在的）。这意味着主机可以进入容器的网络命名空间，但容器无法访问主机或其他对等容器。容器可以与其子容器交互。

The exact format of the command is:

​	命令的确切格式如下：

```console
$ ip netns exec <nsname> <command...>
```

For example:

​	例如：

```console
$ ip netns exec mycontainer netstat -i
```

`ip netns` finds the `mycontainer` container by using namespaces pseudo-files. Each process belongs to one network namespace, one PID namespace, one `mnt` namespace, etc., and those namespaces are materialized under `/proc/<pid>/ns/`. For example, the network namespace of PID 42 is materialized by the pseudo-file `/proc/42/ns/net`.

​	`ip netns` 使用命名空间伪文件来找到 `mycontainer` 容器。每个进程都属于一个网络命名空间、一个 PID 命名空间、一个 `mnt` 命名空间等，而这些命名空间通过 `/proc/<pid>/ns/` 下的伪文件体现。例如，PID 为 42 的进程的网络命名空间由伪文件 `/proc/42/ns/net` 表示。

When you run `ip netns exec mycontainer ...`, it expects `/var/run/netns/mycontainer` to be one of those pseudo-files. (Symlinks are accepted.)

​	当运行 `ip netns exec mycontainer ...` 时，它期望 `/var/run/netns/mycontainer` 是这些伪文件之一（符号链接也可接受）。

In other words, to execute a command within the network namespace of a container, we need to:

​	换句话说，要在容器的网络命名空间中执行命令，需要：

- Find out the PID of any process within the container that we want to investigate;
  - 找到要检查的容器内的任意一个进程的 PID；

- Create a symlink from `/var/run/netns/<somename>` to `/proc/<thepid>/ns/net`
  - 创建从 `/var/run/netns/<somename>` 到 `/proc/<thepid>/ns/net` 的符号链接；

- Execute `ip netns exec <somename> ....`
  - 执行 `ip netns exec <somename> ....`


Review [Enumerate Cgroups](https://docs.docker.com/engine/containers/runmetrics/#enumerate-cgroups) for how to find the cgroup of an in-container process whose network usage you want to measure. From there, you can examine the pseudo-file named `tasks`, which contains all the PIDs in the cgroup (and thus, in the container). Pick any one of the PIDs.

​	查看[列举 Cgroups](https://docs.docker.com/engine/containers/runmetrics/#enumerate-cgroups)了解如何找到要测量网络使用情况的容器进程的 cgroup。然后可以检查名为 `tasks` 的伪文件，其中包含 cgroup 中的所有 PID（即容器内的所有 PID）。选择其中一个 PID 即可。

Putting everything together, if the "short ID" of a container is held in the environment variable `$CID`, then you can do this:

​	综上所述，如果容器的 "短 ID" 存储在环境变量 `$CID` 中，可以这样做：



```console
$ TASKS=/sys/fs/cgroup/devices/docker/$CID*/tasks
$ PID=$(head -n 1 $TASKS)
$ mkdir -p /var/run/netns
$ ln -sf /proc/$PID/ns/net /var/run/netns/$CID
$ ip netns exec $CID netstat -i
```

## 高性能指标采集的技巧 Tips for high-performance metric collection

Running a new process each time you want to update metrics is (relatively) expensive. If you want to collect metrics at high resolutions, and/or over a large number of containers (think 1000 containers on a single host), you don't want to fork a new process each time.

​	每次想更新指标时都启动一个新进程是（相对）昂贵的。如果需要在高分辨率和/或大量容器上（比如单主机上的 1000 个容器）收集指标，不希望每次都 fork 一个新进程。

Here is how to collect metrics from a single process. You need to write your metric collector in C (or any language that lets you do low-level system calls). You need to use a special system call, `setns()`, which lets the current process enter any arbitrary namespace. It requires, however, an open file descriptor to the namespace pseudo-file (remember: that's the pseudo-file in `/proc/<pid>/ns/net`).

​	以下是从单个进程收集指标的方法。需要用 C 编写指标收集器（或任何允许低级系统调用的语言）。需要使用一个特殊的系统调用 `setns()`，它允许当前进程进入任意命名空间。然而，它需要一个指向命名空间伪文件的打开文件描述符（记住：伪文件在 `/proc/<pid>/ns/net`）。

However, there is a catch: you must not keep this file descriptor open. If you do, when the last process of the control group exits, the namespace isn't destroyed, and its network resources (like the virtual interface of the container) stays around forever (or until you close that file descriptor).

​	但是，有一个问题：不能保持此文件描述符打开。如果这样做，当控制组中的最后一个进程退出时，命名空间不会被销毁，网络资源（如容器的虚拟接口）会一直存在（直到关闭该文件描述符）。

The right approach would be to keep track of the first PID of each container, and re-open the namespace pseudo-file each time.

​	正确的方法是跟踪每个容器的第一个 PID，并在每次重新打开命名空间伪文件时这样做。

## 在容器退出时收集指标 Collect metrics when a container exits

Sometimes, you don't care about real time metric collection, but when a container exits, you want to know how much CPU, memory, etc. it has used.

​	有时，不需要实时指标采集，而只需在容器退出时知道它使用了多少 CPU、内存等资源。

Docker makes this difficult because it relies on `lxc-start`, which carefully cleans up after itself. It is usually easier to collect metrics at regular intervals, and this is the way the `collectd` LXC plugin works.

​	Docker 的运行机制增加了难度，因为它依赖于 `lxc-start`，而此命令会小心清理资源。通常，定期采集指标更为容易，这也是 `collectd` LXC 插件的工作方式。

But, if you'd still like to gather the stats when a container stops, here is how:

​	但是，如果仍想在容器停止时收集统计信息，可以这样做：

For each container, start a collection process, and move it to the control groups that you want to monitor by writing its PID to the tasks file of the cgroup. The collection process should periodically re-read the tasks file to check if it's the last process of the control group. (If you also want to collect network statistics as explained in the previous section, you should also move the process to the appropriate network namespace.)

​	对于每个容器，启动一个采集进程，并将其移动到要监控的控制组，通过将其 PID 写入 cgroup 的 tasks 文件来完成。采集进程应定期重新读取 tasks 文件，以检查是否是控制组中的最后一个进程。（如果还需要像前一节所述那样收集网络统计信息，还应将该进程移动到相应的网络命名空间。）

When the container exits, `lxc-start` attempts to delete the control groups. It fails, since the control group is still in use; but that's fine. Your process should now detect that it is the only one remaining in the group. Now is the right time to collect all the metrics you need!

​	当容器退出时，`lxc-start` 尝试删除控制组，但因控制组仍在使用而失败；这没问题。您的进程此时应该检测到它是组内唯一的进程，现在是收集所有所需指标的适当时机！

Finally, your process should move itself back to the root control group, and remove the container control group. To remove a control group, just `rmdir` its directory. It's counter-intuitive to `rmdir` a directory as it still contains files; but remember that this is a pseudo-filesystem, so usual rules don't apply. After the cleanup is done, the collection process can exit safely.

​	最后，进程应将自身移回根控制组，并删除容器的控制组。要删除控制组，只需 `rmdir` 其目录。尽管其目录仍包含文件，但请记住这是一个伪文件系统，通常规则不适用。完成清理后，采集进程可以安全地退出。
