+++
title = "代理配置"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/cli/proxy/](https://docs.docker.com/engine/cli/proxy/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Use a proxy server with the Docker CLI - 在 Docker CLI 中使用代理服务器

This page describes how to configure the Docker CLI to use proxies via environment variables in containers.

​	本页描述了如何在容器中通过环境变量配置 Docker CLI 使用代理。

This page doesn't describe how to configure proxies for Docker Desktop. For instructions, see [configuring Docker Desktop to use HTTP/HTTPS proxies](https://docs.docker.com/desktop/settings/#proxies).

​	本页不介绍如何为 Docker Desktop 配置代理。有关说明，请参阅 [配置 Docker Desktop 使用 HTTP/HTTPS 代理](https://docs.docker.com/desktop/settings/#proxies)。

If you're running Docker Engine without Docker Desktop, refer to [Configure the Docker daemon to use a proxy]({{< ref "/manuals/DockerEngine/Daemon/Daemonproxyconfiguration" >}}) to learn how to configure a proxy server for the Docker daemon (`dockerd`) itself.

​	如果您在没有 Docker Desktop 的情况下运行 Docker Engine，请参阅 [配置 Docker 守护进程使用代理]({{< ref "/manuals/DockerEngine/Daemon/Daemonproxyconfiguration" >}}) 了解如何为 Docker 守护进程 (`dockerd`) 本身配置代理服务器。

If your container needs to use an HTTP, HTTPS, or FTP proxy server, you can configure it in different ways:

​	如果您的容器需要使用 HTTP、HTTPS 或 FTP 代理服务器，您可以通过以下不同方式进行配置：

- [Configure the Docker client](https://docs.docker.com/engine/cli/proxy/#configure-the-docker-client) 配置 Docker 客户端

- [Set proxy using the CLI](https://docs.docker.com/engine/cli/proxy/#set-proxy-using-the-cli) 使用 CLI 设置代理

> **Note**
>
> 
>
> Unfortunately, there's no standard that defines how web clients should handle proxy environment variables, or the format for defining them.
>
> ​	不幸的是，没有标准定义 Web 客户端应如何处理代理环境变量或其定义格式。
>
> If you're interested in the history of these variables, check out this blog post on the subject, by the GitLab team: [We need to talk: Can we standardize NO_PROXY?](https://about.gitlab.com/blog/2021/01/27/we-need-to-talk-no-proxy/).
>
> ​	如果您对这些变量的历史感兴趣，可以查看 GitLab 团队关于该主题的博客：[We need to talk: Can we standardize NO_PROXY?](https://about.gitlab.com/blog/2021/01/27/we-need-to-talk-no-proxy/)。

## 配置 Docker 客户端 Configure the Docker client

You can add proxy configurations for the Docker client using a JSON configuration file, located in `~/.docker/config.json`. Builds and containers use the configuration specified in this file.

​	您可以通过位于 `~/.docker/config.json` 的 JSON 配置文件为 Docker 客户端添加代理配置。构建和容器将使用该文件中指定的配置。



```json
{
 "proxies": {
   "default": {
     "httpProxy": "http://proxy.example.com:3128",
     "httpsProxy": "https://proxy.example.com:3129",
     "noProxy": "*.test.example.com,.example.org,127.0.0.0/8"
   }
 }
}
```

> **Warning**
>
> 
>
> Proxy settings may contain sensitive information. For example, some proxy servers require authentication information to be included in their URL, or their address may expose IP-addresses or hostnames of your company's environment.
>
> ​	代理设置可能包含敏感信息。例如，一些代理服务器要求在其 URL 中包含身份验证信息，或代理地址可能暴露公司环境的 IP 地址或主机名。
>
> Environment variables are stored as plain text in the container's configuration, and as such can be inspected through the remote API or committed to an image when using `docker commit`.
>
> ​	环境变量以纯文本形式存储在容器的配置中，因此可以通过远程 API 检查，或在使用 `docker commit` 时提交到镜像中。

The configuration becomes active after saving the file, you don't need to restart Docker. However, the configuration only applies to new containers and builds, and doesn't affect existing containers.

​	保存文件后配置立即生效，无需重启 Docker。但该配置仅适用于新容器和新构建，不影响现有容器。

The following table describes the available configuration parameters.

​	下表描述了可用的配置参数。

| Property     | Description                                                  |
| :----------- | :----------------------------------------------------------- |
| `httpProxy`  | 设置 `HTTP_PROXY` 和 `http_proxy` 环境变量及构建参数。 Sets the `HTTP_PROXY` and `http_proxy` environment variables and build arguments. |
| `httpsProxy` | 设置 `HTTPS_PROXY` 和 `https_proxy` 环境变量及构建参数。 Sets the `HTTPS_PROXY` and `https_proxy` environment variables and build arguments. |
| `ftpProxy`   | 设置 `FTP_PROXY` 和 `ftp_proxy` 环境变量及构建参数。 Sets the `FTP_PROXY` and `ftp_proxy` environment variables and build arguments. |
| `noProxy`    | 设置 `NO_PROXY` 和 `no_proxy` 环境变量及构建参数。 Sets the `NO_PROXY` and `no_proxy` environment variables and build arguments. |
| `allProxy`   | 设置 `ALL_PROXY` 和 `all_proxy` 环境变量及构建参数。 Sets the `ALL_PROXY` and `all_proxy` environment variables and build arguments. |

These settings are used to configure proxy environment variables for containers only, and not used as proxy settings for the Docker CLI or the Docker Engine itself. Refer to the [environment variables](https://docs.docker.com/reference/cli/docker/#environment-variables) and [configure the Docker daemon to use a proxy server]({{< ref "/manuals/DockerEngine/Daemon/Daemonproxyconfiguration" >}}) sections for configuring proxy settings for the CLI and daemon.

​	这些设置仅用于为容器配置代理环境变量，且不会用于 Docker CLI 或 Docker Engine 本身的代理设置。有关 CLI 和守护进程的代理设置，请参阅 [环境变量](https://docs.docker.com/reference/cli/docker/#environment-variables) 和 [配置 Docker 守护进程使用代理服务器]({{< ref "/manuals/DockerEngine/Daemon/Daemonproxyconfiguration" >}}) 部分。

### 使用代理配置运行容器 Run containers with a proxy configuration

When you start a container, its proxy-related environment variables are set to reflect your proxy configuration in `~/.docker/config.json`.

​	启动容器时，其代理相关环境变量会根据 `~/.docker/config.json` 中的代理配置进行设置。

For example, assuming a proxy configuration like the example shown in the [earlier section](https://docs.docker.com/engine/cli/proxy/#configure-the-docker-client), environment variables for containers that you run are set as follows:

​	例如，假设配置如 [上节示例](https://docs.docker.com/engine/cli/proxy/#configure-the-docker-client) 所示，则运行的容器环境变量设置如下：



```console
$ docker run --rm alpine sh -c 'env | grep -i  _PROXY'
https_proxy=http://proxy.example.com:3129
HTTPS_PROXY=http://proxy.example.com:3129
http_proxy=http://proxy.example.com:3128
HTTP_PROXY=http://proxy.example.com:3128
no_proxy=*.test.example.com,.example.org,127.0.0.0/8
NO_PROXY=*.test.example.com,.example.org,127.0.0.0/8
```

### 使用代理配置进行构建 Build with a proxy configuration

When you invoke a build, proxy-related build arguments are pre-populated automatically, based on the proxy settings in your Docker client configuration file.

​	在构建时，代理相关的构建参数会根据 Docker 客户端配置文件中的代理设置自动预填。

Assuming a proxy configuration like the example shown in the [earlier section](https://docs.docker.com/engine/cli/proxy/#configure-the-docker-client), environment are set as follows during builds:

​	假设代理配置如 [上节示例](https://docs.docker.com/engine/cli/proxy/#configure-the-docker-client) 所示，构建过程中设置的环境变量如下：



```console
$ docker build \
  --no-cache \
  --progress=plain \
  - <<EOF
FROM alpine
RUN env | grep -i _PROXY
EOF
```



```console
#5 [2/2] RUN env | grep -i _PROXY
#5 0.100 HTTPS_PROXY=https://proxy.example.com:3129
#5 0.100 no_proxy=*.test.example.com,.example.org,127.0.0.0/8
#5 0.100 NO_PROXY=*.test.example.com,.example.org,127.0.0.0/8
#5 0.100 https_proxy=https://proxy.example.com:3129
#5 0.100 http_proxy=http://proxy.example.com:3128
#5 0.100 HTTP_PROXY=http://proxy.example.com:3128
#5 DONE 0.1s
```

### 为每个守护进程配置代理设置 Configure proxy settings per daemon

The `default` key under `proxies` in `~/.docker/config.json` configures the proxy settings for all daemons that the client connects to. To configure the proxies for individual daemons, use the address of the daemon instead of the `default` key.

​	在 `~/.docker/config.json` 的 `proxies` 下的 `default` 键可配置客户端连接的所有守护进程的代理设置。要为单独的守护进程配置代理，请使用守护进程的地址替代 `default` 键。

The following example configures both a default proxy config, and a no-proxy override for the Docker daemon on address `tcp://docker-daemon1.example.com`:

​	以下示例同时配置默认代理配置和 `tcp://docker-daemon1.example.com` 地址上的 Docker 守护进程的 no-proxy 覆盖配置：



```json
{
 "proxies": {
   "default": {
     "httpProxy": "http://proxy.example.com:3128",
     "httpsProxy": "https://proxy.example.com:3129",
     "noProxy": "*.test.example.com,.example.org,127.0.0.0/8"
   },
   "tcp://docker-daemon1.example.com": {
     "noProxy": "*.internal.example.net"
   }
 }
}
```

## 使用 CLI 设置代理 Set proxy using the CLI

Instead of [configuring the Docker client](https://docs.docker.com/engine/cli/proxy/#configure-the-docker-client), you can specify proxy configurations on the command-line when you invoke the `docker build` and `docker run` commands.

​	除了 [配置 Docker 客户端](https://docs.docker.com/engine/cli/proxy/#configure-the-docker-client)，您还可以在调用 `docker build` 和 `docker run` 命令时在命令行上指定代理配置。

Proxy configuration on the command-line uses the `--build-arg` flag for builds, and the `--env` flag for when you want to run containers with a proxy.

​	命令行上的代理配置使用构建时的 `--build-arg` 标志和运行容器时的 `--env` 标志。



```console
$ docker build --build-arg HTTP_PROXY="http://proxy.example.com:3128" .
$ docker run --env HTTP_PROXY="http://proxy.example.com:3128" redis
```

For a list of all the proxy-related build arguments that you can use with the `docker build` command, see [Predefined ARGs](https://docs.docker.com/reference/dockerfile/#predefined-args). These proxy values are only available in the build container. They're not included in the build output.

​	要了解可以与 `docker build` 命令一起使用的所有代理相关的构建参数，请参阅 [预定义的 ARGs](https://docs.docker.com/reference/dockerfile/#predefined-args)。这些代理值仅在构建容器中可用，不会包含在构建输出中。

## 构建时使用代理作为环境变量 Proxy as environment variable for builds

Don't use the `ENV` Dockerfile instruction to specify proxy settings for builds. Use build arguments instead.

​	不要使用 Dockerfile 中的 `ENV` 指令来指定构建的代理设置，应使用构建参数。

Using environment variables for proxies embeds the configuration into the image. If the proxy is an internal proxy, it might not be accessible for containers created from that image.

​	使用环境变量设置代理会将配置嵌入镜像中。如果代理是内部代理，可能无法为从该镜像创建的容器访问。

Embedding proxy settings in images also poses a security risk, as the values may include sensitive information.

​	将代理设置嵌入镜像中还可能带来安全风险，因为值可能包含敏感信息。
