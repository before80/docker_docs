+++
title = "故障排除和诊断"
date = 2024-10-23T14:54:40+08:00
weight = 150
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/troubleshoot/](https://docs.docker.com/desktop/troubleshoot/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Troubleshoot Docker Desktop - Docker Desktop 故障排除

This page contains information on how to diagnose and troubleshoot Docker Desktop, and how to check the logs.

​	此页面包含诊断和故障排除 Docker Desktop 的信息，以及如何检查日志。

## 故障排除菜单 Troubleshoot menu

To navigate to **Troubleshoot** either:

​	导航到 **故障排除** 的方法：

- Select the Docker menu Docker menu ![whale menu](_index_img/whale-x.svg) and then **Troubleshoot**.
  - 选择 Docker 菜单![whale menu](_index_img/whale-x.svg)中的 **故障排除** 图标。

- Select the **Troubleshoot** icon near the top-right corner of Docker Dashboard.
  - 选择 Docker Dashboard 右上角附近的 **故障排除** 图标。


The **Troubleshooting** menu contains the following options:

​	**故障排除** 菜单包含以下选项：

- **Restart Docker Desktop**.
  - **重启 Docker Desktop**。

- **Reset Kubernetes cluster**. Select to delete all stacks and Kubernetes resources. For more information, see [Kubernetes](https://docs.docker.com/desktop/settings/#kubernetes).
  - **重置 Kubernetes 集群**：选择此项删除所有堆栈和 Kubernetes 资源。更多信息，请参见 [Kubernetes](https://docs.docker.com/desktop/settings/#kubernetes)。

- **Clean / Purge data**. This option resets all Docker data without a reset to factory defaults. Selecting this option results in the loss of existing settings.
  - **清理 / 清除数据**：此选项重置所有 Docker 数据，而不会恢复出厂默认设置。选择此选项将导致丢失现有设置。

- **Reset to factory defaults**: Choose this option to reset all options on Docker Desktop to their initial state, the same as when Docker Desktop was first installed.
  - **恢复出厂默认值**：选择此项将 Docker Desktop 上的所有选项重置为初始状态，即首次安装 Docker Desktop 时的状态。


If you are a Mac or Linux user, you also have the option to **Uninstall** Docker Desktop from your system.

​	Mac 或 Linux 用户还可以选择从系统中卸载 Docker Desktop。

> **Tip**
>
> 
>
> If you need to contact support, select the **Question mark** icon near the top-right corner of Docker Dashboard, and then select **Contact support**. Users with a paid Docker subscription can use this option to send a support request.
>
> ​	如果您需要联系支持，请选择 Docker Dashboard 右上角的 **问号** 图标，然后选择 **联系支持**。具有付费 Docker 订阅的用户可以使用此选项发送支持请求。

## 诊断 Diagnose

> **Tip**
>
> 
>
> If you do not find a solution in troubleshooting, browse the GitHub repositories or create a new issue:
>
> ​	如果在故障排除中未找到解决方案，请浏览 GitHub 仓库或创建新问题：
>
> - [docker/for-mac](https://github.com/docker/for-mac/issues)
> - [docker/for-win](https://github.com/docker/for-win/issues)
> - [docker/for-linux](https://github.com/docker/for-linux/issues)

### 通过应用程序诊断 Diagnose from the app

1. From **Troubleshoot**, select **Get support**. This opens the in-app Support page and starts collecting the diagnostics. 从 **故障排除** 中选择 **获取支持**。这会打开应用内支持页面并开始收集诊断信息。

2. When the diagnostics collection process is complete, select **Upload to get a Diagnostic ID**. 当诊断收集完成后，选择 **上传以获取诊断 ID**。

3. When the diagnostics are uploaded, Docker Desktop prints a diagnostic ID. Copy this ID. 当诊断上传完成后，Docker Desktop 会打印出一个诊断 ID。复制此 ID。

4. Use your diagnostics ID to get help: 使用诊断 ID 获取帮助：

   - If you have a paid Docker subscription, select **Contact support**. This opens the Docker Desktop support form. Fill in the information required and add the ID you copied in step three to the **Diagnostics ID field**. Then, select **Submit ticket** to request Docker Desktop support. 如果您有付费 Docker 订阅，选择 **联系支持**。这将打开 Docker Desktop 支持表单。填写所需信息，并将第三步中复制的 ID 添加到 **诊断 ID 字段**中。然后选择 **提交票证** 请求 Docker Desktop 支持。

     > **Note**
>
     > 
>
     > You must be signed in to Docker Desktop to access the support form. For information on what's covered as part of Docker Desktop support, see [Support]({{< ref "/manuals/Getsupport" >}}).
>
     > ​	您必须登录 Docker Desktop 才能访问支持表单。有关 Docker Desktop 支持的详细信息，请参阅 [支持]({{< ref "/manuals/Getsupport" >}})。

   - If you don't have a paid Docker subscription, select **Report a Bug** to open a new Docker Desktop issue on GitHub. Complete the information required and ensure you add the diagnostic ID you copied in step three.

     - 如果您没有付费 Docker 订阅，选择 **报告 Bug** 打开一个新的 Docker Desktop GitHub 问题页面。填写所需信息，并确保添加第三步中复制的诊断 ID。


### 通过错误消息诊断 Diagnose from an error message

1. When an error message appears, select **Gather diagnostics**. 当错误消息出现时，选择 **收集诊断信息**。

2. When the diagnostics are uploaded, Docker Desktop prints a diagnostic ID. Copy this ID. 当诊断上传完成后，Docker Desktop 会打印出一个诊断 ID。复制此 ID。

3. Use your diagnostics ID to get help: 使用诊断 ID 获取帮助：

   - If you have a paid Docker subscription, select **Contact support**. This opens the Docker Desktop support form. Fill in the information required and add the ID you copied in step three to the **Diagnostics ID field**. Then, select **Submit ticket** to request Docker Desktop support. 

     - 如果您有付费 Docker 订阅，选择 **联系支持**。填写信息，并在 **诊断 ID 字段**中添加第三步复制的 ID。然后选择 **提交票证** 请求 Docker Desktop 支持。

     
     > **Note**
     >
     > 
     >
     > You must be signed in to Docker Desktop to access the support form. For information on what's covered as part of Docker Desktop support, see [Support]({{< ref "/manuals/Getsupport" >}}).
     >
     > ​	您必须登录 Docker Desktop 才能访问支持表单。有关 Docker Desktop 支持范围的详细信息，请参阅 [支持]({{< ref "/manuals/Getsupport" >}})。
     
   - If you don't have a paid Docker subscription, you can open a new Docker Desktop issue on GitHub for [Mac](https://github.com/docker/for-mac/issues), [Windows](https://github.com/docker/for-win/issues), or [Linux](https://github.com/docker/for-linux/issues). Complete the information required and ensure you add the diagnostic ID printed in step two.
   
     - 如果您没有付费 Docker 订阅，可以在 GitHub 上为 [Mac](https://github.com/docker/for-mac/issues)、[Windows](https://github.com/docker/for-win/issues) 或 [Linux](https://github.com/docker/for-linux/issues) 创建新问题。确保添加第二步中打印的诊断 ID。
   

### 从终端诊断 Diagnose from the terminal

In some cases, it's useful to run the diagnostics yourself, for instance, if Docker Desktop cannot start.

​	在某些情况下，您可以自行运行诊断，例如当 Docker Desktop 无法启动时。

{{< tabpane text=true persist=disabled >}}

{{% tab header="Windows " %}}

1. Locate the `com.docker.diagnose` tool:

   找到 `com.docker.diagnose` 工具：

   ```console
   $ C:\Program Files\Docker\Docker\resources\com.docker.diagnose.exe
   ```

2. Create and upload the diagnostics ID. In PowerShell, run:

   创建并上传诊断 ID。在 PowerShell 中运行：

   ```console
   $ & "C:\Program Files\Docker\Docker\resources\com.docker.diagnose.exe" gather -upload
   ```

After the diagnostics have finished, the terminal displays your diagnostics ID and the path to the diagnostics file. The diagnostics ID is composed of your user ID and a timestamp. For example `BE9AFAAF-F68B-41D0-9D12-84760E6B8740/20190905152051`.

​	诊断完成后，终端将显示您的诊断 ID 和诊断文件的路径。诊断 ID 由用户 ID 和时间戳组成，例如 `BE9AFAAF-F68B-41D0-9D12-84760E6B8740/20190905152051`。

{{% /tab  %}}

{{% tab header="Mac " %}}

1. Locate the `com.docker.diagnose` tool:

   找到 `com.docker.diagnose` 工具：

   ```console
   $ /Applications/Docker.app/Contents/MacOS/com.docker.diagnose
   ```

2. Create and upload the diagnostics ID. Run:

   创建并上传诊断 ID。运行：

   ```console
   $ /Applications/Docker.app/Contents/MacOS/com.docker.diagnose gather -upload
   ```

After the diagnostics have finished, the terminal displays your diagnostics ID and the path to the diagnostics file. The diagnostics ID is composed of your user ID and a timestamp. For example `BE9AFAAF-F68B-41D0-9D12-84760E6B8740/20190905152051`.

​	诊断完成后，终端将显示您的诊断 ID 和诊断文件的路径。诊断 ID 由用户 ID 和时间戳组成，例如 `BE9AFAAF-F68B-41D0-9D12-84760E6B8740/20190905152051`。

{{% /tab  %}}

{{% tab header="Linux" %}}

1. Locate the `com.docker.diagnose` tool:

   找到 `com.docker.diagnose` 工具：

   ```console
   $ /opt/docker-desktop/bin/com.docker.diagnose
   ```

2. Create and upload the diagnostics ID. Run:

   创建并上传诊断 ID。运行：

   ```console
   $ /opt/docker-desktop/bin/com.docker.diagnose gather -upload
   ```

After the diagnostics have finished, the terminal displays your diagnostics ID and the path to the diagnostics file. The diagnostics ID is composed of your user ID and a timestamp. For example `BE9AFAAF-F68B-41D0-9D12-84760E6B8740/20190905152051`.

​	诊断完成后，终端将显示您的诊断 ID 和诊断文件的路径。诊断 ID 由用户 ID 和时间戳组成，例如 `BE9AFAAF-F68B-41D0-9D12-84760E6B8740/20190905152051`。

{{% /tab  %}}

{{< /tabpane >}}

------

To view the contents of the diagnostic file:

​	查看诊断文件内容：

{{< tabpane text=true persist=disabled >}}

{{% tab header="Windows " %}}

1. Unzip the file. In PowerShell, copy and paste the path to the diagnostics file into the following command and then run it. It should be similar to the following example:

   解压文件。在 PowerShell 中，将诊断文件路径复制并粘贴到以下命令中，然后运行。如下所示：

   ```powershell
   $ Expand-Archive -LiteralPath "C:\Users\testUser\AppData\Local\Temp\5DE9978A-3848-429E-8776-950FC869186F\20230607101602.zip" -DestinationPath "C:\Users\testuser\AppData\Local\Temp\5DE9978A-3848-429E-8776-950FC869186F\20230607101602"
   ```

2. Open the file in your preferred text editor. Run:

   在您喜欢的文本编辑器中打开文件。运行：

   ```powershell
   $ code <path-to-file>
   ```

{{% /tab  %}}

{{% tab header="Mac " %}}

Run:

运行：

```console
$ open /tmp/<your-diagnostics-ID>.zip
```

{{% /tab  %}}

{{% tab header="Linux" %}}

Run:

运行：

```console
$ unzip –l /tmp/<your-diagnostics-ID>.zip
```

{{% /tab  %}}

{{< /tabpane >}}

------

#### 使用诊断 ID 获取帮助 Use your diagnostics ID to get help

If you have a paid Docker subscription, select **Contact support**. This opens the Docker Desktop support form. Fill in the information required and add the ID you copied in step three to the **Diagnostics ID field**. Then, select **Submit ticket** to request Docker Desktop support.

​	如果您拥有付费的 Docker 订阅，请选择 **Contact support**。这将打开 Docker Desktop 支持表单。填写所需信息，并将您在第三步复制的诊断 ID 添加到 **Diagnostics ID field** 中。然后，选择 **Submit ticket** 以请求 Docker Desktop 支持。

If you don't have a paid Docker subscription, create an issue on GitHub:

​	如果您没有付费的 Docker 订阅，可以在 GitHub 上创建问题：

- [For Linux](https://github.com/docker/desktop-linux/issues)
- [For Mac](https://github.com/docker/for-mac/issues)
- [For Windows](https://github.com/docker/for-win/issues)

### 自诊断工具 Self-diagnose tool

Docker Desktop contains a self-diagnose tool which can help you identify some common problems.

​	Docker Desktop 包含一个自诊断工具，可帮助您识别一些常见问题。

{{< tabpane text=true persist=disabled >}}

{{% tab header="Windows " %}}

1. Locate the `com.docker.diagnose` tool.

   找到 `com.docker.diagnose` 工具。

   ```console
   $ C:\Program Files\Docker\Docker\resources\com.docker.diagnose.exe
   ```

2. In PowerShell, run the self-diagnose tool:

   在 PowerShell 中运行自诊断工具：

   ```console
   $ & "C:\Program Files\Docker\Docker\resources\com.docker.diagnose.exe" check
   ```

{{% /tab  %}}

{{% tab header="Mac " %}}

1. Locate the `com.docker.diagnose` tool.

   找到 `com.docker.diagnose` 工具。

   ```console
   $ /Applications/Docker.app/Contents/MacOS/com.docker.diagnose
   ```

2. Run the self-diagnose tool:

   运行自诊断工具：

   ```console
   $ /Applications/Docker.app/Contents/MacOS/com.docker.diagnose check
   ```

{{% /tab  %}}

{{% tab header="Linux" %}}

Locate the `com.docker.diagnose` tool.

​	找到 `com.docker.diagnose` 工具。

Run the self-diagnose tool:

​	运行自诊断工具：

```console
$ /opt/docker-desktop/bin/com.docker.diagnose check
```

{{% /tab  %}}

{{< /tabpane >}}

------

The tool runs a suite of checks and displays **PASS** or **FAIL** next to each check. If there are any failures, it highlights the most relevant at the end of the report.

​	该工具运行一系列检查，并在每项检查旁边显示 **PASS** 或 **FAIL**。如果存在任何故障，它将在报告末尾突出显示最相关的问题。

You can then create an issue on GitHub:

​	然后您可以在 GitHub 上创建问题：

- [For Linux](https://github.com/docker/desktop-linux/issues)
- [For Mac](https://github.com/docker/for-mac/issues)
- [For Windows](https://github.com/docker/for-win/issues)

## 检查日志 Check the logs

In addition to using the diagnose option to submit logs, you can browse the logs yourself.

​	除了使用诊断选项提交日志外，您还可以自行浏览日志。

{{< tabpane text=true persist=disabled >}}

{{% tab header="Windows " %}}

In PowerShell, run:

​	在 PowerShell 中运行：

```powershell
$ code $Env:LOCALAPPDATA\Docker\log
```

This opens up all the logs in your preferred text editor for you to explore.

​	这将在您喜欢的文本编辑器中打开所有日志供您查看。	

{{% /tab  %}}

{{% tab header="Mac " %}}

### From terminal

To watch the live flow of Docker Desktop logs in the command line, run the following script from your preferred shell.

​	要在命令行中实时查看 Docker Desktop 日志流量，请从您喜欢的 shell 中运行以下脚本。

```console
$ pred='process matches ".*(ocker|vpnkit).*" || (process in {"taskgated-helper", "launchservicesd", "kernel"} && eventMessage contains[c] "docker")'
$ /usr/bin/log stream --style syslog --level=debug --color=always --predicate "$pred"
```

Alternatively, to collect the last day of logs (`1d`) in a file, run:

​	或者，要将最近一天的日志（`1d`）收集到一个文件中，运行：

```console
$ /usr/bin/log show --debug --info --style syslog --last 1d --predicate "$pred" >/tmp/logs.txt
```

### From the Console app

Mac provides a built-in log viewer, named **Console**, which you can use to check Docker logs.

​	Mac 提供了一个内置日志查看器，名为 **Console**，可用于检查 Docker 日志。

The Console lives in `/Applications/Utilities`. You can search for it with Spotlight Search.

​	Console 位于 `/Applications/Utilities`，您可以通过 Spotlight 搜索找到它。

To read the Docker app log messages, type `docker` in the Console window search bar and press Enter. Then select `ANY` to expand the drop-down list next to your `docker` search entry, and select `Process`.

​	要读取 Docker 应用的日志消息，在 Console 窗口的搜索栏中输入 `docker`，然后按回车键。然后选择 `ANY`，扩展 `docker` 搜索条目旁边的下拉列表，并选择 `Process`。

![Mac Console search for Docker app](_index_img/console.png)

You can use the Console Log Query to search logs, filter the results in various ways, and create reports.

​	您可以使用 Console 日志查询来搜索日志、筛选结果，并创建报告。

{{% /tab  %}}

{{% tab header="Linux" %}}

You can access Docker Desktop logs by running the following command:

​	您可以通过运行以下命令来访问 Docker Desktop 日志：



```console
$ journalctl --user --unit=docker-desktop
```

You can also find the logs for the internal components included in Docker Desktop at `$HOME/.docker/desktop/log/`.

​	您还可以在 `$HOME/.docker/desktop/log/` 找到 Docker Desktop 的内部组件日志。

{{% /tab  %}}

{{< /tabpane >}}



This opens up all the logs in your preferred text editor for you to explore.

​	这将在您喜欢的文本编辑器中打开所有日志供您查看。

------

## 查看 Docker 守护进程日志 View the Docker daemon logs

Refer to the [Read the daemon logs]({{< ref "/manuals/DockerEngine/Daemon/Readthedaemonlogs" >}}) section to learn how to view the Docker Daemon logs.

​	请参阅 [读取守护进程日志]({{< ref "/manuals/DockerEngine/Daemon/Readthedaemonlogs" >}}) 部分，了解如何查看 Docker 守护进程日志。

## 进一步的资源 Further resources

- View specific [troubleshoot topics]({{< ref "/manuals/DockerDesktop/Troubleshootanddiagnose/Commontopics" >}}).
  - 查看特定的 [故障排除主题]({{< ref "/manuals/DockerDesktop/Troubleshootanddiagnose/Commontopics" >}})。
- Implement [workarounds for common problems]({{< ref "/manuals/DockerDesktop/Troubleshootanddiagnose/Workaroundsforcommonproblems" >}})
  - 实施 [常见问题的解决方法]({{< ref "/manuals/DockerDesktop/Troubleshootanddiagnose/Workaroundsforcommonproblems" >}})
- View information on [known issues]({{< ref "/manuals/DockerDesktop/Troubleshootanddiagnose/Knownissues" >}})
  - 查看 [已知问题]({{< ref "/manuals/DockerDesktop/Troubleshootanddiagnose/Knownissues" >}}) 的信息
