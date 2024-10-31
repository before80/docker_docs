+++
title = "使用Compose Watch"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/how-tos/file-watch/](https://docs.docker.com/compose/how-tos/file-watch/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use Compose Watch - 使用Compose Watch

Introduced in Docker Compose version [2.22.0](https://docs.docker.com/compose/releases/release-notes/#2220)

​	在 Docker Compose 版本 [2.22.0](https://docs.docker.com/compose/releases/release-notes/#2220) 中引入

The `watch` attribute automatically updates and previews your running Compose services as you edit and save your code. For many projects, this enables a hands-off development workflow once Compose is running, as services automatically update themselves when you save your work.

​	`watch` 属性会在您编辑并保存代码时自动更新并预览正在运行的 Compose 服务。对于许多项目来说，一旦 Compose 运行起来，服务会在保存时自动更新，从而实现免操作的开发流程。

`watch` adheres to the following file path rules:

​	`watch` 遵循以下文件路径规则：

- All paths are relative to the project directory
  - 所有路径均相对于项目目录

- Directories are watched recursively
  - 目录递归监视

- Glob patterns aren't supported
  - 不支持全局模式（Glob patterns）

- Rules from `.dockerignore` apply  - `.dockerignore` 文件中的规则适用

  - Use `ignore` option to define additional paths to be ignored (same syntax)
    - 使用 `ignore` 选项定义额外需要忽略的路径（语法相同）
  - Temporary/backup files for common IDEs (Vim, Emacs, JetBrains, & more) are ignored automatically
    - 常见 IDE 的临时/备份文件（如 Vim、Emacs、JetBrains 等）会自动忽略
  - `.git` directories are ignored automatically
    - `.git` 目录会自动忽略

You don't need to switch on `watch` for all services in a Compose project. In some instances, only part of the project, for example the Javascript frontend, might be suitable for automatic updates.

​	您不需要对 Compose 项目中的所有服务开启 `watch`。在某些情况下，只有项目的一部分（例如 JavaScript 前端）适合自动更新。

Compose Watch is designed to work with services built from local source code using the `build` attribute. It doesn't track changes for services that rely on pre-built images specified by the `image` attribute.

​	Compose Watch 专为使用 `build` 属性从本地源代码构建的服务而设计。它不会跟踪通过 `image` 属性指定的预构建镜像的更改。

## Compose Watch 与绑定挂载（bind mounts） - Compose Watch versus bind mounts

Compose supports sharing a host directory inside service containers. Watch mode does not replace this functionality but exists as a companion specifically suited to developing in containers.

​	Compose 支持在服务容器内共享主机目录。Watch 模式并不是为了替代此功能，而是作为容器化开发的一个辅助功能。

More importantly, `watch` allows for greater granularity than is practical with a bind mount. Watch rules let you ignore specific files or entire directories within the watched tree.

​	更重要的是，`watch` 可以提供比绑定挂载更精细的控制。Watch 规则允许忽略特定文件或整个目录。

For example, in a JavaScript project, ignoring the `node_modules/` directory has two benefits:

​	例如，在一个 JavaScript 项目中，忽略 `node_modules/` 目录具有以下优点：

- Performance. File trees with many small files can cause high I/O load in some configurations
  - 性能：包含许多小文件的文件树可能会在某些配置下造成较高的 I/O 负载

- Multi-platform. Compiled artifacts cannot be shared if the host OS or architecture is different to the container
  - 多平台兼容性：如果主机的操作系统或架构不同于容器，编译后的产物不能共享


For example, in a Node.js project, it's not recommended to sync the `node_modules/` directory. Even though JavaScript is interpreted, `npm` packages can contain native code that is not portable across platforms.

​	例如，在 Node.js 项目中，不建议同步 `node_modules/` 目录。尽管 JavaScript 是解释型语言，但 `npm` 包可能包含无法跨平台移植的本机代码。

## 配置 Configuration

The `watch` attribute defines a list of rules that control automatic service updates based on local file changes.

​	`watch` 属性定义了一组规则，用于根据本地文件的更改自动更新服务。

Each rule requires, a `path` pattern and `action` to take when a modification is detected. There are two possible actions for `watch` and depending on the `action`, additional fields might be accepted or required.

​	每条规则需要一个 `path` 模式和检测到修改时执行的 `action`。`watch` 有两种可能的 `action`，不同的 `action` 可能会接受或需要额外的字段。

Watch mode can be used with many different languages and frameworks. The specific paths and rules will vary from project to project, but the concepts remain the same.

​	Watch 模式可以与多种语言和框架一起使用。具体路径和规则会因项目而异，但概念相同。

### 前置条件 Prerequisites

In order to work properly, `watch` relies on common executables. Make sure your service image contains the following binaries:

​	为了正常工作，`watch` 依赖于常见的可执行文件。请确保您的服务镜像包含以下二进制文件：

- stat
- mkdir
- rmdir

`watch` also requires that the container's `USER` can write to the target path so it can update files. A common pattern is for initial content to be copied into the container using the `COPY` instruction in a Dockerfile. To ensure such files are owned by the configured user, use the `COPY --chown` flag:

​	此外，`watch` 要求容器的 `USER` 能够写入目标路径以更新文件。常见的模式是使用 Dockerfile 中的 `COPY` 指令将初始内容复制到容器中。为确保这些文件由配置的用户拥有，请使用 `COPY --chown` 标志：



```dockerfile
# Run as a non-privileged user
# 以非特权用户运行
FROM node:18
RUN useradd -ms /bin/sh -u 1001 app
USER app

# Install dependencies
# 安装依赖项
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install

# Copy source files into application directory
# 将源文件复制到应用程序目录
COPY --chown=app:app . /app
```

### `action`

#### Sync

If `action` is set to `sync`, Compose makes sure any changes made to files on your host automatically match with the corresponding files within the service container.

​	如果 `action` 设置为 `sync`，Compose 会确保主机上的文件更改自动匹配服务容器中的相应文件。

`sync` is ideal for frameworks that support "Hot Reload" or equivalent functionality.

​	`sync` 适合支持“热重载”或类似功能的框架。

More generally, `sync` rules can be used in place of bind mounts for many development use cases.

​	通常情况下，`sync` 规则可以代替绑定挂载，适用于许多开发用例。

#### Rebuild

If `action` is set to `rebuild`, Compose automatically builds a new image with BuildKit and replaces the running service container.

​	如果 `action` 设置为 `rebuild`，Compose 会自动使用 BuildKit 构建新镜像并替换正在运行的服务容器。

The behavior is the same as running `docker compose up --build <svc>`.

​	其行为与运行 `docker compose up --build <svc>` 相同。

Rebuild is ideal for compiled languages or as fallbacks for modifications to particular files that require a full image rebuild (e.g. `package.json`).

​	Rebuild 适用于编译型语言，或需要完整镜像重建的特定文件修改（例如 `package.json`）。

#### Sync + Restart

If `action` is set to `sync+restart`, Compose synchronizes your changes with the service containers and restarts it.

​	如果 `action` 设置为 `sync+restart`，Compose 会同步您的更改到服务容器并重新启动该服务。

`sync+restart` is ideal when config file changes, and you don't need to rebuild the image but just restart the main process of the service containers. It will work well when you update a database configuration or your `nginx.conf` file for example

​	`sync+restart` 适用于需要更改配置文件的情况，而无需重建镜像，只需重启服务容器的主进程。例如，当您更新数据库配置或 `nginx.conf` 文件时非常有效。

> **Tip**
>
> 
>
> Optimize your `Dockerfile` for speedy incremental rebuilds with [image layer caching]({{< ref "/manuals/DockerBuild/Cache" >}}) and [multi-stage builds]({{< ref "/manuals/DockerBuild/Building/Multi-stage" >}}).
>
> ​	使用 [镜像层缓存]({{< ref "/manuals/DockerBuild/Cache" >}}) 和 [多阶段构建]({{< ref "/manuals/DockerBuild/Building/Multi-stage" >}}) 优化您的 `Dockerfile` 以加快增量重建速度。

### `path` and `target`

The `target` field controls how the path is mapped into the container.

​	`target` 字段控制路径在容器中的映射方式。

For `path: ./app/html` and a change to `./app/html/index.html`:

​	对于 `path: ./app/html` 和对 `./app/html/index.html` 的更改：

- `target: /app/html` -> `/app/html/index.html`
- `target: /app/static` -> `/app/static/index.html`
- `target: /assets` -> `/assets/index.html`

## Example 1

This minimal example targets a Node.js application with the following structure:

​	以下是一个针对 Node.js 应用的最小示例，其结构如下：

```text
myproject/
├── web/
│   ├── App.jsx
│   └── index.js
├── Dockerfile
├── compose.yaml
└── package.json
```



```yaml
services:
  web:
    build: .
    command: npm start
    develop:
      watch:
        - action: sync
          path: ./web
          target: /src/web
          ignore:
            - node_modules/
        - action: rebuild
          path: package.json
```

In this example, when running `docker compose up --watch`, a container for the `web` service is launched using an image built from the `Dockerfile` in the project's root. The `web` service runs `npm start` for its command, which then launches a development version of the application with Hot Module Reload enabled in the bundler (Webpack, Vite, Turbopack, etc).

​	在此示例中，运行 `docker compose up --watch` 后，会从项目根目录中的 `Dockerfile` 构建 `web` 服务的容器。`web` 服务运行 `npm start` 作为命令，启动启用了模块热重载的应用程序开发版本（例如 Webpack、Vite、Turbopack 等）。

After the service is up, the watch mode starts monitoring the target directories and files. Then, whenever a source file in the `web/` directory is changed, Compose syncs the file to the corresponding location under `/src/web` inside the container. For example, `./web/App.jsx` is copied to `/src/web/App.jsx`.

​	服务启动后，watch 模式开始监控目标目录和文件。然后，每当 `web/` 目录中的源文件发生更改时，Compose 会将文件同步到容器中 `/src/web` 下的相应位置。例如，`./web/App.jsx` 会复制到 `/src/web/App.jsx`。

Once copied, the bundler updates the running application without a restart.

​	在复制后，打包工具会更新正在运行的应用程序而无需重启。

Unlike source code files, adding a new dependency can’t be done on-the-fly, so whenever `package.json` is changed, Compose rebuilds the image and recreates the `web` service container.

​	与源代码文件不同的是，添加新依赖项无法立即生效，因此每当 `package.json` 更改时，Compose 会重建镜像并重新创建 `web` 服务容器。

This pattern can be followed for many languages and frameworks, such as Python with Flask: Python source files can be synced while a change to `requirements.txt` should trigger a rebuild.

​	这种模式适用于许多语言和框架，例如使用 Flask 的 Python：Python 源文件可以同步，而更改 `requirements.txt` 则应触发重建。

## Example 2

Adapting the previous example to demonstrate `sync+restart`:

​	对上例稍作修改，演示 `sync+restart`：

```yaml
services:
  web:
    build: .
    command: npm start
    develop:
      watch:
        - action: sync
          path: ./web
          target: /app/web
          ignore:
            - node_modules/
        - action: sync+restart
          path: ./proxy/nginx.conf
          target: /etc/nginx/conf.d/default.conf

  backend:
    build:
      context: backend
      target: builder
```

This setup demonstrates how to use the `sync+restart` action in Docker Compose to efficiently develop and test a Node.js application with a frontend web server and backend service. The configuration ensures that changes to the application code and configuration files are quickly synchronized and applied, with the `web` service restarting as needed to reflect the changes.

​	此配置演示如何在 Docker Compose 中使用 `sync+restart`，以高效开发和测试包含前端 Web 服务器和后端服务的 Node.js 应用程序。该配置确保应用代码和配置文件的更改快速同步和应用，并在需要时重启 `web` 服务以反映更改。

## Use `watch`

1. Add `watch` sections to one or more services in `compose.yaml`. 将 `watch` 部分添加到 `compose.yaml` 中的一个或多个服务中。
2. Run `docker compose up --watch` to build and launch a Compose project and start the file watch mode. 运行 `docker compose up --watch` 以构建并启动 Compose 项目并开始文件监视模式。
3. Edit service source files using your preferred IDE or editor. 使用您喜欢的 IDE 或编辑器编辑服务源文件。

> **Note**
>
> 
>
> Watch can also be used with the dedicated `docker compose watch` command if you don't want to get the application logs mixed with the (re)build logs and filesystem sync events.
>
> ​	如果不希望应用日志与（重）建日志和文件系统同步事件混杂在一起，也可以使用专用的 `docker compose watch` 命令。

> **Tip**
>
> 
>
> Check out [`dockersamples/avatars`](https://github.com/dockersamples/avatars), or [local setup for Docker docs](https://github.com/docker/docs/blob/main/CONTRIBUTING.md) for a demonstration of Compose `watch`.
>
> ​	查看 [`dockersamples/avatars`](https://github.com/dockersamples/avatars) 或 [Docker 文档本地设置](https://github.com/docker/docs/blob/main/CONTRIBUTING.md)，以了解 Compose `watch` 的演示。

## 反馈 Feedback

We are actively looking for feedback on this feature. Give feedback or report any bugs you may find in the [Compose Specification repository](https://github.com/compose-spec/compose-spec/pull/253).

​	我们正在积极寻求关于此功能的反馈。在 [Compose Specification 仓库](https://github.com/compose-spec/compose-spec/pull/253) 中提供反馈或报告您可能遇到的任何错误。

## 参考 Reference

- [Compose Develop Specification Compose 开发规范]({{< ref "/reference/Composefilereference/ComposeDevelopSpecification" >}})
