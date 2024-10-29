+++
title = "Docker 的 Seccomp 安全配置文件"
date = 2024-10-23T14:54:40+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/security/seccomp/](https://docs.docker.com/engine/security/seccomp/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Seccomp security profiles for Docker - Docker 的 Seccomp 安全配置文件

Secure computing mode (`seccomp`) is a Linux kernel feature. You can use it to restrict the actions available within the container. The `seccomp()` system call operates on the seccomp state of the calling process. You can use this feature to restrict your application's access.

​	安全计算模式（`seccomp`）是 Linux 内核的一项功能，可用于限制容器内的操作。`seccomp()` 系统调用作用于调用进程的 seccomp 状态，您可以使用此功能来限制应用程序的访问权限。

This feature is available only if Docker has been built with `seccomp` and the kernel is configured with `CONFIG_SECCOMP` enabled. To check if your kernel supports `seccomp`:

​	该功能仅在 Docker 构建时启用了 `seccomp` 并且内核配置了 `CONFIG_SECCOMP` 时可用。要检查内核是否支持 `seccomp`：

```console
$ grep CONFIG_SECCOMP= /boot/config-$(uname -r)
CONFIG_SECCOMP=y
```

## 为容器指定配置文件 Pass a profile for a container

The default `seccomp` profile provides a sane default for running containers with seccomp and disables around 44 system calls out of 300+. It is moderately protective while providing wide application compatibility. The default Docker profile can be found [here](https://github.com/moby/moby/blob/master/profiles/seccomp/default.json).

​	默认的 `seccomp` 配置文件为运行容器提供了合理的默认值，禁用了 300 多个系统调用中的约 44 个。在提供广泛应用兼容性的同时具有适度的保护性。默认的 Docker 配置文件可以在[此处](https://github.com/moby/moby/blob/master/profiles/seccomp/default.json)找到。

In effect, the profile is an allowlist which denies access to system calls by default, then allowlists specific system calls. The profile works by defining a `defaultAction` of `SCMP_ACT_ERRNO` and overriding that action only for specific system calls. The effect of `SCMP_ACT_ERRNO` is to cause a `Permission Denied` error. Next, the profile defines a specific list of system calls which are fully allowed, because their `action` is overridden to be `SCMP_ACT_ALLOW`. Finally, some specific rules are for individual system calls such as `personality`, and others, to allow variants of those system calls with specific arguments.

​	实际上，该配置文件是一个允许列表，默认情况下拒绝访问系统调用，然后对特定的系统调用进行允许。配置文件通过定义 `defaultAction` 为 `SCMP_ACT_ERRNO` 来实现此功能，仅对特定的系统调用重写此操作。`SCMP_ACT_ERRNO` 的效果是导致“权限被拒绝”错误。接下来，配置文件定义了一些完全允许的系统调用，因为它们的 `action` 被重写为 `SCMP_ACT_ALLOW`。最后，对于一些特定系统调用（如 `personality` 等），配置文件允许具有特定参数的变体调用。

`seccomp` is instrumental for running Docker containers with least privilege. It is not recommended to change the default `seccomp` profile.

​	`seccomp` 对于在最低权限下运行 Docker 容器至关重要。建议不要更改默认的 `seccomp` 配置文件。

When you run a container, it uses the default profile unless you override it with the `--security-opt` option. For example, the following explicitly specifies a policy:

​	当您运行容器时，除非使用 `--security-opt` 选项覆盖配置，否则将使用默认配置文件。例如，以下命令显式指定了一个策略：



```console
$ docker run --rm \
             -it \
             --security-opt seccomp=/path/to/seccomp/profile.json \
             hello-world
```

### 默认配置文件阻止的重要系统调用 Significant syscalls blocked by the default profile

Docker's default seccomp profile is an allowlist which specifies the calls that are allowed. The table below lists the significant (but not all) syscalls that are effectively blocked because they are not on the Allowlist. The table includes the reason each syscall is blocked rather than white-listed.

​	Docker 的默认 seccomp 配置文件是一个允许列表，指定了被允许的调用。下表列出了一些被有效阻止的重要（但并非全部）系统调用，表中还包括了每个系统调用被阻止的原因。

| Syscall             | Description                                                  |
| ------------------- | ------------------------------------------------------------ |
| `acct`              | 记账系统调用，可能允许容器禁用其自身的资源限制或进程记账。此外受 `CAP_SYS_PACCT` 限制。Accounting syscall which could let containers disable their own resource limits or process accounting. Also gated by `CAP_SYS_PACCT`. |
| `add_key`           | 防止容器使用内核密钥环，密钥环未命名空间化。Prevent containers from using the kernel keyring, which is not namespaced. |
| `bpf`               | 拒绝将潜在持久化的 bpf 程序加载到内核，已由 `CAP_SYS_ADMIN` 限制。Deny loading potentially persistent bpf programs into kernel, already gated by `CAP_SYS_ADMIN`. |
| `clock_adjtime`     | 时间/日期未命名空间化。此外受 `CAP_SYS_TIME` 限制。Time/date is not namespaced. Also gated by `CAP_SYS_TIME`. |
| `clock_settime`     | 时间/日期未命名空间化。此外受 `CAP_SYS_TIME` 限制。Time/date is not namespaced. Also gated by `CAP_SYS_TIME`. |
| `clone`             | 禁止克隆新命名空间。此外受 `CAP_SYS_ADMIN` 限制（除 `CLONE_NEWUSER`）。Deny cloning new namespaces. Also gated by `CAP_SYS_ADMIN` for CLONE_* flags, except `CLONE_NEWUSER`. |
| `create_module`     | 禁止操作内核模块，已废弃。此外受 `CAP_SYS_MODULE` 限制。Deny manipulation and functions on kernel modules. Obsolete. Also gated by `CAP_SYS_MODULE`. |
| `delete_module`     | 禁止操作内核模块。此外受 `CAP_SYS_MODULE` 限制。Deny manipulation and functions on kernel modules. Also gated by `CAP_SYS_MODULE`. |
| `finit_module`      | 禁止操作内核模块。此外受 `CAP_SYS_MODULE` 限制。Deny manipulation and functions on kernel modules. Also gated by `CAP_SYS_MODULE`. |
| `get_kernel_syms`   | 拒绝导出内核和模块符号的检索。已废弃。Deny retrieval of exported kernel and module symbols. Obsolete. |
| `get_mempolicy`     | 修改内核内存和 NUMA 设置的系统调用，已受 `CAP_SYS_NICE` 限制。Syscall that modifies kernel memory and NUMA settings. Already gated by `CAP_SYS_NICE`. |
| `init_module`       | 禁止操作内核模块。此外受 `CAP_SYS_MODULE` 限制。Deny manipulation and functions on kernel modules. Also gated by `CAP_SYS_MODULE`. |
| `ioperm`            | 防止容器修改内核 I/O 特权级别，已受 `CAP_SYS_RAWIO` 限制。Prevent containers from modifying kernel I/O privilege levels. Already gated by `CAP_SYS_RAWIO`. |
| `iopl`              | 防止容器修改内核 I/O 特权级别，已受 `CAP_SYS_RAWIO` 限制。Prevent containers from modifying kernel I/O privilege levels. Already gated by `CAP_SYS_RAWIO`. |
| `kcmp`              | 限制进程检查功能，已通过移除 `CAP_SYS_PTRACE` 阻止。Restrict process inspection capabilities, already blocked by dropping `CAP_SYS_PTRACE`. |
| `kexec_file_load`   | 与 `kexec_load` 类似的系统调用，使用略微不同的参数。此外受 `CAP_SYS_BOOT` 限制。Sister syscall of `kexec_load` that does the same thing, slightly different arguments. Also gated by `CAP_SYS_BOOT`. |
| `kexec_load`        | 禁止加载新内核以供后续执行。此外受 `CAP_SYS_BOOT` 限制。Deny loading a new kernel for later execution. Also gated by `CAP_SYS_BOOT`. |
| `keyctl`            | 防止容器使用内核密钥环，密钥环未命名空间化。Prevent containers from using the kernel keyring, which is not namespaced. |
| `lookup_dcookie`    | 跟踪/分析系统调用，可能泄露大量主机信息。此外受 `CAP_SYS_ADMIN` 限制。Tracing/profiling syscall, which could leak a lot of information on the host. Also gated by `CAP_SYS_ADMIN`. |
| `mbind`             | 修改内核内存和 NUMA 设置的系统调用，已受 `CAP_SYS_NICE` 限制。Syscall that modifies kernel memory and NUMA settings. Already gated by `CAP_SYS_NICE`. |
| `mount`             | 禁止挂载，已受 `CAP_SYS_ADMIN` 限制。Deny mounting, already gated by `CAP_SYS_ADMIN`. |
| `move_pages`        | 修改内核内存和 NUMA 设置的系统调用。Syscall that modifies kernel memory and NUMA settings. |
| `nfsservctl`        | 禁止与内核 nfs 守护进程的交互，自 Linux 3.1 已废弃。Deny interaction with the kernel nfs daemon. Obsolete since Linux 3.1. |
| `open_by_handle_at` | 导致旧容器越狱漏洞。此外受 `CAP_DAC_READ_SEARCH` 限制。Cause of an old container breakout. Also gated by `CAP_DAC_READ_SEARCH`. |
| `perf_event_open`   | 跟踪/分析系统调用，可能泄露大量主机信息。Tracing/profiling syscall, which could leak a lot of information on the host. |
| `personality`       | 防止容器启用 BSD 仿真。非本质危险，但测试不足，可能有大量内核漏洞。Prevent container from enabling BSD emulation. Not inherently dangerous, but poorly tested, potential for a lot of kernel vulns. |
| `pivot_root`        | 禁止 `pivot_root`，应为特权操作。Deny `pivot_root`, should be privileged operation. |
| `process_vm_readv`  | 限制进程检查功能，已通过移除 `CAP_SYS_PTRACE` 阻止。Restrict process inspection capabilities, already blocked by dropping `CAP_SYS_PTRACE`. |
| `process_vm_writev` | 限制进程检查功能，已通过移除 `CAP_SYS_PTRACE` 阻止。Restrict process inspection capabilities, already blocked by dropping `CAP_SYS_PTRACE`. |
| `ptrace`            | 跟踪/分析系统调用。为了避免 seccomp 绕过，在 Linux 内核 4.8 之前被阻止。跟踪/分析任意进程已通过移除 `CAP_SYS_PTRACE` 阻止，因为可能泄露大量主机信息。Tracing/profiling syscall. Blocked in Linux kernel versions before 4.8 to avoid seccomp bypass. Tracing/profiling arbitrary processes is already blocked by dropping `CAP_SYS_PTRACE`, because it could leak a lot of information on the host. |
| `query_module`      | 禁止操作内核模块。已废弃。Deny manipulation and functions on kernel modules. Obsolete. |
| `quotactl`          | 配额系统调用，可能允许容器禁用其自身的资源限制或进程记账。此外受 `CAP_SYS_ADMIN` 限制。Quota syscall which could let containers disable their own resource limits or process accounting. Also gated by `CAP_SYS_ADMIN`. |
| `reboot`            | 防止容器重启主机。此外受 `CAP_SYS_BOOT` 限制。Don't let containers reboot the host. Also gated by `CAP_SYS_BOOT`. |
| `request_key`       | 防止容器使用内核密钥环，密钥环未命名空间化。Prevent containers from using the kernel keyring, which is not namespaced. |
| `set_mempolicy`     | 修改内核内存和 NUMA 设置的系统调用，已受 `CAP_SYS_NICE` 限制。Syscall that modifies kernel memory and NUMA settings. Already gated by `CAP_SYS_NICE`. |
| `setns`             | 禁止将线程与命名空间关联。此外受 `CAP_SYS_ADMIN` 限制。Deny associating a thread with a namespace. Also gated by `CAP_SYS_ADMIN`. |
| `settimeofday`      | 时间/日期未命名空间化。此外受 `CAP_SYS_TIME` 限制。Time/date is not namespaced. Also gated by `CAP_SYS_TIME`. |
| `stime`             | 时间/日期未命名空间化。此外受 `CAP_SYS_TIME` 限制。Time/date is not namespaced. Also gated by `CAP_SYS_TIME`. |
| `swapon`            | 禁止开始/停止对文件/设备的交换。此外受 `CAP_SYS_ADMIN` 限制。Deny start/stop swapping to file/device. Also gated by `CAP_SYS_ADMIN`. |
| `swapoff`           | 禁止开始/停止对文件/设备的交换。此外受 `CAP_SYS_ADMIN` 限制。Deny start/stop swapping to file/device. Also gated by `CAP_SYS_ADMIN`. |
| `sysfs`             | 已废弃的系统调用。Obsolete syscall.                          |
| `_sysctl`           | 已废弃，被 /proc/sys 取代。Obsolete, replaced by /proc/sys.  |
| `umount`            | 应为特权操作。此外受 `CAP_SYS_ADMIN` 限制。Should be a privileged operation. Also gated by `CAP_SYS_ADMIN`. |
| `umount2`           | 应为特权操作。此外受 `CAP_SYS_ADMIN` 限制。Should be a privileged operation. Also gated by `CAP_SYS_ADMIN`. |
| `unshare`           | 禁止克隆新命名空间的进程。此外受 `CAP_SYS_ADMIN` 限制，除 `unshare --user`。Deny cloning new namespaces for processes. Also gated by `CAP_SYS_ADMIN`, with the exception of `unshare --user`. |
| `uselib`            | 与共享库相关的旧系统调用，长时间未使用。Older syscall related to shared libraries, unused for a long time. |
| `userfaultfd`       | 用户空间页面错误处理，主要用于进程迁移。Userspace page fault handling, largely needed for process migration. |
| `ustat`             | 已废弃的系统调用。Obsolete syscall.                          |
| `vm86`              | 内核 x86 实模式虚拟机。此外受 `CAP_SYS_ADMIN` 限制。In kernel x86 real mode virtual machine. Also gated by `CAP_SYS_ADMIN`. |
| `vm86old`           | 内核 x86 实模式虚拟机。此外受 `CAP_SYS_ADMIN` 限制。In kernel x86 real mode virtual machine. Also gated by `CAP_SYS_ADMIN`. |

## 在没有默认 seccomp 配置文件的情况下运行 Run without the default seccomp profile

You can pass `unconfined` to run a container without the default seccomp profile.

​	可以通过传递 `unconfined` 来在没有默认 seccomp 配置文件的情况下运行容器。

```console
$ docker run --rm -it --security-opt seccomp=unconfined debian:jessie \
    unshare --map-root-user --user sh -c whoami
```
