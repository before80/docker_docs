+++
title = "Docker 的 AppArmor 安全配置文件"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/security/apparmor/](https://docs.docker.com/engine/security/apparmor/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# AppArmor security profiles for Docker - Docker 的 AppArmor 安全配置文件

AppArmor (Application Armor) is a Linux security module that protects an operating system and its applications from security threats. To use it, a system administrator associates an AppArmor security profile with each program. Docker expects to find an AppArmor policy loaded and enforced.

​	AppArmor（应用护甲）是一种 Linux 安全模块，用于保护操作系统及其应用程序免受安全威胁。系统管理员可以为每个程序关联一个 AppArmor 安全配置文件。Docker 期望 AppArmor 策略已加载并生效。

Docker automatically generates and loads a default profile for containers named `docker-default`. The Docker binary generates this profile in `tmpfs` and then loads it into the kernel.

​	Docker 会自动为容器生成并加载一个名为 `docker-default` 的默认配置文件。Docker 二进制文件在 `tmpfs` 中生成此配置文件并加载到内核中。

> **Note**
>
> 
>
> This profile is used on containers, not on the Docker daemon.
>
> ​	此配置文件用于容器，而不是 Docker 守护进程。

A profile for the Docker Engine daemon exists but it is not currently installed with the `deb` packages. If you are interested in the source for the daemon profile, it is located in [contrib/apparmor](https://github.com/moby/moby/tree/master/contrib/apparmor) in the Docker Engine source repository.

​	Docker Engine 守护进程也存在一个配置文件，但它目前未随 `deb` 包一起安装。如果您对该守护进程配置文件的源代码感兴趣，可以在 Docker Engine 源代码库的 [contrib/apparmor](https://github.com/moby/moby/tree/master/contrib/apparmor) 中找到。

## 理解策略 Understand the policies

The `docker-default` profile is the default for running containers. It is moderately protective while providing wide application compatibility. The profile is generated from the following [template](https://github.com/moby/moby/blob/master/profiles/apparmor/template.go).

​	`docker-default` 配置文件是运行容器的默认配置文件。它在提供广泛的应用兼容性的同时，提供适度的保护。此配置文件由以下[模板](https://github.com/moby/moby/blob/master/profiles/apparmor/template.go)生成。

When you run a container, it uses the `docker-default` policy unless you override it with the `security-opt` option. For example, the following explicitly specifies the default policy:

​	当您运行容器时，除非通过 `security-opt` 选项覆盖它，否则它会使用 `docker-default` 策略。例如，以下命令明确指定了默认策略：



```console
$ docker run --rm -it --security-opt apparmor=docker-default hello-world
```

## 加载和卸载配置文件 Load and unload profiles

To load a new profile into AppArmor for use with containers:

​	要将新的配置文件加载到 AppArmor 以供容器使用：



```console
$ apparmor_parser -r -W /path/to/your_profile
```

Then, run the custom profile with `--security-opt`:

​	然后，使用 `--security-opt` 选项运行自定义配置文件：



```console
$ docker run --rm -it --security-opt apparmor=your_profile hello-world
```

To unload a profile from AppArmor:

​	要从 AppArmor 中卸载配置文件：



```console
# unload the profile
# 卸载配置文件
$ apparmor_parser -R /path/to/profile
```

### 编写配置文件的资源 Resources for writing profiles

The syntax for file globbing in AppArmor is a bit different than some other globbing implementations. It is highly suggested you take a look at some of the below resources with regard to AppArmor profile syntax.

​	AppArmor 中的文件通配符语法与其他一些通配符实现有所不同。强烈建议您查看以下关于 AppArmor 配置文件语法的资源。

- [Quick Profile Language 快速配置语言](https://gitlab.com/apparmor/apparmor/wikis/QuickProfileLanguage)

- [Globbing Syntax 通配符语法](https://gitlab.com/apparmor/apparmor/wikis/AppArmor_Core_Policy_Reference#AppArmor_globbing_syntax)

## Nginx 示例配置文件 Nginx example profile

In this example, you create a custom AppArmor profile for Nginx. Below is the custom profile.

​	以下是为 Nginx 创建自定义 AppArmor 配置文件的示例。



```c
#include <tunables/global>


profile docker-nginx flags=(attach_disconnected,mediate_deleted) {
  #include <abstractions/base>

  network inet tcp,
  network inet udp,
  network inet icmp,

  deny network raw,

  deny network packet,

  file,
  umount,

  deny /bin/** wl,
  deny /boot/** wl,
  deny /dev/** wl,
  deny /etc/** wl,
  deny /home/** wl,
  deny /lib/** wl,
  deny /lib64/** wl,
  deny /media/** wl,
  deny /mnt/** wl,
  deny /opt/** wl,
  deny /proc/** wl,
  deny /root/** wl,
  deny /sbin/** wl,
  deny /srv/** wl,
  deny /tmp/** wl,
  deny /sys/** wl,
  deny /usr/** wl,

  audit /** w,

  /var/run/nginx.pid w,

  /usr/sbin/nginx ix,

  deny /bin/dash mrwklx,
  deny /bin/sh mrwklx,
  deny /usr/bin/top mrwklx,


  capability chown,
  capability dac_override,
  capability setuid,
  capability setgid,
  capability net_bind_service,

  deny @{PROC}/* w,   # deny write for all files directly in /proc (not in a subdir)
  # deny write to files not in /proc/<number>/** or /proc/sys/**
  deny @{PROC}/{[^1-9],[^1-9][^0-9],[^1-9s][^0-9y][^0-9s],[^1-9][^0-9][^0-9][^0-9]*}/** w,
  deny @{PROC}/sys/[^k]** w,  # deny /proc/sys except /proc/sys/k* (effectively /proc/sys/kernel)
  deny @{PROC}/sys/kernel/{?,??,[^s][^h][^m]**} w,  # deny everything except shm* in /proc/sys/kernel/
  deny @{PROC}/sysrq-trigger rwklx,
  deny @{PROC}/mem rwklx,
  deny @{PROC}/kmem rwklx,
  deny @{PROC}/kcore rwklx,

  deny mount,

  deny /sys/[^f]*/** wklx,
  deny /sys/f[^s]*/** wklx,
  deny /sys/fs/[^c]*/** wklx,
  deny /sys/fs/c[^g]*/** wklx,
  deny /sys/fs/cg[^r]*/** wklx,
  deny /sys/firmware/** rwklx,
  deny /sys/kernel/security/** rwklx,
}
```

1. Save the custom profile to disk in the `/etc/apparmor.d/containers/docker-nginx` file. 将自定义配置文件保存到 `/etc/apparmor.d/containers/docker-nginx` 文件中。

   The file path in this example is not a requirement. In production, you could use another.

   ​	此示例中的文件路径不是强制要求，您可以使用其他路径。

2. Load the profile.

   加载配置文件。

   ```console
   $ sudo apparmor_parser -r -W /etc/apparmor.d/containers/docker-nginx
   ```

3. Run a container with the profile. 使用该配置文件运行一个容器。

   To run nginx in detached mode:

   要在分离模式下运行 nginx：

   ```console
   $ docker run --security-opt "apparmor=docker-nginx" \
        -p 80:80 -d --name apparmor-nginx nginx
   ```

4. Exec into the running container.

   进入正在运行的容器。

   ```console
   $ docker container exec -it apparmor-nginx bash
   ```

5. Try some operations to test the profile.

   尝试一些操作以测试配置文件。

   ```console
   root@6da5a2a930b9:~# ping 8.8.8.8
   ping: Lacking privilege for raw socket.
   
   root@6da5a2a930b9:/# top
   bash: /usr/bin/top: Permission denied
   
   root@6da5a2a930b9:~# touch ~/thing
   touch: cannot touch 'thing': Permission denied
   
   root@6da5a2a930b9:/# sh
   bash: /bin/sh: Permission denied
   
   root@6da5a2a930b9:/# dash
   bash: /bin/dash: Permission denied
   ```

You just deployed a container secured with a custom apparmor profile.

​	您刚刚部署了一个使用自定义 apparmor 配置文件保护的容器。

## 调试 AppArmor - Debug AppArmor

You can use `dmesg` to debug problems and `aa-status` check the loaded profiles.

​	您可以使用 `dmesg` 来调试问题，并使用 `aa-status` 检查加载的配置文件。

### Use dmesg

Here are some helpful tips for debugging any problems you might be facing with regard to AppArmor.

​	以下是一些有关调试 AppArmor 问题的有用提示。

AppArmor sends quite verbose messaging to `dmesg`. Usually an AppArmor line looks like the following:

​	AppArmor 将相当详细的消息发送到 `dmesg`。通常，一条 AppArmor 信息看起来如下：



```text
[ 5442.864673] audit: type=1400 audit(1453830992.845:37): apparmor="ALLOWED" operation="open" profile="/usr/bin/docker" name="/home/jessie/docker/man/man1/docker-attach.1" pid=10923 comm="docker" requested_mask="r" denied_mask="r" fsuid=1000 ouid=0
```

In the above example, you can see `profile=/usr/bin/docker`. This means the user has the `docker-engine` (Docker Engine daemon) profile loaded.

​	在上面的示例中，您可以看到 `profile=/usr/bin/docker`。这意味着用户加载了 `docker-engine`（Docker Engine 守护进程）配置文件。

Look at another log line:

​	再看另一条日志信息：

```text
[ 3256.689120] type=1400 audit(1405454041.341:73): apparmor="DENIED" operation="ptrace" profile="docker-default" pid=17651 comm="docker" requested_mask="receive" denied_mask="receive"
```

This time the profile is `docker-default`, which is run on containers by default unless in `privileged` mode. This line shows that apparmor has denied `ptrace` in the container. This is exactly as expected.

​	这次的配置文件是 `docker-default`，这是在容器上默认运行的配置文件（除非处于 `privileged` 模式）。此行显示了 AppArmor 在容器中拒绝了 `ptrace`。这是预期的行为。

### Use aa-status

If you need to check which profiles are loaded, you can use `aa-status`. The output looks like:

​	如果需要检查加载了哪些配置文件，可以使用 `aa-status`。输出如下：



```console
$ sudo aa-status
apparmor module is loaded.
14 profiles are loaded.
1 profiles are in enforce mode.
   docker-default
13 profiles are in complain mode.
   /usr/bin/docker
   /usr/bin/docker///bin/cat
   /usr/bin/docker///bin/ps
   /usr/bin/docker///sbin/apparmor_parser
   /usr/bin/docker///sbin/auplink
   /usr/bin/docker///sbin/blkid
   /usr/bin/docker///sbin/iptables
   /usr/bin/docker///sbin/mke2fs
   /usr/bin/docker///sbin/modprobe
   /usr/bin/docker///sbin/tune2fs
   /usr/bin/docker///sbin/xtables-multi
   /usr/bin/docker///sbin/zfs
   /usr/bin/docker///usr/bin/xz
38 processes have profiles defined.
37 processes are in enforce mode.
   docker-default (6044)
   ...
   docker-default (31899)
1 processes are in complain mode.
   /usr/bin/docker (29756)
0 processes are unconfined but have a profile defined.
```

The above output shows that the `docker-default` profile running on various container PIDs is in `enforce` mode. This means AppArmor is actively blocking and auditing in `dmesg` anything outside the bounds of the `docker-default` profile.

​	以上输出显示 `docker-default` 配置文件在各种容器 PID 上运行，并处于 `enforce` 模式。这意味着 AppArmor 正在主动阻止并记录超出 `docker-default` 配置文件范围的所有操作。

The output above also shows the `/usr/bin/docker` (Docker Engine daemon) profile is running in `complain` mode. This means AppArmor only logs to `dmesg` activity outside the bounds of the profile. (Except in the case of Ubuntu Trusty, where some interesting behaviors are enforced.)

​	输出还显示 `/usr/bin/docker`（Docker Engine 守护进程）配置文件处于 `complain` 模式。这意味着 AppArmor 仅将超出配置文件范围的活动记录到 `dmesg` 中。（在 Ubuntu Trusty 中会有一些特殊行为。）

## 参与 Docker 的 AppArmor 代码 Contribute to Docker's AppArmor code

Advanced users and package managers can find a profile for `/usr/bin/docker` (Docker Engine daemon) underneath [contrib/apparmor](https://github.com/moby/moby/tree/master/contrib/apparmor) in the Docker Engine source repository.

​	高级用户和软件包管理器可以在 Docker Engine 源代码库的 [contrib/apparmor](https://github.com/moby/moby/tree/master/contrib/apparmor) 下找到 `/usr/bin/docker`（Docker Engine 守护进程）的配置文件。

The `docker-default` profile for containers lives in [profiles/apparmor](https://github.com/moby/moby/tree/master/profiles/apparmor).

​	容器的 `docker-default` 配置文件位于 [profiles/apparmor](https://github.com/moby/moby/tree/master/profiles/apparmor) 中。
