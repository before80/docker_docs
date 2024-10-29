+++
title = "管理内容信任的密钥"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/engine/security/trust/trust_key_mng/](https://docs.docker.com/engine/security/trust/trust_key_mng/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Manage keys for content trust - 管理内容信任的密钥

Trust for an image tag is managed through the use of keys. Docker's content trust makes use of five different types of keys:

​	内容信任通过使用密钥管理镜像标签。Docker 的内容信任使用五种不同类型的密钥：

| Key        | Description                                                  |
| :--------- | :----------------------------------------------------------- |
| root key   | 镜像标签内容信任的根密钥。当启用内容信任时，root 密钥只需创建一次。也称为离线密钥，因为它应保存在离线环境中。Root of content trust for an image tag. When content trust is enabled, you create the root key once. Also known as the offline key, because it should be kept offline. |
| targets    | 此密钥允许您签署镜像标签，管理委托（包括委托的密钥或授权的委托路径）。也称为仓库密钥，因为此密钥决定了可以签署到镜像仓库中的标签。This key allows you to sign image tags, to manage delegations including delegated keys or permitted delegation paths. Also known as the repository key, since this key determines what tags can be signed into an image repository. |
| snapshot   | 此密钥对当前的镜像标签集合进行签名，防止混合和匹配攻击。This key signs the current collection of image tags, preventing mix and match attacks. |
| timestamp  | 该密钥允许 Docker 镜像仓库提供新鲜度安全保证，而无需客户端定期刷新内容。This key allows Docker image repositories to have freshness security guarantees without requiring periodic content refreshes on the client's side. |
| delegation | 委托密钥是可选的标签密钥，允许您将签署镜像标签的权限委托给其他发布者，而不需要共享您的 targets 密钥。Delegation keys are optional tagging keys and allow you to delegate signing image tags to other publishers without having to share your targets key. |

When doing a `docker push` with Content Trust enabled for the first time, the root, targets, snapshot, and timestamp keys are generated automatically for the image repository:

​	首次启用内容信任执行 `docker push` 时，root、targets、snapshot 和 timestamp 密钥会为镜像仓库自动生成：

- The root and targets key are generated and stored locally client-side.
  - root 和 targets 密钥会在客户端本地生成并存储。

- The timestamp and snapshot keys are safely generated and stored in a signing server that is deployed alongside the Docker registry. These keys are generated in a backend service that isn't directly exposed to the internet and are encrypted at rest. Use the Notary CLI to [manage your snapshot key locally](https://github.com/theupdateframework/notary/blob/master/docs/advanced_usage.md#rotate-keys).
  - timestamp 和 snapshot 密钥会安全地生成并存储在与 Docker registry 部署在一起的签名服务器中。这些密钥在不直接暴露于互联网的后台服务中生成，并在静态存储时加密。使用 Notary CLI [本地管理您的 snapshot 密钥](https://github.com/theupdateframework/notary/blob/master/docs/advanced_usage.md#rotate-keys)。


Delegation keys are optional, and not generated as part of the normal `docker` workflow. They need to be [manually generated and added to the repository](https://docs.docker.com/engine/security/trust/trust_delegation/#creating-delegation-keys).

​	委托密钥是可选的，不会在正常的 `docker` 工作流中生成。它们需要[手动生成并添加到仓库](https://docs.docker.com/engine/security/trust/trust_delegation/#creating-delegation-keys)。

## 选择一个密码 Choose a passphrase

The passphrases you chose for both the root key and your repository key should be randomly generated and stored in a password manager. Having the repository key allows users to sign image tags on a repository. Passphrases are used to encrypt your keys at rest and ensure that a lost laptop or an unintended backup doesn't put the private key material at risk.

​	您为 root 密钥和仓库密钥选择的密码应随机生成并存储在密码管理器中。拥有仓库密钥的用户可以在仓库上签署镜像标签。密码用于加密您的密钥，确保丢失的笔记本电脑或意外的备份不会让私钥材料面临风险。

## 备份您的密钥 Back up your keys

All the Docker trust keys are stored encrypted using the passphrase you provide on creation. Even so, you should still take care of the location where you back them up. Good practice is to create two encrypted USB keys.

​	所有 Docker 信任密钥在创建时都使用您提供的密码进行加密存储。即便如此，您仍然应谨慎备份它们的位置。良好的做法是创建两个加密的 USB 密钥备份。

> **Warning**
>
> 
>
> It is very important that you back up your keys to a safe, secure location. The loss of the repository key is recoverable, but the loss of the root key is not.
>
> ​	将密钥备份到安全可靠的位置非常重要。仓库密钥的丢失是可恢复的，但 root 密钥的丢失不可恢复。

The Docker client stores the keys in the `~/.docker/trust/private` directory. Before backing them up, you should `tar` them into an archive:

​	Docker 客户端将密钥存储在 `~/.docker/trust/private` 目录中。在备份之前，您可以将其打包成一个归档文件：



```console
$ umask 077; tar -zcvf private_keys_backup.tar.gz ~/.docker/trust/private; umask 022
```

## 硬件存储和签名 Hardware storage and signing

Docker Content Trust can store and sign with root keys from a Yubikey 4. The Yubikey is prioritized over keys stored in the filesystem. When you initialize a new repository with content trust, Docker Engine looks for a root key locally. If a key is not found and the Yubikey 4 exists, Docker Engine creates a root key in the Yubikey 4. Consult the [Notary documentation](https://github.com/theupdateframework/notary/blob/master/docs/advanced_usage.md#use-a-yubikey) for more details.

​	Docker 内容信任可以从 Yubikey 4 存储和签署 root 密钥。Yubikey 比文件系统中存储的密钥优先级更高。当您使用内容信任初始化新仓库时，Docker 引擎会在本地查找 root 密钥。如果没有找到密钥并且存在 Yubikey 4，Docker 引擎将在 Yubikey 4 中创建一个 root 密钥。有关详细信息，请参考 [Notary 文档](https://github.com/theupdateframework/notary/blob/master/docs/advanced_usage.md#use-a-yubikey)。

Prior to Docker Engine 1.11, this feature was only in the experimental branch.

​	在 Docker 引擎 1.11 之前，此功能仅在实验分支中可用。

## 密钥丢失 Key loss

> **Warning**
>
> 
>
> If a publisher loses keys it means losing the ability to sign images for the repositories in question. If you lose a key, send an email to [Docker Hub Support]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker/Managekeysforcontenttrust" >}}). As a reminder, the loss of a root key is not recoverable.
>
> ​	如果发布者丢失密钥，则意味着丧失对相关仓库中镜像的签名能力。如果您丢失密钥，请发送电子邮件至 [Docker Hub 支持]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker/Managekeysforcontenttrust" >}})。请注意，root 密钥的丢失是不可恢复的。

This loss also requires manual intervention from every consumer that used a signed tag from this repository prior to the loss.
Image consumers get the following error for content previously downloaded from the affected repo(s):

​	这还需要每个使用该仓库中的已签标签的用户手动干预。图像消费者会在尝试从受影响的仓库下载内容时看到以下警告：



```console
Warning: potential malicious behavior - trust data has insufficient signatures for remote repository docker.io/my/image: valid signatures did not meet threshold
```

To correct this, they need to download a new image tag that is signed with the new key.

​	为了解决此问题，用户需要下载使用新密钥签署的镜像标签。

## Related information

- [Content trust in Docker Docker 中的内容信任]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker" >}})
- [Automation with content trust 内容信任的自动化]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker/Automationwithcontenttrust" >}})
- [Delegations for content trust 内容信任的委托]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker/Delegationsforcontenttrust" >}})
- [Play in a content trust sandbox 在内容信任沙盒中操作]({{< ref "/manuals/DockerEngine/Security/ContenttrustinDocker/Playinacontenttrustsandbox" >}})
