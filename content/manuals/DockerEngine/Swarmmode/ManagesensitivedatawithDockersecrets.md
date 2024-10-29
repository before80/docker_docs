+++
title = "使用 Docker Secrets 管理敏感数据"
date = 2024-10-23T14:54:40+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/secrets/](https://docs.docker.com/engine/swarm/secrets/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Manage sensitive data with Docker secrets - 使用 Docker Secrets 管理敏感数据

## 关于 Secrets - About secrets

In terms of Docker Swarm services, a *secret* is a blob of data, such as a password, SSH private key, SSL certificate, or another piece of data that should not be transmitted over a network or stored unencrypted in a Dockerfile or in your application's source code. You can use Docker *secrets* to centrally manage this data and securely transmit it to only those containers that need access to it. Secrets are encrypted during transit and at rest in a Docker swarm. A given secret is only accessible to those services which have been granted explicit access to it, and only while those service tasks are running.

​	在 Docker Swarm 服务中，*secret* 是一段数据，例如密码、SSH 私钥、SSL 证书，或其他不应通过网络传输或在 Dockerfile 或应用程序的源代码中以未加密形式存储的数据。您可以使用 Docker *secrets* 来集中管理这些数据，并将其安全传输到仅需要访问它的容器中。Secrets 在 Docker swarm 中传输和存储时均被加密。只有那些明确获得访问权限的服务才能访问给定的 secret，且只有当这些服务任务正在运行时才可访问。

You can use secrets to manage any sensitive data which a container needs at runtime but you don't want to store in the image or in source control, such as:

​	您可以使用 secrets 管理任何容器在运行时需要但不希望存储在镜像或源代码控制中的敏感数据，例如：

- Usernames and passwords
  - 用户名和密码

- TLS certificates and keys
  - TLS 证书和密钥

- SSH keys
  - SSH 密钥

- Other important data such as the name of a database or internal server
  - 其他重要数据，如数据库名称或内部服务器地址

- Generic strings or binary content (up to 500 kb in size)
  - 通用字符串或二进制内容（最大 500 KB）

> **Note**
>
> 
>
> Docker secrets are only available to swarm services, not to standalone containers. To use this feature, consider adapting your container to run as a service. Stateful containers can typically run with a scale of 1 without changing the container code.
>
> ​	Docker secrets 仅适用于 Swarm 服务，不适用于独立容器。要使用此功能，可以考虑将容器配置为以服务形式运行。状态容器通常可以在不更改容器代码的情况下以 1 的规模运行。

Another use case for using secrets is to provide a layer of abstraction between the container and a set of credentials. Consider a scenario where you have separate development, test, and production environments for your application. Each of these environments can have different credentials, stored in the development, test, and production swarms with the same secret name. Your containers only need to know the name of the secret to function in all three environments.

​	另一种使用 secrets 的情况是为容器和一组凭据之间提供抽象层。例如，您的应用程序拥有不同的开发、测试和生产环境。每个环境可以在开发、测试和生产 Swarm 中存储具有相同 secret 名称的不同凭据。容器只需要知道 secret 的名称即可在所有三个环境中正常工作。

You can also use secrets to manage non-sensitive data, such as configuration files. However, Docker supports the use of [configs]({{< ref "/manuals/DockerEngine/Swarmmode/StoreconfigurationdatausingDockerConfigs" >}}) for storing non-sensitive data. Configs are mounted into the container's filesystem directly, without the use of a RAM disk.

​	您还可以使用 secrets 管理非敏感数据，如配置文件。不过，Docker 支持使用 [configs]({{< ref "/manuals/DockerEngine/Swarmmode/StoreconfigurationdatausingDockerConfigs" >}}) 来存储非敏感数据。Configs 会直接挂载到容器的文件系统中，无需使用 RAM 磁盘。

### Windows 支持 Windows support

Docker includes support for secrets on Windows containers. Where there are differences in the implementations, they are called out in the examples below. Keep the following notable differences in mind:

​	Docker 包含对 Windows 容器中 secrets 的支持。实现上的差异在以下方面被标注出来，需注意以下重要差异：

- Microsoft Windows has no built-in driver for managing RAM disks, so within running Windows containers, secrets are persisted in clear text to the container's root disk. However, the secrets are explicitly removed when a container stops. In addition, Windows does not support persisting a running container as an image using `docker commit` or similar commands.
  - Microsoft Windows 没有内置的 RAM 磁盘驱动程序，因此在 Windows 容器中运行时，secrets 将以明文形式持久保存到容器的根磁盘。不过，容器停止时会显式删除这些 secrets。此外，Windows 不支持使用 `docker commit` 或类似命令将正在运行的容器保存为镜像。

- On Windows, we recommend enabling [BitLocker](https://technet.microsoft.com/en-us/library/cc732774(v=ws.11).aspx) on the volume containing the Docker root directory on the host machine to ensure that secrets for running containers are encrypted at rest.
  - 在 Windows 上，建议在主机上的 Docker 根目录所在的卷上启用 [BitLocker](https://technet.microsoft.com/en-us/library/cc732774(v=ws.11).aspx) 以确保运行中的容器的 secrets 在静态时加密。

- Secret files with custom targets are not directly bind-mounted into Windows containers, since Windows does not support non-directory file bind-mounts. Instead, secrets for a container are all mounted in `C:\ProgramData\Docker\internal\secrets` (an implementation detail which should not be relied upon by applications) within the container. Symbolic links are used to point from there to the desired target of the secret within the container. The default target is `C:\ProgramData\Docker\secrets`.
  - 自定义目标的 secret 文件不会直接绑定挂载到 Windows 容器中，因为 Windows 不支持非目录文件绑定挂载。相反，容器的所有 secrets 都挂载到 `C:\ProgramData\Docker\internal\secrets` 中（应用程序不应依赖此实现细节）。符号链接用于指向容器内的 secrets 目标。默认目标为 `C:\ProgramData\Docker\secrets`。

- When creating a service which uses Windows containers, the options to specify UID, GID, and mode are not supported for secrets. Secrets are currently only accessible by administrators and users with `system` access within the container.
  - 创建使用 Windows 容器的服务时，不支持为 secrets 指定 UID、GID 和模式。当前仅管理员和具有 `system` 访问权限的用户可以在容器内访问 secrets。

## Docker 如何管理 Secrets - How Docker manages secrets

When you add a secret to the swarm, Docker sends the secret to the swarm manager over a mutual TLS connection. The secret is stored in the Raft log, which is encrypted. The entire Raft log is replicated across the other managers, ensuring the same high availability guarantees for secrets as for the rest of the swarm management data.

​	当您将 secret 添加到 Swarm 时，Docker 通过互相的 TLS 连接将 secret 发送到 Swarm 管理器。Secret 存储在加密的 Raft 日志中。整个 Raft 日志在其他管理器之间复制，从而保证与 Swarm 管理数据相同的高可用性。

When you grant a newly-created or running service access to a secret, the decrypted secret is mounted into the container in an in-memory filesystem. The location of the mount point within the container defaults to `/run/secrets/<secret_name>` in Linux containers, or `C:\ProgramData\Docker\secrets` in Windows containers. You can also specify a custom location.

​	当您授予新创建或正在运行的服务访问某个 secret 的权限时，该解密后的 secret 将挂载到容器中的内存文件系统中。在 Linux 容器中，挂载点的位置默认位于 `/run/secrets/<secret_name>`，在 Windows 容器中位于 `C:\ProgramData\Docker\secrets`。您也可以指定自定义位置。

You can update a service to grant it access to additional secrets or revoke its access to a given secret at any time.

​	可以随时更新服务以授予其对其他 secrets 的访问权限或撤销其对某个 secret 的访问。

A node only has access to (encrypted) secrets if the node is a swarm manager or if it is running service tasks which have been granted access to the secret. When a container task stops running, the decrypted secrets shared to it are unmounted from the in-memory filesystem for that container and flushed from the node's memory.

​	只有当节点是 Swarm 管理器或正在运行获得访问权限的服务任务时，该节点才能访问（加密的）secrets。当容器任务停止运行时，分配给它的解密 secret 会从该容器的内存文件系统中卸载，并从节点内存中清除。

If a node loses connectivity to the swarm while it is running a task container with access to a secret, the task container still has access to its secrets, but cannot receive updates until the node reconnects to the swarm.

​	如果节点在运行具有 secret 访问权限的任务容器时与 Swarm 失去连接，任务容器仍可访问其 secrets，但在节点重新连接到 Swarm 之前无法接收更新。

You can add or inspect an individual secret at any time, or list all secrets. You cannot remove a secret that a running service is using. See [Rotate a secret](https://docs.docker.com/engine/swarm/secrets/#example-rotate-a-secret) for a way to remove a secret without disrupting running services.

​	您可以随时添加或检查单个 secret，或列出所有 secrets。但不能删除正在运行的服务使用的 secret。查看 [轮换 secret](https://docs.docker.com/engine/swarm/secrets/#example-rotate-a-secret) 以了解如何在不中断正在运行的服务的情况下删除 secret。

To update or roll back secrets more easily, consider adding a version number or date to the secret name. This is made easier by the ability to control the mount point of the secret within a given container.

​	为便于更新或回滚 secrets，建议在 secret 名称中添加版本号或日期。通过控制 secret 在特定容器内的挂载点可以更容易地实现此操作。

## 了解更多 `docker secret` 命令 Read more about `docker secret` commands

Use these links to read about specific commands, or continue to the [example about using secrets with a service](https://docs.docker.com/engine/swarm/secrets/#simple-example-get-started-with-secrets).

​	使用以下链接了解特定命令，或继续查看[使用服务的 secrets 示例](https://docs.docker.com/engine/swarm/secrets/#simple-example-get-started-with-secrets)。

- [`docker secret create`]({{< ref "/reference/CLIreference/docker/dockersecret/dockersecretcreate" >}})
- [`docker secret inspect`]({{< ref "/reference/CLIreference/docker/dockersecret/dockersecretinspect" >}})
- [`docker secret ls`]({{< ref "/reference/CLIreference/docker/dockersecret/dockersecretls" >}})
- [`docker secret rm`]({{< ref "/reference/CLIreference/docker/dockersecret/dockersecretrm" >}})
- [`--secret`](https://docs.docker.com/reference/cli/docker/service/create/#secret) flag for `docker service create`
- [`--secret-add` and `--secret-rm`](https://docs.docker.com/reference/cli/docker/service/update/#secret-add) flags for `docker service update`

## 示例 Examples

This section includes three graduated examples which illustrate how to use Docker secrets. The images used in these examples have been updated to make it easier to use Docker secrets. To find out how to modify your own images in a similar way, see [Build support for Docker Secrets into your images](https://docs.docker.com/engine/swarm/secrets/#build-support-for-docker-secrets-into-your-images).

​	此部分包含三个分级示例，展示了如何使用 Docker secrets。示例中使用的镜像已更新，使得更容易使用 Docker secrets。要了解如何以类似方式修改您自己的镜像，请参阅 [在镜像中构建 Docker Secrets 支持](https://docs.docker.com/engine/swarm/secrets/#build-support-for-docker-secrets-into-your-images)。

> **Note**
>
> 
>
> These examples use a single-Engine swarm and unscaled services for simplicity. The examples use Linux containers, but Windows containers also support secrets. See [Windows support](https://docs.docker.com/engine/swarm/secrets/#windows-support).
>
> ​	这些示例为简单起见使用单引擎 Swarm 和未扩展的服务。示例使用 Linux 容器，但 Windows 容器同样支持 secrets。请参阅 [Windows 支持](https://docs.docker.com/engine/swarm/secrets/#windows-support)。

### 在 Compose 文件中定义和使用 Secrets - Defining and using secrets in compose files

Both the `docker-compose` and `docker stack` commands support defining secrets in a compose file. See [the Compose file reference]({{< ref "/reference/Composefilereference/Legacyversions" >}}) for details.

​	`docker-compose` 和 `docker stack` 命令均支持在 compose 文件中定义 secrets。详见 [Compose 文件参考]({{< ref "/reference/Composefilereference/Legacyversions" >}})。

### 简单示例：开始使用 secrets - Simple example: Get started with secrets

This simple example shows how secrets work in just a few commands. For a real-world example, continue to [Intermediate example: Use secrets with a Nginx service](https://docs.docker.com/engine/swarm/secrets/#intermediate-example-use-secrets-with-a-nginx-service).

​	此简单示例展示了仅需几个命令即可使用 secrets。若要查看真实案例，请继续查看[中级示例：将 secrets 与 Nginx 服务一起使用](https://docs.docker.com/engine/swarm/secrets/#intermediate-example-use-secrets-with-a-nginx-service)。

1. Add a secret to Docker. The `docker secret create` command reads standard input because the last argument, which represents the file to read the secret from, is set to `-`.

   向 Docker 添加一个 secret。`docker secret create` 命令读取标准输入，因为最后一个参数（表示要从中读取 secret 的文件）设置为 `-`。

   ```console
   $ printf "This is a secret" | docker secret create my_secret_data -
   ```

2. Create a `redis` service and grant it access to the secret. By default, the container can access the secret at `/run/secrets/<secret_name>`, but you can customize the file name on the container using the `target` option.

   创建一个 `redis` 服务并授予其访问该 secret 的权限。默认情况下，容器可以在 `/run/secrets/<secret_name>` 访问该 secret，但您可以使用 `target` 选项自定义容器中的文件名。

   ```console
   $ docker service  create --name redis --secret my_secret_data redis:alpine
   ```

3. Verify that the task is running without issues using `docker service ps`. If everything is working, the output looks similar to this:

   使用 `docker service ps` 验证任务是否正常运行。如果一切正常，输出类似如下：

   ```console
   $ docker service ps redis
   
   ID            NAME     IMAGE         NODE              DESIRED STATE  CURRENT STATE          ERROR  PORTS
   bkna6bpn8r1a  redis.1  redis:alpine  ip-172-31-46-109  Running        Running 8 seconds ago  
   ```

   If there were an error, and the task were failing and repeatedly restarting, you would see something like this:

   如果有错误且任务失败并反复重启，则会看到如下内容：

   ```console
   $ docker service ps redis
   
   NAME                      IMAGE         NODE  DESIRED STATE  CURRENT STATE          ERROR                      PORTS
   redis.1.siftice35gla      redis:alpine  moby  Running        Running 4 seconds ago                             
    \_ redis.1.whum5b7gu13e  redis:alpine  moby  Shutdown       Failed 20 seconds ago      "task: non-zero exit (1)"  
    \_ redis.1.2s6yorvd9zow  redis:alpine  moby  Shutdown       Failed 56 seconds ago      "task: non-zero exit (1)"  
    \_ redis.1.ulfzrcyaf6pg  redis:alpine  moby  Shutdown       Failed about a minute ago  "task: non-zero exit (1)"  
    \_ redis.1.wrny5v4xyps6  redis:alpine  moby  Shutdown       Failed 2 minutes ago       "task: non-zero exit (1)"
   ```

4. Get the ID of the `redis` service task container using `docker ps` , so that you can use `docker container exec` to connect to the container and read the contents of the secret data file, which defaults to being readable by all and has the same name as the name of the secret. The first command below illustrates how to find the container ID, and the second and third commands use shell completion to do this automatically.

   使用 `docker ps` 获取 `redis` 服务任务容器的 ID，以便使用 `docker container exec` 连接到容器并读取 secret 数据文件的内容，该文件默认对所有人可读，且名称与 secret 名称相同。以下命令演示如何查找容器 ID，第二和第三个命令通过 shell 完成此操作。

   ```console
   $ docker ps --filter name=redis -q
   
   5cb1c2348a59
   
   $ docker container exec $(docker ps --filter name=redis -q) ls -l /run/secrets
   
   total 4
   -r--r--r--    1 root     root            17 Dec 13 22:48 my_secret_data
   
   $ docker container exec $(docker ps --filter name=redis -q) cat /run/secrets/my_secret_data
   
   This is a secret
   ```

5. Verify that the secret is not available if you commit the container.

   验证 secret 在提交容器后不可用。

   ```console
   $ docker commit $(docker ps --filter name=redis -q) committed_redis
   
   $ docker run --rm -it committed_redis cat /run/secrets/my_secret_data
   
   cat: can't open '/run/secrets/my_secret_data': No such file or directory
   ```

6. Try removing the secret. The removal fails because the `redis` service is running and has access to the secret.

   尝试删除 secret。删除失败，因为 `redis` 服务正在运行并具有该 secret 的访问权限。

   ```console
   $ docker secret ls
   
   ID                          NAME                CREATED             UPDATED
   wwwrxza8sxy025bas86593fqs   my_secret_data      4 hours ago         4 hours ago
   
   
   $ docker secret rm my_secret_data
   
   Error response from daemon: rpc error: code = 3 desc = secret
   'my_secret_data' is in use by the following service: redis
   ```

7. Remove access to the secret from the running `redis` service by updating the service.

   通过更新服务，从运行的 `redis` 服务中删除该 secret 的访问权限。

   ```console
   $ docker service update --secret-rm my_secret_data redis
   ```

8. Repeat steps 3 and 4 again, verifying that the service no longer has access to the secret. The container ID is different, because the `service update` command redeploys the service.

   再次重复步骤 3 和 4，验证该服务不再具有对 secret 的访问权限。容器 ID 不同，因为 `service update` 命令重新部署了该服务。

   ```console
   $ docker container exec -it $(docker ps --filter name=redis -q) cat /run/secrets/my_secret_data
   
   cat: can't open '/run/secrets/my_secret_data': No such file or directory
   ```

9. Stop and remove the service, and remove the secret from Docker.

   停止并删除服务，然后从 Docker 中删除该 secret。

   ```console
   $ docker service rm redis
   
   $ docker secret rm my_secret_data
   ```

### 简单示例：在 Windows 服务中使用 secrets - Simple example: Use secrets in a Windows service

This is a very simple example which shows how to use secrets with a Microsoft IIS service running on Docker for Windows running Windows containers on Microsoft Windows 10. It is a naive example that stores the webpage in a secret.

​	这是一个展示如何在 Windows 容器中运行的 Microsoft IIS 服务中使用 secrets 的简单示例。此示例将网页内容存储在一个 secret 中。

This example assumes that you have PowerShell installed.

​	此示例假设您已安装 PowerShell。

1. Save the following into a new file `index.html`.

   将以下内容保存为一个新文件 `index.html`。

   ```html
   <html lang="en">
     <head><title>Hello Docker</title></head>
     <body>
       <p>Hello Docker! You have deployed a HTML page.</p>
     </body>
   </html>
   ```

2. If you have not already done so, initialize or join the swarm.

   如果尚未这样做，初始化或加入 swarm。

   ```console
   > docker swarm init
   ```

3. Save the `index.html` file as a swarm secret named `homepage`.

   将 `index.html` 文件保存为名为 `homepage` 的 swarm secret。

   ```console
   > docker secret create homepage index.html
   ```

4. Create an IIS service and grant it access to the `homepage` secret.

   创建一个 IIS 服务并授予其访问 `homepage` secret 的权限。

   ```console
   > docker service create `
       --name my-iis `
       --publish published=8000,target=8000 `
       --secret src=homepage,target="\inetpub\wwwroot\index.html" `
       microsoft/iis:nanoserver
   ```

   > **Note**
   >
   > 
   >
   > There is technically no reason to use secrets for this example; [configs]({{< ref "/manuals/DockerEngine/Swarmmode/StoreconfigurationdatausingDockerConfigs" >}}) are a better fit. This example is for illustration only.
   >
   > ​	从技术上讲，此示例没有必要使用 secrets；[configs]({{< ref "/manuals/DockerEngine/Swarmmode/StoreconfigurationdatausingDockerConfigs" >}}) 更适合该用途。此示例仅用于说明。

5. Access the IIS service at `http://localhost:8000/`. It should serve the HTML content from the first step. 访问 `http://localhost:8000/` 上的 IIS 服务。它应显示第一步的 HTML 内容。

6. Remove the service and the secret.

   删除服务和 secret。

   ```console
   > docker service rm my-iis
   > docker secret rm homepage
   > docker image remove secret-test
   ```

### 中级示例：在 Nginx 服务中使用 Secrets - Intermediate example: Use secrets with a Nginx service

This example is divided into two parts. [The first part](https://docs.docker.com/engine/swarm/secrets/#generate-the-site-certificate) is all about generating the site certificate and does not directly involve Docker secrets at all, but it sets up [the second part](https://docs.docker.com/engine/swarm/secrets/#configure-the-nginx-container), where you store and use the site certificate and Nginx configuration as secrets.

​	此示例分为两部分。[第一部分](https://docs.docker.com/engine/swarm/secrets/#generate-the-site-certificate) 介绍生成站点证书，与 Docker secrets 并无直接关系，但它为[第二部分](https://docs.docker.com/engine/swarm/secrets/#configure-the-nginx-container)做准备，即如何将站点证书和 Nginx 配置存储并用作 secrets。

#### 生成站点证书Generate the site certificate

Generate a root CA and TLS certificate and key for your site. For production sites, you may want to use a service such as `Let’s Encrypt` to generate the TLS certificate and key, but this example uses command-line tools. This step is a little complicated, but is only a set-up step so that you have something to store as a Docker secret. If you want to skip these sub-steps, you can [use Let's Encrypt](https://letsencrypt.org/getting-started/) to generate the site key and certificate, name the files `site.key` and `site.crt`, and skip to [Configure the Nginx container](https://docs.docker.com/engine/swarm/secrets/#configure-the-nginx-container).

​	为站点生成一个根 CA 和 TLS 证书及密钥。对于生产环境，您可能需要使用 `Let’s Encrypt` 等服务生成 TLS 证书和密钥，但本示例使用命令行工具。此步骤稍微复杂一些，但只是为了生成可以存储为 Docker secret 的内容。如果您希望跳过这些子步骤，可以[使用 Let's Encrypt](https://letsencrypt.org/getting-started/)生成站点密钥和证书，将文件命名为 `site.key` 和 `site.crt`，然后直接进入[配置 Nginx 容器](https://docs.docker.com/engine/swarm/secrets/#configure-the-nginx-container)步骤。

​	

1. Generate a root key.

   生成根密钥。

   ```console
   $ openssl genrsa -out "root-ca.key" 4096
   ```

2. Generate a CSR using the root key.

   使用根密钥生成 CSR。

   ```console
   $ openssl req \
             -new -key "root-ca.key" \
             -out "root-ca.csr" -sha256 \
             -subj '/C=US/ST=CA/L=San Francisco/O=Docker/CN=Swarm Secret Example CA'
   ```

3. Configure the root CA. Edit a new file called `root-ca.cnf` and paste the following contents into it. This constrains the root CA to signing leaf certificates and not intermediate CAs.

   配置根 CA。编辑一个新文件 `root-ca.cnf`，并粘贴以下内容。此配置限制根 CA 仅签署叶证书而非中间 CA。

   ```ini
   [root_ca]
   basicConstraints = critical,CA:TRUE,pathlen:1
   keyUsage = critical, nonRepudiation, cRLSign, keyCertSign
   subjectKeyIdentifier=hash
   ```

4. Sign the certificate.

   签署证书。

   ```console
   $ openssl x509 -req  -days 3650  -in "root-ca.csr" \
                  -signkey "root-ca.key" -sha256 -out "root-ca.crt" \
                  -extfile "root-ca.cnf" -extensions \
                  root_ca
   ```

5. Generate the site key.

   生成站点密钥。

   ```console
   $ openssl genrsa -out "site.key" 4096
   ```

6. Generate the site certificate and sign it with the site key.

   使用站点密钥生成站点证书并签名。

   ```console
   $ openssl req -new -key "site.key" -out "site.csr" -sha256 \
             -subj '/C=US/ST=CA/L=San Francisco/O=Docker/CN=localhost'
   ```

7. Configure the site certificate. Edit a new file called `site.cnf` and paste the following contents into it. This constrains the site certificate so that it can only be used to authenticate a server and can't be used to sign certificates.

   配置站点证书。编辑一个新文件 `site.cnf` 并粘贴以下内容。此配置限制站点证书只能用于认证服务器，不能用于签署证书。

   ```ini
   [server]
   authorityKeyIdentifier=keyid,issuer
   basicConstraints = critical,CA:FALSE
   extendedKeyUsage=serverAuth
   keyUsage = critical, digitalSignature, keyEncipherment
   subjectAltName = DNS:localhost, IP:127.0.0.1
   subjectKeyIdentifier=hash
   ```

8. Sign the site certificate.

   签署站点证书。

   ```console
   $ openssl x509 -req -days 750 -in "site.csr" -sha256 \
       -CA "root-ca.crt" -CAkey "root-ca.key"  -CAcreateserial \
       -out "site.crt" -extfile "site.cnf" -extensions server
   ```

9. The `site.csr` and `site.cnf` files are not needed by the Nginx service, but you need them if you want to generate a new site certificate. Protect the `root-ca.key` file. `site.csr` 和 `site.cnf` 文件在 Nginx 服务中不需要，但若要生成新的站点证书，则需要它们。请保护好 `root-ca.key` 文件。

#### 配置 Nginx 容器 Configure the Nginx container

1. Produce a very basic Nginx configuration that serves static files over HTTPS. The TLS certificate and key are stored as Docker secrets so that they can be rotated easily. 创建一个非常基本的 Nginx 配置，通过 HTTPS 提供静态文件。TLS 证书和密钥作为 Docker secrets 存储，以便于轮换。

   In the current directory, create a new file called `site.conf` with the following contents:

   在当前目录中创建名为 `site.conf` 的新文件，内容如下：

   ```nginx
   server {
       listen                443 ssl;
       server_name           localhost;
       ssl_certificate       /run/secrets/site.crt;
       ssl_certificate_key   /run/secrets/site.key;
   
       location / {
           root   /usr/share/nginx/html;
           index  index.html index.htm;
       }
   }
   ```

2. Create three secrets, representing the key, the certificate, and the `site.conf`. You can store any file as a secret as long as it is smaller than 500 KB. This allows you to decouple the key, certificate, and configuration from the services that use them. In each of these commands, the last argument represents the path to the file to read the secret from on the host machine's filesystem. In these examples, the secret name and the file name are the same.

   创建三个 secrets，分别表示密钥、证书和 `site.conf`。任何小于 500 KB 的文件都可以存储为 secret，从而将密钥、证书和配置与使用它们的服务分离。在这些命令中，最后一个参数表示在主机文件系统上读取 secret 的文件路径。在这些示例中，secret 名称与文件名相同。

   ```console
   $ docker secret create site.key site.key
   
   $ docker secret create site.crt site.crt
   
   $ docker secret create site.conf site.conf
   ```

   

   ```console
   $ docker secret ls
   
   ID                          NAME                  CREATED             UPDATED
   2hvoi9mnnaof7olr3z5g3g7fp   site.key       58 seconds ago      58 seconds ago
   aya1dh363719pkiuoldpter4b   site.crt       24 seconds ago      24 seconds ago
   zoa5df26f7vpcoz42qf2csth8   site.conf      11 seconds ago      11 seconds ago
   ```

3. Create a service that runs Nginx and has access to the three secrets. The last part of the `docker service create` command creates a symbolic link from the location of the `site.conf` secret to `/etc/nginx.conf.d/`, where Nginx looks for extra configuration files. This step happens before Nginx actually starts, so you don't need to rebuild your image if you change the Nginx configuration. 创建一个运行 Nginx 的服务，并让它访问三个 secrets。`docker service create` 命令的最后部分在 `site.conf` secret 的位置和 `/etc/nginx.conf.d/` 之间创建一个符号链接，Nginx 会在此目录中查找额外的配置文件。

   > **Note**
   >
   > 
   >
   > Normally you would create a Dockerfile which copies the `site.conf` into place, build the image, and run a container using your custom image. This example does not require a custom image. It puts the `site.conf` into place and runs the container all in one step.
   >
   > ​	通常情况下，您会创建一个 Dockerfile，将 `site.conf` 复制到指定位置，构建镜像，然后使用您的自定义镜像运行容器。本示例不需要自定义镜像。它将 `site.conf` 放置到指定位置并一步完成容器的运行。

   Secrets are located within the `/run/secrets/` directory in the container by default, which may require extra steps in the container to make the secret available in a different path. The example below creates a symbolic link to the true location of the `site.conf` file so that Nginx can read it:

   ​	Secrets 默认位于容器内的 `/run/secrets/` 目录中，这可能需要在容器中执行额外步骤，以便在其他路径中使用这些 Secrets。以下示例创建了一个符号链接，将 `site.conf` 文件链接到 Nginx 可读取的实际位置：

   ```console
   $ docker service create \
        --name nginx \
        --secret site.key \
        --secret site.crt \
        --secret site.conf \
        --publish published=3000,target=443 \
        nginx:latest \
        sh -c "ln -s /run/secrets/site.conf /etc/nginx/conf.d/site.conf && exec nginx -g 'daemon off;'"
   ```

   Instead of creating symlinks, secrets allow you to specify a custom location using the `target` option. The example below illustrates how the `site.conf` secret is made available at `/etc/nginx/conf.d/site.conf` inside the container without the use of symbolic links:

   ​	Secrets 可以使用 `target` 选项指定自定义路径，避免创建符号链接。以下示例展示了 `site.conf` secret 如何在容器内无需符号链接直接位于 `/etc/nginx/conf.d/site.conf` 路径下：

   

   ```console
   $ docker service create \
        --name nginx \
        --secret site.key \
        --secret site.crt \
        --secret source=site.conf,target=/etc/nginx/conf.d/site.conf \
        --publish published=3000,target=443 \
        nginx:latest \
        sh -c "exec nginx -g 'daemon off;'"
   ```

   The `site.key` and `site.crt` secrets use the short-hand syntax, without a custom `target` location set. The short syntax mounts the secrets in `/run/secrets/ with the same name as the secret. Within the running containers, the following three files now exist:

   ​	`site.key` 和 `site.crt` secrets 使用了简写语法，未设置自定义 `target` 位置。简写语法将 secrets 挂载到 `/run/secrets/` 中，名称与 secrets 名称相同。运行中的容器中存在以下三个文件：

   - `/run/secrets/site.key`

   - `/run/secrets/site.crt`
   - `/etc/nginx/conf.d/site.conf`

4. Verify that the Nginx service is running.

   验证 Nginx 服务是否在运行。

   ```console
   $ docker service ls
   
   ID            NAME   MODE        REPLICAS  IMAGE
   zeskcec62q24  nginx  replicated  1/1       nginx:latest
   
   $ docker service ps nginx
   
   NAME                  IMAGE         NODE  DESIRED STATE  CURRENT STATE          ERROR  PORTS
   nginx.1.9ls3yo9ugcls  nginx:latest  moby  Running        Running 3 minutes ago
   ```

5. Verify that the service is operational: you can reach the Nginx server, and that the correct TLS certificate is being used.

   验证服务是否正常运行：您应该能够访问 Nginx 服务器，并且正在使用正确的 TLS 证书。

   ```console
   $ curl --cacert root-ca.crt https://localhost:3000
   
   <!DOCTYPE html>
   <html>
   <head>
   <title>Welcome to nginx!</title>
   <style>
       body {
           width: 35em;
           margin: 0 auto;
           font-family: Tahoma, Verdana, Arial, sans-serif;
       }
   </style>
   </head>
   <body>
   <h1>Welcome to nginx!</h1>
   <p>If you see this page, the nginx web server is successfully installed and
   working. Further configuration is required.</p>
   
   <p>For online documentation and support. refer to
   <a href="https://nginx.org">nginx.org</a>.<br/>
   Commercial support is available at
   <a href="https://www.nginx.com">nginx.com</a>.</p>
   
   <p><em>Thank you for using nginx.</em></p>
   </body>
   </html>
   ```

   

   ```console
   $ openssl s_client -connect localhost:3000 -CAfile root-ca.crt
   
   CONNECTED(00000003)
   depth=1 /C=US/ST=CA/L=San Francisco/O=Docker/CN=Swarm Secret Example CA
   verify return:1
   depth=0 /C=US/ST=CA/L=San Francisco/O=Docker/CN=localhost
   verify return:1
   ---
   Certificate chain
    0 s:/C=US/ST=CA/L=San Francisco/O=Docker/CN=localhost
      i:/C=US/ST=CA/L=San Francisco/O=Docker/CN=Swarm Secret Example CA
   ---
   Server certificate
   -----BEGIN CERTIFICATE-----
   …
   -----END CERTIFICATE-----
   subject=/C=US/ST=CA/L=San Francisco/O=Docker/CN=localhost
   issuer=/C=US/ST=CA/L=San Francisco/O=Docker/CN=Swarm Secret Example CA
   ---
   No client certificate CA names sent
   ---
   SSL handshake has read 1663 bytes and written 712 bytes
   ---
   New, TLSv1/SSLv3, Cipher is AES256-SHA
   Server public key is 4096 bit
   Secure Renegotiation IS supported
   Compression: NONE
   Expansion: NONE
   SSL-Session:
       Protocol  : TLSv1
       Cipher    : AES256-SHA
       Session-ID: A1A8BF35549C5715648A12FD7B7E3D861539316B03440187D9DA6C2E48822853
       Session-ID-ctx:
       Master-Key: F39D1B12274BA16D3A906F390A61438221E381952E9E1E05D3DD784F0135FB81353DA38C6D5C021CB926E844DFC49FC4
       Key-Arg   : None
       Start Time: 1481685096
       Timeout   : 300 (sec)
       Verify return code: 0 (ok)
   ```

6. To clean up after running this example, remove the `nginx` service and the stored secrets.

   运行此示例后进行清理，移除 `nginx` 服务及存储的 secrets。

   ```console
   $ docker service rm nginx
   
   $ docker secret rm site.crt site.key site.conf
   ```

### 高级示例：在 WordPress 服务中使用 Secrets - Advanced example: Use secrets with a WordPress service

In this example, you create a single-node MySQL service with a custom root password, add the credentials as secrets, and create a single-node WordPress service which uses these credentials to connect to MySQL. The [next example](https://docs.docker.com/engine/swarm/secrets/#example-rotate-a-secret) builds on this one and shows you how to rotate the MySQL password and update the services so that the WordPress service can still connect to MySQL.

​	在此示例中，您将创建一个具有自定义 root 密码的单节点 MySQL 服务，将凭据作为 secrets 添加，并创建一个使用这些凭据连接到 MySQL 的单节点 WordPress 服务。[下一个示例](https://docs.docker.com/engine/swarm/secrets/#example-rotate-a-secret)基于此示例，展示如何旋转 MySQL 密码并更新服务，以便 WordPress 服务仍可连接到 MySQL。

This example illustrates some techniques to use Docker secrets to avoid saving sensitive credentials within your image or passing them directly on the command line.

​	此示例展示了如何使用 Docker secrets 避免将敏感凭据保存在镜像中或直接通过命令行传递它们。

> **Note**
>
> 
>
> This example uses a single-Engine swarm for simplicity, and uses a single-node MySQL service because a single MySQL server instance cannot be scaled by simply using a replicated service, and setting up a MySQL cluster is beyond the scope of this example.
>
> ​	本示例为简单起见，使用了单引擎 swarm 和单节点 MySQL 服务，因为单个 MySQL 实例无法通过简单的服务复制进行扩展，且设置 MySQL 集群超出了此示例的范围。
>
> Also, changing a MySQL root passphrase isn’t as simple as changing a file on disk. You must use a query or a `mysqladmin` command to change the password in MySQL.
>
> ​	另外，更改 MySQL root 密码并不像更改磁盘上的文件那样简单，必须使用查询或 `mysqladmin` 命令更改 MySQL 密码。

1. Generate a random alphanumeric password for MySQL and store it as a Docker secret with the name `mysql_password` using the `docker secret create` command. To make the password shorter or longer, adjust the last argument of the `openssl` command. This is just one way to create a relatively random password. You can use another command to generate the password if you choose. 为 MySQL 生成一个随机字母数字密码，并使用 `docker secret create` 命令将其存储为 Docker secret，名称为 `mysql_password`。要缩短或延长密码长度，可以调整 `openssl` 命令的最后一个参数。这只是生成相对随机密码的一种方法，您可以选择其他命令生成密码。

   > **Note**
   >
   > 
   >
   > After you create a secret, you cannot update it. You can only remove and re-create it, and you cannot remove a secret that a service is using. However, you can grant or revoke a running service's access to secrets using `docker service update`. If you need the ability to update a secret, consider adding a version component to the secret name, so that you can later add a new version, update the service to use it, then remove the old version.
   >
   > ​	创建一个 secret 后，无法直接更新它。只能删除并重新创建，且无法删除一个服务正在使用的 secret。不过，可以使用 `docker service update` 来授予或撤销运行中的服务访问 secrets 的权限。如果需要更新一个 secret，可以在 secret 名称中添加版本号，这样以后可以添加新版本，更新服务以使用它，然后移除旧版本。

   The last argument is set to `-`, which indicates that the input is read from standard input.

   ​	最后一个参数设为 `-` 表示从标准输入读取输入。

   ```console
   $ openssl rand -base64 20 | docker secret create mysql_password -
   
   l1vinzevzhj4goakjap5ya409
   ```

   The value returned is not the password, but the ID of the secret. In the remainder of this tutorial, the ID output is omitted.

   ​	返回的值不是密码，而是 secret 的 ID。在本教程的剩余部分，将省略 ID 输出。

   Generate a second secret for the MySQL `root` user. This secret isn't shared with the WordPress service created later. It's only needed to bootstrap the `mysql` service.

   ​	生成一个用于 MySQL `root` 用户的第二个 secret。这个 secret 不会与稍后创建的 WordPress 服务共享，只用于初始化 `mysql` 服务。

   ```console
   $ openssl rand -base64 20 | docker secret create mysql_root_password -
   ```

   List the secrets managed by Docker using `docker secret ls`:

   ​	使用 `docker secret ls` 查看 Docker 管理的 secrets 列表：

   

   ```console
   $ docker secret ls
   
   ID                          NAME                  CREATED             UPDATED
   l1vinzevzhj4goakjap5ya409   mysql_password        41 seconds ago      41 seconds ago
   yvsczlx9votfw3l0nz5rlidig   mysql_root_password   12 seconds ago      12 seconds ago
   ```

   The secrets are stored in the encrypted Raft logs for the swarm.

   ​	这些 secrets 存储在 swarm 的加密 Raft 日志中。

2. Create a user-defined overlay network which is used for communication between the MySQL and WordPress services. There is no need to expose the MySQL service to any external host or container.

   创建一个用户定义的 overlay 网络，用于 MySQL 和 WordPress 服务之间的通信。无需将 MySQL 服务暴露给任何外部主机或容器。

   ```console
   $ docker network create -d overlay mysql_private
   ```

3. Create the MySQL service. The MySQL service has the following characteristics: 创建 MySQL 服务。该服务的特点如下：

   - Because the scale is set to `1`, only a single MySQL task runs. Load-balancing MySQL is left as an exercise to the reader and involves more than just scaling the service.

     - 设置 scale 为 `1`，仅运行一个 MySQL 任务。负载均衡 MySQL 留作读者练习，涉及的步骤比简单的服务扩展要多。

   - Only reachable by other containers on the `mysql_private` network.

     - 仅可通过 `mysql_private` 网络的其他容器访问。

   - Uses the volume `mydata` to store the MySQL data, so that it persists across restarts to the `mysql` service.

     - 使用卷 `mydata` 存储 MySQL 数据，以便服务重启时数据能够持久化。

   - The secrets are each mounted in a `tmpfs` filesystem at `/run/secrets/mysql_password` and `/run/secrets/mysql_root_password`. They are never exposed as environment variables, nor can they be committed to an image if the `docker commit` command is run. The `mysql_password` secret is the one used by the non-privileged WordPress container to connect to MySQL.

     - 每个 secret 挂载在 `/run/secrets/mysql_password` 和 `/run/secrets/mysql_root_password` 的 `tmpfs` 文件系统中。它们不会作为环境变量暴露，也不会在使用 `docker commit` 命令时保存在镜像中。`mysql_password` secret 用于非特权 WordPress 容器连接到 MySQL。

   - Sets the environment variables `MYSQL_PASSWORD_FILE` and `MYSQL_ROOT_PASSWORD_FILE` to point to the files `/run/secrets/mysql_password` and `/run/secrets/mysql_root_password`. The `mysql` image reads the password strings from those files when initializing the system database for the first time. Afterward, the passwords are stored in the MySQL system database itself.

     - 设置环境变量 `MYSQL_PASSWORD_FILE` 和 `MYSQL_ROOT_PASSWORD_FILE`，指向 `/run/secrets/mysql_password` 和 `/run/secrets/mysql_root_password` 文件。`mysql` 镜像在首次初始化系统数据库时从这些文件中读取密码字符串。之后，密码存储在 MySQL 系统数据库本身中。

   - Sets environment variables `MYSQL_USER` and `MYSQL_DATABASE`. A new database called `wordpress` is created when the container starts, and the `wordpress` user has full permissions for this database only. This user cannot create or drop databases or change the MySQL configuration.

     - 设置环境变量 `MYSQL_USER` 和 `MYSQL_DATABASE`。容器启动时创建一个名为 `wordpress` 的新数据库，且 `wordpress` 用户仅对此数据库拥有完全权限。此用户不能创建或删除数据库，也不能更改 MySQL 配置。

     

     ```console
     $ docker service create \
          --name mysql \
          --replicas 1 \
          --network mysql_private \
          --mount type=volume,source=mydata,destination=/var/lib/mysql \
          --secret source=mysql_root_password,target=mysql_root_password \
          --secret source=mysql_password,target=mysql_password \
          -e MYSQL_ROOT_PASSWORD_FILE="/run/secrets/mysql_root_password" \
          -e MYSQL_PASSWORD_FILE="/run/secrets/mysql_password" \
          -e MYSQL_USER="wordpress" \
          -e MYSQL_DATABASE="wordpress" \
          mysql:latest
     ```

4. Verify that the `mysql` container is running using the `docker service ls` command.

   使用 `docker service ls` 命令验证 `mysql` 容器是否在运行。

   ```console
   $ docker service ls
   
   ID            NAME   MODE        REPLICAS  IMAGE
   wvnh0siktqr3  mysql  replicated  1/1       mysql:latest
   ```

5. Now that MySQL is set up, create a WordPress service that connects to the MySQL service. The WordPress service has the following characteristics: 现在 MySQL 设置完成，创建一个连接到 MySQL 服务的 WordPress 服务。该服务的特点如下：

   - Because the scale is set to `1`, only a single WordPress task runs. Load-balancing WordPress is left as an exercise to the reader, because of limitations with storing WordPress session data on the container filesystem.
     - 设置 scale 为 `1`，仅运行一个 WordPress 任务。由于容器文件系统中存储 WordPress 会话数据的限制，WordPress 的负载均衡留作读者练习。

   - Exposes WordPress on port 30000 of the host machine, so that you can access it from external hosts. You can expose port 80 instead if you do not have a web server running on port 80 of the host machine.
     - 将 WordPress 暴露在主机的 30000 端口上，以便从外部主机访问。若主机的 80 端口没有运行其他 Web 服务器，可以改为暴露 80 端口。

   - Connects to the `mysql_private` network so it can communicate with the `mysql` container, and also publishes port 80 to port 30000 on all swarm nodes.
     - 连接到 `mysql_private` 网络，以便与 `mysql` 容器通信，且将 80 端口发布到所有 swarm 节点的 30000 端口。

   - Has access to the `mysql_password` secret, but specifies a different target file name within the container. The WordPress container uses the mount point `/run/secrets/wp_db_password`.
     - 可访问 `mysql_password` secret，但在容器中指定了不同的目标文件名。WordPress 容器使用 `/run/secrets/wp_db_password` 作为挂载点。

   - Sets the environment variable `WORDPRESS_DB_PASSWORD_FILE` to the file path where the secret is mounted. The WordPress service reads the MySQL password string from that file and add it to the `wp-config.php` configuration file.
     - 设置环境变量 `WORDPRESS_DB_PASSWORD_FILE` 指向 secret 的文件路径。WordPress 服务从该文件中读取 MySQL 密码字符串并将其添加到 `wp-config.php` 配置文件中。

   - Connects to the MySQL container using the username `wordpress` and the password in `/run/secrets/wp_db_password` and creates the `wordpress` database if it does not yet exist.
     - 使用 `wordpress` 用户和 `/run/secrets/wp_db_password` 中的密码连接到 MySQL 容器，并在数据库尚不存在时创建 `wordpress` 数据库。

   - Stores its data, such as themes and plugins, in a volume called `wpdata` so these files persist when the service restarts.
     - 使用名为 `wpdata` 的卷存储数据（如主题和插件），以便服务重启时这些文件能够持久化。


   

   ```console
   $ docker service create \
        --name wordpress \
        --replicas 1 \
        --network mysql_private \
        --publish published=30000,target=80 \
        --mount type=volume,source=wpdata,destination=/var/www/html \
        --secret source=mysql_password,target=wp_db_password \
        -e WORDPRESS_DB_USER="wordpress" \
        -e WORDPRESS_DB_PASSWORD_FILE="/run/secrets/wp_db_password" \
        -e WORDPRESS_DB_HOST="mysql:3306" \
        -e WORDPRESS_DB_NAME="wordpress" \
        wordpress:latest
   ```

6. Verify the service is running using `docker service ls` and `docker service ps` commands.

   使用 `docker service ls` 和 `docker service ps` 命令验证服务是否在运行。

   ```console
   $ docker service ls
   
   ID            NAME       MODE        REPLICAS  IMAGE
   wvnh0siktqr3  mysql      replicated  1/1       mysql:latest
   nzt5xzae4n62  wordpress  replicated  1/1       wordpress:latest
   ```

   

   ```console
   $ docker service ps wordpress
   
   ID            NAME         IMAGE             NODE  DESIRED STATE  CURRENT STATE           ERROR  PORTS
   aukx6hgs9gwc  wordpress.1  wordpress:latest  moby  Running        Running 52 seconds ago   
   ```

   At this point, you could actually revoke the WordPress service's access to the `mysql_password` secret, because WordPress has copied the secret to its configuration file `wp-config.php`. Don't do that for now, because we use it later to facilitate rotating the MySQL password.

   ​	此时，您可以撤销 WordPress 服务对 `mysql_password` secret 的访问，因为 WordPress 已将 secret 复制到其 `wp-config.php` 配置文件中。但暂时不要这样做，因为稍后需要用它来进行 MySQL 密码的旋转。

7. Access `http://localhost:30000/` from any swarm node and set up WordPress using the web-based wizard. All of these settings are stored in the MySQL `wordpress` database. WordPress automatically generates a password for your WordPress user, which is completely different from the password WordPress uses to access MySQL. Store this password securely, such as in a password manager. You need it to log into WordPress after [rotating the secret](https://docs.docker.com/engine/swarm/secrets/#example-rotate-a-secret). 从任一 swarm 节点访问 `http://localhost:30000/` 并使用 Web 向导设置 WordPress。这些设置均存储在 MySQL `wordpress` 数据库中。WordPress 会自动生成一个 WordPress 用户的密码，该密码与 WordPress 用于访问 MySQL 的密码完全不同。将此密码安全存储，例如在密码管理器中。您需要它来在 [旋转 secret](https://docs.docker.com/engine/swarm/secrets/#example-rotate-a-secret) 后登录到 WordPress。

   Go ahead and write a blog post or two and install a WordPress plugin or theme to verify that WordPress is fully operational and its state is saved across service restarts.

   ​	请继续撰写博客文章或安装 WordPress 插件或主题，以验证 WordPress 是否完全正常工作，且其状态在服务重启后能够持久保存。

8. Do not clean up any services or secrets if you intend to proceed to the next example, which demonstrates how to rotate the MySQL root password.

   若您打算进行下一个示例，不要清理任何服务或 secrets，该示例演示如何旋转 MySQL root 密码。


### 示例：轮换一个 Secret - Example: Rotate a secret

This example builds upon the previous one. In this scenario, you create a new secret with a new MySQL password, update the `mysql` and `wordpress` services to use it, then remove the old secret.

​	本示例基于前一个场景。在该场景中，你创建了一个新的 MySQL 密码 secret，并更新 `mysql` 和 `wordpress` 服务以使用新密码，随后移除旧的 secret。

> **Note**
>
> 
>
> Changing the password on a MySQL database involves running extra queries or commands, as opposed to just changing a single environment variable or a file, since the image only sets the MySQL password if the database doesn’t already exist, and MySQL stores the password within a MySQL database by default. Rotating passwords or other secrets may involve additional steps outside of Docker.
>
> ​	更改 MySQL 数据库的密码需要执行额外的查询或命令，而不仅仅是更改单个环境变量或文件，因为镜像只在数据库不存在时设置 MySQL 密码，MySQL 默认将密码存储在其数据库中。轮换密码或其他 secret 可能涉及 Docker 之外的额外步骤。

1. Create the new password and store it as a secret named `mysql_password_v2`.

   创建新密码并将其存储为名为 `mysql_password_v2` 的 secret。

   ```console
   $ openssl rand -base64 20 | docker secret create mysql_password_v2 -
   ```

2. Update the MySQL service to give it access to both the old and new secrets. Remember that you cannot update or rename a secret, but you can revoke a secret and grant access to it using a new target filename.

   更新 MySQL 服务，使其可以访问新旧两个 secret。请记住，不能直接更新或重命名一个 secret，但可以撤销 secret 并使用新的目标文件名重新授予访问权限。

   ```console
   $ docker service update \
        --secret-rm mysql_password mysql
   
   $ docker service update \
        --secret-add source=mysql_password,target=old_mysql_password \
        --secret-add source=mysql_password_v2,target=mysql_password \
        mysql
   ```

   Updating a service causes it to restart, and when the MySQL service restarts the second time, it has access to the old secret under `/run/secrets/old_mysql_password` and the new secret under `/run/secrets/mysql_password`.

   ​	更新服务会导致其重启，而 MySQL 服务在第二次重启时，能够访问位于 `/run/secrets/old_mysql_password` 的旧 secret 和 `/run/secrets/mysql_password` 的新 secret。

   Even though the MySQL service has access to both the old and new secrets now, the MySQL password for the WordPress user has not yet been changed.

   ​	尽管 MySQL 服务现在可以访问新旧两个 secret，但 WordPress 用户的 MySQL 密码尚未更改。

   > **Note**
   >
   > 
   >
   > This example does not rotate the MySQL `root` password.
   >
   > ​	本示例不会轮换 MySQL `root` 密码。

3. Now, change the MySQL password for the `wordpress` user using the `mysqladmin` CLI. This command reads the old and new password from the files in `/run/secrets` but does not expose them on the command line or save them in the shell history. CLI 更改 `wordpress` 用户的 MySQL 密码。此命令从 `/run/secrets` 文件中读取旧密码和新密码，但不会在命令行中暴露或保存在 shell 历史记录中。

   Do this quickly and move on to the next step, because WordPress loses the ability to connect to MySQL.

   ​	快速执行此步骤并继续下一步，因为 WordPress 将失去与 MySQL 的连接能力。

   First, find the ID of the `mysql` container task.

   ​	首先，找到 `mysql` 容器任务的 ID。

   ```console
   $ docker ps --filter name=mysql -q
   
   c7705cf6176f
   ```

   Substitute the ID in the command below, or use the second variant which uses shell expansion to do it all in a single step.

   ​	将 ID 替换到以下命令，或使用第二种变体，通过 shell 扩展一步完成。

   

   ```console
   $ docker container exec CONTAINER_ID \
       bash -c 'mysqladmin --user=wordpress --password="$(< /run/secrets/old_mysql_password)" password "$(< /run/secrets/mysql_password)"'
   ```

   Or:

   或者：

   ```console
   $ docker container exec $(docker ps --filter name=mysql -q) \
       bash -c 'mysqladmin --user=wordpress --password="$(< /run/secrets/old_mysql_password)" password "$(< /run/secrets/mysql_password)"'
   ```

4. Update the `wordpress` service to use the new password, keeping the target path at `/run/secrets/wp_db_password`. This triggers a rolling restart of the WordPress service and the new secret is used.

   更新 `wordpress` 服务以使用新密码，并将目标路径保持为 `/run/secrets/wp_db_password`。此操作将触发 WordPress 服务的滚动重启，新的 secret 将被使用。

   ```console
   $ docker service update \
        --secret-rm mysql_password \
        --secret-add source=mysql_password_v2,target=wp_db_password \
        wordpress    
   ```

5. Verify that WordPress works by browsing to http://localhost:30000/ on any swarm node again. Use the WordPress username and password from when you ran through the WordPress wizard in the previous task. 再次通过任何 swarm 节点访问 http://localhost:30000/ ，验证 WordPress 是否正常工作。使用之前在 WordPress 向导中设置的用户名和密码。

   Verify that the blog post you wrote still exists, and if you changed any configuration values, verify that they are still changed.

   ​	验证你编写的博客文章仍然存在，如果更改了任何配置值，也要验证它们是否仍然有效。

6. Revoke access to the old secret from the MySQL service and remove the old secret from Docker.

   撤销 MySQL 服务对旧 secret 的访问，并从 Docker 中删除旧的 secret。

   ```console
   $ docker service update \
        --secret-rm mysql_password \
        mysql
   
   $ docker secret rm mysql_password
   ```

7. Run the following commands to remove the WordPress service, the MySQL container, the `mydata` and `wpdata` volumes, and the Docker secrets:

   执行以下命令来删除 WordPress 服务、MySQL 容器、`mydata` 和 `wpdata` 卷，以及 Docker secrets：

   ```console
   $ docker service rm wordpress mysql
   
   $ docker volume rm mydata wpdata
   
   $ docker secret rm mysql_password_v2 mysql_root_password
   ```

## 在镜像中支持 Docker Secrets - Build support for Docker Secrets into your images

If you develop a container that can be deployed as a service and requires sensitive data, such as a credential, as an environment variable, consider adapting your image to take advantage of Docker secrets. One way to do this is to ensure that each parameter you pass to the image when creating the container can also be read from a file.

​	如果你开发的容器作为服务部署并需要敏感数据（如凭据）作为环境变量，建议调整你的镜像以支持 Docker secrets。实现的一种方式是确保在创建容器时传递给镜像的每个参数也可以从文件中读取。

Many of the Docker Official Images in the [Docker library](https://github.com/docker-library/), such as the [wordpress](https://github.com/docker-library/wordpress/) image used in the above examples, have been updated in this way.

​	在 [Docker library](https://github.com/docker-library/) 中，许多 Docker 官方镜像（例如使用在上述示例中的 [wordpress](https://github.com/docker-library/wordpress/) 镜像）已做了此类更新。

When you start a WordPress container, you provide it with the parameters it needs by setting them as environment variables. The WordPress image has been updated so that the environment variables which contain important data for WordPress, such as `WORDPRESS_DB_PASSWORD`, also have variants which can read their values from a file (`WORDPRESS_DB_PASSWORD_FILE`). This strategy ensures that backward compatibility is preserved, while allowing your container to read the information from a Docker-managed secret instead of being passed directly.

​	启动 WordPress 容器时，通过设置环境变量提供所需的参数。WordPress 镜像更新后，其包含重要数据的环境变量（如 `WORDPRESS_DB_PASSWORD`）也有可以从文件读取值的变体（`WORDPRESS_DB_PASSWORD_FILE`）。此策略确保了向后兼容性，同时允许容器从 Docker 管理的 secret 中读取信息，而不是直接传递。

> **Note**
>
> 
>
> Docker secrets do not set environment variables directly. This was a conscious decision, because environment variables can unintentionally be leaked between containers (for instance, if you use `--link`).
>
> ​	Docker secrets 不会直接设置环境变量。这是一个有意识的决定，因为环境变量可能会在容器之间意外泄露（例如，如果使用 `--link`）。

## 在 Compose 中使用 Secrets - Use Secrets in Compose



```yaml
services:
   db:
     image: mysql:latest
     volumes:
       - db_data:/var/lib/mysql
     environment:
       MYSQL_ROOT_PASSWORD_FILE: /run/secrets/db_root_password
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD_FILE: /run/secrets/db_password
     secrets:
       - db_root_password
       - db_password

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "8000:80"
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD_FILE: /run/secrets/db_password
     secrets:
       - db_password


secrets:
   db_password:
     file: db_password.txt
   db_root_password:
     file: db_root_password.txt

volumes:
    db_data:
```

This example creates a simple WordPress site using two secrets in a Compose file.

​	此示例使用 Compose 文件中的两个 secrets 创建了一个简单的 WordPress 站点。

The top-level element `secrets` defines two secrets `db_password` and `db_root_password`.

​	顶级元素 `secrets` 定义了两个 secrets：`db_password` 和 `db_root_password`。

When deploying, Docker creates these two secrets and populates them with the content from the file specified in the Compose file.

​	在部署时，Docker 创建这两个 secrets 并将它们的内容填充为 Compose 文件中指定的文件内容。

The `db` service uses both secrets, and `wordpress` is using one.

​	`db` 服务使用了两个 secrets，而 `wordpress` 使用了其中一个。

When you deploy, Docker mounts a file under `/run/secrets/<secret_name>` in the services. These files are never persisted on disk, but are managed in memory.

​	部署时，Docker 将文件挂载到服务中的 `/run/secrets/<secret_name>` 下。这些文件从不保存在磁盘上，而是以内存方式管理。

Each service uses environment variables to specify where the service should look for that secret data.

​	每个服务使用环境变量指定服务应从何处获取该 secret 数据。

More information on short and long syntax for secrets can be found in the [Compose Specification]({{< ref "/reference/Composefilereference/Secretstop-levelelements" >}}).

​	更多关于 secrets 的短语法和长语法的信息，可以在 [Compose 规范]({{< ref "/reference/Composefilereference/Secretstop-levelelements" >}}) 中找到。
