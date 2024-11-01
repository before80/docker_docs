+++
title = "资源节约模式"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/use-desktop/resource-saver/](https://docs.docker.com/desktop/use-desktop/resource-saver/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker Desktop's Resource Saver mode - Docker Desktop 的资源节约模式

Resource Saver is a new feature available in Docker Desktop version 4.24 and later. It significantly reduces Docker Desktop's CPU and memory utilization on the host by 2 GBs or more, by automatically stopping the Docker Desktop Linux VM when no containers are running for a period of time. The default time is set to 5 minutes, but this can be adjusted to suit your needs.

​	**资源节约** 是 Docker Desktop 版本 4.24 及更高版本中的新功能。它通过在没有容器运行一段时间后自动停止 Docker Desktop 的 Linux VM，显著降低主机的 CPU 和内存使用，减少 2 GB 或更多。默认时间设置为 5 分钟，但可以根据需要进行调整。

With Resource Saver mode, Docker Desktop uses minimal system resources when it's idle, thereby allowing you to save battery life on your laptop and improve your multi-tasking experience.

​	在资源节约模式下，Docker Desktop 在空闲时使用的系统资源非常少，从而节省笔记本电脑的电池寿命并改善多任务处理体验。

## 如何配置资源节约模式 How to configure Resource Saver

Resource Saver is enabled by default but can be disabled by navigating to the **Resources** tab, in **Settings**. You can also configure the idle timer as shown below.

​	资源节约模式默认启用，但可以在 **设置** 的 **资源** 选项卡中禁用。您还可以按照以下说明配置空闲计时器。

![Resource Saver Settings](ResourceSavermode_img/resource-saver-settings.png)

If the values available aren't sufficient for your needs, you can reconfigure it to any value, as long as the value is larger than 30 seconds, by changing `autoPauseTimeoutSeconds` in the Docker Desktop `settings.json` file:

​	如果现有设置无法满足您的需求，可以通过更改 Docker Desktop `settings.json` 文件中的 `autoPauseTimeoutSeconds` 设置来配置为任意大于 30 秒的值：

- Mac: `~/Library/Group Containers/group.com.docker/settings.json`
- Windows: `C:\Users\[USERNAME]\AppData\Roaming\Docker\settings.json`
- Linux: `~/.docker/desktop/settings.json`

There's no need to restart Docker Desktop after reconfiguring.

​	重新配置后无需重启 Docker Desktop。

When Docker Desktop enters Resource Saver mode:

​	当 Docker Desktop 进入资源节约模式时：

- A leaf icon displays on the Docker Desktop status bar as well as on the Docker icon in the system tray. The following image shows the Linux VM CPU and memory utilization reduced to zero when Resource Saver mode is on.

  - 在 Docker Desktop 状态栏和系统托盘中的 Docker 图标上将显示叶子图标。以下图像显示了资源节约模式开启时，Linux VM 的 CPU 和内存使用量减少到零的情况。


  ![Resource Saver Status Bar](ResourceSavermode_img/resource-saver-status-bar.png)

- Docker commands that don't run containers, for example listing container images or volumes, don't necessarily trigger an exit from Resource Saver mode as Docker Desktop can serve such commands without unnecessarily waking up the Linux VM.

  - 不运行容器的 Docker 命令（例如列出镜像或卷）不会自动退出资源节约模式，因为 Docker Desktop 可以在不唤醒 Linux VM 的情况下提供这些命令的服务。


> **Note**
>
> 
>
> Docker Desktop exits the Resource Saver mode automatically when it needs to. Commands that cause an exit from Resource Saver take a little longer to execute (about 3 to 10 seconds) as Docker Desktop restarts the Linux VM. It's generally faster on Mac and Linux, and slower on Windows with Hyper-V. Once the Linux VM is restarted, subsequent container runs occur immediately as usual.
>
> ​	Docker Desktop 在需要时会自动退出资源节约模式。导致退出资源节约模式的命令执行时间稍长（约 3 到 10 秒），因为 Docker Desktop 需要重启 Linux VM。通常在 Mac 和 Linux 上速度更快，在使用 Hyper-V 的 Windows 上较慢。Linux VM 重启后，后续容器运行将立即如常进行。

## 资源节约模式与暂停模式的对比 Resource Saver mode versus Pause

Resource Saver has higher precedence than the older [Pause]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/PauseDockerDesktop" >}}) feature, meaning that while Docker Desktop is in Resource Saver mode, manually pausing Docker Desktop is not possible (nor does it make sense since Resource Saver actually stops the Docker Desktop Linux VM). In general, we recommend keeping Resource Saver enabled as opposed to disabling it and using the manual Pause feature, as it results in much better CPU and memory savings.

​	资源节约优先于旧的[暂停]({{< ref "/manuals/DockerDesktop/ExploreDockerDesktop/PauseDockerDesktop" >}})功能，这意味着在 Docker Desktop 处于资源节约模式时，无法手动暂停 Docker Desktop（因为资源节约实际上停止了 Docker Desktop 的 Linux VM）。总体而言，我们建议保持启用资源节约，而不是禁用它并使用手动暂停功能，因为它在 CPU 和内存节省方面表现更佳。

## Windows 上的资源节约模式 Resource Saver mode on Windows

Resource Saver works a bit differently on Windows with WSL. Instead of stopping the WSL VM, it only pauses the Docker Engine inside the `docker-desktop` WSL distro. That's because in WSL there's a single Linux VM shared by all WSL distros, so Docker Desktop can't stop the Linux VM (i.e., the WSL Linux VM is not owned by Docker Desktop). As a result, Resource Saver reduces CPU utilization on WSL, but it does not reduce Docker's memory utilization.

​	在使用 WSL 的 Windows 上，资源节约的工作方式略有不同。它不会停止 WSL VM，而只是暂停 `docker-desktop` WSL 发行版中的 Docker 引擎。原因是 WSL 中的所有发行版共享一个 Linux VM，因此 Docker Desktop 无法停止 Linux VM（即，WSL Linux VM 不归 Docker Desktop 所有）。因此，资源节约减少了 WSL 上的 CPU 利用率，但无法减少 Docker 的内存使用量。

To reduce memory utilization on WSL, we instead recommend that users enable WSL's `autoMemoryReclaim` feature as described in the [Docker Desktop WSL docs]({{< ref "/manuals/DockerDesktop/WSL" >}}). Finally, since Docker Desktop does not stop the Linux VM on WSL, exit from Resource Saver mode is immediate (there's no exit delay).

​	要减少 WSL 上的内存使用量，我们建议用户按照 [Docker Desktop WSL 文档]({{< ref "/manuals/DockerDesktop/WSL" >}})中的说明启用 WSL 的 `autoMemoryReclaim` 功能。最后，由于 Docker Desktop 并未停止 WSL 上的 Linux VM，因此退出资源节约模式是即时的（没有退出延迟）。

## 反馈 Feedback

To give feedback or report any bugs you may find, create an issue on the appropriate Docker Desktop GitHub repository:

​	要提供反馈或报告您可能发现的任何问题，请在相应的 Docker Desktop GitHub 存储库上创建一个问题：

- [for-mac](https://github.com/docker/for-mac)
- [for-win](https://github.com/docker/for-win)
