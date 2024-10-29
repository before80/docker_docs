+++
title = "使用证书验证仓库客户端"
date = 2024-10-23T14:54:40+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/security/certificates/](https://docs.docker.com/engine/security/certificates/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Verify repository client with certificates - 使用证书验证仓库客户端

In [Running Docker with HTTPS]({{< ref "/manuals/DockerEngine/Security/ProtecttheDockerdaemonsocket" >}}), you learned that, by default, Docker runs via a non-networked Unix socket and TLS must be enabled in order to have the Docker client and the daemon communicate securely over HTTPS. TLS ensures authenticity of the registry endpoint and that traffic to/from registry is encrypted.

​	在[使用 HTTPS 运行 Docker]({{< ref "/manuals/DockerEngine/Security/ProtecttheDockerdaemonsocket" >}})中，您了解到默认情况下，Docker 通过非网络化的 Unix 套接字运行，必须启用 TLS 才能让 Docker 客户端和守护进程通过 HTTPS 安全通信。TLS 确保了注册表端点的真实性，并加密了与注册表之间的通信。

This article demonstrates how to ensure the traffic between the Docker registry server and the Docker daemon (a client of the registry server) is encrypted and properly authenticated using certificate-based client-server authentication.

​	本文演示如何使用基于证书的客户端-服务器身份验证来确保 Docker 注册表服务器和 Docker 守护进程（注册表服务器的客户端）之间的流量是加密且经过适当验证的。

We show you how to install a Certificate Authority (CA) root certificate for the registry and how to set the client TLS certificate for verification.

​	我们将向您展示如何为注册表安装证书颁发机构（CA）根证书以及如何设置客户端 TLS 证书以进行验证。

## 理解配置Understand the configuration

A custom certificate is configured by creating a directory under `/etc/docker/certs.d` using the same name as the registry's hostname, such as `localhost`. All `*.crt` files are added to this directory as CA roots.

​	通过在 `/etc/docker/certs.d` 下创建与注册表主机名（例如 `localhost`）相同的目录来配置自定义证书。所有 `*.crt` 文件将被添加到此目录中作为 CA 根证书。

> **Note**
>
> 
>
> On Linux any root certificates authorities are merged with the system defaults, including the host's root CA set. If you are running Docker on Windows Server, or Docker Desktop for Windows with Windows containers, the system default certificates are only used when no custom root certificates are configured.
>
> ​	在 Linux 上，任何根证书颁发机构都会与系统默认的根证书合并，包括主机的根 CA 集。如果您在 Windows Server 上运行 Docker，或在使用 Windows 容器的 Docker Desktop for Windows 上运行 Docker，系统默认证书仅在未配置自定义根证书时使用。

The presence of one or more `<filename>.key/cert` pairs indicates to Docker that there are custom certificates required for access to the desired repository.

​	存在一个或多个 `<filename>.key/cert` 对时，Docker 会识别到需要自定义证书来访问目标仓库。

> **Note**
>
> 
>
> If multiple certificates exist, each is tried in alphabetical order. If there is a 4xx-level or 5xx-level authentication error, Docker continues to try with the next certificate.
>
> ​	如果存在多个证书，每个证书将按字母顺序依次尝试。如果出现 4xx 或 5xx 级别的身份验证错误，Docker 将继续尝试下一个证书。

The following illustrates a configuration with custom certificates:

​	以下展示了一个具有自定义证书的配置：



```text
    /etc/docker/certs.d/        <-- Certificate directory
    └── localhost:5000          <-- Hostname:port
       ├── client.cert          <-- Client certificate
       ├── client.key           <-- Client key
       └── ca.crt               <-- Root CA that signed
                                    the registry certificate, in PEM
```

The preceding example is operating-system specific and is for illustrative purposes only. You should consult your operating system documentation for creating an os-provided bundled certificate chain.

​	上面的示例是特定于操作系统的，仅用于说明。您应查阅操作系统文档以创建由操作系统提供的捆绑证书链。

## 创建客户端证书 Create the client certificates

Use OpenSSL's `genrsa` and `req` commands to first generate an RSA key and then use the key to create the certificate.

​	使用 OpenSSL 的 `genrsa` 和 `req` 命令首先生成一个 RSA 密钥，然后使用该密钥创建证书。



```console
$ openssl genrsa -out client.key 4096
$ openssl req -new -x509 -text -key client.key -out client.cert
```

> **Note**
>
> 
>
> These TLS commands only generate a working set of certificates on Linux. The version of OpenSSL in macOS is incompatible with the type of certificate Docker requires.
>
> ​	这些 TLS 命令仅在 Linux 上生成有效的证书。macOS 中的 OpenSSL 版本与 Docker 所需的证书类型不兼容。

## 故障排除提示 Troubleshooting tips

The Docker daemon interprets `.crt` files as CA certificates and `.cert` files as client certificates. If a CA certificate is accidentally given the extension `.cert` instead of the correct `.crt` extension, the Docker daemon logs the following error message:

​	Docker 守护进程将 `.crt` 文件解释为 CA 证书，将 `.cert` 文件解释为客户端证书。如果 CA 证书错误地使用了 `.cert` 扩展名而非正确的 `.crt` 扩展名，Docker 守护进程将记录以下错误信息：



```text
Missing key KEY_NAME for client certificate CERT_NAME. CA certificates should use the extension .crt.
```

If the Docker registry is accessed without a port number, do not add the port to the directory name. The following shows the configuration for a registry on default port 443 which is accessed with `docker login my-https.registry.example.com`:

​	如果在不带端口号的情况下访问 Docker 注册表，请不要在目录名中添加端口。以下示例展示了在默认端口 443 上访问的注册表配置，通过 `docker login my-https.registry.example.com` 访问：



```text
    /etc/docker/certs.d/
    └── my-https.registry.example.com          <-- Hostname without port
       ├── client.cert
       ├── client.key
       └── ca.crt
```

## 相关信息 Related information

- [Use trusted images 使用受信任的镜像]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker" >}})
- [Protect the Docker daemon socket 保护 Docker 守护进程套接字]({{< ref "/manuals/DockerEngine/Security/ProtecttheDockerdaemonsocket" >}})
