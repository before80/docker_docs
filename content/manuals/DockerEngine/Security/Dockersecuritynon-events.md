+++
title = "Docker 安全性非事件"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/security/non-events/](https://docs.docker.com/engine/security/non-events/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker security non-events - Docker 安全性非事件

This page lists security vulnerabilities which Docker mitigated, such that processes run in Docker containers were never vulnerable to the bug—even before it was fixed. This assumes containers are run without adding extra capabilities or not run as `--privileged`.

​	此页面列出了一些安全漏洞，Docker 通过预先的缓解措施，使运行在 Docker 容器中的进程未受到漏洞影响——即便这些漏洞在修复之前也是如此。这是假设容器在没有添加额外权限或不使用 `--privileged` 模式的情况下运行。

The list below is not even remotely complete. Rather, it is a sample of the few bugs we've actually noticed to have attracted security review and publicly disclosed vulnerabilities. In all likelihood, the bugs that haven't been reported far outnumber those that have. Luckily, since Docker's approach to secure by default through apparmor, seccomp, and dropping capabilities, it likely mitigates unknown bugs just as well as it does known ones.

​	以下列表并不完全。相反，这只是我们注意到并经过安全审查和公开披露的漏洞样本。很可能未报告的漏洞数量远远超过了已报告的数量。幸运的是，由于 Docker 通过 AppArmor、seccomp 和移除权限的默认安全方法，它对未知漏洞的缓解效果可能与已知漏洞一样好。

Bugs mitigated:

​	缓解的漏洞：

- [CVE-2013-1956](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2013-1956), [1957](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2013-1957), [1958](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2013-1958), [1959](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2013-1959), [1979](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2013-1979), [CVE-2014-4014](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-4014), [5206](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-5206), [5207](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-5207), [7970](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-7970), [7975](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-7975), [CVE-2015-2925](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-2925), [8543](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-8543), [CVE-2016-3134](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-3134), [3135](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-3135), etc.: The introduction of unprivileged user namespaces lead to a huge increase in the attack surface available to unprivileged users by giving such users legitimate access to previously root-only system calls like `mount()`. All of these CVEs are examples of security vulnerabilities due to introduction of user namespaces. Docker can use user namespaces to set up containers, but then disallows the process inside the container from creating its own nested namespaces through the default seccomp profile, rendering these vulnerabilities unexploitable.
  - [CVE-2013-1956](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2013-1956)、[1957](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2013-1957)、[1958](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2013-1958)、[1959](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2013-1959)、[1979](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2013-1979)、[CVE-2014-4014](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-4014)、[5206](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-5206)、[5207](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-5207)、[7970](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-7970)、[7975](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-7975)、[CVE-2015-2925](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-2925)、[8543](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-8543)、[CVE-2016-3134](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-3134)、[3135](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-3135) 等：用户命名空间的引入大幅增加了非特权用户可用的攻击面，因为这些用户可以合法地访问以前仅限 root 用户使用的系统调用（如 `mount()`）。这些 CVE 都是由于引入用户命名空间而导致的安全漏洞。Docker 可以使用用户命名空间来设置容器，但随后通过默认的 seccomp 配置文件禁止容器内的进程创建嵌套的命名空间，使得这些漏洞无法利用。

- [CVE-2014-0181](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-0181), [CVE-2015-3339](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-3339): These are bugs that require the presence of a setuid binary. Docker disables setuid binaries inside containers via the `NO_NEW_PRIVS` process flag and other mechanisms.
  - [CVE-2014-0181](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-0181)、[CVE-2015-3339](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-3339)：这些是需要 setuid 二进制文件的漏洞。Docker 通过 `NO_NEW_PRIVS` 进程标志和其他机制禁用容器内的 setuid 二进制文件。

- [CVE-2014-4699](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-4699): A bug in `ptrace()` could allow privilege escalation. Docker disables `ptrace()` inside the container using apparmor, seccomp and by dropping `CAP_PTRACE`. Three times the layers of protection there!
  - [CVE-2014-4699](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-4699)：`ptrace()` 中的一个漏洞可能导致权限提升。Docker 通过 AppArmor、seccomp 和删除 `CAP_PTRACE` 来禁用容器内的 `ptrace()`，提供了三层保护！

- [CVE-2014-9529](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-9529): A series of crafted `keyctl()` calls could cause kernel DoS / memory corruption. Docker disables `keyctl()` inside containers using seccomp.
  - [CVE-2014-9529](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-9529)：一系列精心构造的 `keyctl()` 调用可能导致内核 DoS/内存损坏。Docker 使用 seccomp 禁用容器内的 `keyctl()`。

- [CVE-2015-3214](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-3214), [4036](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-4036): These are bugs in common virtualization drivers which could allow a guest OS user to execute code on the host OS. Exploiting them requires access to virtualization devices in the guest. Docker hides direct access to these devices when run without `--privileged`. Interestingly, these seem to be cases where containers are "more secure" than a VM, going against common wisdom that VMs are "more secure" than containers.
  - [CVE-2015-3214](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-3214)、[4036](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-4036)：这些是常见的虚拟化驱动程序中的漏洞，可能允许来宾操作系统用户在主机操作系统上执行代码。利用它们需要访问来宾中的虚拟化设备。Docker 在未使用 `--privileged` 的情况下隐藏了对这些设备的直接访问。有趣的是，这些情况表明容器可能比虚拟机更安全，这与“虚拟机比容器更安全”的普遍看法相反。

- [CVE-2016-0728](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-0728): Use-after-free caused by crafted `keyctl()` calls could lead to privilege escalation. Docker disables `keyctl()` inside containers using the default seccomp profile.
  - [CVE-2016-0728](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-0728)：由构造的 `keyctl()` 调用引发的释放后重用问题可能导致权限提升。Docker 使用默认 seccomp 配置文件禁用容器内的 `keyctl()`。

- [CVE-2016-2383](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-2383): A bug in eBPF -- the special in-kernel DSL used to express things like seccomp filters -- allowed arbitrary reads of kernel memory. The `bpf()` system call is blocked inside Docker containers using (ironically) seccomp.
  - [CVE-2016-2383](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-2383)：eBPF（用于表达 seccomp 过滤器等的内核内专用 DSL）中的一个漏洞允许任意读取内核内存。Docker 使用 seccomp（讽刺地）阻止容器内的 `bpf()` 系统调用。

- [CVE-2016-3134](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-3134), [4997](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-4997), [4998](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-4998): A bug in setsockopt with `IPT_SO_SET_REPLACE`, `ARPT_SO_SET_REPLACE`, and `ARPT_SO_SET_REPLACE` causing memory corruption / local privilege escalation. These arguments are blocked by `CAP_NET_ADMIN`, which Docker does not allow by default.
  - [CVE-2016-3134](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-3134)、[4997](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-4997)、[4998](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-4998)：`IPT_SO_SET_REPLACE`、`ARPT_SO_SET_REPLACE` 和 `ARPT_SO_SET_REPLACE` 的 `setsockopt` 参数中的漏洞导致内存损坏/本地权限提升。这些参数被 `CAP_NET_ADMIN` 阻止，而 Docker 默认不允许此权限。


Bugs not mitigated:

​	未缓解的漏洞：

- [CVE-2015-3290](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-3290), [5157](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-5157): Bugs in the kernel's non-maskable interrupt handling allowed privilege escalation. Can be exploited in Docker containers because the `modify_ldt()` system call is not currently blocked using seccomp.
  - [CVE-2015-3290](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-3290)、[5157](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-5157)：内核的不可屏蔽中断处理中的漏洞允许权限提升。可以在 Docker 容器中利用，因为目前未使用 seccomp 阻止 `modify_ldt()` 系统调用。
- [CVE-2016-5195](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-5195): A race condition was found in the way the Linux kernel's memory subsystem handled the copy-on-write (COW) breakage of private read-only memory mappings, which allowed unprivileged local users to gain write access to read-only memory. Also known as "dirty COW." *Partial mitigations:* on some operating systems this vulnerability is mitigated by the combination of seccomp filtering of `ptrace` and the fact that `/proc/self/mem` is read-only.
  - [CVE-2016-5195](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-5195)：在 Linux 内核的内存子系统处理私有只读内存映射的写时复制（COW）破坏过程中发现了一个竞争条件，允许非特权本地用户对只读内存进行写访问。也被称为“Dirty COW”。*部分缓解措施*：在某些操作系统上，该漏洞通过 seccomp 过滤 `ptrace` 和 `/proc/self/mem` 只读的组合得到了缓解。
