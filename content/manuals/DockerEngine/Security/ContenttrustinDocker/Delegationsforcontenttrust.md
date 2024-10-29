+++
title = "内容信任的委托"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/security/trust/trust_delegation/](https://docs.docker.com/engine/security/trust/trust_delegation/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Delegations for content trust - 内容信任的委托

Delegations in Docker Content Trust (DCT) allow you to control who can and cannot sign an image tag. A delegation will have a pair of private and public delegation keys. A delegation could contain multiple pairs of keys and contributors in order to a) allow multiple users to be part of a delegation, and b) to support key rotation.

​	在 Docker 内容信任 (DCT) 中，委托允许您控制谁可以或不可以签署镜像标签。委托拥有一对私钥和公钥，并且可以包含多对密钥和多个贡献者，以便 a) 允许多个用户成为委托的一部分，b) 支持密钥轮换。

The most important delegation within Docker Content Trust is `targets/releases`. This is seen as the canonical source of a trusted image tag, and without a contributor's key being under this delegation, they will be unable to sign a tag.

​	在 Docker 内容信任中，最重要的委托是 `targets/releases`，它被视为受信任镜像标签的规范来源。如果贡献者的密钥不在此委托下，他们将无法签署标签。

Fortunately when using the `$ docker trust` commands, we will automatically initialize a repository, manage the repository keys, and add a collaborator's key to the `targets/releases` delegation via `docker trust signer add`.

​	幸运的是，当使用 `$ docker trust` 命令时，我们会自动初始化仓库、管理仓库密钥，并通过 `docker trust signer add` 将协作者的密钥添加到 `targets/releases` 委托中。

## 配置 Docker 客户端 Configuring the Docker client

By default, the `$ docker trust` commands expect the notary server URL to be the same as the registry URL specified in the image tag (following a similar logic to `$ docker push`). When using Docker Hub or DTR, the notary server URL is the same as the registry URL. However, for self-hosted environments or 3rd party registries, you will need to specify an alternative URL for the notary server. This is done with:

​	默认情况下，`$ docker trust` 命令假定 Notary 服务器 URL 与镜像标签中指定的注册表 URL 相同（与 `$ docker push` 的逻辑类似）。在使用 Docker Hub 或 DTR 时，Notary 服务器 URL 与注册表 URL 相同。然而，对于自托管环境或第三方注册表，您需要指定 Notary 服务器的替代 URL。这可以通过以下方式完成：



```console
$ export DOCKER_CONTENT_TRUST_SERVER=https://URL:PORT
```

If you do not export this variable in self-hosted environments, you may see errors such as:

​	如果在自托管环境中未导出此变量，您可能会看到以下错误：



```console
$ docker trust signer add --key cert.pem jeff registry.example.com/admin/demo
Adding signer "jeff" to registry.example.com/admin/demo...
<...>
Error: trust data missing for remote repository registry.example.com/admin/demo or remote repository not found: timestamp key trust data unavailable.  Has a notary repository been initialized?

$ docker trust inspect registry.example.com/admin/demo --pretty
WARN[0000] Error while downloading remote metadata, using cached timestamp - this might not be the latest version available remotely
<...>
```

If you have enabled authentication for your notary server, or are using DTR, you will need to log in before you can push data to the notary server.

​	如果启用了 Notary 服务器的身份验证或使用 DTR，则需要在将数据推送到 Notary 服务器之前登录。



```console
$ docker login registry.example.com/user/repo
Username: admin
Password:

Login Succeeded

$ docker trust signer add --key cert.pem jeff registry.example.com/user/repo
Adding signer "jeff" to registry.example.com/user/repo...
Initializing signed repository for registry.example.com/user/repo...
Successfully initialized "registry.example.com/user/repo"
Successfully added signer: jeff to registry.example.com/user/repo
```

If you do not log in, you will see:

​	如果未登录，您将看到以下提示：



```console
$ docker trust signer add --key cert.pem jeff registry.example.com/user/repo
Adding signer "jeff" to registry.example.com/user/repo...
Initializing signed repository for registry.example.com/user/repo...
you are not authorized to perform this operation: server returned 401.

Failed to add signer to: registry.example.com/user/repo
```

## 配置 Notary 客户端 Configuring the Notary client

Some of the more advanced features of DCT require the Notary CLI. To install and configure the Notary CLI:

