+++
title = "Security"
date = 2024-10-23T14:54:40+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/engine/security/](https://docs.docker.com/engine/security/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker Engine security - Docker Engine 安全性

There are four major areas to consider when reviewing Docker security:

​	在审查 Docker 安全性时，需要考虑四个主要方面：

- The intrinsic security of the kernel and its support for namespaces and cgroups
  - 内核的固有安全性及其对命名空间和 cgroups 的支持

- The attack surface of the Docker daemon itself
  - Docker 守护进程本身的攻击面

- Loopholes in the container configuration profile, either by default, or when customized by users.
  - 容器配置文件中的漏洞，无论是默认配置还是用户自定义配置

- The "hardening" security features of the kernel and how they interact with containers.
  - 内核的“加固”安全功能及其与容器的交互

## 内核命名空间 Kernel namespaces

Docker containers are very similar to LXC containers, and they have similar security features. When you start a container with `docker run`, behind the scenes Docker creates a set of namespaces and control groups for the container.

​	Docker 容器与 LXC 容器非常相似，并具有类似的安全特性。当您使用 `docker run` 启动容器时，Docker 会在后台为该容器创建一组命名空间和控制组。

Namespaces provide the first and most straightforward form of isolation. Processes running within a container cannot see, and even less affect, processes running in another container, or in the host system.

​	命名空间提供了第一层最直接的隔离。容器内运行的进程无法看到，也无法影响其他容器或主机系统中的进程。

Each container also gets its own network stack, meaning that a container doesn't get privileged access to the sockets or interfaces of another container. Of course, if the host system is setup accordingly, containers can interact with each other through their respective network interfaces — just like they can interact with external hosts. When you specify public ports for your containers or use [links]({{< ref "/manuals/DockerEngine/Networking/Legacycontainerlinks" >}}) then IP traffic is allowed between containers. They can ping each other, send/receive UDP packets, and establish TCP connections, but that can be restricted if necessary. From a network architecture point of view, all containers on a given Docker host are sitting on bridge interfaces. This means that they are just like physical machines connected through a common Ethernet switch; no more, no less.

​	每个容器还拥有自己的网络栈，这意味着容器无法直接访问其他容器的套接字或接口。当然，如果主机系统设置适当，容器可以通过各自的网络接口互相通信——就像它们可以与外部主机通信一样。当您为容器指定公共端口或使用[链接]({{< ref "/manuals/DockerEngine/Networking/Legacycontainerlinks" >}})时，允许容器之间的 IP 流量。它们可以互相 ping，发送/接收 UDP 包并建立 TCP 连接，但如果需要可以进行限制。在网络架构方面，给定 Docker 主机上的所有容器都位于网桥接口上。这意味着它们就像通过以太网交换机连接的物理机器。

How mature is the code providing kernel namespaces and private networking? Kernel namespaces were introduced [between kernel version 2.6.15 and 2.6.26](https://man7.org/linux/man-pages/man7/namespaces.7.html). This means that since July 2008 (date of the 2.6.26 release ), namespace code has been exercised and scrutinized on a large number of production systems. And there is more: the design and inspiration for the namespaces code are even older. Namespaces are actually an effort to reimplement the features of [OpenVZ](https://en.wikipedia.org/wiki/OpenVZ) in such a way that they could be merged within the mainstream kernel. And OpenVZ was initially released in 2005, so both the design and the implementation are pretty mature.

​	内核命名空间和私有网络的代码有多成熟？命名空间功能在 [内核版本 2.6.15 和 2.6.26 之间](https://man7.org/linux/man-pages/man7/namespaces.7.html) 引入。这意味着自 2008 年 7 月（2.6.26 版本发布之日）以来，命名空间代码在大量生产系统中得到了应用和审查。更重要的是，命名空间代码的设计和灵感源自更早的技术。命名空间实际上是对 [OpenVZ](https://en.wikipedia.org/wiki/OpenVZ) 功能的重新实现，使其可以合并到主流内核中。OpenVZ 最初发布于 2005 年，因此其设计和实现都相当成熟。

## 控制组 Control groups

Control Groups are another key component of Linux containers. They implement resource accounting and limiting. They provide many useful metrics, but they also help ensure that each container gets its fair share of memory, CPU, disk I/O; and, more importantly, that a single container cannot bring the system down by exhausting one of those resources.

​	控制组（Control Groups）是 Linux 容器的另一个关键组件。它们实现了资源记账和限制功能。控制组不仅提供许多有用的指标，还能确保每个容器都能获得公平的内存、CPU 和磁盘 I/O 份额，更重要的是，单个容器无法通过耗尽这些资源而导致系统瘫痪。

So while they do not play a role in preventing one container from accessing or affecting the data and processes of another container, they are essential to fend off some denial-of-service attacks. They are particularly important on multi-tenant platforms, like public and private PaaS, to guarantee a consistent uptime (and performance) even when some applications start to misbehave.

​	因此，虽然控制组在阻止一个容器访问或影响另一个容器的数据和进程方面并不起到作用，但它们对于防范某些拒绝服务攻击至关重要。控制组在多租户平台（如公有和私有 PaaS）上尤为重要，确保即使某些应用程序开始出现异常行为，仍能保证系统的一致性运行时间和性能。

Control Groups have been around for a while as well: the code was started in 2006, and initially merged in kernel 2.6.24.

​	控制组自 2006 年开始开发，最初合并到内核 2.6.24。

## Docker 守护进程的攻击面 Docker daemon attack surface

Running containers (and applications) with Docker implies running the Docker daemon. This daemon requires `root` privileges unless you opt-in to [Rootless mode]({{< ref "/manuals/DockerEngine/Security/Rootlessmode" >}}), and you should therefore be aware of some important details.

​	运行 Docker 容器（和应用程序）需要运行 Docker 守护进程。该守护进程需要 `root` 权限，除非您选择[无根模式]({{< ref "/manuals/DockerEngine/Security/Rootlessmode" >}})，因此需要注意一些重要的细节。

First of all, only trusted users should be allowed to control your Docker daemon. This is a direct consequence of some powerful Docker features. Specifically, Docker allows you to share a directory between the Docker host and a guest container; and it allows you to do so without limiting the access rights of the container. This means that you can start a container where the `/host` directory is the `/` directory on your host; and the container can alter your host filesystem without any restriction. This is similar to how virtualization systems allow filesystem resource sharing. Nothing prevents you from sharing your root filesystem (or even your root block device) with a virtual machine.

​	首先，只有受信任的用户才能控制您的 Docker 守护进程。这是 Docker 一些强大功能的直接结果。具体来说，Docker 允许主机和来宾容器之间共享目录，而不限制容器的访问权限。这意味着您可以启动一个容器，将主机的 `/host` 目录作为容器中的 `/` 目录，并且容器可以不受限制地更改主机文件系统。这类似于虚拟化系统允许的文件系统资源共享方式。没有任何限制可以阻止您与虚拟机共享根文件系统（甚至根块设备）。

This has a strong security implication: for example, if you instrument Docker from a web server to provision containers through an API, you should be even more careful than usual with parameter checking, to make sure that a malicious user cannot pass crafted parameters causing Docker to create arbitrary containers.

​	这具有很强的安全隐患：例如，如果您通过 API 从 Web 服务器管理 Docker 来配置容器，那么在参数检查上需要比平时更加小心，以确保恶意用户无法传递恶意参数导致 Docker 创建任意容器。

For this reason, the REST API endpoint (used by the Docker CLI to communicate with the Docker daemon) changed in Docker 0.5.2, and now uses a Unix socket instead of a TCP socket bound on 127.0.0.1 (the latter being prone to cross-site request forgery attacks if you happen to run Docker directly on your local machine, outside of a VM). You can then use traditional Unix permission checks to limit access to the control socket.

​	因此，从 Docker 0.5.2 版本开始，REST API 端点（用于 Docker CLI 与 Docker 守护进程通信）被更改为使用 Unix 套接字，而不是绑定在 127.0.0.1 的 TCP 套接字（如果您直接在本地机器上运行 Docker，而不是在虚拟机中，这种配置容易受到跨站请求伪造攻击）。可以使用传统的 Unix 权限检查来限制对控制套接字的访问。

You can also expose the REST API over HTTP if you explicitly decide to do so. However, if you do that, be aware of the above mentioned security implications. Note that even if you have a firewall to limit accesses to the REST API endpoint from other hosts in the network, the endpoint can be still accessible from containers, and it can easily result in the privilege escalation. Therefore it is *mandatory* to secure API endpoints with [HTTPS and certificates]({{< ref "/manuals/DockerEngine/Security/ProtecttheDockerdaemonsocket" >}}). Exposing the daemon API over HTTP without TLS is not permitted, and such a configuration causes the daemon to fail early on startup, see [Unauthenticated TCP connections](https://docs.docker.com/engine/deprecated/#unauthenticated-tcp-connections). It is also recommended to ensure that it is reachable only from a trusted network or VPN.

​	您还可以选择通过 HTTP 暴露 REST API，但要注意上述安全隐患。即使通过防火墙限制网络中其他主机对 REST API 端点的访问，容器仍然可以访问端点，且可能导致权限升级。因此，*强制*使用 [HTTPS 和证书]({{< ref "/manuals/DockerEngine/Security/ProtecttheDockerdaemonsocket" >}})来保护 API 端点。禁止在没有 TLS 的情况下通过 HTTP 暴露守护进程 API，这种配置会在启动时导致守护进程无法启动，详情请见[未经验证的 TCP 连接](https://docs.docker.com/engine/deprecated/#unauthenticated-tcp-connections)。建议确保 API 端点只能从受信任的网络或 VPN 访问。

You can also use `DOCKER_HOST=ssh://USER@HOST` or `ssh -L /path/to/docker.sock:/var/run/docker.sock` instead if you prefer SSH over TLS.

​	如果您更喜欢通过 SSH 而不是 TLS，也可以使用 `DOCKER_HOST=ssh://USER@HOST` 或 `ssh -L /path/to/docker.sock:/var/run/docker.sock`。

The daemon is also potentially vulnerable to other inputs, such as image loading from either disk with `docker load`, or from the network with `docker pull`. As of Docker 1.3.2, images are now extracted in a chrooted subprocess on Linux/Unix platforms, being the first-step in a wider effort toward privilege separation. As of Docker 1.10.0, all images are stored and accessed by the cryptographic checksums of their contents, limiting the possibility of an attacker causing a collision with an existing image.

​	守护进程还可能受到其他输入的攻击，例如通过 `docker load` 从磁盘加载镜像或通过 `docker pull` 从网络加载镜像。自 Docker 1.3.2 起，镜像在 Linux/Unix 平台上通过 chroot 子进程提取，这是一项更大权限隔离工作的第一步。从 Docker 1.10.0 开始，所有镜像都以其内容的加密校验和存储和访问，限制了攻击者与现有镜像发生冲突的可能性。

Finally, if you run Docker on a server, it is recommended to run exclusively Docker on the server, and move all other services within containers controlled by Docker. Of course, it is fine to keep your favorite admin tools (probably at least an SSH server), as well as existing monitoring/supervision processes, such as NRPE and collectd.

​	最后，如果在服务器上运行 Docker，建议在该服务器上专门运行 Docker，并将所有其他服务移入由 Docker 控制的容器中。当然，保留常用的管理工具（例如至少一个 SSH 服务器）以及现有的监控/管理进程（如 NRPE 和 collectd）是可以的。

## Linux 内核功能 Linux kernel capabilities

By default, Docker starts containers with a restricted set of capabilities. What does that mean?

​	默认情况下，Docker 启动的容器具有一组受限的功能。这意味着什么？

Capabilities turn the binary "root/non-root" dichotomy into a fine-grained access control system. Processes (like web servers) that just need to bind on a port below 1024 do not need to run as root: they can just be granted the `net_bind_service` capability instead. And there are many other capabilities, for almost all the specific areas where root privileges are usually needed. This means a lot for container security.

​	功能将“root/非 root”的二元权限系统转变为一种细粒度的访问控制系统。比如，只需要在 1024 以下端口绑定的进程（如 Web 服务器）不需要以 root 身份运行：它们只需授予 `net_bind_service` 功能即可。大多数情况下，root 权限所需的特定区域都可以通过单独的功能来实现。这对容器安全性至关重要。

Typical servers run several processes as `root`, including the SSH daemon, `cron` daemon, logging daemons, kernel modules, network configuration tools, and more. A container is different, because almost all of those tasks are handled by the infrastructure around the container:

​	典型服务器以 `root` 身份运行多个进程，包括 SSH 守护进程、`cron` 守护进程、日志守护进程、内核模块和网络配置工具。容器不同，因为几乎所有这些任务都是由围绕容器的基础设施处理的：

- SSH access are typically managed by a single server running on the Docker host
  - SSH 访问通常由在 Docker 主机上运行的单个服务器管理

- `cron`, when necessary, should run as a user process, dedicated and tailored for the app that needs its scheduling service, rather than as a platform-wide facility
  - `cron`（如有需要）应以用户进程运行，专用于需要调度服务的应用程序，而非作为平台范围的服务

- Log management is also typically handed to Docker, or to third-party services like Loggly or Splunk
  - 日志管理通常交由 Docker 或第三方服务（如 Loggly 或 Splunk）处理

- Hardware management is irrelevant, meaning that you never need to run `udevd` or equivalent daemons within containers
  - 硬件管理无关紧要，因此不需要在容器中运行 `udevd` 或等效守护进程

- Network management happens outside of the containers, enforcing separation of concerns as much as possible, meaning that a container should never need to perform `ifconfig`, `route`, or ip commands (except when a container is specifically engineered to behave like a router or firewall, of course)
  - 网络管理在容器之外进行，尽可能地实现职责分离，这意味着容器不需要执行 `ifconfig`、`route` 或 ip 命令（除非容器专门设计为路由器或防火墙）


This means that in most cases, containers do not need "real" root privileges at all* And therefore, containers can run with a reduced capability set; meaning that "root" within a container has much less privileges than the real "root". For instance, it is possible to:

​	这意味着大多数情况下，容器不需要“真正的”root 权限。因此，容器可以使用较少的权限集运行，这意味着容器内的“root”权限远不如真实的“root”权限。例如，可以：

- Deny all "mount" operations
  - 禁止所有“挂载”操作

- Deny access to raw sockets (to prevent packet spoofing)
  - 禁止访问原始套接字（防止数据包伪造）

- Deny access to some filesystem operations, like creating new device nodes, changing the owner of files, or altering attributes (including the immutable flag)
  - 禁止执行某些文件系统操作，如创建新设备节点、更改文件所有者或修改属性（包括不可变标志）

- Deny module loading
  - 禁止加载模块


This means that even if an intruder manages to escalate to root within a container, it is much harder to do serious damage, or to escalate to the host.

​	即使入侵者在容器内获得 root 权限，也更难以进行严重破坏或提升到主机权限。

This doesn't affect regular web apps, but reduces the vectors of attack by malicious users considerably. By default Docker drops all capabilities except [those needed](https://github.com/moby/moby/blob/master/oci/caps/defaults.go#L6-L19), an allowlist instead of a denylist approach. You can see a full list of available capabilities in [Linux manpages](https://man7.org/linux/man-pages/man7/capabilities.7.html).

​	这对常规 Web 应用没有影响，但显著减少了恶意用户的攻击向量。默认情况下，Docker 除了[必要功能](https://github.com/moby/moby/blob/master/oci/caps/defaults.go#L6-L19)外会禁用所有功能（采用允许清单，而不是拒绝清单）。您可以在 [Linux 手册页](https://man7.org/linux/man-pages/man7/capabilities.7.html)中查看完整功能列表。

One primary risk with running Docker containers is that the default set of capabilities and mounts given to a container may provide incomplete isolation, either independently, or when used in combination with kernel vulnerabilities.

​	运行 Docker 容器的主要风险之一是，默认分配给容器的功能和挂载可能无法提供完整隔离，尤其是在与内核漏洞组合使用时。

Docker supports the addition and removal of capabilities, allowing use of a non-default profile. This may make Docker more secure through capability removal, or less secure through the addition of capabilities. The best practice for users would be to remove all capabilities except those explicitly required for their processes.

​	Docker 支持添加和删除功能，从而允许使用非默认配置文件。这可以通过删除功能来提升 Docker 安全性，也可能因为添加功能而降低安全性。用户的最佳做法是删除所有功能，除了明确要求的功能。

## Docker 内容信任签名验证 Docker Content Trust signature verification

Docker Engine can be configured to only run signed images. The Docker Content Trust signature verification feature is built directly into the `dockerd` binary.

​	Docker Engine 可以配置为仅运行签名镜像。Docker 内容信任签名验证功能直接内置在 `dockerd` 二进制文件中，并在 Dockerd 配置文件中配置。This is configured in the Dockerd configuration file.

To enable this feature, trustpinning can be configured in `daemon.json`, whereby only repositories signed with a user-specified root key can be pulled and run.

​	启用该功能后，可在 `daemon.json` 中配置信任绑定（trustpinning），这样只有使用用户指定的根密钥签名的仓库才能被拉取和运行。

This feature provides more insight to administrators than previously available with the CLI for enforcing and performing image signature verification.

​	此功能为管理员提供了比 CLI 更深入的视角，用于强制和执行镜像签名验证。

For more information on configuring Docker Content Trust Signature Verification, go to [Content trust in Docker]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker" >}}).

​	有关 Docker 内容信任签名验证的更多信息，请参阅[Docker 中的内容信任]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker" >}})。

## 其他内核安全功能 Other kernel security features

Capabilities are just one of the many security features provided by modern Linux kernels. It is also possible to leverage existing, well-known systems like TOMOYO, AppArmor, SELinux, GRSEC, etc. with Docker.

​	功能仅是现代 Linux 内核提供的众多安全功能之一。可以结合 Docker 使用现有的知名系统，如 TOMOYO、AppArmor、SELinux、GRSEC 等。

While Docker currently only enables capabilities, it doesn't interfere with the other systems. This means that there are many different ways to harden a Docker host. Here are a few examples.

​	尽管 Docker 目前仅启用了功能，但它不会干扰其他系统。这意味着有多种不同的方法可以加固 Docker 主机。以下是一些示例。

- You can run a kernel with GRSEC and PAX. This adds many safety checks, both at compile-time and run-time; it also defeats many exploits, thanks to techniques like address randomization. It doesn't require Docker-specific configuration, since those security features apply system-wide, independent of containers.
  - 您可以运行带有 GRSEC 和 PAX 的内核。这增加了许多安全检查，包括编译时和运行时；它还通过地址随机化等技术抵御许多攻击。此类安全功能适用于整个系统，与容器无关，不需要特定的 Docker 配置。

- If your distribution comes with security model templates for Docker containers, you can use them out of the box. For instance, we ship a template that works with AppArmor and Red Hat comes with SELinux policies for Docker. These templates provide an extra safety net (even though it overlaps greatly with capabilities).
  - 如果您的发行版附带 Docker 容器的安全模型模板，您可以直接使用它们。例如，我们提供了适用于 AppArmor 的模板，而 Red Hat 则附带了适用于 Docker 的 SELinux 策略。这些模板提供了额外的安全保护（尽管它们与功能大致重叠）。

- You can define your own policies using your favorite access control mechanism.
  - 您可以使用您喜欢的访问控制机制来定义自己的策略。


Just as you can use third-party tools to augment Docker containers, including special network topologies or shared filesystems, tools exist to harden Docker containers without the need to modify Docker itself.

​	正如您可以使用第三方工具来扩展 Docker 容器（包括特殊的网络拓扑或共享文件系统），也存在工具可以在不修改 Docker 本身的情况下加强 Docker 容器的安全性。

As of Docker 1.10 User Namespaces are supported directly by the docker daemon. This feature allows for the root user in a container to be mapped to a non uid-0 user outside the container, which can help to mitigate the risks of container breakout. This facility is available but not enabled by default.

​	自 Docker 1.10 起，Docker 守护进程直接支持用户命名空间。此功能允许将容器内的 root 用户映射为容器外的非 uid-0 用户，从而帮助缓解容器逃逸的风险。该功能可用但默认未启用。

Refer to the [daemon command](https://docs.docker.com/reference/cli/dockerd/#daemon-user-namespace-options) in the command line reference for more information on this feature. Additional information on the implementation of User Namespaces in Docker can be found in [this blog post](https://integratedcode.us/2015/10/13/user-namespaces-have-arrived-in-docker/).

​	有关此功能的更多信息，请参阅[守护进程命令](https://docs.docker.com/reference/cli/dockerd/#daemon-user-namespace-options)命令行参考。关于 Docker 中用户命名空间实现的其他信息，请参阅[此博客文章](https://integratedcode.us/2015/10/13/user-namespaces-have-arrived-in-docker/)。

## 结论 Conclusions

Docker containers are, by default, quite secure; especially if you run your processes as non-privileged users inside the container.

​	默认情况下，Docker 容器相当安全；尤其是在容器内部以非特权用户身份运行进程时。

You can add an extra layer of safety by enabling AppArmor, SELinux, GRSEC, or another appropriate hardening system.

​	您可以通过启用 AppArmor、SELinux、GRSEC 或其他适当的加固系统来添加额外的安全层。

If you think of ways to make docker more secure, we welcome feature requests, pull requests, or comments on the Docker community forums.

​	如果您对提高 Docker 安全性有任何想法，我们欢迎功能请求、拉取请求或在 Docker 社区论坛上的评论。

## 相关信息 Related information

- [Use trusted images 使用可信镜像]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker" >}})
- [Seccomp security profiles for Docker Docker 的 Seccomp 安全配置文件]({{< ref "/manuals/DockerEngine/Security/SeccompsecurityprofilesforDocker" >}})
- [AppArmor security profiles for Docker Docker 的 AppArmor 安全配置文件]({{< ref "/manuals/DockerEngine/Security/AppArmorsecurityprofilesforDocker" >}})
- [On the Security of Containers (2014) 关于容器安全性的探讨（2014）](https://medium.com/@ewindisch/on-the-security-of-containers-2c60ffe25a9e)
- [Docker swarm mode overlay network security model - Docker 群模式的覆盖网络安全模型]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Overlaynetworkdriver" >}})
