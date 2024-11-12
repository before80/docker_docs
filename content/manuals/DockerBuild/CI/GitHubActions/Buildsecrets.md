+++
title = "Build secrets"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/build/ci/github-actions/secrets/](https://docs.docker.com/build/ci/github-actions/secrets/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Using secrets with GitHub Actions - 使用 GitHub Actions 管理密钥

A build secret is sensitive information, such as a password or API token, consumed as part of the build process. Docker Build supports two forms of secrets:

​	构建密钥是敏感信息，例如密码或 API 令牌，作为构建过程的一部分被消费。Docker Build 支持两种形式的密钥：

- [Secret mounts](https://docs.docker.com/build/ci/github-actions/secrets/#secret-mounts) add secrets as files in the build container (under `/run/secrets` by default).
  - [Secret 挂载](https://docs.docker.com/build/ci/github-actions/secrets/#secret-mounts) 作为文件添加密钥到构建容器中（默认在 `/run/secrets` 下）。

- [SSH mounts](https://docs.docker.com/build/ci/github-actions/secrets/#ssh-mounts) add SSH agent sockets or keys into the build container.
  - [SSH 挂载](https://docs.docker.com/build/ci/github-actions/secrets/#ssh-mounts) 将 SSH 代理套接字或密钥添加到构建容器中。


This page shows how to use secrets with GitHub Actions. For an introduction to secrets in general, see [Build secrets]({{< ref "/manuals/DockerBuild/Building/Secrets" >}}).

​	本页介绍如何在 GitHub Actions 中使用密钥。有关密钥的入门知识，请参见 [构建密钥]({{< ref "/manuals/DockerBuild/Building/Secrets" >}})。

## Secret 挂载 Secret mounts

In the following example uses and exposes the [`GITHUB_TOKEN` secret](https://docs.github.com/en/actions/security-guides/automatic-token-authentication#about-the-github_token-secret) as provided by GitHub in your workflow.

​	以下示例使用并公开 GitHub 提供的 [`GITHUB_TOKEN` 密钥](https://docs.github.com/en/actions/security-guides/automatic-token-authentication#about-the-github_token-secret)。

First, create a `Dockerfile` that uses the secret:

​	首先，创建一个使用该密钥的 `Dockerfile`：

```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
RUN --mount=type=secret,id=github_token,env=GITHUB_TOKEN ...
```

In this example, the secret name is `github_token`. The following workflow exposes this secret using the `secrets` input:

​	在此示例中，密钥名称为 `github_token`。以下工作流通过 `secrets` 输入公开此密钥：

```yaml
name: ci

on:
  push:

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Set up QEMU
        uses: docker/setup-qemu-action@v3
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      
      - name: Build
        uses: docker/build-push-action@v6
        with:
          platforms: linux/amd64,linux/arm64
          tags: user/app:latest
          secrets: |
            "github_token=${{ secrets.GITHUB_TOKEN }}"            
```

> **Note**
>
> 
>
> You can also expose a secret file to the build with the `secret-files` input:
>
> ​	您还可以使用 `secret-files` 输入将密钥文件公开到构建中：
>
> ```yaml
> secret-files: |
>   "MY_SECRET=./secret.txt"  
> ```

If you're using [GitHub secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets) and need to handle multi-line value, you will need to place the key-value pair between quotes:

​	如果您使用的是 [GitHub 密钥](https://docs.github.com/en/actions/security-guides/encrypted-secrets) 并需要处理多行值，则需要将键值对放在引号中：

```yaml
secrets: |
  "MYSECRET=${{ secrets.GPG_KEY }}"
  GIT_AUTH_TOKEN=abcdefghi,jklmno=0123456789
  "MYSECRET=aaaaaaaa
  bbbbbbb
  ccccccccc"
  FOO=bar
  "EMPTYLINE=aaaa

  bbbb
  ccc"
  "JSON_SECRET={""key1"":""value1"",""key2"":""value2""}"  
```

| Key              | Value                               |
| ---------------- | ----------------------------------- |
| `MYSECRET`       | `***********************`           |
| `GIT_AUTH_TOKEN` | `abcdefghi,jklmno=0123456789`       |
| `MYSECRET`       | `aaaaaaaa\nbbbbbbb\nccccccccc`      |
| `FOO`            | `bar`                               |
| `EMPTYLINE`      | `aaaa\n\nbbbb\nccc`                 |
| `JSON_SECRET`    | `{"key1":"value1","key2":"value2"}` |

> **Note**
>
> 
>
> Double escapes are needed for quote signs.
>
> ​	双重转义需要用于引号。

## SSH 挂载 SSH mounts

SSH mounts let you authenticate with SSH servers. For example to perform a `git clone`, or to fetch application packages from a private repository.

​	SSH 挂载允许您使用 SSH 服务器进行身份验证，例如执行 `git clone` 或从私有仓库获取应用程序包。

The following Dockerfile example uses an SSH mount to fetch Go modules from a private GitHub repository.

​	以下 `Dockerfile` 示例使用 SSH 挂载从私有 GitHub 仓库获取 Go 模块。

```dockerfile
# syntax=docker/dockerfile:1

ARG GO_VERSION="1.23"

FROM golang:${GO_VERSION}-alpine AS base
ENV CGO_ENABLED=0
ENV GOPRIVATE="github.com/foo/*"
RUN apk add --no-cache file git rsync openssh-client
RUN mkdir -p -m 0700 ~/.ssh && ssh-keyscan github.com >> ~/.ssh/known_hosts
WORKDIR /src

FROM base AS vendor
# this step configure git and checks the ssh key is loaded
# 该步骤配置 git 并检查 ssh 密钥是否加载
RUN --mount=type=ssh <<EOT
  set -e
  echo "Setting Git SSH protocol"
  git config --global url."git@github.com:".insteadOf "https://github.com/"
  (
    set +e
    ssh -T git@github.com
    if [ ! "$?" = "1" ]; then
      echo "No GitHub SSH key loaded exiting..."
      exit 1
    fi
  )
EOT
# this one download go modules
# 下载 go 模块
RUN --mount=type=bind,target=. \
    --mount=type=cache,target=/go/pkg/mod \
    --mount=type=ssh \
    go mod download -x

FROM vendor AS build
RUN --mount=type=bind,target=. \
    --mount=type=cache,target=/go/pkg/mod \
    --mount=type=cache,target=/root/.cache \
    go build ...
```

To build this Dockerfile, you must specify an SSH mount that the builder can use in the steps with `--mount=type=ssh`.

​	要构建此 `Dockerfile`，必须指定一个 SSH 挂载，以便构建步骤可以在 `--mount=type=ssh` 中使用它。

The following GitHub Action workflow uses the `MrSquaare/ssh-setup-action` third-party action to bootstrap SSH setup on the GitHub runner. The action creates a private key defined by the GitHub Action secret `SSH_GITHUB_PPK` and adds it to the SSH agent socket file at `SSH_AUTH_SOCK`. The SSH mount in the build step assume `SSH_AUTH_SOCK` by default, so there's no need to specify the ID or path for the SSH agent socket explicitly.

​	以下 GitHub Actions 工作流使用第三方操作 `MrSquaare/ssh-setup-action` 在 GitHub runner 上引导 SSH 设置。该操作创建由 GitHub Action 密钥 `SSH_GITHUB_PPK` 定义的私钥，并将其添加到 `SSH_AUTH_SOCK` 文件中的 SSH 代理套接字。构建步骤中的 SSH 挂载默认假定 `SSH_AUTH_SOCK`，因此无需明确指定 SSH 代理套接字的 ID 或路径。

{{< tabpane text=true persist=disabled >}}

{{% tab header="`docker/build-push-action`" %}}

```yaml
name: ci

on:
  push:

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Set up SSH
        uses: MrSquaare/ssh-setup-action@2d028b70b5e397cf8314c6eaea229a6c3e34977a # v3.1.0
        with:
          host: github.com
          private-key: ${{ secrets.SSH_GITHUB_PPK }}
          private-key-name: github-ppk
      
      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          ssh: default
          push: true
          tags: user/app:latest
```

{{% /tab  %}}

{{% tab header="`docker/bake-action`" %}}

```yaml
name: ci

on:
  push:

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      - name: Set up SSH
        uses: MrSquaare/ssh-setup-action@2d028b70b5e397cf8314c6eaea229a6c3e34977a # v3.1.0
        with:
          host: github.com
          private-key: ${{ secrets.SSH_GITHUB_PPK }}
          private-key-name: github-ppk
      
      - name: Build
        uses: docker/bake-action@v5
        with:
          set: |
            *.ssh=default
```

{{% /tab  %}}

{{< /tabpane >}}
