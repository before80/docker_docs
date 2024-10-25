+++
title = "Part 4: 持久化数据库"
date = 2024-10-23T14:54:35+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/get-started/workshop/05_persisting_data/](https://docs.docker.com/get-started/workshop/05_persisting_data/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# Persist the DB - 持久化数据库

In case you didn't notice, your todo list is empty every single time you launch the container. Why is this? In this part, you'll dive into how the container is working.

​	如果你没有注意到，每次启动容器时，你的待办事项列表都是空的。这是为什么呢？在本节中，你将深入了解容器的工作原理。

## 容器的文件系统 The container's filesystem

When a container runs, it uses the various layers from an image for its filesystem. Each container also gets its own "scratch space" to create/update/remove files. Any changes won't be seen in another container, even if they're using the same image.

​	当容器运行时，它会使用镜像中的各种层来构建其文件系统。每个容器还会获得自己的“临时空间”来创建、更新或删除文件。任何文件更改都不会影响到其他容器，即使它们使用相同的镜像。

### 实际操作 See this in practice

To see this in action, you're going to start two containers. In one container, you'll create a file. In the other container, you'll verify the file exists. What you'll see is that the file created in one container isn't available in another.

​	为了演示这个概念，你将启动两个容器。在一个容器中创建一个文件，然后在另一个容器中验证文件是否存在。你将看到，在一个容器中创建的文件在另一个容器中是不可用的。

1. Start an Alpine container and access its shell. 启动一个 Alpine 容器并进入其 shell。

   

   ```console
   $ docker run -ti --name=mytest alpine
   ```

2. In the container, create a `greeting.txt` file with `hello` inside. 在容器中创建一个名为 `greeting.txt` 的文件，并写入内容 `hello`。

   

   ```console
   / # echo "hello" > greeting.txt
   ```

3. Exit the container. 退出容器。

   

   ```console
   / # exit
   ```

4. Run a new Alpine container and use the `cat` command to verify that the file does not exist. 启动一个新的 Alpine 容器，并使用 `cat` 命令验证文件是否存在。

   

   ```console
   $ docker run alpine cat greeting.txt
   ```

   You should see output similar to the following that indicates the file does not exist in the new container.

   你将看到类似如下的输出，表明文件在新容器中不存在。

   ```console
   cat: can't open 'greeting.txt': No such file or directory
   ```

5. Go ahead and remove the containers using `docker ps --all` to get the IDs, and then `docker rm -f <container-id>` to remove the containers. 使用 `docker ps --all` 获取容器 ID，然后使用 `docker rm -f <container-id>` 删除容器。

## 容器卷 Container volumes

With the previous experiment, you saw that each container starts from the image definition each time it starts. While containers can create, update, and delete files, those changes are lost when you remove the container and Docker isolates all changes to that container. With volumes, you can change all of this.

​	通过之前的实验，你已经看到了每次启动容器时，它都会从镜像定义重新开始。虽然容器可以创建、更新和删除文件，但这些更改会在删除容器时丢失，并且 Docker 会将所有更改隔离在该容器内。使用卷可以改变这一点。

[Volumes]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}) provide the ability to connect specific filesystem paths of the container back to the host machine. If you mount a directory in the container, changes in that directory are also seen on the host machine. If you mount that same directory across container restarts, you'd see the same files.

​	[卷]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}) 提供了将容器的特定文件系统路径连接回主机的能力。如果你在容器中挂载一个目录，该目录中的更改也会反映在主机上。如果在容器重新启动时挂载相同的目录，你会看到相同的文件。

There are two main types of volumes. You'll eventually use both, but you'll start with volume mounts.

​	卷有两种主要类型，你最终会用到这两种类型，但你将首先使用卷挂载。

## 持久化待办事项数据 Persist the todo data

