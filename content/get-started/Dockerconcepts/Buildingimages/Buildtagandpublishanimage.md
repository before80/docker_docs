+++
title = "构建、标记和发布镜像"
date = 2024-10-23T14:54:35+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/get-started/docker-concepts/building-images/build-tag-and-publish-an-image/](https://docs.docker.com/get-started/docker-concepts/building-images/build-tag-and-publish-an-image/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# Build, tag, and publish an image - 构建、标记和发布镜像

{{< youtube "chiiGLlYRlY">}}

## 说明 Explanation

In this guide, you will learn the following:

​	在本指南中，您将学习以下内容：

- Building images - the process of building an image based on a `Dockerfile` 构建镜像——基于 `Dockerfile` 构建镜像的过程

- Tagging images - the process of giving an image a name, which also determines where the image can be distributed 标记镜像——为镜像命名的过程，也决定了镜像的分发位置
- Publishing images - the process to distribute or share the newly created image using a container registry 发布镜像——使用容器注册表分发或共享新创建的镜像的过程

### 构建镜像 Building images

Most often, images are built using a Dockerfile. The most basic `docker build` command might look like the following:

​	通常，镜像是使用 Dockerfile 构建的。最基本的 `docker build` 命令可能如下所示：



```bash
docker build .
```

The final `.` in the command provides the path or URL to the [build context](https://docs.docker.com/build/concepts/context/#what-is-a-build-context). At this location, the builder will find the `Dockerfile` and other referenced files.

​	命令中的最后一个 `.` 指定了[构建上下文](https://docs.docker.com/build/concepts/context/#what-is-a-build-context)的路径或 URL。在该位置，构建器将找到 `Dockerfile` 和其他引用文件。

When you run a build, the builder pulls the base image, if needed, and then runs the instructions specified in the Dockerfile.

​	当您运行构建时，构建器会根据需要拉取基础镜像，并按照 Dockerfile 中的指令逐步执行。

With the previous command, the image will have no name, but the output will provide the ID of the image. As an example, the previous command might produce the following output:

​	在上一个命令中，生成的镜像没有名称，但输出会提供镜像的 ID。例如，上一个命令可能产生如下输出：



```console
$ docker build .
[+] Building 3.5s (11/11) FINISHED                                              docker:desktop-linux
 => [internal] load build definition from Dockerfile                                            0.0s
 => => transferring dockerfile: 308B                                                            0.0s
 => [internal] load metadata for docker.io/library/python:3.12                                  0.0s
 => [internal] load .dockerignore                                                               0.0s
 => => transferring context: 2B                                                                 0.0s
 => [1/6] FROM docker.io/library/python:3.12                                                    0.0s
 => [internal] load build context                                                               0.0s
 => => transferring context: 123B                                                               0.0s
 => [2/6] WORKDIR /usr/local/app                                                                0.0s
 => [3/6] RUN useradd app                                                                       0.1s
 => [4/6] COPY ./requirements.txt ./requirements.txt                                            0.0s
 => [5/6] RUN pip install --no-cache-dir --upgrade -r requirements.txt                          3.2s
 => [6/6] COPY ./app ./app                                                                      0.0s
 => exporting to image                                                                          0.1s
 => => exporting layers                                                                         0.1s
 => => writing image sha256:9924dfd9350407b3df01d1a0e1033b1e543523ce7d5d5e2c83a724480ebe8f00    0.0s
```

With the previous output, you could start a container by using the referenced image:

​	在上面的输出中，您可以使用镜像 ID 启动容器：



```console
docker run sha256:9924dfd9350407b3df01d1a0e1033b1e543523ce7d5d5e2c83a724480ebe8f00
```

That name certainly isn't memorable, which is where tagging becomes useful.

​	这种名称显然不易记忆，这就是标记的作用。

### 标记镜像 Tagging images

Tagging images is the method to provide an image with a memorable name. However, there is a structure to the name of an image. A full image name has the following structure:

​	标记镜像是为镜像提供一个易记名称的方法。镜像的完整名称具有以下结构：



```text
[HOST[:PORT_NUMBER]/]PATH[:TAG]
```

- `HOST`: The optional registry hostname where the image is located. If no host is specified, Docker's public registry at `docker.io` is used by default. `HOST`: 可选的注册表主机名。如果未指定主机，则默认使用 Docker 的公共注册表 `docker.io`。
- `PORT_NUMBER`: The registry port number if a hostname is provided `PORT_NUMBER`: 如果提供了主机名，则需要指定注册表的端口号
- `PATH`: The path of the image, consisting of slash-separated components. For Docker Hub, the format follows `[NAMESPACE/]REPOSITORY`, where namespace is either a user's or organization's name. If no namespace is specified, `library` is used, which is the namespace for Docker Official Images. `PATH`: 镜像的路径，由用斜杠分隔的组件组成。对于 Docker Hub，格式为 `[NAMESPACE/]REPOSITORY`，其中命名空间是用户或组织的名称。如果未指定命名空间，则使用 `library`，这是 Docker 官方镜像的命名空间。
- `TAG`: A custom, human-readable identifier that's typically used to identify different versions or variants of an image. If no tag is specified, `latest` is used by default. `TAG`: 自定义的人类可读标识符，通常用于标识镜像的不同版本或变体。如果未指定标签，则默认为 `latest`。

Some examples of image names include:

​	镜像名称的示例包括：

- `nginx`, equivalent to `docker.io/library/nginx:latest`: this pulls an image from the `docker.io` registry, the `library` namespace, the `nginx` image repository, and the `latest` tag.  -- `nginx`，相当于 `docker.io/library/nginx`：该命令会从 `docker.io` 注册表中拉取镜像，属于 `library` 命名空间，`nginx `镜像仓库，并带有 `latest` 标签。

- `docker/welcome-to-docker`, equivalent to `docker.io/docker/welcome-to-docker:latest`: this pulls an image from the `docker.io` registry, the `docker` namespace, the `welcome-to-docker` image repository, and the `latest` tag  -- `docker/welcome-to-docker`，相当于 `docker.io/docker/welcome-to-docker:latest`：该命令会从 `docker.io` 注册表中拉取镜像，属于 `docker` 命名空间，`welcome-to-docker` 镜像仓库，并带有 `latest` 标签。
- `ghcr.io/dockersamples/example-voting-app-vote:pr-311`: this pulls an image from the GitHub Container Registry, the `dockersamples` namespace, the `example-voting-app-vote` image repository, and the `pr-311` tag -- `ghcr.io/dockersamples/example-voting-app-vote:pr-311`：该命令会从 GitHub Container Registry 中拉取镜像，属于 `dockersamples` 命名空间，`example-voting-app-vote` 镜像仓库，并带有 `pr-311` 标签。

To tag an image during a build, add the `-t` or `--tag` flag:

​	在构建期间标记镜像，可添加 `-t` 或 `--tag` 标志：

```console
docker build -t my-username/my-image .
```

If you've already built an image, you can add another tag to the image by using the [`docker image tag`](https://docs.docker.com/engine/reference/commandline/image_tag/) command:

​	如果您已经构建了一个镜像，可以使用 [`docker image tag`](https://docs.docker.com/engine/reference/commandline/image_tag/) 命令为镜像添加另一个标签：



```console
docker image tag my-username/my-image another-username/another-image:v1
```

### 发布镜像 Publishing images

Once you have an image built and tagged, you're ready to push it to a registry. To do so, use the [`docker push`](https://docs.docker.com/engine/reference/commandline/image_push/) command:

​	构建并标记镜像后，您可以将其推送到注册表。为此，请使用 [`docker push`](https://docs.docker.com/engine/reference/commandline/image_push/) 命令：



```console
docker push my-username/my-image
```

Within a few seconds, all of the layers for your image will be pushed to the registry.

​	几秒钟后，您的镜像的所有层都将被推送到注册表。

> **Requiring authentication 需要身份验证**
>
> Before you're able to push an image to a repository, you will need to be authenticated. To do so, simply use the [docker login](https://docs.docker.com/engine/reference/commandline/login/) command.
>
> ​	在您将镜像推送到仓库之前，需要进行身份验证。可以使用 [docker login](https://docs.docker.com/engine/reference/commandline/login/) 命令完成此操作。

## 试试看 Try it out

In this hands-on guide, you will build a simple image using a provided Dockerfile and push it to Docker Hub.

​	在本次动手指南中，您将使用提供的 Dockerfile 构建一个简单镜像并将其推送到 Docker Hub。

### 设置 Set up

1. Get the sample application. 获取示例应用程序。

   If you have Git, you can clone the repository for the sample application. Otherwise, you can download the sample application. Choose one of the following options.

   如果您有 Git，可以克隆示例应用程序的仓库。否则，您可以下载示例应用程序。选择以下选项之一。

   {{< tabpane text=true persist=disabled >}}

   {{% tab header="Clone with git" %}}

   Use the following command in a terminal to clone the sample application repository.

   在终端中使用以下命令克隆示例应用程序仓库：

   ```console
   $ git clone https://github.com/docker/getting-started-todo-app
   ```

   {{% /tab  %}}

   {{% tab header="Download" %}}

   Download the source and extract it.

   ​	下载源码并解压。

   [Download the source](https://github.com/docker/getting-started-todo-app/raw/cd61f824da7a614a8298db503eed6630eeee33a3/app.zip)

   {{% /tab  %}}

   {{< /tabpane >}}

   ------

2. [Download and install](https://www.docker.com/products/docker-desktop/) Docker Desktop. [下载并安装](https://www.docker.com/products/docker-desktop/) Docker Desktop。

3. If you don't have a Docker account yet, [create one now](https://hub.docker.com/). Once you've done that, sign in to Docker Desktop using that account. 

4. 如果还没有 Docker 账户，请[立即创建](https://hub.docker.com/)。完成后，使用该账户登录 Docker Desktop。

### 构建镜像 Build an image

Now that you have a repository on Docker Hub, it's time for you to build an image and push it to the repository.

​	现在您已经在 Docker Hub 上有了一个仓库，可以构建一个镜像并将其推送到该仓库。

1. Using a terminal in the root of the sample app repository, run the following command. Replace `YOUR_DOCKER_USERNAME` with your Docker Hub username: 在示例应用的根目录中使用终端，运行以下命令。将 `YOUR_DOCKER_USERNAME` 替换为您的 Docker Hub 用户名：

   

   ```console
   $ docker build -t YOUR_DOCKER_USERNAME/concepts-build-image-demo .
   ```

   As an example, if your username is `mobywhale`, you would run the command:

   例如，如果您的用户名是 `mobywhale`，则运行以下命令：

   ```console
   $ docker build -t mobywhale/concepts-build-image-demo .
   ```

2. Once the build has completed, you can view the image by using the following command:

   构建完成后，可以使用以下命令查看镜像：

   ```console
   $ docker image ls
   ```

   The command will produce output similar to the following:

   命令将生成如下输出：

   ```plaintext
   REPOSITORY                             TAG       IMAGE ID       CREATED          SIZE
   mobywhale/concepts-build-image-demo    latest    746c7e06537f   24 seconds ago   354MB
   ```

3. You can actually view the history (or how the image was created) by using the [docker image history]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimagehistory" >}}) command: 您可以使用 [docker image history]({{< ref "/reference/CLIreference/docker/dockerimage/dockerimagehistory" >}}) 命令查看镜像的历史记录：

   

   ```console
   $ docker image history mobywhale/concepts-build-image-demo
   ```

   You'll then see output similar to the following:

   您将看到类似以下的输出：

   ```plaintext
   IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
   f279389d5f01   8 seconds ago   CMD ["node" "./src/index.js"]                   0B        buildkit.dockerfile.v0
   <missing>      8 seconds ago   EXPOSE map[3000/tcp:{}]                         0B        buildkit.dockerfile.v0 
   <missing>      8 seconds ago   WORKDIR /app                                    8.19kB    buildkit.dockerfile.v0
   <missing>      4 days ago      /bin/sh -c #(nop)  CMD ["node"]                 0B
   <missing>      4 days ago      /bin/sh -c #(nop)  ENTRYPOINT ["docker-entry…   0B
   <missing>      4 days ago      /bin/sh -c #(nop) COPY file:4d192565a7220e13…   20.5kB
   <missing>      4 days ago      /bin/sh -c apk add --no-cache --virtual .bui…   7.92MB
   <missing>      4 days ago      /bin/sh -c #(nop)  ENV YARN_VERSION=1.22.19     0B
   <missing>      4 days ago      /bin/sh -c addgroup -g 1000 node     && addu…   126MB
   <missing>      4 days ago      /bin/sh -c #(nop)  ENV NODE_VERSION=20.12.0     0B
   <missing>      2 months ago    /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B
   <missing>      2 months ago    /bin/sh -c #(nop) ADD file:d0764a717d1e9d0af…   8.42MB
   ```

   This output shows the layers of the image, highlighting the layers you added and those that were inherited from your base image.
   
   ​	此输出显示了镜像的层次结构，突显出您添加的层和基础镜像继承的层。

### 推送镜像 Push the image

Now that you have an image built, it's time to push the image to a registry.

​	现在您已经构建了镜像，是时候将其推送到注册表了。

1. Push the image using the [docker push]({{< ref "/reference/CLIreference/docker/dockerimage/dockerpush" >}}) command: 使用 [docker push]({{< ref "/reference/CLIreference/docker/dockerimage/dockerpush" >}}) 命令推送镜像：

   

   ```console
   $ docker push YOUR_DOCKER_USERNAME/concepts-build-image-demo
   ```

   If you receive a `requested access to the resource is denied`, make sure you are both logged in and that your Docker username is correct in the image tag.

   ​	如果收到 `requested access to the resource is denied` 错误，请确保您已登录，并且镜像标签中的 Docker 用户名正确。
   
   After a moment, your image should be pushed to Docker Hub.
   
   ​	片刻之后，您的镜像应被推送到 Docker Hub。

## 其他资源 Additional resources

To learn more about building, tagging, and publishing images, visit the following resources:

​	要了解更多有关构建、标记和发布镜像的信息，请访问以下资源：

- [What is a build context?](https://docs.docker.com/build/concepts/context/#what-is-a-build-context) 什么是构建上下文？

- [docker build reference ](https://docs.docker.com/engine/reference/commandline/image_build/) docker build 参考
- [docker image tag reference](https://docs.docker.com/engine/reference/commandline/image_tag/) docker image tag 参考
- [docker push reference](https://docs.docker.com/engine/reference/commandline/image_push/) docker push 参考
- [What is a registry? 什么是注册表？]({{< ref "/get-started/Dockerconcepts/Thebasics/Whatisaregistry" >}})

## 接下来 Next steps

Now that you have learned about building and publishing images, it's time to learn how to speed up the build process using the Docker build cache.

​	现在您已经了解了构建和发布镜像的知识，是时候学习如何使用 Docker 构建缓存来加快构建过程了。

[Using the build cache 使用构建缓存]({{< ref "/get-started/Dockerconcepts/Buildingimages/Usingthebuildcache" >}})
