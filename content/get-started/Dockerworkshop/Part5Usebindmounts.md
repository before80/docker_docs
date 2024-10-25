+++
title = "Part 5: 使用绑定挂载"
date = 2024-10-23T14:54:35+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/get-started/workshop/06_bind_mounts/](https://docs.docker.com/get-started/workshop/06_bind_mounts/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# Use bind mounts - 使用绑定挂载

In [part 4]({{< ref "/get-started/Dockerworkshop/Part4PersisttheDB" >}}), you used a volume mount to persist the data in your database. A volume mount is a great choice when you need somewhere persistent to store your application data.

​	在[第4部分]({{< ref "/get-started/Dockerworkshop/Part4PersisttheDB" >}})中，你使用卷挂载来持久化数据库中的数据。当你需要一个持久化存储应用数据的地方时，卷挂载是一个很好的选择。

A bind mount is another type of mount, which lets you share a directory from the host's filesystem into the container. When working on an application, you can use a bind mount to mount source code into the container. The container sees the changes you make to the code immediately, as soon as you save a file. This means that you can run processes in the container that watch for filesystem changes and respond to them.

​	绑定挂载是另一种类型的挂载，它允许你将主机文件系统中的目录共享到容器中。当开发应用程序时，你可以使用绑定挂载将源代码挂载到容器中。容器会立即看到你对代码所做的更改，只要你保存文件，容器就会感知到更改。这意味着你可以在容器中运行一些进程来监控文件系统的变化并响应它们。

In this chapter, you'll see how you can use bind mounts and a tool called [nodemon](https://npmjs.com/package/nodemon) to watch for file changes, and then restart the application automatically. There are equivalent tools in most other languages and frameworks.

​	在本章中，你将学习如何使用绑定挂载和一个名为 [nodemon](https://npmjs.com/package/nodemon) 的工具来监控文件更改，并在应用程序自动重启时更新内容。大多数其他语言和框架中都有类似的工具。

## 快速比较卷类型 Quick volume type comparisons

The following are examples of a named volume and a bind mount using `--mount`:

​	以下是使用 `--mount` 的命名卷和绑定挂载的示例：

- Named volume: `type=volume,src=my-volume,target=/usr/local/data` 命名卷：`type=volume,src=my-volume,target=/usr/local/data`

- Bind mount: `type=bind,src=/path/to/data,target=/usr/local/data` 绑定挂载：`type=bind,src=/path/to/data,target=/usr/local/data`

The following table outlines the main differences between volume mounts and bind mounts.

​	下表概述了卷挂载和绑定挂载的主要区别。

|                                                              | Named volumes 命名卷 | Bind mounts 绑定挂载 |
| ------------------------------------------------------------ | -------------------- | -------------------- |
| Host location 主机位置                                       | Docker chooses       | You decide           |
| Populates new volume with container contents 用容器内容填充新卷 | Yes                  | No                   |
| Supports Volume Drivers 支持卷驱动程序                       | Yes                  | No                   |

## 试用绑定挂载 Trying out bind mounts

Before looking at how you can use bind mounts for developing your application, you can run a quick experiment to get a practical understanding of how bind mounts work.

​	在了解如何使用绑定挂载开发应用程序之前，你可以通过快速实验来实际理解绑定挂载的工作原理。

1. Verify that your `getting-started-app` directory is in a directory defined in Docker Desktop's file sharing setting. This setting defines which parts of your filesystem you can share with containers. For details about accessing the setting, see [File sharing](https://docs.docker.com/desktop/settings/#file-sharing). 验证你的 `getting-started-app` 目录是否位于 Docker Desktop 的文件共享设置中定义的目录中。此设置定义了哪些文件系统部分可以与容器共享。有关如何访问该设置的详细信息，请参阅[文件共享](https://docs.docker.com/desktop/settings/#file-sharing)。

2. Open a terminal and change directory to the `getting-started-app` directory. 打开终端并切换到 `getting-started-app` 目录。

3. Run the following command to start `bash` in an `ubuntu` container with a bind mount. 运行以下命令在 `ubuntu` 容器中启动 `bash`，并使用绑定挂载。

   {{< tabpane text=true persist=disabled >}}

   {{% tab header="Mac / Linux " %}}

   ```console
    docker run -it --mount type=bind,src="$(pwd)",target=/src ubuntu bash
   ```

   {{% /tab  %}}

   {{% tab header="Command Prompt" %}}

   ```console
    docker run -it --mount "type=bind,src=%cd%,target=/src" ubuntu bash
   ```

   {{% /tab  %}}

   {{% tab header="Git Bash" %}}

   ```console
    docker run -it --mount type=bind,src="/$(pwd)",target=/src ubuntu bash
   ```

   {{% /tab  %}}

   {{% tab header="PowerShell" %}}

   ```console
    docker run -it --mount "type=bind,src=$($pwd),target=/src" ubuntu bash
   ```

   {{% /tab  %}}

   {{< /tabpane >}}

   

   The `--mount type=bind` option tells Docker to create a bind mount, where `src` is the current working directory on your host machine (`getting-started-app`), and `target` is where that directory should appear inside the container (`/src`).

   ​	`--mount type=bind` 选项告诉 Docker 创建一个绑定挂载，其中 `src` 是主机机器上的当前工作目录（`getting-started-app`），`target` 是该目录在容器中显示的位置（`/src`）。

4. After running the command, Docker starts an interactive `bash` session in the root directory of the container's filesystem. 运行命令后，Docker 将启动容器文件系统根目录中的交互式 `bash` 会话。

   

   ```console
   root@ac1237fad8db:/# pwd
   /
   root@ac1237fad8db:/# ls
   bin   dev  home  media  opt   root  sbin  srv  tmp  var
   boot  etc  lib   mnt    proc  run   src   sys  usr
   ```

5. Change directory to the `src` directory. 切换到 `src` 目录。

   This is the directory that you mounted when starting the container. Listing the contents of this directory displays the same files as in the `getting-started-app` directory on your host machine.

   这是启动容器时挂载的目录。列出该目录的内容将显示与主机机器上 `getting-started-app` 目录相同的文件。

   ```console
   root@ac1237fad8db:/# cd src
   root@ac1237fad8db:/src# ls
   Dockerfile  node_modules  package.json  spec  src  yarn.lock
   ```

6. Create a new file named `myfile.txt`. 创建一个名为 `myfile.txt` 的新文件。

   

   ```console
   root@ac1237fad8db:/src# touch myfile.txt
   root@ac1237fad8db:/src# ls
   Dockerfile  myfile.txt  node_modules  package.json  spec  src  yarn.lock
   ```

7. Open the `getting-started-app` directory on the host and observe that the `myfile.txt` file is in the directory. 打开主机上的 `getting-started-app` 目录，观察 `myfile.txt` 文件是否在目录中。

   

   ```text
   ├── getting-started-app/
   │ ├── Dockerfile
   │ ├── myfile.txt
   │ ├── node_modules/
   │ ├── package.json
   │ ├── spec/
   │ ├── src/
   │ └── yarn.lock
   ```

8. From the host, delete the `myfile.txt` file. 从主机删除 `myfile.txt` 文件。

9. In the container, list the contents of the `app` directory once more. Observe that the file is now gone. 在容器中再次列出 `app` 目录的内容。你会发现文件已经消失了。

   

   ```console
   root@ac1237fad8db:/src# ls
   Dockerfile  node_modules  package.json  spec  src  yarn.lock
   ```

10. Stop the interactive container session with `Ctrl` + `D`. 使用 `Ctrl` + `D` 停止交互式容器会话。

That's all for a brief introduction to bind mounts. This procedure demonstrated how files are shared between the host and the container, and how changes are immediately reflected on both sides. Now you can use bind mounts to develop software.

​	这是对绑定挂载的简要介绍。此过程展示了文件如何在主机和容器之间共享，以及如何在两者之间立即反映更改。现在，你可以使用绑定挂载进行软件开发了。

## 开发容器 Development containers

Using bind mounts is common for local development setups. The advantage is that the development machine doesn’t need to have all of the build tools and environments installed. With a single docker run command, Docker pulls dependencies and tools.

​	使用绑定挂载是本地开发环境中常见的做法。其优点是开发机器不需要安装所有的构建工具和环境。通过一个简单的 `docker run` 命令，Docker 可以拉取依赖项和工具。

### 在开发容器中运行应用 Run your app in a development container

The following steps describe how to run a development container with a bind mount that does the following:

​	以下步骤描述了如何运行带有绑定挂载的开发容器，该容器执行以下操作：

- Mount your source code into the container 将你的源代码挂载到容器中

- Install all dependencies 安装所有依赖项
- Start `nodemon` to watch for filesystem changes 启动 `nodemon` 以监控文件系统更改

You can use the CLI or Docker Desktop to run your container with a bind mount.

​	你可以使用 CLI 或 Docker Desktop 来运行绑定挂载的容器。

{{< tabpane text=true persist=disabled >}}

{{% tab header="Mac / Linux CLI " %}}

1. Make sure you don't have any `getting-started` containers currently running. 确保没有正在运行的 `getting-started` 容器。

2. Run the following command from the `getting-started-app` directory. 从 `getting-started-app` 目录中运行以下命令。

   

   ```console
   $ docker run -dp 127.0.0.1:3000:3000 \
       -w /app --mount type=bind,src="$(pwd)",target=/app \
       node:18-alpine \
       sh -c "yarn install && yarn run dev"
   ```

   The following is a breakdown of the command:

   ​	该命令的分解如下：

   - `-dp 127.0.0.1:3000:3000` - same as before. Run in detached (background) mode and create a port mapping `-dp 127.0.0.1:3000:3000` - 与之前相同。以分离模式（后台）运行，并创建端口映射

   - `-w /app` - sets the "working directory" or the current directory that the command will run from `-w /app` - 设置工作目录或将命令从该目录运行
   - `--mount type=bind,src="$(pwd)",target=/app` - bind mount the current directory from the host into the `/app` directory in the container `--mount type=bind,src="$(pwd)",target=/app` - 将主机的当前目录绑定挂载到容器中的 `/app` 目录
   - `node:18-alpine` - the image to use. Note that this is the base image for your app from the Dockerfile `node:18-alpine` - 要使用的镜像。请注意，这是 Dockerfile 中用于你的应用的基础镜像
   - `sh -c "yarn install && yarn run dev"` - the command. You're starting a shell using `sh` (alpine doesn't have `bash`) and running `yarn install` to install packages and then running `yarn run dev` to start the development server. If you look in the `package.json`, you'll see that the `dev` script starts `nodemon`. `sh -c "yarn install && yarn run dev"` - 该命令。你正在使用 `sh`（alpine 没有 `bash`）启动一个 shell，并运行 `yarn install` 来安装依赖项，然后运行 `yarn run dev` 来启动开发服务器。如果你查看 `package.json`，你会看到 `dev` 脚本启动了 `nodemon`。

3. You can watch the logs using `docker logs <container-id>`. You'll know you're ready to go when you see this: 你可以使用 `docker logs <container-id>` 观看日志。当你看到如下内容时，说明你已准备就绪：

   

   ```console
   $ docker logs -f <container-id>
   nodemon -L src/index.js
   [nodemon] 2.0.20
   [nodemon] to restart at any time, enter `rs`
   [nodemon] watching path(s): *.*
   [nodemon] watching extensions: js,mjs,json
   [nodemon] starting `node src/index.js`
   Using sqlite database at /etc/todos/todo.db
   Listening on port 3000
   ```

   When you're done watching the logs, exit out by hitting `Ctrl`+`C`.
   
   ​	当你不再需要查看日志时，按 `Ctrl`+`C` 退出。

{{% /tab  %}}

{{% tab header="PowerShell CLI" %}}

1. Make sure you don't have any `getting-started` containers currently running. 确保没有正在运行的 `getting-started` 容器。

2. Run the following command from the `getting-started-app` directory. 从 `getting-started-app` 目录中运行以下命令。

   

   ```powershell
   $ docker run -dp 127.0.0.1:3000:3000 `
       -w /app --mount "type=bind,src=$pwd,target=/app" `
       node:18-alpine `
       sh -c "yarn install && yarn run dev"
   ```

   The following is a breakdown of the command:

   ​	以下是该命令的详细说明：

   - `-dp 127.0.0.1:3000:3000` - same as before. Run in detached (background) mode and create a port mapping `-dp 127.0.0.1:3000:3000` - 和之前一样。在分离（后台）模式下运行并创建端口映射。
   - `-w /app` - sets the "working directory" or the current directory that the command will run from `-w /app` - 设置“工作目录”，即命令运行的当前目录。
   - `--mount "type=bind,src=$pwd,target=/app"` - bind mount the current directory from the host into the `/app` directory in the container  `--mount "type=bind,src=$pwd,target=/app"` - 绑定挂载，将主机的当前目录挂载到容器的 `/app` 目录中。
   - `node:18-alpine` - the image to use. Note that this is the base image for your app from the Dockerfile `node:18-alpine` - 要使用的镜像。注意，这是 Dockerfile 中应用的基础镜像。
   - `sh -c "yarn install && yarn run dev"` - the command. You're starting a shell using `sh` (alpine doesn't have `bash`) and running `yarn install` to install packages and then running `yarn run dev` to start the development server. If you look in the `package.json`, you'll see that the `dev` script starts `nodemon`. `sh -c "yarn install && yarn run dev"` - 运行的命令。这里启动了一个 `sh` shell（alpine 没有 `bash`），并运行 `yarn install` 安装依赖包，然后运行 `yarn run dev` 启动开发服务器。在 `package.json` 中可以看到 `dev` 脚本启动了 `nodemon`。

3. You can watch the logs using `docker logs <container-id>`. You'll know you're ready to go when you see this: 你可以使用 `docker logs <container-id>` 来查看日志。当你看到以下内容时，就表示准备就绪：

   

   ```console
   $ docker logs -f <container-id>
   nodemon -L src/index.js
   [nodemon] 2.0.20
   [nodemon] to restart at any time, enter `rs`
   [nodemon] watching path(s): *.*
   [nodemon] watching extensions: js,mjs,json
   [nodemon] starting `node src/index.js`
   Using sqlite database at /etc/todos/todo.db
   Listening on port 3000
   ```

   When you're done watching the logs, exit out by hitting `Ctrl`+`C`.
   
   ​	观看日志完毕后，按下 `Ctrl`+`C` 退出。

{{% /tab  %}}

{{% tab header="Command Prompt CLI" %}}

1. Make sure you don't have any `getting-started` containers currently running. 确保当前没有任何 `getting-started` 容器在运行。

2. Run the following command from the `getting-started-app` directory. 在 `getting-started-app` 目录中运行以下命令：

   

   ```console
   $ docker run -dp 127.0.0.1:3000:3000 ^
       -w /app --mount "type=bind,src=%cd%,target=/app" ^
       node:18-alpine ^
       sh -c "yarn install && yarn run dev"
   ```

   The following is a breakdown of the command: 以下是该命令的详细说明：

   - `-dp 127.0.0.1:3000:3000` - same as before. Run in detached (background) mode and create a port mapping `-dp 127.0.0.1:3000:3000` - 与之前一样，在分离（后台）模式下运行并创建端口映射。
   - `-w /app` - sets the "working directory" or the current directory that the command will run from `-w /app` - 设置工作目录，即命令将从该目录运行。
   - `--mount "type=bind,src=%cd%,target=/app"` - bind mount the current directory from the host into the `/app` directory in the container `--mount "type=bind,src=%cd%,target=/app"` - 绑定挂载，将主机的当前目录挂载到容器内的 `/app` 目录。
   - `node:18-alpine` - the image to use. Note that this is the base image for your app from the Dockerfile `node:18-alpine` - 使用的镜像，注意这是 Dockerfile 中应用的基础镜像。
   - `sh -c "yarn install && yarn run dev"` - the command. You're starting a shell using `sh` (alpine doesn't have `bash`) and running `yarn install` to install packages and then running `yarn run dev` to start the development server. If you look in the `package.json`, you'll see that the `dev` script starts `nodemon`.  `sh -c "yarn install && yarn run dev"` - 命令。使用 `sh` 启动一个 shell（alpine 没有 `bash`），运行 `yarn install` 安装依赖，然后运行 `yarn run dev` 启动开发服务器。在 `package.json` 中可以看到 `dev` 脚本启动了 `nodemon`。

3. You can watch the logs using `docker logs <container-id>`. You'll know you're ready to go when you see this: 你可以使用 `docker logs <container-id>` 查看日志。当你看到以下内容时，就表示准备就绪：

   

   ```console
   $ docker logs -f <container-id>
   nodemon -L src/index.js
   [nodemon] 2.0.20
   [nodemon] to restart at any time, enter `rs`
   [nodemon] watching path(s): *.*
   [nodemon] watching extensions: js,mjs,json
   [nodemon] starting `node src/index.js`
   Using sqlite database at /etc/todos/todo.db
   Listening on port 3000
   ```

   When you're done watching the logs, exit out by hitting `Ctrl`+`C`.
   
   ​	查看完日志后，可以通过按下 `Ctrl`+`C` 退出。

{{% /tab  %}}

{{% tab header="Git Bash CLI" %}}

1. Make sure you don't have any `getting-started` containers currently running. 确保当前没有任何 `getting-started` 容器在运行。

2. Run the following command from the `getting-started-app` directory. 在 `getting-started-app` 目录中运行以下命令：

   

   ```console
   $ docker run -dp 127.0.0.1:3000:3000 \
       -w //app --mount type=bind,src="/$(pwd)",target=/app \
       node:18-alpine \
       sh -c "yarn install && yarn run dev"
   ```

   The following is a breakdown of the command:  以下是该命令的详细说明：

   - `-dp 127.0.0.1:3000:3000` - same as before. Run in detached (background) mode and create a port mapping `-dp 127.0.0.1:3000:3000` - 与之前一样，在分离（后台）模式下运行并创建端口映射。
   - `-w //app` - sets the "working directory" or the current directory that the command will run from `-w //app` - 设置工作目录，即命令将从该目录运行。
   - `--mount type=bind,src="/$(pwd)",target=/app` - bind mount the current directory from the host into the `/app` directory in the container `--mount type=bind,src="/$(pwd)",target=/app` - 绑定挂载，将主机的当前目录挂载到容器内的 `/app` 目录。
   - `node:18-alpine` - the image to use. Note that this is the base image for your app from the Dockerfile `node:18-alpine` - 使用的镜像，注意这是 Dockerfile 中应用的基础镜像。
   - `sh -c "yarn install && yarn run dev"` - the command. You're starting a shell using `sh` (alpine doesn't have `bash`) and running `yarn install` to install packages and then running `yarn run dev` to start the development server. If you look in the `package.json`, you'll see that the `dev` script starts `nodemon`. `sh -c "yarn install && yarn run dev"` - 命令。使用 `sh` 启动一个 shell（alpine 没有 `bash`），运行 `yarn install` 安装依赖，然后运行 `yarn run dev` 启动开发服务器。在 `package.json` 中可以看到 `dev` 脚本启动了 `nodemon`。

