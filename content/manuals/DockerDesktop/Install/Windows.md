+++
title = "Windows"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/desktop/install/windows-install/](https://docs.docker.com/desktop/install/windows-install/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install Docker Desktop on Windows

> **Docker Desktop terms**
>
> Commercial use of Docker Desktop in larger enterprises (more than 250 employees OR more than $10 million USD in annual revenue) requires a [paid subscription](https://www.docker.com/pricing/).

This page contains the download URL, information about system requirements, and instructions on how to install Docker Desktop for Windows.



[Docker Desktop for Windows - x86_64](https://desktop.docker.com/win/main/amd64/Docker Desktop Installer.exe?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-win-amd64)

[Docker Desktop for Windows - Arm (Beta)](https://desktop.docker.com/win/main/arm64/Docker Desktop Installer.exe?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-win-arm64)



*For checksums, see [Release notes](https://docs.docker.com/desktop/release-notes/)*

## [System requirements](https://docs.docker.com/desktop/install/windows-install/#system-requirements)

> **Tip**
>
> 
>
> **Should I use Hyper-V or WSL?**
>
> Docker Desktop's functionality remains consistent on both WSL and Hyper-V, without a preference for either architecture. Hyper-V and WSL have their own advantages and disadvantages, depending on your specific set up and your planned use case.

WSL 2 backend, x86_64 Hyper-V backend, x86_64 WSL 2 backend, Arm (Beta)

------

- WSL version 1.1.3.0 or later.
- Windows 11 64-bit: Home or Pro version 21H2 or higher, or Enterprise or Education version 21H2 or higher.
- Windows 10 64-bit:
  - We recommend Home or Pro 22H2 (build 19045) or higher, or Enterprise or Education 22H2 (build 19045) or higher.
  - Minimum required is Home or Pro 21H2 (build 19044) or higher, or Enterprise or Education 21H2 (build 19044) or higher.
- Turn on the WSL 2 feature on Windows. For detailed instructions, refer to the [Microsoft documentation](https://docs.microsoft.com/en-us/windows/wsl/install-win10).
- The following hardware prerequisites are required to successfully run WSL 2 on Windows 10 or Windows 11:
  - 64-bit processor with [Second Level Address Translation (SLAT)](https://en.wikipedia.org/wiki/Second_Level_Address_Translation)
  - 4GB system RAM
  - Enable hardware virtualization in BIOS. For more information, see [Virtualization](https://docs.docker.com/desktop/troubleshoot/topics/#virtualization).

For more information on setting up WSL 2 with Docker Desktop, see [WSL](https://docs.docker.com/desktop/wsl/).

> **Note**
>
> 
>
> Docker only supports Docker Desktop on Windows for those versions of Windows that are still within [Microsoft’s servicing timeline](https://support.microsoft.com/en-us/help/13853/windows-lifecycle-fact-sheet). Docker Desktop is not supported on server versions of Windows, such as Windows Server 2019 or Windows Server 2022. For more information on how to run containers on Windows Server, see [Microsoft's official documentation](https://learn.microsoft.com/virtualization/windowscontainers/quick-start/set-up-environment).

> **Important**
>
> 
>
> To run Windows containers, you need Windows 10 or Windows 11 Professional or Enterprise edition. Windows Home or Education editions only allow you to run Linux containers.

------

Containers and images created with Docker Desktop are shared between all user accounts on machines where it is installed. This is because all Windows accounts use the same VM to build and run containers. Note that it is not possible to share containers and images between user accounts when using the Docker Desktop WSL 2 backend.

Running Docker Desktop inside a VMware ESXi or Azure VM is supported for Docker Business customers. It requires enabling nested virtualization on the hypervisor first. For more information, see [Running Docker Desktop in a VM or VDI environment](https://docs.docker.com/desktop/vm-vdi/).

## [Install Docker Desktop on Windows](https://docs.docker.com/desktop/install/windows-install/#install-docker-desktop-on-windows)

> **Tip**
>
> 
>
> See the [FAQs](https://docs.docker.com/desktop/faqs/general/#how-do-I-run-docker-desktop-without-administrator-privileges) on how to install and run Docker Desktop without needing administrator privileges.

### [Install interactively](https://docs.docker.com/desktop/install/windows-install/#install-interactively)

1. Download the installer using the download button at the top of the page, or from the [release notes](https://docs.docker.com/desktop/release-notes/).

2. Double-click `Docker Desktop Installer.exe` to run the installer. By default, Docker Desktop is installed at `C:\Program Files\Docker\Docker`.

3. When prompted, ensure the **Use WSL 2 instead of Hyper-V** option on the Configuration page is selected or not depending on your choice of backend.

   If your system only supports one of the two options, you won't be able to select which backend to use.

4. Follow the instructions on the installation wizard to authorize the installer and proceed with the install.

5. When the installation is successful, select **Close** to complete the installation process.

6. [Start Docker Desktop](https://docs.docker.com/desktop/install/windows-install/#start-docker-desktop).

If your administrator account is different to your user account, you must add the user to the **docker-users** group:

1. Run **Computer Management** as an **administrator**.
2. Navigate to **Local Users and Groups** > **Groups** > **docker-users**.
3. Right-click to add the user to the group.
4. Sign out and sign back in for the changes to take effect.

### [Install from the command line](https://docs.docker.com/desktop/install/windows-install/#install-from-the-command-line)

After downloading `Docker Desktop Installer.exe`, run the following command in a terminal to install Docker Desktop:



```console
$ "Docker Desktop Installer.exe" install
```

If you’re using PowerShell you should run it as:



```powershell
Start-Process 'Docker Desktop Installer.exe' -Wait install
```

If using the Windows Command Prompt:



```sh
start /w "" "Docker Desktop Installer.exe" install
```

By default, Docker Desktop is installed at `C:\Program Files\Docker\Docker`.

The `install` command accepts the following flags:

- `--quiet`: Suppresses information output when running the installer
- `--accept-license`: Accepts the [Docker Subscription Service Agreement](https://www.docker.com/legal/docker-subscription-service-agreement) now, rather than requiring it to be accepted when the application is first run
- `--no-windows-containers`: Disables the Windows containers integration
- `--allowed-org=<org name>`: Requires the user to sign in and be part of the specified Docker Hub organization when running the application
- `--backend=<backend name>`: Selects the default backend to use for Docker Desktop, `hyper-v`, `windows` or `wsl-2` (default)
- `--installation-dir=<path>`: Changes the default installation location (`C:\Program Files\Docker\Docker`)
- `--admin-settings`: Automatically creates an `admin-settings.json` file which is used by admins to control certain Docker Desktop settings on client machines within their organization. For more information, see [Settings Management](https://docs.docker.com/security/for-admins/hardened-desktop/settings-management/).
  - It must be used together with the `--allowed-org=<org name>` flag.
  - For example:`--allowed-org=<org name> --admin-settings="{'configurationFileVersion': 2, 'enhancedContainerIsolation': {'value': true, 'locked': false}}"`
- `--proxy-http-mode=<mode>`: Sets the HTTP Proxy mode, `system` (default) or `manual`
- `--override-proxy-http=<URL>`: Sets the URL of the HTTP proxy that must be used for outgoing HTTP requests, requires `--proxy-http-mode` to be `manual`
- `--override-proxy-https=<URL>`: Sets the URL of the HTTP proxy that must be used for outgoing HTTPS requests, requires `--proxy-http-mode` to be `manual`
- `--override-proxy-exclude=<hosts/domains>`: Bypasses proxy settings for the hosts and domains. Uses a comma-separated list.
- `--proxy-enable-kerberosntlm`: Enables Kerberos and NTLM proxy authentication. If you are enabling this, ensure your proxy server is properly configured for Kerberos/NTLM authentication. Available with Docker Desktop 4.32 and later.
- `--hyper-v-default-data-root=<path>`: Specifies the default location for the Hyper-V VM disk.
- `--windows-containers-default-data-root=<path>`: Specifies the default location for the Windows containers.
- `--wsl-default-data-root=<path>`: Specifies the default location for the WSL distribution disk.
- `--always-run-service`: After installation completes, starts `com.docker.service` and sets the service startup type to Automatic. This circumvents the need for administrator privileges, which are otherwise necessary to start `com.docker.service`. `com.docker.service` is required by Windows containers and Hyper-V backend.

> **Note**
>
> 
>
> If you're using PowerShell, you need to use the `ArgumentList` parameter before any flags. For example:
>
> 
>
> ```powershell
> Start-Process 'Docker Desktop Installer.exe' -Wait -ArgumentList 'install', '--accept-license'
> ```

If your admin account is different to your user account, you must add the user to the **docker-users** group:



```console
$ net localgroup docker-users <user> /add
```

## [Start Docker Desktop](https://docs.docker.com/desktop/install/windows-install/#start-docker-desktop)

Docker Desktop does not start automatically after installation. To start Docker Desktop:

1. Search for Docker, and select **Docker Desktop** in the search results.

2. The Docker menu ( ![whale menu](Windows_img/whale-x.svg+xml) ) displays the Docker Subscription Service Agreement.

   Here’s a summary of the key points:

   - Docker Desktop is free for small businesses (fewer than 250 employees AND less than $10 million in annual revenue), personal use, education, and non-commercial open source projects.
   - Otherwise, it requires a paid subscription for professional use.
   - Paid subscriptions are also required for government entities.
   - The Docker Pro, Team, and Business subscriptions include commercial use of Docker Desktop.

3. Select **Accept** to continue. Docker Desktop starts after you accept the terms.

   Note that Docker Desktop won't run if you do not agree to the terms. You can choose to accept the terms at a later date by opening Docker Desktop.

   For more information, see [Docker Desktop Subscription Service Agreement](https://www.docker.com/legal/docker-subscription-service-agreement/). It is recommended that you read the [FAQs](https://www.docker.com/pricing/faq).

> **Tip**
>
> 
>
> As an IT administrator, you can use endpoint management (MDM) software to identify the number of Docker Desktop instances and their versions within your environment. This can provide accurate license reporting, help ensure your machines use the latest version of Docker Desktop, and enable you to [enforce sign-in](https://docs.docker.com/security/for-admins/enforce-sign-in/).
>
> - [Intune](https://learn.microsoft.com/en-us/mem/intune/apps/app-discovered-apps)
> - [Jamf](https://docs.jamf.com/10.25.0/jamf-pro/administrator-guide/Application_Usage.html)
> - [Kandji](https://support.kandji.io/support/solutions/articles/72000559793-view-a-device-application-list)
> - [Kolide](https://www.kolide.com/features/device-inventory/properties/mac-apps)
> - [Workspace One](https://blogs.vmware.com/euc/2022/11/how-to-use-workspace-one-intelligence-to-manage-app-licenses-and-reduce-costs.html)

## [Where to go next](https://docs.docker.com/desktop/install/windows-install/#where-to-go-next)

- Explore [Docker's core subscriptions](https://www.docker.com/pricing/) to see what Docker can offer you.
- [Get started with Docker](https://docs.docker.com/get-started/introduction/).
- [Explore Docker Desktop](https://docs.docker.com/desktop/use-desktop/) and all its features.
- [Troubleshooting](https://docs.docker.com/desktop/troubleshoot/) describes common problems, workarounds, and how to get support.
- [FAQs](https://docs.docker.com/desktop/faqs/general/) provide answers to frequently asked questions.
- [Release notes](https://docs.docker.com/desktop/release-notes/) lists component updates, new features, and improvements associated with Docker Desktop releases.
- [Back up and restore data](https://docs.docker.com/desktop/backup-and-restore/) provides instructions on backing up and restoring data related to Docker.
