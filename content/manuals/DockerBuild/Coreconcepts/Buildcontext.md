+++
title = "Build context"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/build/concepts/context/](https://docs.docker.com/build/concepts/context/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Build context - 构建上下文

The `docker build` and `docker buildx build` commands build Docker images from a [Dockerfile]({{< ref "/reference/Dockerfilereference" >}}) and a context.

​	`docker build` 和 `docker buildx build` 命令从 [Dockerfile]({{< ref "/reference/Dockerfilereference" >}}) 和上下文中构建 Docker 镜像。

## 什么是构建上下文？ What is a build context?

The build context is the set of files that your build can access. The positional argument that you pass to the build command specifies the context that you want to use for the build:

​	构建上下文是构建时可以访问的文件集合。传递给构建命令的位置参数指定了要用于构建的上下文：



```console
$ docker build [OPTIONS] PATH | URL | -
                         ^^^^^^^^^^^^^^
```

You can pass any of the following inputs as the context for a build:

​	你可以将以下任意输入作为构建上下文传递：

- The relative or absolute path to a local directory
  - 本地目录的相对或绝对路径

- A remote URL of a Git repository, tarball, or plain-text file
  - Git 仓库、tar 包或纯文本文件的远程 URL

- A plain-text file or tarball piped to the `docker build` command through standard input
  - 通过标准输入传递给 `docker build` 命令的纯文本文件或 tar 包


### 文件系统上下文 Filesystem contexts

When your build context is a local directory, a remote Git repository, or a tar file, then that becomes the set of files that the builder can access during the build. Build instructions such as `COPY` and `ADD` can refer to any of the files and directories in the context.

​	当构建上下文是本地目录、远程 Git 仓库或 tar 文件时，它会成为构建器在构建期间可以访问的文件集合。构建指令如 `COPY` 和 `ADD` 可以引用上下文中的任何文件和目录。

A filesystem build context is processed recursively:

​	文件系统构建上下文会递归处理：

- When you specify a local directory or a tarball, all subdirectories are included
  - 当指定一个本地目录或 tar 包时，所有子目录都会包含在内

- When you specify a remote Git repository, the repository and all submodules are included
  - 当指定一个远程 Git 仓库时，仓库及其所有子模块都会包含在内


For more information about the different types of filesystem contexts that you can use with your builds, see:

​	有关可以用于构建的不同类型的文件系统上下文的更多信息，请参见：

- [Local files](#本地上下文-local-context)
- [Git repositories](#git-repositories)
- [Remote tarballs](#远程上下文-remote-context)

### 文本文件上下文 Text file contexts

When your build context is a plain-text file, the builder interprets the file as a Dockerfile. With this approach, the build doesn't use a filesystem context.

​	当构建上下文是纯文本文件时，构建器将该文件解释为 Dockerfile。这种方式的构建不使用文件系统上下文。

For more information, see [empty build context](https://docs.docker.com/build/concepts/context/#empty-context).

​	有关更多信息，请参见[空构建上下文](#空上下文-empty-context)。

## 本地上下文 Local context

To use a local build context, you can specify a relative or absolute filepath to the `docker build` command. The following example shows a build command that uses the current directory (`.`) as a build context:

​	要使用本地构建上下文，可以向 `docker build` 命令指定相对或绝对文件路径。以下示例展示了使用当前目录 (`.`) 作为构建上下文的构建命令：



```console
$ docker build .
...
#16 [internal] load build context
#16 sha256:23ca2f94460dcbaf5b3c3edbaaa933281a4e0ea3d92fe295193e4df44dc68f85
#16 transferring context: 13.16MB 2.2s done
...
```

This makes files and directories in the current working directory available to the builder. The builder loads the files it needs from the build context when needed.

​	这使得当前工作目录中的文件和目录对构建器可用。构建器在需要时加载构建上下文中的文件。

You can also use local tarballs as build context, by piping the tarball contents to the `docker build` command. See [Tarballs](https://docs.docker.com/build/concepts/context/#local-tarballs).

​	你还可以使用本地 tar 包作为构建上下文，将 tar 包内容通过管道传递给 `docker build` 命令。请参见 [Tar 包](#本地-tar-包-local-tarballs)。

### 本地目录 Local directories

Consider the following directory structure:

​	考虑以下目录结构：

```text
.
├── index.ts
├── src/
├── Dockerfile
├── package.json
└── package-lock.json
```

Dockerfile instructions can reference and include these files in the build if you pass this directory as a context.

​	如果将此目录作为上下文传递，Dockerfile 指令可以在构建中引用并包含这些文件。

```dockerfile
# syntax=docker/dockerfile:1
FROM node:latest
WORKDIR /src
COPY package.json package-lock.json .
RUN npm ci
COPY index.ts src .
```



```console
$ docker build .
```

### 使用来自标准输入的 Dockerfile 的本地上下文 Local context with Dockerfile from stdin

Use the following syntax to build an image using files on your local filesystem, while using a Dockerfile from stdin.

​	使用以下语法，在使用本地文件系统上的文件时，通过标准输入使用 Dockerfile 构建镜像。



```console
$ docker build -f- PATH
```

The syntax uses the `-f` (or `--file`) option to specify the Dockerfile to use, and it uses a hyphen (`-`) as filename to instruct Docker to read the Dockerfile from stdin.

​	该语法使用 `-f`（或 `--file`）选项指定要使用的 Dockerfile，并使用连字符（`-`）作为文件名，指示 Docker 从标准输入读取 Dockerfile。

The following example uses the current directory (.) as the build context, and builds an image using a Dockerfile passed through stdin using a here-document.

​	以下示例使用当前目录 (`.`) 作为构建上下文，并使用通过标准输入传递的 Dockerfile 进行构建。

```bash
# create a directory to work in
mkdir example
cd example

# create an example file
touch somefile.txt

# build an image using the current directory as context
# and a Dockerfile passed through stdin
docker build -t myimage:latest -f- . <<EOF
FROM busybox
COPY somefile.txt ./
RUN cat /somefile.txt
EOF
```

### 本地 tar 包 Local tarballs

When you pipe a tarball to the build command, the build uses the contents of the tarball as a filesystem context.

​	将 tar 包通过管道传递给构建命令时，构建使用 tar 包的内容作为文件系统上下文。

For example, given the following project directory:

​	例如，给定以下项目目录：



```text
.
├── Dockerfile
├── Makefile
├── README.md
├── main.c
├── scripts
├── src
└── test.Dockerfile
```

You can create a tarball of the directory and pipe it to the build for use as a context:

​	你可以创建该目录的 tar 包并通过管道将其传递给构建命令作为上下文：





```console
$ tar czf foo.tar.gz *
$ docker build - < foo.tar.gz
```

The build resolves the Dockerfile from the tarball context. You can use the `--file` flag to specify the name and location of the Dockerfile relative to the root of the tarball. The following command builds using `test.Dockerfile` in the tarball:

​	构建从 tar 包上下文中解析 Dockerfile。你可以使用 `--file` 标志指定相对于 tar 包根目录的 Dockerfile 名称和位置。以下命令使用 tar 包中的 `test.Dockerfile` 进行构建：





```console
$ docker build --file test.Dockerfile - < foo.tar.gz
```

## 远程上下文 Remote context

You can specify the address of a remote Git repository, tarball, or plain-text file as your build context.

​	您可以指定远程 Git 仓库、tarball 压缩包或纯文本文件作为构建上下文。

- For Git repositories, the builder automatically clones the repository. See [Git repositories](https://docs.docker.com/build/concepts/context/#git-repositories).
  - 对于 Git 仓库，构建器会自动克隆该仓库。请参阅[Git 仓库](#git-repositories)。

- For tarballs, the builder downloads and extracts the contents of the tarball. See [Tarballs](https://docs.docker.com/build/concepts/context/#remote-tarballs).
  - 对于 tarball 压缩包，构建器会下载并解压该 tarball。请参阅[tarball 压缩包](#远程-tarball-压缩包-remote-tarballs)。


If the remote tarball is a text file, the builder receives no [filesystem context](https://docs.docker.com/build/concepts/context/#filesystem-contexts), and instead assumes that the remote file is a Dockerfile. See [Empty build context](https://docs.docker.com/build/concepts/context/#empty-context).

​	如果远程 tarball 是一个文本文件，构建器不会收到[文件系统上下文](#文件系统上下文-filesystem-contexts)，而是会假设远程文件是 Dockerfile。请参阅[空构建上下文](#空上下文-empty-context)。

### Git repositories

When you pass a URL pointing to the location of a Git repository as an argument to `docker build`, the builder uses the repository as the build context.

​	当您将指向 Git 仓库位置的 URL 作为参数传递给 `docker build` 时，构建器会将该仓库用作构建上下文。

The builder performs a shallow clone of the repository, downloading only the HEAD commit, not the entire history.

​	构建器会对仓库执行浅克隆，仅下载 HEAD 提交，而不是整个历史记录。

The builder recursively clones the repository and any submodules it contains.

​	构建器会递归克隆仓库及其包含的所有子模块。





```console
$ docker build https://github.com/user/myrepo.git
```

By default, the builder clones the latest commit on the default branch of the repository that you specify.

​	默认情况下，构建器会克隆您指定的仓库的默认分支上的最新提交。

#### URL 片段 URL fragments

You can append URL fragments to the Git repository address to make the builder clone a specific branch, tag, and subdirectory of a repository.

​	您可以向 Git 仓库地址附加 URL 片段，使构建器克隆特定的分支、标签或仓库中的子目录。

The format of the URL fragment is `#ref:dir`, where:

​	URL 片段的格式为 `#ref:dir`，其中：

- `ref` is the name of the branch, tag, or commit hash
  - `ref` 是分支、标签或提交哈希的名称

- `dir` is a subdirectory inside the repository
  - `dir` 是仓库中的子目录


For example, the following command uses the `container` branch, and the `docker` subdirectory in that branch, as the build context:

​	例如，以下命令使用 `container` 分支及该分支中的 `docker` 子目录作为构建上下文：

```console
$ docker build https://github.com/user/myrepo.git#container:docker
```

The following table represents all the valid suffixes with their build contexts:

​	下表展示了所有有效的后缀及其对应的构建上下文：

| Build Syntax Suffix            | Commit Used                   | Build Context Used |
| ------------------------------ | ----------------------------- | ------------------ |
| `myrepo.git`                   | `refs/heads/<default branch>` | `/`                |
| `myrepo.git#mytag`             | `refs/tags/mytag`             | `/`                |
| `myrepo.git#mybranch`          | `refs/heads/mybranch`         | `/`                |
| `myrepo.git#pull/42/head`      | `refs/pull/42/head`           | `/`                |
| `myrepo.git#:myfolder`         | `refs/heads/<default branch>` | `/myfolder`        |
| `myrepo.git#master:myfolder`   | `refs/heads/master`           | `/myfolder`        |
| `myrepo.git#mytag:myfolder`    | `refs/tags/mytag`             | `/myfolder`        |
| `myrepo.git#mybranch:myfolder` | `refs/heads/mybranch`         | `/myfolder`        |

When you use a commit hash as the `ref` in the URL fragment, use the full, 40-character string SHA-1 hash of the commit. A short hash, for example a hash truncated to 7 characters, is not supported.

​	当您在 URL 片段中使用提交哈希作为 `ref` 时，请使用完整的 40 字符串 SHA-1 哈希。短哈希（例如截短到 7 个字符的哈希）不被支持。

```bash
# ✅ The following works:
docker build github.com/docker/buildx#d4f088e689b41353d74f1a0bfcd6d7c0b213aed2
# ❌ The following doesn't work because the commit hash is truncated:
docker build github.com/docker/buildx#d4f088e
```

#### 保留 `.git` 目录 Keep `.git` directory

By default, BuildKit doesn't keep the `.git` directory when using Git contexts. You can configure BuildKit to keep the directory by setting the [`BUILDKIT_CONTEXT_KEEP_GIT_DIR` build argument](https://docs.docker.com/reference/dockerfile/#buildkit-built-in-build-args). This can be useful to if you want to retrieve Git information during your build:

​	默认情况下，当使用 Git 上下文时，BuildKit 不会保留 `.git` 目录。您可以通过设置 [`BUILDKIT_CONTEXT_KEEP_GIT_DIR` 构建参数]({{< ref "/reference/Dockerfilereference#buildkit-内置构建参数-buildkit-built-in-build-args">}})来配置 BuildKit 保留该目录。这在构建期间需要获取 Git 信息时可能会很有用：

```dockerfile
# syntax=docker/dockerfile:1
FROM alpine
WORKDIR /src
RUN --mount=target=. \
  make REVISION=$(git rev-parse HEAD) build
```



```console
$ docker build \
  --build-arg BUILDKIT_CONTEXT_KEEP_GIT_DIR=1
  https://github.com/user/myrepo.git#main
```

#### 私有仓库 Private repositories

When you specify a Git context that's also a private repository, the builder needs you to provide the necessary authentication credentials. You can use either SSH or token-based authentication.

​	当您指定的 Git 上下文是私有仓库时，构建器需要您提供必要的身份验证凭据。您可以使用 SSH 或基于令牌的身份验证。

Buildx automatically detects and uses SSH credentials if the Git context you specify is an SSH or Git address. By default, this uses `$SSH_AUTH_SOCK`. You can configure the SSH credentials to use with the [`--ssh` flag](https://docs.docker.com/reference/cli/docker/buildx/build/#ssh).

​	如果您指定的 Git 上下文是 SSH 或 Git 地址，Buildx 会自动检测并使用 SSH 凭据。默认情况下，它会使用 `$SSH_AUTH_SOCK`。您可以使用 [`--ssh` 标志]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxbuild#向构建公开-ssh-代理套接字或密钥---ssh-ssh-agent-socket-or-keys-to-expose-to-the-build---ssh">}})来配置要使用的 SSH 凭据。





```console
$ docker buildx build --ssh default git@github.com:user/private.git
```

If you want to use token-based authentication instead, you can pass the token using the [`--secret` flag](https://docs.docker.com/reference/cli/docker/buildx/build/#secret).

​	如果您想使用基于令牌的身份验证，可以通过 [`--secret` 标志]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxbuild#向构建公开的秘密---secret-secret-to-expose-to-the-build---secret">}})传递令牌。





```console
$ GIT_AUTH_TOKEN=<token> docker buildx build \
  --secret id=GIT_AUTH_TOKEN \
  https://github.com/user/private.git
```

> **Note**
>
> 
>
> Don't use `--build-arg` for secrets.
>
> ​	不要使用 `--build-arg` 来传递密钥。

### 从标准输入的 Dockerfile 构建远程上下文 Remote context with Dockerfile from stdin

Use the following syntax to build an image using files on your local filesystem, while using a Dockerfile from stdin.

​	使用以下语法可以使用本地文件系统上的文件，并从标准输入中读取 Dockerfile 进行构建：





```console
$ docker build -f- URL
```

The syntax uses the -f (or --file) option to specify the Dockerfile to use, and it uses a hyphen (-) as filename to instruct Docker to read the Dockerfile from stdin.

​	该语法使用 `-f`（或 `--file`）选项指定要使用的 Dockerfile，并使用连字符（-）作为文件名来指示 Docker 从标准输入读取 Dockerfile。

This can be useful in situations where you want to build an image from a repository that doesn't contain a Dockerfile. Or if you want to build with a custom Dockerfile, without maintaining your own fork of the repository.

​	这在您希望从不包含 Dockerfile 的仓库构建镜像，或希望使用自定义 Dockerfile 构建而不维护仓库的分支时很有用。

The following example builds an image using a Dockerfile from stdin, and adds the `hello.c` file from the [hello-world](https://github.com/docker-library/hello-world) repository on GitHub.

​	以下示例使用来自标准输入的 Dockerfile 构建镜像，并从 GitHub 上的 [hello-world](https://github.com/docker-library/hello-world) 仓库中添加 `hello.c` 文件。





```bash
docker build -t myimage:latest -f- https://github.com/docker-library/hello-world.git <<EOF
FROM busybox
COPY hello.c ./
EOF
```

### 远程 tarball 压缩包 Remote tarballs

If you pass the URL to a remote tarball, the URL itself is sent to the builder.

​	如果您传递远程 tarball 的 URL，则 URL 本身将被发送给构建器。



```console
$ docker build http://server/context.tar.gz
#1 [internal] load remote build context
#1 DONE 0.2s

#2 copy /context /
#2 DONE 0.1s
...
```

The download operation will be performed on the host where the BuildKit daemon is running. Note that if you're using a remote Docker context or a remote builder, that's not necessarily the same machine as where you issue the build command. BuildKit fetches the `context.tar.gz` and uses it as the build context. Tarball contexts must be tar archives conforming to the standard `tar` Unix format and can be compressed with any one of the `xz`, `bzip2`, `gzip` or `identity` (no compression) formats.

​	下载操作将在运行 BuildKit 守护进程的主机上执行。请注意，如果您使用远程 Docker 上下文或远程构建器，这不一定是您发出构建命令的同一台机器。BuildKit 会获取 `context.tar.gz` 并将其用作构建上下文。tarball 上下文必须符合标准的 `tar` Unix 格式，并且可以使用 `xz`、`bzip2`、`gzip` 或 `identity`（无压缩）格式进行压缩。

## 空上下文 Empty context

When you use a text file as the build context, the builder interprets the file as a Dockerfile. Using a text file as context means that the build has no filesystem context.

​	当您使用文本文件作为构建上下文时，构建器会将文件解释为 Dockerfile。使用文本文件作为上下文意味着构建没有文件系统上下文。

You can build with an empty build context when your Dockerfile doesn't depend on any local files.

​	当您的 Dockerfile 不依赖任何本地文件时，可以使用空的构建上下文。

### 如何在没有上下文的情况下构建 How to build without a context

You can pass the text file using a standard input stream, or by pointing at the URL of a remote text file.

​	您可以通过标准输入流传递文本文件，或指向远程文本文件的 URL。

{{< tabpane text=true persist=disabled >}}

{{% tab header="Unix pipe" %}}

```console
$ docker build - < Dockerfile
```

{{% /tab  %}}

{{% tab header="PowerShell " %}}

```powershell
Get-Content Dockerfile | docker build -
```

{{% /tab  %}}

{{% tab header="Heredocs " %}}

```sh
docker build -t myimage:latest - <<EOF
FROM busybox
RUN echo "hello world"
EOF
```

{{% /tab  %}}

{{% tab header=" Remote file" %}}

```sh
docker build https://raw.githubusercontent.com/dvdksn/clockbox/main/Dockerfile
```

{{% /tab  %}}

{{< /tabpane >}}

------

When you build without a filesystem context, Dockerfile instructions such as `COPY` can't refer to local files:

​	在没有文件系统上下文的情况下构建时，Dockerfile 指令如 `COPY` 无法引用本地文件：





```console
$ ls
main.c
$ docker build -<<< $'FROM scratch\nCOPY main.c .'
[+] Building 0.0s (4/4) FINISHED
 => [internal] load build definition from Dockerfile       0.0s
 => => transferring dockerfile: 64B                        0.0s
 => [internal] load .dockerignore                          0.0s
 => => transferring context: 2B                            0.0s
 => [internal] load build context                          0.0s
 => => transferring context: 2B                            0.0s
 => ERROR [1/1] COPY main.c .                              0.0s
------
 > [1/1] COPY main.c .:
------
Dockerfile:2
--------------------
   1 |     FROM scratch
   2 | >>> COPY main.c .
   3 |
--------------------
ERROR: failed to solve: failed to compute cache key: failed to calculate checksum of ref 7ab2bb61-0c28-432e-abf5-a4c3440bc6b6::4lgfpdf54n5uqxnv9v6ymg7ih: "/main.c": not found
```

## .dockerignore files

You can use a `.dockerignore` file to exclude files or directories from the build context.

​	您可以使用 `.dockerignore` 文件从构建上下文中排除文件或目录。





```text
# .dockerignore
node_modules
bar
```

This helps avoid sending unwanted files and directories to the builder, improving build speed, especially when using a remote builder.

​	这有助于避免将不需要的文件和目录发送给构建器，尤其是在使用远程构建器时可以提高构建速度。

### 文件名与位置 Filename and location

When you run a build command, the build client looks for a file named `.dockerignore` in the root directory of the context. If this file exists, the files and directories that match patterns in the files are removed from the build context before it's sent to the builder.

​	当您运行构建命令时，构建客户端会在上下文的根目录中查找名为 `.dockerignore` 的文件。如果此文件存在，则匹配文件中的模式的文件和目录将在发送给构建器之前从构建上下文中删除。

If you use multiple Dockerfiles, you can use different ignore-files for each Dockerfile. You do so using a special naming convention for the ignore-files. Place your ignore-file in the same directory as the Dockerfile, and prefix the ignore-file with the name of the Dockerfile, as shown in the following example.

​	如果您使用多个 Dockerfile，则可以为每个 Dockerfile 使用不同的忽略文件。您可以使用特殊的命名约定来指定每个 Dockerfile 的忽略文件。将忽略文件放在与 Dockerfile 相同的目录中，并在忽略文件名前加上 Dockerfile 的名称，如下所示。





```text
.
├── index.ts
├── src/
├── docker
│   ├── build.Dockerfile
│   ├── build.Dockerfile.dockerignore
│   ├── lint.Dockerfile
│   ├── lint.Dockerfile.dockerignore
│   ├── test.Dockerfile
│   └── test.Dockerfile.dockerignore
├── package.json
└── package-lock.json
```

A Dockerfile-specific ignore-file takes precedence over the `.dockerignore` file at the root of the build context if both exist.

​	如果 `.dockerignore` 文件和特定 Dockerfile 的忽略文件同时存在，则 Dockerfile 的特定忽略文件优先于根目录下的 `.dockerignore` 文件。

### Syntax

The `.dockerignore` file is a newline-separated list of patterns similar to the file globs of Unix shells. Leading and trailing slashes in ignore patterns are disregarded. The following patterns all exclude a file or directory named `bar` in the subdirectory `foo` under the root of the build context:

​	`.dockerignore` 文件是类似于 Unix shell 文件 glob 的换行分隔的模式列表。忽略模式中的前导和尾部斜杠会被忽略。以下模式都可以排除构建上下文根目录下 `foo` 子目录中的文件或目录 `bar`：

- `/foo/bar/`

- `/foo/bar`
- `foo/bar/`
- `foo/bar`

If a line in `.dockerignore` file starts with `#` in column 1, then this line is considered as a comment and is ignored before interpreted by the CLI.

​	如果 `.dockerignore` 文件中的一行以 `#` 开头，则此行为注释并在 CLI 解析前会被忽略。





```gitignore
#/this/is/a/comment
```

If you're interested in learning the precise details of the `.dockerignore` pattern matching logic, check out the [moby/patternmatcher repository](https://github.com/moby/patternmatcher/tree/main/ignorefile) on GitHub, which contains the source code.

​	如果您想了解 `.dockerignore` 模式匹配逻辑的具体细节，可以查看 GitHub 上的 [moby/patternmatcher 仓库](https://github.com/moby/patternmatcher/tree/main/ignorefile)，其中包含了源代码。

#### Matching

The following code snippet shows an example `.dockerignore` file.

​	以下代码片段展示了一个示例 `.dockerignore` 文件。





```text
# comment
*/temp*
*/*/temp*
temp?
```

This file causes the following build behavior:

​	此文件会导致以下构建行为：

| Rule        | Behavior                                                     |
| :---------- | :----------------------------------------------------------- |
| `# comment` | 被忽略。 Ignored.                                            |
| `*/temp*`   | 排除根目录的任何直接子目录中的文件或目录，其名称以 `temp` 开头。例如，排除文件 `/somedir/temporary.txt` 和目录 `/somedir/temp`。 Exclude files and directories whose names start with `temp` in any immediate subdirectory of the root. For example, the plain file `/somedir/temporary.txt` is excluded, as is the directory `/somedir/temp`. |
| `*/*/temp*` | 排除从根目录起向下两级子目录中名称以 `temp` 开头的文件或目录。例如，排除 `/somedir/subdir/temporary.txt`。 Exclude files and directories starting with `temp` from any subdirectory that is two levels below the root. For example, `/somedir/subdir/temporary.txt` is excluded. |
| `temp?`     | 排除根目录中的文件或目录，其名称为 `temp` 的一个字符扩展。例如，排除 `/tempa` 和 `/tempb`。 Exclude files and directories in the root directory whose names are a one-character extension of `temp`. For example, `/tempa` and `/tempb` are excluded. |

Matching is done using Go's [`filepath.Match` function](https://golang.org/pkg/path/filepath#Match) rules. A preprocessing step uses Go's [`filepath.Clean` function](https://golang.org/pkg/path/filepath/#Clean) to trim whitespace and remove `.` and `..`. Lines that are blank after preprocessing are ignored.

​	匹配是通过 Go 的 [`filepath.Match` 函数](https://golang.org/pkg/path/filepath#Match) 规则进行的。预处理步骤使用 Go 的 [`filepath.Clean` 函数](https://golang.org/pkg/path/filepath/#Clean) 去除空白字符，并移除 `.` 和 `..`。预处理后为空的行会被忽略。

> **Note**
>
> 
>
> For historical reasons, the pattern `.` is ignored.
>
> ​	出于历史原因，模式 `.` 会被忽略。

Beyond Go's `filepath.Match` rules, Docker also supports a special wildcard string `**` that matches any number of directories (including zero). For example, `**/*.go` excludes all files that end with `.go` found anywhere in the build context.

​	除了 Go 的 `filepath.Match` 规则外，Docker 还支持一个特殊的通配符 `**`，它可以匹配任意数量的目录（包括零个）。例如，`**/*.go` 会排除构建上下文中任何位置以 `.go` 结尾的所有文件。

You can use the `.dockerignore` file to exclude the `Dockerfile` and `.dockerignore` files. These files are still sent to the builder as they're needed for running the build. But you can't copy the files into the image using `ADD`, `COPY`, or bind mounts.

​	您可以使用 `.dockerignore` 文件排除 `Dockerfile` 和 `.dockerignore` 文件。这些文件仍会被发送给构建器，因为它们在构建过程中是必需的。但您无法使用 `ADD`、`COPY` 或绑定挂载将这些文件复制到镜像中。

#### 否定匹配 Negating matches

You can prepend lines with a `!` (exclamation mark) to make exceptions to exclusions. The following is an example `.dockerignore` file that uses this mechanism:

​	您可以在行前加上 `!`（感叹号）来对排除规则做出例外。以下是一个使用该机制的 `.dockerignore` 文件示例：





```text
*.md
!README.md
```

All markdown files right under the context directory *except* `README.md` are excluded from the context. Note that markdown files under subdirectories are still included.

​	此规则会排除上下文目录下所有的 markdown 文件，但 *不包括* `README.md`。注意，子目录下的 markdown 文件仍会包含在上下文中。

The placement of `!` exception rules influences the behavior: the last line of the `.dockerignore` that matches a particular file determines whether it's included or excluded. Consider the following example:

​	`!` 例外规则的位置会影响行为：`.dockerignore` 中最后一条匹配特定文件的规则决定了该文件是否包含在上下文中。以下是一个示例：



```text
*.md
!README*.md
README-secret.md
```

No markdown files are included in the context except README files other than `README-secret.md`.

​	此规则会排除所有 markdown 文件，但包含除 `README-secret.md` 之外的 README 文件。

Now consider this example:

​	再来看这个示例：





```text
*.md
README-secret.md
!README*.md
```

All of the README files are included. The middle line has no effect because `!README*.md` matches `README-secret.md` and comes last.

​	此规则会包含所有 README 文件。中间的行没有效果，因为 `!README*.md` 匹配 `README-secret.md` 且位于最后。
