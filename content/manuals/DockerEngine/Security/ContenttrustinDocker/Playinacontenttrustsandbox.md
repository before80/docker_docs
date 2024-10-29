+++
title = "在内容信任沙盒中操作"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/security/trust/trust_sandbox/](https://docs.docker.com/engine/security/trust/trust_sandbox/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Play in a content trust sandbox - 在内容信任沙盒中操作

This page explains how to set up and use a sandbox for experimenting with trust. The sandbox allows you to configure and try trust operations locally without impacting your production images.

​	本文解释了如何设置和使用一个沙盒来实验信任操作。沙盒允许您在本地配置和尝试信任操作，而不会影响生产环境中的镜像。

Before working through this sandbox, you should have read through the [trust overview]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker" >}}).

​	在开始使用此沙盒之前，您应该先阅读[信任概述]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker" >}})。

## 前置条件 Prerequisites

These instructions assume you are running in Linux or macOS. You can run this sandbox on a local machine or on a virtual machine. You need to have privileges to run docker commands on your local machine or in the VM.

​	这些说明假设您正在 Linux 或 macOS 上运行。您可以在本地计算机或虚拟机上运行此沙盒。您需要有在本地计算机或虚拟机上运行 Docker 命令的权限。

This sandbox requires you to install two Docker tools: Docker Engine >= 1.10.0 and Docker Compose >= 1.6.0. To install the Docker Engine, choose from the [list of supported platforms]({{< ref "/manuals/DockerEngine/Install" >}}). To install Docker Compose, see the [detailed instructions here]({{< ref "/manuals/DockerCompose/Install" >}}).

​	此沙盒需要您安装两个 Docker 工具：Docker Engine >= 1.10.0 和 Docker Compose >= 1.6.0。要安装 Docker Engine，请从[支持的平台列表]({{< ref "/manuals/DockerEngine/Install" >}})中选择。要安装 Docker Compose，请查看[详细安装说明]({{< ref "/manuals/DockerCompose/Install" >}})。

## 沙盒包含什么？ What is in the sandbox?

If you are just using trust out-of-the-box you only need your Docker Engine client and access to the Docker Hub. The sandbox mimics a production trust environment, and sets up these additional components.

​	如果您只是开箱即用地使用信任，您只需要 Docker Engine 客户端和访问 Docker Hub 的权限。沙盒模拟了一个生产信任环境，并设置了以下附加组件：

| Container       | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| trustsandbox    | 包含最新版本的 Docker Engine 和一些预配置证书的容器。您可以在此沙盒中使用 `docker` 客户端测试信任操作。A container with the latest version of Docker Engine and with some preconfigured certificates. This is your sandbox where you can use the `docker` client to test trust operations. |
| Registry server | 本地注册表服务。A local registry service.                    |
| Notary server   | 处理信任管理的服务。The service that does all the heavy-lifting of managing trust |

This means you run your own content trust (Notary) server and registry. If you work exclusively with the Docker Hub, you would not need these components. They are built into the Docker Hub for you. For the sandbox, however, you build your own entire, mock production environment.

​	这意味着您运行了自己的内容信任（Notary）服务器和注册表。如果您只使用 Docker Hub，就不需要这些组件。它们已内置在 Docker Hub 中。但在沙盒中，您需要构建自己的完整模拟生产环境。

Within the `trustsandbox` container, you interact with your local registry rather than the Docker Hub. This means your everyday image repositories are not used. They are protected while you play.

​	在 `trustsandbox` 容器中，您与本地注册表交互，而不是 Docker Hub，这意味着您不会使用常用的镜像库。这样可以保护您的常用镜像不被影响。

When you play in the sandbox, you also create root and repository keys. The sandbox is configured to store all the keys and files inside the `trustsandbox` container. Since the keys you create in the sandbox are for play only, destroying the container destroys them as well.

​	在沙盒中，您还会创建 root 和仓库密钥。沙盒配置为将所有密钥和文件存储在 `trustsandbox` 容器内。由于您在沙盒中创建的密钥仅用于实验，销毁该容器也会销毁这些密钥。

By using a docker-in-docker image for the `trustsandbox` container, you also don't pollute your real Docker daemon cache with any images you push and pull. The images are stored in an anonymous volume attached to this container, and can be destroyed after you destroy the container.

​	通过使用 docker-in-docker 镜像作为 `trustsandbox` 容器，您也不会污染真实的 Docker 守护进程缓存中任何推送和拉取的镜像。镜像存储在附加到此容器的匿名卷中，销毁容器后即可销毁这些镜像。

## 构建沙盒 Build the sandbox

In this section, you use Docker Compose to specify how to set up and link together the `trustsandbox` container, the Notary server, and the Registry server.

​	在本节中，您将使用 Docker Compose 指定如何设置和链接 `trustsandbox` 容器、Notary 服务器和 Registry 服务器。

1. Create a new `trustsandbox` directory and change into it. 创建一个新的 `trustsandbox` 目录并进入其中。

   ```
    $ mkdir trustsandbox
    $ cd trustsandbox
   ```

2. Create a file called `compose.yml` with your favorite editor. For example, using vim: 使用您喜欢的编辑器创建一个名为 `compose.yml` 的文件。例如，使用 vim：

   ```
    $ touch compose.yml
    $ vim compose.yml
   ```

3. Add the following to the new file. 将以下内容添加到新文件中。

   ```
    version: "2"
    services:
      notaryserver:
        image: dockersecurity/notary_autobuilds:server-v0.5.1
        volumes:
          - notarycerts:/var/lib/notary/fixtures
        networks:
          - sandbox
        environment:
          - NOTARY_SERVER_STORAGE_TYPE=memory
          - NOTARY_SERVER_TRUST_SERVICE_TYPE=local
      sandboxregistry:
        image: registry:2.4.1
        networks:
          - sandbox
        container_name: sandboxregistry
      trustsandbox:
        image: docker:dind
        networks:
          - sandbox
        volumes:
          - notarycerts:/notarycerts
        privileged: true
        container_name: trustsandbox
        entrypoint: ""
        command: |-
            sh -c '
                cp /notarycerts/root-ca.crt /usr/local/share/ca-certificates/root-ca.crt &&
                update-ca-certificates &&
                dockerd-entrypoint.sh --insecure-registry sandboxregistry:5000'
    volumes:
      notarycerts:
        external: false
    networks:
      sandbox:
        external: false
   ```

4. Save and close the file. 保存并关闭文件。

5. Run the containers on your local system. 在本地系统上运行容器。

   ```
    $ docker compose up -d
   ```

   The first time you run this, the docker-in-docker, Notary server, and registry images are downloaded from Docker Hub.
   
   ​	第一次运行此命令时，会从 Docker Hub 下载 docker-in-docker、Notary 服务器和注册表镜像。

## 在沙盒中操作 Play in the sandbox

Now that everything is setup, you can go into your `trustsandbox` container and start testing Docker content trust. From your host machine, obtain a shell in the `trustsandbox` container.

​	现在一切已设置完成，您可以进入 `trustsandbox` 容器并开始测试 Docker 内容信任。从主机中获得 `trustsandbox` 容器的 shell。

```
$ docker container exec -it trustsandbox sh
/ #
```

### 测试一些信任操作 Test some trust operations

Now, pull some images from within the `trustsandbox` container.

​	现在，从 `trustsandbox` 容器内拉取一些镜像进行测试。

1. Download a `docker` image to test with. 下载一个 `docker` 镜像进行测试。

   ```
    / # docker pull docker/trusttest
    docker pull docker/trusttest
    Using default tag: latest
    latest: Pulling from docker/trusttest
   
    b3dbab3810fc: Pull complete
    a9539b34a6ab: Pull complete
    Digest: sha256:d149ab53f8718e987c3a3024bb8aa0e2caadf6c0328f1d9d850b2a2a67f2819a
    Status: Downloaded newer image for docker/trusttest:latest
   ```

2. Tag it to be pushed to our sandbox registry: 将其标记为推送到我们的沙盒注册表：

   ```
    / # docker tag docker/trusttest sandboxregistry:5000/test/trusttest:latest
   ```

3. Enable content trust. 启用内容信任。

   ```
    / # export DOCKER_CONTENT_TRUST=1
   ```

4. Identify the trust server. 指定信任服务器。

   ```
    / # export DOCKER_CONTENT_TRUST_SERVER=https://notaryserver:4443
   ```

   This step is only necessary because the sandbox is using its own server. Normally, if you are using the Docker Public Hub this step isn't necessary.

   ​	由于沙盒使用自己的服务器，此步骤是必要的。通常，使用 Docker 公共 Hub 时不需要此步骤。

5. Pull the test image. 拉取测试镜像。

   ```
    / # docker pull sandboxregistry:5000/test/trusttest
    Using default tag: latest
    Error: remote trust data does not exist for sandboxregistry:5000/test/trusttest: notaryserver:4443 does not have trust data for sandboxregistry:5000/test/trusttest
   ```

   You see an error, because this content doesn't exist on the `notaryserver` yet.

   ​	您会看到一个错误，因为此内容在 `notaryserver` 上尚不存在。

6. Push and sign the trusted image. 推送并签署受信任的镜像。

   ```
    / # docker push sandboxregistry:5000/test/trusttest:latest
    The push refers to a repository [sandboxregistry:5000/test/trusttest]
    5f70bf18a086: Pushed
    c22f7bc058a9: Pushed
    latest: digest: sha256:ebf59c538accdf160ef435f1a19938ab8c0d6bd96aef8d4ddd1b379edf15a926 size: 734
    Signing and pushing trust metadata
    You are about to create a new root signing key passphrase. This passphrase
    will be used to protect the most sensitive key in your signing system. Please
    choose a long, complex passphrase and be careful to keep the password and the
    key file itself secure and backed up. It is highly recommended that you use a
    password manager to generate the passphrase and keep it safe. There will be no
    way to recover this key. You can find the key in your config directory.
    Enter passphrase for new root key with ID 27ec255:
    Repeat passphrase for new root key with ID 27ec255:
    Enter passphrase for new repository key with ID 58233f9 (sandboxregistry:5000/test/trusttest):
    Repeat passphrase for new repository key with ID 58233f9 (sandboxregistry:5000/test/trusttest):
    Finished initializing "sandboxregistry:5000/test/trusttest"
    Successfully signed "sandboxregistry:5000/test/trusttest":latest
   ```

   Because you are pushing this repository for the first time, Docker creates new root and repository keys and asks you for passphrases with which to encrypt them. If you push again after this, it only asks you for repository passphrase so it can decrypt the key and sign again.

   ​	因为您是第一次推送这个存储库，Docker 会创建新的 root 和 repository 密钥，并要求您设置密码来加密这些密钥。如果在此之后再次推送，它只会要求您输入 repository 密钥的密码，以便解密密钥并重新签名。

7. Try pulling the image you just pushed:  尝试拉取您刚推送的镜像：

   ```
    / # docker pull sandboxregistry:5000/test/trusttest
    Using default tag: latest
    Pull (1 of 1): sandboxregistry:5000/test/trusttest:latest@sha256:ebf59c538accdf160ef435f1a19938ab8c0d6bd96aef8d4ddd1b379edf15a926
    sha256:ebf59c538accdf160ef435f1a19938ab8c0d6bd96aef8d4ddd1b379edf15a926: Pulling from test/trusttest
    Digest: sha256:ebf59c538accdf160ef435f1a19938ab8c0d6bd96aef8d4ddd1b379edf15a926
    Status: Downloaded newer image for sandboxregistry:5000/test/trusttest@sha256:ebf59c538accdf160ef435f1a19938ab8c0d6bd96aef8d4ddd1b379edf15a926
    Tagging sandboxregistry:5000/test/trusttest@sha256:ebf59c538accdf160ef435f1a19938ab8c0d6bd96aef8d4ddd1b379edf15a926 as sandboxregistry:5000/test/trusttest:latest
   ```

### 使用恶意镜像进行测试 Test with malicious images

What happens when data is corrupted and you try to pull it when trust is enabled? In this section, you go into the `sandboxregistry` and tamper with some data. Then, you try and pull it.

​	当数据被破坏且信任已启用时尝试拉取镜像，会发生什么？在本节中，您将进入 `sandboxregistry` 并篡改一些数据，然后尝试拉取它。

1. Leave the `trustsandbox` shell and container running. 保持 `trustsandbox` shell 和容器运行。

2. Open a new interactive terminal from your host, and obtain a shell into the `sandboxregistry` container. 从主机打开一个新终端并进入 `sandboxregistry` 容器的 shell。

   ```
   $ docker container exec -it sandboxregistry bash
   root@65084fc6f047:/#
   ```

3. List the layers for the `test/trusttest` image you pushed:

   列出您推送的 `test/trusttest` 镜像的层：

   ```console
   root@65084fc6f047:/# ls -l /var/lib/registry/docker/registry/v2/repositories/test/trusttest/_layers/sha256
   total 12
   drwxr-xr-x 2 root root 4096 Jun 10 17:26 a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4
   drwxr-xr-x 2 root root 4096 Jun 10 17:26 aac0c133338db2b18ff054943cee3267fe50c75cdee969aed88b1992539ed042
   drwxr-xr-x 2 root root 4096 Jun 10 17:26 cc7629d1331a7362b5e5126beb5bf15ca0bf67eb41eab994c719a45de53255cd
   ```

4. Change into the registry storage for one of those layers (this is in a different directory): 切换到这些层中一个的注册表存储（这是在不同的目录中）：

   ```
   root@65084fc6f047:/# cd /var/lib/registry/docker/registry/v2/blobs/sha256/aa/aac0c133338db2b18ff054943cee3267fe50c75cdee969aed88b1992539ed042
   ```

5. Add malicious data to one of the `trusttest` layers: 在 `trusttest` 的某个层中添加恶意数据：

   ```
   root@65084fc6f047:/# echo "Malicious data" > data
   ```

6. Go back to your `trustsandbox` terminal. 返回 `trustsandbox` 终端。

7. List the `trusttest` image. 列出 `trusttest` 镜像。

   ```
   / # docker image ls | grep trusttest
   REPOSITORY                            TAG                 IMAGE ID            CREATED             SIZE
   docker/trusttest                      latest              cc7629d1331a        11 months ago       5.025 MB
   sandboxregistry:5000/test/trusttest   latest              cc7629d1331a        11 months ago       5.025 MB
   sandboxregistry:5000/test/trusttest   <none>              cc7629d1331a        11 months ago       5.025 MB
   ```

8. Remove the `trusttest:latest` image from our local cache. 从本地缓存中删除 `trusttest:latest` 镜像。

   ```
   / # docker image rm -f cc7629d1331a
   Untagged: docker/trusttest:latest
   Untagged: sandboxregistry:5000/test/trusttest:latest
   Untagged: sandboxregistry:5000/test/trusttest@sha256:ebf59c538accdf160ef435f1a19938ab8c0d6bd96aef8d4ddd1b379edf15a926
   Deleted: sha256:cc7629d1331a7362b5e5126beb5bf15ca0bf67eb41eab994c719a45de53255cd
   Deleted: sha256:2a1f6535dc6816ffadcdbe20590045e6cbf048d63fd4cc753a684c9bc01abeea
   Deleted: sha256:c22f7bc058a9a8ffeb32989b5d3338787e73855bf224af7aa162823da015d44c
   ```

   Docker does not re-download images that it already has cached, but we want Docker to attempt to download the tampered image from the registry and reject it because it is invalid. 

   ​	Docker 不会重新下载已经缓存的镜像，但我们希望 Docker 尝试从注册表下载篡改的镜像并因无效而拒绝它。

9. Pull the image again. This downloads the image from the registry, because we don't have it cached. 再次拉取镜像。因为我们没有缓存它，所以这次会从注册表下载。

   ```
   / # docker pull sandboxregistry:5000/test/trusttest
   Using default tag: latest
   Pull (1 of 1): sandboxregistry:5000/test/trusttest:latest@sha256:35d5bc26fd358da8320c137784fe590d8fcf9417263ef261653e8e1c7f15672e
   sha256:35d5bc26fd358da8320c137784fe590d8fcf9417263ef261653e8e1c7f15672e: Pulling from test/trusttest
   
   aac0c133338d: Retrying in 5 seconds
   a3ed95caeb02: Download complete
   error pulling image configuration: unexpected EOF
   ```

   The pull did not complete because the trust system couldn't verify the image.
   
   ​	拉取未完成，因为信任系统无法验证镜像。

## 在沙盒中进一步操作 More play in the sandbox

Now, you have a full Docker content trust sandbox on your local system, feel free to play with it and see how it behaves. If you find any security issues with Docker, feel free to send us an email at [security@docker.com]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker/Playinacontenttrustsandbox" >}}).

​	现在，您在本地系统上有了一个完整的 Docker 内容信任沙盒，可以随意操作，看看它的行为。如果发现 Docker 的安全问题，可以发送邮件至 [security@docker.com]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker/Playinacontenttrustsandbox" >}})。

## 清理沙盒 Clean up your sandbox

When you are done, and want to clean up all the services you've started and any anonymous volumes that have been created, just run the following command in the directory where you've created your Docker Compose file:

​	完成后，如果要清理已启动的所有服务和创建的任何匿名卷，只需在创建 Docker Compose 文件的目录中运行以下命令：

```
    $ docker compose down -v
```
