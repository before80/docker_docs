+++
title = "Compose中的机密"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/how-tos/use-secrets/](https://docs.docker.com/compose/how-tos/use-secrets/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# How to use secrets in Docker Compose - 如何在 Docker Compose 中使用机密

A secret is any piece of data, such as a password, certificate, or API key, that shouldn’t be transmitted over a network or stored unencrypted in a Dockerfile or in your application’s source code.

​	机密（Secret）是任何不应通过网络传输或未加密存储在 Dockerfile 或应用源代码中的数据，例如密码、证书或 API 密钥。

Docker Compose provides a way for you to use secrets without having to use environment variables to store information. If you’re injecting passwords and API keys as environment variables, you risk unintentional information exposure. Services can only access secrets when explicitly granted by a `secrets` attribute within the `services` top-level element.

​	Docker Compose 提供了一种使用机密的方式，不需要使用环境变量来存储信息。如果通过环境变量注入密码和 API 密钥，您可能会无意间泄露信息。服务只有在顶级 `services` 元素中显式分配 `secrets` 属性时，才能访问机密。

Environment variables are often available to all processes, and it can be difficult to track access. They can also be printed in logs when debugging errors without your knowledge. Using secrets mitigates these risks.

​	环境变量通常对所有进程可见，并且可能难以跟踪访问。此外，在调试错误时，它们可能会在日志中打印出来而您并不知情。使用机密可以降低这些风险。

## 使用机密 Use secrets

Getting a secret into a container is a two-step process. First, define the secret using the [top-level secrets element in your Compose file]({{< ref "/reference/Composefilereference/Secretstop-levelelements" >}}). Next, update your service definitions to reference the secrets they require with the [secrets attribute](https://docs.docker.com/reference/compose-file/services/#secrets). Compose grants access to secrets on a per-service basis.

​	将机密注入容器是一个两步过程。首先，使用 [Compose 文件中的顶级 secrets 元素]({{< ref "/reference/Composefilereference/Secretstop-levelelements" >}}) 定义机密。接下来，更新服务定义以通过 [secrets 属性](https://docs.docker.com/reference/compose-file/services/#secrets)引用它们所需的机密。Compose 会按服务授予机密访问权限。

Unlike the other methods, this permits granular access control within a service container via standard filesystem permissions.

​	与其他方法不同，这允许通过标准文件系统权限在服务容器内实现细粒度的访问控制。

## 示例 Examples

### 简单示例 Simple

In the following example, the frontend service is given access to the `my_secret` secret. In the container, `/run/secrets/my_secret` is set to the contents of the file `./my_secret.txt`.

​	在以下示例中，frontend 服务被赋予了对 `my_secret` 机密的访问权限。在容器中，`/run/secrets/my_secret` 设置为文件 `./my_secret.txt` 的内容。

```yaml
services:
  myapp:
    image: myapp:latest
    secrets:
      - my_secret
secrets:
  my_secret:
    file: ./my_secret.txt
```

### 高级示例 Advanced



```yaml
services:
   db:
     image: mysql:latest
     volumes:
       - db_data:/var/lib/mysql
     environment:
       MYSQL_ROOT_PASSWORD_FILE: /run/secrets/db_root_password
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD_FILE: /run/secrets/db_password
     secrets:
       - db_root_password
       - db_password

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "8000:80"
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD_FILE: /run/secrets/db_password
     secrets:
       - db_password


secrets:
   db_password:
     file: db_password.txt
   db_root_password:
     file: db_root_password.txt

volumes:
    db_data:
```

In the advanced example above:

​	在上述高级示例中：

- The `secrets` attribute under each service defines the secrets you want to inject into the specific container.
  - 每个服务下的 `secrets` 属性定义了您希望注入到特定容器中的机密。

- The top-level `secrets` section defines the variables `db_password` and `db_root_password` and provides the `file` that populates their values.
  - 顶级 `secrets` 部分定义了 `db_password` 和 `db_root_password` 变量，并提供了包含其值的 `file`。

- The deployment of each container means Docker creates a temporary filesystem mount under `/run/secrets/<secret_name>` with their specific values.
  - 每个容器的部署会使 Docker 在 `/run/secrets/<secret_name>` 下创建一个临时文件系统挂载，并包含它们的特定值。


> **Note**
>
> 
>
> The `_FILE` environment variables demonstrated here are a convention used by some images, including Docker Official Images like [mysql](https://hub.docker.com/_/mysql) and [postgres](https://hub.docker.com/_/postgres).
>
> ​	这里展示的 `_FILE` 环境变量是一种惯例，适用于某些镜像，包括 Docker 官方镜像，如 [mysql](https://hub.docker.com/_/mysql) 和 [postgres](https://hub.docker.com/_/postgres)。

### 构建机密 Build secrets

In the following example, the `npm_token` secret is made available at build time. Its value is taken from the `NPM_TOKEN` environment variable.

​	在以下示例中，`npm_token` 机密在构建时可用，其值来自 `NPM_TOKEN` 环境变量。

```yaml
services:
  myapp:
    build:
      secrets:
        - npm_token
      context: .

secrets:
  npm_token:
    environment: NPM_TOKEN
```

## 资源 Resources

- [Secrets top-level element Secrets 顶级元素]({{< ref "/reference/Composefilereference/Secretstop-levelelements" >}})
- [Secrets attribute for services top-level element 服务顶级元素的 secrets 属性](https://docs.docker.com/reference/compose-file/services/#secrets)
- [Build secrets 构建机密]({{< ref "/manuals/DockerBuild/Building/Secrets" >}})
