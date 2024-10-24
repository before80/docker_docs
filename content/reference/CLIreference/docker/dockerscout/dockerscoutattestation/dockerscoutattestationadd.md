+++
title = "docker scout attestation add"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/scout/attestation/add/](https://docs.docker.com/reference/cli/docker/scout/attestation/add/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker scout attestation add

| Description | Add attestation to image                                |
| :---------- | ------------------------------------------------------- |
| Usage       | `docker scout attestation add OPTIONS IMAGE [IMAGE...]` |
| Aliases     | `docker scout attest add`                               |

**Experimental**

**This command is experimental.**

Experimental features are intended for testing and feedback as their functionality or design may change between releases without warning or can be removed entirely in a future release.

## Description

The docker scout attestation add command adds attestations to images.

## Options

| Option             | Default | Description                             |
| ------------------ | ------- | --------------------------------------- |
| `--file`           |         | File location of attestations to attach |
| `--predicate-type` |         | Predicate-type for attestations         |
