+++
title = "安装与配置"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/install/msi/install-and-configure/](https://docs.docker.com/desktop/install/msi/install-and-configure/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install and configure - 安装与配置

## 交互安装 Install interactively

1. In the [Docker Admin Console](http://admin.docker.com/), navigate to your organization. 在 [Docker 管理控制台](http://admin.docker.com/)中，导航到您的组织。
2. Under **Security and access**, select the **Deploy Docker Desktop** page.在**安全与访问**下，选择**部署 Docker Desktop**页面。
3. Select the **Download MSI installer** button. 点击**下载 MSI 安装程序**按钮。
4. Once downloaded, double-click `Docker Desktop Installer.msi` to run the installer. 下载完成后，双击 `Docker Desktop Installer.msi` 以运行安装程序。
5. Once you've accepted the license agreement, you can choose the install location. By default, Docker Desktop is installed at `C:\Program Files\Docker\Docker`.  接受许可协议后，您可以选择安装位置。默认情况下，Docker Desktop 安装在 `C:\Program Files\Docker\Docker`。
6. Configure the Docker Desktop installation. You can: 配置 Docker Desktop 安装。您可以：
   - Create a desktop shortcut
     - 创建桌面快捷方式
   - Set the Docker Desktop service startup type to automatic
     - 设置 Docker Desktop 服务启动类型为自动
   - Disable Windows Container usage
     - 禁用 Windows 容器的使用
   - Select the engine for Docker Desktop. Either WSL or Hyper-V. If your system only supports one of the two options, you won't be able to select which backend to use.
     - 选择 Docker Desktop 的引擎，可以是 WSL 或 Hyper-V。如果您的系统仅支持其中一个选项，则无法选择使用哪个后端。
7. Follow the instructions on the installation wizard to authorize the installer and proceed with the install. 按照安装向导中的说明授权安装程序并继续安装。
8. When the installation is successful, select **Finish** to complete the installation process. 安装成功后，选择**完成**以完成安装过程。

If your administrator account is different to your user account, you must add the user to the **docker-users** group:

​	如果您的管理员帐户与用户帐户不同，则必须将用户添加到 **docker-users** 组：

1. Run **Computer Management** as an **administrator**. 以**管理员**身份运行**计算机管理**。
2. Navigate to **Local Users and Groups** > **Groups** > **docker-users**. 导航到**本地用户和组** > **组** > **docker-users**。
3. Right-click to add the user to the group. 右键单击以将用户添加到该组。
4. Sign out and sign back in for the changes to take effect. 注销并重新登录以使更改生效。

> **Note**
>
> 
>
> When installing Docker Desktop with the MSI, in-app updates are automatically disabled. This feature ensures your organization maintains the required Docker Desktop version. For Docker Desktop installed with the .exe installer, in-app updates remain supported.
>
> ​	使用 MSI 安装 Docker Desktop 时，将自动禁用应用内更新。此功能可确保您的组织保持所需的 Docker Desktop 版本。对于使用 .exe 安装程序安装的 Docker Desktop，应用内更新仍然受支持。
>
> Docker Desktop notifies you when an update is available. To update Docker Desktop, download the latest installer from the Docker Admin Console. Navigate to the **Deploy Docker Desktop** page > under **Security and access**.
>
> ​	Docker Desktop 会在有更新时通知您。要更新 Docker Desktop，请从 Docker 管理控制台下载最新安装程序。导航到**部署 Docker Desktop**页面 > **安全与访问**下。
>
> To keep up to date with new releases, check the [release notes]({{< ref "/manuals/DockerDesktop/Releasenotes" >}}) page.
>
> ​	要跟踪最新版本，请查看[发行说明]({{< ref "/manuals/DockerDesktop/Releasenotes" >}})页面。

## 从命令行安装 Install from the command line

This section covers command line installations of Docker Desktop using PowerShell. It provides common installation commands that you can run. You can also add additional arguments which are outlined in [configuration options](https://docs.docker.com/desktop/install/msi/install-and-configure/#configuration-options).

​	本节介绍使用 PowerShell 进行 Docker Desktop 命令行安装。提供了一些常见的安装命令。您还可以添加其他参数，详见[配置选项](https://docs.docker.com/desktop/install/msi/install-and-configure/#configuration-options)。

When installing Docker Desktop, you can choose between interactive or non-interactive installations.

​	安装 Docker Desktop 时，可以选择交互式或非交互式安装。

Interactive installations, without specifying `/quiet` or `/qn`, display the user interface and let you select your own properties.

​	交互式安装（未指定 `/quiet` 或 `/qn`）会显示用户界面，并允许您自行选择属性。

When installing via the user interface it's possible to:

​	通过用户界面安装时，可以：

- Choose the destination folder
  - 选择目标文件夹

- Create a desktop shortcut
  - 创建桌面快捷方式

- Configure the Docker Desktop service startup type
  - 配置 Docker Desktop 服务启动类型

- Disable Windows Containers
  - 禁用 Windows 容器

- Choose between the WSL or Hyper-V engine
  - 在 WSL 和 Hyper-V 引擎之间进行选择


Non-interactive installations are silent and any additional configuration must be passed as arguments.

​	非交互式安装是静默的，任何附加配置必须作为参数传递。

### 常见安装命令 Common installation commands

> **Important**
>
> 
>
> Admin rights are required to run any of the following commands.
>
> ​	运行以下任何命令都需要管理员权限。

#### 带详细日志的交互安装 Installing interactively with verbose logging



```powershell
msiexec /i "DockerDesktop.msi" /L*V ".\msi.log"
```

#### 不带详细日志的交互安装 Installing interactively without verbose logging



```powershell
msiexec /i "DockerDesktop.msi"
```

#### 带详细日志的非交互安装 Installing non-interactively with verbose logging



```powershell
msiexec /i "DockerDesktop.msi" /L*V ".\msi.log" /quiet
```

#### 非交互安装并禁止重启 Installing non-interactively and suppressing reboots



```powershell
msiexec /i "DockerDesktop.msi" /L*V ".\msi.log" /quiet /norestart
```

#### 带管理员设置的非交互安装 Installing non-interactively with admin settings



```powershell
msiexec /i "DockerDesktop.msi" /L*V ".\msi.log" /quiet /norestart ADMINSETTINGS="{"configurationFileVersion":2,"enhancedContainerIsolation":{"value":true,"locked":false}}" ALLOWEDORG="docker"
```

#### 带被动显示选项的安装 Installing with the passive display option

You can use the `/passive` display option instead of `/quiet` when you want to perform a non-interactive installation but show a progress dialog.

​	当您希望执行非交互安装但显示进度对话框时，可以使用 `/passive` 显示选项而不是 `/quiet`。

In passive mode the installer doesn't display any prompts or error messages to the user and the installation cannot be cancelled.

​	在被动模式下，安装程序不会向用户显示任何提示或错误消息，安装过程无法取消。

For example:

​	例如：

```powershell
msiexec /i "DockerDesktop.msi" /L*V ".\msi.log" /passive /norestart
```

> **Tip**
>
> 
>
> Some useful tips to remember when creating a value that expects a JSON string as it’s value:
>
> ​	创建一个需要 JSON 字符串作为值的值时，记住一些有用的提示：
>
> - The property expects a JSON formatted string
>   - 属性期望一个 JSON 格式的字符串
> - The string should be wrapped in double quotes
>   - 字符串应该用双引号包裹
> - The string shouldn't contain any whitespace
>   - 字符串不应包含空格
> - Property names are expected to be in double quotes
>   - 属性名应在双引号内

### 常见卸载命令 Common uninstall commands

When uninstalling Docker Desktop, you need to use the same `.msi` file that was originally used to install the application.

​	卸载 Docker Desktop 时，需要使用最初用于安装应用程序的 `.msi` 文件。

If you no longer have the original `.msi` file, you need to use the product code associated with the installation. To find the product code, run:

​	如果不再有原始的 `.msi` 文件，则需要使用与安装相关的产品代码。要查找产品代码，运行：



```powershell
Get-WmiObject Win32_Product | Select-Object IdentifyingNumber, Name | Where-Object {$_.Name -eq "Docker Desktop"}
```

It should return output similar to the following:

​	它将返回类似以下的输出：



```text
IdentifyingNumber                      Name
-----------------                      ----
{10FC87E2-9145-4D7D-B493-2E99E8D8E103} Docker Desktop
```

> **Note**
>
> 
>
> This command can take some time to return, depending on the number of installed applications.
>
> ​	此命令可能需要一些时间返回结果，具体取决于已安装的应用程序数量。

`IdentifyingNumber` is the applications product code and can be used to uninstall Docker Desktop. For example:

​	`IdentifyingNumber` 是应用程序的产品代码，可用于卸载 Docker Desktop。例如：



```powershell
msiexec /x {10FC87E2-9145-4D7D-B493-2E99E8D8E103} /L*V ".\msi.log" /quiet
```

#### 带详细日志的交互卸载 Uninstalling interactively with verbose logging



```powershell
msiexec /x "DockerDesktop.msi" /L*V ".\msi.log"
```

#### 不带详细日志的交互卸载 Uninstalling interactively without verbose logging



```powershell
msiexec /x "DockerDesktop.msi"
```

#### 带详细日志的非交互卸载 Uninstalling non-interactively with verbose logging



```powershell
msiexec /x "DockerDesktop.msi" /L*V ".\msi.log" /quiet
```

#### 不带详细日志的非交互卸载 Uninstalling non-interactively without verbose logging



```powershell
msiexec /x "DockerDesktop.msi" /quiet
```

### 配置选项 Configuration options

> **Important**
>
> 
>
> In addition to the following custom properties, the Docker Desktop MSI installer also supports the standard [Windows Installer command line options](https://learn.microsoft.com/en-us/windows/win32/msi/standard-installer-command-line-options).
>
> ​	除以下自定义属性外，Docker Desktop MSI 安装程序还支持标准的 [Windows 安装程序命令行选项](https://learn.microsoft.com/en-us/windows/win32/msi/standard-installer-command-line-options)。

| Property                           | Description                                                  | Default                 |
| :--------------------------------- | :----------------------------------------------------------- | :---------------------- |
| `ENABLEDESKTOPSHORTCUT`            | 创建桌面快捷方式。 Creates a desktop shortcut.               | 1                       |
| `INSTALLFOLDER`                    | 指定 Docker Desktop 将安装的自定义位置。 Specifies a custom location where Docker Desktop will be installed. | C:\Program Files\Docker |
| `ADMINSETTINGS`                    | 自动创建 `admin-settings.json` 文件，用于在组织内的客户端机器上[控制某些 Docker Desktop 设置]({{< ref "/manuals/Security/Foradmins/HardenedDockerDesktop/SettingsManagement" >}})。必须与 `ALLOWEDORG` 属性一起使用。Automatically creates an `admin-settings.json` file which is used to [control certain Docker Desktop settings]({{< ref "/manuals/Security/Foradmins/HardenedDockerDesktop/SettingsManagement" >}}) on client machines within organizations. It must be used together with the `ALLOWEDORG` property. | None                    |
| `ALLOWEDORG`                       | 运行应用程序时，要求用户登录并属于指定的 Docker Hub 组织。这将在 `HKLM\Software\Policies\Docker\Docker Desktop` 中创建一个名为 `allowedOrgs` 的注册表项。Requires the user to sign in and be part of the specified Docker Hub organization when running the application. This creates a registry key called `allowedOrgs` in `HKLM\Software\Policies\Docker\Docker Desktop`. | None                    |
| `ALWAYSRUNSERVICE`                 | 允许用户切换到 Windows 容器而无需管理员权限Lets users switch to Windows containers without needing admin rights | 0                       |
| `DISABLEWINDOWSCONTAINERS`         | 禁用 Windows 容器集成Disables the Windows containers integration | 0                       |
| `ENGINE`                           | 设置运行容器的 Docker 引擎。可以是 `wsl`、`hyperv` 或 `windows`Sets the Docker Engine that's used to run containers. This can be either `wsl` , `hyperv`, or `windows` | `wsl`                   |
| `PROXYENABLEKERBEROSNTLM`          | 设置为 1 时，启用对 Kerberos 和 NTLM 代理身份验证的支持。适用于 Docker Desktop 4.33 及更高版本When set to 1, enables support for Kerberos and NTLM proxy authentication. Available with Docker Desktop 4.33 and later | 0                       |
| `PROXYHTTPMODE`                    | 设置 HTTP 代理模式。可以是 `system` 或 `manual`Sets the HTTP Proxy mode. This can be either `system` or `manual` | `system`                |
| `OVERRIDEPROXYHTTP`                | 设置用于传出 HTTP 请求的 HTTP 代理的 URL。Sets the URL of the HTTP proxy that must be used for outgoing HTTP requests. | None                    |
| `OVERRIDEPROXYHTTPS`               | 设置用于传出 HTTPS 请求的 HTTPS 代理的 URL。Sets the URL of the HTTP proxy that must be used for outgoing HTTPS requests. | None                    |
| `OVERRIDEPROXYEXCLUDE`             | 为主机和域名绕过代理设置。使用逗号分隔的列表。Bypasses proxy settings for the hosts and domains. Uses a comma-separated list. | None                    |
| `HYPERVDEFAULTDATAROOT`            | 指定 Hyper-V VM 磁盘的默认位置。Specifies the default location for the Hyper-V VM disk. | None                    |
| `WINDOWSCONTAINERSDEFAULTDATAROOT` | 指定 Windows 容器的默认位置。Specifies the default location for Windows containers. | None                    |
| `WSLDEFAULTDATAROOT`               | 指定 WSL 分发磁盘的默认位置。Specifies the default location for the WSL distribution disk. | None                    |
| `DISABLEANALYTICS`                 | 设置为 1 时，将禁用 MSI 的分析数据收集。有关详细信息，请参阅 [分析数据](https://docs.docker.com/desktop/install/msi/install-and-configure/#analytics)。When set to 1, analytics collection will be disabled for the MSI. For more information, see [Analytics](https://docs.docker.com/desktop/install/msi/install-and-configure/#analytics). | 0                       |

Additionally, you can also use `/norestart` or `/forcerestart` to control reboot behaviour.

​	此外，您还可以使用 `/norestart` 或 `/forcerestart` 控制重启行为。

By default, the installer reboots the machine after a successful installation. When ran silently, the reboot is automatic and the user is not prompted.

​	默认情况下，安装程序在成功安装后会重启计算机。静默运行时，重启是自动的，用户不会收到提示。

## 分析数据 Analytics

The MSI installer collects anonymous usage statistics to better understand user behaviour and to improve the user experience by identifying and addressing issues or optimizing popular features.

​	MSI 安装程序会收集匿名使用统计数据，以更好地了解用户行为，并通过识别和解决问题或优化流行功能来改进用户体验。

### 如何选择退出 How to opt-out

{{< tabpane text=true persist=disabled >}}

{{% tab header="From the GUI" %}}

When you install Docker Desktop from the default installer GUI, select the **Disable analytics** checkbox located on the bottom-left corner of the **Welcome** dialog.

​	使用默认安装程序 GUI 安装 Docker Desktop 时，请在**欢迎**对话框的左下角选择**禁用分析数据**复选框。

{{% /tab  %}}

{{% tab header="From the command line" %}}

When you install Docker Desktop from the command line, use the `DISABLEANALYTICS` property.

​	从命令行安装 Docker Desktop 时，使用 `DISABLEANALYTICS` 属性。



```powershell
msiexec /i "win\msi\bin\en-US\DockerDesktop.msi" /L*V ".\msi.log" DISABLEANALYTICS=1
```

{{% /tab  %}}

{{< /tabpane >}}

------

### 持久性Persistence

If you decide to disable analytics for an installation, your choice is persisted in the registry and honoured across future upgrades and uninstalls.

​	如果您决定禁用安装的分析数据，则您的选择将持久保存在注册表中，并在未来的升级和卸载中得到遵守。

However, the key is removed when Docker Desktop is uninstalled and must be configured again via one of the previous methods.

​	但是，在卸载 Docker Desktop 时会删除该键，并且必须通过前述方法之一重新配置。

The registry key is as follows:

​	注册表键如下：

```powershell
SOFTWARE\Docker Inc.\Docker Desktop\DisableMsiAnalytics
```

When analytics is disabled, this key has a value of `1`.

​	当禁用分析数据时，该键的值为 `1`。

## 其他资源 Additional resources

- [Explore the FAQs]({{< ref "/manuals/DockerDesktop/Install/UsetheMSIinstaller/MSIFAQs" >}})
  - [查看常见问题]({{< ref "/manuals/DockerDesktop/Install/UsetheMSIinstaller/MSIFAQs" >}})
