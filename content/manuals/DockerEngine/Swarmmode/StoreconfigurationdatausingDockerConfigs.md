+++
title = "使用 Docker Configs 存储配置信息"
date = 2024-10-23T14:54:40+08:00
weight = 120
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/configs/](https://docs.docker.com/engine/swarm/configs/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Store configuration data using Docker Configs - 使用 Docker Configs 存储配置信息

## 关于配置 About configs

Docker swarm service configs allow you to store non-sensitive information, such as configuration files, outside a service's image or running containers. This allows you to keep your images as generic as possible, without the need to bind-mount configuration files into the containers or use environment variables.

​	Docker 群集服务的配置允许您将非敏感信息（如配置文件）存储在服务的镜像或运行的容器之外。这让您可以使镜像尽可能通用，无需将配置文件绑定挂载到容器中或使用环境变量。

Configs operate in a similar way to [secrets]({{< ref "/manuals/DockerEngine/Swarmmode/ManagesensitivedatawithDockersecrets" >}}), except that they are not encrypted at rest and are mounted directly into the container's filesystem without the use of RAM disks. Configs can be added or removed from a service at any time, and services can share a config. You can even use configs in conjunction with environment variables or labels, for maximum flexibility. Config values can be generic strings or binary content (up to 500 kb in size).

​	配置的操作方式类似于 [secrets]({{< ref "/manuals/DockerEngine/Swarmmode/ManagesensitivedatawithDockersecrets" >}})，但配置不会在存储时加密，并且会直接挂载到容器文件系统中而无需使用 RAM 磁盘。配置可以在任何时间添加或移除，并且多个服务可以共享同一个配置。配置还可与环境变量或标签一起使用，以实现最大灵活性。配置值可以是普通字符串或二进制内容（最大 500 KB）。

> **Note**
>
> 
>
> Docker configs are only available to swarm services, not to standalone containers. To use this feature, consider adapting your container to run as a service with a scale of 1.
>
> ​	Docker 配置仅适用于群集服务，而不适用于独立的容器。若要使用此功能，请考虑将容器改为以单一实例的服务方式运行。

Configs are supported on both Linux and Windows services.

​	配置支持 Linux 和 Windows 服务。

### Windows 支持 Windows support

Docker includes support for configs on Windows containers, but there are differences in the implementations, which are called out in the examples below. Keep the following notable differences in mind:

​	Docker 在 Windows 容器上支持配置，但实现有所不同，下面的例子中会指出这些差异。请记住以下几个重要区别：

- Config files with custom targets are not directly bind-mounted into Windows containers, since Windows does not support non-directory file bind-mounts. Instead, configs for a container are all mounted in `C:\ProgramData\Docker\internal\configs` (an implementation detail which should not be relied upon by applications) within the container. Symbolic links are used to point from there to the desired target of the config within the container. The default target is `C:\ProgramData\Docker\configs`.
  - 自定义目标的配置文件不会直接绑定挂载到 Windows 容器中，因为 Windows 不支持非目录文件的绑定挂载。相反，配置在容器内全部挂载到 `C:\ProgramData\Docker\internal\configs`（这是实现细节，不建议应用依赖于此），并使用符号链接指向配置文件的目标位置，默认目标是 `C:\ProgramData\Docker\configs`。

- When creating a service which uses Windows containers, the options to specify UID, GID, and mode are not supported for configs. Configs are currently only accessible by administrators and users with `system` access within the container.
  - 创建使用 Windows 容器的服务时，配置的 UID、GID 和模式选项不受支持。目前在容器中配置只能由管理员和具有系统访问权限的用户访问。

- On Windows, create or update a service using `--credential-spec` with the `config://<config-name>` format. This passes the gMSA credentials file directly to nodes before a container starts. No gMSA credentials are written to disk on worker nodes. For more information, refer to [Deploy services to a swarm](https://docs.docker.com/engine/swarm/services/#gmsa-for-swarm).
  - 在 Windows 上使用 `--credential-spec` 创建或更新服务时，可以使用 `config://<config-name>` 格式传递 gMSA 凭据文件。这样，gMSA 凭据不会写入工作节点磁盘，而是在容器启动前直接传递给节点。详细信息请参阅 [在群集中部署服务](https://docs.docker.com/engine/swarm/services/#gmsa-for-swarm)。


## Docker 管理配置的方式 How Docker manages configs

When you add a config to the swarm, Docker sends the config to the swarm manager over a mutual TLS connection. The config is stored in the Raft log, which is encrypted. The entire Raft log is replicated across the other managers, ensuring the same high availability guarantees for configs as for the rest of the swarm management data.

​	将配置添加到群集时，Docker 会通过 TLS 加密连接将配置传递到群集管理器。该配置存储在加密的 Raft 日志中，并在其他管理器之间复制，确保配置具有与其他群集管理数据相同的高可用性。

When you grant a newly-created or running service access to a config, the config is mounted as a file in the container. The location of the mount point within the container defaults to `/<config-name>` in Linux containers. In Windows containers, configs are all mounted into `C:\ProgramData\Docker\configs` and symbolic links are created to the desired location, which defaults to `C:\<config-name>`.

​	当您为新建或正在运行的服务授予访问配置的权限时，配置会作为文件挂载到容器中。Linux 容器中的挂载位置默认为 `/<config-name>`。在 Windows 容器中，配置挂载到 `C:\ProgramData\Docker\configs`，并创建符号链接到目标位置，默认为 `C:\<config-name>`。

You can set the ownership (`uid` and `gid`) for the config, using either the numerical ID or the name of the user or group. You can also specify the file permissions (`mode`). These settings are ignored for Windows containers.

​	您可以设置配置的所有者（`uid` 和 `gid`），使用用户或组的数字 ID 或名称。您也可以指定文件权限（`mode`）。这些设置对 Windows 容器无效。

- If not set, the config is owned by the user running the container command (often `root`) and that user's default group (also often `root`).
  - 如果未设置，配置的所有者为运行容器命令的用户（通常为 `root`）及其默认组（通常也是 `root`）。

- If not set, the config has world-readable permissions (mode `0444`), unless a `umask` is set within the container, in which case the mode is impacted by that `umask` value.
  - 如果未设置，配置具有世界可读权限（模式 `0444`），但如果容器中设置了 `umask`，则模式会受该 `umask` 值影响。


You can update a service to grant it access to additional configs or revoke its access to a given config at any time.

​	可以随时更新服务，授予其访问额外配置的权限或撤销对配置的访问权限。

A node only has access to configs if the node is a swarm manager or if it is running service tasks which have been granted access to the config. When a container task stops running, the configs shared to it are unmounted from the in-memory filesystem for that container and flushed from the node's memory.

​	节点只有在作为群集管理器或运行已授予配置访问权限的服务任务时，才可访问配置。当容器任务停止运行时，分配给它的配置会从该容器的内存文件系统中卸载并从节点内存中清除。

If a node loses connectivity to the swarm while it is running a task container with access to a config, the task container still has access to its configs, but cannot receive updates until the node reconnects to the swarm.

​	如果节点在运行带配置访问权限的任务容器时失去与群集的连接，任务容器仍能访问其配置，但在重新连接到群集之前无法接收更新。

You can add or inspect an individual config at any time, or list all configs. You cannot remove a config that a running service is using. See [Rotate a config](https://docs.docker.com/engine/swarm/configs/#example-rotate-a-config) for a way to remove a config without disrupting running services.

​	可以随时添加或检查个别配置或列出所有配置。无法删除正在运行的服务正在使用的配置。请参阅 [轮换配置](https://docs.docker.com/engine/swarm/configs/#example-rotate-a-config) 了解如何在不中断服务的情况下删除配置。

To update or roll back configs more easily, consider adding a version number or date to the config name. This is made easier by the ability to control the mount point of the config within a given container.

​	为更方便地更新或回滚配置，建议在配置名称中添加版本号或日期。通过控制配置在容器内的挂载点可以更轻松地实现这一点。

To update a stack, make changes to your Compose file, then re-run `docker stack deploy -c <new-compose-file> <stack-name>`. If you use a new config in that file, your services start using them. Keep in mind that configurations are immutable, so you can't change the file for an existing service. Instead, you create a new config to use a different file

​	更新 stack 时，请修改您的 Compose 文件，然后重新运行 `docker stack deploy -c <new-compose-file> <stack-name>`。如果在文件中使用了新配置，服务将开始使用新配置。请注意，配置是不可变的，因此无法为现有服务更改文件。相反，您可以创建一个新配置以使用不同的文件。

You can run `docker stack rm` to stop the app and take down the stack. This removes any config that was created by `docker stack deploy` with the same stack name. This removes *all* configs, including those not referenced by services and those remaining after a `docker service update --config-rm`.

​	可以运行 `docker stack rm` 来停止应用并移除 stack。这将删除 `docker stack deploy` 用同一个 stack 名称创建的所有配置，包括未被服务引用的配置和在 `docker service update --config-rm` 后仍保留的配置。

## 进一步了解 `docker config` 命令 Read more about `docker config` commands

Use these links to read about specific commands, or continue to the [example about using configs with a service](https://docs.docker.com/engine/swarm/configs/#advanced-example-use-configs-with-a-nginx-service).

​	使用以下链接了解特定命令，或继续阅读[使用配置与服务的示例](https://docs.docker.com/engine/swarm/configs/#advanced-example-use-configs-with-a-nginx-service)。

- [`docker config create`]({{< ref "/reference/CLIreference/docker/dockerconfig/dockerconfigcreate" >}})
- [`docker config inspect`]({{< ref "/reference/CLIreference/docker/dockerconfig/dockerconfiginspect" >}})
- [`docker config ls`]({{< ref "/reference/CLIreference/docker/dockerconfig/dockerconfigls" >}})
- [`docker config rm`]({{< ref "/reference/CLIreference/docker/dockerconfig/dockerconfigrm" >}})

## 示例 Examples

This section includes graduated examples which illustrate how to use Docker configs.

​	本节包含一些分级示例，展示如何使用 Docker 配置。

> **Note**
>
> 
>
> These examples use a single-engine swarm and unscaled services for simplicity. The examples use Linux containers, but Windows containers also support configs.
>
> ​	这些示例使用单一引擎群集和未缩放的服务以简化操作。示例使用 Linux 容器，但 Windows 容器也支持配置。

### 在 Compose 文件中定义和使用配置 Defining and using configs in compose files

The `docker stack` command supports defining configs in a Compose file. However, the `configs` key is not supported for `docker compose`. See [the Compose file reference]({{< ref "/reference/Composefilereference/Legacyversions" >}}) for details.

​	`docker stack` 命令支持在 Compose 文件中定义配置，但 `docker compose` 不支持 `configs` 键。详情请参阅 [Compose 文件参考]({{< ref "/reference/Composefilereference/Legacyversions" >}})。

### 简单示例：开始使用配置 Simple example: Get started with configs

This simple example shows how configs work in just a few commands. For a real-world example, continue to [Advanced example: Use configs with a Nginx service](https://docs.docker.com/engine/swarm/configs/#advanced-example-use-configs-with-a-nginx-service).

​	本示例展示了通过几个命令来使用配置。若需更实际的例子，请参阅 [高级示例：在 Nginx 服务中使用配置](https://docs.docker.com/engine/swarm/configs/#advanced-example-use-configs-with-a-nginx-service)。

1. Add a config to Docker. The `docker config create` command reads standard input because the last argument, which represents the file to read the config from, is set to `-`.

   添加配置到 Docker。`docker config create` 命令读取标准输入，因为最后一个参数（表示读取配置的文件）设置为 `-`。

   ```console
   $ echo "This is a config" | docker config create my-config -
   ```

2. Create a `redis` service and grant it access to the config. By default, the container can access the config at `/my-config`, but you can customize the file name on the container using the `target` option.

   创建一个 `redis` 服务并授予它访问配置的权限。默认情况下，容器可以在 `/my-config` 访问该配置，您也可以使用 `target` 选项自定义文件名。

   ```console
   $ docker service create --name redis --config my-config redis:alpine
   ```

3. Verify that the task is running without issues using `docker service ps`. If everything is working, the output looks similar to this:

   使用 `docker service ps` 验证任务是否正常运行。如果一切正常，输出类似如下：

   ```console
   $ docker service ps redis
   
   ID            NAME     IMAGE         NODE              DESIRED STATE  CURRENT STATE          ERROR  PORTS
   bkna6bpn8r1a  redis.1  redis:alpine  ip-172-31-46-109  Running        Running 8 seconds ago
   ```

4. Get the ID of the `redis` service task container using `docker ps`, so that you can use `docker container exec` to connect to the container and read the contents of the config data file, which defaults to being readable by all and has the same name as the name of the config. The first command below illustrates how to find the container ID, and the second and third commands use shell completion to do this automatically.

   使用 `docker ps` 获取 `redis` 服务任务容器的 ID，然后使用 `docker container exec` 连接到容器并读取配置数据文件的内容，该文件默认为所有人可读，名称与配置名称相同。以下命令展示了如何找到容器 ID，并使用 shell 自动完成执行读取操作。

   ```console
   $ docker ps --filter name=redis -q
   
   5cb1c2348a59
   
   $ docker container exec $(docker ps --filter name=redis -q) ls -l /my-config
   
   -r--r--r--    1 root     root            12 Jun  5 20:49 my-config
   
   $ docker container exec $(docker ps --filter name=redis -q) cat /my-config
   
   This is a config
   ```

5. Try removing the config. The removal fails because the `redis` service is running and has access to the config.

   尝试删除配置。由于 `redis` 服务正在运行并访问该配置，因此删除操作会失败。

   ```console
   $ docker config ls
   
   ID                          NAME                CREATED             UPDATED
   fzwcfuqjkvo5foqu7ts7ls578   hello               31 minutes ago      31 minutes ago
   
   
   $ docker config rm my-config
   
   Error response from daemon: rpc error: code = 3 desc = config 'my-config' is
   in use by the following service: redis
   ```

6. Remove access to the config from the running `redis` service by updating the service.

   通过更新服务移除 `redis` 服务对该配置的访问权限。

   ```console
   $ docker service update --config-rm my-config redis
   ```

7. Repeat steps 3 and 4 again, verifying that the service no longer has access to the config. The container ID is different, because the `service update` command redeploys the service.

   再次执行步骤 3 和 4，验证服务不再具有对配置的访问权限。容器 ID 会不同，因为 `service update` 命令重新部署了该服务。

   ```none
   $ docker container exec -it $(docker ps --filter name=redis -q) cat /my-config
   
   cat: can't open '/my-config': No such file or directory
   ```

8. Stop and remove the service, and remove the config from Docker.

   停止并移除该服务，然后从 Docker 中移除配置。

   ```console
   $ docker service rm redis
   
   $ docker config rm my-config
   ```

### 简单示例：在 Windows 服务中使用配置 Simple example: Use configs in a Windows service

This is a very simple example which shows how to use configs with a Microsoft IIS service running on Docker for Windows running Windows containers on Microsoft Windows 10. It is a naive example that stores the webpage in a config.

​	此示例展示了如何在 Windows 10 上运行 Windows 容器的 Microsoft IIS 服务中使用配置。这个简单的示例将网页内容存储在配置中。

This example assumes that you have PowerShell installed.

​	该示例假设您已安装 PowerShell。

1. Save the following into a new file `index.html`.

   将以下内容保存到新文件 `index.html` 中。

   ```html
   <html lang="en">
     <head><title>Hello Docker</title></head>
     <body>
       <p>Hello Docker! You have deployed a HTML page.</p>
     </body>
   </html>
   ```

2. If you have not already done so, initialize or join the swarm.

   如果尚未执行，请初始化或加入群集。

   ```powershell
   docker swarm init
   ```

3. Save the `index.html` file as a swarm config named `homepage`.

   将 `index.html` 文件保存为名为 `homepage` 的群集配置。

   ```powershell
   docker config create homepage index.html
   ```

4. Create an IIS service and grant it access to the `homepage` config.

   创建一个 IIS 服务并授予它对 `homepage` 配置的访问权限。

   ```powershell
   docker service create
       --name my-iis
       --publish published=8000,target=8000
       --config src=homepage,target="\inetpub\wwwroot\index.html"
       microsoft/iis:nanoserver
   ```

5. Access the IIS service at `http://localhost:8000/`. It should serve the HTML content from the first step. 通过 `http://localhost:8000/` 访问 IIS 服务，它应显示第一步中的 HTML 内容。

6. Remove the service and the config.

   删除服务和配置。

   ```powershell
   docker service rm my-iis
   
   docker config rm homepage
   ```

### 示例：使用模板化配置 Example: Use a templated config

To create a configuration in which the content will be generated using a template engine, use the `--template-driver` parameter and specify the engine name as its argument. The template will be rendered when container is created.

​	要创建内容基于模板引擎生成的配置，使用 `--template-driver` 参数并指定引擎名称。模板将在创建容器时渲染。

1. Save the following into a new file `index.html.tmpl`.

   将以下内容保存到新文件 `index.html.tmpl` 中。

   ```html
   <html lang="en">
     <head><title>Hello Docker</title></head>
     <body>
       <p>Hello {{ env "HELLO" }}! I'm service {{ .Service.Name }}.</p>
     </body>
   </html>
   ```

2. Save the `index.html.tmpl` file as a swarm config named `homepage`. Provide parameter `--template-driver` and specify `golang` as template engine.

   将 `index.html.tmpl` 文件保存为名为 `homepage` 的群集配置，提供 `--template-driver` 参数并指定 `golang` 作为模板引擎。

   ```console
   $ docker config create --template-driver golang homepage index.html.tmpl
   ```

3. Create a service that runs Nginx and has access to the environment variable HELLO and to the config.

   创建运行 Nginx 且具有访问环境变量 `HELLO` 和配置权限的服务。

   ```console
   $ docker service create \
        --name hello-template \
        --env HELLO="Docker" \
        --config source=homepage,target=/usr/share/nginx/html/index.html \
        --publish published=3000,target=80 \
        nginx:alpine
   ```

4. Verify that the service is operational: you can reach the Nginx server, and that the correct output is being served.

   验证服务是否正常运行：访问 Nginx 服务器，确认输出正确。

   ```console
   $ curl http://0.0.0.0:3000
   
   <html lang="en">
     <head><title>Hello Docker</title></head>
     <body>
       <p>Hello Docker! I'm service hello-template.</p>
     </body>
   </html>
   ```

### 高级示例：在 Nginx 服务中使用配置 Advanced example: Use configs with a Nginx service

This example is divided into two parts. [The first part](https://docs.docker.com/engine/swarm/configs/#generate-the-site-certificate) is all about generating the site certificate and does not directly involve Docker configs at all, but it sets up [the second part](https://docs.docker.com/engine/swarm/configs/#configure-the-nginx-container), where you store and use the site certificate as a series of secrets and the Nginx configuration as a config. The example shows how to set options on the config, such as the target location within the container and the file permissions (`mode`).

​	本示例分为两部分。[第一部分](https://docs.docker.com/engine/swarm/configs/#generate-the-site-certificate) 生成站点证书，并未直接涉及 Docker 配置，但为 [第二部分](https://docs.docker.com/engine/swarm/configs/#configure-the-nginx-container) 使用站点证书及配置 Nginx 做准备。该示例展示了如何为配置设置选项，例如在容器内的目标位置和文件权限（`mode`）。

#### 生成站点证书 Generate the site certificate

Generate a root CA and TLS certificate and key for your site. For production sites, you may want to use a service such as `Let’s Encrypt` to generate the TLS certificate and key, but this example uses command-line tools. This step is a little complicated, but is only a set-up step so that you have something to store as a Docker secret. If you want to skip these sub-steps, you can [use Let's Encrypt](https://letsencrypt.org/getting-started/) to generate the site key and certificate, name the files `site.key` and `site.crt`, and skip to [Configure the Nginx container](https://docs.docker.com/engine/swarm/configs/#configure-the-nginx-container).

​	生成一个根 CA 和用于站点的 TLS 证书和密钥。在生产站点中，可以使用 `Let’s Encrypt` 等服务生成 TLS 证书和密钥，但此示例使用命令行工具。以下步骤较复杂，但仅用于设置以便存储为 Docker secret。如果想跳过这些子步骤，可以 [使用 Let’s Encrypt](https://letsencrypt.org/getting-started/) 生成站点密钥和证书，将文件命名为 `site.key` 和 `site.crt`，然后直接进入 [配置 Nginx 容器](https://docs.docker.com/engine/swarm/configs/#configure-the-nginx-container)。

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

3. Configure the root CA. Edit a new file called `root-ca.cnf` and paste the following contents into it. This constrains the root CA to only sign leaf certificates and not intermediate CAs.

   配置根 CA。编辑一个名为 `root-ca.cnf` 的新文件，并将以下内容粘贴到其中。这将根 CA 限制为仅签署叶证书，而不是中间 CA。

   ```none
   [root_ca]
   basicConstraints = critical,CA:TRUE,pathlen:1
   keyUsage = critical, nonRepudiation, cRLSign, keyCertSign
   subjectKeyIdentifier=hash
   ```

4. Sign the certificate.

   签署证书。

   ```console
   $ openssl x509 -req -days 3650 -in "root-ca.csr" \
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

   生成站点证书并使用站点密钥对其签名。

   ```console
   $ openssl req -new -key "site.key" -out "site.csr" -sha256 \
             -subj '/C=US/ST=CA/L=San Francisco/O=Docker/CN=localhost'
   ```

7. Configure the site certificate. Edit a new file called `site.cnf` and paste the following contents into it. This constrains the site certificate so that it can only be used to authenticate a server and can't be used to sign certificates.

   配置站点证书。编辑一个名为 `site.cnf` 的新文件，并将以下内容粘贴到其中。这将站点证书限制为只能用于服务器身份验证，不能用于签署其他证书。

   ```none
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
       -CA "root-ca.crt" -CAkey "root-ca.key" -CAcreateserial \
       -out "site.crt" -extfile "site.cnf" -extensions server
   ```

9. The `site.csr` and `site.cnf` files are not needed by the Nginx service, but you need them if you want to generate a new site certificate. Protect the `root-ca.key` file. Nginx 服务不需要 `site.csr` 和 `site.cnf` 文件，但如果要生成新的站点证书，则需要这些文件。保护好 `root-ca.key` 文件。

#### 配置 Nginx 容器 Configure the Nginx container

1. Produce a very basic Nginx configuration that serves static files over HTTPS. The TLS certificate and key are stored as Docker secrets so that they can be rotated easily. 创建一个非常基本的 Nginx 配置，用于通过 HTTPS 提供静态文件。TLS 证书和密钥存储为 Docker secrets，以便轻松进行轮换。

   In the current directory, create a new file called `site.conf` with the following contents:

   在当前目录中创建一个名为 `site.conf` 的新文件，并添加以下内容：

   ```none
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

2. Create two secrets, representing the key and the certificate. You can store any file as a secret as long as it is smaller than 500 KB. This allows you to decouple the key and certificate from the services that use them. In these examples, the secret name and the file name are the same.

   创建两个 secrets，分别表示密钥和证书。只要文件小于 500 KB，就可以将任何文件存储为 secret，从而可以将密钥和证书与使用它们的服务分离。在这些示例中，secret 名称和文件名称相同。

   ```console
   $ docker secret create site.key site.key
   
   $ docker secret create site.crt site.crt
   ```

3. Save the `site.conf` file in a Docker config. The first parameter is the name of the config, and the second parameter is the file to read it from.

   将 `site.conf` 文件保存为 Docker 配置。第一个参数是配置的名称，第二个参数是要从中读取的文件。

   ```console
   $ docker config create site.conf site.conf
   ```

   List the configs:

   列出配置：

   ```console
   $ docker config ls
   
   ID                          NAME                CREATED             UPDATED
   4ory233120ccg7biwvy11gl5z   site.conf           4 seconds ago       4 seconds ago
   ```

4. Create a service that runs Nginx and has access to the two secrets and the config. Set the mode to `0440` so that the file is only readable by its owner and that owner's group, not the world.

   创建一个运行 Nginx 且有访问这两个 secrets 和配置权限的服务。将模式设置为 `0440`，这样该文件仅对其所有者及其所在组可读，而不对其他人可读。

   ```console
   $ docker service create \
        --name nginx \
        --secret site.key \
        --secret site.crt \
        --config source=site.conf,target=/etc/nginx/conf.d/site.conf,mode=0440 \
        --publish published=3000,target=443 \
        nginx:latest \
        sh -c "exec nginx -g 'daemon off;'"
   ```

   Within the running containers, the following three files now exist:

   ​	在运行中的容器内，现在存在以下三个文件：

   - `/run/secrets/site.key`
   - `/run/secrets/site.crt`
   - `/etc/nginx/conf.d/site.conf`

5. Verify that the Nginx service is running.

   验证 Nginx 服务是否正在运行。

   ```console
   $ docker service ls
   
   ID            NAME   MODE        REPLICAS  IMAGE
   zeskcec62q24  nginx  replicated  1/1       nginx:latest
   
   $ docker service ps nginx
   
   NAME                  IMAGE         NODE  DESIRED STATE  CURRENT STATE          ERROR  PORTS
   nginx.1.9ls3yo9ugcls  nginx:latest  moby  Running        Running 3 minutes ago
   ```

6. Verify that the service is operational: you can reach the Nginx server, and that the correct TLS certificate is being used.

   验证服务是否正常运行：您可以访问 Nginx 服务器，并确认正在使用正确的 TLS 证书。

   ```console
   $ curl --cacert root-ca.crt https://0.0.0.0:3000
   
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
   
   <p>For online documentation and support, refer to
   <a href="https://nginx.org">nginx.org</a>.<br/>
   Commercial support is available at
   <a href="https://www.nginx.com">www.nginx.com</a>.</p>
   
   <p><em>Thank you for using nginx.</em></p>
   </body>
   </html>
   ```

   

   ```console
   $ openssl s_client -connect 0.0.0.0:3000 -CAfile root-ca.crt
   
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

7. Unless you are going to continue to the next example, clean up after running this example by removing the `nginx` service and the stored secrets and config.

   如果不继续进行下一个示例，请清理环境，移除 `nginx` 服务以及存储的 secrets 和配置。

   ```console
   $ docker service rm nginx
   
   $ docker secret rm site.crt site.key
   
   $ docker config rm site.conf
   ```

You have now configured a Nginx service with its configuration decoupled from its image. You could run multiple sites with exactly the same image but separate configurations, without the need to build a custom image at all.

​	您现在已配置了一个 Nginx 服务，其配置与镜像分离。可以使用相同的镜像运行多个站点但配置不同，而无需创建自定义镜像。

### 示例：轮换配置 Example: Rotate a config

To rotate a config, you first save a new config with a different name than the one that is currently in use. You then redeploy the service, removing the old config and adding the new config at the same mount point within the container. This example builds upon the previous one by rotating the `site.conf` configuration file.

​	要轮换配置，首先以与当前使用的不同的名称保存新配置。然后重新部署服务，移除旧配置并在容器内相同的挂载点添加新配置。本示例基于上一个示例，轮换 `site.conf` 配置文件。

1. Edit the `site.conf` file locally. Add `index.php` to the `index` line, and save the file.

   本地编辑 `site.conf` 文件。将 `index` 行添加 `index.php` 并保存文件。

   ```none
   server {
       listen                443 ssl;
       server_name           localhost;
       ssl_certificate       /run/secrets/site.crt;
       ssl_certificate_key   /run/secrets/site.key;
   
       location / {
           root   /usr/share/nginx/html;
           index  index.html index.htm index.php;
       }
   }
   ```

2. Create a new Docker config using the new `site.conf`, called `site-v2.conf`.

   使用新的 `site.conf` 创建一个新的 Docker 配置，命名为 `site-v2.conf`。

   ```bah
   $ docker config create site-v2.conf site.conf
   ```

3. Update the `nginx` service to use the new config instead of the old one.

   更新 `nginx` 服务以使用新配置替换旧配置。

   ```console
   $ docker service update \
     --config-rm site.conf \
     --config-add source=site-v2.conf,target=/etc/nginx/conf.d/site.conf,mode=0440 \
     nginx
   ```

4. Verify that the `nginx` service is fully re-deployed, using `docker service ps nginx`. When it is, you can remove the old `site.conf` config.

   使用 `docker service ps nginx` 确认 `nginx` 服务已完全重新部署。当完成后，您可以删除旧的 `site.conf` 配置。

   ```console
   $ docker config rm site.conf
   ```

5. To clean up, you can remove the `nginx` service, as well as the secrets and configs.

   为清理环境，可以移除 `nginx` 服务以及 secrets 和配置。

   ```console
   $ docker service rm nginx
   
   $ docker secret rm site.crt site.key
   
   $ docker config rm site-v2.conf
   ```

You have now updated your `nginx` service's configuration without the need to rebuild its image.

​	现在，您已更新 `nginx` 服务的配置而无需重新构建镜像。