By default, the todo app stores its data in a SQLite database at `/etc/todos/todo.db` in the container's filesystem. If you're not familiar with SQLite, no worries! It's simply a relational database that stores all the data in a single file. While this isn't the best for large-scale applications, it works for small demos. You'll learn how to switch this to a different database engine later.

​	默认情况下，待办事项应用会将数据存储在容器文件系统的 `/etc/todos/todo.db` SQLite 数据库中。SQLite 是一种关系型数据库，将所有数据存储在单个文件中。尽管它不适合大规模应用，但对于小型演示是有效的。

With the database being a single file, if you can persist that file on the host and make it available to the next container, it should be able to pick up where the last one left off. By creating a volume and attaching (often called "mounting") it to the directory where you stored the data, you can persist the data. As your container writes to the `todo.db` file, it will persist the data to the host in the volume.

​	既然数据库是单个文件，如果你可以将该文件持久化到主机上，并在下次容器启动时提供它，容器应该能够接着上次的状态继续运行。通过创建一个卷并将其挂载到存储数据的目录，你可以持久化数据。随着容器向 `todo.db` 文件写入数据，它将通过卷将数据持久化到主机。

As mentioned, you're going to use a volume mount. Think of a volume mount as an opaque bucket of data. Docker fully manages the volume, including the storage location on disk. You only need to remember the name of the volume.

​	你将使用卷挂载。卷挂载就像一个不透明的数据存储桶，Docker 完全管理卷，包括存储在磁盘上的位置。你只需记住卷的名称即可。

### 创建卷并启动容器 Create a volume and start the container

You can create the volume and start the container using the CLI or Docker Desktop's graphical interface.

​	你可以使用 CLI 或 Docker Desktop 图形界面创建卷并启动容器。

{{< tabpane text=true persist=disabled >}}

{{% tab header="CLI" %}}

1. Create a volume by using the `docker volume create` command. 使用 `docker volume create` 命令创建一个卷。

   

   ```console
   $ docker volume create todo-db
   ```

2. Stop and remove the todo app container once again with `docker rm -f <id>`, as it is still running without using the persistent volume. 停止并删除仍在运行但未使用持久卷的待办事项应用容器，使用 `docker rm -f <id>` 命令删除它。

