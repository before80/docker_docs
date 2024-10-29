+++
title = "保护 Docker 守护进程套接字"
date = 2024-10-23T14:54:40+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/security/protect-access/](https://docs.docker.com/engine/security/protect-access/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Protect the Docker daemon socket - 保护 Docker 守护进程套接字

By default, Docker runs through a non-networked UNIX socket. It can also optionally communicate using SSH or a TLS (HTTPS) socket.

​	默认情况下，Docker 通过非网络化的 UNIX 套接字运行。它还可以选择使用 SSH 或 TLS（HTTPS）套接字进行通信。

## 使用 SSH 保护 Docker 守护进程套接字 Use SSH to protect the Docker daemon socket

> **Note**
>
> 
>
> The given `USERNAME` must have permissions to access the docker socket on the remote machine. Refer to [manage Docker as a non-root user](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user) to learn how to give a non-root user access to the docker socket.
>
> ​	给定的 `USERNAME` 必须具有访问远程机器上 Docker 套接字的权限。有关如何为非 root 用户授予 Docker 套接字访问权限的信息，请参阅 [管理 Docker 作为非 root 用户](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)。

The following example creates a [`docker context`]({{< ref "/manuals/DockerEngine/Manageresources/Dockercontexts" >}}) to connect with a remote `dockerd` daemon on `host1.example.com` using SSH, and as the `docker-user` user on the remote machine:

​	以下示例使用 SSH 创建一个 [`docker context`]({{< ref "/manuals/DockerEngine/Manageresources/Dockercontexts" >}})，以连接到 `host1.example.com` 上的远程 `dockerd` 守护进程，并以远程机器上的 `docker-user` 用户身份运行：



```console
$ docker context create \
    --docker host=ssh://docker-user@host1.example.com \
    --description="Remote engine" \
    my-remote-engine

my-remote-engine
Successfully created context "my-remote-engine"
```

After creating the context, use `docker context use` to switch the `docker` CLI to use it, and to connect to the remote engine:

​	创建 context 后，使用 `docker context use` 切换 `docker` CLI 以使用该 context，并连接到远程引擎：



```console
$ docker context use my-remote-engine
my-remote-engine
Current context is now "my-remote-engine"

$ docker info
<prints output of the remote engine>
```

Use the `default` context to switch back to the default (local) daemon:

​	使用 `default` context 可切换回默认（本地）守护进程：



```console
$ docker context use default
default
Current context is now "default"
```

Alternatively, use the `DOCKER_HOST` environment variable to temporarily switch the `docker` CLI to connect to the remote host using SSH. This does not require creating a context, and can be useful to create an ad-hoc connection with a different engine:

​	或者，使用 `DOCKER_HOST` 环境变量临时切换 `docker` CLI 以使用 SSH 连接到远程主机。此方法不需要创建 context，并且适用于创建与不同引擎的临时连接：



```console
$ export DOCKER_HOST=ssh://docker-user@host1.example.com
$ docker info
<prints output of the remote engine>
```

### SSH Tips

For the best user experience with SSH, configure `~/.ssh/config` as follows to allow reusing a SSH connection for multiple invocations of the `docker` CLI:

​	为了获得最佳的 SSH 用户体验，可以按以下方式配置 `~/.ssh/config`，以便在 `docker` CLI 的多次调用之间重用 SSH 连接：



```text
ControlMaster     auto
ControlPath       ~/.ssh/control-%C
ControlPersist    yes
```

## 使用 TLS（HTTPS）保护 Docker 守护进程套接字 Use TLS (HTTPS) to protect the Docker daemon socket

If you need Docker to be reachable through HTTP rather than SSH in a safe manner, you can enable TLS (HTTPS) by specifying the `tlsverify` flag and pointing Docker's `tlscacert` flag to a trusted CA certificate.

​	如果需要通过 HTTP 而不是 SSH 安全地访问 Docker，可以通过指定 `tlsverify` 标志并将 Docker 的 `tlscacert` 标志指向受信的 CA 证书来启用 TLS（HTTPS）。

In the daemon mode, it only allows connections from clients authenticated by a certificate signed by that CA. In the client mode, it only connects to servers with a certificate signed by that CA.

​	在守护进程模式下，它仅允许由该 CA 签名的客户端证书进行身份验证。在客户端模式下，它仅连接到由该 CA 签名的服务器。

> **Important**
>
> 
>
> Using TLS and managing a CA is an advanced topic. Familiarize yourself with OpenSSL, x509, and TLS before using it in production.
>
> ​	使用 TLS 和管理 CA 是一个高级主题。在生产环境中使用之前，请熟悉 OpenSSL、x509 和 TLS。

### 使用 OpenSSL 创建 CA、服务器和客户端密钥 Create a CA, server and client keys with OpenSSL

> **Note**
>
> 
>
> Replace all instances of `$HOST` in the following example with the DNS name of your Docker daemon's host.
>
> ​	在以下示例中，将所有 `$HOST` 替换为 Docker 守护进程主机的 DNS 名称。

First, on the Docker daemon's host machine, generate CA private and public keys:

​	首先，在 Docker 守护进程的主机上生成 CA 私钥和公钥：



```console
$ openssl genrsa -aes256 -out ca-key.pem 4096
Generating RSA private key, 4096 bit long modulus
..............................................................................++
........++
e is 65537 (0x10001)
Enter pass phrase for ca-key.pem:
Verifying - Enter pass phrase for ca-key.pem:

$ openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem
Enter pass phrase for ca-key.pem:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:
State or Province Name (full name) [Some-State]:Queensland
Locality Name (eg, city) []:Brisbane
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Docker Inc
Organizational Unit Name (eg, section) []:Sales
Common Name (e.g. server FQDN or YOUR name) []:$HOST
Email Address []:Sven@home.org.au
```

Now that you have a CA, you can create a server key and certificate signing request (CSR). Make sure that "Common Name" matches the hostname you use to connect to Docker:

​	生成 CA 后，可以创建服务器密钥和证书签名请求（CSR）。确保“Common Name”与连接 Docker 的主机名匹配：

> **Note**
>
> 
>
> Replace all instances of `$HOST` in the following example with the DNS name of your Docker daemon's host.
>
> ​	将示例中所有的 `$HOST` 替换为 Docker 守护进程主机的 DNS 名称。



```console
$ openssl genrsa -out server-key.pem 4096
Generating RSA private key, 4096 bit long modulus
.....................................................................++
.................................................................................................++
e is 65537 (0x10001)

$ openssl req -subj "/CN=$HOST" -sha256 -new -key server-key.pem -out server.csr
```

Next, we're going to sign the public key with our CA:

​	接下来，我们使用 CA 签署公钥：

Since TLS connections can be made through IP address as well as DNS name, the IP addresses need to be specified when creating the certificate. For example, to allow connections using `10.10.10.20` and `127.0.0.1`:

​	因为 TLS 连接可以通过 IP 地址和 DNS 名称进行，因此需要在创建证书时指定 IP 地址。例如，允许通过 `10.10.10.20` 和 `127.0.0.1` 连接：

```console
$ echo subjectAltName = DNS:$HOST,IP:10.10.10.20,IP:127.0.0.1 >> extfile.cnf
```

Set the Docker daemon key's extended usage attributes to be used only for server authentication:

​	将 Docker 守护进程密钥的扩展使用属性设置为仅用于服务器身份验证：

```console
$ echo extendedKeyUsage = serverAuth >> extfile.cnf
```

Now, generate the signed certificate:

​	生成签名证书：

```console
$ openssl x509 -req -days 365 -sha256 -in server.csr -CA ca.pem -CAkey ca-key.pem \
  -CAcreateserial -out server-cert.pem -extfile extfile.cnf
Signature ok
subject=/CN=your.host.com
Getting CA Private Key
Enter pass phrase for ca-key.pem:
```

[Authorization plugins]({{< ref "/manuals/DockerEngine/DockerEngineplugins/Accessauthorizationplugin" >}}) offer more fine-grained control to supplement authentication from mutual TLS. In addition to other information described in the above document, authorization plugins running on a Docker daemon receive the certificate information for connecting Docker clients.

​	[授权插件]({{< ref "/manuals/DockerEngine/DockerEngineplugins/Accessauthorizationplugin" >}})提供了更细粒度的控制，以补充双向 TLS 身份验证。除了上述文档描述的其他信息外，在 Docker 守护进程上运行的授权插件还会接收连接 Docker 客户端的证书信息。

For client authentication, create a client key and certificate signing request:

​	为了进行客户端身份验证，创建客户端密钥和证书签名请求：

> **Note**
>
> 
>
> For simplicity of the next couple of steps, you may perform this step on the Docker daemon's host machine as well.
>
> ​	为简化后续步骤，也可以在 Docker 守护进程的主机上执行此步骤。



```console
$ openssl genrsa -out key.pem 4096
Generating RSA private key, 4096 bit long modulus
.........................................................++
................++
e is 65537 (0x10001)

$ openssl req -subj '/CN=client' -new -key key.pem -out client.csr
```

To make the key suitable for client authentication, create a new extensions config file:

​	为了使密钥适用于客户端身份验证，请创建一个新的扩展配置文件：

```console
$ echo extendedKeyUsage = clientAuth > extfile-client.cnf
```

Now, generate the signed certificate:

​	生成签名证书：

```console
$ openssl x509 -req -days 365 -sha256 -in client.csr -CA ca.pem -CAkey ca-key.pem \
  -CAcreateserial -out cert.pem -extfile extfile-client.cnf
Signature ok
subject=/CN=client
Getting CA Private Key
Enter pass phrase for ca-key.pem:
```

After generating `cert.pem` and `server-cert.pem` you can safely remove the two certificate signing requests and extensions config files:

​	生成 `cert.pem` 和 `server-cert.pem` 后，可以安全地删除两个证书签名请求和扩展配置文件：

```console
$ rm -v client.csr server.csr extfile.cnf extfile-client.cnf
```

With a default `umask` of 022, your secret keys are *world-readable* and writable for you and your group.

​	在默认 `umask` 为 022 的情况下，您的密钥对所有用户可读，且对您和您的组可写。

To protect your keys from accidental damage, remove their write permissions. To make them only readable by you, change file modes as follows:

​	为了保护密钥免受意外破坏，移除其写权限。为了使它们仅对您可读，请按以下方式更改文件模式：

​	

```console
$ chmod -v 0400 ca-key.pem key.pem server-key.pem
```

Certificates can be world-readable, but you might want to remove write access to prevent accidental damage:

​	证书可以对所有用户可读，但您可能想移除写权限以防止意外损坏：



```console
$ chmod -v 0444 ca.pem server-cert.pem cert.pem
```

Now you can make the Docker daemon only accept connections from clients providing a certificate trusted by your CA:

​	现在可以使 Docker 守护进程仅接受由您的 CA 信任的证书的客户端连接：



```console
$ dockerd \
    --tlsverify \
    --tlscacert=ca.pem \
    --tlscert=server-cert.pem \
    --tlskey=server-key.pem \
    -H=0.0.0.0:2376
```

To connect to Docker and validate its certificate, provide your client keys, certificates and trusted CA:

​	要连接到 Docker 并验证其证书，请提供客户端密钥、证书和受信的 CA：

> **Tip**
>
> 
>
> This step should be run on your Docker client machine. As such, you need to copy your CA certificate, your server certificate, and your client certificate to that machine.
>
> ​	该步骤应在 Docker 客户端机器上运行。因此，您需要将 CA 证书、服务器证书和客户端证书复制到该机器。

> **Note**
>
> 
>
> Replace all instances of `$HOST` in the following example with the DNS name of your Docker daemon's host.
>
> ​	在以下示例中，将所有的 `$HOST` 替换为 Docker 守护进程主机的 DNS 名称。



```console
$ docker --tlsverify \
    --tlscacert=ca.pem \
    --tlscert=cert.pem \
    --tlskey=key.pem \
    -H=$HOST:2376 version
```

> **Note**
>
> 
>
> Docker over TLS should run on TCP port 2376.
>
> ​	使用 TLS 的 Docker 应该在 TCP 端口 2376 上运行。

> **Warning**
>
> 
>
> As shown in the example above, you don't need to run the `docker` client with `sudo` or the `docker` group when you use certificate authentication. That means anyone with the keys can give any instructions to your Docker daemon, giving them root access to the machine hosting the daemon. Guard these keys as you would a root password!
>
> ​	如上例所示，在使用证书认证时，不需要使用 `sudo` 或 `docker` 组来运行 `docker` 客户端。这意味着任何拥有密钥的人都可以向您的 Docker 守护进程发送指令，从而获得托管守护进程的机器的 root 访问权限。请像保护 root 密码一样保护这些密钥！

### 默认安全连接 Secure by default

If you want to secure your Docker client connections by default, you can move the files to the `.docker` directory in your home directory --- and set the `DOCKER_HOST` and `DOCKER_TLS_VERIFY` variables as well (instead of passing `-H=tcp://$HOST:2376` and `--tlsverify` on every call).

​	如果希望默认情况下将 Docker 客户端连接设置为安全连接，可以将文件移动到主目录下的 `.docker` 目录中，并设置 `DOCKER_HOST` 和 `DOCKER_TLS_VERIFY` 变量（而不是在每次调用中传递 `-H=tcp://$HOST:2376` 和 `--tlsverify`）。



```console
$ mkdir -pv ~/.docker
$ cp -v {ca,cert,key}.pem ~/.docker

$ export DOCKER_HOST=tcp://$HOST:2376 DOCKER_TLS_VERIFY=1
```

Docker now connects securely by default:

​	现在，Docker 默认安全连接：

```
$ docker ps
```

### 其他模式 Other modes

If you don't want to have complete two-way authentication, you can run Docker in various other modes by mixing the flags.

​	如果不想完全使用双向身份验证，可以通过组合不同的标志以不同模式运行 Docker。

#### Daemon modes

- `tlsverify`, `tlscacert`, `tlscert`, `tlskey` set: Authenticate clients

  - 设置 `tlsverify`、`tlscacert`、`tlscert`、`tlskey`：认证客户端

- `tls`, `tlscert`, `tlskey`: Do not authenticate clients

  - 设置 `tls`、`tlscert`、`tlskey`：不认证客户端

  

#### Client modes

- `tls`: Authenticate server based on public/default CA pool
  - 设置 `tls`：根据公用/默认 CA 池认证服务器

- `tlsverify`, `tlscacert`: Authenticate server based on given CA
  - 设置 `tlsverify`、`tlscacert`：根据给定的 CA 认证服务器

- `tls`, `tlscert`, `tlskey`: Authenticate with client certificate, do not authenticate server based on given CA
  - 设置 `tls`、`tlscert`、`tlskey`：用客户端证书认证，但不根据给定 CA 认证服务器

- `tlsverify`, `tlscacert`, `tlscert`, `tlskey`: Authenticate with client certificate and authenticate server based on given CA
  - 设置 `tlsverify`、`tlscacert`、`tlscert`、`tlskey`：用客户端证书认证并根据给定 CA 认证服务器


If found, the client sends its client certificate, so you just need to drop your keys into `~/.docker/{ca,cert,key}.pem`. Alternatively, if you want to store your keys in another location, you can specify that location using the environment variable `DOCKER_CERT_PATH`.

​	如果找到，客户端会发送其客户端证书，因此只需将密钥放入 `~/.docker/{ca,cert,key}.pem` 中即可。或者，如果想将密钥存储在其他位置，可以使用环境变量 `DOCKER_CERT_PATH` 指定该位置。



```console
$ export DOCKER_CERT_PATH=~/.docker/zone1/
$ docker --tlsverify ps
```

#### 使用 `curl` 连接安全 Docker 端口 Connecting to the secure Docker port using `curl`

To use `curl` to make test API requests, you need to use three extra command line flags:

​	要使用 `curl` 进行测试 API 请求，需要使用三个额外的命令行标志：



```console
$ curl https://$HOST:2376/images/json \
  --cert ~/.docker/cert.pem \
  --key ~/.docker/key.pem \
  --cacert ~/.docker/ca.pem
```

## 相关信息 Related information

- [Using certificates for repository client verification 使用证书进行仓库客户端验证]({{< ref "/manuals/DockerEngine/Security/Verifyrepositoryclientwithcertificates" >}})
- [Use trusted images 使用受信任的镜像]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker" >}})
