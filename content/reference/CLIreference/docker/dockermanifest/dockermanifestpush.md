+++
title = "docker manifest push"
date = 2024-10-23T14:54:43+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/manifest/push/](https://docs.docker.com/reference/cli/docker/manifest/push/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker manifest push

| Description | Push a manifest list to a repository           |
| :---------- | ---------------------------------------------- |
| Usage       | `docker manifest push [OPTIONS] MANIFEST_LIST` |

**Experimental**

**This command is experimental.**

Experimental features are intended for testing and feedback as their functionality or design may change between releases without warning or can be removed entirely in a future release.

## Description

Push a manifest list to a repository

## Options

| Option        | Default | Description                               |
| ------------- | ------- | ----------------------------------------- |
| `--insecure`  |         | Allow push to an insecure registry        |
| `-p, --purge` |         | Remove the local manifest list after push |
