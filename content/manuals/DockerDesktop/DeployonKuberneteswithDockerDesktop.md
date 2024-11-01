+++
title = "在 Kubernetes 上使用 Docker Desktop 部署"
date = 2024-10-23T14:54:40+08:00
weight = 100
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/kubernetes/](https://docs.docker.com/desktop/kubernetes/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Deploy on Kubernetes with Docker Desktop - 在 Kubernetes 上使用 Docker Desktop 部署

Docker Desktop includes a standalone Kubernetes server and client, as well as Docker CLI integration that runs on your machine.

​	Docker Desktop 包含一个独立的 Kubernetes 服务器和客户端，以及在您机器上运行的 Docker CLI 集成。

The Kubernetes server runs locally within your Docker instance, is not configurable, and is a single-node cluster. It runs within a Docker container on your local system, and is only for local testing.

​	Kubernetes 服务器在您的 Docker 实例内本地运行，无法配置且是单节点集群。它在本地系统上的 Docker 容器中运行，仅用于本地测试。

Turning on Kubernetes allows you to deploy your workloads in parallel, on Kubernetes, Swarm, and as standalone containers. Turning on or off the Kubernetes server does not affect your other workloads.

​	启用 Kubernetes 允许您并行在 Kubernetes、Swarm 和独立容器上部署工作负载。打开或关闭 Kubernetes 服务器不会影响您的其他工作负载。

## 安装并启用 Kubernetes - Install and turn on Kubernetes

1. From the Docker Dashboard, select the **Settings**. 从 Docker Dashboard 中选择 **Settings**。

2. Select **Kubernetes** from the left sidebar. 在左侧边栏选择 **Kubernetes**。

3. Next to **Enable Kubernetes**, select the checkbox. 勾选 **Enable Kubernetes**。

4. Select **Apply & Restart** to save the settings and then select **Install** to confirm. This instantiates images required to run the Kubernetes server as containers, and installs the `/usr/local/bin/kubectl` command on your machine. 选择 **Apply & Restart** 保存设置，然后选择 **Install** 确认。这将实例化运行 Kubernetes 服务器所需的镜像作为容器，并在您的机器上安装 `/usr/local/bin/kubectl` 命令。

   > **Important**
   >
   > 
   >
   > The `kubectl` binary is not automatically packaged with Docker Desktop for Linux. To install the kubectl command for Linux, see [Kubernetes documentation](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/). It should be installed at `/usr/local/bin/kubectl`.
   >
   > ​	`kubectl` 二进制文件不会自动包含在 Linux 版 Docker Desktop 中。要在 Linux 上安装 kubectl 命令，请参阅 [Kubernetes 文档](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)，它应安装在 `/usr/local/bin/kubectl`。

By default, Kubernetes containers are hidden from commands like `docker ps`, because managing them manually is not supported. Most users do not need this option. To see these internal containers, select **Show system containers (advanced)**.

​	默认情况下，Kubernetes 容器不会显示在 `docker ps` 等命令中，因为不支持手动管理它们。大多数用户不需要此选项。要查看这些内部容器，请选择 **Show system containers (advanced)**。

When Kubernetes is turned on and running, an additional status bar in the Docker Dashboard footer and Docker menu displays.

​	当 Kubernetes 启用且正在运行时，Docker Dashboard 底部状态栏和 Docker 菜单将显示额外的状态。

> **Note**
>
> 
>
> Docker Desktop does not upgrade your Kubernetes cluster automatically after a new update. To upgrade your Kubernetes cluster to the latest version, select **Reset Kubernetes Cluster**.
>
> ​	Docker Desktop 不会在新更新后自动升级您的 Kubernetes 集群。要将 Kubernetes 集群升级到最新版本，请选择 **Reset Kubernetes Cluster**。

## 使用 kubectl 命令 Use the kubectl command

Kubernetes integration provides the Kubernetes CLI command at `/usr/local/bin/kubectl` on Mac and at `C:\Program Files\Docker\Docker\Resources\bin\kubectl.exe` on Windows. This location may not be in your shell's `PATH` variable, so you may need to type the full path of the command or add it to the `PATH`.

​	Kubernetes 集成在 Mac 上提供了位于 `/usr/local/bin/kubectl` 的 Kubernetes CLI 命令，在 Windows 上则位于 `C:\Program Files\Docker\Docker\Resources\bin\kubectl.exe`。该位置可能不在 shell 的 `PATH` 变量中，因此您可能需要输入命令的完整路径或将其添加到 `PATH` 中。

If you have already installed `kubectl` and it is pointing to some other environment, such as `minikube` or a GKE cluster, ensure you change the context so that `kubectl` is pointing to `docker-desktop`:

​	如果您已安装 `kubectl` 并指向了其他环境，例如 `minikube` 或 GKE 集群，请确保更改上下文，使 `kubectl` 指向 `docker-desktop`：

```console
$ kubectl config get-contexts
$ kubectl config use-context docker-desktop
```

> **Tip**
>
> 
>
> Run the `kubectl` command in a CMD or PowerShell terminal, otherwise `kubectl config get-contexts` may return an empty result.
>
> ​	在 CMD 或 PowerShell 终端中运行 `kubectl` 命令，否则 `kubectl config get-contexts` 可能返回空结果。
>
> If you are using a different terminal and this happens, you can try setting the `kubeconfig` environment variable to the location of the `.kube/config` file.
>
> ​	如果您使用其他终端且出现这种情况，可以尝试将 `kubeconfig` 环境变量设置为 `.kube/config` 文件的位置。

If you installed `kubectl` using Homebrew, or by some other method, and experience conflicts, remove `/usr/local/bin/kubectl`.

​	如果您使用 Homebrew 或其他方法安装了 `kubectl` 并遇到冲突，请移除 `/usr/local/bin/kubectl`。

You can test the command by listing the available nodes:

​	您可以通过列出可用节点来测试命令：

```console
$ kubectl get nodes

NAME                 STATUS    ROLES            AGE       VERSION
docker-desktop       Ready     control-plane    3h        v1.29.1
```

For more information about `kubectl`, see the [`kubectl` documentation](https://kubernetes.io/docs/reference/kubectl/overview/).

​	有关 `kubectl` 的更多信息，请参阅 [`kubectl` 文档](https://kubernetes.io/docs/reference/kubectl/overview/)。

## 关闭并卸载 Kubernetes - Turn off and uninstall Kubernetes

To turn off Kubernetes in Docker Desktop: 要在 Docker Desktop 中关闭 Kubernetes：

1. From the Docker Dashboard, select the **Settings** icon. 从 Docker Dashboard 中选择 **Settings** 图标。
2. Select **Kubernetes** from the left sidebar. 在左侧边栏选择 **Kubernetes**。
3. Next to **Enable Kubernetes**, clear the checkbox 取消勾选 **Enable Kubernetes**。
4. Select **Apply & Restart** to save the settings.This stops and removes Kubernetes containers, and also removes the `/usr/local/bin/kubectl` command. 选择 **Apply & Restart** 保存设置。此操作将停止并删除 Kubernetes 容器，并移除 `/usr/local/bin/kubectl` 命令。
