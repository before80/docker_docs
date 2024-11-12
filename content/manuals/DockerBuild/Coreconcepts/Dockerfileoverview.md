+++
title = "Dockerfile 概述"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/concepts/dockerfile/](https://docs.docker.com/build/concepts/dockerfile/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Dockerfile overview - Dockerfile 概述

## Dockerfile

It all starts with a Dockerfile.

​	一切都从 Dockerfile 开始。

Docker builds images by reading the instructions from a Dockerfile. A Dockerfile is a text file containing instructions for building your source code. The Dockerfile instruction syntax is defined by the specification reference in the [Dockerfile reference]({{< ref "/reference/Dockerfilereference" >}}).

​	Docker 通过读取 Dockerfile 中的指令来构建镜像。Dockerfile 是一个包含构建源代码指令的文本文件。Dockerfile 指令语法由 Dockerfile 参考中的规范定义。

Here are the most common types of instructions:

​	以下是最常用的指令类型：

| Instruction                                                  | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`FROM `](https://docs.docker.com/reference/dockerfile/#from) | 定义镜像的基础。 Defines a base for your image.              |
| [`RUN `](https://docs.docker.com/reference/dockerfile/#run)  | 在当前镜像顶部的新层中执行命令并提交结果。`RUN` 还有用于运行命令的 shell 形式。 Executes any commands in a new layer on top of the current image and commits the result. `RUN` also has a shell form for running commands. |
| [`WORKDIR `](https://docs.docker.com/reference/dockerfile/#workdir) | 为后续的 `RUN`、`CMD`、`ENTRYPOINT`、`COPY` 和 `ADD` 指令设置工作目录。 Sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, and `ADD` instructions that follow it in the Dockerfile. |
| [`COPY  `](https://docs.docker.com/reference/dockerfile/#copy) | 将新的文件或目录从 `<src>` 复制到容器的文件系统中指定的 `<dest>` 路径。 Copies new files or directories from `<src>` and adds them to the filesystem of the container at the path `<dest>`. |
| [`CMD `](https://docs.docker.com/reference/dockerfile/#cmd)  | 定义容器启动时运行的默认程序。每个 Dockerfile 只有一个 `CMD`，多个 `CMD` 存在时只有最后一个 `CMD` 生效。 Lets you define the default program that is run once you start the container based on this image. Each Dockerfile only has one `CMD`, and only the last `CMD` instance is respected when multiple exist. |

Dockerfiles are crucial inputs for image builds and can facilitate automated, multi-layer image builds based on your unique configurations. Dockerfiles can start simple and grow with your needs to support more complex scenarios.

​	Dockerfile 是镜像构建的关键输入，可以根据您的独特配置支持自动化的多层镜像构建。Dockerfile 可以从简单开始，随着需求增长支持更复杂的场景。

### Filename

The default filename to use for a Dockerfile is `Dockerfile`, without a file extension. Using the default name allows you to run the `docker build` command without having to specify additional command flags.

​	Dockerfile 的默认文件名是 `Dockerfile`，没有文件扩展名。使用默认名称可以让您运行 `docker build` 命令时无需指定额外的命令标志。

Some projects may need distinct Dockerfiles for specific purposes. A common convention is to name these `<something>.Dockerfile`. You can specify the Dockerfile filename using the `--file` flag for the `docker build` command. Refer to the [`docker build` CLI reference](https://docs.docker.com/reference/cli/docker/buildx/build/#file) to learn about the `--file` flag.

​	一些项目可能需要特定用途的不同 Dockerfile。一个常见的约定是将这些文件命名为 `<something>.Dockerfile`。您可以使用 `docker build` 命令的 `--file` 标志指定 Dockerfile 文件名。有关 `--file` 标志的详细信息，请参阅 [`docker build` CLI 参考]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxbuild#指定-dockerfile--f---file-specify-a-dockerfile--f---file">}})。

> **Note**
>
> 
>
> We recommend using the default (`Dockerfile`) for your project's primary Dockerfile.
>
> ​	我们建议使用默认的 `Dockerfile` 作为项目的主 Dockerfile。

## Docker images

Docker images consist of layers. Each layer is the result of a build instruction in the Dockerfile. Layers are stacked sequentially, and each one is a delta representing the changes applied to the previous layer.

​	Docker 镜像由多层组成。每一层都是 Dockerfile 中构建指令的结果。层按顺序堆叠，每一层都是表示对前一层的更改的增量。

### Example

Here's what a typical workflow for building applications with Docker looks like.

​	以下是使用 Docker 构建应用程序的典型工作流程。

The following example code shows a small "Hello World" application written in Python, using the Flask framework.

​	以下示例代码展示了一个使用 Flask 框架编写的小型 “Hello World” 应用程序：





```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"
```

In order to ship and deploy this application without Docker Build, you would need to make sure that:

​	若要在没有 Docker Build 的情况下发布和部署此应用程序，您需要确保：

- The required runtime dependencies are installed on the server
  - 服务器上安装了所需的运行时依赖项

- The Python code gets uploaded to the server's filesystem
  - Python 代码被上传到服务器的文件系统

- The server starts your application, using the necessary parameters
  - 服务器使用必要的参数启动您的应用程序


The following Dockerfile creates a container image, which has all the dependencies installed and that automatically starts your application.

​	以下 Dockerfile 创建了一个包含所有已安装依赖项并自动启动应用程序的容器镜像：

```dockerfile
# syntax=docker/dockerfile:1
FROM ubuntu:22.04

# install app dependencies
RUN apt-get update && apt-get install -y python3 python3-pip
RUN pip install flask==3.0.*

# install app
COPY hello.py /

# final configuration
ENV FLASK_APP=hello
EXPOSE 8000
CMD ["flask", "run", "--host", "0.0.0.0", "--port", "8000"]
```

Here's a breakdown of what this Dockerfile does:

​	以下是此 Dockerfile 的分解说明：

- [Dockerfile syntax](https://docs.docker.com/build/concepts/dockerfile/#dockerfile-syntax)
- [Base image](https://docs.docker.com/build/concepts/dockerfile/#base-image)
- [Environment setup](https://docs.docker.com/build/concepts/dockerfile/#environment-setup)
- [Comments](https://docs.docker.com/build/concepts/dockerfile/#comments)
- [Installing dependencies](https://docs.docker.com/build/concepts/dockerfile/#installing-dependencies)
- [Copying files](https://docs.docker.com/build/concepts/dockerfile/#copying-files)
- [Setting environment variables](https://docs.docker.com/build/concepts/dockerfile/#setting-environment-variables)
- [Exposed ports](https://docs.docker.com/build/concepts/dockerfile/#exposed-ports)
- [Starting the application](https://docs.docker.com/build/concepts/dockerfile/#starting-the-application)

### Dockerfile 语法 Dockerfile syntax

The first line to add to a Dockerfile is a [`# syntax` parser directive](https://docs.docker.com/reference/dockerfile/#syntax). While optional, this directive instructs the Docker builder what syntax to use when parsing the Dockerfile, and allows older Docker versions with [BuildKit enabled](https://docs.docker.com/build/buildkit/#getting-started) to use a specific [Dockerfile frontend]({{< ref "/manuals/DockerBuild/BuildKit/CustomDockerfilesyntax" >}}) before starting the build. [Parser directives](https://docs.docker.com/reference/dockerfile/#parser-directives) must appear before any other comment, whitespace, or Dockerfile instruction in your Dockerfile, and should be the first line in Dockerfiles.

​	添加到 Dockerfile 的第一行是 [`# syntax` 解析器指令](https://docs.docker.com/reference/dockerfile/#syntax)。虽然是可选的，但该指令告诉 Docker 构建器在解析 Dockerfile 时使用哪种语法，并允许启用了 [BuildKit](https://docs.docker.com/build/buildkit/#getting-started) 的旧版本 Docker 使用特定的 [Dockerfile 前端]({{< ref "/manuals/DockerBuild/BuildKit/CustomDockerfilesyntax" >}}) 在开始构建之前。[解析器指令](https://docs.docker.com/reference/dockerfile/#parser-directives)必须出现在 Dockerfile 中的其他注释、空白或指令之前，并且应作为 Dockerfile 的第一行。





```dockerfile
# syntax=docker/dockerfile:1
```

> **Tip**
>
> 
>
> We recommend using `docker/dockerfile:1`, which always points to the latest release of the version 1 syntax. BuildKit automatically checks for updates of the syntax before building, making sure you are using the most current version.
>
> ​	我们建议使用 `docker/dockerfile:1`，它始终指向最新的版本 1 语法。BuildKit 会在构建前自动检查语法更新，以确保您使用的是最新版本。

### 基础镜像 Base image

The line following the syntax directive defines what base image to use:

​	语法指令后的行定义了要使用的基础镜像：

```dockerfile
FROM ubuntu:22.04
```

The [`FROM` instruction](https://docs.docker.com/reference/dockerfile/#from) sets your base image to the 22.04 release of Ubuntu. All instructions that follow are executed in this base image: an Ubuntu environment. The notation `ubuntu:22.04`, follows the `name:tag` standard for naming Docker images. When you build images, you use this notation to name your images. There are many public images you can leverage in your projects, by importing them into your build steps using the Dockerfile `FROM` instruction.

​	[`FROM` 指令]({{< ref "/reference/Dockerfilereference#from">}}) 将您的基础镜像设置为 Ubuntu 的 22.04 版本。后续的所有指令都将在此基础镜像（Ubuntu 环境）中执行。`ubuntu:22.04` 采用 `name:tag` 的镜像命名标准。您可以在构建步骤中使用 Dockerfile `FROM` 指令导入许多公共镜像，将它们用于项目中。

[Docker Hub](https://hub.docker.com/search?image_filter=official&q=&type=image) contains a large set of official images that you can use for this purpose.

​	[Docker Hub](https://hub.docker.com/search?image_filter=official&q=&type=image) 包含大量可供此用途使用的官方镜像。

### 环境设置 Environment setup

The following line executes a build command inside the base image.

​	以下行在基础镜像中执行构建命令。

```dockerfile
# install app dependencies
# 安装应用程序依赖项
RUN apt-get update && apt-get install -y python3 python3-pip
```

This [`RUN` instruction](https://docs.docker.com/reference/dockerfile/#run) executes a shell in Ubuntu that updates the APT package index and installs Python tools in the container.

​	此 [`RUN` 指令]({{< ref "/reference/Dockerfilereference#run">}}) 在 Ubuntu 中执行一个 shell，用于更新 APT 包索引并在容器中安装 Python 工具。

### Comments

Note the `# install app dependencies` line. This is a comment. Comments in Dockerfiles begin with the `#` symbol. As your Dockerfile evolves, comments can be instrumental to document how your Dockerfile works for any future readers and editors of the file, including your future self!

​	注意 `# 安装应用程序依赖项` 行。这是一条注释。Dockerfile 中的注释以 `#` 符号开头。随着 Dockerfile 的演变，注释对于任何未来的文件读者和编辑者，包括您自己，都是了解文件如何工作的关键。

> **Note**
>
> 
>
> You might've noticed that comments are denoted using the same symbol as the [syntax directive](https://docs.docker.com/build/concepts/dockerfile/#dockerfile-syntax) on the first line of the file. The symbol is only interpreted as a directive if the pattern matches a directive and appears at the beginning of the Dockerfile. Otherwise, it's treated as a comment.
>
> ​	您可能已经注意到，注释使用的符号与文件第一行的 [语法指令](#dockerfile-语法-dockerfile-syntax) 相同。只有当模式匹配指令并出现在 Dockerfile 开头时，该符号才会被解释为指令，否则它将被视为注释。

### 安装依赖项 Installing dependencies

The second `RUN` instruction installs the `flask` dependency required by the Python application.

​	第二个 `RUN` 指令安装了 Python 应用程序所需的 `flask` 依赖项。

```dockerfile
RUN pip install flask==3.0.*
```

A prerequisite for this instruction is that `pip` is installed into the build container. The first `RUN` command installs `pip`, which ensures that we can use the command to install the flask web framework.

​	此指令的前提条件是 `pip` 已安装在构建容器中。第一个 `RUN` 命令安装了 `pip`，确保我们可以使用该命令安装 flask web 框架。

### Copying files

The next instruction uses the [`COPY` instruction](https://docs.docker.com/reference/dockerfile/#copy) to copy the `hello.py` file from the local build context into the root directory of our image.

​	下一条指令使用 [`COPY` 指令]({{< ref "/reference/Dockerfilereference#copy">}}) 将 `hello.py` 文件从本地构建上下文复制到镜像的根目录中。

```dockerfile
COPY hello.py /
```

A [build context]({{< ref "/manuals/DockerBuild/Coreconcepts/Buildcontext" >}}) is the set of files that you can access in Dockerfile instructions such as `COPY` and `ADD`.

​	[构建上下文]({{< ref "/manuals/DockerBuild/Coreconcepts/Buildcontext" >}})是可以在 Dockerfile 指令（如 `COPY` 和 `ADD`）中访问的文件集合。

After the `COPY` instruction, the `hello.py` file is added to the filesystem of the build container.

​	在 `COPY` 指令之后，`hello.py` 文件被添加到构建容器的文件系统中。

### 设置环境变量 Setting environment variables

If your application uses environment variables, you can set environment variables in your Docker build using the [`ENV` instruction](https://docs.docker.com/reference/dockerfile/#env).

​	如果您的应用程序使用环境变量，可以使用 [`ENV` 指令]({{< ref "/reference/Dockerfilereference#env">}}) 在 Docker 构建中设置环境变量。

```dockerfile
ENV FLASK_APP=hello
```

This sets a Linux environment variable we'll need later. Flask, the framework used in this example, uses this variable to start the application. Without this, flask wouldn't know where to find our application to be able to run it.

​	这设置了稍后需要的 Linux 环境变量。在此示例中使用的框架 Flask 使用此变量启动应用程序。否则，Flask 将不知道在哪里找到我们的应用程序以运行它。

### 暴露端口 Exposed ports

The [`EXPOSE` instruction](https://docs.docker.com/reference/dockerfile/#expose) marks that our final image has a service listening on port `8000`.

​	[`EXPOSE` 指令]({{< ref "/reference/Dockerfilereference#expose">}}) 指出最终镜像的服务在端口 `8000` 上监听。



```dockerfile
EXPOSE 8000
```

This instruction isn't required, but it is a good practice and helps tools and team members understand what this application is doing.

​	该指令不是必需的，但它是一个好的实践，有助于工具和团队成员理解该应用程序的行为。

### 启动应用程序 Starting the application

Finally, [`CMD` instruction](https://docs.docker.com/reference/dockerfile/#cmd) sets the command that is run when the user starts a container based on this image.

​	最后，[`CMD` 指令]({{< ref "/reference/Dockerfilereference#cmd">}}) 设置在基于此镜像启动容器时运行的命令。

```dockerfile
CMD ["flask", "run", "--host", "0.0.0.0", "--port", "8000"]
```

This command starts the flask development server listening on all addresses on port `8000`. The example here uses the "exec form" version of `CMD`. It's also possible to use the "shell form":

​	该命令启动 flask 开发服务器，监听所有地址的 8000 端口。此示例使用 `CMD` 的“exec 形式”。也可以使用“shell 形式”：

```dockerfile
CMD flask run --host 0.0.0.0 --port 8000
```

There are subtle differences between these two versions, for example in how they trap signals like `SIGTERM` and `SIGKILL`. For more information about these differences, see [Shell and exec form](https://docs.docker.com/reference/dockerfile/#shell-and-exec-form)

​	这两种形式之间存在细微差异，例如在处理 `SIGTERM` 和 `SIGKILL` 信号时的不同。有关这些差异的更多信息，请参阅 [Shell 和 exec 形式]({{< ref "/reference/Dockerfilereference#shell-形式-shell-form">}})。

## Building

To build a container image using the Dockerfile example from the [previous section](https://docs.docker.com/build/concepts/dockerfile/#example), you use the `docker build` command:

​	要使用上一节 [Dockerfile 示例](#example)构建容器镜像，使用 `docker build` 命令：

```console
$ docker build -t test:latest .
```

The `-t test:latest` option specifies the name and tag of the image.

​	`-t test:latest` 选项指定镜像的名称和标签。

The single dot (`.`) at the end of the command sets the [build context]({{< ref "/manuals/DockerBuild/Coreconcepts/Buildcontext" >}}) to the current directory. This means that the build expects to find the Dockerfile and the `hello.py` file in the directory where the command is invoked. If those files aren't there, the build fails.

​	命令末尾的单点 (`.`) 设置 [构建上下文]({{< ref "/manuals/DockerBuild/Coreconcepts/Buildcontext" >}})为当前目录。这意味着构建时期望在命令运行的目录中找到 Dockerfile 和 `hello.py` 文件。如果这些文件不存在，构建会失败。

After the image has been built, you can run the application as a container with `docker run`, specifying the image name:

​	镜像构建完成后，您可以使用 `docker run` 命令以容器形式运行该应用程序，并指定镜像名称：



```console
$ docker run -p 127.0.0.1:8000:8000 test:latest
```

This publishes the container's port 8000 to `http://localhost:8000` on the Docker host.

​	此命令将容器的 8000 端口发布到 Docker 主机的 `http://localhost:8000`。
