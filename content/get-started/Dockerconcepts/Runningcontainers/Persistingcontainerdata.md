+++
title = "持久化容器数据"
date = 2024-10-23T14:54:35+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/get-started/docker-concepts/running-containers/persisting-container-data/](https://docs.docker.com/get-started/docker-concepts/running-containers/persisting-container-data/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# Persisting container data - 持久化容器数据



{{< youtube "10_2BjqB_Ls">}}

## 说明 Explanation

When a container starts, it uses the files and configuration provided by the image. Each container is able to create, modify, and delete files and does so without affecting any other containers. When the container is deleted, these file changes are also deleted.

​	当一个容器启动时，它会使用镜像中提供的文件和配置。每个容器都可以创建、修改和删除文件，且不会影响其他容器。当容器被删除时，这些文件的更改也会被删除。

While this ephemeral nature of containers is great, it poses a challenge when you want to persist the data. For example, if you restart a database container, you might not want to start with an empty database. So, how do you persist files?

​	虽然容器的这种短暂性很棒，但当你想要持久化数据时，这就带来了挑战。例如，如果你重启一个数据库容器，你可能不希望从一个空数据库开始。那么，如何持久化文件呢？

### 容器卷 Container volumes

Volumes are a storage mechanism that provide the ability to persist data beyond the lifecycle of an individual container. Think of it like providing a shortcut or symlink from inside the container to outside the container.

​	卷（Volumes）是一种存储机制，能够将数据的存储周期超越单个容器的生命周期。可以把它看作是从容器内部到外部提供的一个快捷方式或符号链接。

As an example, imagine you create a volume named `log-data`.

​	例如，假设你创建了一个名为 `log-data` 的卷：

```console
$ docker volume create log-data
```

When starting a container with the following command, the volume will be mounted (or attached) into the container at `/logs`:

​	当你使用以下命令启动容器时，卷将会被挂载到容器中的 `/logs` 目录：



```console
$ docker run -d -p 80:80 -v log-data:/logs docker/welcome-to-docker
```

If the volume `log-data` doesn't exist, Docker will automatically create it for you.

​	如果卷 `log-data` 不存在，Docker 将自动为你创建它。

When the container runs, all files it writes into the `/logs` folder will be saved in this volume, outside of the container. If you delete the container and start a new container using the same volume, the files will still be there.

​	当容器运行时，所有它写入到 `/logs` 文件夹中的文件都将保存到该卷中，即使容器被删除，文件仍然存在。如果你使用同一个卷启动一个新容器，文件依然会在那里。

> **Sharing files using volumes 使用卷共享文件**
>
> You can attach the same volume to multiple containers to share files between containers. This might be helpful in scenarios such as log aggregation, data pipelines, or other event-driven applications.
>
> ​	你可以将同一个卷附加到多个容器上，以在容器之间共享文件。在日志聚合、数据管道或其他事件驱动应用场景中，这可能会非常有用。

### 管理卷 Managing volumes

Volumes have their own lifecycle beyond that of containers and can grow quite large depending on the type of data and applications you’re using. The following commands will be helpful to manage volumes:

​	卷的生命周期超越容器，并且根据你使用的数据类型和应用程序，它们可能会变得相当大。以下命令将有助于管理卷：

- `docker volume ls` - list all volumes `docker volume ls` - 列出所有卷
- `docker volume rm <volume-name-or-id>` - remove a volume (only works when the volume is not attached to any containers) `docker volume rm <volume-name-or-id>` - 删除卷（仅在卷未附加到任何容器时有效）
- `docker volume prune` - remove all unused (unattached) volumes `docker volume prune` - 删除所有未使用的卷

## 试试看 Try it out

In this guide, you’ll practice creating and using volumes to persist data created by a Postgres container. When the database runs, it stores files into the `/var/lib/postgresql/data` directory. By attaching the volume here, you will be able to restart the container multiple times while keeping the data.

​	在本指南中，你将练习创建和使用卷来持久化由 Postgres 容器创建的数据。当数据库运行时，它会将文件存储到 `/var/lib/postgresql/data` 目录。通过在此处附加卷，你可以多次重启容器并保持数据不变。

### 使用卷 Use volumes

1. [Download and install]({{< ref "/get-started/GetDocker" >}}) Docker Desktop.

2. Start a container using the [Postgres image](https://hub.docker.com/_/postgres) with the following command: 启动一个使用 [Postgres 镜像](https://hub.docker.com/_/postgres) 的容器，使用以下命令：

   

   ```console
   $ docker run --name=db -e POSTGRES_PASSWORD=secret -d -v postgres_data:/var/lib/postgresql/data postgres
   ```

   This will start the database in the background, configure it with a password, and attach a volume to the directory PostgreSQL will persist the database files.

   ​	这将启动数据库并在后台运行，配置密码，并将卷附加到 PostgreSQL 用于存储数据库文件的目录。

3. Connect to the database by using the following command: 使用以下命令连接到数据库：

   

   ```console
   $ docker exec -ti db psql -U postgres
   ```

4. In the PostgreSQL command line, run the following to create a database table and insert two records: 在 PostgreSQL 命令行中运行以下命令，创建一个数据库表并插入两条记录：

   

   ```text
   CREATE TABLE tasks (
       id SERIAL PRIMARY KEY,
       description VARCHAR(100)
   );
   INSERT INTO tasks (description) VALUES ('Finish work'), ('Have fun');
   ```

5. Verify the data is in the database by running the following in the PostgreSQL command line: 通过在 PostgreSQL 命令行中运行以下命令，验证数据已插入数据库：

   

   ```text
   SELECT * FROM tasks;
   ```

   You should get output that looks like the following: 你应该会看到如下输出：

   

   ```text
    id | description
   ----+-------------
     1 | Finish work
     2 | Have fun
   (2 rows)
   ```

6. Exit out of the PostgreSQL shell by running the following command: 运行以下命令退出 PostgreSQL shell：

   

   ```console
   \q
   ```

7. Stop and remove the database container. Remember that, even though the container has been deleted, the data is persisted in the `postgres_data` volume. 停止并删除数据库容器。即使容器已被删除，数据仍然保存在 `postgres_data` 卷中：

   

   ```console
   $ docker stop db
   $ docker rm db
   ```

8. Start a new container by running the following command, attaching the same volume with the persisted data: 使用以下命令启动一个新容器，附加相同的持久化数据卷：

   

   ```console
   $ docker run --name=new-db -d -v postgres_data:/var/lib/postgresql/data postgres 
   ```

   You might have noticed that the `POSTGRES_PASSWORD` environment variable has been omitted. That’s because that variable is only used when bootstrapping a new database.

   ​	你可能注意到此处没有设置 `POSTGRES_PASSWORD` 环境变量，因为该变量只在初始化新数据库时使用。

9. Verify the database still has the records by running the following command: 通过以下命令验证数据库中的记录仍然存在：

   

   ```console
   $ docker exec -ti new-db psql -U postgres -c "SELECT * FROM tasks"
   ```

### 查看卷内容 View volume contents

The Docker Dashboard provides the ability to view the contents of any volume, as well as the ability to export, import, and clone volumes.

​	Docker Dashboard 提供了查看任何卷内容的功能，此外还可以导出、导入和克隆卷。

1. Open the Docker Dashboard and navigate to the **Volumes** view. In this view, you should see the **postgres_data** volume. 打开 Docker Dashboard 并导航到 **Volumes** 视图。在该视图中，你应该会看到 **postgres_data** 卷。
2. Select the **postgres_data** volume’s name. 选择 **postgres_data** 卷的名称。
3. The **Data** tab shows the contents of the volume and provides the ability to navigate the files. Double-clicking on a file will let you see the contents and make changes. **Data** 标签页显示卷的内容，并提供浏览文件的功能。双击文件可以查看内容并进行更改。
4. Right-click on any file to save it or delete it. 右键单击任意文件可以保存或删除它。

### 删除卷 Remove volumes

Before removing a volume, it must not be attached to any containers. If you haven’t removed the previous container, do so with the following command (the `-f` will stop the container first and then remove it):

​	在删除卷之前，必须确保它没有附加到任何容器上。如果你还没有删除之前的容器，请使用以下命令删除它（`-f` 标志会先停止容器然后删除它）：

```console
$ docker rm -f new-db
```

There are a few methods to remove volumes, including the following:

​	有几种删除卷的方法，包括：

- Select the **Delete Volume** option on a volume in the Docker Dashboard. 在 Docker Dashboard 中选择卷并选择 **Delete Volume** 选项。

- Use the `docker volume rm` command: 使用 `docker volume rm` 命令：

  

  ```console
  $ docker volume rm postgres_data
  ```

- Use the `docker volume prune` command to remove all unused volumes: 使用 `docker volume prune` 命令删除所有未使用的卷：

  

  ```console
  $ docker volume prune
  ```

## 其他资源 Additional resources

The following resources will help you learn more about volumes:

​	以下资源将帮助你进一步了解卷：

- [Manage data in Docker]({{< ref "/manuals/DockerEngine/Storage" >}}) - [在 Docker 中管理数据]({{< ref "/manuals/DockerEngine/Storage" >}})
- [Volumes]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}) - [卷]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})
- [Volume mounts](https://docs.docker.com/engine/containers/run/#volume-mounts)

## 接下来 Next steps

Now that you have learned about persisting container data, it’s time to learn about sharing local files with containers.

​	现在你已经了解了如何持久化容器数据，接下来可以学习如何与容器共享本地文件。

[Sharing local files with containers]({{< ref "/get-started/Dockerconcepts/Runningcontainers/Sharinglocalfileswithcontainers" >}}) - [与容器共享本地文件]({{< ref "/get-started/Dockerconcepts/Runningcontainers/Sharinglocalfileswithcontainers" >}})
