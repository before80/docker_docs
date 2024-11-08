+++
title = "卷"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/use-desktop/volumes/](https://docs.docker.com/desktop/use-desktop/volumes/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Explore the Volumes view in Docker Desktop - 探索 Docker Desktop 中的卷视图

The **Volumes** view in Docker Dashboard lets you create, delete, and perform other actions on your [volumes]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}). You can also see which volumes are being used as well as inspect the files and folders in your volumes.

​	Docker 仪表板中的 **卷** 视图允许您创建、删除并执行其他卷操作。您还可以查看哪些卷正在使用，并检查卷中的文件和文件夹。

## 查看您的卷 View your volumes

You can view the following information about your volumes:

​	您可以查看卷的以下信息：

- Name: The name of the volume.
  - 名称：卷的名称。

- Status: Whether the volume is in-use by a container or not.
  - 状态：卷是否被容器使用。

- Created: How long ago the volume was created.
  - 创建时间：卷的创建时间。

- Size: The size of the volume.
  - 大小：卷的大小。

- Scheduled exports: Whether a scheduled export is active or not.
  - 计划导出：计划导出是否激活。


By default, the **Volumes** view displays a list of all the volumes.

​	默认情况下，**卷** 视图会显示所有卷的列表。

You can filter and sort volumes as well as modify which columns are displayed by doing the following:

​	您可以过滤和排序卷，并通过以下方式修改显示的列：

- Filter volumes by name: Use the **Search** field.
  - 按名称过滤卷：使用 **搜索** 字段。

- Filter volumes by status: To the right of the search bar, filter volumes by **In use** or **Unused**.
  - 按状态过滤卷：在搜索栏右侧，按 **使用中** 或 **未使用** 筛选卷。

- Sort volumes: Select a column name to sort the volumes.
  - 排序卷：选择列名称来排序卷。

- Customize columns: To the right of the search bar, choose what volume information to display.
  - 自定义列：在搜索栏右侧选择要显示的卷信息。


## 创建卷 Create a volume