​	DCT 的某些高级功能需要 Notary CLI。要安装和配置 Notary CLI：

1. Download the [client](https://github.com/theupdateframework/notary/releases) and ensure that it is available on your path. 下载 [客户端](https://github.com/theupdateframework/notary/releases) 并确保其路径可用。
2. Create a configuration file at `~/.notary/config.json` with the following content: 在 `~/.notary/config.json` 中创建配置文件，内容如下：



```json
{
  "trust_dir" : "~/.docker/trust",
  "remote_server": {
    "url": "https://registry.example.com",
    "root_ca": "../.docker/ca.pem"
  }
}
```

The newly created configuration file contains information about the location of your local Docker trust data and the notary server URL.

​	新创建的配置文件包含有关本地 Docker 信任数据位置和 Notary 服务器 URL 的信息。

For more detailed information about how to use notary outside of the Docker Content Trust use cases, refer to the Notary CLI documentation [here](https://github.com/theupdateframework/notary/blob/master/docs/command_reference.md)

​	关于如何在 Docker 内容信任用例之外使用 Notary 的详细信息，请参考 Notary CLI 文档 [这里](https://github.com/theupdateframework/notary/blob/master/docs/command_reference.md)。

## 创建委托密钥 Creating delegation keys

A prerequisite to adding your first contributor is a pair of delegation keys. These keys can either be generated locally using `$ docker trust`, generated by a certificate authority.

​	添加第一个贡献者的前提条件是拥有一对委托密钥。这些密钥可以通过 `$ docker trust` 本地生成，也可以通过证书机构生成。

### 使用 Docker Trust 生成密钥 Using Docker Trust to generate keys

Docker trust has a built-in generator for a delegation key pair, `$ docker trust generate <name>`. Running this command will automatically load the delegation private key in to the local Docker trust store.

​	Docker trust 内置了一个委托密钥对生成器 `$ docker trust generate <name>`。运行此命令将自动将委托私钥加载到本地 Docker 信任存储中。



```console
$ docker trust key generate jeff

Generating key for jeff...
Enter passphrase for new jeff key with ID 9deed25: 
Repeat passphrase for new jeff key with ID 9deed25: 
Successfully generated and loaded private key. Corresponding public key available: /home/ubuntu/Documents/mytrustdir/jeff.pub
```

### 手动生成密钥 Manually generating keys

If you need to manually generate a private key (either RSA or ECDSA) and a x509 certificate containing the public key, you can use local tools like openssl or cfssl along with a local or company-wide Certificate Authority.

​	如果需要手动生成私钥（RSA 或 ECDSA）和包含公钥的 x509 证书，可以使用 openssl 或 cfssl 等工具以及本地或公司内部的证书机构。

Here is an example of how to generate a 2048-bit RSA portion key (all RSA keys must be at least 2048 bits):

​	以下是如何生成 2048 位 RSA 密钥的示例（所有 RSA 密钥必须至少为 2048 位）：



```console
$ openssl genrsa -out delegation.key 2048

Generating RSA private key, 2048 bit long modulus
....................................................+++
............+++
e is 65537 (0x10001)
```

They should keep `delegation.key` private because it is used to sign tags.

​	请保持 `delegation.key` 的私密性，因为它用于签署标签。

Then they need to generate an x509 certificate containing the public key, which is what you need from them. Here is the command to generate a CSR (certificate signing request):

​	接下来生成一个包含公钥的 x509 证书，这正是您所需要的。以下是生成 CSR（证书签名请求）的命令：



```console
$ openssl req -new -sha256 -key delegation.key -out delegation.csr
```

Then they can send it to whichever CA you trust to sign certificates, or they can self-sign the certificate (in this example, creating a certificate that is valid for 1 year

​	然后可以将其发送给您信任的 CA 进行签署，或者自行签署证书（在此示例中，创建一个有效期为 1 年的证书）：



```console
$ openssl x509 -req -sha256 -days 365 -in delegation.csr -signkey delegation.key -out delegation.crt
```

Then they need to give you `delegation.crt`, whether it is self-signed or signed by a CA.

​	然后，他们需要将 `delegation.crt` 提供给您，无论是自签名的还是 CA 签名的。

Finally you will need to add the private key into your local Docker trust store.

​	最后，您需要将私钥添加到本地 Docker 信任存储中。

```console
$ docker trust key load delegation.key --name jeff

Loading key from "delegation.key"...
Enter passphrase for new jeff key with ID 8ae710e: 
Repeat passphrase for new jeff key with ID 8ae710e: 
Successfully imported key from delegation.key
```

### 查看本地委托密钥 Viewing local delegation keys

To list the keys that have been imported in to the local Docker trust store we can use the Notary CLI.

​	要列出已导入到本地 Docker 信任存储中的密钥，我们可以使用 Notary CLI。

```console
$ notary key list

ROLE       GUN                          KEY ID                                                              LOCATION
----       ---                          ------                                                              --------
root                                    f6c6a4b00fefd8751f86194c7d87a3bede444540eb3378c4a11ce10852ab1f96    /home/ubuntu/.docker/trust/private
jeff                                    9deed251daa1aa6f9d5f9b752847647cf8d705da0763aa5467650d0987ed5306    /home/ubuntu/.docker/trust/private
```

## 在 Notary 服务器中管理委托 Managing delegations in a Notary Server

When the first delegation is added to the Notary Server using `$ docker trust`, we automatically initiate trust data for the repository. This includes creating the notary target and snapshots keys, and rotating the snapshot key to be managed by the notary server. More information on these keys can be found [here]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker/Managekeysforcontenttrust" >}})

​	使用 `$ docker trust` 向 Notary 服务器添加第一个委托时，我们会自动为仓库初始化信任数据。这包括创建 notary 目标和快照密钥，并将快照密钥旋转为由 notary 服务器管理。有关这些密钥的更多信息可以在[此处]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker/Managekeysforcontenttrust" >}})找到。

When initiating a repository, you will need the key and the passphrase of a local Notary Canonical Root Key. If you have not initiated a repository before, and therefore don't have a Notary root key, `$ docker trust` will create one for you.

​	当初始化一个仓库时，您需要本地 Notary Canonical Root Key 的密钥和密码。如果之前没有初始化仓库，因此没有 Notary 根密钥，`$ docker trust` 会为您创建一个。

> **Important**
>
> 
>
> Be sure to protect and back up your [Notary Canonical Root Key]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker/Managekeysforcontenttrust" >}}).
>
> ​	请务必保护和备份您的 [Notary Canonical Root Key]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker/Managekeysforcontenttrust" >}})。

### 初始化仓库 Initiating the repository

To upload the first key to a delegation, at the same time initiating a repository, you can use the `$ docker trust signer add` command. This will add the contributor's public key to the `targets/releases` delegation, and create a second `targets/<name>` delegation.

​	要上传委托的第一个密钥并同时初始化仓库，可以使用 `$ docker trust signer add` 命令。这样将把贡献者的公钥添加到 `targets/releases` 委托，并创建第二个 `targets/<name>` 委托。

For DCT the name of the second delegation, in the below example `jeff`, is there to help you keep track of the owner of the keys. In more advanced use cases of Notary additional delegations are used for hierarchy.

​	对于 DCT，第二个委托的名称（在以下示例中为 `jeff`）有助于您跟踪密钥的所有者。在 Notary 的高级用例中，额外的委托用于建立层次结构。

```console
$ docker trust signer add --key cert.pem jeff registry.example.com/admin/demo

Adding signer "jeff" to registry.example.com/admin/demo...
Initializing signed repository for registry.example.com/admin/demo...
Enter passphrase for root key with ID f6c6a4b: 
Enter passphrase for new repository key with ID b0014f8: 
Repeat passphrase for new repository key with ID b0014f8: 
Successfully initialized "registry.example.com/admin/demo"
Successfully added signer: jeff to registry.example.com/admin/demo
```

You can see which keys have been pushed to the Notary server for each repository with the `$ docker trust inspect` command.

​	您可以使用 `$ docker trust inspect` 命令查看为每个仓库推送到 Notary 服务器的密钥。

```console
$ docker trust inspect --pretty registry.example.com/admin/demo

No signatures for registry.example.com/admin/demo


List of signers and their keys for registry.example.com/admin/demo

SIGNER              KEYS
jeff                1091060d7bfd

Administrative keys for registry.example.com/admin/demo

  Repository Key:	b0014f8e4863df2d028095b74efcb05d872c3591de0af06652944e310d96598d
  Root Key:	64d147e59e44870311dd2d80b9f7840039115ef3dfa5008127d769a5f657a5d7
```

You could also use the Notary CLI to list delegations and keys. Here you can clearly see the keys were attached to `targets/releases` and `targets/jeff`.

​	您也可以使用 Notary CLI 列出委托和密钥。您可以清楚地看到这些密钥已附加到 `targets/releases` 和 `targets/jeff`。

```console
$ notary delegation list registry.example.com/admin/demo

ROLE                PATHS             KEY IDS                                                             THRESHOLD
----                -----             -------                                                             ---------
targets/jeff        "" <all paths>    1091060d7bfd938dfa5be703fa057974f9322a4faef6f580334f3d6df44c02d1    1
                                          
targets/releases    "" <all paths>    1091060d7bfd938dfa5be703fa057974f9322a4faef6f580334f3d6df44c02d1    1 
```

### 添加额外的签名者 Adding additional signers

Docker Trust allows you to configure multiple delegations per repository, allowing you to manage the lifecycle of delegations. When adding additional delegations with `$ docker trust` the collaborators key is once again added to the `targets/release` role.

​	Docker Trust 允许您为每个仓库配置多个委托，从而管理委托的生命周期。在使用 `$ docker trust` 添加额外的委托时，协作者的密钥会再次添加到 `targets/releases` 角色中。

> Note you will need the passphrase for the repository key; this would have been configured when you first initiated the repository.
>
> ​	注意：您需要仓库密钥的密码；这是您首次初始化仓库时配置的。



```console
$ docker trust signer add --key ben.pub ben registry.example.com/admin/demo

Adding signer "ben" to registry.example.com/admin/demo...
Enter passphrase for repository key with ID b0014f8: 
Successfully added signer: ben to registry.example.com/admin/demo
```

Check to prove that there are now 2 delegations (Signer).

​	检查以验证现在有两个委托（签名者）：

```console
$ docker trust inspect --pretty registry.example.com/admin/demo

No signatures for registry.example.com/admin/demo

List of signers and their keys for registry.example.com/admin/demo

SIGNER              KEYS
ben                 afa404703b25
jeff                1091060d7bfd

Administrative keys for registry.example.com/admin/demo

  Repository Key:	b0014f8e4863df2d028095b74efcb05d872c3591de0af06652944e310d96598d
  Root Key:	64d147e59e44870311dd2d80b9f7840039115ef3dfa5008127d769a5f657a5d7
```

### 向现有委托添加密钥 Adding keys to an existing delegation

To support things like key rotation and expiring / retiring keys you can publish multiple contributor keys per delegation. The only prerequisite here is to make sure you use the same the delegation name, in this case `jeff`. Docker trust will automatically handle adding this new key to `targets/releases`.

​	为支持密钥轮换和密钥过期/退役，您可以为每个委托发布多个贡献者密钥。唯一的前提是确保使用相同的委托名称，在此例中为 `jeff`。Docker Trust 将自动处理将新密钥添加到 `targets/releases`。

> **Note**
>
> 
>
> You will need the passphrase for the repository key; this would have been configured when you first initiated the repository.
>
> ​	您需要仓库密钥的密码；这是您首次初始化仓库时配置的。



```console
$ docker trust signer add --key cert2.pem jeff registry.example.com/admin/demo

Adding signer "jeff" to registry.example.com/admin/demo...
Enter passphrase for repository key with ID b0014f8: 
Successfully added signer: jeff to registry.example.com/admin/demo
```

Check to prove that the delegation (Signer) now contains multiple Key IDs.

​	检查以验证委托（签名者）现在包含多个密钥 ID：



```console
$ docker trust inspect --pretty registry.example.com/admin/demo

No signatures for registry.example.com/admin/demo


List of signers and their keys for registry.example.com/admin/demo

SIGNER              KEYS
jeff                1091060d7bfd, 5570b88df073

Administrative keys for registry.example.com/admin/demo

  Repository Key:	b0014f8e4863df2d028095b74efcb05d872c3591de0af06652944e310d96598d
  Root Key:	64d147e59e44870311dd2d80b9f7840039115ef3dfa5008127d769a5f657a5d7
```

### 移除委托 Removing a delegation

If you need to remove a delegation, including the contributor keys that are attached to the `targets/releases` role, you can use the `$ docker trust signer remove` command.

​	如果需要移除委托（包括附加到 `targets/releases` 角色的贡献者密钥），可以使用 `$ docker trust signer remove` 命令。

> **Note**
>
> 
>
> Tags that were signed by the removed delegation will need to be resigned by an active delegation
>
> ​	被移除委托签署的标签需要由一个有效的委托重新签署。



```console
$ docker trust signer remove ben registry.example.com/admin/demo
Removing signer "ben" from registry.example.com/admin/demo...
Enter passphrase for repository key with ID b0014f8: 
Successfully removed ben from registry.example.com/admin/demo
```

#### 故障排除 Troubleshooting

1. If you see an error that there are no usable keys in `targets/releases`, you will need to add additional delegations using `docker trust signer add` before resigning images.

   如果您看到提示 `targets/releases` 中没有可用密钥的错误信息，您需要使用 `docker trust signer add` 添加额外的委托，然后重新签署镜像。

   ```text
   WARN[0000] role targets/releases has fewer keys than its threshold of 1; it will not be usable until keys are added to it
   ```

2. If you have added additional delegations already and are seeing an error message that there are no valid signatures in `targest/releases`, you will need to resign the `targets/releases` delegation file with the Notary CLI.

   如果您已经添加了额外的委托但仍然看到 `targets/releases` 中没有有效签名的错误信息，您需要使用 Notary CLI 重新签署 `targets/releases` 委托文件。

   ```text
   WARN[0000] Error getting targets/releases: valid signatures did not meet threshold for targets/releases 
   ```

   Resigning the delegation file is done with the `$ notary witness` command

   使用 `$ notary witness` 命令重新签署委托文件：

   ```console
   $ notary witness registry.example.com/admin/demo targets/releases --publish
   ```

   More information on the `$ notary witness` command can be found [here](https://github.com/theupdateframework/notary/blob/master/docs/advanced_usage.md#recovering-a-delegation)
   
   ​	有关 `$ notary witness` 命令的更多信息，请参阅 [这里](https://github.com/theupdateframework/notary/blob/master/docs/advanced_usage.md#recovering-a-delegation)。

### 从委托中移除贡献者的密钥 Removing a contributor's key from a delegation

As part of rotating keys for a delegation, you may want to remove an individual key but retain the delegation. This can be done with the Notary CLI.

​	在轮换委托密钥的过程中，您可能想要移除某个特定密钥而保留委托。可以使用 Notary CLI 完成此操作。

Remember you will have to remove the key from both the `targets/releases` role and the role specific to that signer `targets/<name>`.

​	记住，您需要从 `targets/releases` 角色和特定签名者的角色 `targets/<name>` 中删除密钥。

1. We will need to grab the Key ID from the Notary Server

   从 Notary 服务器获取密钥 ID：

   ```console
   $ notary delegation list registry.example.com/admin/demo
   
   ROLE                PATHS             KEY IDS                                                             THRESHOLD
   ----                -----             -------                                                             ---------
   targets/jeff        "" <all paths>    8fb597cbaf196f0781628b2f52bff6b3912e4e8075720378fda60d17232bbcf9    1
                                         1091060d7bfd938dfa5be703fa057974f9322a4faef6f580334f3d6df44c02d1    
   targets/releases    "" <all paths>    8fb597cbaf196f0781628b2f52bff6b3912e4e8075720378fda60d17232bbcf9    1
                                         1091060d7bfd938dfa5be703fa057974f9322a4faef6f580334f3d6df44c02d1    
   ```

2. Remove from the `targets/releases` delegation

   从 `targets/releases` 委托中移除：

   ```console
   $ notary delegation remove registry.example.com/admin/demo targets/releases 1091060d7bfd938dfa5be703fa057974f9322a4faef6f580334f3d6df44c02d1 --publish
   
   Auto-publishing changes to registry.example.com/admin/demo
   Enter username: admin
   Enter password: 
   Enter passphrase for targets key with ID b0014f8: 
   Successfully published changes for repository registry.example.com/admin/demo
   ```

3. Remove from the `targets/<name>` delegation

   从 `targets/<name>` 委托中移除：

   ```console
   $ notary delegation remove registry.example.com/admin/demo targets/jeff 1091060d7bfd938dfa5be703fa057974f9322a4faef6f580334f3d6df44c02d1 --publish
   
   Removal of delegation role targets/jeff with keys [5570b88df0736c468493247a07e235e35cf3641270c944d0e9e8899922fc6f99], to repository "registry.example.com/admin/demo" staged for next publish.
   
   Auto-publishing changes to registry.example.com/admin/demo
   Enter username: admin    
   Enter password: 
   Enter passphrase for targets key with ID b0014f8: 
   Successfully published changes for repository registry.example.com/admin/demo
   ```

4. Check the remaining delegation list

   检查剩余的委托列表：

   ```console
   $ notary delegation list registry.example.com/admin/demo
   
   ROLE                PATHS             KEY IDS                                                             THRESHOLD
   ----                -----             -------                                                             ---------
   targets/jeff        "" <all paths>    8fb597cbaf196f0781628b2f52bff6b3912e4e8075720378fda60d17232bbcf9    1    
   targets/releases    "" <all paths>    8fb597cbaf196f0781628b2f52bff6b3912e4e8075720378fda60d17232bbcf9    1    
   ```

### 移除本地委托私钥 Removing a local delegation private key

As part of rotating delegation keys, you may need to remove a local delegation key from the local Docker trust store. This is done with the Notary CLI, using the `$ notary key remove` command.

​	在轮换委托密钥的过程中，您可能需要从本地 Docker 信任存储中移除本地委托密钥。可以使用 Notary CLI 的 `$ notary key remove` 命令完成此操作。

1. We will need to get the Key ID from the local Docker Trust store

   从本地 Docker 信任存储中获取密钥 ID：

   ```console
   $ notary key list
   
   ROLE       GUN                          KEY ID                                                              LOCATION
   ----       ---                          ------                                                              --------
   root                                    f6c6a4b00fefd8751f86194c7d87a3bede444540eb3378c4a11ce10852ab1f96    /home/ubuntu/.docker/trust/private
   admin                                   8fb597cbaf196f0781628b2f52bff6b3912e4e8075720378fda60d17232bbcf9    /home/ubuntu/.docker/trust/private
   jeff                                    1091060d7bfd938dfa5be703fa057974f9322a4faef6f580334f3d6df44c02d1    /home/ubuntu/.docker/trust/private
   targets    ...example.com/admin/demo    c819f2eda8fba2810ec6a7f95f051c90276c87fddfc3039058856fad061c009d    /home/ubuntu/.docker/trust/private
   ```

2. Remove the key from the local Docker Trust store

   从本地 Docker 信任存储中删除密钥：

   ```console
   $ notary key remove 1091060d7bfd938dfa5be703fa057974f9322a4faef6f580334f3d6df44c02d1
   
   Are you sure you want to remove 1091060d7bfd938dfa5be703fa057974f9322a4faef6f580334f3d6df44c02d1 (role jeff) from /home/ubuntu/.docker/trust/private?  (yes/no)  y
   
   Deleted 1091060d7bfd938dfa5be703fa057974f9322a4faef6f580334f3d6df44c02d1 (role jeff) from /home/ubuntu/.docker/trust/private.
   ```

## 从仓库中移除所有信任数据 Removing all trust data from a repository

You can remove all trust data from a repository, including repository, target, snapshot and all delegations keys using the Notary CLI.

​	您可以使用 Notary CLI 从仓库中移除所有信任数据，包括仓库、目标、快照和所有委托密钥。

This is often required by a container registry before a particular repository can be deleted.

​	此操作通常是在删除特定仓库前所需的操作。

```console
$ notary delete registry.example.com/admin/demo --remote

Deleting trust data for repository registry.example.com/admin/demo
Enter username: admin
Enter password: 
Successfully deleted local and remote trust data for repository registry.example.com/admin/demo

$ docker trust inspect --pretty registry.example.com/admin/demo

No signatures or cannot access registry.example.com/admin/demo
```

## 相关信息 Related information

- [Content trust in Docker Docker 中的内容信任]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker" >}})
- [Manage keys for content trust 管理内容信任的密钥]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker/Managekeysforcontenttrust" >}})
- [Automation with content trust 内容信任的自动化]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker/Automationwithcontenttrust" >}})
- [Play in a content trust sandbox 在内容信任沙盒中操作]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker/Playinacontenttrustsandbox" >}})
