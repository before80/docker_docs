+++
title = "使用构建缓存"
date = 2024-10-23T14:54:35+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/get-started/docker-concepts/building-images/using-the-build-cache/](https://docs.docker.com/get-started/docker-concepts/building-images/using-the-build-cache/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# Using the build cache - 使用构建缓存

{{< youtube "Ri6jMknjprY">}}

## 说明 Explanation

Consider the following Dockerfile that you created for the [getting-started]({{< ref "/get-started/Dockerconcepts/Buildingimages/WritingaDockerfile" >}}) app.

​	看看你为 [入门应用]({{< ref "/get-started/Dockerconcepts/Buildingimages/WritingaDockerfile" >}}) 创建的以下 Dockerfile：

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "./src/index.js"]
```

When you run the `docker build` command to create a new image, Docker executes each instruction in your Dockerfile, creating a layer for each command and in the order specified. For each instruction, Docker checks whether it can reuse the instruction from a previous build. If it finds that you've already executed a similar instruction before, Docker doesn't need to redo it. Instead, it’ll use the cached result. This way, your build process becomes faster and more efficient, saving you valuable time and resources.

​	当你运行 `docker build` 命令来创建新镜像时，Docker 会按 Dockerfile 中指定的顺序执行每条指令，并为每个命令创建一个层。对于每条指令，Docker 会检查是否可以重用以前构建的结果。如果 Docker 发现你之前已经执行过类似的指令，它就不需要重新执行，而是会使用缓存的结果。通过这种方式，你的构建过程变得更快更高效，节省了时间和资源。

Using the build cache effectively lets you achieve faster builds by reusing results from previous builds and skipping unnecessary work. In order to maximize cache usage and avoid resource-intensive and time-consuming rebuilds, it's important to understand how cache invalidation works. Here are a few examples of situations that can cause cache to be invalidated:

​	有效地使用构建缓存可以通过重用以前构建的结果来实现更快的构建，跳过不必要的工作。为了最大化缓存的使用并避免耗费资源和时间的重建，理解缓存失效的工作原理非常重要。以下是一些可能导致缓存失效的情况：

- Any changes to the command of a `RUN` instruction invalidates that layer. Docker detects the change and invalidates the build cache if there's any modification to a `RUN` command in your Dockerfile.
- 对 `RUN` 指令的命令进行任何更改都会使该层的缓存失效。Docker 检测到 `RUN` 命令的任何修改，都会使构建缓存失效。

- Any changes to files copied into the image with the `COPY` or `ADD` instructions. Docker keeps an eye on any alterations to files within your project directory. Whether it's a change in content or properties like permissions, Docker considers these modifications as triggers to invalidate the cache.
- 使用 `COPY` 或 `ADD` 指令将文件复制到镜像时的任何更改都会导致缓存失效。无论是内容变化还是文件属性（如权限）的改变，Docker 都会将这些修改视为触发缓存失效的原因。
- Once one layer is invalidated, all following layers are also invalidated. If any previous layer, including the base image or intermediary layers, has been invalidated due to changes, Docker ensures that subsequent layers relying on it are also invalidated. This keeps the build process synchronized and prevents inconsistencies.
- 一旦某一层缓存失效，后续的所有层也会失效。如果任何前面的层（包括基础镜像或中间层）因更改而失效，Docker 会确保依赖该层的后续层也失效，以保持构建过程的一致性，防止出现不一致。

When you're writing or editing a Dockerfile, keep an eye out for unnecessary cache misses to ensure that builds run as fast and efficiently as possible.

​	当你编写或编辑 Dockerfile 时，请注意避免不必要的缓存失效，以确保构建尽可能快和高效。

## 试试看 Try it out

In this hands-on guide, you will learn how to use the Docker build cache effectively for a Node.js application.

​	在本实践指南中，你将学习如何为 Node.js 应用程序有效地使用 Docker 构建缓存。

### 构建应用程序 Build the application

1. [Download and install](https://www.docker.com/products/docker-desktop/) Docker Desktop. [下载并安装](https://www.docker.com/products/docker-desktop/) Docker Desktop。

2. Open a terminal and [clone this sample application](https://github.com/dockersamples/todo-list-app). 打开终端并 [克隆该示例应用程序](https://github.com/dockersamples/todo-list-app)。

   

   ```console
   $ git clone https://github.com/dockersamples/todo-list-app
   ```

3. Navigate into the `todo-list-app` directory: 进入 `todo-list-app` 目录：

   

   ```console
   $ cd todo-list-app
   ```

   Inside this directory, you'll find a file named `Dockerfile` with the following content:

   在这个目录中，你会找到一个名为 `Dockerfile` 的文件，内容如下：

   ```dockerfile
   FROM node:20-alpine
   WORKDIR /app
   COPY . .
   RUN yarn install --production
   EXPOSE 3000
   CMD ["node", "./src/index.js"]
   ```

4. Execute the following command to build the Docker image: 执行以下命令来构建 Docker 镜像：

   

   ```console
   $ docker build .
   ```

   Here’s the result of the build process:

   这是构建过程的结果：

   ```console
   [+] Building 20.0s (10/10) FINISHED
   ```

   The first line indicates that the entire build process took *20.0 seconds*. The first build may take some time as it installs dependencies.

   ​	第一次构建可能需要一些时间来安装依赖项。

5. Rebuild without making changes. 在不进行任何更改的情况下重新构建。

   Now, re-run the `docker build` command without making any change in the source code or Dockerfile as shown:

   现在，在没有修改源代码或 Dockerfile 的情况下，重新运行 `docker build` 命令：

   ```console
   $ docker build .
   ```

   Subsequent builds after the initial are faster due to the caching mechanism, as long as the commands and context remain unchanged. Docker caches the intermediate layers generated during the build process. When you rebuild the image without making any changes to the Dockerfile or the source code, Docker can reuse the cached layers, significantly speeding up the build process.

   ​	后续的构建由于缓存机制而更快，只要命令和上下文保持不变。Docker 缓存了构建过程中生成的中间层。当你在没有修改 Dockerfile 或源代码的情况下重新构建镜像时，Docker 可以重用缓存的层，从而显著加快构建速度。

   ```console
   [+] Building 1.0s (9/9) FINISHED                                                                            docker:desktop-linux
    => [internal] load build definition from Dockerfile                                                                        0.0s
    => => transferring dockerfile: 187B                                                                                        0.0s
    ...
    => [internal] load build context                                                                                           0.0s
    => => transferring context: 8.16kB                                                                                         0.0s
    => CACHED [2/4] WORKDIR /app                                                                                               0.0s
    => CACHED [3/4] COPY . .                                                                                                   0.0s
    => CACHED [4/4] RUN yarn install --production                                                                              0.0s
    => exporting to image                                                                                                      0.0s
    => => exporting layers                                                                                                     0.0s
    => => exporting manifest
   ```

   The subsequent build was completed in just 1.0 second by leveraging the cached layers. No need to repeat time-consuming steps like installing dependencies.

   ​	第二次构建仅用了 1.0 秒，通过利用缓存的层避免了重复执行耗时的步骤，如安装依赖项。

   | Steps | Description                                        | Time Taken(1st Run) | Time Taken (2nd Run) |
   | ----- | -------------------------------------------------- | ------------------- | -------------------- |
   | 1     | Load build definition from Dockerfile              | 0.0 seconds         | 0.0 seconds          |
   | 2     | Load metadata for docker.io/library/node:20-alpine | 2.7 seconds         | 0.9 seconds          |
   | 3     | Load .dockerignore                                 | 0.0 seconds         | 0.0 seconds          |
   | 4     | Load build context(Context size: 4.60MB)           | 0.1 seconds         | 0.0 seconds          |
   | 5     | Set the working directory (WORKDIR)                | 0.1 seconds         | 0.0 seconds          |
   | 6     | Copy the local code into the container             | 0.0 seconds         | 0.0 seconds          |
   | 7     | Run yarn install --production                      | 10.0 seconds        | 0.0 seconds          |
   | 8     | Exporting layers                                   | 2.2 seconds         | 0.0 seconds          |
   | 9     | Exporting the final image                          | 3.0 seconds         | 0.0 seconds          |

   Going back to the `docker image history` output, you see that each command in the Dockerfile becomes a new layer in the image. You might remember that when you made a change to the image, the `yarn` dependencies had to be reinstalled. Is there a way to fix this? It doesn't make much sense to reinstall the same dependencies every time you build, right?

   ​	回到 `docker image history` 的输出，你会看到 Dockerfile 中的每个命令都成为镜像中的一个新层。你可能还记得，当你更改镜像时，`yarn` 依赖项需要重新安装。有没有办法解决这个问题？每次构建时重新安装相同的依赖项似乎没有多大意义，对吧？

   To fix this, restructure your Dockerfile so that the dependency cache remains valid unless it really needs to be invalidated. For Node-based applications, dependencies are defined in the `package.json` file. You'll want to reinstall the dependencies if that file changes, but use cached dependencies if the file is unchanged. So, start by copying only that file first, then install the dependencies, and finally copy everything else. Then, you only need to recreate the yarn dependencies if there was a change to the `package.json` file.

   ​	为了解决这个问题，重构你的 Dockerfile，使依赖项的缓存保持有效，除非确实需要失效。对于基于 Node 的应用程序，依赖项通常定义在 `package.json` 文件中。如果该文件发生变化，你需要重新安装依赖项，但如果文件没有变化，则可以使用缓存的依赖项。因此，首先只复制该文件，然后安装依赖项，最后再复制其他文件。这样，只有当 `package.json` 文件发生变化时，才需要重新创建 yarn 依赖项。

6. Update the Dockerfile to copy in the `package.json` file first, install dependencies, and then copy everything else in. 更新 Dockerfile，首先复制 `package.json` 文件，安装依赖项，然后再复制其他内容。

   

   ```dockerfile
   FROM node:20-alpine
   WORKDIR /app
   COPY package.json yarn.lock ./
   RUN yarn install --production 
   COPY . . 
   EXPOSE 3000
   CMD ["node", "src/index.js"]
   ```

7. Create a file named `.dockerignore` in the same folder as the Dockerfile with the following contents. 在与 Dockerfile 相同的文件夹中创建一个名为 `.dockerignore` 的文件，内容如下：

   

   ```plaintext
   node_modules
   ```

8. Build the new image: 构建新的镜像：

   

   ```console
   $ docker build .
   ```

   You'll then see output similar to the following:

   你将看到类似以下的输出：

   ```console
   [+] Building 16.1s (10/10) FINISHED
   => [internal] load build definition from Dockerfile                                               0.0s
   => => transferring dockerfile: 175B                                                               0.0s
   => [internal] load .dockerignore                                                                  0.0s
   => => transferring context: 2B                                                                    0.0s
   => [internal] load metadata for docker.io/library/node:21-alpine                                  0.0s
   => [internal] load build context                                                                  0.8s
   => => transferring context: 53.37MB                                                               0.8s
   => [1/5] FROM docker.io/library/node:21-alpine                                                    0.0s
   => CACHED [2/5] WORKDIR /app                                                                      0.0s
   => [3/5] COPY package.json yarn.lock ./                                                           0.2s
   => [4/5] RUN yarn install --production                                                           14.0s
   => [5/5] COPY . .                                                                                 0.5s
   => exporting to image                                                                             0.6s
   => => exporting layers                                                                            0.6s
   => => writing image     
   sha256:d6f819013566c54c50124ed94d5e66c452325327217f4f04399b45f94e37d25        0.0s
   => => naming to docker.io/library/node-app:2.0                                                 0.0s
   ```

   You'll see that all layers were rebuilt. Perfectly fine since you changed the Dockerfile quite a bit.

   ​	你会看到所有层都重新构建了。很正常，因为你对 Dockerfile 做了不少改动。

9. Now, make a change to the `src/static/index.html` file (like change the title to say "The Awesome Todo App"). 现在，对 `src/static/index.html` 文件进行更改（例如，将标题更改为 "The Awesome Todo App"）。

10. Build the Docker image. This time, your output should look a little different. 构建 Docker 镜像。这次你的输出应该有所不同。

    

    ```console
    $ docker build -t node-app:3.0 .
    ```

    You'll then see output similar to the following:

    你将看到类似以下的输出：

    ```console
    [+] Building 1.2s (10/10) FINISHED 
    => [internal] load build definition from Dockerfile                                               0.0s
    => => transferring dockerfile: 37B                                                                0.0s
    => [internal] load .dockerignore                                                                  0.0s
    => => transferring context: 2B                                                                    0.0s
    => [internal] load metadata for docker.io/library/node:21-alpine                                  0.0s 
    => [internal] load build context                                                                  0.2s
    => => transferring context: 450.43kB                                                              0.2s
    => [1/5] FROM docker.io/library/node:21-alpine                                                    0.0s
    => CACHED [2/5] WORKDIR /app                                                                      0.0s
    => CACHED [3/5] COPY package.json yarn.lock ./                                                    0.0s
    => CACHED [4/5] RUN yarn install --production                                                     0.0s
    => [5/5] COPY . .                                                                                 0.5s 
    => exporting to image                                                                             0.3s
    => => exporting layers                                                                            0.3s
    => => writing image     
    sha256:91790c87bcb096a83c2bd4eb512bc8b134c757cda0bdee4038187f98148e2eda       0.0s
    => => naming to docker.io/library/node-app:3.0                                                 0.0s
    ```

    First off, you should notice that the build was much faster. You'll see that several steps are using previously cached layers. That's good news; you're using the build cache. Pushing and pulling this image and updates to it will be much faster as well.
    
    ​	首先，你会注意到构建速度快了很多。你会看到有几个步骤正在使用先前缓存的层。这是个好消息，说明你正在使用构建缓存。推送和拉取这个镜像及其更新也会变得更快。

By following these optimization techniques, you can make your Docker builds faster and more efficient, leading to quicker iteration cycles and improved development productivity.

​	通过遵循这些优化技术，你可以让 Docker 构建变得更快更高效，从而加快迭代周期并提高开发效率。

## 其他资源 Additional resources

- [Optimizing builds with cache management]({{< ref "/manuals/DockerBuild/Cache" >}})
- [Cache Storage Backend]({{< ref "/manuals/DockerBuild/Cache/Cachestoragebackends" >}})
- [Build cache invalidation]({{< ref "/manuals/DockerBuild/Cache/Buildcacheinvalidation" >}})

## 接下来 Next steps

Now that you understand how to use the Docker build cache effectively, you're ready to learn about Multi-stage builds.

​	现在你已经了解了如何有效地使用 Docker 构建缓存，接下来可以学习关于多阶段构建的内容。

[Multi-stage builds]({{< ref "/get-started/Dockerconcepts/Buildingimages/Multi-stagebuilds" >}}) [多阶段构建]({{< ref "/get-started/Dockerconcepts/Buildingimages/Multi-stagebuilds" >}})
