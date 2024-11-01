+++
title = "启动开发环境"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/dev-environments/create-dev-env/](https://docs.docker.com/desktop/dev-environments/create-dev-env/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Launch a dev environment - 启动开发环境 

> **Important**
>
> 
>
> Dev Environments is no longer under active development.
>
> ​	开发环境不再进行活跃开发。
>
> While the current functionality remains available, it may take us longer to respond to support requests.
>
> ​	尽管当前功能仍然可用，但我们可能会对支持请求的响应时间更长。

You can launch a dev environment from a:

​	您可以从以下来源启动开发环境：

- Git repository
  - Git 仓库

- Branch or tag of a Git repository
  - Git 仓库的分支或标签

- Sub-folder of a Git repository
  - Git 仓库的子文件夹

- Local folder
  - 本地文件夹


This does not conflict with any of the local files or local tooling set up on your host.

​	这不会与主机上设置的本地文件或本地工具冲突。

> Tip
>
> Install the [Dev Environments browser extension](https://github.com/docker/dev-envs-extension) for [Chrome](https://chrome.google.com/webstore/detail/docker-dev-environments/gnagpachnalcofcblcgdbofnfakdbeka) or [Firefox](https://addons.mozilla.org/en-US/firefox/addon/docker-dev-environments/), to launch a dev environment faster.
>
> ​	安装 [开发环境浏览器扩展](https://github.com/docker/dev-envs-extension) 以更快启动开发环境：[Chrome](https://chrome.google.com/webstore/detail/docker-dev-environments/gnagpachnalcofcblcgdbofnfakdbeka) 或 [Firefox](https://addons.mozilla.org/en-US/firefox/addon/docker-dev-environments/)。

## 前置条件 Prerequisites

To get started with Dev Environments, you must also install the following tools and extension on your machine:

​	要开始使用开发环境，您还需要在您的机器上安装以下工具和扩展：

- [Git](https://git-scm.com/). Make sure add Git to your PATH if you're a Windows user.
  - [Git](https://git-scm.com/)（如果您是 Windows 用户，请确保将 Git 添加到 PATH）。

- [Visual Studio Code](https://code.visualstudio.com/)
- [Visual Studio Code Remote Containers Extension - Visual Studio Code 远程容器扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

After Git is installed, restart Docker Desktop. Select **Quit Docker Desktop**, and then start it again.

​	安装 Git 后，重启 Docker Desktop。选择 **退出 Docker Desktop**，然后重新启动它。

## 从 Git 仓库启动开发环境 Launch a dev environment from a Git repository

> **Note**
>
> 
>
> When cloning a Git repository using SSH, ensure you've added your SSH key to the ssh-agent. To do this, open a terminal and run `ssh-add <path to your private ssh key>`.
>
> ​	使用 SSH 克隆 Git 仓库时，请确保已将您的 SSH 密钥添加到 ssh-agent 中。要执行此操作，请打开终端并运行 `ssh-add <您的私钥路径>`。

> **Important**
>
> 
>
> If you have enabled the WSL 2 integration in Docker Desktop for Windows, make sure you have an SSH agent running in your WSL 2 distribution.
>
> ​	如果您在 Windows 上的 Docker Desktop 中启用了 WSL 2 集成，请确保在 WSL 2 分发中运行 SSH 代理。

To launch a dev environment:

​	启动开发环境的步骤：

1. From the **Dev Environments** tab in Docker Dashboard, select **Create**. The **Create a Dev Environment** dialog displays. 在 Docker Dashboard 的 **开发环境** 选项卡中，选择 **创建**。将显示 **创建开发环境** 对话框。
2. Select **Get Started**. 选择 **开始**。
3. Optional: Provide a name for you dev environment. 可选：为您的开发环境提供一个名称。
4. Select **Existing Git repo** as the source and then paste your Git repository link into the field provided. 选择源为 **已有 Git 仓库**，然后将您的 Git 仓库链接粘贴到提供的字段中。
5. Choose your IDE. You can choose either: 选择您的 IDE，可以选择以下两种：
   - **Visual Studio Code**. The Git repository is cloned into a Volume and attaches to your containers. This allows you to develop directly inside of them using Visual Studio Code. **Visual Studio Code**。Git 仓库被克隆到一个卷中并附加到您的容器中，这使您可以直接在容器中使用 Visual Studio Code 进行开发。
   - **Other**. The Git repository is cloned into your chosen local directory and attaches to your containers as a bind mount. This shares the directory from your computer to the container, and allows you to develop using any local editor or IDE. **其他**。Git 仓库被克隆到您选择的本地目录中并作为绑定挂载到您的容器中，这会将目录从计算机共享到容器中，使您可以使用任何本地编辑器或 IDE 进行开发。
6. Select **Continue**. 选择 **继续**。

To launch the application, run the command `make run` in your terminal. This opens an http server on port 8080. Open [http://localhost:8080](http://localhost:8080/) in your browser to see the running application.

​	要启动应用程序，请在终端中运行 `make run` 命令。这将在端口 8080 上启动一个 http 服务器。打开浏览器并访问 [http://localhost:8080](http://localhost:8080/) 以查看运行的应用程序。

## 从特定分支或标签启动 Launch from a specific branch or tag

You can launch a dev environment from a specific branch, for example a branch corresponding to a Pull Request, or a tag by adding `@mybranch` or `@tag` as a suffix to your Git URL:

​	您可以从特定的分支（例如对应于 Pull Request 的分支）或标签启动开发环境，方法是在 Git URL 后添加 `@mybranch` 或 `@tag` 后缀：

```
https://github.com/dockersamples/single-dev-env@mybranch
```

or

```
git@github.com:dockersamples/single-dev-env.git@mybranch
```

Docker then clones the repository with your specified branch or tag.

​	Docker 将使用您指定的分支或标签克隆该仓库。

## 从 Git 仓库的子文件夹启动 Launch from a subfolder of a Git repository

> Note
>
> Currently, Dev Environments is not able to detect the main language of the subdirectory. You need to define your own base image or services in a `compose-dev.yaml`file located in your subdirectory. For more information on how to configure, see the [React application with a Spring backend and a MySQL database sample](https://github.com/docker/awesome-compose/tree/master/react-java-mysql) or the [Go server with an Nginx proxy and a Postgres database sample](https://github.com/docker/awesome-compose/tree/master/nginx-golang-postgres).
>
> ​	目前，开发环境无法检测子目录的主要语言。您需要在子目录中的 `compose-dev.yaml` 文件中定义自己的基础镜像或服务。有关如何配置的更多信息，请参阅 [带有 Spring 后端和 MySQL 数据库的 React 应用示例](https://github.com/docker/awesome-compose/tree/master/react-java-mysql) 或 [带有 Nginx 代理和 Postgres 数据库的 Go 服务器示例](https://github.com/docker/awesome-compose/tree/master/nginx-golang-postgres)。

1. From **Dev Environments** in Docker Dashboard, select **Create**. The **Create a Dev Environment** dialog displays. 在 Docker Dashboard 的 **开发环境** 中选择 **创建**，会显示 **创建开发环境** 对话框。
2. Select **Get Started**. 选择 **开始**。
3. Optional: Provide a name for you dev environment. 可选：为您的开发环境提供一个名称。
4. Select **Existing Git repo** as the source and then paste the link of your Git repo subfolder into the field provided. 选择源为 **已有 Git 仓库**，然后将 Git 仓库子文件夹的链接粘贴到提供的字段中。
5. Choose your IDE. You can choose either: 选择您的 IDE。可以选择以下两种：
   - **Visual Studio Code**. The Git repository is cloned into a Volume and attaches to your containers. This allows you to develop directly inside of them using Visual Studio Code. **Visual Studio Code**。Git 仓库克隆到一个卷中并附加到您的容器中，允许您在 Visual Studio Code 中直接在容器内开发。
   - **Other**. The Git repository is cloned into your chosen local directory and attaches to your containers as a bind mount. This shares the directory from your computer to the container, and allows you to develop using any local editor or IDE. **其他**。Git 仓库克隆到您选择的本地目录中并作为绑定挂载到容器中，允许您使用任何本地编辑器或 IDE 进行开发。
6. Select **Continue**. 选择 **继续**。

To launch the application, run the command `make run` in your terminal. This opens an http server on port 8080. Open [http://localhost:8080](http://localhost:8080/) in your browser to see the running application.

​	要启动应用程序，在终端中运行 `make run` 命令。这将在端口 8080 上启动一个 HTTP 服务器。打开浏览器并访问 [http://localhost:8080](http://localhost:8080/) 以查看运行的应用程序。

## 从本地文件夹启动 Launch from a local folder

1. From **Dev Environments** in Docker Dashboard, select **Create**. The **Create a Dev Environment** dialog displays. 在 Docker Dashboard 的 **开发环境** 中选择 **创建**，会显示 **创建开发环境** 对话框。

2. Select **Get Started**. 选择 **开始**。

3. Optional: Provide a name for your dev environment. 可选：为您的开发环境提供一个名称。

4. Choose **Local directory** as the source. 选择源为 **本地目录**。

5. Select **Select** to open the root directory of the code that you would like to work on. 选择 **选择** 以打开您要开发的代码根目录。

   A directory from your computer is bind mounted to the container, so any changes you make locally is reflected in the dev environment. You can use an editor or IDE of your choice.
   
   ​	您的计算机目录将作为绑定挂载到容器中，因此您在本地所做的任何更改都会反映在开发环境中。您可以使用任何编辑器或 IDE。

> **Note**
>
> 
>
> When using a local folder for a dev environment, file changes are synchronized between your environment container and your local files. This can affect the performance inside the container, depending on the number of files in your local folder and the operations performed in the container.
>
> ​	使用本地文件夹作为开发环境时，文件更改会在环境容器和本地文件之间同步。根据本地文件夹中文件的数量和容器中执行的操作，这可能会影响容器内的性能。

## What's next?

Learn how to:

​	学习如何：

- [Set up a dev environment 设置开发环境]({{< ref "/manuals/DockerDesktop/DevEnvironmentsBeta/Setupadevenvironment" >}})
- [Distribute your dev environment 分发您的开发环境]({{< ref "/manuals/DockerDesktop/DevEnvironmentsBeta/Distributeyourdevenvironment" >}})
