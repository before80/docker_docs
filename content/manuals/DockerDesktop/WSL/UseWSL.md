+++
title = "Use WSL"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/desktop/wsl/use-wsl/](https://docs.docker.com/desktop/wsl/use-wsl/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use WSL

The following section describes how to start developing your applications using Docker and WSL 2. We recommend that you have your code in your default Linux distribution for the best development experience using Docker and WSL 2. After you have turned on the WSL 2 feature on Docker Desktop, you can start working with your code inside the Linux distro and ideally with your IDE still in Windows. This workflow is straightforward if you are using [VS Code](https://code.visualstudio.com/download).

## [Develop with Docker and WSL 2](https://docs.docker.com/desktop/wsl/use-wsl/#develop-with-docker-and-wsl-2)

1. Open VS Code and install the [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) extension. This extension lets you work with a remote server in the Linux distro and your IDE client still on Windows.

2. Open your terminal and type:

   

   ```console
   $ wsl
   ```

3. Navigate to your project directory and then type:

   

   ```console
   $ code .
   ```

   This opens a new VS Code window connected remotely to your default Linux distro which you can check in the bottom corner of the screen.

Alternatively, you can open your default Linux distribution from the **Start** menu, navigate to your project directory, and then run `code .`
