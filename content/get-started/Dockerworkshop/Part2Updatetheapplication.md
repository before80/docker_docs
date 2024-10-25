+++
title = "Part 2: 更新应用程序"
date = 2024-10-23T14:54:35+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/get-started/workshop/03_updating_app/](https://docs.docker.com/get-started/workshop/03_updating_app/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# Update the application - 更新应用程序

In [part 1]({{< ref "/get-started/Dockerworkshop/Part1Containerizeanapplication" >}}), you containerized a todo application. In this part, you'll update the application and image. You'll also learn how to stop and remove a container.

​	在[第一部分]({{< ref "/get-started/Dockerworkshop/Part1Containerizeanapplication" >}})中，你已经将一个待办事项应用程序容器化。在本部分中，你将更新应用程序和镜像。你还将学习如何停止和删除容器。

## 更新源代码 Update the source code

In the following steps, you'll change the "empty text" when you don't have any todo list items to "You have no todo items yet! Add one above!"

​	在以下步骤中，你将更改当待办事项列表为空时的显示文本，将其从"没有项目！在上方添加一个！"改为"你还没有待办事项！在上方添加一个！"

1. In the `src/static/js/app.js` file, update line 56 to use the new empty text. 在 `src/static/js/app.js` 文件中，更新第 56 行的文本。

   

   ```diff
   - <p className="text-center">No items yet! Add one above!</p>
   + <p className="text-center">You have no todo items yet! Add one above!</p>
   ```

2. Build your updated version of the image, using the `docker build` command. 使用 `docker build` 命令构建应用程序的更新镜像。

   

   ```console
   $ docker build -t getting-started .
   ```

3. Start a new container using the updated code. 使用更新后的代码启动一个新容器。

   

   ```console
   $ docker run -dp 127.0.0.1:3000:3000 getting-started
   ```

You probably saw an error like this: 

​	你可能会看到一个类似这样的错误：

```console
docker: Error response from daemon: driver failed programming external connectivity on endpoint laughing_burnell 
(bb242b2ca4d67eba76e79474fb36bb5125708ebdabd7f45c8eaf16caaabde9dd): Bind for 127.0.0.1:3000 failed: port is already allocated.
```

The error occurred because you aren't able to start the new container while your old container is still running. The reason is that the old container is already using the host's port 3000 and only one process on the machine (containers included) can listen to a specific port. To fix this, you need to remove the old container.

​	出现错误的原因是你无法在旧容器仍在运行时启动新容器，因为旧容器已经占用了主机的 3000 端口，而同一机器上的某个端口只能被一个进程（包括容器）监听。为了解决这个问题，你需要删除旧容器。

## 删除旧容器 Remove the old container

To remove a container, you first need to stop it. Once it has stopped, you can remove it. You can remove the old container using the CLI or Docker Desktop's graphical interface. Choose the option that you're most comfortable with.

​	要删除容器，首先需要停止它。一旦它停止后，你可以删除它。你可以使用 CLI 或 Docker Desktop 的图形界面删除旧容器。选择你最熟悉的方式。

{{< tabpane text=true persist=disabled >}}

{{% tab header="CLI" %}}

**Remove a container using the CLI**

1. Get the ID of the container by using the `docker ps` command. 使用 `docker ps` 命令获取容器的 ID。

   

   ```console
   $ docker ps
   ```

2. Use the `docker stop` command to stop the container. Replace `<the-container-id>` with the ID from `docker ps`. 使用 `docker stop` 命令停止容器。用 `docker ps` 中的容器 ID 替换 `<the-container-id>`。

   

   ```console
   $ docker stop <the-container-id>
   ```

3. Once the container has stopped, you can remove it by using the `docker rm` command. 一旦容器停止后，你可以使用 `docker rm` 命令删除它。

   

   ```console
   $ docker rm <the-container-id>
   ```

> **Note**
>
> You can stop and remove a container in a single command by adding the `force` flag to the `docker rm` command. For example: `docker rm -f <the-container-id>`
>
> ​	你可以通过在 `docker rm` 命令中添加 `-f` 强制标志来在一个命令中停止并删除容器。例如：`docker rm -f <the-container-id>`

{{% /tab  %}}

{{% tab header="Docker Desktop" %}}

**Remove a container using Docker Desktop 使用 Docker Desktop 删除容器**

1. Open Docker Desktop to the **Containers** view. 打开 Docker Desktop，进入 **Containers** 视图
2. Select the trash can icon under the **Actions** column for the container that you want to delete. 在你想要删除的容器的 **Actions** 列下，选择垃圾桶图标。
3. In the confirmation dialog, select **Delete forever**. 在确认对话框中，选择 **Delete forever**。

{{% /tab  %}}

{{< /tabpane >}}



------

### 启动更新后的应用容器 Start the updated app container

1. Now, start your updated app using the `docker run` command. 现在，使用 `docker run` 命令启动更新后的应用程序。

   

   ```console
   $ docker run -dp 127.0.0.1:3000:3000 getting-started
   ```

2. Refresh your browser on [http://localhost:3000](http://localhost:3000/) and you should see your updated help text. 刷新浏览器，访问 [http://localhost:3000](http://localhost:3000/)，你应该能看到更新后的帮助文本。

## 总结 Summary

In this section, you learned how to update and rebuild a container, as well as how to stop and remove a container.

​	在本节中，你学习了如何更新和重建容器，以及如何停止和删除容器。

Related information: 相关信息：

- [docker CLI reference]({{< ref "/reference/CLIreference/docker" >}}) - [docker CLI 参考]({{< ref "/reference/CLIreference/docker" >}})

## 接下来 Next steps

Next, you'll learn how to share images with others.

​	接下来，你将学习如何与他人共享镜像。

[Share the application]({{< ref "/get-started/Dockerworkshop/Part3Sharetheapplication" >}}) - [共享应用程序]({{< ref "/get-started/Dockerworkshop/Part3Sharetheapplication" >}})
