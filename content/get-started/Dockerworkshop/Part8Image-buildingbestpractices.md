+++
title = "Part 8: 镜像构建最佳实践"
date = 2024-10-23T14:54:35+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/get-started/workshop/09_image_best/](https://docs.docker.com/get-started/workshop/09_image_best/)
>
> 收录该文档的时间：`2024-10-23T14:54:35+08:00`

# Image-building best practices - 镜像构建最佳实践

## 镜像分层 Image layering

Using the `docker image history` command, you can see the command that was used to create each layer within an image.

​	使用 `docker image history` 命令，你可以查看镜像中每一层的创建命令。

1. Use the `docker image history` command to see the layers in the `getting-started` image you created. 使用 `docker image history` 命令查看你创建的 `getting-started` 镜像中的各层信息。

   

   ```console
   $ docker image history getting-started
   ```

   You should get output that looks something like the following.

   ​	输出可能如下所示：

   ```plaintext
   IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
   a78a40cbf866        18 seconds ago      /bin/sh -c #(nop)  CMD ["node" "src/index.j…    0B                  
   f1d1808565d6        19 seconds ago      /bin/sh -c yarn install --production            85.4MB              
   a2c054d14948        36 seconds ago      /bin/sh -c #(nop) COPY dir:5dc710ad87c789593…   198kB               
   9577ae713121        37 seconds ago      /bin/sh -c #(nop) WORKDIR /app                  0B                  
   b95baba1cfdb        13 days ago         /bin/sh -c #(nop)  CMD ["node"]                 0B                  
   <missing>           13 days ago         /bin/sh -c #(nop)  ENTRYPOINT ["docker-entry…   0B                  
   <missing>           13 days ago         /bin/sh -c #(nop) COPY file:238737301d473041…   116B                
   <missing>           13 days ago         /bin/sh -c apk add --no-cache --virtual .bui…   5.35MB              
   <missing>           13 days ago         /bin/sh -c #(nop)  ENV YARN_VERSION=1.21.1      0B                  
   <missing>           13 days ago         /bin/sh -c addgroup -g 1000 node     && addu…   74.3MB              
   <missing>           13 days ago         /bin/sh -c #(nop)  ENV NODE_VERSION=12.14.1     0B                  
   <missing>           13 days ago         /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B                  
   <missing>           13 days ago         /bin/sh -c #(nop) ADD file:e69d441d729412d24…   5.59MB   
   ```

   Each of the lines represents a layer in the image. The display here shows the base at the bottom with the newest layer at the top. Using this, you can also quickly see the size of each layer, helping diagnose large images.

   ​	每行表示镜像中的一层，最底层为基础镜像层，最上层为最新的镜像层。通过此输出，你可以快速了解每层的大小，以帮助诊断大型镜像。

2. You'll notice that several of the lines are truncated. If you add the `--no-trunc` flag, you'll get the full output. 如果添加 `--no-trunc` 参数，可以查看完整的输出。

   

   ```console
   $ docker image history --no-trunc getting-started
   ```

## 分层缓存 Layer caching

Now that you've seen the layering in action, there's an important lesson to learn to help decrease build times for your container images. Once a layer changes, all downstream layers have to be recreated as well.

​	你已经了解了镜像分层的工作原理，接下来是一个关于减少构建时间的重要提示：一旦某一层发生变化，之后的所有层都必须重新创建。

Look at the following Dockerfile you created for the getting started app.

​	看看你为入门应用创建的以下 Dockerfile。

```dockerfile
# syntax=docker/dockerfile:1
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
```

Going back to the image history output, you see that each command in the Dockerfile becomes a new layer in the image. You might remember that when you made a change to the image, the yarn dependencies had to be reinstalled. It doesn't make much sense to ship around the same dependencies every time you build.

​	回顾镜像历史记录输出，你会看到 Dockerfile 中的每个命令都成为镜像中的一个新层。你可能还记得，当你对镜像进行更改时，需要重新安装 yarn 依赖。每次构建时都传输相同的依赖显然没有多大意义。

To fix it, you need to restructure your Dockerfile to help support the caching of the dependencies. For Node-based applications, those dependencies are defined in the `package.json` file. You can copy only that file in first, install the dependencies, and then copy in everything else. Then, you only recreate the yarn dependencies if there was a change to the `package.json`.

​	为了解决这个问题，你需要重构 Dockerfile 来帮助支持依赖的缓存。对于基于 Node 的应用程序，这些依赖在 `package.json` 文件中定义。你可以首先只复制该文件，安装依赖，然后再复制其他所有文件。这样，只有在 `package.json` 发生变化时才重新创建 yarn 依赖。

1. Update the Dockerfile to copy in the `package.json` first, install dependencies, and then copy everything else in. 更新 Dockerfile，先复制 `package.json`，安装依赖，然后复制其他所有文件。

   

   ```dockerfile
   # syntax=docker/dockerfile:1
   FROM node:18-alpine
   WORKDIR /app
   COPY package.json yarn.lock ./
   RUN yarn install --production
   COPY . .
   CMD ["node", "src/index.js"]
   ```

2. Build a new image using `docker build`. 使用 `docker build` 构建新的镜像。

   

   ```console
   $ docker build -t getting-started .
   ```

   You should see output like the following.

   ​	你应该看到以下输出。

   ```plaintext
   [+] Building 16.1s (10/10) FINISHED
   => [internal] load build definition from Dockerfile
   => => transferring dockerfile: 175B
   => [internal] load .dockerignore
   => => transferring context: 2B
   => [internal] load metadata for docker.io/library/node:18-alpine
   => [internal] load build context
   => => transferring context: 53.37MB
   => [1/5] FROM docker.io/library/node:18-alpine
   => CACHED [2/5] WORKDIR /app
   => [3/5] COPY package.json yarn.lock ./
   => [4/5] RUN yarn install --production
   => [5/5] COPY . .
   => exporting to image
   => => exporting layers
   => => writing image     sha256:d6f819013566c54c50124ed94d5e66c452325327217f4f04399b45f94e37d25
   => => naming to docker.io/library/getting-started
   ```

3. Now, make a change to the `src/static/index.html` file. For example, change the `<title>` to "The Awesome Todo App". 现在，对 `src/static/index.html` 文件做出修改。例如，将 `<title>` 改为 "The Awesome Todo App"。

4. Build the Docker image now using `docker build -t getting-started .` again. This time, your output should look a little different. 再次使用 `docker build -t getting-started .` 构建 Docker 镜像。这次，你的输出应该有所不同。

   

   ```plaintext
   [+] Building 1.2s (10/10) FINISHED
   => [internal] load build definition from Dockerfile
   => => transferring dockerfile: 37B
   => [internal] load .dockerignore
   => => transferring context: 2B
   => [internal] load metadata for docker.io/library/node:18-alpine
   => [internal] load build context
   => => transferring context: 450.43kB
   => [1/5] FROM docker.io/library/node:18-alpine
   => CACHED [2/5] WORKDIR /app
   => CACHED [3/5] COPY package.json yarn.lock ./
   => CACHED [4/5] RUN yarn install --production
   => [5/5] COPY . .
   => exporting to image
   => => exporting layers
   => => writing image     sha256:91790c87bcb096a83c2bd4eb512bc8b134c757cda0bdee4038187f98148e2eda
   => => naming to docker.io/library/getting-started
   ```

   First off, you should notice that the build was much faster. And, you'll see that several steps are using previously cached layers. Pushing and pulling this image and updates to it will be much faster as well.
   
   ​	首先，你会注意到构建速度大大加快了。并且，你会看到几个步骤使用了之前缓存的层。推送和拉取这个镜像及其更新也会更快。

## 多阶段构建 Multi-stage builds

Multi-stage builds are an incredibly powerful tool to help use multiple stages to create an image. There are several advantages for them:

​	多阶段构建是一个非常强大的工具，可以帮助使用多个阶段来创建镜像。它们有几个优点：

- Separate build-time dependencies from runtime dependencies 将构建时依赖与运行时依赖分开

- Reduce overall image size by shipping only what your app needs to run 通过只传输应用运行所需的内容，减少总镜像大小

### Maven/Tomcat 示例 Maven/Tomcat example

When building Java-based applications, you need a JDK to compile the source code to Java bytecode. However, that JDK isn't needed in production. Also, you might be using tools like Maven or Gradle to help build the app. Those also aren't needed in your final image. Multi-stage builds help.

​	在构建基于 Java 的应用程序时，你需要 JDK 来将源代码编译为 Java 字节码。然而，生产环境中不需要 JDK。你可能还使用了像 Maven 或 Gradle 这样的工具来帮助构建应用程序。这些工具在最终镜像中也不是必需的。多阶段构建有帮助。

```dockerfile
# syntax=docker/dockerfile:1
FROM maven AS build
WORKDIR /app
COPY . .
RUN mvn package

FROM tomcat
COPY --from=build /app/target/file.war /usr/local/tomcat/webapps 
```

In this example, you use one stage (called `build`) to perform the actual Java build using Maven. In the second stage (starting at `FROM tomcat`), you copy in files from the `build` stage. The final image is only the last stage being created, which can be overridden using the `--target` flag.

​	在这个例子中，你使用一个阶段（称为 `build`）来执行 Maven 的实际 Java 构建。在第二阶段（从 `FROM tomcat` 开始），你从 `build` 阶段复制文件。最终镜像只是创建的最后一个阶段，可以使用 `--target` 标志进行覆盖。

### React 示例 React example

When building React applications, you need a Node environment to compile the JS code (typically JSX), SASS stylesheets, and more into static HTML, JS, and CSS. If you aren't doing server-side rendering, you don't even need a Node environment for your production build. You can ship the static resources in a static nginx container.

​	在构建 React 应用程序时，你需要一个 Node 环境来编译 JS 代码（通常是 JSX）、SASS 样式表等到静态 HTML、JS 和 CSS。如果你不进行服务器端渲染，你甚至不需要为你的生产构建提供 Node 环境。你可以在一个静态的 nginx 容器中传输静态资源。

```dockerfile
# syntax=docker/dockerfile:1
FROM node:18 AS build
WORKDIR /app
COPY package* yarn.lock ./
RUN yarn install
COPY public ./public
COPY src ./src
RUN yarn run build

FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
```

In the previous Dockerfile example, it uses the `node:18` image to perform the build (maximizing layer caching) and then copies the output into an nginx container.

​	在前面的 Dockerfile 示例中，它使用 `node:18` 镜像执行构建（最大化层缓存），然后将输出复制到一个 nginx 容器中。

## 总结 Summary

In this section, you learned a few image building best practices, including layer caching and multi-stage builds.

​	在本节中，你学到了一些构建镜像的最佳实践，包括层缓存和多阶段构建。

Related information: 相关信息：

- [Dockerfile reference]({{< ref "/reference/Dockerfilereference" >}})
- [Dockerfile best practices]({{< ref "/manuals/DockerBuild/Building/Bestpractices" >}})

## 接下来 Next steps

In the next section, you'll learn about additional resources you can use to continue learning about containers.

​	在下一节中，你将学习可以用来继续了解容器的其他资源。

[What next]({{< ref "/get-started/Dockerworkshop/Part9Whatnext" >}})
