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

## Options

| Option  | Default | Description                                                 |
| ------- | ------- | ----------------------------------------------------------- |
| `--dir` |         | Directory to generate key in, defaults to current directory |

## Examples

### Generate a key-pair



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

The public key component `alice.pub` will be available in the current working directory, and can be used directly by `docker trust signer add`.

Provide the `--dir` argument to specify a directory to generate the key in:



```console
$ docker trust key generate alice --dir /foo

Generating key for alice...
Enter passphrase for new alice key with ID 17acf3c:
Repeat passphrase for new alice key with ID 17acf3c:
Successfully generated and loaded private key. Corresponding public key available: alice.pub
$ ls /foo
alice.pub
```
