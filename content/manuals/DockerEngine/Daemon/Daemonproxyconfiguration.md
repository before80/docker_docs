+++
title = "Docker 守护进程代理配置"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/daemon/proxy/](https://docs.docker.com/engine/daemon/proxy/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Daemon proxy configuration - Docker 守护进程代理配置



If your organization uses a proxy server to connect to the internet, you may need to configure the Docker daemon to use the proxy server. The daemon uses a proxy server to access images stored on Docker Hub and other registries, and to reach other nodes in a Docker swarm.

​	如果您的组织使用代理服务器连接互联网，您可能需要配置 Docker 守护进程以使用代理服务器。守护进程使用代理服务器访问 Docker Hub 和其他注册表中的镜像，并访问 Docker swarm 中的其他节点。

This page describes how to configure a proxy for the Docker daemon. For instructions on configuring proxy settings for the Docker CLI, see [Configure Docker CLI to use a proxy server]({{< ref "/manuals/DockerEngine/CLI/Proxyconfiguration" >}}).

​	本页描述了如何为 Docker 守护进程配置代理。关于为 Docker CLI 配置代理的说明，请参阅 [配置 Docker CLI 使用代理服务器]({{< ref "/manuals/DockerEngine/CLI/Proxyconfiguration" >}})。

There are two ways you can configure these settings:

​	您可以通过以下两种方式配置这些设置：

- [Configuring the daemon](https://docs.docker.com/engine/daemon/proxy/#daemon-configuration) through a configuration file or CLI flags
  - 通过配置文件或 CLI 标志 [直接配置守护进程](https://docs.docker.com/engine/daemon/proxy/#daemon-configuration)

- Setting [environment variables](https://docs.docker.com/engine/daemon/proxy/#environment-variables) on the system
  - 在系统上设置 [环境变量](https://docs.docker.com/engine/daemon/proxy/#environment-variables)


Configuring the daemon directly takes precedence over environment variables.

​	直接配置守护进程优先于环境变量。

## 守护进程配置 Daemon configuration

You may configure proxy behavior for the daemon in the `daemon.json` file, or using CLI flags for the `--http-proxy` or `--https-proxy` flags for the `dockerd` command. Configuration using `daemon.json` is recommended.

​	您可以在 `daemon.json` 文件中配置守护进程的代理行为，或使用 `dockerd` 命令的 `--http-proxy` 或 `--https-proxy` 标志。推荐使用 `daemon.json` 进行配置。

```json
{
  "proxies": {
    "http-proxy": "http://proxy.example.com:3128",
    "https-proxy": "https://proxy.example.com:3129",
    "no-proxy": "*.test.example.com,.example.org,127.0.0.0/8"
  }
}
```

After changing the configuration file, restart the daemon for the proxy configuration to take effect:

​	更改配置文件后，重启守护进程以使代理配置生效：



```console
$ sudo systemctl restart docker
```

## 环境变量 Environment variables

The Docker daemon checks the following environment variables in its start-up environment to configure HTTP or HTTPS proxy behavior:

​	Docker 守护进程在启动环境中检查以下环境变量以配置 HTTP 或 HTTPS 代理行为：

- `HTTP_PROXY`

- `http_proxy`
- `HTTPS_PROXY`
- `https_proxy`
- `NO_PROXY`
- `no_proxy`

### systemd 单元文件 systemd unit file

If you're running the Docker daemon as a systemd service, you can create a systemd drop-in file that sets the variables for the `docker` service.

​	如果您将 Docker 守护进程作为 systemd 服务运行，可以为 `docker` 服务创建一个 systemd drop-in 文件来设置变量。

> **Note for rootless mode - Rootless 模式的注意事项**
>
> The location of systemd configuration files are different when running Docker in [rootless mode]({{< ref "/manuals/DockerEngine/Security/Rootlessmode" >}}). When running in rootless mode, Docker is started as a user-mode systemd service, and uses files stored in each users' home directory in `~/.config/systemd/<user>/docker.service.d/`. In addition, `systemctl` must be executed without `sudo` and with the `--user` flag. Select the "Rootless mode" tab if you are running Docker in rootless mode.
>
> ​	在 [rootless 模式]({{< ref "/manuals/DockerEngine/Security/Rootlessmode" >}}) 下运行 Docker 时，systemd 配置文件的位置不同。此模式下 Docker 以用户模式的 systemd 服务启动，配置文件位于用户的主目录中 `~/.config/systemd/<user>/docker.service.d/`。此外，`systemctl` 必须在不使用 `sudo` 且带有 `--user` 标志时执行。如果您在 rootless 模式下运行 Docker，请选择“Rootless 模式”选项卡。

{{< tabpane text=true persist=disabled >}}

{{% tab header="Regular install" %}}

1. Create a systemd drop-in directory for the `docker` service:

   为 `docker` 服务创建一个 systemd drop-in 目录：

   ```console
   $ sudo mkdir -p /etc/systemd/system/docker.service.d
   ```

2. Create a file named `/etc/systemd/system/docker.service.d/http-proxy.conf` that adds the `HTTP_PROXY` environment variable:

   创建一个名为 `/etc/systemd/system/docker.service.d/http-proxy.conf` 的文件，添加 `HTTP_PROXY` 环境变量：

   ```systemd
   [Service]
   Environment="HTTP_PROXY=http://proxy.example.com:3128"
   ```

   If you are behind an HTTPS proxy server, set the `HTTPS_PROXY` environment variable:

   如果您在 HTTPS 代理服务器后面，设置 `HTTPS_PROXY` 环境变量：

   ```systemd
   [Service]
   Environment="HTTPS_PROXY=https://proxy.example.com:3129"
   ```

   Multiple environment variables can be set; to set both a non-HTTPS and a HTTPs proxy;

   可以设置多个环境变量，以同时配置 HTTP 和 HTTPS 代理：

   ```systemd
   [Service]
   Environment="HTTP_PROXY=http://proxy.example.com:3128"
   Environment="HTTPS_PROXY=https://proxy.example.com:3129"
   ```

   > **Note**
   >
   > 
   >
   > Special characters in the proxy value, such as `#?!()[]{}`, must be double escaped using `%%`. For example:
   >
   > ​	代理值中的特殊字符，如 `#?!()[]{}`，必须使用 `%%` 双重转义。例如：
   >
   > ```systemd
   > [Service]
   > Environment="HTTP_PROXY=http://domain%%5Cuser:complex%%23pass@proxy.example.com:3128/"
   > ```

3. If you have internal Docker registries that you need to contact without proxying, you can specify them via the `NO_PROXY` environment variable. 如果您有需要不通过代理访问的内部 Docker 注册表，可以通过 `NO_PROXY` 环境变量指定它们。

   The `NO_PROXY` variable specifies a string that contains comma-separated values for hosts that should be excluded from proxying. These are the options you can specify to exclude hosts:

   ​	`NO_PROXY` 变量指定一个字符串，其中包含不应通过代理访问的主机的逗号分隔值。以下是可用于排除主机的选项：

   - IP address prefix (`1.2.3.4`)
     - IP 地址前缀（如 `1.2.3.4`）

   - Domain name, or a special DNS label (`*`)
     - 域名或特殊 DNS 标签（如 `*`）

   - A domain name matches that name and all subdomains. A domain name with a leading "." matches subdomains only. For example, given the domains `foo.example.com` and `example.com`: 一个域名匹配该名称及所有子域名。以 `.` 开头的域名仅匹配子域名。例如，给定域名`foo.example.com` 和`example.com`: 
     - `example.com` matches `example.com` and `foo.example.com`, and
       - `example.com` 匹配 `example.com` 和 `foo.example.com`，而
     - `.example.com` matches only `foo.example.com`
       - `.example.com` 仅匹配 `foo.example.com`
   - A single asterisk (`*`) indicates that no proxying should be done
     - 单个星号 (`*`) 表示不应进行任何代理

   - Literal port numbers are accepted by IP address prefixes (`1.2.3.4:80`) and domain names (`foo.example.com:80`)
     - IP 地址前缀（如 `1.2.3.4:80`）和域名（如 `foo.example.com:80`）可接受端口号


   Example:

   ​	示例：

   ```systemd
   [Service]
   Environment="HTTP_PROXY=http://proxy.example.com:3128"
   Environment="HTTPS_PROXY=https://proxy.example.com:3129"
   Environment="NO_PROXY=localhost,127.0.0.1,docker-registry.example.com,.corp"
   ```

4. Flush changes and restart Docker

   刷新更改并重启 Docker：

   ```console
   $ sudo systemctl daemon-reload
   $ sudo systemctl restart docker
   ```

5. Verify that the configuration has been loaded and matches the changes you made, for example:

   验证配置已加载并与您所做的更改一致，例如：

   ```console
   $ sudo systemctl show --property=Environment docker
   
   Environment=HTTP_PROXY=http://proxy.example.com:3128 HTTPS_PROXY=https://proxy.example.com:3129 NO_PROXY=localhost,127.0.0.1,docker-registry.example.com,.corp
   ```



{{% /tab  %}}

{{% tab header="Rootless mode" %}}

1. Create a systemd drop-in directory for the `docker` service:

   为 `docker` 服务创建一个 systemd drop-in 目录：

   ```console
   $ mkdir -p ~/.config/systemd/user/docker.service.d
   ```

2. Create a file named `~/.config/systemd/user/docker.service.d/http-proxy.conf` that adds the `HTTP_PROXY` environment variable:

   创建一个名为 `~/.config/systemd/user/docker.service.d/http-proxy.conf` 的文件，以添加 `HTTP_PROXY` 环境变量：

   ```systemd
   [Service]
   Environment="HTTP_PROXY=http://proxy.example.com:3128"
   ```

   If you are behind an HTTPS proxy server, set the `HTTPS_PROXY` environment variable:

   ​	如果您在 HTTPS 代理服务器之后，则设置 `HTTPS_PROXY` 环境变量：

   ```systemd
   [Service]
   Environment="HTTPS_PROXY=https://proxy.example.com:3129"
   ```

   Multiple environment variables can be set; to set both a non-HTTPS and a HTTPs proxy;

   ​	可以设置多个环境变量；设置 HTTP 和 HTTPS 代理：

   ```systemd
   [Service]
   Environment="HTTP_PROXY=http://proxy.example.com:3128"
   Environment="HTTPS_PROXY=https://proxy.example.com:3129"
   ```

   > **Note**
   >
   > 
   >
   > Special characters in the proxy value, such as `#?!()[]{}`, must be double escaped using `%%`. For example:
   >
   > ​	代理值中的特殊字符，如 `#?!()[]{}`，必须使用 `%%` 双重转义。例如：
   >
   > ```systemd
   > [Service]
   > Environment="HTTP_PROXY=http://domain%%5Cuser:complex%%23pass@proxy.example.com:3128/"
   > ```

3. If you have internal Docker registries that you need to contact without proxying, you can specify them via the `NO_PROXY` environment variable. 如果您有无需代理访问的内部 Docker 注册表，可以通过 `NO_PROXY` 环境变量指定。

   The `NO_PROXY` variable specifies a string that contains comma-separated values for hosts that should be excluded from proxying. These are the options you can specify to exclude hosts:

   ​	`NO_PROXY` 变量指定了一个包含以逗号分隔的主机值的字符串，这些主机将被排除在代理之外。可以指定以下选项来排除主机：

   - IP address prefix (`1.2.3.4`)
     - IP 地址前缀（如 `1.2.3.4`）
   - Domain name, or a special DNS label (`*`)
     - 域名，或一个特殊的 DNS 标签（如 `*`）
   - A domain name matches that name and all subdomains. A domain name with a leading "." matches subdomains only. For example, given the domains `foo.example.com` and `example.com`:  域名匹配该名称及其所有子域名。以 "." 开头的域名仅匹配子域。例如，给定以下域名`foo.example.com` 和`example.com`: 
     - `example.com` matches `example.com` and `foo.example.com`, and
       - `example.com` 匹配 `example.com` 和 `foo.example.com`，而
     - `.example.com` matches only `foo.example.com`
       - `.example.com` 仅匹配 `foo.example.com`
   - A single asterisk (`*`) indicates that no proxying should be done
     - 单个星号（`*`）表示不进行代理
   - Literal port numbers are accepted by IP address prefixes (`1.2.3.4:80`) and domain names (`foo.example.com:80`)
     - IP 地址前缀（如 `1.2.3.4:80`）和域名（如 `foo.example.com:80`）接受字面端口号

   Example:

   ​	示例：

   ```systemd
   [Service]
   Environment="HTTP_PROXY=http://proxy.example.com:3128"
   Environment="HTTPS_PROXY=https://proxy.example.com:3129"
   Environment="NO_PROXY=localhost,127.0.0.1,docker-registry.example.com,.corp"
   ```

4. Flush changes and restart Docker

   刷新更改并重新启动 Docker：

   ```console
   $ systemctl --user daemon-reload
   $ systemctl --user restart docker
   ```

5. Verify that the configuration has been loaded and matches the changes you made, for example:

   验证配置是否已加载并匹配您所做的更改，例如：

   ```console
   $ systemctl --user show --property=Environment docker
   
   Environment=HTTP_PROXY=http://proxy.example.com:3128 HTTPS_PROXY=https://proxy.example.com:3129 NO_PROXY=localhost,127.0.0.1,docker-registry.example.com,.corp
   ```

{{% /tab  %}}

{{< /tabpane >}}


