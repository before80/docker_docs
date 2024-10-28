+++
title = "替代容器运行时"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/daemon/alternative-runtimes/](https://docs.docker.com/engine/daemon/alternative-runtimes/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Alternative container runtimes - 替代容器运行时

Docker Engine uses containerd for managing the container lifecycle, which includes creating, starting, and stopping containers. By default, containerd uses runc as its container runtime.

​	Docker 引擎使用 containerd 管理容器生命周期，包括创建、启动和停止容器。默认情况下，containerd 使用 runc 作为其容器运行时。

## 我可以使用哪些运行时？ What runtimes can I use?

You can use any runtime that implements the containerd [shim API](https://github.com/containerd/containerd/blob/main/core/runtime/v2/README.md). Such runtimes ship with a containerd shim, and you can use them without any additional configuration. See [Use containerd shims](https://docs.docker.com/engine/daemon/alternative-runtimes/#use-containerd-shims).

​	您可以使用任何实现了 containerd [shim API](https://github.com/containerd/containerd/blob/main/core/runtime/v2/README.md) 的运行时。这类运行时带有 containerd 的 shim，无需额外配置即可使用。请参阅 [使用 containerd shim](https://docs.docker.com/engine/daemon/alternative-runtimes/#use-containerd-shims)。

Examples of runtimes that implement their own containerd shims include:

​	以下是实现了其自身 containerd shim 的运行时示例：

- [Wasmtime](https://wasmtime.dev/)

- [gVisor](https://github.com/google/gvisor)
- [Kata Containers](https://katacontainers.io/)

You can also use runtimes designed as drop-in replacements for runc. Such runtimes depend on the runc containerd shim for invoking the runtime binary. You must manually register such runtimes in the daemon configuration.

​	您还可以使用设计为 runc 插入替代品的运行时。这类运行时依赖 runc 的 containerd shim 来调用运行时二进制文件。您必须在守护进程配置中手动注册此类运行时。

[youki](https://github.com/containers/youki) is one example of a runtime that can function as a runc drop-in replacement. Refer to the [youki example](https://docs.docker.com/engine/daemon/alternative-runtimes/#youki) explaining the setup.

​	[youki](https://github.com/containers/youki) 就是可以充当 runc 插入替代品的运行时示例。请参阅 [youki 示例](https://docs.docker.com/engine/daemon/alternative-runtimes/#youki) 了解设置方法。

## Use containerd shims

containerd shims let you use alternative runtimes without having to change the configuration of the Docker daemon. To use a containerd shim, install the shim binary on `PATH` on the system where the Docker daemon is running.

​	containerd shim 允许您使用替代运行时，而无需更改 Docker 守护进程的配置。要使用 containerd shim，将 shim 二进制文件安装在 Docker 守护进程运行的系统上的 `PATH` 路径中。

To use a shim with `docker run`, specify the fully qualified name of the runtime as the value to the `--runtime` flag:

​	要在 `docker run` 中使用 shim，请将运行时的全限定名称作为 `--runtime` 标志的值指定：



```console
$ docker run --runtime io.containerd.kata.v2 hello-world
```

### 在未安装到 PATH 的情况下使用 containerd shim - Use a containerd shim without installing on PATH

You can use a shim without installing it on `PATH`, in which case you need to register the shim in the daemon configuration as follows:

​	您可以在未将 shim 安装到 `PATH` 的情况下使用它，在这种情况下，您需要在守护进程配置中注册该 shim，如下所示：



```json
{
  "runtimes": {
    "foo": {
      "runtimeType": "/path/to/containerd-shim-foobar-v1"
    }
  }
}
```

To use the shim, specify the name that you assigned to it:

​	要使用该 shim，指定您分配给它的名称：

```console
$ docker run --runtime foo hello-world
```

### 配置 shim - Configure shims

If you need to pass additional configuration for a containerd shim, you can use the `runtimes` option in the daemon configuration file.

​	如果需要为 containerd shim 传递其他配置，可以在守护进程配置文件中使用 `runtimes` 选项。

1. Edit the daemon configuration file by adding a `runtimes` entry for the shim you want to configure. 通过添加 `runtimes` 条目来编辑守护进程配置文件以配置所需的 shim。

   - Specify the fully qualified name for the runtime in `runtimeType` key 在 `runtimeType` 键中指定运行时的全限定名称
   - Add your runtime configuration under the `options` key 在 `options` 键下添加运行时配置

   

   ```json
   {
     "runtimes": {
       "gvisor": {
         "runtimeType": "io.containerd.runsc.v1",
         "options": {
           "TypeUrl": "io.containerd.runsc.v1.options",
           "ConfigPath": "/etc/containerd/runsc.toml"
         }
       }
     }
   }
   ```

2. Reload the daemon's configuration.

   重新加载守护进程配置。

   ```console
   # systemctl reload docker
   ```

3. Use the customized runtime using the `--runtime` flag for `docker run`.

   使用 `docker run` 的 `--runtime` 标志使用自定义的运行时。

   ```console
   $ docker run --runtime gvisor hello-world
   ```

For more information about the configuration options for containerd shims, see [Configure containerd shims](https://docs.docker.com/reference/cli/dockerd/#configure-containerd-shims).

​	有关 containerd shim 配置选项的更多信息，请参阅 [配置 containerd shim](https://docs.docker.com/reference/cli/dockerd/#configure-containerd-shims)。

## 示例 Examples

The following examples show you how to set up and use alternative container runtimes with Docker Engine.

​	以下示例演示如何在 Docker 引擎中设置和使用替代容器运行时。

- [youki](#youki)

- [Wasmtime](#wasmtime)

### youki

youki is a container runtime written in Rust. youki claims to be faster and use less memory than runc, making it a good choice for resource-constrained environments.

​	youki 是一个用 Rust 编写的容器运行时。youki 声称比 runc 更快且占用更少的内存，非常适合资源受限的环境。

youki functions as a drop-in replacement for runc, meaning it relies on the runc shim to invoke the runtime binary. When you register runtimes acting as runc replacements, you configure the path to the runtime executable, and optionally a set of runtime arguments. For more information, see [Configure runc drop-in replacements](https://docs.docker.com/reference/cli/dockerd/#configure-runc-drop-in-replacements).

​	youki 作为 runc 的插入替代品工作，这意味着它依赖 runc shim 调用运行时二进制文件。注册作为 runc 替代品的运行时时，您需配置运行时可执行文件的路径，并可选地设置一组运行时参数。详细信息请参阅 [配置 runc 插入替代品](https://docs.docker.com/reference/cli/dockerd/#configure-runc-drop-in-replacements)。

To add youki as a container runtime:

​	将 youki 添加为容器运行时的步骤：

1. Install youki and its dependencies. 安装 youki 及其依赖项。

   For instructions, refer to the [official setup guide](https://containers.github.io/youki/user/basic_setup.html).

   ​	有关说明，请参阅 [官方设置指南](https://containers.github.io/youki/user/basic_setup.html)。

2. Register youki as a runtime for Docker by editing the Docker daemon configuration file, located at `/etc/docker/daemon.json` by default. 将 youki 注册为 Docker 的运行时，编辑位于 `/etc/docker/daemon.json` 的 Docker 守护进程配置文件。

   The `path` key should specify the path to wherever you installed youki.

   `path` 键应指定您安装 youki 的路径。

   ```console
   # cat > /etc/docker/daemon.json <<EOF
   {
     "runtimes": {
       "youki": {
         "path": "/usr/local/bin/youki"
       }
     }
   }
   EOF
   ```

3. Reload the daemon's configuration.

   重新加载守护进程配置。

   ```console
   # systemctl reload docker
   ```

Now you can run containers that use youki as a runtime.

​	现在您可以运行使用 youki 作为运行时的容器。

```console
$ docker run --rm --runtime youki hello-world
```

### Wasmtime

Wasmtime is a [Bytecode Alliance](https://bytecodealliance.org/) project, and a Wasm runtime that lets you run Wasm containers. Running Wasm containers with Docker provides two layers of security. You get all the benefits from container isolation, plus the added sandboxing provided by the Wasm runtime environment.

​	Wasmtime 是一个 [Bytecode Alliance](https://bytecodealliance.org/) 项目，是一个允许运行 Wasm 容器的 Wasm 运行时。使用 Docker 运行 Wasm 容器提供了双重安全性。除了容器隔离的所有好处，还增加了 Wasm 运行时环境提供的沙箱保护。

To add Wasmtime as a container runtime, follow these steps:

​	将 Wasmtime 添加为容器运行时的步骤：

1. Turn on the [containerd image store]({{< ref "/manuals/DockerEngine/Storage/containerdimagestore" >}}) feature in the daemon configuration file. 在守护进程配置文件中启用 containerd 镜像存储 功能。

   > **Note**
   >
   > 
   >
   > This is an experimental feature.
   >
   > ​	这是一个实验性功能。

   

   ```json
   {
     "features": {
       "containerd-snapshotter": true
     }
   }
   ```

2. Restart the Docker daemon.

   重启 Docker 守护进程。

   ```console
   # systemctl restart docker
   ```

3. Install the Wasmtime containerd shim on `PATH`. 将 Wasmtime 的 containerd shim 安装到 `PATH`。

   The following command Dockerfile builds the Wasmtime binary from source and exports it to `./containerd-shim-wasmtime-v1`.

   以下命令 Dockerfile 从源代码构建 Wasmtime 二进制文件并导出到 `./containerd-shim-wasmtime-v1`。

   ```console
   $ docker build --output . - <<EOF
   FROM rust:latest as build
   RUN cargo install \
       --git https://github.com/containerd/runwasi.git \
       --bin containerd-shim-wasmtime-v1 \
       --root /out \
       containerd-shim-wasmtime
   FROM scratch
   COPY --from=build /out/bin /
   EOF
   ```

   Put the binary in a directory on `PATH`.

   将该二进制文件放置在 `PATH` 的某个目录中。

   ```console
   $ mv ./containerd-shim-wasmtime-v1 /usr/local/bin
   ```

Now you can run containers that use Wasmtime as a runtime.

​	现在您可以运行使用 Wasmtime 作为运行时的容器。

```console
$ docker run --rm \
 --runtime io.containerd.wasmtime.v1 \
 --platform wasi/wasm32 \
 michaelirwin244/wasm-example
```

## 相关信息 Related information

- To learn more about the configuration options for container runtimes, see [Configure container runtimes]({{< ref "/reference/CLIreference/dockerd#runtime-options">}}).
  - 了解更多容器运行时配置选项，请参阅 [配置容器运行时]({{< ref "/reference/CLIreference/dockerd#runtime-options">}})。
- You can configure which runtime that the daemon should use as its default. Refer to [Configure the default container runtime]({{< ref "/reference/CLIreference/dockerd#configure-the-default-container-runtime">}}).
  - 您可以配置守护进程使用的默认运行时。请参考 [配置默认容器运行时]({{< ref "/reference/CLIreference/dockerd#configure-the-default-container-runtime">}})。
