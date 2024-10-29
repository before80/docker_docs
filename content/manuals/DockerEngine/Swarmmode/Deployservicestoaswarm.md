+++
title = "将服务部署到 Swarm 集群"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/swarm/services/](https://docs.docker.com/engine/swarm/services/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Deploy services to a swarm - 将服务部署到 Swarm 集群

Swarm services use a declarative model, which means that you define the desired state of the service, and rely upon Docker to maintain this state. The state includes information such as (but not limited to):

​	Swarm 服务采用声明式模型，这意味着您定义服务的期望状态，然后依赖 Docker 来维持该状态。状态信息包括（但不限于）以下内容：

- The image name and tag the service containers should run
  - 服务容器应运行的镜像名称和标签

- How many containers participate in the service
  - 服务中容器的数量

- Whether any ports are exposed to clients outside the swarm
  - 是否将任何端口暴露给集群外部的客户端

- Whether the service should start automatically when Docker starts
  - 是否应在 Docker 启动时自动启动服务

- The specific behavior that happens when the service is restarted (such as whether a rolling restart is used)
  - 服务重启时的特定行为（如是否使用滚动重启）

- Characteristics of the nodes where the service can run (such as resource constraints and placement preferences)
  - 服务可以运行的节点特性（例如资源限制和位置偏好）


For an overview of Swarm mode, see [Swarm mode key concepts]({{< ref "/manuals/DockerEngine/Swarmmode/Swarmmodekeyconcepts" >}}). For an overview of how services work, see [How services work]({{< ref "/manuals/DockerEngine/Swarmmode/Howswarmworks/Howserviceswork" >}}).

​	关于 Swarm 模式概述，请参见 [Swarm 模式关键概念]({{< ref "/manuals/DockerEngine/Swarmmode/Swarmmodekeyconcepts" >}})。关于服务的概述，请参见 [服务如何工作]({{< ref "/manuals/DockerEngine/Swarmmode/Howswarmworks/Howserviceswork" >}})。

## 创建服务 Create a service

To create a single-replica service with no extra configuration, you only need to supply the image name. This command starts an Nginx service with a randomly-generated name and no published ports. This is a naive example, since you can't interact with the Nginx service.

​	要创建没有额外配置的单实例服务，只需提供镜像名称。以下命令启动一个随机生成名称且无发布端口的 Nginx 服务。这是一个简单示例，因为您无法与 Nginx 服务进行交互。



```console
$ docker service create nginx
```

The service is scheduled on an available node. To confirm that the service was created and started successfully, use the `docker service ls` command:

​	服务将被调度到可用的节点上。使用 `docker service ls` 命令确认服务是否成功创建并启动：



```console
$ docker service ls

ID                  NAME                MODE                REPLICAS            IMAGE                                                                                             PORTS
a3iixnklxuem        quizzical_lamarr    replicated          1/1                 docker.io/library/nginx@sha256:41ad9967ea448d7c2b203c699b429abe1ed5af331cd92533900c6d77490e0268
```

Created services do not always run right away. A service can be in a pending state if its image is unavailable, if no node meets the requirements you configure for the service, or for other reasons. See [Pending services](https://docs.docker.com/engine/swarm/how-swarm-mode-works/services/#pending-services) for more information.

​	服务创建后不一定会立即运行。如果镜像不可用、没有满足服务要求的节点，或其他原因，服务可能处于待定状态。有关更多信息，请参见 [待定服务](https://docs.docker.com/engine/swarm/how-swarm-mode-works/services/#pending-services)。

To provide a name for your service, use the `--name` flag:

​	要为服务指定名称，请使用 `--name` 标志：

```console
$ docker service create --name my_web nginx
```

Just like with standalone containers, you can specify a command that the service's containers should run, by adding it after the image name. This example starts a service called `helloworld` which uses an `alpine` image and runs the command `ping docker.com`:

​	如同独立容器一样，可以指定服务容器应运行的命令，将放在镜像名称后。例如，以下命令启动名为 `helloworld` 的服务，使用 `alpine` 镜像并运行 `ping docker.com` 命令：



```console
$ docker service create --name helloworld alpine ping docker.com
```

You can also specify an image tag for the service to use. This example modifies the previous one to use the `alpine:3.6` tag:

​	还可以为服务指定镜像标签。以下示例将上述命令修改为使用 `alpine:3.6` 标签：

```console
$ docker service create --name helloworld alpine:3.6 ping docker.com
```

For more details about image tag resolution, see [Specify the image version the service should use](https://docs.docker.com/engine/swarm/services/#specify-the-image-version-a-service-should-use).

​	关于镜像标签解析的更多详情，请参见 [指定服务使用的镜像版本](https://docs.docker.com/engine/swarm/services/#specify-the-image-version-a-service-should-use)。

### gMSA for Swarm

> **Note**
>
> 
>
> This example only works for a Windows container.
>
> ​	仅适用于 Windows 容器。

Swarm now allows using a Docker config as a gMSA credential spec - a requirement for Active Directory-authenticated applications. This reduces the burden of distributing credential specs to the nodes they're used on.

​	Swarm 现在允许使用 Docker 配置作为 gMSA 凭证规范，这对基于 Active Directory 认证的应用来说是一项必要条件。这样可以减轻凭证规范在节点上的分发负担。

The following example assumes a gMSA and its credential spec (called credspec.json) already exists, and that the nodes being deployed to are correctly configured for the gMSA.

​	以下示例假设已有一个 gMSA 和其凭证规范（称为 credspec.json），并且部署的节点已正确配置为支持该 gMSA。

To use a config as a credential spec, first create the Docker config containing the credential spec:

​	要将配置用作凭证规范，首先创建包含凭证规范的 Docker 配置：



```console
$ docker config create credspec credspec.json
```

Now, you should have a Docker config named credspec, and you can create a service using this credential spec. To do so, use the --credential-spec flag with the config name, like this:

​	现在，您应该已有一个名为 `credspec` 的 Docker 配置，可以使用此凭证规范创建服务。为此，请使用 `--credential-spec` 标志指定配置名称，如下所示：



```console
$ docker service create --credential-spec="config://credspec" <your image>
```

Your service uses the gMSA credential spec when it starts, but unlike a typical Docker config (used by passing the --config flag), the credential spec is not mounted into the container.

​	服务启动时会使用 gMSA 凭证规范，但不同于典型的 Docker 配置（通过 `--config` 标志传递），凭证规范不会挂载到容器中。

### 使用私有注册表中的镜像创建服务 Create a service using an image on a private registry

If your image is available on a private registry which requires login, use the `--with-registry-auth` flag with `docker service create`, after logging in. If your image is stored on `registry.example.com`, which is a private registry, use a command like the following:

​	如果您的镜像位于需要登录的私有注册表中，请在登录后使用 `docker service create` 时添加 `--with-registry-auth` 标志。如果镜像存储在私有注册表 `registry.example.com` 中，可以使用如下命令：



```console
$ docker login registry.example.com

$ docker service  create \
  --with-registry-auth \
  --name my_service \
  registry.example.com/acme/my_image:latest
```

This passes the login token from your local client to the swarm nodes where the service is deployed, using the encrypted WAL logs. With this information, the nodes are able to log into the registry and pull the image.

​	此命令会将登录令牌从本地客户端传递到服务部署所在的 Swarm 节点，节点使用加密的 WAL 日志保存登录信息，从而登录注册表并拉取镜像。

### 为托管服务账户提供凭证规范 Provide credential specs for managed service accounts

In Enterprise Edition 3.0, security is improved through the centralized distribution and management of Group Managed Service Account(gMSA) credentials using Docker config functionality. Swarm now allows using a Docker config as a gMSA credential spec, which reduces the burden of distributing credential specs to the nodes on which they are used.

​	在企业版 3.0 中，通过 Docker 配置功能集中分发和管理 Group Managed Service Account (gMSA) 凭证，以增强安全性。Swarm 现在允许将 Docker 配置用作 gMSA 凭证规范，减少了在节点上分发凭证的负担。

> **Note**
>
> 
>
> This option is only applicable to services using Windows containers.
>
> ​	仅适用于使用 Windows 容器的服务。

Credential spec files are applied at runtime, eliminating the need for host-based credential spec files or registry entries - no gMSA credentials are written to disk on worker nodes. You can make credential specs available to Docker Engine running swarm kit worker nodes before a container starts. When deploying a service using a gMSA-based config, the credential spec is passed directly to the runtime of containers in that service.

​	凭证规范文件在运行时应用，无需主机上的凭证规范文件或注册表条目，也不会在工作节点上写入任何 gMSA 凭证。在容器启动前，您可以将凭证规范提供给运行 swarm kit 的 Docker 引擎。部署使用基于 gMSA 配置的服务时，凭证规范会直接传递给该服务的容器运行时。

The `--credential-spec` must be in one of the following formats:

​	`--credential-spec` 必须符合以下格式之一：

- `file://<filename>`: The referenced file must be present in the `CredentialSpecs` subdirectory in the docker data directory, which defaults to `C:\ProgramData\Docker\` on Windows. For example, specifying `file://spec.json` loads `C:\ProgramData\Docker\CredentialSpecs\spec.json`.
  - `file://<filename>`：引用的文件必须位于 Docker 数据目录的 `CredentialSpecs` 子目录中，Windows 上默认为 `C:\ProgramData\Docker\`。例如，指定 `file://spec.json` 会加载 `C:\ProgramData\Docker\CredentialSpecs\spec.json`。

- `registry://<value-name>`: The credential spec is read from the Windows registry on the daemon’s host.
  - `registry://<value-name>`：凭证规范从守护进程主机的 Windows 注册表中读取。

- `config://<config-name>`: The config name is automatically converted to the config ID in the CLI. The credential spec contained in the specified `config` is used.
  - `config://<config-name>`：CLI 中的配置名称会自动转换为配置 ID，使用指定 `config` 中的凭证规范。


The following simple example retrieves the gMSA name and JSON contents from your Active Directory (AD) instance:

​	以下简单示例从您的 Active Directory (AD) 实例中获取 gMSA 名称和 JSON 内容：

```console
$ name="mygmsa"
$ contents="{...}"
$ echo $contents > contents.json
```

Make sure that the nodes to which you are deploying are correctly configured for the gMSA.

​	确保部署的节点已为 gMSA 正确配置。

To use a config as a credential spec, create a Docker config in a credential spec file named `credpspec.json`. You can specify any name for the name of the `config`.

​	要使用配置作为凭证规范，请在名为 `credpspec.json` 的凭证规范文件中创建 Docker 配置。可以任意指定 `config` 的名称。

```console
$ docker config create --label com.docker.gmsa.name=mygmsa credspec credspec.json
```

Now you can create a service using this credential spec. Specify the `--credential-spec` flag with the config name:

​	现在可以使用此凭证规范创建服务。指定 `--credential-spec` 标志并提供配置名称：

```console
$ docker service create --credential-spec="config://credspec" <your image>
```

Your service uses the gMSA credential spec when it starts, but unlike a typical Docker config (used by passing the --config flag), the credential spec is not mounted into the container.

​	当服务启动时，它会使用 gMSA 凭证规范，但不同于典型的 Docker 配置（通过 `--config` 标志传递），凭证规范不会挂载到容器中。

## 更新服务 Update a service

You can change almost everything about an existing service using the `docker service update` command. When you update a service, Docker stops its containers and restarts them with the new configuration.

​	可以使用 `docker service update` 命令更改现有服务的大部分配置。当更新服务时，Docker 会停止其容器，并使用新的配置重新启动它们。

Since Nginx is a web service, it works much better if you publish port 80 to clients outside the swarm. You can specify this when you create the service, using the `-p` or `--publish` flag. When updating an existing service, the flag is `--publish-add`. There is also a `--publish-rm` flag to remove a port that was previously published.

​	由于 Nginx 是 Web 服务，若将端口 80 发布给集群外的客户端，它的效果会更好。创建服务时可以使用 `-p` 或 `--publish` 标志指定，更新现有服务时则使用 `--publish-add` 标志。另有 `--publish-rm` 标志可以移除已发布的端口。

Assuming that the `my_web` service from the previous section still exists, use the following command to update it to publish port 80.

​	假设之前创建的 `my_web` 服务仍然存在，可以使用以下命令将其更新为发布端口 80。



```console
$ docker service update --publish-add 80 my_web
```

To verify that it worked, use `docker service ls`:

​	使用 `docker service ls` 验证更新是否成功：

```console
$ docker service ls

ID                  NAME                MODE                REPLICAS            IMAGE                                                                                             PORTS
4nhxl7oxw5vz        my_web              replicated          1/1                 docker.io/library/nginx@sha256:41ad9967ea448d7c2b203c699b429abe1ed5af331cd92533900c6d77490e0268   *:0->80/tcp
```

For more information on how publishing ports works, see [publish ports](https://docs.docker.com/engine/swarm/services/#publish-ports).

​	关于发布端口的更多信息，请参考 [发布端口](https://docs.docker.com/engine/swarm/services/#publish-ports)。

You can update almost every configuration detail about an existing service, including the image name and tag it runs. See [Update a service's image after creation](https://docs.docker.com/engine/swarm/services/#update-a-services-image-after-creation).

​	可以更新现有服务的几乎所有配置细节，包括其运行的镜像名称和标签。参考 [创建后更新服务的镜像](https://docs.docker.com/engine/swarm/services/#update-a-services-image-after-creation)。

## 删除服务 Remove a service

To remove a service, use the `docker service remove` command. You can remove a service by its ID or name, as shown in the output of the `docker service ls` command. The following command removes the `my_web` service.

​	使用 `docker service remove` 命令可以删除服务。可以根据 `docker service ls` 命令输出中的 ID 或名称删除服务。以下命令删除 `my_web` 服务。

```console
$ docker service remove my_web
```

## 服务配置详情 Service configuration details

The following sections provide details about service configuration. This topic does not cover every flag or scenario. In almost every instance where you can define a configuration at service creation, you can also update an existing service's configuration in a similar way.

​	以下部分提供服务配置的详细信息。几乎所有在创建服务时定义的配置都可以以类似方式更新现有服务的配置。

See the command-line references for [`docker service create`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}}) and [`docker service update`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceupdate" >}}), or run one of those commands with the `--help` flag.

​	查看 [`docker service create`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}}) 和 [`docker service update`]({{< ref "/reference/CLIreference/docker/dockerservice/dockerserviceupdate" >}}) 的命令行参考，或使用 `--help` 标志运行这些命令。

### 配置运行时环境 Configure the runtime environment

You can configure the following options for the runtime environment in the container:

​	可以为容器的运行时环境配置以下选项：

- Environment variables using the `--env` flag
  - 使用 `--env` 标志设置环境变量

- The working directory inside the container using the `--workdir` flag
  - 使用 `--workdir` 标志设置容器内部的工作目录

- The username or UID using the `--user` flag
  - 使用 `--user` 标志指定用户名或 UID


The following service's containers have an environment variable `$MYVAR` set to `myvalue`, run from the `/tmp/` directory, and run as the `my_user` user.

​	以下服务容器的环境变量 `$MYVAR` 设置为 `myvalue`，运行于 `/tmp/` 目录，并以 `my_user` 用户身份运行。



```console
$ docker service create --name helloworld \
  --env MYVAR=myvalue \
  --workdir /tmp \
  --user my_user \
  alpine ping docker.com
```

### 更新现有服务的运行命令 Update the command an existing service runs

To update the command an existing service runs, you can use the `--args` flag. The following example updates an existing service called `helloworld` so that it runs the command `ping docker.com` instead of whatever command it was running before:

​	可以使用 `--args` 标志更新现有服务的运行命令。以下示例更新名为 `helloworld` 的现有服务，使其运行 `ping docker.com` 命令，而不是之前的命令：



```console
$ docker service update --args "ping docker.com" helloworld
```

### 指定服务应使用的镜像版本 Specify the image version a service should use

When you create a service without specifying any details about the version of the image to use, the service uses the version tagged with the `latest` tag. You can force the service to use a specific version of the image in a few different ways, depending on your desired outcome.

​	当创建服务时未指定镜像的版本信息时，服务会使用标记为 `latest` 的版本。可以通过不同方式强制服务使用特定镜像版本，具体方式取决于所需结果。

An image version can be expressed in several different ways:

​	镜像版本可以通过几种方式表示：

- If you specify a tag, the manager (or the Docker client, if you use [content trust]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker" >}})) resolves that tag to a digest. When the request to create a container task is received on a worker node, the worker node only sees the digest, not the tag.

  如果指定了一个标签，管理器（或启用 [内容信任]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker" >}}) 的 Docker 客户端）会将标签解析为摘要。当工作节点收到创建容器任务的请求时，工作节点仅看到摘要而不是标签。

  ```console
  $ docker service create --name="myservice" ubuntu:16.04
  ```

  Some tags represent discrete releases, such as `ubuntu:16.04`. Tags like this almost always resolve to a stable digest over time. It is recommended that you use this kind of tag when possible.

  ​	某些标签代表离散发布，例如 `ubuntu:16.04`。这种标签通常在时间上始终解析为一个稳定的摘要，推荐在可能的情况下使用此类标签。

  Other types of tags, such as `latest` or `nightly`, may resolve to a new digest often, depending on how often an image's author updates the tag. It is not recommended to run services using a tag which is updated frequently, to prevent different service replica tasks from using different image versions.

  ​	其他类型的标签（例如 `latest` 或 `nightly`）可能会经常解析为新的摘要，取决于镜像作者更新标签的频率。不建议使用更新频繁的标签运行服务，以防止不同服务副本任务使用不同版本的镜像。

- If you don't specify a version at all, by convention the image's `latest` tag is resolved to a digest. Workers use the image at this digest when creating the service task. 如果不指定版本，默认情况下，镜像的 `latest` 标签会解析为摘要。创建服务任务时，工作节点会使用该摘要。

  Thus, the following two commands are equivalent:

  因此，以下两个命令是等价的：

  ```console
  $ docker service create --name="myservice" ubuntu
  
  $ docker service create --name="myservice" ubuntu:latest
  ```

- If you specify a digest directly, that exact version of the image is always used when creating service tasks.

  如果直接指定摘要，则始终使用该确切版本的镜像来创建服务任务。

  ```console
  $ docker service create \
      --name="myservice" \
      ubuntu:16.04@sha256:35bc48a1ca97c3971611dc4662d08d131869daa692acb281c7e9e052924e38b1
  ```

When you create a service, the image's tag is resolved to the specific digest the tag points to **at the time of service creation**. Worker nodes for that service use that specific digest forever unless the service is explicitly updated. This feature is particularly important if you do use often-changing tags such as `latest`, because it ensures that all service tasks use the same version of the image.

​	创建服务时，镜像的标签会解析为标签指向的特定摘要 **（服务创建时）**。该服务的工作节点将一直使用该特定摘要，除非显式更新服务。对于 `latest` 等经常更改的标签，这一特性尤为重要，因为它确保所有服务任务使用相同的镜像版本。

> **Note**
>
> 
>
> If [content trust]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker" >}}) is enabled, the client actually resolves the image's tag to a digest before contacting the swarm manager, to verify that the image is signed. Thus, if you use content trust, the swarm manager receives the request pre-resolved. In this case, if the client cannot resolve the image to a digest, the request fails.
>
> ​	如果启用了 [内容信任]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker" >}})，客户端在联系 swarm 管理器之前会将镜像标签解析为摘要，以验证镜像是否已签名。因此，如果使用内容信任，swarm 管理器会收到预先解析的请求。如果客户端无法将镜像解析为摘要，则请求会失败。

If the manager can't resolve the tag to a digest, each worker node is responsible for resolving the tag to a digest, and different nodes may use different versions of the image. If this happens, a warning like the following is logged, substituting the placeholders for real information.

​	如果管理器无法将标签解析为摘要，则每个工作节点负责将标签解析为摘要，不同节点可能会使用不同版本的镜像。如果发生这种情况，会记录类似以下的警告，并用实际信息替换占位符。



```none
unable to pin image <IMAGE-NAME> to digest: <REASON>
```

To see an image's current digest, issue the command `docker inspect <IMAGE>:<TAG>` and look for the `RepoDigests` line. The following is the current digest for `ubuntu:latest` at the time this content was written. The output is truncated for clarity.

​	可以使用 `docker inspect <IMAGE>:<TAG>` 命令查看镜像的当前摘要，并查找 `RepoDigests` 行。以下是 `ubuntu:latest` 的当前摘要（输出已简化以提高清晰度）。



```console
$ docker inspect ubuntu:latest
```



```json
"RepoDigests": [
    "ubuntu@sha256:35bc48a1ca97c3971611dc4662d08d131869daa692acb281c7e9e052924e38b1"
],
```

After you create a service, its image is never updated unless you explicitly run `docker service update` with the `--image` flag as described below. Other update operations such as scaling the service, adding or removing networks or volumes, renaming the service, or any other type of update operation do not update the service's image.

​	创建服务后，除非显式运行 `docker service update` 并使用 `--image` 标志，否则镜像永不更新。其他更新操作（例如扩展服务、添加或移除网络或卷、重命名服务等）均不会更新服务的镜像。

### 服务创建后更新镜像 Update a service's image after creation

Each tag represents a digest, similar to a Git hash. Some tags, such as `latest`, are updated often to point to a new digest. Others, such as `ubuntu:16.04`, represent a released software version and are not expected to update to point to a new digest often if at all. When you create a service, it is constrained to create tasks using a specific digest of an image until you update the service using `service update` with the `--image` flag.

​	每个标签都代表一个摘要，类似于 Git 哈希值。一些标签（如 `latest`）会经常更新以指向新摘要，而其他标签（如 `ubuntu:16.04`）则通常不会更改。创建服务时，任务会使用镜像的特定摘要，直到使用 `service update` 命令并带有 `--image` 标志更新服务。

When you run `service update` with the `--image` flag, the swarm manager queries Docker Hub or your private Docker registry for the digest the tag currently points to and updates the service tasks to use that digest.

​	当使用 `--image` 标志运行 `service update` 时，swarm 管理器会查询 Docker Hub 或您的私有 Docker 注册表，以获取该标签当前指向的摘要，并更新服务任务以使用该摘要。

> **Note**
>
> 
>
> If you use [content trust]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker" >}}), the Docker client resolves image and the swarm manager receives the image and digest, rather than a tag.
>
> ​	如果使用 [内容信任]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker" >}})，Docker 客户端会解析镜像，并将图像和摘要发送给 swarm 管理器，而不是标签。

Usually, the manager can resolve the tag to a new digest and the service updates, redeploying each task to use the new image. If the manager can't resolve the tag or some other problem occurs, the next two sections outline what to expect.

​	通常情况下，管理器可以将标签解析为新摘要，并重新部署每个任务以使用新镜像。如果管理器无法解析标签或发生其他问题，以下内容概述了可能的情况。

#### 如果管理器能够解析标签 If the manager resolves the tag

If the swarm manager can resolve the image tag to a digest, it instructs the worker nodes to redeploy the tasks and use the image at that digest.

​	如果 swarm 管理器能够将镜像标签解析为摘要，它会指示工作节点重新部署任务并使用该摘要下的镜像。

- If a worker has cached the image at that digest, it uses it.
  - 如果工作节点已经缓存了该摘要下的镜像，则直接使用。

- If not, it attempts to pull the image from Docker Hub or the private registry.  否则，尝试从 Docker Hub 或私有注册表中拉取镜像。
  - If it succeeds, the task is deployed using the new image. 如果成功，任务将使用新镜像部署。
  - If the worker fails to pull the image, the service fails to deploy on that worker node. Docker tries again to deploy the task, possibly on a different worker node. 如果拉取失败，则服务无法在该节点上部署，Docker 会尝试在其他工作节点上部署任务。

#### 如果管理器无法解析标签 If the manager cannot resolve the tag

If the swarm manager cannot resolve the image to a digest, all is not lost:

​	如果管理器无法将镜像解析为摘要，仍然可以继续部署：

- The manager instructs the worker nodes to redeploy the tasks using the image at that tag.
  - 管理器指示工作节点使用该标签下的镜像重新部署任务。

- If the worker has a locally cached image that resolves to that tag, it uses that image.
  - 如果工作节点已本地缓存了解析为该标签的镜像，则直接使用。

- If the worker does not have a locally cached image that resolves to the tag, the worker tries to connect to Docker Hub or the private registry to pull the image at that tag. 如果工作节点未缓存该镜像，则尝试连接 Docker Hub 或私有注册表以拉取该标签的镜像。
  - If this succeeds, the worker uses that image.
    - 如果成功，使用该镜像。
  - If this fails, the task fails to deploy and the manager tries again to deploy the task, possibly on a different worker node.
    - 如果失败，任务无法部署，管理器会尝试在其他节点上部署任务。

### 发布端口 Publish ports

When you create a swarm service, you can publish that service's ports to hosts outside the swarm in two ways:

​	创建 swarm 服务时，可以通过两种方式将服务的端口发布到 swarm 外部的主机：

- [You can rely on the routing mesh](https://docs.docker.com/engine/swarm/services/#publish-a-services-ports-using-the-routing-mesh). When you publish a service port, the swarm makes the service accessible at the target port on every node, regardless of whether there is a task for the service running on that node or not. This is less complex and is the right choice for many types of services.
  - [使用路由网格](https://docs.docker.com/engine/swarm/services/#publish-a-services-ports-using-the-routing-mesh)。当您发布服务端口时，swarm 在每个节点上都使该端口可访问，无论该节点是否运行服务任务。这种方式较简单，适用于多种服务类型。

- [You can publish a service task's port directly on the swarm node](https://docs.docker.com/engine/swarm/services/#publish-a-services-ports-directly-on-the-swarm-node) where that service is running. This bypasses the routing mesh and provides the maximum flexibility, including the ability for you to develop your own routing framework. However, you are responsible for keeping track of where each task is running and routing requests to the tasks, and load-balancing across the nodes.
  - [在 swarm 节点上直接发布服务任务的端口](https://docs.docker.com/engine/swarm/services/#publish-a-services-ports-directly-on-the-swarm-node)。这绕过了路由网格，提供了最大灵活性，包括构建自定义路由框架的能力。但您需要跟踪每个任务的位置，负责将请求路由到任务并进行负载均衡。


Keep reading for more information and use cases for each of these methods.

​	以下内容进一步介绍了这两种方法的使用情况。

#### 使用路由网格发布服务的端口 Publish a service's ports using the routing mesh

To publish a service's ports externally to the swarm, use the `--publish <PUBLISHED-PORT>:<SERVICE-PORT>` flag. The swarm makes the service accessible at the published port on every swarm node. If an external host connects to that port on any swarm node, the routing mesh routes it to a task. The external host does not need to know the IP addresses or internally-used ports of the service tasks to interact with the service. When a user or process connects to a service, any worker node running a service task may respond. For more details about swarm service networking, see [Manage swarm service networks]({{< ref "/manuals/DockerEngine/Swarmmode/Manageswarmservicenetworks" >}}).

​	要向 swarm 外部发布服务端口，可以使用 `--publish <PUBLISHED-PORT>:<SERVICE-PORT>` 标志。swarm 在每个节点上将该端口设为可访问的。如果外部主机连接到任意 swarm 节点的该端口，路由网格会将其路由到某个任务。外部主机无需了解任务的 IP 地址或内部端口。有关 swarm 服务网络的更多信息，请参阅 [管理 swarm 服务网络]({{< ref "/manuals/DockerEngine/Swarmmode/Manageswarmservicenetworks" >}})。

##### 示例：在10节点的 swarm 上运行3任务的 Nginx 服务 Example: Run a three-task Nginx service on 10-node swarm

Imagine that you have a 10-node swarm, and you deploy an Nginx service running three tasks on a 10-node swarm:

​	假设您有一个 10 节点的 swarm，并在其上部署一个运行三项任务的 Nginx 服务：

```console
$ docker service create --name my_web \
                        --replicas 3 \
                        --publish published=8080,target=80 \
                        nginx
```

Three tasks run on up to three nodes. You don't need to know which nodes are running the tasks; connecting to port 8080 on any of the 10 nodes connects you to one of the three `nginx` tasks. You can test this using `curl`. The following example assumes that `localhost` is one of the swarm nodes. If this is not the case, or `localhost` does not resolve to an IP address on your host, substitute the host's IP address or resolvable host name.

​	三项任务在多达三个节点上运行。您无需知道任务运行在哪些节点上；连接到任意节点的 8080 端口即可访问 nginx 任务。可以使用 `curl` 进行测试。例如，假设 `localhost` 是某个 swarm 节点。如果不是这种情况，可以替换为主机的 IP 地址或可解析的主机名。

The HTML output is truncated:

​	HTML 输出内容简化如下：

```console
$ curl localhost:8080

<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...truncated...
</html>
```

Subsequent connections may be routed to the same swarm node or a different one.

​	后续的连接可能会路由到同一节点或其他节点。

#### 在 swarm 节点上直接发布服务端口 Publish a service's ports directly on the swarm node

Using the routing mesh may not be the right choice for your application if you need to make routing decisions based on application state or you need total control of the process for routing requests to your service's tasks. To publish a service's port directly on the node where it is running, use the `mode=host` option to the `--publish` flag.

​	如果应用程序需要基于应用程序状态进行路由决策，或者需要完全控制路由请求到服务任务的过程，则使用路由网格可能不适合。可以使用 `--publish` 标志的 `mode=host` 选项，将服务端口直接发布到其运行的节点上。

> **Note**
>
> 
>
> If you publish a service's ports directly on the swarm node using `mode=host` and also set `published=<PORT>` this creates an implicit limitation that you can only run one task for that service on a given swarm node. You can work around this by specifying `published` without a port definition, which causes Docker to assign a random port for each task.
>
> ​	使用 `mode=host` 并设置 `published=<PORT>` 时，隐含的限制是每个 swarm 节点只能运行一个该服务的任务。可以通过省略端口定义来解决此问题，Docker 会为每个任务分配一个随机端口。
>
> In addition, if you use `mode=host` and you do not use the `--mode=global` flag on `docker service create`, it is difficult to know which nodes are running the service to route work to them.
>
> ​	另外，如果使用 `mode=host` 而未在 `docker service create` 中使用 `--mode=global` 标志，则很难确定哪些节点正在运行该服务以路由任务。

##### 示例：在每个 swarm 节点上运行 `nginx` Web 服务 Example: Run an `nginx` web server service on every swarm node

[nginx](https://hub.docker.com/_/nginx/) is an open source reverse proxy, load balancer, HTTP cache, and a web server. If you run nginx as a service using the routing mesh, connecting to the nginx port on any swarm node shows you the web page for (effectively) a random swarm node running the service.

​	[nginx](https://hub.docker.com/_/nginx/) 是一个开源的反向代理、负载均衡、HTTP 缓存和 Web 服务器。使用路由网格运行 nginx 服务时，连接到任意 swarm 节点的 nginx 端口时会看到一个随机节点的网页。

The following example runs nginx as a service on each node in your swarm and exposes nginx port locally on each swarm node.

​	以下示例在 swarm 中的每个节点上运行 nginx 服务，并将 nginx 端口暴露在每个 swarm 节点的本地。

```console
$ docker service create \
  --mode global \
  --publish mode=host,target=80,published=8080 \
  --name=nginx \
  nginx:latest
```

You can reach the nginx server on port 8080 of every swarm node. If you add a node to the swarm, a nginx task is started on it. You cannot start another service or container on any swarm node which binds to port 8080.

​	可以通过 swarm 中每个节点的 8080 端口访问 nginx 服务器。添加节点时会在新节点上启动一个 nginx 任务。无法在绑定到 8080 端口的 swarm 节点上启动其他服务或容器。

> **Note**
>
> 
>
> This is a purely illustrative example. Creating an application-layer routing framework for a multi-tiered service is complex and out of scope for this topic.
>
> ​	该示例仅用于说明。为多层服务构建应用层路由框架较为复杂，此处不做讨论。

### 将服务连接到覆盖网络 Connect the service to an overlay network

You can use overlay networks to connect one or more services within the swarm.

​	可以使用覆盖网络将一个或多个服务连接到 swarm 中。

First, create overlay network on a manager node using the `docker network create` command with the `--driver overlay` flag.

​	首先，在管理节点上使用 `docker network create` 命令和 `--driver overlay` 标志创建覆盖网络。



```console
$ docker network create --driver overlay my-network
```

After you create an overlay network in swarm mode, all manager nodes have access to the network.

​	在 swarm 模式中创建覆盖网络后，所有管理节点均可访问该网络。

You can create a new service and pass the `--network` flag to attach the service to the overlay network:

​	可以创建新服务并使用 `--network` 标志将服务连接到覆盖网络：

```console
$ docker service create \
  --replicas 3 \
  --network my-network \
  --name my-web \
  nginx
```

The swarm extends `my-network` to each node running the service.

​	swarm 会在运行服务的每个节点上扩展 `my-network`。

You can also connect an existing service to an overlay network using the `--network-add` flag.

​	还可以使用 `--network-add` 标志将现有服务连接到覆盖网络：

```console
$ docker service update --network-add my-network my-web
```

To disconnect a running service from a network, use the `--network-rm` flag.

​	若要从网络中断开运行中的服务，请使用 `--network-rm` 标志：

```console
$ docker service update --network-rm my-network my-web
```

For more information on overlay networking and service discovery, refer to [Attach services to an overlay network]({{< ref "/manuals/DockerEngine/Swarmmode/Manageswarmservicenetworks" >}}) and [Docker swarm mode overlay network security model]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Overlaynetworkdriver" >}}).

​	有关覆盖网络和服务发现的更多信息，请参考 [将服务附加到覆盖网络]({{< ref "/manuals/DockerEngine/Swarmmode/Manageswarmservicenetworks" >}}) 和 [Docker swarm 模式覆盖网络安全模型]({{< ref "/manuals/DockerEngine/Networking/Networkdrivers/Overlaynetworkdriver" >}})。

### 授予服务访问秘密的权限 Grant a service access to secrets

To create a service with access to Docker-managed secrets, use the `--secret` flag. For more information, see [Manage sensitive strings (secrets) for Docker services]({{< ref "/manuals/DockerEngine/Swarmmode/ManagesensitivedatawithDockersecrets" >}})

​	要创建可以访问 Docker 管理的秘密的服务，使用 `--secret` 标志。有关更多信息，请参阅 [管理 Docker 服务的敏感字符串（秘密）]({{< ref "/manuals/DockerEngine/Swarmmode/ManagesensitivedatawithDockersecrets" >}})。

### 自定义服务的隔离模式 Customize a service's isolation mode

> **Important**
>
> 
>
> This setting applies to Windows hosts only and is ignored for Linux hosts.
>
> ​	该设置仅适用于 Windows 主机，对 Linux 主机无效。

Docker allows you to specify a swarm service's isolation mode. The isolation mode can be one of the following:

​	Docker 允许您指定 Swarm 服务的隔离模式，隔离模式可以是以下几种之一：

- `default`: Use the default isolation mode configured for the Docker host, as configured by the `-exec-opt` flag or `exec-opts` array in `daemon.json`. If the daemon does not specify an isolation technology, `process` is the default for Windows Server, and `hyperv` is the default (and only) choice for Windows 10.
  - `default`：使用 Docker 主机配置的默认隔离模式，由 `-exec-opt` 标志或 `daemon.json` 中的 `exec-opts` 数组指定。如果守护进程未指定隔离技术，则 Windows Server 默认使用 `process`，Windows 10 默认（也是唯一支持）的选项为 `hyperv`。

- `process`: Run the service tasks as a separate process on the host. 

  - `process`：以独立进程的方式在主机上运行服务任务。


  > **Note**
  >
  > 
  >
  > `process` isolation mode is only supported on Windows Server. Windows 10 only supports `hyperv` isolation mode.
  >
  > `process` 隔离模式仅支持 Windows Server。Windows 10 仅支持 `hyperv` 隔离模式。

- `hyperv`: Run the service tasks as isolated `hyperv` tasks. This increases overhead but provides more isolation.

  - `hyperv`：以隔离的 `hyperv` 任务运行服务任务，这会增加开销，但提供了更多隔离。


You can specify the isolation mode when creating or updating a new service using the `--isolation` flag.

​	您可以在创建或更新服务时使用 `--isolation` 标志来指定隔离模式。

### 控制服务的部署 Control service placement

Swarm services provide a few different ways for you to control scale and placement of services on different nodes.

​	Swarm 服务提供了几种不同的方法来控制服务的扩展和在不同节点上的部署。

- You can specify whether the service needs to run a specific number of replicas or should run globally on every worker node. See [Replicated or global services](https://docs.docker.com/engine/swarm/services/#replicated-or-global-services).
  - 您可以指定服务是否需要运行特定数量的副本，或者应在每个工作节点上全局运行。详情请参阅[复制或全局服务](https://docs.docker.com/engine/swarm/services/#replicated-or-global-services)。

- You can configure the service's [CPU or memory requirements](https://docs.docker.com/engine/swarm/services/#reserve-memory-or-cpus-for-a-service), and the service only runs on nodes which can meet those requirements.

  - 您可以配置服务的 [CPU 或内存需求](https://docs.docker.com/engine/swarm/services/#reserve-memory-or-cpus-for-a-service)，服务仅在能够满足这些需求的节点上运行。

- [Placement constraints](https://docs.docker.com/engine/swarm/services/#placement-constraints) let you configure the service to run only on nodes with specific (arbitrary) metadata set, and cause the deployment to fail if appropriate nodes do not exist. For instance, you can specify that your service should only run on nodes where an arbitrary label `pci_compliant` is set to `true`.

  - [放置约束](https://docs.docker.com/engine/swarm/services/#placement-constraints)允许您配置服务仅在具有特定（任意）元数据的节点上运行，如果没有合适的节点，部署将失败。例如，可以指定服务仅在标签 `pci_compliant` 设置为 `true` 的节点上运行。

- [Placement preferences](https://docs.docker.com/engine/swarm/services/#placement-preferences) let you apply an arbitrary label with a range of values to each node, and spread your service's tasks across those nodes using an algorithm. Currently, the only supported algorithm is `spread`, which tries to place them evenly. For instance, if you label each node with a label `rack` which has a value from 1-10, then specify a placement preference keyed on `rack`, then service tasks are placed as evenly as possible across all nodes with the label `rack`, after taking other placement constraints, placement preferences, and other node-specific limitations into account.

  - [放置偏好](https://docs.docker.com/engine/swarm/services/#placement-preferences)允许您向每个节点应用一组值的任意标签，并使用算法将服务任务分布在这些节点上。目前唯一支持的算法是 `spread`，即尝试将任务均匀地分布在符合条件的节点上。例如，如果每个节点上都有一个 `rack` 标签并且值范围为 1-10，您可以指定一个基于 `rack` 的放置偏好，那么服务任务将尽可能均匀地分布在所有具有 `rack` 标签的节点上，同时考虑其他放置约束、偏好和节点特定的限制。

  Unlike constraints, placement preferences are best-effort, and a service does not fail to deploy if no nodes can satisfy the preference. If you specify a placement preference for a service, nodes that match that preference are ranked higher when the swarm managers decide which nodes should run the service tasks. Other factors, such as high availability of the service, also factor into which nodes are scheduled to run service tasks. For example, if you have N nodes with the rack label (and then some others), and your service is configured to run N+1 replicas, the +1 is scheduled on a node that doesn't already have the service on it if there is one, regardless of whether that node has the `rack` label or not.

  ​	与约束不同，放置偏好是尽力而为的，如果没有节点能够满足偏好，服务不会因此而部署失败。如果为服务指定放置偏好，符合该偏好的节点在调度服务任务时的优先级较高。例如，若有 N 个带有 `rack` 标签的节点（以及一些其他节点），且服务配置为运行 N+1 个副本，那么 `+1` 将被安排在尚未运行该服务的节点上。


#### 复制或全局服务 Replicated or global services

Swarm mode has two types of services: replicated and global. For replicated services, you specify the number of replica tasks for the swarm manager to schedule onto available nodes. For global services, the scheduler places one task on each available node that meets the service's [placement constraints](https://docs.docker.com/engine/swarm/services/#placement-constraints) and [resource requirements](https://docs.docker.com/engine/swarm/services/#reserve-memory-or-cpus-for-a-service).

​	Swarm 模式有两种服务类型：复制和全局。对于复制服务，您可以指定 Swarm 管理器在可用节点上调度的副本任务数量。对于全局服务，调度器会在满足服务[放置约束](https://docs.docker.com/engine/swarm/services/#placement-constraints)和[资源需求](https://docs.docker.com/engine/swarm/services/#reserve-memory-or-cpus-for-a-service)的每个可用节点上放置一个任务。

You control the type of service using the `--mode` flag. If you don't specify a mode, the service defaults to `replicated`. For replicated services, you specify the number of replica tasks you want to start using the `--replicas` flag. For example, to start a replicated nginx service with 3 replica tasks:

​	通过 `--mode` 标志控制服务类型。如果未指定模式，服务默认是 `replicated`。对于复制服务，可以使用 `--replicas` 标志指定所需的副本数量。例如，启动一个具有 3 个副本任务的 nginx 复制服务：



```console
$ docker service create \
  --name my_web \
  --replicas 3 \
  nginx
```

To start a global service on each available node, pass `--mode global` to `docker service create`. Every time a new node becomes available, the scheduler places a task for the global service on the new node. For example to start a service that runs alpine on every node in the swarm:

​	要在每个可用节点上启动全局服务，请向 `docker service create` 传递 `--mode global`。每次新节点可用时，调度器会在该新节点上放置一个全局服务任务。例如，在 Swarm 中的每个节点上运行 alpine 服务：



```console
$ docker service create \
  --name myservice \
  --mode global \
  alpine top
```

Service constraints let you set criteria for a node to meet before the scheduler deploys a service to the node. You can apply constraints to the service based upon node attributes and metadata or engine metadata. For more information on constraints, refer to the `docker service create` [CLI reference]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}}).

​	服务约束允许您设置节点必须满足的条件，以便调度器可以在该节点上部署服务。可以根据节点属性和元数据或引擎元数据将约束应用于服务。有关约束的更多信息，请参阅 `docker service create` [CLI参考]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}})。

#### 为服务保留内存或CPU - Reserve memory or CPUs for a service

To reserve a given amount of memory or number of CPUs for a service, use the `--reserve-memory` or `--reserve-cpu` flags. If no available nodes can satisfy the requirement (for instance, if you request 4 CPUs and no node in the swarm has 4 CPUs), the service remains in a pending state until an appropriate node is available to run its tasks.

​	要为服务保留指定数量的内存或CPU，请使用 `--reserve-memory` 或 `--reserve-cpu` 标志。如果没有可用节点能满足此要求（例如，您请求4个CPU，而Swarm中没有节点具备4个CPU），则服务将保持挂起状态，直到有合适的节点可用于运行其任务。

##### 内存不足异常（OOME） Out Of Memory Exceptions (OOME)

If your service attempts to use more memory than the swarm node has available, you may experience an Out Of Memory Exception (OOME) and a container, or the Docker daemon, might be killed by the kernel OOM killer. To prevent this from happening, ensure that your application runs on hosts with adequate memory and see [Understand the risks of running out of memory](https://docs.docker.com/engine/containers/resource_constraints/#understand-the-risks-of-running-out-of-memory).

​	如果您的服务尝试使用超过Swarm节点可用内存的资源，可能会遇到内存不足异常（OOME），并且容器或Docker守护进程可能会被内核的OOM终止程序杀死。为防止发生此类问题，请确保您的应用程序在具有足够内存的主机上运行，并查看[了解内存不足的风险](https://docs.docker.com/engine/containers/resource_constraints/#understand-the-risks-of-running-out-of-memory)。

Swarm services allow you to use resource constraints, placement preferences, and labels to ensure that your service is deployed to the appropriate swarm nodes.

​	Swarm服务允许您使用资源约束、放置偏好和标签，确保您的服务部署在合适的Swarm节点上。

#### 放置约束 Placement constraints

Use placement constraints to control the nodes a service can be assigned to. In the following example, the service only runs on nodes with the [label](https://docs.docker.com/engine/swarm/manage-nodes/#add-or-remove-label-metadata) `region` set to `east`. If no appropriately-labelled nodes are available, tasks will wait in `Pending` until they become available. The `--constraint` flag uses an equality operator (`==` or `!=`). For replicated services, it is possible that all services run on the same node, or each node only runs one replica, or that some nodes don't run any replicas. For global services, the service runs on every node that meets the placement constraint and any [resource requirements](https://docs.docker.com/engine/swarm/services/#reserve-memory-or-cpus-for-a-service).

​	使用放置约束可以控制服务可以分配到的节点。在以下示例中，服务仅在标签 [label](https://docs.docker.com/engine/swarm/manage-nodes/#add-or-remove-label-metadata) `region` 设置为 `east` 的节点上运行。如果没有适当标记的节点，则任务将等待 `Pending` 状态，直到它们变为可用。`--constraint` 标志使用等式运算符（`==` 或 `!=`）。对于复制服务，所有副本可能都在同一节点上运行，或每个节点仅运行一个副本，也可能某些节点不运行任何副本。对于全局服务，服务将在满足放置约束和[资源需求](https://docs.docker.com/engine/swarm/services/#reserve-memory-or-cpus-for-a-service)的每个节点上运行。



```console
$ docker service create \
  --name my-nginx \
  --replicas 5 \
  --constraint node.labels.region==east \
  nginx
```

You can also use the `constraint` service-level key in a `compose.yml` file.

​	在 `compose.yml` 文件中也可以使用 `constraint` 服务级别的键。

If you specify multiple placement constraints, the service only deploys onto nodes where they are all met. The following example limits the service to run on all nodes where `region` is set to `east` and `type` is not set to `devel`:

​	如果指定多个放置约束，服务仅在满足所有条件的节点上部署。以下示例将服务限制在 `region` 为 `east` 且 `type` 不为 `devel` 的所有节点上运行：

```console
$ docker service create \
  --name my-nginx \
  --mode global \
  --constraint node.labels.region==east \
  --constraint node.labels.type!=devel \
  nginx
```

You can also use placement constraints in conjunction with placement preferences and CPU/memory constraints. Be careful not to use settings that are not possible to fulfill.

​	您还可以结合使用放置约束、放置偏好和 CPU/内存约束，但需小心不要使用无法满足的设置。

For more information on constraints, refer to the `docker service create` [CLI reference]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}}).

​	更多放置约束的详细信息，请参阅 `docker service create` [CLI参考]({{< ref "/reference/CLIreference/docker/dockerservice/dockerservicecreate" >}})。

#### 放置偏好 Placement preferences

While [placement constraints](https://docs.docker.com/engine/swarm/services/#placement-constraints) limit the nodes a service can run on, *placement preferences* try to place tasks on appropriate nodes in an algorithmic way (currently, only spread evenly). For instance, if you assign each node a `rack` label, you can set a placement preference to spread the service evenly across nodes with the `rack` label, by value. This way, if you lose a rack, the service is still running on nodes on other racks.

​	虽然[放置约束](https://docs.docker.com/engine/swarm/services/#placement-constraints)限制了服务可以运行的节点，*放置偏好*尝试在合适的节点上以算法方式（目前仅支持均匀分布）分布任务。例如，如果您为每个节点分配一个 `rack` 标签，可以设置放置偏好，使服务在具有 `rack` 标签的节点之间按值均匀分布。这样，即使某个 `rack` 出现问题，服务仍在其他 `rack` 节点上运行。

Placement preferences are not strictly enforced. If no node has the label you specify in your preference, the service is deployed as though the preference were not set.

​	放置偏好并不是严格的。如果没有节点具有您在偏好中指定的标签，服务仍会部署。

> **Note**
>
> 
>
> Placement preferences are ignored for global services.
>
> ​	放置偏好不适用于全局服务。

The following example sets a preference to spread the deployment across nodes based on the value of the `datacenter` label. If some nodes have `datacenter=us-east` and others have `datacenter=us-west`, the service is deployed as evenly as possible across the two sets of nodes.

​	以下示例设置了偏好，根据 `datacenter` 标签的值在节点间分布部署。如果一些节点具有 `datacenter=us-east`，另一些节点具有 `datacenter=us-west`，则服务尽可能均匀地分布在两组节点中。



```console
$ docker service create \
  --replicas 9 \
  --name redis_2 \
  --placement-pref 'spread=node.labels.datacenter' \
  redis:3.0.6
```

> **Note**
>
> 
>
> Nodes which are missing the label used to spread still receive task assignments. As a group, these nodes receive tasks in equal proportion to any of the other groups identified by a specific label value. In a sense, a missing label is the same as having the label with a null value attached to it. If the service should only run on nodes with the label being used for the spread preference, the preference should be combined with a constraint.
>
> ​	缺少标签的节点仍将接收任务分配。这些节点整体上接收的任务与每个其他特定标签值组的节点接收的任务成比例。在某种意义上，缺少标签相当于具有标签且值为空的节点。如果服务仅应在带有扩展偏好标签的节点上运行，应将偏好与约束结合使用。

You can specify multiple placement preferences, and they are processed in the order they are encountered. The following example sets up a service with multiple placement preferences. Tasks are spread first over the various datacenters, and then over racks (as indicated by the respective labels):

​	您可以指定多个放置偏好，它们会按照遇到的顺序进行处理。以下示例为服务设置了多个放置偏好。任务首先分布在不同的数据中心，然后再分布在机架之间（由相应的标签指示）：



```console
$ docker service create \
  --replicas 9 \
  --name redis_2 \
  --placement-pref 'spread=node.labels.datacenter' \
  --placement-pref 'spread=node.labels.rack' \
  redis:3.0.6
```

You can also use placement preferences in conjunction with placement constraints or CPU/memory constraints. Be careful not to use settings that are not possible to fulfill.

​	您还可以将放置偏好与放置约束或 CPU/内存约束结合使用，但请注意不要使用无法满足的设置。

This diagram illustrates how placement preferences work:

​	下图说明了放置偏好的工作原理：

![How placement preferences work](Deployservicestoaswarm_img/placement_prefs.png)

When updating a service with `docker service update`, `--placement-pref-add` appends a new placement preference after all existing placement preferences. `--placement-pref-rm` removes an existing placement preference that matches the argument.

​	在使用 `docker service update` 更新服务时，`--placement-pref-add` 会在所有现有放置偏好后追加一个新的放置偏好。`--placement-pref-rm` 会删除与参数匹配的现有放置偏好。

### 配置服务的更新行为 Configure a service's update behavior

When you create a service, you can specify a rolling update behavior for how the swarm should apply changes to the service when you run `docker service update`. You can also specify these flags as part of the update, as arguments to `docker service update`.

​	创建服务时，可以指定滚动更新行为，以定义在运行 `docker service update` 时 Swarm 应如何应用更改。也可以在更新时通过 `docker service update` 参数指定这些标志。

The `--update-delay` flag configures the time delay between updates to a service task or sets of tasks. You can describe the time `T` as a combination of the number of seconds `Ts`, minutes `Tm`, or hours `Th`. So `10m30s` indicates a 10 minute 30 second delay.

​	`--update-delay` 标志用于配置服务任务或任务组之间的更新延迟时间。时间 `T` 可以是秒（Ts）、分钟（Tm）或小时（Th）的组合，例如 `10m30s` 表示10分30秒的延迟。

By default the scheduler updates 1 task at a time. You can pass the `--update-parallelism` flag to configure the maximum number of service tasks that the scheduler updates simultaneously.

​	默认情况下，调度器每次更新1个任务。可以通过 `--update-parallelism` 标志配置调度器同时更新的服务任务的最大数量。

When an update to an individual task returns a state of `RUNNING`, the scheduler continues the update by continuing to another task until all tasks are updated. If at any time during an update a task returns `FAILED`, the scheduler pauses the update. You can control the behavior using the `--update-failure-action` flag for `docker service create` or `docker service update`.

​	当单个任务更新状态变为 `RUNNING` 时，调度器会继续更新下一个任务，直到所有任务都更新完毕。如果在更新过程中某个任务返回 `FAILED`，调度器会暂停更新。您可以使用 `docker service create` 或 `docker service update` 的 `--update-failure-action` 标志控制行为。

In the example service below, the scheduler applies updates to a maximum of 2 replicas at a time. When an updated task returns either `RUNNING` or `FAILED`, the scheduler waits 10 seconds before stopping the next task to update:

​	以下示例服务中，调度器最多同时更新2个副本。当更新的任务返回 `RUNNING` 或 `FAILED` 时，调度器会在更新下一个任务前等待10秒：



```console
$ docker service create \
  --replicas 10 \
  --name my_web \
  --update-delay 10s \
  --update-parallelism 2 \
  --update-failure-action continue \
  alpine
```

The `--update-max-failure-ratio` flag controls what fraction of tasks can fail during an update before the update as a whole is considered to have failed. For example, with `--update-max-failure-ratio 0.1 --update-failure-action pause`, after 10% of the tasks being updated fail, the update is paused.

​	`--update-max-failure-ratio` 标志控制在更新过程中可以容忍的任务失败比例。例如，使用 `--update-max-failure-ratio 0.1 --update-failure-action pause`，如果有10%的任务更新失败，则更新将暂停。

An individual task update is considered to have failed if the task doesn't start up, or if it stops running within the monitoring period specified with the `--update-monitor` flag. The default value for `--update-monitor` is 30 seconds, which means that a task failing in the first 30 seconds after it's started counts towards the service update failure threshold, and a failure after that is not counted.

​	如果任务未启动或在 `--update-monitor` 标志指定的监视时间内停止，则视为任务更新失败。默认值为30秒，即任务在启动30秒内失败将计入服务更新失败阈值，而之后的失败不计入。

### 回滚到服务的前一个版本 Roll back to the previous version of a service

In case the updated version of a service doesn't function as expected, it's possible to manually roll back to the previous version of the service using `docker service update`'s `--rollback` flag. This reverts the service to the configuration that was in place before the most recent `docker service update` command.

​	如果更新后的服务未按预期工作，可以使用 `docker service update` 的 `--rollback` 标志手动回滚到服务的前一个版本。这会将服务恢复到最近一次 `docker service update` 命令之前的配置。

Other options can be combined with `--rollback`; for example, `--update-delay 0s`, to execute the rollback without a delay between tasks:

​	可以将其他选项与 `--rollback` 结合使用，例如 `--update-delay 0s`，以无延迟地执行回滚：



```console
$ docker service update \
  --rollback \
  --update-delay 0s
  my_web
```

You can configure a service to roll back automatically if a service update fails to deploy. See [Automatically roll back if an update fails](https://docs.docker.com/engine/swarm/services/#automatically-roll-back-if-an-update-fails).

​	您可以配置服务在更新失败时自动回滚。请参阅[如果更新失败自动回滚](https://docs.docker.com/engine/swarm/services/#automatically-roll-back-if-an-update-fails)。

Manual rollback is handled at the server side, which allows manually-initiated rollbacks to respect the new rollback parameters. Note that `--rollback` cannot be used in conjunction with other flags to `docker service update`.

​	手动回滚在服务器端处理，允许手动回滚操作尊重新的回滚参数。请注意，`--rollback` 不能与 `docker service update` 的其他标志一起使用。

### 如果更新失败自动回滚 Automatically roll back if an update fails

You can configure a service in such a way that if an update to the service causes redeployment to fail, the service can automatically roll back to the previous configuration. This helps protect service availability. You can set one or more of the following flags at service creation or update. If you do not set a value, the default is used.

​	您可以将服务配置为在更新导致重新部署失败时自动回滚到以前的配置，这有助于保护服务的可用性。可以在创建或更新服务时设置以下一个或多个标志。如果未设置值，则使用默认值。

| Flag                           | Default | Description                                                  |
| :----------------------------- | :------ | :----------------------------------------------------------- |
| `--rollback-delay`             | `0s`    | 在回滚任务后等待的时间，然后再回滚下一个任务。值为 `0` 表示在第一个任务回滚完成后立即回滚第二个任务。 Amount of time to wait after rolling back a task before rolling back the next one. A value of `0` means to roll back the second task immediately after the first rolled-back task deploys. |
| `--rollback-failure-action`    | `pause` | 当任务回滚失败时，选择是否暂停（`pause`）或继续（`continue`）尝试回滚其他任务。When a task fails to roll back, whether to `pause` or `continue` trying to roll back other tasks. |
| `--rollback-max-failure-ratio` | `0`     | 在回滚过程中可以容忍的失败比例，指定为 0 到 1 之间的浮点数。例如，对于 5 个任务，失败比例为 `.2` 表示可以容忍一个任务回滚失败。值为 `0` 表示不容忍任何失败，而值为 `1` 表示容忍任意数量的失败。The failure rate to tolerate during a rollback, specified as a floating-point number between 0 and 1. For instance, given 5 tasks, a failure ratio of `.2` would tolerate one task failing to roll back. A value of `0` means no failure are tolerated, while a value of `1` means any number of failure are tolerated. |
| `--rollback-monitor`           | `5s`    | 每个任务回滚后的监视时间。如果任务在此期间停止，回滚将被视为失败。Duration after each task rollback to monitor for failure. If a task stops before this time period has elapsed, the rollback is considered to have failed. |
| `--rollback-parallelism`       | `1`     | 最大并行回滚任务数。默认情况下每次仅回滚一个任务，值为 `0` 表示所有任务并行回滚。The maximum number of tasks to roll back in parallel. By default, one task is rolled back at a time. A value of `0` causes all tasks to be rolled back in parallel. |

The following example configures a `redis` service to roll back automatically if a `docker service update` fails to deploy. Two tasks can be rolled back in parallel. Tasks are monitored for 20 seconds after rollback to be sure they do not exit, and a maximum failure ratio of 20% is tolerated. Default values are used for `--rollback-delay` and `--rollback-failure-action`.

​	以下示例配置了 `redis` 服务，以便在 `docker service update` 部署失败时自动回滚。最多可以并行回滚两个任务。任务在回滚后监视 20 秒以确保它们不会退出，并容忍最高 20% 的失败率。`--rollback-delay` 和 `--rollback-failure-action` 使用默认值。



```console
$ docker service create --name=my_redis \
                        --replicas=5 \
                        --rollback-parallelism=2 \
                        --rollback-monitor=20s \
                        --rollback-max-failure-ratio=.2 \
                        redis:latest
```

### 为服务提供数据卷或绑定挂载的访问权限 Give a service access to volumes or bind mounts

For best performance and portability, you should avoid writing important data directly into a container's writable layer. You should instead use data volumes or bind mounts. This principle also applies to services.

​	为了获得最佳性能和可移植性，应避免将重要数据直接写入容器的可写层，而应使用数据卷或绑定挂载。此原则同样适用于服务。

You can create two types of mounts for services in a swarm, `volume` mounts or `bind` mounts. Regardless of which type of mount you use, configure it using the `--mount` flag when you create a service, or the `--mount-add` or `--mount-rm` flag when updating an existing service. The default is a data volume if you don't specify a type.

​	在 Swarm 中，您可以为服务创建两种类型的挂载：`volume` 挂载或 `bind` 挂载。无论使用哪种挂载类型，都可以在创建服务时通过 `--mount` 标志进行配置，或在更新现有服务时使用 `--mount-add` 或 `--mount-rm`。如果未指定类型，默认情况下为数据卷。

#### 数据卷 Data volumes

Data volumes are storage that exist independently of a container. The lifecycle of data volumes under swarm services is similar to that under containers. Volumes outlive tasks and services, so their removal must be managed separately. Volumes can be created before deploying a service, or if they don't exist on a particular host when a task is scheduled there, they are created automatically according to the volume specification on the service.

​	数据卷是独立于容器存在的存储。Swarm 服务下的数据卷生命周期类似于容器，它们在任务和服务之中依旧存在，因此需要单独管理其删除。数据卷可以在部署服务前创建，或者在任务调度到特定主机上时，根据服务的卷规范自动创建。

To use existing data volumes with a service use the `--mount` flag:

​	要将现有的数据卷与服务一起使用，可以使用 `--mount` 标志：



```console
$ docker service create \
  --mount src=<VOLUME-NAME>,dst=<CONTAINER-PATH> \
  --name myservice \
  IMAGE
```

If a volume with the name `<VOLUME-NAME>` doesn't exist when a task is scheduled to a particular host, then one is created. The default volume driver is `local`. To use a different volume driver with this create-on-demand pattern, specify the driver and its options with the `--mount` flag:

​	如果在调度任务到特定主机时不存在名为 `<VOLUME-NAME>` 的卷，则会自动创建一个。默认的卷驱动程序为 `local`。如果希望在按需创建模式下使用不同的卷驱动程序，可以使用 `--mount` 标志指定驱动程序及其选项：



```console
$ docker service create \
  --mount type=volume,src=<VOLUME-NAME>,dst=<CONTAINER-PATH>,volume-driver=DRIVER,volume-opt=<KEY0>=<VALUE0>,volume-opt=<KEY1>=<VALUE1>
  --name myservice \
  IMAGE
```

For more information on how to create data volumes and the use of volume drivers, see [Use volumes]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}}).

​	有关如何创建数据卷和使用卷驱动程序的更多信息，请参见[使用卷]({{< ref "/manuals/DockerEngine/Storage/Volumes" >}})。

#### 绑定挂载 Bind mounts

Bind mounts are file system paths from the host where the scheduler deploys the container for the task. Docker mounts the path into the container. The file system path must exist before the swarm initializes the container for the task.

​	绑定挂载是调度器将任务容器部署的主机文件系统路径。Docker 会将该路径挂载到容器中。文件系统路径在 Swarm 初始化容器之前必须存在。

The following examples show bind mount syntax:

​	以下示例展示了绑定挂载的语法：

- To mount a read-write bind:

  读写绑定挂载：

  ```console
  $ docker service create \
    --mount type=bind,src=<HOST-PATH>,dst=<CONTAINER-PATH> \
    --name myservice \
    IMAGE
  ```

- To mount a read-only bind:

  只读绑定挂载：

  ```console
  $ docker service create \
    --mount type=bind,src=<HOST-PATH>,dst=<CONTAINER-PATH>,readonly \
    --name myservice \
    IMAGE
  ```

> **Important**
>
> 
>
> Bind mounts can be useful but they can also cause problems. In most cases, it is recommended that you architect your application such that mounting paths from the host is unnecessary. The main risks include the following:
>
> ​	绑定挂载虽有用，但可能引发问题。在大多数情况下，建议将应用程序架构化，使得不必从主机挂载路径。主要风险包括：
>
> - If you bind mount a host path into your service’s containers, the path must exist on every swarm node. The Docker swarm mode scheduler can schedule containers on any machine that meets resource availability requirements and satisfies all constraints and placement preferences you specify.
>   - 如果将主机路径绑定挂载到服务的容器中，则每个 Swarm 节点上必须存在该路径。Docker Swarm 模式的调度器可以在满足资源可用性要求并满足所有约束和放置偏好设置的任意机器上调度容器。
> - The Docker swarm mode scheduler may reschedule your running service containers at any time if they become unhealthy or unreachable.
>   - 如果运行中的服务容器变得不健康或不可访问，Docker Swarm 模式的调度器可能会随时重新调度它们。
> - Host bind mounts are non-portable. When you use bind mounts, there is no guarantee that your application runs the same way in development as it does in production.
>   - 主机绑定挂载是非便携的。使用绑定挂载时，无法保证应用程序在开发环境和生产环境中具有相同的运行方式。

### 使用模板创建服务 Create services using templates

You can use templates for some flags of `service create`, using the syntax provided by the Go's [text/template](https://golang.org/pkg/text/template/) package.

​	您可以在 `service create` 的某些标志中使用模板，使用 Go 的 [text/template](https://golang.org/pkg/text/template/) 包提供的语法。

The following flags are supported:

​	以下标志支持模板：

- `--hostname`
- `--mount`
- `--env`

Valid placeholders for the Go template are:

​	Go 模板的有效占位符为：

| Placeholder       | Description              |
| :---------------- | :----------------------- |
| `.Service.ID`     | 服务 IDService ID        |
| `.Service.Name`   | 服务名称Service name     |
| `.Service.Labels` | 服务标签Service labels   |
| `.Node.ID`        | 节点 ID Node ID          |
| `.Node.Hostname`  | 节点主机名 Node hostname |
| `.Task.Name`      | 任务名称 Task name       |
| `.Task.Slot`      | 任务槽位 Task slot       |

#### 模板示例 Template example

This example sets the template of the created containers based on the service's name and the ID of the node where the container is running:

​	以下示例基于服务名称和运行容器的节点 ID 设置创建的容器模板：



```console
$ docker service create --name hosttempl \
                        --hostname="{{.Node.ID}}-{{.Service.Name}}"\
                         busybox top
```

To see the result of using the template, use the `docker service ps` and `docker inspect` commands.

​	要查看使用模板的结果，请使用 `docker service ps` 和 `docker inspect` 命令。



```console
$ docker service ps va8ew30grofhjoychbr6iot8c

ID            NAME         IMAGE                                                                                   NODE          DESIRED STATE  CURRENT STATE               ERROR  PORTS
wo41w8hg8qan  hosttempl.1  busybox:latest@sha256:29f5d56d12684887bdfa50dcd29fc31eea4aaf4ad3bec43daf19026a7ce69912  2e7a8a9c4da2  Running        Running about a minute ago
```



```console
$ docker inspect --format="{{.Config.Hostname}}" hosttempl.1.wo41w8hg8qanxwjwsg4kxpprj
```

## 了解更多 Learn More

- [Swarm administration guide Swarm 管理指南]({{< ref "/manuals/DockerEngine/Swarmmode/AdministerandmaintainaswarmofDockerEngines" >}})
- [Docker Engine command line reference Docker 引擎命令行参考]({{< ref "/reference/CLIreference/docker" >}})
- [Swarm mode tutorial Swarm 模式教程]({{< ref "/manuals/DockerEngine/Swarmmode/GettingstartedwithSwarmmode" >}})
