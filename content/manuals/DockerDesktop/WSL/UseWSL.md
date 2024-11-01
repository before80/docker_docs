+++
title = "使用 WSL"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/wsl/use-wsl/](https://docs.docker.com/desktop/wsl/use-wsl/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use WSL - 使用 WSL

The following section describes how to start developing your applications using Docker and WSL 2. We recommend that you have your code in your default Linux distribution for the best development experience using Docker and WSL 2. After you have turned on the WSL 2 feature on Docker Desktop, you can start working with your code inside the Linux distro and ideally with your IDE still in Windows. This workflow is straightforward if you are using [VS Code](https://code.visualstudio.com/download).

​	以下部分描述了如何使用 Docker 和 WSL 2 开始开发应用程序。我们建议您将代码放在默认的 Linux 发行版中，以便使用 Docker 和 WSL 2 进行最佳开发体验。在 Docker Desktop 启用了 WSL 2 功能后，您可以在 Linux 发行版中处理代码，理想情况下，您的 IDE 仍在 Windows 中运行。如果您使用 [VS Code](https://code.visualstudio.com/download)，此工作流非常简单。

## 使用 Docker 和 WSL 2 进行开发 Develop with Docker and WSL 2

1. Open VS Code and install the [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) extension. This extension lets you work with a remote server in the Linux distro and your IDE client still on Windows. 打开 VS Code 并安装 [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) 扩展。该扩展允许您在 Linux 发行版的远程服务器上工作，同时在 Windows 上使用 IDE 客户端。

2. Open your terminal and type:

   打开终端并输入：

   ```console
   $ wsl
   ```

3. Navigate to your project directory and then type:

   导航到项目目录，然后输入：

   ```console
   $ code .
   ```

   This opens a new VS Code window connected remotely to your default Linux distro which you can check in the bottom corner of the screen.
   
   ​	这将在新的 VS Code 窗口中打开远程连接到您的默认 Linux 发行版，您可以在屏幕右下角查看连接的发行版。

Alternatively, you can open your default Linux distribution from the **Start** menu, navigate to your project directory, and then run `code .`

​	或者，您可以从 **开始** 菜单打开默认的 Linux 发行版，导航到项目目录，然后运行 `code .`。
