+++
title = "使用 Intune"
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

# Use Intune - 使用 Intune

Learn how to deploy Docker Desktop using Intune, Microsoft's cloud-based device management tool.

​	了解如何使用 Microsoft 的云设备管理工具 Intune 部署 Docker Desktop。

1. Sign in to your Intune admin center. 登录 Intune 管理中心。

2. Add a new app. Select **Apps**, then **Windows**, then **Add**. 添加一个新应用。选择**应用**，然后选择**Windows**，再选择**添加**。

3. For the app type, select **Windows app (Win32)** 对于应用类型，选择**Windows 应用 (Win32)**。

4. Select the `intunewin` package. 选择 `intunewin` 包。

5. Complete any relevant details such as the description, publisher, or app version and then select **Next**.完成描述、发布者或应用版本等相关详细信息，然后选择**下一步**。

6. Optional: On the **Program** tab, you can update the **Install command** field to suit your needs. The field is pre-populated with `msiexec /i "DockerDesktop.msi" /qn`. See the [Common installation scenarios]({{< ref "/manuals/DockerDesktop/Install/UsetheMSIinstaller/Installandconfigure" >}}) for examples on the changes you can make.

   1. 可选：在**程序**选项卡上，您可以根据需要更新**安装命令**字段。该字段预填为 `msiexec /i "DockerDesktop.msi" /qn`。有关可做的更改示例，请参阅[常见安装场景]({{< ref "/manuals/DockerDesktop/Install/UsetheMSIinstaller/Installandconfigure" >}})。

      > **Tip**
      >
      > 
      >
      > It's recommended you configure the Intune deployment to schedule a reboot of the machine on successful installs.
      >
      > 建议将 Intune 部署配置为在成功安装时安排机器重启。
      >
      > This is because the Docker Desktop installer installs Windows features depending on your engine selection and also updates the membership of the `docker-users` local group.
      >
      > ​	这是因为 Docker Desktop 安装程序会根据您的引擎选择安装 Windows 功能，还会更新 `docker-users` 本地组的成员。
      >
      > You may also want to set Intune to determine behaviour based on return codes and watch for a return code of `3010`.
      >
      > ​	您可能还希望设置 Intune 以根据返回代码确定行为，并关注返回代码 `3010`。

7. Complete the rest of the tabs and then review and create the app. 完成其余选项卡，然后查看并创建应用。

## 其他资源 Additional resources

- [Explore the FAQs]({{< ref "/manuals/DockerDesktop/Install/UsetheMSIinstaller/MSIFAQs" >}}).
  - [查看常见问题]({{< ref "/manuals/DockerDesktop/Install/UsetheMSIinstaller/MSIFAQs" >}})。
