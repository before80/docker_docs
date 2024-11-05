+++
title = "docker init"
date = 2024-10-23T14:54:43+08:00
weight = 100
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/init/](https://docs.docker.com/reference/cli/docker/init/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker init

| Description | Creates Docker-related starter files for your project |
| :---------- | ----------------------------------------------------- |
| Usage       | `docker init [OPTIONS]`                               |

## Description

Initialize a project with the files necessary to run the project in a container.

​	初始化一个项目，生成运行在容器中所需的文件。

Docker Desktop provides the `docker init` CLI command. Run `docker init` in your project directory to be walked through the creation of the following files with sensible defaults for your project:

​	Docker Desktop 提供了 `docker init` CLI 命令。在项目目录中运行 `docker init`，系统会引导您创建以下文件，并根据您的项目提供合理的默认值：

- .dockerignore

- Dockerfile
- compose.yaml
- README.Docker.md

If any of the files already exist, a prompt appears and provides a warning as well as giving you the option to overwrite all the files. If `docker-compose.yaml` already exists instead of `compose.yaml`, `docker init` can overwrite it, using `docker-compose.yaml` as the name for the Compose file.

​	如果这些文件中有任何已存在的文件，系统会出现提示并给出警告，同时让您选择是否覆盖所有文件。如果已经存在 `docker-compose.yaml` 文件而非 `compose.yaml`，则 `docker init` 可以覆盖它，并使用 `docker-compose.yaml` 作为 Compose 文件的名称。

> **Warning**
>
> You can't recover overwritten files. To back up an existing file before selecting to overwrite it, rename the file or copy it to another directory.
>
> ​	覆盖的文件无法恢复。要在选择覆盖之前备份现有文件，请重命名文件或将其复制到另一个目录。

After running `docker init`, you can choose one of the following templates:

​	运行 `docker init` 后，您可以选择以下模板之一：

- ASP.NET Core: Suitable for an ASP.NET Core application.
  - ASP.NET Core：适合 ASP.NET Core 应用程序。

- Go: Suitable for a Go server application.
  - Go：适合 Go 服务应用程序。

- Java: suitable for a Java application that uses Maven and packages as an uber jar.
  - Java：适合使用 Maven 并打包为 uber jar 的 Java 应用程序。

- Node: Suitable for a Node server application.
  - Node：适合 Node 服务应用程序。

- PHP with Apache: Suitable for a PHP web application.
  - PHP with Apache：适合 PHP Web 应用程序。

- Python: Suitable for a Python server application.
  - Python：适合 Python 服务应用程序。

- Rust: Suitable for a Rust server application.
  - Rust：适合 Rust 服务应用程序。

- Other: General purpose starting point for containerizing your application.
  - Other：容器化您的应用程序的通用起点。


After `docker init` has completed, you may need to modify the created files and tailor them to your project. Visit the following topics to learn more about the files:

​	在 `docker init` 完成后，您可能需要修改生成的文件并根据项目需求进行调整。访问以下主题以了解文件的详细信息：

- [.dockerignore](https://docs.docker.com/reference/dockerfile/#dockerignore-file)
- [Dockerfile]({{< ref "/reference/Dockerfilereference" >}})
- [compose.yaml]({{< ref "/manuals/DockerCompose/IntroductiontoCompose/HowComposeworks" >}})

## Options

| Option      | Default | Description                                             |
| ----------- | ------- | ------------------------------------------------------- |
| `--version` |         | 显示初始化插件的版本 Display version of the init plugin |

## Examples

### 运行 `docker init` 的示例 Example of running `docker init`

The following example shows the initial menu after running `docker init`. See the additional examples to view the options for each language or framework.

​	以下示例显示运行 `docker init` 后的初始菜单。查看每种语言或框架的选项示例。



```console
$ docker init

Welcome to the Docker Init CLI!

This utility will walk you through creating the following files with sensible defaults for your project:
  - .dockerignore
  - Dockerfile
  - compose.yaml
  - README.Docker.md

Let's get started!

? What application platform does your project use?  [Use arrows to move, type to filter]
> PHP with Apache - (detected) suitable for a PHP web application
  Go - suitable for a Go server application
  Java - suitable for a Java application that uses Maven and packages as an uber jar
  Python - suitable for a Python server application
  Node - suitable for a Node server application
  Rust - suitable for a Rust server application
  ASP.NET Core - suitable for an ASP.NET Core application
  Other - general purpose starting point for containerizing your application
  Don't see something you need? Let us know!
  Quit
```

### 选择 Go 的示例 Example of selecting Go

The following example shows the prompts that appear after selecting `Go` and example input.

​	以下示例显示选择 `Go` 后出现的提示以及示例输入。



```console
? What application platform does your project use? Go
? What version of Go do you want to use? 1.20
? What's the relative directory (with a leading .) of your main package? .
? What port does your server listen on? 3333

CREATED: .dockerignore
CREATED: Dockerfile
CREATED: compose.yaml
CREATED: README.Docker.md

✔ Your Docker files are ready!

Take a moment to review them and tailor them to your application.

When you're ready, start your application by running: docker compose up --build

Your application will be available at http://localhost:3333

Consult README.Docker.md for more information about using the generated files.
```

### 选择 Node 的示例 Example of selecting Node

The following example shows the prompts that appear after selecting `Node` and example input.

​	以下示例显示选择 `Node` 后出现的提示以及示例输入。



```console
? What application platform does your project use? Node
? What version of Node do you want to use? 18
? Which package manager do you want to use? yarn
? Do you want to run "yarn run build" before starting your server? Yes
? What directory is your build output to? (comma-separate if multiple) output
? What command do you want to use to start the app? node index.js
? What port does your server listen on? 8000

CREATED: .dockerignore
CREATED: Dockerfile
CREATED: compose.yaml
CREATED: README.Docker.md

✔ Your Docker files are ready!

Take a moment to review them and tailor them to your application.

When you're ready, start your application by running: docker compose up --build

Your application will be available at http://localhost:8000

Consult README.Docker.md for more information about using the generated files.
```

### 选择 Python 的示例 Example of selecting Python

The following example shows the prompts that appear after selecting `Python` and example input.

​	以下示例显示选择 `Python` 后出现的提示以及示例输入。



```console
? What application platform does your project use? Python
? What version of Python do you want to use? 3.8
? What port do you want your app to listen on? 8000
? What is the command to run your app (e.g., gunicorn 'myapp.example:app' --bind=0.0.0.0:8000)? python ./app.py

CREATED: .dockerignore
CREATED: Dockerfile
CREATED: compose.yaml
CREATED: README.Docker.md

✔ Your Docker files are ready!

Take a moment to review them and tailor them to your application.

When you're ready, start your application by running: docker compose up --build

Your application will be available at http://localhost:8000

Consult README.Docker.md for more information about using the generated files.
```

### 选择 Rust 的示例 Example of selecting Rust

The following example shows the prompts that appear after selecting `Rust` and example input.

​	以下示例显示选择 `Rust` 后出现的提示以及示例输入。

​	

```console
? What application platform does your project use? Rust
? What version of Rust do you want to use? 1.70.0
? What port does your server listen on? 8000

CREATED: .dockerignore
CREATED: Dockerfile
CREATED: compose.yaml
CREATED: README.Docker.md

✔ Your Docker files are ready!

Take a moment to review them and tailor them to your application.

When you're ready, start your application by running: docker compose up --build

Your application will be available at http://localhost:8000

Consult README.Docker.md for more information about using the generated files.
```

### 选择 ASP.NET Core 的示例 Example of selecting ASP.NET Core

The following example shows the prompts that appear after selecting `ASP.NET Core` and example input.

​	以下示例显示选择 `ASP.NET Core` 后出现的提示以及示例输入。



```console
? What application platform does your project use? ASP.NET Core
? What's the name of your solution's main project? myapp
? What version of .NET do you want to use? 6.0
? What local port do you want to use to access your server? 8000

CREATED: .dockerignore
CREATED: Dockerfile
CREATED: compose.yaml
CREATED: README.Docker.md

✔ Your Docker files are ready!

Take a moment to review them and tailor them to your application.

When you're ready, start your application by running: docker compose up --build

Your application will be available at http://localhost:8000

Consult README.Docker.md for more information about using the generated files.
```

### 选择 PHP with Apache 的示例 Example of selecting PHP with Apache

The following example shows the prompts that appear after selecting `PHP with Apache` and example input. The PHP with Apache template is suitable for both pure PHP applications and applications using Composer as a dependency manager. After running `docker init`, you must manually add any PHP extensions that are required by your application to the Dockerfile.

​	以下示例显示选择 `PHP with Apache` 后出现的提示以及示例输入。PHP with Apache 模板适用于纯 PHP 应用程序和使用 Composer 作为依赖管理的应用程序。运行 `docker init` 后，您必须手动将应用程序所需的 PHP 扩展添加到 Dockerfile 中。



```console
? What application platform does your project use? PHP with Apache
? What version of PHP do you want to use? 8.2
? What's the relative directory (with a leading .) for your app? ./src
? What local port do you want to use to access your server? 9000

CREATED: .dockerignore
CREATED: Dockerfile
CREATED: compose.yaml
CREATED: README.Docker.md

✔ Your Docker files are ready!

Take a moment to review them and tailor them to your application.

If your application requires specific PHP extensions, you can follow the instructions in the Dockerfile to add them.

When you're ready, start your application by running: docker compose up --build

Your application will be available at http://localhost:9000

Consult README.Docker.md for more information about using the generated files.
```

### 选择 Java 的示例 Example of selecting Java

The following example shows the prompts that appear after selecting `Java` and example input.

​	以下示例显示选择 `Java` 后出现的提示以及示例输入。



```console
? What application platform does your project use? Java
? What version of Java do you want to use? 17
? What's the relative directory (with a leading .) for your app? ./src
? What port does your server listen on? 9000

CREATED: .dockerignore
CREATED: Dockerfile
CREATED: compose.yaml
CREATED: README.Docker.md

✔ Your Docker files are ready!

Take a moment to review them and tailor them to your application.

When you're ready, start your application by running: docker compose up --build

Your application will be available at http://localhost:9000

Consult README.Docker.md for more information about using the generated files.
```

### 选择 Other 的示例 Example of selecting Other

The following example shows the output after selecting `Other`.

​	以下示例显示选择 `Other` 后的输出。



```console
? What application platform does your project use? Other

CREATED: .dockerignore
CREATED: Dockerfile
CREATED: compose.yaml
CREATED: README.Docker.md

✔ Your Docker files are ready!

Take a moment to review them and tailor them to your application.

When you're ready, start your application by running: docker compose up --build

Consult README.Docker.md for more information about using the generated files.
```