3. You can watch the logs using `docker logs <container-id>`. You'll know you're ready to go when you see this: 你可以使用 `docker logs <container-id>` 查看日志。当你看到以下内容时，就表示准备就绪：

   

   ```console
   $ docker logs -f <container-id>
   nodemon -L src/index.js
   [nodemon] 2.0.20
   [nodemon] to restart at any time, enter `rs`
   [nodemon] watching path(s): *.*
   [nodemon] watching extensions: js,mjs,json
   [nodemon] starting `node src/index.js`
   Using sqlite database at /etc/todos/todo.db
   Listening on port 3000
   ```

   When you're done watching the logs, exit out by hitting `Ctrl`+`C`.
   
   ​	查看完日志后，可以通过按下 `Ctrl`+`C` 退出。

{{% /tab  %}}

{{% tab header="Docker Desktop" %}}

Make sure you don't have any `getting-started` containers currently running.

​	确保当前没有任何 `getting-started` 容器在运行。

Run the image with a bind mount.

​	使用绑定挂载运行镜像。

1. Select the search box at the top of Docker Desktop. 在 Docker Desktop 顶部选择搜索框。

2. In the search window, select the **Images** tab. 在搜索窗口中，选择 **Images** 标签。

3. In the search box, specify the container name, `getting-started`. 在搜索框中输入容器名称 `getting-started`。

   > **Tip**
   >
   > 
   >
   > Use the search filter to filter images and only show **Local images**.
   >
   > ​	使用搜索筛选器过滤镜像，仅显示 **本地镜像**。

