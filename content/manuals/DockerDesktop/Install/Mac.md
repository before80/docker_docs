+++
title = "Mac"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/desktop/install/mac-install/](https://docs.docker.com/desktop/install/mac-install/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Install Docker Desktop on Mac

> **Docker Desktop terms**
>
> Commercial use of Docker Desktop in larger enterprises (more than 250 employees OR more than $10 million USD in annual revenue) requires a [paid subscription](https://www.docker.com/pricing/).

This page contains download URLs, information about system requirements, and instructions on how to install Docker Desktop for Mac.



[Docker Desktop for Mac with Apple silicon](https://desktop.docker.com/mac/main/arm64/Docker.dmg?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-mac-arm64)

[Docker Desktop for Mac with Intel chip](https://desktop.docker.com/mac/main/amd64/Docker.dmg?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-mac-amd64)



*For checksums, see [Release notes](https://docs.docker.com/desktop/release-notes/).*

## [System requirements](https://docs.docker.com/desktop/install/mac-install/#system-requirements)

Mac with Intel chip Mac with Apple silicon

------

- A supported version of macOS.

  > **Important**
  >
  > 
  >
  > Docker supports Docker Desktop on the most recent versions of macOS. That is, the current release of macOS and the previous two releases. As new major versions of macOS are made generally available, Docker stops supporting the oldest version and supports the newest version of macOS (in addition to the previous two releases).

- At least 4 GB of RAM.

------

## [Install and run Docker Desktop on Mac](https://docs.docker.com/desktop/install/mac-install/#install-and-run-docker-desktop-on-mac)

> **Tip**
>
> 
>
> See the [FAQs](https://docs.docker.com/desktop/faqs/general/#how-do-I-run-docker-desktop-without-administrator-privileges) on how to install and run Docker Desktop without needing administrator privileges.

### [Install interactively](https://docs.docker.com/desktop/install/mac-install/#install-interactively)

1. Download the installer using the download buttons at the top of the page, or from the [release notes](https://docs.docker.com/desktop/release-notes/).

2. Double-click `Docker.dmg` to open the installer, then drag the Docker icon to the **Applications** folder. By default, Docker Desktop is installed at `/Applications/Docker.app`.

3. Double-click `Docker.app` in the **Applications** folder to start Docker.

4. The Docker menu displays the Docker Subscription Service Agreement.

   Here’s a summary of the key points:

   - Docker Desktop is free for small businesses (fewer than 250 employees AND less than $10 million in annual revenue), personal use, education, and non-commercial open source projects.
   - Otherwise, it requires a paid subscription for professional use.
   - Paid subscriptions are also required for government entities.
   - Docker Pro, Team, and Business subscriptions include commercial use of Docker Desktop.

5. Select **Accept** to continue.

   Note that Docker Desktop won't run if you do not agree to the terms. You can choose to accept the terms at a later date by opening Docker Desktop.

   For more information, see [Docker Desktop Subscription Service Agreement](https://www.docker.com/legal/docker-subscription-service-agreement). It is recommended that you also read the [FAQs](https://www.docker.com/pricing/faq).

6. From the installation window, select either:

   - **Use recommended settings (Requires password)**. This lets Docker Desktop automatically set the necessary configuration settings.
   - **Use advanced settings**. You can then set the location of the Docker CLI tools either in the system or user directory, enable the default Docker socket, and enable privileged port mapping. See [Settings](https://docs.docker.com/desktop/settings/#advanced), for more information and how to set the location of the Docker CLI tools.

7. Select **Finish**. If you have applied any of the previous configurations that require a password in step 6, enter your password to confirm your choice.

### [Install from the command line](https://docs.docker.com/desktop/install/mac-install/#install-from-the-command-line)

After downloading `Docker.dmg` from either the download buttons at the top of the page or from the [release notes](https://docs.docker.com/desktop/release-notes/), run the following commands in a terminal to install Docker Desktop in the **Applications** folder:



```console
$ sudo hdiutil attach Docker.dmg
$ sudo /Volumes/Docker/Docker.app/Contents/MacOS/install
$ sudo hdiutil detach /Volumes/Docker
```

By default, Docker Desktop is installed at `/Applications/Docker.app`. As macOS typically performs security checks the first time an application is used, the `install` command can take several minutes to run.

The `install` command accepts the following flags:

- `--accept-license`: Accepts the [Docker Subscription Service Agreement](https://www.docker.com/legal/docker-subscription-service-agreement) now, rather than requiring it to be accepted when the application is first run.

- `--allowed-org=<org name>`: Requires the user to sign in and be part of the specified Docker Hub organization when running the application

- `--user=<username>`: Performs the privileged configurations once during installation. This removes the need for the user to grant root privileges on first run. For more information, see [Privileged helper permission requirements](https://docs.docker.com/desktop/install/mac-permission-requirements/#permission-requirements). To find the username, enter `ls /Users` in the CLI.

- ```
  --admin-settings
  ```

  : Automatically creates an

   

  ```
  admin-settings.json
  ```

   

  file which is used by administrators to control certain Docker Desktop settings on client machines within their organization. For more information, see

   

  Settings Management

  .

  - It must be used together with the `--allowed-org=<org name>` flag.
  - For example: `--allowed-org=<org name> --admin-settings="{'configurationFileVersion': 2, 'enhancedContainerIsolation': {'value': true, 'locked': false}}"`

- `--proxy-http-mode=<mode>`: Sets the HTTP Proxy mode. The two modes are `system` (default) or `manual`.

- `--override-proxy-http=<URL>`: Sets the URL of the HTTP proxy that must be used for outgoing HTTP requests. It requires `--proxy-http-mode` to be `manual`.

- `--override-proxy-https=<URL>`: Sets the URL of the HTTP proxy that must be used for outgoing HTTPS requests, requires `--proxy-http-mode` to be `manual`

- `--override-proxy-exclude=<hosts/domains>`: Bypasses proxy settings for the hosts and domains. It's a comma-separated list.

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

## [Where to go next](https://docs.docker.com/desktop/install/mac-install/#where-to-go-next)

- Explore [Docker's core subscriptions](https://www.docker.com/pricing/) to see what Docker can offer you.
- [Get started with Docker](https://docs.docker.com/get-started/introduction/).
- [Explore Docker Desktop](https://docs.docker.com/desktop/use-desktop/) and all its features.
- [Troubleshooting](https://docs.docker.com/desktop/troubleshoot/) describes common problems, workarounds, how to run and submit diagnostics, and submit issues.
- [FAQs](https://docs.docker.com/desktop/faqs/general/) provide answers to frequently asked questions.
- [Release notes](https://docs.docker.com/desktop/release-notes/) lists component updates, new features, and improvements associated with Docker Desktop releases.
- [Back up and restore data](https://docs.docker.com/desktop/backup-and-restore/) provides instructions on backing up and restoring data related to Docker.
