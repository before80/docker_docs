+++
title = "Automation with content trust"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/engine/security/trust/trust_automation/](https://docs.docker.com/engine/security/trust/trust_automation/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Automation with content trust - 内容信任的自动化

It is very common for Docker Content Trust to be built into existing automation systems. To allow tools to wrap Docker and push trusted content, there are environment variables that can be passed through to the client.

​	在现有的自动化系统中集成 Docker 内容信任（DCT）非常常见。为了让工具能够包装 Docker 并推送受信任的内容，可以将环境变量传递给客户端。

This guide follows the steps as described in [Signing images with Docker Content Trust](https://docs.docker.com/engine/security/trust/#signing-images-with-docker-content-trust). Make sure you understand and follow the prerequisites.

​	本指南遵循[使用 Docker 内容信任签署镜像](https://docs.docker.com/engine/security/trust/#signing-images-with-docker-content-trust)中描述的步骤。请确保理解并遵循这些先决条件。

When working directly with the Notary client, it uses its [own set of environment variables](https://github.com/theupdateframework/notary/blob/master/docs/reference/client-config.md#environment-variables-optional).

​	当直接使用 Notary 客户端时，它使用[自己的一组环境变量](https://github.com/theupdateframework/notary/blob/master/docs/reference/client-config.md#environment-variables-optional)。

## 添加委托私钥 Add a delegation private key

To automate importing a delegation private key to the local Docker trust store, we need to pass a passphrase for the new key. This passphrase will be required everytime that delegation signs a tag.

​	为了自动将委托私钥导入本地 Docker 信任存储，我们需要为新密钥传递一个密码。每次使用该委托签署标签时都需要输入此密码。

```console
$ export DOCKER_CONTENT_TRUST_REPOSITORY_PASSPHRASE="mypassphrase123"

$ docker trust key load delegation.key --name jeff
Loading key from "delegation.key"...
Successfully imported key from delegation.key
```

## 添加委托公钥 Add a delegation public key

If you initialize a repository at the same time as adding a delegation public key, then you will need to use the local Notary Canonical Root Key's passphrase to create the repositories trust data. If the repository has already been initiated then you only need the repositories passphrase.

​	如果在添加委托公钥的同时初始化仓库，则需要使用本地 Notary Canonical Root Key 的密码来创建仓库的信任数据。如果仓库已被初始化，则只需要仓库的密码。



```console
# Export the Local Root Key Passphrase if required.
$ export DOCKER_CONTENT_TRUST_ROOT_PASSPHRASE="rootpassphrase123"

# Export the Repository Passphrase
$ export DOCKER_CONTENT_TRUST_REPOSITORY_PASSPHRASE="repopassphrase123"

# Initialise Repo and Push Delegation
$ docker trust signer add --key delegation.crt jeff registry.example.com/admin/demo
Adding signer "jeff" to registry.example.com/admin/demo...
Initializing signed repository for registry.example.com/admin/demo...
Successfully initialized "registry.example.com/admin/demo"
Successfully added signer: registry.example.com/admin/demo
```

## 签署镜像 Sign an image

Finally when signing an image, we will need to export the passphrase of the signing key. This was created when the key was loaded into the local Docker trust store with `$ docker trust key load`.

​	最后，当签署镜像时，我们需要导出签名密钥的密码。这是在密钥加载到本地 Docker 信任存储时创建的，使用 `$ docker trust key load`。

```console
$ export DOCKER_CONTENT_TRUST_REPOSITORY_PASSPHRASE="mypassphrase123"

$ docker trust sign registry.example.com/admin/demo:1
Signing and pushing trust data for local image registry.example.com/admin/demo:1, may overwrite remote trust data
The push refers to repository [registry.example.com/admin/demo]
428c97da766c: Layer already exists
2: digest: sha256:1a6fd470b9ce10849be79e99529a88371dff60c60aab424c077007f6979b4812 size: 524
Signing and pushing trust metadata
Successfully signed registry.example.com/admin/demo:1
```

## 使用内容信任构建 Build with content trust

You can also build with content trust. Before running the `docker build` command, you should set the environment variable `DOCKER_CONTENT_TRUST` either manually or in a scripted fashion. Consider the simple Dockerfile below.

​	您还可以使用内容信任进行构建。在运行 `docker build` 命令之前，应手动或通过脚本设置环境变量 `DOCKER_CONTENT_TRUST`。以下是一个简单的 Dockerfile 示例。



```dockerfile
# syntax=docker/dockerfile:1
FROM docker/trusttest:latest
RUN echo
```

The `FROM` tag is pulling a signed image. You cannot build an image that has a `FROM` that is not either present locally or signed. Given that content trust data exists for the tag `latest`, the following build should succeed:

​	`FROM` 标签拉取的是已签名的镜像。如果标签不是本地存在或已签名的，您无法构建此镜像。假设 `latest` 标签有内容信任数据，以下构建将成功：



```console
$  docker build -t docker/trusttest:testing .
Using default tag: latest
latest: Pulling from docker/trusttest

b3dbab3810fc: Pull complete
a9539b34a6ab: Pull complete
Digest: sha256:d149ab53f871
```

If content trust is enabled, building from a Dockerfile that relies on tag without trust data, causes the build command to fail:

​	如果启用了内容信任，从 Dockerfile 构建依赖于没有信任数据的标签，将导致构建命令失败：



```console
$  docker build -t docker/trusttest:testing .
unable to process Dockerfile: No trust data for notrust
```

## 相关信息 Related information

- [Delegations for content trust 内容信任的委托]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker/Delegationsforcontenttrust" >}})
- [Content trust in Docker Docker 中的内容信任]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker" >}})
- [Manage keys for content trust 管理内容信任的密钥]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker/Managekeysforcontenttrust" >}})
- [Play in a content trust sandbox 在内容信任沙箱中体验]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker/Playinacontenttrustsandbox" >}})
