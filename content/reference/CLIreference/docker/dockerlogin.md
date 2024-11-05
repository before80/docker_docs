+++
title = "docker login"
date = 2024-10-23T14:54:43+08:00
weight = 120
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/login/](https://docs.docker.com/reference/cli/docker/login/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker login

| Description | Authenticate to a registry        |
| :---------- | --------------------------------- |
| Usage       | `docker login [OPTIONS] [SERVER]` |

## Description

Authenticate to a registry.

​	对注册表进行身份验证。

You can authenticate to any public or private registry for which you have credentials. Authentication may be required for pulling and pushing images. Other commands, such as `docker scout` and `docker build`, may also require authentication to access subscription-only features or data related to your Docker organization.

​	您可以对任何拥有凭据的公共或私有注册表进行身份验证。拉取和推送镜像时可能需要身份验证。其他命令如 `docker scout` 和 `docker build` 也可能需要身份验证以访问订阅专属功能或与您的 Docker 组织相关的数据。

Authentication credentials are stored in the configured [credential store](https://docs.docker.com/reference/cli/docker/login/#credential-stores). If you use Docker Desktop, credentials are automatically saved to the native keychain of your operating system. If you're not using Docker Desktop, you can configure the credential store in the Docker configuration file, which is located at `$HOME/.docker/config.json` on Linux or `%USERPROFILE%/.docker/config.json` on Windows. If you don't configure a credential store, Docker stores credentials in the `config.json` file in a base64-encoded format. This method is less secure than configuring and using a credential store.

​	身份验证凭据存储在配置的[凭据存储](https://docs.docker.com/reference/cli/docker/login/#credential-stores)中。如果您使用 Docker Desktop，凭据会自动保存到操作系统的原生密钥链中。如果不使用 Docker Desktop，可以在 Docker 配置文件中配置凭据存储，该文件位于 Linux 的 `$HOME/.docker/config.json` 或 Windows 的 `%USERPROFILE%/.docker/config.json` 路径下。如果未配置凭据存储，Docker 将凭据以 base64 编码格式存储在 `config.json` 文件中，这种方法的安全性低于配置并使用凭据存储。

`docker login` also supports [credential helpers](https://docs.docker.com/reference/cli/docker/login/#credential-helpers) to help you handle credentials for specific registries.

​	`docker login` 还支持[凭据助手](https://docs.docker.com/reference/cli/docker/login/#credential-helpers)以帮助您处理特定注册表的凭据。

### 身份验证方法 Authentication methods

You can authenticate to a registry using a username and access token or password. Docker Hub also supports a web-based sign-in flow, which signs you in to your Docker account without entering your password. For Docker Hub, the `docker login` command uses a device code flow by default, unless the `--username` flag is specified. The device code flow is a secure way to sign in. See [Authenticate to Docker Hub using device code](https://docs.docker.com/reference/cli/docker/login/#authenticate-to-docker-hub-with-web-based-login).

​	您可以使用用户名和访问令牌或密码对注册表进行身份验证。Docker Hub 还支持基于 Web 的登录流程，可无需输入密码就登录到 Docker 账户。对于 Docker Hub，`docker login` 命令默认使用设备代码流程，除非指定了 `--username` 标志。设备代码流程是一种安全的登录方式。请参阅[使用设备代码登录 Docker Hub](https://docs.docker.com/reference/cli/docker/login/#authenticate-to-docker-hub-with-web-based-login)。

### 凭据存储 Credential stores

The Docker Engine can keep user credentials in an external credential store, such as the native keychain of the operating system. Using an external store is more secure than storing credentials in the Docker configuration file.

​	Docker 引擎可以在外部凭据存储中保存用户凭据，如操作系统的原生密钥链。使用外部存储比将凭据存储在 Docker 配置文件中更安全。

To use a credential store, you need an external helper program to interact with a specific keychain or external store. Docker requires the helper program to be in the client's host `$PATH`.

​	要使用凭据存储，需要一个外部助手程序与特定密钥链或外部存储交互。Docker 要求客户端主机的 `$PATH` 中有该助手程序。

You can download the helpers from the `docker-credential-helpers` [releases page](https://github.com/docker/docker-credential-helpers/releases). Helpers are available for the following credential stores:

​	您可以从 `docker-credential-helpers` 的[发布页面](https://github.com/docker/docker-credential-helpers/releases)下载这些助手。以下是支持的凭据存储：

- D-Bus Secret Service
- Apple macOS keychain
  - Apple macOS 密钥链

- Microsoft Windows Credential Manager
  - Microsoft Windows 凭据管理器

- [pass](https://www.passwordstore.org/)

With Docker Desktop, the credential store is already installed and configured for you. Unless you want to change the credential store used by Docker Desktop, you can skip the following steps.

​	对于 Docker Desktop，凭据存储已预先安装和配置。除非您希望更改 Docker Desktop 使用的凭据存储，否则可以跳过以下步骤。

#### 配置凭据存储 Configure the credential store

You need to specify the credential store in `$HOME/.docker/config.json` to tell the Docker Engine to use it. The value of the config property should be the suffix of the program to use (i.e. everything after `docker-credential-`). For example, to use `docker-credential-osxkeychain`:

​	您需要在 `$HOME/.docker/config.json` 中指定凭据存储，以指示 Docker 引擎使用它。配置属性的值应为要使用程序的后缀（即 `docker-credential-` 之后的部分）。例如，要使用 `docker-credential-osxkeychain`：



```json
{
  "credsStore": "osxkeychain"
}
```

If you are currently logged in, run `docker logout` to remove the credentials from the file and run `docker login` again.

​	如果您当前已登录，运行 `docker logout` 来从文件中删除凭据，然后重新运行 `docker login`。

#### 默认行为 Default behavior

By default, Docker looks for the native binary on each of the platforms, i.e. `osxkeychain` on macOS, `wincred` on Windows, and `pass` on Linux. A special case is that on Linux, Docker will fall back to the `secretservice` binary if it cannot find the `pass` binary. If none of these binaries are present, it stores the base64-encoded credentials in the `config.json` configuration file.

​	默认情况下，Docker 会在各平台上查找本地二进制文件，即 macOS 上的 `osxkeychain`、Windows 上的 `wincred` 以及 Linux 上的 `pass`。Linux 上的一个特殊情况是，如果找不到 `pass` 二进制文件，Docker 将回退到 `secretservice` 二进制文件。如果这些二进制文件均不存在，则将凭据以 base64 编码的形式存储在 `config.json` 配置文件中。

#### 凭据助手协议 Credential helper protocol

Credential helpers can be any program or script that implements the credential helper protocol. This protocol is inspired by Git, but differs in the information shared.

​	凭据助手可以是实现凭据助手协议的任何程序或脚本。该协议受到 Git 的启发，但共享的信息有所不同。

The helpers always use the first argument in the command to identify the action. There are only three possible values for that argument: `store`, `get`, and `erase`.

​	助手始终使用命令的第一个参数来标识操作。此参数只有三个可能的值：`store`、`get` 和 `erase`。

The `store` command takes a JSON payload from the standard input. That payload carries the server address, to identify the credential, the username, and either a password or an identity token.

​	`store` 命令从标准输入接收 JSON 负载。该负载包含服务器地址（用于标识凭据）、用户名以及密码或身份令牌。



```json
{
  "ServerURL": "https://index.docker.io/v1",
  "Username": "david",
  "Secret": "passw0rd1"
}
```

If the secret being stored is an identity token, the Username should be set to `<token>`.

​	如果存储的密钥是身份令牌，用户名应设置为 `<token>`。

The `store` command can write error messages to `STDOUT` that the Docker Engine will show if there was an issue.

​	`store` 命令可以将错误消息写入 `STDOUT`，如果有问题，Docker 引擎会显示这些信息。

The `get` command takes a string payload from the standard input. That payload carries the server address that the Docker Engine needs credentials for. This is an example of that payload: `https://index.docker.io/v1`.

​	`get` 命令从标准输入接收一个字符串负载，该负载包含 Docker 引擎需要凭据的服务器地址。以下是该负载的示例：`https://index.docker.io/v1`。

The `get` command writes a JSON payload to `STDOUT`. Docker reads the user name and password from this payload:

​	`get` 命令将 JSON 负载写入 `STDOUT`，Docker 从此负载读取用户名和密码：



```json
{
  "Username": "david",
  "Secret": "passw0rd1"
}
```

The `erase` command takes a string payload from `STDIN`. That payload carries the server address that the Docker Engine wants to remove credentials for. This is an example of that payload: `https://index.docker.io/v1`.

​	`erase` 命令从 `STDIN` 接收一个字符串负载，该负载包含 Docker 引擎要删除凭据的服务器地址。以下是该负载的示例：`https://index.docker.io/v1`。

The `erase` command can write error messages to `STDOUT` that the Docker Engine will show if there was an issue.

​	`erase` 命令可以将错误消息写入 `STDOUT`，如果有问题，Docker 引擎会显示这些信息。

### 凭据助手 Credential helpers

Credential helpers are similar to [credential stores](https://docs.docker.com/reference/cli/docker/login/#credential-stores), but act as the designated programs to handle credentials for specific registries. The default credential store will not be used for operations concerning credentials of the specified registries.

​	凭据助手类似于[凭据存储](https://docs.docker.com/reference/cli/docker/login/#credential-stores)，但充当指定注册表的凭据处理程序。指定的注册表的凭据操作不会使用默认凭据存储。

#### 配置凭据助手 Configure credential helpers

If you are currently logged in, run `docker logout` to remove the credentials from the default store.

​	如果您当前已登录，运行 `docker logout` 来从默认存储中删除凭据。

Credential helpers are specified in a similar way to `credsStore`, but allow for multiple helpers to be configured at a time. Keys specify the registry domain, and values specify the suffix of the program to use (i.e. everything after `docker-credential-`). For example:

​	凭据助手的指定方式类似于 `credsStore`，但允许同时配置多个助手。键指定注册表域，值指定要使用程序的后缀（即 `docker-credential-` 之后的部分）。例如：

```json
{
  "credHelpers": {
    "myregistry.example.com": "secretservice",
    "docker.internal.example": "pass",
  }
}
```

## Options

| Option                                                       | Default | Description                                    |
| ------------------------------------------------------------ | ------- | ---------------------------------------------- |
| `-p, --password`                                             |         | 密码 Password                                  |
| [`--password-stdin`](https://docs.docker.com/reference/cli/docker/login/#password-stdin) |         | 从 stdin 获取密码 Take the password from stdin |
| [`-u, --username`](https://docs.docker.com/reference/cli/docker/login/#username) |         | 用户名 Username                                |

## Examples

### 使用基于 Web 的登录方式对 Docker Hub 进行身份验证 Authenticate to Docker Hub with web-based login

By default, the `docker login` command authenticates to Docker Hub, using a device code flow. This flow lets you authenticate to Docker Hub without entering your password. Instead, you visit a URL in your web browser, enter a code, and authenticate.

​	默认情况下，`docker login` 命令使用设备代码流程对 Docker Hub 进行身份验证。此流程允许您无需输入密码即可登录 Docker Hub。相反，您可以在 Web 浏览器中访问一个 URL，输入代码并进行身份验证。

```console
$ docker login

USING WEB-BASED LOGIN
To sign in with credentials on the command line, use 'docker login -u <username>'

Your one-time device confirmation code is: LNFR-PGCJ
Press ENTER to open your browser or submit your device code here: https://login.docker.com/activate

Waiting for authentication in the browser…
```

After entering the code in your browser, you are authenticated to Docker Hub using the account you're currently signed in with on the Docker Hub website or in Docker Desktop. If you aren't signed in, you are prompted to sign in after entering the device code.

​	在浏览器中输入代码后，您将使用当前在 Docker Hub 网站或 Docker Desktop 上登录的帐户进行 Docker Hub 身份验证。如果尚未登录，您将在输入设备代码后被提示登录。

### 对自托管注册表进行身份验证 Authenticate to a self-hosted registry

If you want to authenticate to a self-hosted registry you can specify this by adding the server name.

​	如果希望对自托管注册表进行身份验证，可以通过添加服务器名称来指定。



```console
$ docker login registry.example.com
```

By default, the `docker login` command assumes that the registry listens on port 443 or 80. If the registry listens on a different port, you can specify it by adding the port number to the server name.

​	默认情况下，`docker login` 命令假设注册表监听 443 或 80 端口。如果注册表监听其他端口，可以通过在服务器名称中添加端口号来指定。



```console
$ docker login registry.example.com:1337
```

> **Note**
>
> Registry addresses should not include URL path components, only the hostname and (optionally) the port. Registry addresses with URL path components may result in an error. For example, `docker login registry.example.com/foo/` is incorrect, while `docker login registry.example.com` is correct.
>
> ​	注册表地址不应包含 URL 路径组件，仅包含主机名和（可选的）端口。包含 URL 路径组件的注册表地址可能会导致错误。例如，`docker login registry.example.com/foo/` 是不正确的，而 `docker login registry.example.com` 是正确的。
>
> The exception to this rule is the Docker Hub registry, which may use the `/v1/` path component in the address for historical reasons.
>
> ​	此规则的例外是 Docker Hub 注册表，出于历史原因可能在地址中使用 `/v1/` 路径组件。

### 使用用户名和密码对注册表进行身份验证 Authenticate to a registry with a username and password

To authenticate to a registry with a username and password, you can use the `--username` or `-u` flag. The following example authenticates to Docker Hub with the username `moby`. The password is entered interactively.

​	要使用用户名和密码对注册表进行身份验证，可以使用 `--username` 或 `-u` 标志。以下示例使用用户名 `moby` 对 Docker Hub 进行身份验证。密码为交互式输入。



```console
$ docker login -u moby
```

### 使用 STDIN 提供密码 (`--password-stdin`) Provide a password using STDIN (`--password-stdin`)

To run the `docker login` command non-interactively, you can set the `--password-stdin` flag to provide a password through `STDIN`. Using `STDIN` prevents the password from ending up in the shell's history, or log-files.

​	要以非交互方式运行 `docker login` 命令，可以设置 `--password-stdin` 标志以通过 `STDIN` 提供密码。使用 `STDIN` 可防止密码记录在 shell 历史记录或日志文件中。

The following example reads a password from a file, and passes it to the `docker login` command using `STDIN`:

​	以下示例从文件中读取密码，并通过 `STDIN` 将其传递给 `docker login` 命令：

```console
$ cat ~/my_password.txt | docker login --username foo --password-stdin
```
