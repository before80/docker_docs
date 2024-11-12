+++
title = "远程 Bake 文件定义"
date = 2024-10-23T14:54:40+08:00
weight = 110
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/bake/remote-definition/](https://docs.docker.com/build/bake/remote-definition/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Remote Bake file definition - 远程 Bake 文件定义

You can build Bake files directly from a remote Git repository or HTTPS URL:

​	您可以直接从远程 Git 仓库或 HTTPS URL 构建 Bake 文件：



```console
$ docker buildx bake "https://github.com/docker/cli.git#v20.10.11" --print
#1 [internal] load git source https://github.com/docker/cli.git#v20.10.11
#1 0.745 e8f1871b077b64bcb4a13334b7146492773769f7       refs/tags/v20.10.11
#1 2.022 From https://github.com/docker/cli
#1 2.022  * [new tag]         v20.10.11  -> v20.10.11
#1 DONE 2.9s
```

This fetches the Bake definition from the specified remote location and executes the groups or targets defined in that file. If the remote Bake definition doesn't specify a build context, the context is automatically set to the Git remote. For example, [this case](https://github.com/docker/cli/blob/2776a6d694f988c0c1df61cad4bfac0f54e481c8/docker-bake.hcl#L17-L26) uses `https://github.com/docker/cli.git`:

​	这会从指定的远程位置获取 Bake 定义，并执行该文件中定义的组或目标。如果远程 Bake 定义未指定构建上下文，则上下文会自动设置为 Git 远程。例如，[此情况](https://github.com/docker/cli/blob/2776a6d694f988c0c1df61cad4bfac0f54e481c8/docker-bake.hcl#L17-L26)使用 `https://github.com/docker/cli.git`：



```json
{
  "group": {
    "default": {
      "targets": ["binary"]
    }
  },
  "target": {
    "binary": {
      "context": "https://github.com/docker/cli.git#v20.10.11",
      "dockerfile": "Dockerfile",
      "args": {
        "BASE_VARIANT": "alpine",
        "GO_STRIP": "",
        "VERSION": ""
      },
      "target": "binary",
      "platforms": ["local"],
      "output": ["build"]
    }
  }
}
```

## 使用远程定义的本地上下文 Use the local context with a remote definition

When building with a remote Bake definition, you may want to consume local files relative to the directory where the Bake command is executed. You can define contexts as relative to the command context using a `cwd://` prefix.

​	使用远程 Bake 定义构建时，您可能希望使用相对于执行 Bake 命令的目录的本地文件。可以使用 `cwd://` 前缀定义相对于命令上下文的上下文。

https://github.com/dvdksn/buildx/blob/bake-remote-example/docker-bake.hcl



```hcl
target "default" {
  context = "cwd://"
  dockerfile-inline = <<EOT
FROM alpine
WORKDIR /src
COPY . .
RUN ls -l && stop
EOT
}
```



```console
$ touch foo bar
$ docker buildx bake "https://github.com/dvdksn/buildx.git#bake-remote-example"
```



```text
...
 > [4/4] RUN ls -l && stop:
#8 0.101 total 0
#8 0.102 -rw-r--r--    1 root     root             0 Jul 27 18:47 bar
#8 0.102 -rw-r--r--    1 root     root             0 Jul 27 18:47 foo
#8 0.102 /bin/sh: stop: not found
```

You can append a path to the `cwd://` prefix if you want to use a specific local directory as a context. Note that if you do specify a path, it must be within the working directory where the command gets executed. If you use an absolute path, or a relative path leading outside of the working directory, Bake will throw an error.

​	如果希望使用特定的本地目录作为上下文，可以在 `cwd://` 前缀后附加路径。请注意，如果指定路径，它必须在执行命令的工作目录内。如果使用绝对路径或指向工作目录外的相对路径，Bake 将会抛出错误。

### 本地命名上下文 Local named contexts

You can also use the `cwd://` prefix to define local directories in the Bake execution context as named contexts.

​	您还可以使用 `cwd://` 前缀将 Bake 执行上下文中的本地目录定义为命名上下文。

The following example defines the `docs` context as `./src/docs/content`, relative to the current working directory where Bake is run as a named context.

​	以下示例将 `docs` 上下文定义为 `./src/docs/content`，相对于运行 Bake 的当前工作目录。



```hcl
target "default" {
  contexts = {
    docs = "cwd://src/docs/content"
  }
  dockerfile = "Dockerfile"
}
```

By contrast, if you omit the `cwd://` prefix, the path would be resolved relative to the build context.

​	相比之下，如果省略 `cwd://` 前缀，路径将相对于构建上下文解析。

## 指定使用的 Bake 定义 Specify the Bake definition to use

When loading a Bake file from a remote Git repository, if the repository contains more than one Bake file, you can specify which Bake definition to use with the `--file` or `-f` flag:

​	从远程 Git 仓库加载 Bake 文件时，如果仓库包含多个 Bake 文件，可以使用 `--file` 或 `-f` 标志指定要使用的 Bake 定义：



```console
docker buildx bake -f bake.hcl "https://github.com/crazy-max/buildx.git#remote-with-local"
```



```text
...
#4 [2/2] RUN echo "hello world"
#4 0.270 hello world
#4 DONE 0.3s
```

## 组合本地和远程 Bake 定义 Combine local and remote Bake definitions

You can also combine remote definitions with local ones using the `cwd://` prefix with `-f`.

​	您还可以结合使用远程定义和本地定义，通过 `cwd://` 前缀与 `-f` 一起使用。

Given the following local Bake definition in the current working directory:

​	假设当前工作目录中有以下本地 Bake 定义：

```hcl
# local.hcl
target "default" {
  args = {
    HELLO = "foo"
  }
}
```

The following example uses `-f` to specify two Bake definitions:

​	以下示例使用 `-f` 指定两个 Bake 定义：

- `-f bake.hcl`: this definition is loaded relative to the Git URL.
  - `-f bake.hcl`：该定义相对于 Git URL 加载。

- `-f cwd://local.hcl`: this definition is loaded relative to the current working directory where the Bake command is executed.
  - `-f cwd://local.hcl`：该定义相对于执行 Bake 命令的当前工作目录加载。



```console
docker buildx bake -f bake.hcl -f cwd://local.hcl "https://github.com/crazy-max/buildx.git#remote-with-local" --print
```



```json
{
  "target": {
    "default": {
      "context": "https://github.com/crazy-max/buildx.git#remote-with-local",
      "dockerfile": "Dockerfile",
      "args": {
        "HELLO": "foo"
      },
      "target": "build",
      "output": [
        "type=cacheonly"
      ]
    }
  }
}
```

One case where combining local and remote Bake definitions becomes necessary is when you're building with a remote Bake definition in GitHub Actions and want to use the [metadata-action](https://github.com/docker/metadata-action) to generate tags, annotations, or labels. The metadata action generates a Bake file available in the runner's local Bake execution context. To use both the remote definition and the local "metadata-only" Bake file, specify both files and use the `cwd://` prefix for the metadata Bake file:

​	当您在 GitHub Actions 中使用远程 Bake 定义构建并希望使用 [metadata-action](https://github.com/docker/metadata-action) 生成标签、注解或标签时，组合本地和远程 Bake 定义变得必要。metadata 动作会生成一个可用于运行器本地 Bake 执行上下文的 Bake 文件。要同时使用远程定义和本地 “仅元数据” Bake 文件，请指定两个文件并对元数据 Bake 文件使用 `cwd://` 前缀：



```yml
      -
        name: Build
        uses: docker/bake-action@v4
        with:
          source: "${{ github.server_url }}/${{ github.repository }}.git#${{ github.ref }}"
          files: |
            ./docker-bake.hcl
            cwd://${{ steps.meta.outputs.bake-file }}            
          targets: build
```

## 使用私有仓库中的远程定义 Remote definition in a private repository

If you want to use a remote definition that lives in a private repository, you may need to specify credentials for Bake to use when fetching the definition.

​	如果您希望使用位于私有仓库中的远程定义，可能需要为 Bake 提供凭据以便获取定义。

If you can authenticate to the private repository using the default `SSH_AUTH_SOCK`, then you don't need to specify any additional authentication parameters for Bake. Bake automatically uses your default agent socket.

​	如果可以使用默认的 `SSH_AUTH_SOCK` 进行身份验证，则不需要为 Bake 指定任何额外的身份验证参数。Bake 会自动使用默认的代理套接字。

For authentication using an HTTP token, or custom SSH agents, use the following environment variables to configure Bake's authentication strategy:

​	对于使用 HTTP 令牌或自定义 SSH 代理的身份验证，可以使用以下环境变量配置 Bake 的身份验证策略：

- [`BUILDX_BAKE_GIT_AUTH_TOKEN`](https://docs.docker.com/build/building/variables/#buildx_bake_git_auth_token)

- [`BUILDX_BAKE_GIT_AUTH_HEADER`](https://docs.docker.com/build/building/variables/#buildx_bake_git_auth_header)
- [`BUILDX_BAKE_GIT_SSH`](https://docs.docker.com/build/building/variables/#buildx_bake_git_ssh)
