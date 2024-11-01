+++
title = "Wasm 工作负载 (Beta)"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/wasm/](https://docs.docker.com/desktop/wasm/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Wasm workloads (Beta) - Wasm 工作负载 (Beta)

**Beta**

The Wasm feature is currently in [Beta](https://docs.docker.com/release-lifecycle/#beta). We recommend that you do not use this feature in production environments as this feature may change or be removed from future releases.

​	Wasm 功能目前处于 [Beta](https://docs.docker.com/release-lifecycle/#beta) 阶段。建议不要在生产环境中使用此功能，因为该功能可能会发生更改或在未来版本中移除。

Wasm (short for WebAssembly) is a fast, light alternative to the Linux and Windows containers you’re using in Docker today (with [some tradeoffs](https://www.docker.com/blog/docker-wasm-technical-preview/)).

​	Wasm（WebAssembly 的缩写）是 Docker 中 Linux 和 Windows 容器的一种快速、轻量的替代方案（有 [一些权衡](https://www.docker.com/blog/docker-wasm-technical-preview/)）。

This page provides information about the new ability to run Wasm applications alongside your Linux containers in Docker.

​	本页提供有关在 Docker 中运行 Wasm 应用程序（与 Linux 容器并行）的信息。

## 启用 Wasm 工作负载 Turn on Wasm workloads

Wasm workloads require the [containerd image store]({{< ref "/manuals/DockerDesktop/containerdimagestore" >}}) feature to be turned on. If you’re not already using the containerd image store, then pre-existing images and containers will be inaccessible.

​	Wasm 工作负载需要启用 [containerd 镜像存储]({{< ref "/manuals/DockerDesktop/containerdimagestore" >}}) 功能。如果您还未使用 containerd 镜像存储，则现有的镜像和容器将无法访问。

1. Navigate to **Settings** in Docker Desktop. 在 Docker Desktop 中进入 **Settings**。
2. In the **General** tab, check **Use containerd for pulling and storing images**. 在 **General** 选项卡中，勾选 **Use containerd for pulling and storing images**。
3. Go to **Features in development** and check the **Enable Wasm** option. 前往 **Features in development**，勾选 **Enable Wasm** 选项。
4. Select **Apply & restart** to save the settings. 选择 **Apply & restart** 以保存设置。
5. In the confirmation dialog, select **Install** to install the Wasm runtimes. 在确认对话框中，选择 **Install** 以安装 Wasm 运行时。

Docker Desktop downloads and installs the following runtimes that you can use to run Wasm workloads:

​	Docker Desktop 将下载并安装以下运行时，可用于运行 Wasm 工作负载：

- `io.containerd.slight.v1`
- `io.containerd.spin.v2`
- `io.containerd.wasmedge.v1`
- `io.containerd.wasmtime.v1`
- `io.containerd.lunatic.v1`
- `io.containerd.wws.v1`
- `io.containerd.wasmer.v1`

## 使用示例 Usage examples

### 使用 `docker run` 运行 Wasm 应用程序 Running a Wasm application with `docker run`

The following `docker run` command starts a Wasm container on your system:

​	以下 `docker run` 命令在您的系统上启动一个 Wasm 容器：

```console
$ docker run \
  --runtime=io.containerd.wasmedge.v1 \
  --platform=wasi/wasm \
  secondstate/rust-example-hello
```

After running this command, you can visit http://localhost:8080/ to see the "Hello world" output from this example module.

​	运行此命令后，您可以访问 http://localhost:8080/ 查看该示例模块的 "Hello world" 输出。

If you are receiving an error message, see the [troubleshooting section](https://docs.docker.com/desktop/wasm/#troubleshooting) for help.

​	如果收到错误消息，请参见 [故障排除部分](https://docs.docker.com/desktop/wasm/#troubleshooting) 以获取帮助。

Note the `--runtime` and `--platform` flags used in this command:

​	请注意此命令中的 `--runtime` 和 `--platform` 标志：

- `--runtime=io.containerd.wasmedge.v1`: Informs the Docker engine that you want to use the Wasm containerd shim instead of the standard Linux container runtime
  - `--runtime=io.containerd.wasmedge.v1`：告知 Docker 引擎使用 Wasm 的 containerd shim，而非标准的 Linux 容器运行时。

- `--platform=wasi/wasm`: Specifies the architecture of the image you want to use. By leveraging a Wasm architecture, you don’t need to build separate images for the different machine architectures. The Wasm runtime takes care of the final step of converting the Wasm binary to machine instructions.
  - `--platform=wasi/wasm`：指定要使用的镜像架构。使用 Wasm 架构，无需为不同机器架构构建单独的镜像。Wasm 运行时会将 Wasm 二进制文件转换为机器指令。


### 使用 Docker Compose 运行 Wasm 应用程序 Running a Wasm application with Docker Compose

The same application can be run using the following Docker Compose file:

​	可以使用以下 Docker Compose 文件运行相同的应用程序：

```yaml
services:
  app:
    image: secondstate/rust-example-hello
    platform: wasi/wasm
    runtime: io.containerd.wasmedge.v1
```

Start the application using the normal Docker Compose commands:

​	使用常规的 Docker Compose 命令启动应用程序：



```console
$ docker compose up
```

### 使用 Wasm 运行多服务应用程序 Running a multi-service application with Wasm

Networking works the same as you expect with Linux containers, giving you the flexibility to combine Wasm applications with other containerized workloads, such as a database, in a single application stack.

​	网络功能与 Linux 容器相同，允许您将 Wasm 应用程序与其他容器化工作负载（如数据库）组合在一个应用程序堆栈中。

In the following example, the Wasm application leverages a MariaDB database running in a container.

​	以下示例中，Wasm 应用程序利用在容器中运行的 MariaDB 数据库。

1. Clone the repository.

   克隆存储库。

   ```console
   $ git clone https://github.com/second-state/microservice-rust-mysql.git
   Cloning into 'microservice-rust-mysql'...
   remote: Enumerating objects: 75, done.
   remote: Counting objects: 100% (75/75), done.
   remote: Compressing objects: 100% (42/42), done.
   remote: Total 75 (delta 29), reused 48 (delta 14), pack-reused 0
   Receiving objects: 100% (75/75), 19.09 KiB | 1.74 MiB/s, done.
   Resolving deltas: 100% (29/29), done.
   ```

2. Navigate into the cloned project and start the project using Docker Compose.

   进入克隆的项目，并使用 Docker Compose 启动项目。

   ```
   
   ```

   ```console
   $ cd microservice-rust-mysql
   $ docker compose up
   [+] Running 0/1
   ⠿ server Warning                                                                                                  0.4s
   [+] Building 4.8s (13/15)
   ...
   microservice-rust-mysql-db-1      | 2022-10-19 19:54:45 0 [Note] mariadbd: ready for connections.
   microservice-rust-mysql-db-1      | Version: '10.9.3-MariaDB-1:10.9.3+maria~ubu2204'  socket: '/run/mysqld/mysqld.sock'  port: 3306  mariadb.org binary distribution
   ```

   If you run `docker image ls` from another terminal window, you can see the Wasm image in your image store.

   ​	如果在另一个终端窗口运行 `docker image ls`，您可以在镜像存储中看到 Wasm 镜像。

   

   ```console
   $ docker image ls
   REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
   server       latest    2c798ddecfa1   2 minutes ago   3MB
   ```

   Inspecting the image shows the image has a `wasi/wasm` platform, a combination of OS and architecture:

   ​	检查镜像可以看到它的 `wasi/wasm` 平台，是操作系统和架构的组合：

   

   ```console
   $ docker image inspect server | grep -A 3 "Architecture"
           "Architecture": "wasm",
           "Os": "wasi",
           "Size": 3001146,
           "VirtualSize": 3001146,
   ```

3. Open the URL `http://localhost:8090` in a browser and create a few sample orders. All of these are interacting with the Wasm server.  在浏览器中打开 URL `http://localhost:8090` 并创建一些示例订单。所有这些都与 Wasm 服务器进行交互。

4. When you're all done, tear everything down by hitting `Ctrl+C` in the terminal you launched the application. 完成后，在启动应用的终端中按 `Ctrl+C` 键将其关闭。

### 构建和推送 Wasm 模块 Building and pushing a Wasm module

1. Create a Dockerfile that builds your Wasm application. 创建一个 Dockerfile 来构建您的 Wasm 应用。

   Exactly how to do this varies depending on the programming language you use.

   ​	具体做法取决于您使用的编程语言。

2. In a separate stage in your `Dockerfile`, extract the module and set it as the `ENTRYPOINT`.

   在 `Dockerfile` 中的一个单独阶段中，提取模块并将其设置为 `ENTRYPOINT`。

   ```dockerfile
   # syntax=docker/dockerfile:1
   FROM scratch
   COPY --from=build /build/hello_world.wasm /hello_world.wasm
   ENTRYPOINT [ "/hello_world.wasm" ]
   ```

3. Build and push the image specifying the `wasi/wasm` architecture. Buildx makes this easy to do in a single command.

   指定 `wasi/wasm` 架构来构建并推送镜像。使用 Buildx 可以在一条命令中轻松完成。

   ```console
   $ docker buildx build --platform wasi/wasm -t username/hello-world .
   ...
   => exporting to image                                                                             0.0s
   => => exporting layers                                                                            0.0s
   => => exporting manifest sha256:2ca02b5be86607511da8dc688234a5a00ab4d58294ab9f6beaba48ab3ba8de56  0.0s
   => => exporting config sha256:a45b465c3b6760a1a9fd2eda9112bc7e3169c9722bf9e77cf8c20b37295f954b    0.0s
   => => naming to docker.io/username/hello-world:latest                                            0.0s
   => => unpacking to docker.io/username/hello-world:latest                                         0.0s
   $ docker push username/hello-world
   ```

## 故障排查 Troubleshooting

This section contains instructions on how to resolve common issues.

​	本部分包含解决常见问题的说明。

### 未知的运行时指定 Unknown runtime specified

If you try to run a Wasm container without the [containerd image store]({{< ref "/manuals/DockerDesktop/containerdimagestore" >}}), an error similar to the following displays:

​	如果您尝试在未启用 [containerd 镜像存储]({{< ref "/manuals/DockerDesktop/containerdimagestore" >}})的情况下运行 Wasm 容器，则会出现类似以下的错误：

```text
docker: Error response from daemon: Unknown runtime specified io.containerd.wasmedge.v1.
```

[Turn on the containerd feature](https://docs.docker.com/desktop/containerd/#enable-the-containerd-image-store) in Docker Desktop settings and try again.

​	在 Docker Desktop 设置中[启用 containerd 功能](https://docs.docker.com/desktop/containerd/#enable-the-containerd-image-store)，然后重试。

### 无法启动 shim：无法解析运行时路径 Failed to start shim: failed to resolve runtime path

If you use an older version of Docker Desktop that doesn't support running Wasm workloads, you will see an error message similar to the following:

​	如果您使用不支持运行 Wasm 工作负载的旧版本 Docker Desktop，将会看到类似以下的错误消息：

```text
docker: Error response from daemon: failed to start shim: failed to resolve runtime path: runtime "io.containerd.wasmedge.v1" binary not installed "containerd-shim-wasmedge-v1": file does not exist: unknown.
```

Update your Docker Desktop to the latest version and try again.

​	更新您的 Docker Desktop 至最新版本，然后重试。

## 已知问题 Known issues

- Docker Compose may not exit cleanly when interrupted - Docker Compose 在被中断时可能无法正常退出

  - Workaround: Clean up `docker-compose` processes by sending them a SIGKILL (`killall -9 docker-compose`). 解决方法：通过发送 SIGKILL (`killall -9 docker-compose`) 来清理 `docker-compose` 进程。

- Pushes to Hub might give an error stating `server message: insufficient_scope: authorization failed`, even after logging in using Docker Desktop  即使已使用 Docker Desktop 登录，推送到 Hub 时可能会出现错误提示 `server message: insufficient_scope: authorization failed`

  - Workaround: Run `docker login` in the CLI 解决方法：在 CLI 中运行 `docker login`。

## 反馈 Feedback

Thanks for trying out Wasm workloads with Docker. Give feedback or report any bugs you may find through the issues tracker on the [public roadmap item](https://github.com/docker/roadmap/issues/426).

​	感谢您尝试使用 Docker 的 Wasm 工作负载。您可以通过[公共路线图项目](https://github.com/docker/roadmap/issues/426)中的问题追踪器提供反馈或报告任何您可能发现的错误。
