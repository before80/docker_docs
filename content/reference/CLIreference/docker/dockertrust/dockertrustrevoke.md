+++
title = "docker trust revoke"
date = 2024-10-23T14:54:43+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/trust/revoke/](https://docs.docker.com/reference/cli/docker/trust/revoke/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker trust revoke

| Description | Remove trust for an image                   |
| :---------- | ------------------------------------------- |
| Usage       | `docker trust revoke [OPTIONS] IMAGE[:TAG]` |

## Description

`docker trust revoke` removes signatures from tags in signed repositories.

​	`docker trust revoke` 从已签名的仓库中删除标签签名。

## Options

| Option      | Default | Description                               |
| ----------- | ------- | ----------------------------------------- |
| `-y, --yes` |         | 不提示确认 Do not prompt for confirmation |

## Examples

### 从已签名标签中撤销签名 Revoke signatures from a signed tag

Here's an example of a repository with two signed tags:

​	以下是包含两个已签名标签的仓库示例：



```console
$ docker trust inspect --pretty example/trust-demo
SIGNED TAG          DIGEST                                                              SIGNERS
red                 852cc04935f930a857b630edc4ed6131e91b22073bcc216698842e44f64d2943    alice
blue                f1c38dbaeeb473c36716f6494d803fbfbe9d8a76916f7c0093f227821e378197    alice, bob

List of signers and their keys for example/trust-demo:

SIGNER              KEYS
alice               05e87edcaecb
bob                 5600f5ab76a2

Administrative keys for example/trust-demo:
Repository Key: ecc457614c9fc399da523a5f4e24fe306a0a6ee1cc79a10e4555b3c6ab02f71e
Root Key:       3cb2228f6561e58f46dbc4cda4fcaff9d5ef22e865a94636f82450d1d2234949
```

When `alice`, one of the signers, runs `docker trust revoke`:

​	当签名者之一 `alice` 执行 `docker trust revoke` 命令时：

​	



```console
$ docker trust revoke example/trust-demo:red
Enter passphrase for delegation key with ID 27d42a8:
Successfully deleted signature for example/trust-demo:red
```

After revocation, the tag is removed from the list of released tags:

​	撤销签名后，标签将从已发布标签列表中移除：





```console
$ docker trust inspect --pretty example/trust-demo
SIGNED TAG          DIGEST                                                              SIGNERS
blue                f1c38dbaeeb473c36716f6494d803fbfbe9d8a76916f7c0093f227821e378197    alice, bob

List of signers and their keys for example/trust-demo:

SIGNER              KEYS
alice               05e87edcaecb
bob                 5600f5ab76a2

Administrative keys for example/trust-demo:
Repository Key: ecc457614c9fc399da523a5f4e24fe306a0a6ee1cc79a10e4555b3c6ab02f71e
Root Key:       3cb2228f6561e58f46dbc4cda4fcaff9d5ef22e865a94636f82450d1d2234949
```

### 撤销仓库中所有标签的签名 Revoke signatures on all tags in a repository

When no tag is specified, `docker trust` revokes all signatures that you have a signing key for.

​	如果没有指定标签，`docker trust` 会撤销所有有签名密钥的签名：



```console
$ docker trust inspect --pretty example/trust-demo
SIGNED TAG          DIGEST                                                              SIGNERS
red                 852cc04935f930a857b630edc4ed6131e91b22073bcc216698842e44f64d2943    alice
blue                f1c38dbaeeb473c36716f6494d803fbfbe9d8a76916f7c0093f227821e378197    alice, bob

List of signers and their keys for example/trust-demo:

SIGNER              KEYS
alice               05e87edcaecb
bob                 5600f5ab76a2

Administrative keys for example/trust-demo:
Repository Key: ecc457614c9fc399da523a5f4e24fe306a0a6ee1cc79a10e4555b3c6ab02f71e
Root Key:       3cb2228f6561e58f46dbc4cda4fcaff9d5ef22e865a94636f82450d1d2234949
```

When `alice`, one of the signers, runs `docker trust revoke`:

​	当签名者之一 `alice` 执行 `docker trust revoke` 命令时：

```console
$ docker trust revoke example/trust-demo
Confirm you would like to delete all signature data for example/trust-demo? [y/N] y
Enter passphrase for delegation key with ID 27d42a8:
Successfully deleted signature for example/trust-demo
```

All tags that have `alice`'s signature on them are removed from the list of released tags:

​	所有包含 `alice` 签名的标签将从已发布标签列表中移除：

```console
$ docker trust inspect --pretty example/trust-demo

No signatures for example/trust-demo


List of signers and their keys for example/trust-demo:

SIGNER              KEYS
alice               05e87edcaecb
bob                 5600f5ab76a2

Administrative keys for example/trust-demo:
Repository Key: ecc457614c9fc399da523a5f4e24fe306a0a6ee1cc79a10e4555b3c6ab02f71e
Root Key:       3cb2228f6561e58f46dbc4cda4fcaff9d5ef22e865a94636f82450d1d2234949
```
