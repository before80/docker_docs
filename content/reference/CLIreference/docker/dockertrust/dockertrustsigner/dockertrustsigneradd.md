+++
title = "docker trust signer add"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/trust/signer/add/](https://docs.docker.com/reference/cli/docker/trust/signer/add/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker trust signer add

| Description | Add a signer                                                 |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker trust signer add OPTIONS NAME REPOSITORY [REPOSITORY...]` |

## Description

`docker trust signer add` adds signers to signed repositories.

​	`docker trust signer add` 将签名者添加到已签名的仓库。

## Options

| Option  | Default | Description                                               |
| ------- | ------- | --------------------------------------------------------- |
| `--key` |         | 签名者公钥文件的路径 Path to the signer's public key file |

## Examples

###  向仓库添加签名者 Add a signer to a repository

To add a new signer, `alice`, to this repository:

​	要将新的签名者 `alice` 添加到该仓库：

```console
$ docker trust inspect --pretty example/trust-demo

No signatures for example/trust-demo


List of signers and their keys:

SIGNER              KEYS
bob                 5600f5ab76a2

Administrative keys for example/trust-demo:
Repository Key: 642692c14c9fc399da523a5f4e24fe306a0a6ee1cc79a10e4555b3c6ab02f71e
Root Key:       3cb2228f6561e58f46dbc4cda4fcaff9d5ef22e865a94636f82450d1d2234949
```

Add `alice` with `docker trust signer add`:

​	使用 `docker trust signer add` 添加 `alice`：

```console
$ docker trust signer add alice example/trust-demo --key alice.crt
  Adding signer "alice" to example/trust-demo...
  Enter passphrase for repository key with ID 642692c:
Successfully added signer: alice to example/trust-demo
```

`docker trust inspect --pretty` now lists `alice` as a valid signer:

​	`docker trust inspect --pretty` 现在列出 `alice` 作为有效的签名者：

```console
$ docker trust inspect --pretty example/trust-demo

No signatures for example/trust-demo


List of signers and their keys:

SIGNER              KEYS
alice               05e87edcaecb
bob                 5600f5ab76a2

Administrative keys for example/trust-demo:
Repository Key: 642692c14c9fc399da523a5f4e24fe306a0a6ee1cc79a10e4555b3c6ab02f71e
Root Key:       3cb2228f6561e58f46dbc4cda4fcaff9d5ef22e865a94636f82450d1d2234949
```
