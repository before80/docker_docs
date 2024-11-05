+++
title = "docker trust key generate"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/trust/key/generate/](https://docs.docker.com/reference/cli/docker/trust/key/generate/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker trust key generate

| Description | Generate and load a signing key-pair |
| :---------- | ------------------------------------ |
| Usage       | `docker trust key generate NAME`     |

## Description

`docker trust key generate` generates a key-pair to be used with signing, and loads the private key into the local Docker trust keystore.

​	`docker trust key generate` 生成一个密钥对，用于签名，并将私钥加载到本地 Docker 信任密钥库中。

## Options

| Option  | Default | Description                                                  |
| ------- | ------- | ------------------------------------------------------------ |
| `--dir` |         | 生成密钥的目录，默认为当前目录 Directory to generate key in, defaults to current directory |

## Examples

### 生成密钥对 Generate a key-pair



```console
$ docker trust key generate alice

Generating key for alice...
Enter passphrase for new alice key with ID 17acf3c:
Repeat passphrase for new alice key with ID 17acf3c:
Successfully generated and loaded private key. Corresponding public key available: alice.pub
$ ls
alice.pub
```

The private signing key is encrypted by the passphrase and loaded into the Docker trust keystore. All passphrase requests to sign with the key will be referred to by the provided `NAME`.

​	私钥通过密码短语加密，并加载到 Docker 信任密钥库中。所有使用该密钥签名时的密码短语请求都会通过提供的 `NAME` 进行引用。

The public key component `alice.pub` will be available in the current working directory, and can be used directly by `docker trust signer add`.

​	公钥组件 `alice.pub` 将保存在当前工作目录中，可以通过 `docker trust signer add` 直接使用。

Provide the `--dir` argument to specify a directory to generate the key in:

​	提供 `--dir` 参数来指定生成密钥的目录：



```console
$ docker trust key generate alice --dir /foo

Generating key for alice...
Enter passphrase for new alice key with ID 17acf3c:
Repeat passphrase for new alice key with ID 17acf3c:
Successfully generated and loaded private key. Corresponding public key available: alice.pub
$ ls /foo
alice.pub
```