3. Start the todo app container, but add the `--mount` option to specify a volume mount. Give the volume a name, and mount it to `/etc/todos` in the container, which captures all files created at the path. 启动待办事项应用容器，并添加 `--mount` 选项指定卷挂载。为卷命名，并将其挂载到容器中的 `/etc/todos` 路径，该路径下创建的所有文件都将被捕获。

   

   ```console
   $ docker run -dp 127.0.0.1:3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-started
   ```

   > **Note**
   >
   > 
   >
   > If you're using Git Bash, you must use different syntax for this command.
   >
   > ​	如果你使用的是 Git Bash，必须为此命令使用不同的语法。
   >
   > ```console
   > $ docker run -dp 127.0.0.1:3000:3000 --mount type=volume,src=todo-db,target=//etc/todos getting-started
   > ```
   >
   > For more details about Git Bash's syntax differences, see [Working with Git Bash](https://docs.docker.com/desktop/troubleshoot/topics/#working-with-git-bash).
   >
   > ​	有关 Git Bash 语法差异的更多详细信息，请参阅 [使用 Git Bash](https://docs.docker.com/desktop/troubleshoot/topics/#working-with-git-bash)。

{{% /tab  %}}

{{% tab header=" Docker Desktop" %}}

To create a volume:

​	在 Docker Desktop 中创建卷：

1. Select **Volumes** in Docker Desktop. 在 Docker Desktop 中选择 **Volumes**。
2. In **Volumes**, select **Create**. 在 **Volumes** 中，选择 **Create**。
3. Specify `todo-db` as the volume name, and then select **Create**. 指定 `todo-db` 作为卷名称，然后选择 **Create**。

To stop and remove the app container: 停止并删除应用容器：

1. Select **Containers** in Docker Desktop. 在 Docker Desktop 中选择 **Containers**。
2. Select **Delete** in the **Actions** column for the container. 在容器的 **Actions** 列中选择 **Delete**。

To start the todo app container with the volume mounted: 使用卷挂载启动待办事项应用容器：

1. Select the search box at the top of Docker Desktop. 在 Docker Desktop 顶部选择搜索框。

2. In the search window, select the **Images** tab. 在搜索窗口中，选择 **Images** 选项卡。

3. In the search box, specify the image name, `getting-started`. 在搜索框中指定镜像名称，`getting-started`。

   > **Tip**
   >
   > 
   >
   > Use the search filter to filter images and only show **Local images**.
   >
   > ​	使用搜索筛选器仅显示 **本地镜像**。

4. Select your image and then select **Run**. 选择你的镜像，然后选择 **Run**。

5. Select **Optional settings**. 选择 **Optional settings**。

6. In **Host port**, specify the port, for example, `3000`. 在 **Host port** 中指定端口，例如 `3000`。

7. In **Host path**, specify the name of the volume, `todo-db`. 在 **Host path** 中指定卷名称，`todo-db`。

8. In **Container path**, specify `/etc/todos`. 在 **Container path** 中指定 `/etc/todos`。

9. Select **Run**. 选择 **Run**。

{{% /tab  %}}

{{< /tabpane >}}



------

### 验证数据持久化 Verify that the data persists

1. Once the container starts up, open the app and add a few items to your todo list. 容器启动后，打开应用并在待办事项列表中添加一些事项。

   ![Items added to todo list](Part4PersisttheDB_img/items-added.webp)

2. Stop and remove the container for the todo app. Use Docker Desktop or `docker ps` to get the ID and then `docker rm -f <id>` to remove it. 停止并删除待办事项应用的容器。使用 Docker Desktop 或 `docker ps` 获取容器 ID，然后使用 `docker rm -f <id>` 删除它。

3. Start a new container using the previous steps. 按照之前的步骤启动一个新容器。

4. Open the app. You should see your items still in your list. 打开应用。你应该会看到你的项目仍然在列表中。

5. Go ahead and remove the container when you're done checking out your list. 检查完列表后，可以删除容器。

You've now learned how to persist data.

​	你现在已经学会了如何持久化数据。

## 探索卷 Dive into the volume

A lot of people frequently ask "Where is Docker storing my data when I use a volume?" If you want to know, you can use the `docker volume inspect` command.

​	很多人经常问：“使用卷时，Docker 将我的数据存储在哪里？”如果你想知道，可以使用 `docker volume inspect` 命令。



```console
$ docker volume inspect todo-db
```

You should see output like the following:

​	你应该会看到类似如下的输出：



```console
[
    {
        "CreatedAt": "2019-09-26T02:18:36Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/todo-db/_data",
        "Name": "todo-db",
        "Options": {},
        "Scope": "local"
    }
]
```

The `Mountpoint` is the actual location of the data on the disk. Note that on most machines, you will need to have root access to access this directory from the host.

​	`Mountpoint` 是数据在磁盘上的实际位置。请注意，在大多数机器上，你需要有 root 访问权限才能从主机访问此目录。

## 总结 Summary

In this section, you learned how to persist container data.

​	在本节中，你学习了如何持久化容器数据。

Related information: 相关信息：

- [docker CLI reference]({{< ref "/reference/CLIreference/docker" >}}) - [docker CLI 参考]({{< ref "/reference/CLIreference/docker" >}})
- [Volumes]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}) - [卷]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})

## 接下来 Next steps

Next, you'll learn how you can develop your app more efficiently using bind mounts.

​	接下来，你将学习如何使用绑定挂载更高效地开发你的应用程序。

[Use bind mounts]({{< ref "/get-started/Dockerworkshop/Part5Usebindmounts" >}}) -  [使用绑定挂载]({{< ref "/get-started/Dockerworkshop/Part5Usebindmounts" >}})