You use the following steps to create an empty volume. Alternatively, if you [start a container with a volume](https://docs.docker.com/engine/storage/volumes/#start-a-container-with-a-volume) that doesn't yet exist, Docker creates the volume for you.

​	使用以下步骤创建空卷。或者，如果您[启动一个带卷的容器](https://docs.docker.com/engine/storage/volumes/#start-a-container-with-a-volume)而该卷尚不存在，Docker 会为您创建该卷。

To create a volume:

​	要创建卷：

1. In the **Volumes** view, select the **Create** button. 在 **卷** 视图中，选择 **创建** 按钮。
2. In the **New Volume** modal, specify a volume name, and then select **Create**. 在 **新建卷** 模态中，指定卷名称，然后选择 **创建**。

To use the volume with a container, see [Use volumes](https://docs.docker.com/engine/storage/volumes/#start-a-container-with-a-volume).

​	要将卷用于容器，请参阅[使用卷](https://docs.docker.com/engine/storage/volumes/#start-a-container-with-a-volume)。

## 检查卷 Inspect a volume

To explore the details of a specific volume, select a volume from the list. This opens the detailed view.

​	要查看特定卷的详细信息，请从列表中选择一个卷。这样将打开详细视图。

The **Container in-use** tab displays the name of the container using the volume, the image name, the port number used by the container, and the target. A target is a path inside a container that gives access to the files in the volume.

​	**使用中的容器** 选项卡显示使用该卷的容器名称、镜像名称、容器使用的端口号以及目标。目标是容器内用于访问卷中文件的路径。

The **Stored data** tab displays the files and folders in the volume and the file size. To save a file or a folder, right-click on the file or folder to display the options menu, select **Save as...**, and then specify a location to download the file.

​	**存储的数据** 选项卡显示卷中的文件和文件夹以及文件大小。要保存文件或文件夹，右键点击文件或文件夹以显示选项菜单，选择 **另存为…**，然后指定下载位置。

To delete a file or a folder from the volume, right-click on the file or folder to display the options menu, select **Delete**, and then select **Delete** again to confirm.

​	要从卷中删除文件或文件夹，右键点击文件或文件夹以显示选项菜单，选择 **删除**，然后再次选择 **删除** 以确认。

The **Exports** tab lets you [export the volume](https://docs.docker.com/desktop/use-desktop/volumes/#export-a-volume).

​	**导出** 选项卡允许您[导出卷](https://docs.docker.com/desktop/use-desktop/volumes/#export-a-volume)。

## 克隆卷 Clone a volume

Cloning a volume creates a new volume with a copy of all of the data from the cloned volume. When cloning a volume used by one or more running containers, the containers are temporarily stopped while Docker clones the data, and then restarted when the cloning process is completed.

​	克隆卷会创建一个新卷，并将克隆卷中的所有数据复制到新卷中。当克隆一个由一个或多个正在运行的容器使用的卷时，容器会暂时停止，Docker 完成数据克隆后再重新启动。

To clone a volume:

​	要克隆卷：

1. Sign in to Docker Desktop. You must be signed in to clone a volume. 登录 Docker Desktop。必须登录才能克隆卷。
2. In the **Volumes** view, select the **Clone** icon in the **Actions** column for the volume you want to clone. 在 **卷** 视图中，选择要克隆的卷的 **操作** 列中的 **克隆** 图标。
3. In the **Clone a volume** modal, specify a **Volume name**, and then select **Clone**. 在 **克隆卷** 模态中，指定 **卷名称**，然后选择 **克隆**。

## 删除一个或多个卷 Delete one or more volumes

Deleting a volume deletes the volume and all its data. When a container is using a volume, you can't delete the volume, even if the container is stopped. You must first stop and remove any containers using the volume before you can delete the volume.

​	删除卷会删除卷及其所有数据。当容器正在使用卷时，即使容器已停止，也无法删除该卷。必须先停止并删除使用该卷的所有容器，然后才能删除卷。

To delete a volume:

​	要删除一个卷：

1. In the **Volumes** view, select **Delete** icon in the **Actions** column for the volume you want to delete. 在 **卷** 视图中，选择要删除的卷的 **操作** 列中的 **删除** 图标。
2. In the **Delete volume?** modal, select **Delete forever**. 在 **删除卷？** 模态中，选择 **永久删除**。

To delete multiple volumes:

​	要删除多个卷：

1. In the **Volumes** view, select the checkbox next to all the volumes you want to delete. 在 **卷** 视图中，勾选所有要删除的卷旁边的复选框。
2. Select **Delete**. 选择 **删除**。
3. In the **Delete volumes?** modal, select **Delete forever**. 在 **删除卷？** 模态中，选择 **永久删除**。

## 清空卷 Empty a volume

Emptying a volume deletes all a volume's data, but doesn't delete the volume. When emptying a volume used by one or more running containers, the containers are temporarily stopped while Docker empties the data, and then restarted when the emptying process is completed.

​	清空卷会删除卷的所有数据，但不会删除卷。当清空一个由一个或多个正在运行的容器使用的卷时，容器会暂时停止，Docker 完成数据清空后再重新启动。

To empty a volume:

​	要清空卷：

1. Sign in to Docker Desktop. You must be signed in to empty a volume. 登录 Docker Desktop。必须登录才能清空卷。
2. In the **Volumes** view, select the volume you want to empty. 在 **卷** 视图中，选择要清空的卷。
3. Next to **Import**, select the **More volume actions** icon, and then select **Empty volume**. 在 **导入** 旁边，选择 **更多卷操作** 图标，然后选择 **清空卷**。
4. In the **Empty a volume?** modal, select **Empty**. 在 **清空卷？** 模态中，选择 **清空**。

## 导出卷 Export a volume

**Beta feature**

The export volume feature is currently in [Beta](https://docs.docker.com/release-lifecycle/#beta).

​	导出卷功能目前处于 [Beta](https://docs.docker.com/release-lifecycle/#beta) 阶段。

You can export the content of a volume to a local file, a local image, an to an image in Docker Hub, or to a supported cloud provider. When exporting content from a volume used by one or more running containers, the containers are temporarily stopped while Docker exports the content, and then restarted when the export process is completed.

​	您可以将卷的内容导出到本地文件、本地镜像、Docker Hub 中的镜像或支持的云提供商。导出一个由一个或多个正在运行的容器使用的卷时，容器会暂时停止，Docker 完成内容导出后再重新启动。

You can either [export a volume now](https://docs.docker.com/desktop/use-desktop/volumes/#export-a-volume-now) or [schedule a recurring export](https://docs.docker.com/desktop/use-desktop/volumes/#schedule-a-volume-export).

​	您可以选择[立即导出卷](https://docs.docker.com/desktop/use-desktop/volumes/#export-a-volume-now)或[安排定期导出](https://docs.docker.com/desktop/use-desktop/volumes/#schedule-a-volume-export)。

### 立即导出卷 Export a volume now

1. Sign in to Docker Desktop. You must be signed in to export a volume. 登录 Docker Desktop。必须登录才能导出卷。

2. In the **Volumes** view, select the volume you want to export. 在 **卷** 视图中，选择要导出的卷。

3. Select the **Exports** tab. 选择 **导出** 选项卡。

4. Select **Quick export**. 选择 **快速导出**。

5. Select whether to export the volume to **Local or Hub storage** or **External cloud storage**, then specify the following additional details depending on your selection. 选择是将卷导出到 **本地或 Hub 存储** 还是 **外部云存储**，然后根据您的选择指定以下详细信息。

   {{< tabpane text=true persist=disabled >}}

   {{% tab header="Local or Hub storage" %}}

   - **Local file**: Specify a file name and select a folder.
     - **本地文件**：指定文件名并选择文件夹。
   - **Local image**: Select a local image to export the content to. Any existing data in the image will be replaced by the exported content.
     - **本地镜像**：选择一个本地镜像来导出内容，任何现有数据将被导出的内容替换。
   - **New image**: Specify a name for the new image.
     - **新镜像**：为新镜像指定一个名称。
   - **Registry**: Specify a Docker Hub repository. Note that Docker Hub repositories can be publicly accessible which means your data can be publicly accessible. For more details, see [Change a repository from public to private](https://docs.docker.com/docker-hub/repos/#change-a-repository-from-public-to-private).
     - **注册表**：指定一个 Docker Hub 仓库。请注意，Docker Hub 仓库可能是公开的，这意味着您的数据可能会公开访问。有关更多详细信息，请参阅[将仓库从公共更改为私有](https://docs.docker.com/docker-hub/repos/#change-a-repository-from-public-to-private)。

   {{% /tab  %}}

   {{% tab header=" External cloud storage" %}}

   You must have a [Docker Business subscription]({{< ref "/manuals/Subscription/DockerCore/Subscriptionsandfeatures" >}}) to export to an external cloud provider.

   ​	您必须拥有 [Docker Business 订阅]({{< ref "/manuals/Subscription/DockerCore/Subscriptionsandfeatures" >}}) 才能导出到外部云提供商。

   Select your cloud provider and then specify the URL to upload to the storage. Refer to the following documentation for your cloud provider to learn how to obtain a URL.

   ​	选择您的云提供商并指定用于上传到存储的 URL。请参阅以下文档，了解如何获取云提供商的 URL。

   - Amazon Web Services: [Create a presigned URL for Amazon S3 using an AWS SDK](https://docs.aws.amazon.com/AmazonS3/latest/userguide/example_s3_Scenario_PresignedUrl_section.html)
     - Amazon Web Services：[使用 AWS SDK 为 Amazon S3 创建预签名 URL](https://docs.aws.amazon.com/AmazonS3/latest/userguide/example_s3_Scenario_PresignedUrl_section.html)

   - Microsoft Azure: [Generate a SAS token and URL](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/api/connection-strings/generate-sas-token)
     - Microsoft Azure：[生成 SAS 令牌和 URL](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/api/connection-strings/generate-sas-token)
   - Google Cloud: [Create a signed URL to upload an object](https://cloud.google.com/storage/docs/access-control/signing-urls-with-helpers#upload-object)
     - Google Cloud：[创建用于上传对象的签名 URL](https://cloud.google.com/storage/docs/access-control/signing-urls-with-helpers#upload-object)

   {{% /tab  %}}

   {{< /tabpane >}}

   ------

6. Select **Save**.

### 安排卷导出 Schedule a volume export

1. Sign in to Docker Desktop. You must be signed in and have a paid [Docker subscription]({{< ref "/manuals/Subscription/DockerCore/Subscriptionsandfeatures" >}}) to schedule a volume export. 登录 Docker Desktop。您必须登录并拥有付费的 [Docker 订阅]({{< ref "/manuals/Subscription/DockerCore/Subscriptionsandfeatures" >}}) 才能安排卷导出。

2. In the **Volumes** view, select the volume you want to export. 在 **卷** 视图中，选择要导出的卷。

3. Select the **Exports** tab. 选择 **导出** 选项卡。

4. Select **Schedule export**. 选择 **安排导出**。

5. In **Recurrence**, select how often the export occurs, and then specify the following additional details based on your selection. 在 **重复** 中，选择导出发生的频率，然后根据您的选择指定以下详细信息。

   - **Daily**: Specify the time that the backup occurs each day. **每日**：指定备份每天发生的时间。
   - **Weekly**: Specify one or more days, and the time that the backup occurs each week. **每周**：指定一周中的一个或多个天数，以及备份每周发生的时间。
   - **Monthly**: Specify which day of the month and the time that the backup occurs each month. **每月**：指定每月的某一天和备份发生的时间。

6. Select whether to export the volume to **Local or Hub storage** or **External cloud storage**, then specify the following additional details depending on your selection. 选择是将卷导出到 **本地或 Hub 存储** 还是 **外部云存储**，然后根据您的选择指定以下详细信息。

   {{< tabpane text=true persist=disabled >}}

   {{% tab header="Local or Hub storage" %}}

   - **Local file**: Specify a file name and select a folder. **本地文件**：指定文件名并选择文件夹。
   - **Local image**: Select a local image to export the content to. Any existing data in the image will be replaced by the exported content. **本地镜像**：选择一个本地镜像来导出内容，任何现有数据将被导出的内容替换。
   - **New image**: Specify a name for the new image. **新镜像**：为新镜像指定一个名称。
   - **Registry**: Specify a Docker Hub repository. Note that Docker Hub repositories can be publicly accessible which means your data can be publicly accessible. For more details, see [Change a repository from public to private](https://docs.docker.com/docker-hub/repos/#change-a-repository-from-public-to-private). **注册表**：指定一个 Docker Hub 仓库。请注意，Docker Hub 仓库可能是公开的，这意味着您的数据可能会公开访问。有关更多详细信息，请参阅[将仓库从公共更改为私有](https://docs.docker.com/docker-hub/repos/#change-a-repository-from-public-to-private)。

   {{% /tab  %}}

   {{% tab header=" External cloud storage" %}}

   You must have a [Docker Business subscription]({{< ref "/manuals/Subscription/DockerCore/Subscriptionsandfeatures" >}}) to export to an external cloud provider.

   ​	您必须拥有 [Docker Business 订阅]({{< ref "/manuals/Subscription/DockerCore/Subscriptionsandfeatures" >}}) 才能导出到外部云提供商。

   Select your cloud provider and then specify the URL to upload to the storage. Refer to the following documentation for your cloud provider to learn how to obtain a URL.

   ​	选择您的云提供商并指定用于上传到存储的 URL。请参阅以下文档，了解如何获取云提供商的 URL。

   - Amazon Web Services: [Create a presigned URL for Amazon S3 using an AWS SDK](https://docs.aws.amazon.com/AmazonS3/latest/userguide/example_s3_Scenario_PresignedUrl_section.html)
     - Amazon Web Services：[使用 AWS SDK 为 Amazon S3 创建预签名 URL](https://docs.aws.amazon.com/AmazonS3/latest/userguide/example_s3_Scenario_PresignedUrl_section.html)

   - Microsoft Azure: [Generate a SAS token and URL](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/api/connection-strings/generate-sas-token)
     - Microsoft Azure：[生成 SAS 令牌和 URL](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/api/connection-strings/generate-sas-token)
   - Google Cloud: [Create a signed URL to upload an object](https://cloud.google.com/storage/docs/access-control/signing-urls-with-helpers#upload-object)
     - Google Cloud：[创建用于上传对象的签名 URL](https://cloud.google.com/storage/docs/access-control/signing-urls-with-helpers#upload-object)

   {{% /tab  %}}

   {{< /tabpane >}}

   ------

7. Select **Save**.

## 导入卷 Import a volume

You can import a local file, a local image, or an image from Docker Hub. Any existing data in the volume is replaced by the imported content. When importing content to a volume used by one or more running containers, the containers are temporarily stopped while Docker imports the content, and then restarted when the import process is completed.

​	您可以导入本地文件、本地镜像或来自 Docker Hub 的镜像。卷中的任何现有数据将被导入的内容替换。导入内容到一个由一个或多个正在运行的容器使用的卷时，容器会暂时停止，Docker 完成内容导入后再重新启动。

To import a volume:

​	要导入卷：

1. Sign in to Docker Desktop. You must be signed in to import a volume. 登录 Docker Desktop。必须登录才能导入卷。
2. Optionally, [create](https://docs.docker.com/desktop/use-desktop/volumes/#create-a-volume) a new volume to import the content into. 可选地，先[创建](https://docs.docker.com/desktop/use-desktop/volumes/#create-a-volume)一个新卷以导入内容。
3. Select the volume you want to import content in to. 选择要导入内容的卷。
4. Select **Import**. 选择 **导入**。
5. Select where the content is coming from and then specify the following additional details depending on your selection: 选择内容来源，然后根据您的选择指定以下详细信息：
   - **Local file**: Select the file that contains the content. **本地文件**：选择包含内容的文件。
   - **Local image**: Select the local image that contains the content. **本地镜像**：选择包含内容的本地镜像。
   - **Registry**: Specify the image from Docker Hub that contains the content. **注册表**：指定包含内容的 Docker Hub 镜像。
6. Select **Import**. 选择 **导入**。

## 其他资源 Additional resources

- [Persisting container data]({{< ref "/get-started/Dockerconcepts/Runningcontainers/Persistingcontainerdata" >}})
  - [持久化容器数据]({{< ref "/get-started/Dockerconcepts/Runningcontainers/Persistingcontainerdata" >}})
- [Use volumes]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})
  - [使用卷]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})
