+++
title = "docker trust signer remove"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/trust/signer/remove/](https://docs.docker.com/reference/cli/docker/trust/signer/remove/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker trust signer remove

| Description | Remove a signer                                              |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker trust signer remove [OPTIONS] NAME REPOSITORY [REPOSITORY...]` |

## Description

`docker trust signer remove` removes signers from signed repositories.

​	`docker trust signer remove` 从已签名的仓库中移除签名者。

## Options

| Option        | Default | Description                                                  |
| ------------- | ------- | ------------------------------------------------------------ |
| `-f, --force` |         | 在移除最近的签名者之前，不提示确认 Do not prompt for confirmation before removing the most recent signer |

## Examples

### 从仓库中移除签名者 Remove a signer from a repository

To remove an existing signer, `alice`, from this repository:

​	要从该仓库中移除现有的签名者 `alice`：

```console
$ docker trust inspect --pretty example/trust-demo

No signatures for example/trust-demo


List of signers and their keys:

SIGNER              KEYS
alice               05e87edcaecb
bob                 5600f5ab76a2

Administrative keys for example/trust-demo:
Repository Key: ecc457614c9fc399da523a5f4e24fe306a0a6ee1cc79a10e4555b3c6ab02f71e
Root Key:       3cb2228f6561e58f46dbc4cda4fcaff9d5ef22e865a94636f82450d1d2234949
```

Remove `alice` with `docker trust signer remove`:

​	使用 `docker trust signer remove` 移除 `alice`：



```console
$ docker trust signer remove alice example/trust-demo

Removing signer "alice" from image example/trust-demo...
Enter passphrase for repository key with ID 642692c:
Successfully removed alice from example/trust-demo
```

`docker trust inspect --pretty` now doesn't list `alice` as a valid signer:

​	`docker trust inspect --pretty` 现在不再列出 `alice` 作为有效的签名者：



```console
$ docker trust inspect --pretty example/trust-demo

No signatures for example/trust-demo


List of signers and their keys:

SIGNER              KEYS
bob                 5600f5ab76a2

Administrative keys for example/trust-demo:
Repository Key: ecc457614c9fc399da523a5f4e24fe306a0a6ee1cc79a10e4555b3c6ab02f71e
Root Key:       3cb2228f6561e58f46dbc4cda4fcaff9d5ef22e865a94636f82450d1d2234949
```

### 从多个仓库中移除签名者 Remove a signer from multiple repositories

To remove an existing signer, `alice`, from multiple repositories:

​	要从多个仓库中移除现有的签名者 `alice`：



```console
$ docker trust inspect --pretty example/trust-demo

SIGNED TAG          DIGEST                                                             SIGNERS
v1                  74d4bfa917d55d53c7df3d2ab20a8d926874d61c3da5ef6de15dd2654fc467c4   alice, bob

List of signers and their keys:

SIGNER              KEYS
alice               05e87edcaecb
bob                 5600f5ab76a2

Administrative keys for example/trust-demo:
Repository Key: 95b9e5514c9fc399da523a5f4e24fe306a0a6ee1cc79a10e4555b3c6ab02f71e
Root Key:       3cb2228f6561e58f46dbc4cda4fcaff9d5ef22e865a94636f82450d1d2234949
```



```console
$ docker trust inspect --pretty example/trust-demo2

SIGNED TAG          DIGEST                                                             SIGNERS
v1                  74d4bfa917d55d53c7df3d2ab20a8d926874d61c3da5ef6de15dd2654fc467c4   alice, bob

List of signers and their keys:

SIGNER              KEYS
alice               05e87edcaecb
bob                 5600f5ab76a2

Administrative keys for example/trust-demo2:
Repository Key: ece554f14c9fc399da523a5f4e24fe306a0a6ee1cc79a10e4553d2ab20a8d9268
Root Key:       3cb2228f6561e58f46dbc4cda4fcaff9d5ef22e865a94636f82450d1d2234949
```

Remove `alice` from both images with a single `docker trust signer remove` command:

​	使用单个 `docker trust signer remove` 命令从两个镜像中移除 `alice`：



```console
$ docker trust signer remove alice example/trust-demo example/trust-demo2

Removing signer "alice" from image example/trust-demo...
Enter passphrase for repository key with ID 95b9e55:
Successfully removed alice from example/trust-demo

Removing signer "alice" from image example/trust-demo2...
Enter passphrase for repository key with ID ece554f:
Successfully removed alice from example/trust-demo2
```

Run `docker trust inspect --pretty` to confirm that `alice` is no longer listed as a valid signer of either `example/trust-demo` or `example/trust-demo2`:

​	运行 `docker trust inspect --pretty` 确认 `alice` 不再被列为 `example/trust-demo` 或 `example/trust-demo2` 的有效签名者：



```console
$ docker trust inspect --pretty example/trust-demo

SIGNED TAG          DIGEST                                                             SIGNERS
v1                  74d4bfa917d55d53c7df3d2ab20a8d926874d61c3da5ef6de15dd2654fc467c4   bob

List of signers and their keys:

SIGNER              KEYS
bob                 5600f5ab76a2

Administrative keys for example/trust-demo:
Repository Key: ecc457614c9fc399da523a5f4e24fe306a0a6ee1cc79a10e4555b3c6ab02f71e
Root Key:       3cb2228f6561e58f46dbc4cda4fcaff9d5ef22e865a94636f82450d1d2234949
```



```console
$ docker trust inspect --pretty example/trust-demo2

SIGNED TAG          DIGEST                                                             SIGNERS
v1                  74d4bfa917d55d53c7df3d2ab20a8d926874d61c3da5ef6de15dd2654fc467c4   bob

List of signers and their keys:

SIGNER              KEYS
bob                 5600f5ab76a2

Administrative keys for example/trust-demo2:
Repository Key: ece554f14c9fc399da523a5f4e24fe306a0a6ee1cc79a10e4553d2ab20a8d9268
Root Key:       3cb2228f6561e58f46dbc4cda4fcaff9d5ef22e865a94636f82450d1d2234949
```

`docker trust signer remove` removes signers to repositories on a best effort basis. It continues to remove the signer from subsequent repositories if one attempt fails:

​	`docker trust signer remove` 会尽最大努力从仓库中移除签名者。如果一次尝试失败，它会继续从后续的仓库中移除该签名者。



```console
$ docker trust signer remove alice example/unauthorized example/authorized

Removing signer "alice" from image example/unauthorized...
No signer alice for image example/unauthorized

Removing signer "alice" from image example/authorized...
Enter passphrase for repository key with ID c6772a0:
Successfully removed alice from example/authorized

Error removing signer from: example/unauthorized
```
