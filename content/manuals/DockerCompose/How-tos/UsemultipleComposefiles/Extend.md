+++
title = "扩展"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/how-tos/multiple-compose-files/extends/](https://docs.docker.com/compose/how-tos/multiple-compose-files/extends/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Extend your Compose file - 扩展您的 Compose 文件

Docker Compose's [`extends` attribute](https://docs.docker.com/reference/compose-file/services/#extends) lets you share common configurations among different files, or even different projects entirely.

​	Docker Compose 的 [`extends` 属性](https://docs.docker.com/reference/compose-file/services/#extends) 允许您在不同文件或不同项目之间共享常用的配置。

Extending services is useful if you have several services that reuse a common set of configuration options. With `extends` you can define a common set of service options in one place and refer to it from anywhere. You can refer to another Compose file and select a service you want to also use in your own application, with the ability to override some attributes for your own needs.

​	当多个服务共用一组通用配置选项时，扩展服务非常有用。通过 `extends`，您可以在一个地方定义一组服务选项，并在任何地方引用它。您可以引用另一个 Compose 文件并选择要在应用中使用的服务，同时根据需求覆盖一些属性。

> **Important**
>
> 
>
> When you use multiple Compose files, you must make sure all paths in the files are relative to the base Compose file (i.e. the Compose file in your main-project folder). This is required because extend files need not be valid Compose files. Extend files can contain small fragments of configuration. Tracking which fragment of a service is relative to which path is difficult and confusing, so to keep paths easier to understand, all paths must be defined relative to the base file.
>
> ​	当您使用多个 Compose 文件时，必须确保文件中的所有路径都相对于基础 Compose 文件（即您的主项目文件夹中的 Compose 文件）。这是必需的，因为扩展文件不一定是有效的 Compose 文件。扩展文件可以包含少量配置片段。要跟踪服务的哪个片段相对于哪个路径是困难且混乱的，因此，为了使路径更易于理解，所有路径必须相对于基础文件定义。

## 工作原理 How it works

### 从另一个文件扩展服务 Extending services from another file

Take the following example:

​	以下示例说明了如何操作：

```yaml
services:
  web:
    extends:
      file: common-services.yml
      service: webapp
```

This instructs Compose to re-use only the properties of the `webapp` service defined in the `common-services.yml` file. The `webapp` service itself is not part of the final project.

​	这指示 Compose 重用 `common-services.yml` 文件中定义的 `webapp` 服务的属性。`webapp` 服务本身不属于最终项目。

If `common-services.yml` looks like this:

​	如果 `common-services.yml` 如下所示：

```yaml
services:
  webapp:
    build: .
    ports:
      - "8000:8000"
    volumes:
      - "/data"
```

You get exactly the same result as if you wrote `docker-compose.yml` with the same `build`, `ports`, and `volumes` configuration values defined directly under `web`.

​	则结果与在 `docker-compose.yml` 中直接为 `web` 定义相同的 `build`、`ports` 和 `volumes` 配置值完全一致。

To include the service `webapp` in the final project when extending services from another file, you need to explicitly include both services in your current Compose file. For example (note this is a non-normative example):

​	要在扩展服务时将 `webapp` 服务包含在最终项目中，您需要在当前的 Compose 文件中明确包含这两个服务。例如（这是一个非规范性示例）：

```yaml
services:
  web:
    build: alpine
    command: echo
    extends:
      file: common-services.yml
      service: webapp
  webapp:
    extends:
      file: common-services.yml
      service: webapp
```

Alternatively, you can use [include]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Include" >}}).

​	或者，您可以使用[包含]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Include" >}})。

### 在同一文件中扩展服务 Extending services within the same file

If you define services in the same Compose file and extend one service from another, both the original service and the extended service will be part of your final configuration. For example:

​	如果在同一个 Compose 文件中定义服务并扩展一个服务至另一个服务，则原始服务和扩展服务都会成为最终配置的一部分。例如：

```yaml
services:
  web:
    build: alpine
    extends: webapp
  webapp:
    environment:
      - DEBUG=1
```

### 在同一文件中和从其他文件扩展服务 Extending services within the same file and from another file

You can go further and define, or re-define, configuration locally in `compose.yaml`:

​	您可以进一步在 `compose.yaml` 中本地定义或重新定义配置：

```yaml
services:
  web:
    extends:
      file: common-services.yml
      service: webapp
    environment:
      - DEBUG=1
    cpu_shares: 5

  important_web:
    extends: web
    cpu_shares: 10
```

## 其他示例 Additional example

Extending an individual service is useful when you have multiple services that have a common configuration. The example below is a Compose app with two services, a web application and a queue worker. Both services use the same codebase and share many configuration options.

​	扩展单个服务在多个服务共享通用配置时非常有用。以下示例是一个包含两个服务的 Compose 应用程序，一个是 Web 应用程序，一个是队列处理程序。两个服务使用相同的代码库，并共享许多配置选项。

The `common.yaml` file defines the common configuration:

​	`common.yaml` 文件定义了通用配置：

```yaml
services:
  app:
    build: .
    environment:
      CONFIG_FILE_PATH: /code/config
      API_KEY: xxxyyy
    cpu_shares: 5
```

The `docker-compose.yaml` defines the concrete services which use the common configuration:

​	`docker-compose.yaml` 定义了使用通用配置的具体服务：

```yaml
services:
  webapp:
    extends:
      file: common.yaml
      service: app
    command: /code/run_web_app
    ports:
      - 8080:8080
    depends_on:
      - queue
      - db

  queue_worker:
    extends:
      file: common.yaml
      service: app
    command: /code/run_worker
    depends_on:
      - queue
```

## 例外情况和限制 Exceptions and limitations

`volumes_from` and `depends_on` are never shared between services using `extends`. These exceptions exist to avoid implicit dependencies; you always define `volumes_from` locally. This ensures dependencies between services are clearly visible when reading the current file. Defining these locally also ensures that changes to the referenced file don't break anything.

​	`volumes_from` 和 `depends_on` 不会在使用 `extends` 的服务之间共享。这些例外情况存在是为了避免隐式依赖；您始终在本地定义 `volumes_from`。这样可以确保在阅读当前文件时清晰地看到服务之间的依赖关系。这样定义也能确保引用文件的更改不会导致问题。

`extends` is useful if you only need a single service to be shared and you are familiar with the file you're extending to, so you can tweak the configuration. But this isn’t an acceptable solution when you want to re-use someone else's unfamiliar configurations and you don’t know about its own dependencies.

​	`extends` 在您只需要共享一个服务并且熟悉所扩展文件的情况下非常有用，以便可以调整配置。但在您想要重用他人不熟悉的配置时，这不是一种合适的解决方案，因为您不清楚它的依赖关系。

## 相对路径 Relative paths

When using `extends` with a `file` attribute which points to another folder, relative paths declared by the service being extended are converted so they still point to the same file when used by the extending service. This is illustrated in the following example:

​	当使用 `extends` 并指向另一个文件夹中的 `file` 属性时，被扩展的服务声明的相对路径会转换，以便在扩展服务使用时仍指向相同的文件。以下示例说明了这一点：

Base Compose file:

​	基础 Compose 文件：

```yaml
services:
  webapp:
    image: example
    extends:
      file: ../commons/compose.yaml
      service: base
```

The `commons/compose.yaml` file:

​	`commons/compose.yaml` 文件：

```yaml
services:
  base:
    env_file: ./container.env
```

The resulting service refers to the original `container.env` file within the `commons` directory. This can be confirmed with `docker compose config` which inspects the actual model:

​	最终服务指向 `commons` 目录中的原始 `container.env` 文件。可以通过 `docker compose config` 确认实际模型：

```yaml
services:
  webapp:
    image: example
    env_file: 
      - ../commons/container.env
```

## 参考信息 Reference information

- [`extends`](https://docs.docker.com/reference/compose-file/services/#extends)
