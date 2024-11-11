+++
title = "密钥"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/building/secrets/](https://docs.docker.com/build/building/secrets/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Build secrets - 构建密钥

A build secret is any piece of sensitive information, such as a password or API token, consumed as part of your application's build process.

​	构建密钥是构建过程中使用的任何敏感信息，如密码或 API 令牌。

Build arguments and environment variables are inappropriate for passing secrets to your build, because they persist in the final image. Instead, you should use secret mounts or SSH mounts, which expose secrets to your builds securely.

​	构建参数和环境变量不适合将密钥传递给构建过程，因为它们会保留在最终的镜像中。相反，您应该使用密钥挂载或 SSH 挂载，以安全地将密钥暴露给构建过程。

## 构建密钥的类型 Types of build secrets

- [Secret mounts](https://docs.docker.com/build/building/secrets/#secret-mounts) are general-purpose mounts for passing secrets into your build. A secret mount takes a secret from the build client and makes it temporarily available inside the build container, for the duration of the build instruction. This is useful if, for example, your build needs to communicate with a private artifact server or API.

  - [密钥挂载](https://docs.docker.com/build/building/secrets/#secret-mounts)：用于将通用密钥传递到构建过程中的挂载方式。密钥挂载从构建客户端获取密钥，并在构建容器中临时提供该密钥，适用于需要与私有服务器或 API 通信的构建过程。

- [SSH mounts](https://docs.docker.com/build/building/secrets/#ssh-mounts) are special-purpose mounts for making SSH sockets or keys available inside builds. They're commonly used when you need to fetch private Git repositories in your builds.

  - [SSH 挂载](https://docs.docker.com/build/building/secrets/#ssh-mounts)：用于在构建过程中提供 SSH 套接字或密钥的专用挂载方式，常用于需要克隆私有 Git 仓库的情况。

- [Git authentication for remote contexts](https://docs.docker.com/build/building/secrets/#git-authentication-for-remote-contexts) is a set of pre-defined secrets for when you build with a remote Git context that's also a private repository. These secrets are "pre-flight" secrets: they are not consumed within your build instruction, but they're used to provide the builder with the necessary credentials to fetch the context.

  - [远程上下文的 Git 认证](https://docs.docker.com/build/building/secrets/#git-authentication-for-remote-contexts)：一组预定义密钥，用于通过远程 Git 上下文构建私有仓库。此类密钥属于“预处理”密钥，在构建指令中不会使用，但用于为构建器提供获取上下文所需的凭据。

  

## 使用构建密钥 Using build secrets

For secret mounts and SSH mounts, using build secrets is a two-step process. First you need to pass the secret into the `docker build` command, and then you need to consume the secret in your Dockerfile.

​	对于密钥挂载和 SSH 挂载，使用构建密钥包括两个步骤。首先将密钥传递给 `docker build` 命令，然后在 Dockerfile 中使用该密钥。

To pass a secret to a build, use the [`docker build --secret` flag](https://docs.docker.com/reference/cli/docker/buildx/build/#secret), or the equivalent options for [Bake](https://docs.docker.com/build/bake/reference/#targetsecret).

​	要将密钥传递给构建，使用 [`docker build --secret` 标志](https://docs.docker.com/reference/cli/docker/buildx/build/#secret)，或 [Bake](https://docs.docker.com/build/bake/reference/#targetsecret) 的等效选项。

{{< tabpane text=true persist=disabled >}}

{{% tab header="CLI" %}}

```sh
docker build --secret id=aws,src=$HOME/.aws/credentials .
```

{{% /tab  %}}

{{% tab header=" Bake" %}}

```sh
variable "HOME" {
  default = null
}

target "default" {
  secret = [
    "id=aws,src=${HOME}/.aws/credentials"
  ]
}
```

{{% /tab  %}}

{{< /tabpane >}}

------

To consume a secret in a build and make it accessible to the `RUN` instruction, use the [`--mount=type=secret`](https://docs.docker.com/reference/dockerfile/#run---mounttypesecret) flag in the Dockerfile.

​	在构建中使用密钥并让其对 `RUN` 指令可见时，在 Dockerfile 中使用 [`--mount=type=secret`](https://docs.docker.com/reference/dockerfile/#run---mounttypesecret) 标志。

```dockerfile
RUN --mount=type=secret,id=aws \
    AWS_SHARED_CREDENTIALS_FILE=/run/secrets/aws \
    aws s3 cp ...
```

## 密钥挂载 Secret mounts

Secret mounts expose secrets to the build containers, as files or environment variables. You can use secret mounts to pass sensitive information to your builds, such as API tokens, passwords, or SSH keys.

​	密钥挂载将密钥暴露给构建容器，可以作为文件或环境变量。您可以使用密钥挂载向构建过程传递敏感信息，例如 API 令牌、密码或 SSH 密钥。

### Sources

The source of a secret can be either a [file](https://docs.docker.com/reference/cli/docker/buildx/build/#file) or an [environment variable](https://docs.docker.com/reference/cli/docker/buildx/build/#env). When you use the CLI or Bake, the type can be detected automatically. You can also specify it explicitly with `type=file` or `type=env`.

​	密钥的来源可以是 [文件](https://docs.docker.com/reference/cli/docker/buildx/build/#file) 或 [环境变量](https://docs.docker.com/reference/cli/docker/buildx/build/#env)。使用 CLI 或 Bake 时，类型可以自动检测。您也可以通过 `type=file` 或 `type=env` 显式指定。

The following example mounts the environment variable `KUBECONFIG` to secret ID `kube`, as a file in the build container at `/run/secrets/kube`.

​	以下示例将环境变量 `KUBECONFIG` 挂载为密钥 ID `kube`，在构建容器中以文件形式提供，路径为 `/run/secrets/kube`。

```console
$ docker build --secret id=kube,env=KUBECONFIG .
```

When you use secrets from environment variables, you can omit the `env` parameter to bind the secret to a file with the same name as the variable. In the following example, the value of the `API_TOKEN` variable is mounted to `/run/secrets/API_TOKEN` in the build container.

​	使用环境变量中的密钥时，您可以省略 `env` 参数，将密钥绑定到与变量同名的文件。在以下示例中，`API_TOKEN` 变量的值被挂载到构建容器中的 `/run/secrets/API_TOKEN`。

```console
$ docker build --secret id=API_TOKEN .
```

### 目标 Target

When consuming a secret in a Dockerfile, the secret is mounted to a file by default. The default file path of the secret, inside the build container, is `/run/secrets/<id>`. You can customize how the secrets get mounted in the build container using the `target` and `env` options for the `RUN --mount` flag in the Dockerfile.

​	在 Dockerfile 中使用密钥时，密钥默认作为文件挂载。密钥在构建容器中的默认文件路径为 `/run/secrets/<id>`。您可以使用 Dockerfile 中 `RUN --mount` 标志的 `target` 和 `env` 选项自定义密钥的挂载方式。

The following example takes secret id `aws` and mounts it to a file at `/run/secrets/aws` in the build container.

​	以下示例将密钥 ID `aws` 挂载到构建容器中的 `/run/secrets/aws` 文件。

```dockerfile
RUN --mount=type=secret,id=aws \
    AWS_SHARED_CREDENTIALS_FILE=/run/secrets/aws \
    aws s3 cp ...
```

To mount a secret as a file with a different name, use the `target` option in the `--mount` flag.

​	要以不同的名称将密钥作为文件挂载，请在 `--mount` 标志中使用 `target` 选项。

```dockerfile
RUN --mount=type=secret,id=aws,target=/root/.aws/credentials \
    aws s3 cp ...
```

To mount a secret as an environment variable instead of a file, use the `env` option in the `--mount` flag.

​	要将密钥作为环境变量挂载而不是文件，请使用 `--mount` 标志中的 `env` 选项。

```dockerfile
RUN --mount=type=secret,id=aws-key-id,env=AWS_ACCESS_KEY_ID \
    --mount=type=secret,id=aws-secret-key,env=AWS_SECRET_ACCESS_KEY \
    --mount=type=secret,id=aws-session-token,env=AWS_SESSION_TOKEN \
    aws s3 cp ...
```

It's possible to use the `target` and `env` options together to mount a secret as both a file and an environment variable.

​	可以同时使用 `target` 和 `env` 选项，将密钥同时挂载为文件和环境变量。

## SSH 挂载 SSH mounts

If the credential you want to use in your build is an SSH agent socket or key, you can use the SSH mount instead of a secret mount. Cloning private Git repositories is a common use case for SSH mounts.

​	如果您在构建中要使用的凭据是 SSH 代理套接字或密钥，可以使用 SSH 挂载而非密钥挂载。克隆私有 Git 仓库是 SSH 挂载的常见用例。

The following example clones a private GitHub repository using a [Dockerfile SSH mount](https://docs.docker.com/reference/dockerfile/#run---mounttypessh).

​	以下示例使用 [Dockerfile SSH 挂载](https://docs.docker.com/reference/dockerfile/#run---mounttypessh) 克隆一个私有 GitHub 仓库。

```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
ADD git@github.com:me/myprivaterepo.git /src/
```

To pass an SSH socket the build, you use the [`docker build --ssh` flag](https://docs.docker.com/reference/cli/docker/buildx/build/#ssh), or equivalent options for [Bake](https://docs.docker.com/build/bake/reference/#targetssh).

​	要向构建传递 SSH 套接字，使用 [`docker build --ssh` 标志](https://docs.docker.com/reference/cli/docker/buildx/build/#ssh)，或 [Bake](https://docs.docker.com/build/bake/reference/#targetssh) 的等效选项。

```console
$ docker buildx build --ssh default .
```

## 远程上下文的 Git 认证 Git authentication for remote contexts

BuildKit supports two pre-defined build secrets, `GIT_AUTH_TOKEN` and `GIT_AUTH_HEADER`. Use them to specify HTTP authentication parameters when building with remote, private Git repositories, including:

​	BuildKit 支持两种预定义的构建密钥，`GIT_AUTH_TOKEN` 和 `GIT_AUTH_HEADER`。使用它们可在构建包含私有 Git 仓库的上下文时指定 HTTP 认证参数，包括：

- Building with a private Git repository as build context
  - 使用私有 Git 仓库作为构建上下文

- Fetching private Git repositories in a build with `ADD`
  - 在构建中使用 `ADD` 提取私有 Git 仓库


For example, say you have a private GitLab project at `https://gitlab.com/example/todo-app.git`, and you want to run a build using that repository as the build context. An unauthenticated `docker build` command fails because the builder isn't authorized to pull the repository:

​	例如，假设您有一个私有的 GitLab 项目位于 `https://gitlab.com/example/todo-app.git`，并希望使用该仓库作为构建上下文运行构建。未经身份验证的 `docker build` 命令会因构建器无权拉取仓库而失败：

```console
$ docker build https://gitlab.com/example/todo-app.git
[+] Building 0.4s (1/1) FINISHED
 => ERROR [internal] load git source https://gitlab.com/example/todo-app.git
------
 > [internal] load git source https://gitlab.com/example/todo-app.git:
0.313 fatal: could not read Username for 'https://gitlab.com': terminal prompts disabled
------
```

To authenticate the builder to the Git server, set the `GIT_AUTH_TOKEN` environment variable to contain a valid GitLab access token, and pass it as a secret to the build:

​	要向 Git 服务器认证构建器，将环境变量 `GIT_AUTH_TOKEN` 设置为有效的 GitLab 访问令牌，并作为密钥传递给构建：

```console
$ GIT_AUTH_TOKEN=$(cat gitlab-token.txt) docker build \
  --secret id=GIT_AUTH_TOKEN \
  https://gitlab.com/example/todo-app.git
```

The `GIT_AUTH_TOKEN` also works with `ADD` to fetch private Git repositories as part of your build:

​	`GIT_AUTH_TOKEN` 也可以与 `ADD` 一起使用，以便在构建过程中提取私有 Git 仓库：

```dockerfile
FROM alpine
ADD https://gitlab.com/example/todo-app.git /src
```

### HTTP 认证方案 HTTP authentication scheme

By default, Git authentication over HTTP uses the Bearer authentication scheme:

​	默认情况下，Git 的 HTTP 认证使用 Bearer 认证方案：

```http
Authorization: Bearer GIT_AUTH_TOKEN
```

If you need to use a Basic scheme, with a username and password, you can set the `GIT_AUTH_HEADER` build secret:

​	如果需要使用 Basic 方案（用户名和密码），可以设置 `GIT_AUTH_HEADER` 构建密钥：

```console
$ export GIT_AUTH_TOKEN=$(cat gitlab-token.txt)
$ export GIT_AUTH_HEADER=basic
$ docker build \
  --secret id=GIT_AUTH_TOKEN \
  --secret id=GIT_AUTH_HEADER \
  https://gitlab.com/example/todo-app.git
```

BuildKit currently only supports the Bearer and Basic schemes.

​	BuildKit 目前仅支持 Bearer 和 Basic 方案。

### 多个主机 Multiple hosts

You can set the `GIT_AUTH_TOKEN` and `GIT_AUTH_HEADER` secrets on a per-host basis, which lets you use different authentication parameters for different hostnames. To specify a hostname, append the hostname as a suffix to the secret ID:

​	可以基于主机设置 `GIT_AUTH_TOKEN` 和 `GIT_AUTH_HEADER` 密钥，这样可以为不同主机名使用不同的认证参数。要指定主机名，可以在密钥 ID 后附加主机名作为后缀。

```console
$ export GITLAB_TOKEN=$(cat gitlab-token.txt)
$ export GERRIT_TOKEN=$(cat gerrit-username-password.txt)
$ export GERRIT_SCHEME=basic
$ docker build \
  --secret id=GIT_AUTH_TOKEN.gitlab.com,env=GITLAB_TOKEN \
  --secret id=GIT_AUTH_TOKEN.gerrit.internal.example,env=GERRIT_TOKEN \
  --secret id=GIT_AUTH_HEADER.gerrit.internal.example,env=GERRIT_SCHEME \
  https://gitlab.com/example/todo-app.git
```
