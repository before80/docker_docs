+++
title = "使用 Compose 文件构建 Bake"
date = 2024-10-23T14:54:40+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/bake/compose-file/](https://docs.docker.com/build/bake/compose-file/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Building with Bake from a Compose file - 使用 Compose 文件构建 Bake

Bake supports the [Compose file format]({{< ref "/reference/Composefilereference" >}}) to parse a Compose file and translate each service to a [target](https://docs.docker.com/build/bake/reference/#target).

​	Bake 支持 [Compose 文件格式]({{< ref "/reference/Composefilereference" >}})，可以解析 Compose 文件并将每个服务转换为一个 [target（目标）](https://docs.docker.com/build/bake/reference/#target)。



```yaml
# docker-compose.yml
services:
  webapp-dev:
    build: &build-dev
      dockerfile: Dockerfile.webapp
      tags:
        - docker.io/username/webapp:latest
      cache_from:
        - docker.io/username/webapp:cache
      cache_to:
        - docker.io/username/webapp:cache

  webapp-release:
    build:
      <<: *build-dev
      x-bake:
        platforms:
          - linux/amd64
          - linux/arm64

  db:
    image: docker.io/username/db
    build:
      dockerfile: Dockerfile.db
```



```console
$ docker buildx bake --print
```



```json
{
  "group": {
    "default": {
      "targets": ["db", "webapp-dev", "webapp-release"]
    }
  },
  "target": {
    "db": {
      "context": ".",
      "dockerfile": "Dockerfile.db",
      "tags": ["docker.io/username/db"]
    },
    "webapp-dev": {
      "context": ".",
      "dockerfile": "Dockerfile.webapp",
      "tags": ["docker.io/username/webapp:latest"],
      "cache-from": ["docker.io/username/webapp:cache"],
      "cache-to": ["docker.io/username/webapp:cache"]
    },
    "webapp-release": {
      "context": ".",
      "dockerfile": "Dockerfile.webapp",
      "tags": ["docker.io/username/webapp:latest"],
      "cache-from": ["docker.io/username/webapp:cache"],
      "cache-to": ["docker.io/username/webapp:cache"],
      "platforms": ["linux/amd64", "linux/arm64"]
    }
  }
}
```

The compose format has some limitations compared to the HCL format:

​	相较于 HCL 格式，Compose 格式有一些限制：

- Specifying variables or global scope attributes is not yet supported
  - 尚不支持指定变量或全局范围属性

- `inherits` service field is not supported, but you can use [YAML anchors]({{< ref "/reference/Composefilereference/Fragments" >}}) to reference other services, as demonstrated in the previous example with `&build-dev`.

  - 不支持 `inherits` 服务字段，但可以使用 [YAML 锚点]({{< ref "/reference/Composefilereference/Fragments" >}}) 来引用其他服务，如上例中的 `&build-dev`。

  

## `.env` file

You can declare default environment variables in an environment file named `.env`. This file will be loaded from the current working directory, where the command is executed and applied to compose definitions passed with `-f`.

​	您可以在名为 `.env` 的环境文件中声明默认的环境变量。该文件将从命令执行的当前工作目录中加载，并应用于使用 `-f` 传递的 Compose 定义。



```yaml
# docker-compose.yml
services:
  webapp:
    image: docker.io/username/webapp:${TAG:-v1.0.0}
    build:
      dockerfile: Dockerfile
```



```sh
# .env
TAG=v1.1.0
```



```console
$ docker buildx bake --print
```



```json
{
  "group": {
    "default": {
      "targets": ["webapp"]
    }
  },
  "target": {
    "webapp": {
      "context": ".",
      "dockerfile": "Dockerfile",
      "tags": ["docker.io/username/webapp:v1.1.0"]
    }
  }
}
```

> **Note**
>
> 
>
> System environment variables take precedence over environment variables in `.env` file.
>
> ​	系统环境变量的优先级高于 `.env` 文件中的环境变量。

## 使用 `x-bake` 扩展字段 Extension field with `x-bake`

Where some fields are not available in the compose specification, you can use the [special extension]({{< ref "/reference/Composefilereference/Extensions" >}}) field `x-bake` in your compose file to evaluate extra fields:

​	在 Compose 规范中不可用的某些字段，您可以在 Compose 文件中使用特殊扩展字段 `x-bake` 来评估额外的字段：



```yaml
# docker-compose.yml
services:
  addon:
    image: ct-addon:bar
    build:
      context: .
      dockerfile: ./Dockerfile
      args:
        CT_ECR: foo
        CT_TAG: bar
      x-bake:
        tags:
          - ct-addon:foo
          - ct-addon:alp
        platforms:
          - linux/amd64
          - linux/arm64
        cache-from:
          - user/app:cache
          - type=local,src=path/to/cache
        cache-to:
          - type=local,dest=path/to/cache
        pull: true

  aws:
    image: ct-fake-aws:bar
    build:
      dockerfile: ./aws.Dockerfile
      args:
        CT_ECR: foo
        CT_TAG: bar
      x-bake:
        secret:
          - id=mysecret,src=./secret
          - id=mysecret2,src=./secret2
        platforms: linux/arm64
        output: type=docker
        no-cache: true
```



```console
$ docker buildx bake --print
```



```json
{
  "group": {
    "default": {
      "targets": ["aws", "addon"]
    }
  },
  "target": {
    "addon": {
      "context": ".",
      "dockerfile": "./Dockerfile",
      "args": {
        "CT_ECR": "foo",
        "CT_TAG": "bar"
      },
      "tags": ["ct-addon:foo", "ct-addon:alp"],
      "cache-from": ["user/app:cache", "type=local,src=path/to/cache"],
      "cache-to": ["type=local,dest=path/to/cache"],
      "platforms": ["linux/amd64", "linux/arm64"],
      "pull": true
    },
    "aws": {
      "context": ".",
      "dockerfile": "./aws.Dockerfile",
      "args": {
        "CT_ECR": "foo",
        "CT_TAG": "bar"
      },
      "tags": ["ct-fake-aws:bar"],
      "secret": ["id=mysecret,src=./secret", "id=mysecret2,src=./secret2"],
      "platforms": ["linux/arm64"],
      "output": ["type=docker"],
      "no-cache": true
    }
  }
}
```

Complete list of valid fields for `x-bake`:

​	`x-bake` 的有效字段完整列表：

- `cache-from`
- `cache-to`
- `contexts`
- `no-cache`
- `no-cache-filter`
- `output`
- `platforms`
- `pull`
- `secret`
- `ssh`
- `tags`
