+++
title = "使用用户命名空间隔离容器"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/security/userns-remap/](https://docs.docker.com/engine/security/userns-remap/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Isolate containers with a user namespace - 使用用户命名空间隔离容器

Linux namespaces provide isolation for running processes, limiting their access to system resources without the running process being aware of the limitations. For more information on Linux namespaces, see [Linux namespaces](https://www.linux.com/news/understanding-and-securing-linux-namespaces).

​	Linux 命名空间提供了运行进程的隔离，限制它们对系统资源的访问，而不会让运行的进程察觉到这些限制。关于 Linux 命名空间的更多信息，请参见 [Linux 命名空间](https://www.linux.com/news/understanding-and-securing-linux-namespaces)。

The best way to prevent privilege-escalation attacks from within a container is to configure your container's applications to run as unprivileged users. For containers whose processes must run as the `root` user within the container, you can re-map this user to a less-privileged user on the Docker host. The mapped user is assigned a range of UIDs which function within the namespace as normal UIDs from 0 to 65536, but have no privileges on the host machine itself.

​	防止容器内部权限提升攻击的最佳方法是将容器应用配置为以非特权用户身份运行。对于必须在容器内以 `root` 用户运行的进程，可以将此用户映射为 Docker 主机上的低特权用户。映射的用户被分配了一系列 UIDs，这些 UIDs 在命名空间内作为正常的 0 到 65536 的 UID 工作，但在主机上没有权限。

## 关于重新映射和从属用户与组 ID  About remapping and subordinate user and group IDs

The remapping itself is handled by two files: `/etc/subuid` and `/etc/subgid`. Each file works the same, but one is concerned with the user ID range, and the other with the group ID range. Consider the following entry in `/etc/subuid`:

​	重新映射由两个文件处理：`/etc/subuid` 和 `/etc/subgid`。这两个文件的工作方式相同，但分别处理用户 ID 和组 ID 范围。例如，以下是 `/etc/subuid` 中的一个条目：



```none
testuser:231072:65536
```

This means that `testuser` is assigned a subordinate user ID range of `231072` and the next 65536 integers in sequence. UID `231072` is mapped within the namespace (within the container, in this case) as UID `0` (`root`). UID `231073` is mapped as UID `1`, and so forth. If a process attempts to escalate privilege outside of the namespace, the process is running as an unprivileged high-number UID on the host, which does not even map to a real user. This means the process has no privileges on the host system at all.

​	这表示 `testuser` 被分配了从属用户 ID 范围，起始于 `231072`，共有 65536 个连续整数。UID `231072` 在命名空间内被映射为 UID `0`（即 `root`）。UID `231073` 被映射为 UID `1`，依此类推。如果进程尝试在命名空间之外提升权限，则它在主机上以无特权的高数值 UID 运行，而该 UID 不映射到任何实际用户，因此在主机系统上没有任何权限。



> **Note**
>
> 
>
> It is possible to assign multiple subordinate ranges for a given user or group by adding multiple non-overlapping mappings for the same user or group in the `/etc/subuid` or `/etc/subgid` file. In this case, Docker uses only the first five mappings, in accordance with the kernel's limitation of only five entries in `/proc/self/uid_map` and `/proc/self/gid_map`.
>
> ​	可以通过在 `/etc/subuid` 或 `/etc/subgid` 文件中为同一用户或组添加多个不重叠的映射来为其分配多个从属范围。在这种情况下，Docker 仅使用前五个映射，以符合内核对 `/proc/self/uid_map` 和 `/proc/self/gid_map` 仅支持五个条目的限制。

When you configure Docker to use the `userns-remap` feature, you can optionally specify an existing user and/or group, or you can specify `default`. If you specify `default`, a user and group `dockremap` is created and used for this purpose.

​	配置 Docker 使用 `userns-remap` 功能时，可以选择指定现有的用户和/或组，也可以指定 `default`。如果指定 `default`，则会创建一个用户和组 `dockremap` 并用于此目的。

> **Warning**
>
> 
>
> Some distributions do not automatically add the new group to the `/etc/subuid` and `/etc/subgid` files. If that's the case, you are may have to manually edit these files and assign non-overlapping ranges. This step is covered in [Prerequisites](https://docs.docker.com/engine/security/userns-remap/#prerequisites).
>
> ​	某些发行版不会自动将新组添加到 `/etc/subuid` 和 `/etc/subgid` 文件中。如果是这种情况，可能需要手动编辑这些文件并分配不重叠的范围。此步骤在[先决条件](https://docs.docker.com/engine/security/userns-remap/#prerequisites)中进行了说明。

It is very important that the ranges do not overlap, so that a process cannot gain access in a different namespace. On most Linux distributions, system utilities manage the ranges for you when you add or remove users.

​	确保范围不重叠非常重要，以防止进程在不同命名空间之间获得访问权限。在大多数 Linux 发行版中，系统工具会在添加或删除用户时自动管理这些范围。

This re-mapping is transparent to the container, but introduces some configuration complexity in situations where the container needs access to resources on the Docker host, such as bind mounts into areas of the filesystem that the system user cannot write to. From a security standpoint, it is best to avoid these situations.

​	这种重新映射对容器是透明的，但在容器需要访问 Docker 主机上的资源（例如绑定挂载文件系统中系统用户无写权限的区域）时会引入一些配置复杂性。从安全的角度来看，最好避免这种情况。

## 前置条件 Prerequisites

1. The subordinate UID and GID ranges must be associated with an existing user, even though the association is an implementation detail. The user owns the namespaced storage directories under `/var/lib/docker/`. If you don't want to use an existing user, Docker can create one for you and use that. If you want to use an existing username or user ID, it must already exist. Typically, this means that the relevant entries need to be in `/etc/passwd` and `/etc/group`, but if you are using a different authentication back-end, this requirement may translate differently. 必须将从属 UID 和 GID 范围与现有用户关联，即使该关联只是实现细节。该用户拥有 `/var/lib/docker/` 下的命名空间存储目录。如果不想使用现有用户，Docker 可以为您创建并使用一个用户。如果要使用现有的用户名或用户 ID，则该用户必须已存在。通常，这意味着相关条目需要在 `/etc/passwd` 和 `/etc/group` 中，但如果使用的是不同的认证后端，则此要求可能会有所不同。

   To verify this, use the `id` command:

   使用 `id` 命令验证此点：

   ```console
   $ id testuser
   
   uid=1001(testuser) gid=1001(testuser) groups=1001(testuser)
   ```

2. The way the namespace remapping is handled on the host is using two files, `/etc/subuid` and `/etc/subgid`. These files are typically managed automatically when you add or remove users or groups, but on some distributions, you may need to manage these files manually. 主机上的命名空间重新映射处理依赖于两个文件：`/etc/subuid` 和 `/etc/subgid`。这些文件通常会在添加或删除用户或组时自动管理，但在某些发行版中，可能需要手动管理这些文件。

   Each file contains three fields: the username or ID of the user, followed by a beginning UID or GID (which is treated as UID or GID 0 within the namespace) and a maximum number of UIDs or GIDs available to the user. For instance, given the following entry:

   每个文件包含三个字段：用户名或用户 ID、起始 UID 或 GID（在命名空间内视为 UID 或 GID 0）和该用户可用的最大 UID 或 GID 数量。例如，以下条目：

   ```none
   testuser:231072:65536
   ```

   This means that user-namespaced processes started by `testuser` are owned by host UID `231072` (which looks like UID `0` inside the namespace) through 296607 (231072 + 65536 - 1). These ranges should not overlap, to ensure that namespaced processes cannot access each other's namespaces.

   ​	表示 `testuser` 启动的用户命名空间进程属于主机 UID `231072`（在命名空间内显示为 UID `0`），直到 296607（231072 + 65536 - 1）。这些范围不应重叠，以确保命名空间进程无法访问彼此的命名空间。

   After adding your user, check `/etc/subuid` and `/etc/subgid` to see if your user has an entry in each. If not, you need to add it, being careful to avoid overlap.

   ​	添加用户后，检查 `/etc/subuid` 和 `/etc/subgid` 看用户是否在其中。如果没有，则需要添加条目，注意避免重叠。

   If you want to use the `dockremap` user automatically created by Docker, check for the `dockremap` entry in these files after configuring and restarting Docker.

   ​	如果想使用 Docker 自动创建的 `dockremap` 用户，请在配置并重启 Docker 后检查这些文件中的 `dockremap` 条目。

3. If there are any locations on the Docker host where the unprivileged user needs to write, adjust the permissions of those locations accordingly. This is also true if you want to use the `dockremap` user automatically created by Docker, but you can't modify the permissions until after configuring and restarting Docker. 如果 Docker 主机上有非特权用户需要写入的位置，调整这些位置的权限。同样适用于 Docker 自动创建的 `dockremap` 用户，但在配置并重启 Docker 后才能修改权限。

4. Enabling `userns-remap` effectively masks existing image and container layers, as well as other Docker objects within `/var/lib/docker/`. This is because Docker needs to adjust the ownership of these resources and actually stores them in a subdirectory within `/var/lib/docker/`. It is best to enable this feature on a new Docker installation rather than an existing one. 启用 `userns-remap` 有效地屏蔽了现有的镜像和容器层以及 `/var/lib/docker/` 中的其他 Docker 对象。因为 Docker 需要调整这些资源的所有权，实际存储在 `/var/lib/docker/` 的子目录中。最好在新的 Docker 安装上启用此功能，而不是在现有的安装上启用。

   Along the same lines, if you disable `userns-remap` you can't access any of the resources created while it was enabled.

   ​	同样地，如果禁用 `userns-remap`，将无法访问启用期间创建的资源。

5. Check the [limitations](https://docs.docker.com/engine/security/userns-remap/#user-namespace-known-limitations) on user namespaces to be sure your use case is possible. 检查用户命名空间的[已知限制](https://docs.docker.com/engine/security/userns-remap/#user-namespace-known-limitations)，确保您的使用场景可行。

## 在守护进程上启用 userns-remap -  Enable userns-remap on the daemon

You can start `dockerd` with the `--userns-remap` flag or follow this procedure to configure the daemon using the `daemon.json` configuration file. The `daemon.json` method is recommended. If you use the flag, use the following command as a model:

​	可以使用 `--userns-remap` 标志启动 `dockerd`，或按照以下步骤使用 `daemon.json` 配置文件来配置守护进程。建议使用 `daemon.json` 方法。若使用标志，参考以下命令：



```console
$ dockerd --userns-remap="testuser:testuser"
```

1. Edit `/etc/docker/daemon.json`. Assuming the file was previously empty, the following entry enables `userns-remap` using user and group called `testuser`. You can address the user and group by ID or name. You only need to specify the group name or ID if it is different from the user name or ID. If you provide both the user and group name or ID, separate them by a colon (`:`) character. The following formats all work for the value, assuming the UID and GID of `testuser` are `1001`: 编辑 `/etc/docker/daemon.json`。假设文件之前为空，以下条目启用 `userns-remap` 并使用名为 `testuser` 的用户和组。可以通过 ID 或名称指定用户和组。只有在组名或 ID 与用户名或 ID 不同时，才需要指定组名或 ID。如果同时提供用户和组名或 ID，请用冒号 (`:`) 分隔。以下格式都适用，假设 `testuser` 的 UID 和 GID 为 `1001`：

   - `testuser`
   - `testuser:testuser`
   - `1001`
   - `1001:1001`
   - `testuser:1001`
   - `1001:testuser`

   

   ```json
   {
     "userns-remap": "testuser"
   }
   ```

   > **Note**
   >
   > 
   >
   > To use the `dockremap` user and have Docker create it for you, set the value to `default` rather than `testuser`.
   >
   > ​	要使用 Docker 自动创建的 `dockremap` 用户，将值设置为 `default` 而不是 `testuser`。

   Save the file and restart Docker.

   ​	保存文件并重启 Docker。

2. If you are using the `dockremap` user, verify that Docker created it using the `id` command.

   如果使用 `dockremap` 用户，请使用 `id` 命令验证 Docker 是否已创建该用户。

   ```console
   $ id dockremap
   
   uid=112(dockremap) gid=116(dockremap) groups=116(dockremap)
   ```

   Verify that the entry has been added to `/etc/subuid` and `/etc/subgid`:

   ​	验证条目已添加到 `/etc/subuid` 和 `/etc/subgid` 中：

   ```console
   $ grep dockremap /etc/subuid
   
   dockremap:231072:65536
   
   $ grep dockremap /etc/subgid
   
   dockremap:231072:65536
   ```

   If these entries are not present, edit the files as the `root` user and assign a starting UID and GID that is the highest-assigned one plus the offset (in this case, `65536`). Be careful not to allow any overlap in the ranges.

   ​	如果没有这些条目，请以 `root` 用户身份编辑文件，分配起始 UID 和 GID，为最高分配值加上偏移量（此例为 `65536`）。注意不要使范围重叠。

3. Verify that previous images are not available using the `docker image ls` command. The output should be empty. 使用 `docker image ls` 命令验证先前的镜像不可用。输出应为空。

4. Start a container from the `hello-world` image.

   从 `hello-world` 镜像启动一个容器。

   ```console
   $ docker run hello-world
   ```

5. Verify that a namespaced directory exists within `/var/lib/docker/` named with the UID and GID of the namespaced user, owned by that UID and GID, and not group-or-world-readable. Some of the subdirectories are still owned by `root` and have different permissions.

   验证 `/var/lib/docker/` 下是否存在以命名空间用户 UID 和 GID 命名的目录，并且由该 UID 和 GID 所有，且不允许组或其他用户读取。部分子目录仍由 `root` 拥有，权限有所不同。

   ```console
   $ sudo ls -ld /var/lib/docker/231072.231072/
   
   drwx------ 11 231072 231072 11 Jun 21 21:19 /var/lib/docker/231072.231072/
   
   $ sudo ls -l /var/lib/docker/231072.231072/
   
   total 14
   drwx------ 5 231072 231072 5 Jun 21 21:19 aufs
   drwx------ 3 231072 231072 3 Jun 21 21:21 containers
   drwx------ 3 root   root   3 Jun 21 21:19 image
   drwxr-x--- 3 root   root   3 Jun 21 21:19 network
   drwx------ 4 root   root   4 Jun 21 21:19 plugins
   drwx------ 2 root   root   2 Jun 21 21:19 swarm
   drwx------ 2 231072 231072 2 Jun 21 21:21 tmp
   drwx------ 2 root   root   2 Jun 21 21:19 trust
   drwx------ 2 231072 231072 3 Jun 21 21:19 volumes
   ```

   Your directory listing may have some differences, especially if you use a different container storage driver than `aufs`.

   ​	您的目录列表可能会有一些差异，特别是如果您使用不同的容器存储驱动程序而不是 `aufs`。
   
   The directories which are owned by the remapped user are used instead of the same directories directly beneath `/var/lib/docker/` and the unused versions (such as `/var/lib/docker/tmp/` in the example here) can be removed. Docker does not use them while `userns-remap` is enabled. 
   
   ​	属于重新映射用户的目录会替代 `/var/lib/docker/` 下的相同目录，未使用的版本（例如 `/var/lib/docker/tmp/`）可以删除。启用 `userns-remap` 时 Docker 不会使用它们。

## 为特定容器禁用命名空间重新映射 Disable namespace remapping for a container

If you enable user namespaces on the daemon, all containers are started with user namespaces enabled by default. In some situations, such as privileged containers, you may need to disable user namespaces for a specific container. See [user namespace known limitations](https://docs.docker.com/engine/security/userns-remap/#user-namespace-known-limitations) for some of these limitations.

​	如果在守护进程上启用了用户命名空间，则默认情况下所有容器都会启用用户命名空间。在某些情况下（如特权容器），可能需要为特定容器禁用用户命名空间。有关这些限制的一些说明，请参阅[用户命名空间已知限制](https://docs.docker.com/engine/security/userns-remap/#user-namespace-known-limitations)。

To disable user namespaces for a specific container, add the `--userns=host` flag to the `docker container create`, `docker container run`, or `docker container exec` command.

​	要为特定容器禁用用户命名空间，请在 `docker container create`、`docker container run` 或 `docker container exec` 命令中添加 `--userns=host` 标志。

There is a side effect when using this flag: user remapping will not be enabled for that container but, because the read-only (image) layers are shared between containers, ownership of the containers filesystem will still be remapped.

​	使用此标志时会产生一个副作用：该容器不会启用用户重新映射，但由于只读层（镜像层）在容器之间共享，容器文件系统的所有权仍将重新映射。

What this means is that the whole container filesystem will belong to the user specified in the `--userns-remap` daemon config (`231072` in the example above). This can lead to unexpected behavior of programs inside the container. For instance `sudo` (which checks that its binaries belong to user `0`) or binaries with a `setuid` flag.

​	这意味着整个容器文件系统将属于 `--userns-remap` 守护进程配置中指定的用户（此示例中为 `231072`）。这可能会导致容器内程序的意外行为，例如 `sudo`（它会检查二进制文件是否属于用户 `0`）或带有 `setuid` 标志的二进制文件。

## 用户命名空间的已知限制 User namespace known limitations

The following standard Docker features are incompatible with running a Docker daemon with user namespaces enabled:

​	以下标准 Docker 功能与启用了用户命名空间的 Docker 守护进程不兼容：

- Sharing PID or NET namespaces with the host (`--pid=host` or `--network=host`).
  - 与主机共享 PID 或 NET 命名空间（`--pid=host` 或 `--network=host`）。

- External (volume or storage) drivers which are unaware or incapable of using daemon user mappings.
  - 不支持或无法使用守护进程用户映射的外部（卷或存储）驱动程序。

- Using the `--privileged` mode flag on `docker run` without also specifying `--userns=host`.
  - 在 `docker run` 上使用 `--privileged` 模式标志而不指定 `--userns=host`。


User namespaces are an advanced feature and require coordination with other capabilities. For example, if volumes are mounted from the host, file ownership must be pre-arranged need read or write access to the volume contents.

​	用户命名空间是一个高级功能，需要与其他功能协调使用。例如，如果从主机挂载卷，则必须预先安排文件所有权以满足卷内容的读写访问需求。

While the root user inside a user-namespaced container process has many of the expected privileges of the superuser within the container, the Linux kernel imposes restrictions based on internal knowledge that this is a user-namespaced process. One notable restriction is the inability to use the `mknod` command. Permission is denied for device creation within the container when run by the `root` user.

​	虽然用户命名空间内的容器进程的 root 用户具有容器内超级用户的大部分预期权限，但 Linux 内核根据这是用户命名空间进程的内部知识强加了限制。一个显著的限制是无法使用 `mknod` 命令。容器内 `root` 用户在创建设备时权限被拒绝。