4. Select your image and then select **Run**. 选择你的镜像，然后点击 **Run**。

5. Select **Optional settings**. 选择 **Optional settings**。

6. In **Host path**, specify the path to the `getting-started-app` directory on your host machine. 在 **Host path** 中，指定主机上 `getting-started-app` 目录的路径。

7. In **Container path**, specify `/app`. 在 **Container path** 中，指定 `/app`。

8. Select **Run**. 选择 **Run**。

You can watch the container logs using Docker Desktop.

​	你可以在 Docker Desktop 中查看容器日志。

1. Select **Containers** in Docker Desktop. 在 Docker Desktop 中选择 **Containers**。
2. Select your container name. 选择你的容器名称。

You'll know you're ready to go when you see this:

​	当你看到以下内容时，说明已经准备就绪：

```console
nodemon -L src/index.js
[nodemon] 2.0.20
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node src/index.js`
Using sqlite database at /etc/todos/todo.db
Listening on port 3000
```

{{% /tab  %}}

{{< /tabpane >}}



------

### 使用开发容器进行应用开发 Develop your app with the development container

Update your app on your host machine and see the changes reflected in the container.

​	在主机上更新你的应用，并查看容器中的变化。

1. In the `src/static/js/app.js` file, on line 109, change the "Add Item" button to simply say "Add": 在 `src/static/js/app.js` 文件中，将第 109 行的 "Add Item" 按钮文本更改为 "Add"：

   

   ```diff
   - {submitting ? 'Adding...' : 'Add Item'}
   + {submitting ? 'Adding...' : 'Add'}
   ```

   Save the file. 保存文件。

2. Refresh the page in your web browser, and you should see the change reflected almost immediately because of the bind mount. Nodemon detects the change and restarts the server. It might take a few seconds for the Node server to restart. If you get an error, try refreshing after a few seconds. 刷新浏览器中的页面，你应能立即看到绑定挂载所反映的更改。Nodemon 检测到更改并重启服务器。Node 服务器可能需要几秒钟重启，如果遇到错误，稍等片刻后再刷新。

   ![Screenshot of updated label for Add button](Part5Usebindmounts_img/updated-add-button.webp)

3. Feel free to make any other changes you'd like to make. Each time you make a change and save a file, the change is reflected in the container because of the bind mount. When Nodemon detects a change, it restarts the app inside the container automatically. When you're done, stop the container and build your new image using: 随意进行其他更改。每次更改并保存文件后，容器中会立即反映这些更改。Nodemon 检测到更改后会自动重启容器中的应用。完成后，停止容器并使用以下命令构建新镜像：

   

   ```console
   $ docker build -t getting-started .
   ```

## 总结 Summary

At this point, you can persist your database and see changes in your app as you develop without rebuilding the image.

​	至此，你可以持久化数据库并在开发时查看应用的更改，而无需重建镜像。

In addition to volume mounts and bind mounts, Docker also supports other mount types and storage drivers for handling more complex and specialized use cases.

​	除了卷挂载和绑定挂载，Docker 还支持其他类型的挂载和存储驱动，以处理更复杂和专业的用例。

Related information: 相关信息：

- [docker CLI reference]({{< ref "/reference/CLIreference/docker" >}})
- [Manage data in Docker](https://docs.docker.com/storage/)

## 接下来 Next steps

In order to prepare your app for production, you need to migrate your database from working in SQLite to something that can scale a little better. For simplicity, you'll keep using a relational database and switch your application to use MySQL. But, how should you run MySQL? How do you allow the containers to talk to each other? You'll learn about that in the next section.

​	为了将你的应用准备部署到生产环境，需要将数据库从 SQLite 迁移到更具扩展性的解决方案。为了简单起见，你将继续使用关系型数据库并将应用切换到 MySQL。但如何运行 MySQL？如何使容器彼此通信？你将在下一节中学习这些内容。

[Multi container apps]({{< ref "/get-started/Dockerworkshop/Part6Multi-containerapps" >}})
