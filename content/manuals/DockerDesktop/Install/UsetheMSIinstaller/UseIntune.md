+++
title = "Use Intune"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/desktop/install/msi/use-intune/](https://docs.docker.com/desktop/install/msi/use-intune/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use Intune

Learn how to deploy Docker Desktop using Intune, Microsoft's cloud-based device management tool.

1. Sign in to your Intune admin center.

2. Add a new app. Select **Apps**, then **Windows**, then **Add**.

3. For the app type, select **Windows app (Win32)**

4. Select the `intunewin` package.

5. Complete any relevant details such as the description, publisher, or app version and then select **Next**.

6. Optional: On the **Program** tab, you can update the **Install command** field to suit your needs. The field is pre-populated with `msiexec /i "DockerDesktop.msi" /qn`. See the [Common installation scenarios]({{< ref "/manuals/DockerDesktop/Install/UsetheMSIinstaller/Installandconfigure" >}}) for examples on the changes you can make.

   > **Tip**
   >
   > 
   >
   > It's recommended you configure the Intune deployment to schedule a reboot of the machine on successful installs.
   >
   > This is because the Docker Desktop installer installs Windows features depending on your engine selection and also updates the membership of the `docker-users` local group.
   >
   > You may also want to set Intune to determine behaviour based on return codes and watch for a return code of `3010`.

7. Complete the rest of the tabs and then review and create the app.

## Additional resources

- [Explore the FAQs]({{< ref "/manuals/DockerDesktop/Install/UsetheMSIinstaller/MSIFAQs" >}}).
