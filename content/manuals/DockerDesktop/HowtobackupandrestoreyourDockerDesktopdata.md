+++
title = "如何备份和恢复 Docker Desktop 数据"
date = 2024-10-23T14:54:40+08:00
weight = 120
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/backup-and-restore/](https://docs.docker.com/desktop/backup-and-restore/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# How to back up and restore your Docker Desktop data - 如何备份和恢复 Docker Desktop 数据

Use the following procedure to save and restore your images and container data. This is useful if you want to reset your VM disk or to move your Docker environment to a new computer, for example.

​	使用以下步骤保存和恢复您的镜像和容器数据。例如，这在您想重置虚拟机磁盘或将 Docker 环境迁移到新计算机时非常有用。

> Should I back up my containers? 我是否需要备份容器？
>
> If you use volumes or bind-mounts to store your container data, backing up your containers may not be needed, but make sure to remember the options that were used when creating the container or use a [Docker Compose file]({{< ref "/reference/Composefilereference" >}}) if you want to re-create your containers with the same configuration after re-installation.
>
> ​	如果您使用卷或绑定挂载存储容器数据，可能不需要备份容器，但请记住创建容器时使用的选项，或者使用 [Docker Compose 文件]({{< ref "/reference/Composefilereference" >}}) 重新创建容器以保持相同的配置。

## 保存您的数据 Save your data

1. Commit your containers to an image with [`docker container commit`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainercommit" >}}). 使用 [`docker container commit`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockercontainercommit" >}}) 提交您的容器到一个镜像。

   Committing a container stores the container filesystem changes and some of the container's configuration, for example labels and environment-variables, as a local image. Be aware that environment variables may contain sensitive information such as passwords or proxy-authentication, so care should be taken when pushing the resulting image to a registry.

   ​	提交容器会将容器文件系统的更改及一些容器的配置（例如标签和环境变量）存储为本地镜像。请注意，环境变量可能包含敏感信息，如密码或代理认证，因此将镜像推送到注册表时需谨慎。

   Also note that filesystem changes in volume that are attached to the container are not included in the image, and must be backed up separately.

   ​	还需注意，连接到容器的卷中的文件系统更改不包含在镜像中，必须单独备份。

   If you used a [named volume](https://docs.docker.com/engine/storage/#more-details-about-mount-types) to store container data, such as databases, refer to the [back up, restore, or migrate data volumes](https://docs.docker.com/engine/storage/volumes/#back-up-restore-or-migrate-data-volumes) page in the storage section.

   ​	如果您使用 [命名卷](https://docs.docker.com/engine/storage/#more-details-about-mount-types) 存储容器数据（例如数据库），请参阅存储部分中的 [备份、恢复或迁移数据卷](https://docs.docker.com/engine/storage/volumes/#back-up-restore-or-migrate-data-volumes) 页面。

2. Use [`docker push`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerpush" >}}) to push any images you have built locally and want to keep to the [Docker Hub registry]({{< ref "/manuals/DockerHub" >}}). 使用 [`docker push`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerpush" >}}) 将您在本地构建并想要保存的镜像推送到 [Docker Hub 注册表]({{< ref "/manuals/DockerHub" >}})。

   Make sure to configure the [repository's visibility as "private"]({{< ref "/manuals/DockerHub/Managerepositories" >}}) for images that should not be publicly accessible.

   ​	确保将不应公开访问的镜像配置为 [“私有”可见性]({{< ref "/manuals/DockerHub/Managerepositories" >}})。
   
   Alternatively, use [`docker image save -o images.tar image1 [image2 ...\]`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimagesave" >}}) to save any images you want to keep to a local tar file.
   
   ​	或者，使用 [`docker image save -o images.tar image1 [image2 ...\]`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimagesave" >}}) 将您想要保存的镜像保存为本地 tar 文件。

After backing up your data, you can uninstall the current version of Docker Desktop and [install a different version]({{< ref "/manuals/DockerDesktop/Releasenotes" >}}) or reset Docker Desktop to factory defaults.

​	备份数据后，您可以卸载当前版本的 Docker Desktop 并 [安装其他版本]({{< ref "/manuals/DockerDesktop/Releasenotes" >}}) 或将 Docker Desktop 重置为出厂默认设置。

## 恢复您的数据 Restore your data

1. Use [`docker pull`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerpull" >}}) to restore images you pushed to Docker Hub. 使用 [`docker pull`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerpull" >}}) 恢复您推送到 Docker Hub 的镜像。

   If you backed up your images to a local tar file, use [`docker image load -i images.tar`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimageload" >}}) to restore previously saved images.

   ​	如果您将镜像备份为本地 tar 文件，请使用 [`docker image load -i images.tar`]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimageload" >}}) 恢复之前保存的镜像。

2. Re-create your containers if needed, using [`docker run`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerrun" >}}), or [Docker Compose]({{< ref "/manuals/DockerCompose" >}}). 如有需要，使用 [`docker run`]({{< ref "/reference/CLIreference/docker/dockercontainer/dockerrun" >}}) 或 [Docker Compose]({{< ref "/manuals/DockerCompose" >}}) 重新创建容器。

Refer to the [backup, restore, or migrate data volumes](https://docs.docker.com/engine/storage/volumes/#back-up-restore-or-migrate-data-volumes) page in the storage section to restore volume data.

​	请参考存储部分的 [备份、恢复或迁移数据卷](https://docs.docker.com/engine/storage/volumes/#back-up-restore-or-migrate-data-volumes) 页面来恢复卷数据。
