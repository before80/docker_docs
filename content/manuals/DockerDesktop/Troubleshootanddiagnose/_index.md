+++
title = "Troubleshoot and diagnose"
date = 2024-10-23T14:54:40+08:00
weight = 150
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/desktop/troubleshoot/](https://docs.docker.com/desktop/troubleshoot/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Troubleshoot Docker Desktop

This page contains information on how to diagnose and troubleshoot Docker Desktop, and how to check the logs.

## [Troubleshoot menu](https://docs.docker.com/desktop/troubleshoot/#troubleshoot-menu)

To navigate to **Troubleshoot** either:

- Select the Docker menu Docker menu ![whale menu](_index_img/whale-x.svg+xml) and then **Troubleshoot**.
- Select the **Troubleshoot** icon near the top-right corner of Docker Dashboard.

The **Troubleshooting** menu contains the following options:

- **Restart Docker Desktop**.
- **Reset Kubernetes cluster**. Select to delete all stacks and Kubernetes resources. For more information, see [Kubernetes](https://docs.docker.com/desktop/settings/#kubernetes).
- **Clean / Purge data**. This option resets all Docker data without a reset to factory defaults. Selecting this option results in the loss of existing settings.
- **Reset to factory defaults**: Choose this option to reset all options on Docker Desktop to their initial state, the same as when Docker Desktop was first installed.

If you are a Mac or Linux user, you also have the option to **Uninstall** Docker Desktop from your system.

> **Tip**
>
> 
>
> If you need to contact support, select the **Question mark** icon near the top-right corner of Docker Dashboard, and then select **Contact support**. Users with a paid Docker subscription can use this option to send a support request.

## [Diagnose](https://docs.docker.com/desktop/troubleshoot/#diagnose)

> **Tip**
>
> 
>
> If you do not find a solution in troubleshooting, browse the GitHub repositories or create a new issue:
>
> - [docker/for-mac](https://github.com/docker/for-mac/issues)
> - [docker/for-win](https://github.com/docker/for-win/issues)
> - [docker/for-linux](https://github.com/docker/for-linux/issues)

### [Diagnose from the app](https://docs.docker.com/desktop/troubleshoot/#diagnose-from-the-app)

1. From **Troubleshoot**, select **Get support**. This opens the in-app Support page and starts collecting the diagnostics.

2. When the diagnostics collection process is complete, select **Upload to get a Diagnostic ID**.

3. When the diagnostics are uploaded, Docker Desktop prints a diagnostic ID. Copy this ID.

4. Use your diagnostics ID to get help:

   - If you have a paid Docker subscription, select

      

     Contact support

     . This opens the Docker Desktop support form. Fill in the information required and add the ID you copied in step three to the

      

     Diagnostics ID field

     . Then, select

      

     Submit ticket

      

     to request Docker Desktop support.

     > **Note**
     >
     > 
     >
     > You must be signed in to Docker Desktop to access the support form. For information on what's covered as part of Docker Desktop support, see [Support](https://docs.docker.com/support/).

   - If you don't have a paid Docker subscription, select **Report a Bug** to open a new Docker Desktop issue on GitHub. Complete the information required and ensure you add the diagnostic ID you copied in step three.

### [Diagnose from an error message](https://docs.docker.com/desktop/troubleshoot/#diagnose-from-an-error-message)

1. When an error message appears, select **Gather diagnostics**.

2. When the diagnostics are uploaded, Docker Desktop prints a diagnostic ID. Copy this ID.

3. Use your diagnostics ID to get help:

   - If you have a paid Docker subscription, select

      

     Contact support

     . This opens the Docker Desktop support form. Fill in the information required and add the ID you copied in step three to the

      

     Diagnostics ID field

     . Then, select

      

     Submit ticket

      

     to request Docker Desktop support.

     > **Note**
     >
     > 
     >
     > You must be signed in to Docker Desktop to access the support form. For information on what's covered as part of Docker Desktop support, see [Support](https://docs.docker.com/support/).

   - If you don't have a paid Docker subscription, you can open a new Docker Desktop issue on GitHub for [Mac](https://github.com/docker/for-mac/issues), [Windows](https://github.com/docker/for-win/issues), or [Linux](https://github.com/docker/for-linux/issues). Complete the information required and ensure you add the diagnostic ID printed in step two.

### [Diagnose from the terminal](https://docs.docker.com/desktop/troubleshoot/#diagnose-from-the-terminal)

In some cases, it's useful to run the diagnostics yourself, for instance, if Docker Desktop cannot start.

Windows Mac Linux

------

1. Locate the `com.docker.diagnose` tool:

   

   ```console
   $ C:\Program Files\Docker\Docker\resources\com.docker.diagnose.exe
   ```

2. Create and upload the diagnostics ID. In PowerShell, run:

   

   ```console
   $ & "C:\Program Files\Docker\Docker\resources\com.docker.diagnose.exe" gather -upload
   ```

After the diagnostics have finished, the terminal displays your diagnostics ID and the path to the diagnostics file. The diagnostics ID is composed of your user ID and a timestamp. For example `BE9AFAAF-F68B-41D0-9D12-84760E6B8740/20190905152051`.

------

To view the contents of the diagnostic file:

Windows Mac Linux

------

1. Unzip the file. In PowerShell, copy and paste the path to the diagnostics file into the following command and then run it. It should be similar to the following example:

   

   ```powershell
   $ Expand-Archive -LiteralPath "C:\Users\testUser\AppData\Local\Temp\5DE9978A-3848-429E-8776-950FC869186F\20230607101602.zip" -DestinationPath "C:\Users\testuser\AppData\Local\Temp\5DE9978A-3848-429E-8776-950FC869186F\20230607101602"
   ```

2. Open the file in your preferred text editor. Run:

   

   ```powershell
   $ code <path-to-file>
   ```

------

#### [Use your diagnostics ID to get help](https://docs.docker.com/desktop/troubleshoot/#use-your-diagnostics-id-to-get-help)

If you have a paid Docker subscription, select **Contact support**. This opens the Docker Desktop support form. Fill in the information required and add the ID you copied in step three to the **Diagnostics ID field**. Then, select **Submit ticket** to request Docker Desktop support.

If you don't have a paid Docker subscription, create an issue on GitHub:

- [For Linux](https://github.com/docker/desktop-linux/issues)
- [For Mac](https://github.com/docker/for-mac/issues)
- [For Windows](https://github.com/docker/for-win/issues)

### [Self-diagnose tool](https://docs.docker.com/desktop/troubleshoot/#self-diagnose-tool)

Docker Desktop contains a self-diagnose tool which can help you identify some common problems.

Windows Mac Linux

------

1. Locate the `com.docker.diagnose` tool.

   

   ```console
   $ C:\Program Files\Docker\Docker\resources\com.docker.diagnose.exe
   ```

2. In PowerShell, run the self-diagnose tool:

   

   ```console
   $ & "C:\Program Files\Docker\Docker\resources\com.docker.diagnose.exe" check
   ```

------

The tool runs a suite of checks and displays **PASS** or **FAIL** next to each check. If there are any failures, it highlights the most relevant at the end of the report.

You can then create an issue on GitHub:

- [For Linux](https://github.com/docker/desktop-linux/issues)
- [For Mac](https://github.com/docker/for-mac/issues)
- [For Windows](https://github.com/docker/for-win/issues)

## [Check the logs](https://docs.docker.com/desktop/troubleshoot/#check-the-logs)

In addition to using the diagnose option to submit logs, you can browse the logs yourself.

Windows Mac Linux

------

In PowerShell, run:



```powershell
$ code $Env:LOCALAPPDATA\Docker\log
```

This opens up all the logs in your preferred text editor for you to explore.

------

## [View the Docker daemon logs](https://docs.docker.com/desktop/troubleshoot/#view-the-docker-daemon-logs)

Refer to the [Read the daemon logs](https://docs.docker.com/engine/daemon/logs/) section to learn how to view the Docker Daemon logs.

## [Further resources](https://docs.docker.com/desktop/troubleshoot/#further-resources)

- View specific [troubleshoot topics](https://docs.docker.com/desktop/troubleshoot/topics/).
- Implement [workarounds for common problems](https://docs.docker.com/desktop/troubleshoot/workarounds/)
- View information on [known issues](https://docs.docker.com/desktop/troubleshoot/known-issues/)
