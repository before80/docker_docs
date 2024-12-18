+++
title = "docker trust key load"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/trust/key/load/](https://docs.docker.com/reference/cli/docker/trust/key/load/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker trust key load

| Description | Load a private key file for signing       |
| :---------- | ----------------------------------------- |
| Usage       | `docker trust key load [OPTIONS] KEYFILE` |

## Description

`docker trust key load` adds private keys to the local Docker trust keystore.

​	`docker trust key load` 将私钥添加到本地 Docker 信任密钥库中。

To add a signer to a repository use `docker trust signer add`.

​	要将签名者添加到仓库中，请使用 `docker trust signer add`。

## Options

| Option   | Default  | Description                                  |
| -------- | -------- | -------------------------------------------- |
| `--name` | `signer` | 加载密钥时使用的名称 Name for the loaded key |

## Examples

### 加载单个私钥 Load a single private key

For a private key `alice.pem` with permissions `-rw-------`

​	对于权限为 `-rw-------` 的私钥 `alice.pem`：

```console
$ docker trust key load alice.pem

Loading key from "alice.pem"...
Enter passphrase for new signer key with ID f8097df:
Repeat passphrase for new signer key with ID f8097df:
Successfully imported key from alice.pem
```

To specify a name use the `--name` flag:

​	要指定名称，可以使用 `--name` 标志：

```console
$ docker trust key load --name alice-key alice.pem

Loading key from "alice.pem"...
Enter passphrase for new alice-key key with ID f8097df:
Repeat passphrase for new alice-key key with ID f8097df:
Successfully imported key from alice.pem
```
