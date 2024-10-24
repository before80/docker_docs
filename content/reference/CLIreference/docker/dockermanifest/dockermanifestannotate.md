+++
title = "docker manifest annotate"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/manifest/annotate/](https://docs.docker.com/reference/cli/docker/manifest/annotate/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker manifest annotate

| Description | Add additional information to a local image manifest        |
| :---------- | ----------------------------------------------------------- |
| Usage       | `docker manifest annotate [OPTIONS] MANIFEST_LIST MANIFEST` |

**Experimental**

**This command is experimental.**

Experimental features are intended for testing and feedback as their functionality or design may change between releases without warning or can be removed entirely in a future release.

## Description

Add additional information to a local image manifest

## Options

| Option          | Default | Description                  |
| --------------- | ------- | ---------------------------- |
| `--arch`        |         | Set architecture             |
| `--os`          |         | Set operating system         |
| `--os-features` |         | Set operating system feature |
| `--os-version`  |         | Set operating system version |
| `--variant`     |         | Set architecture variant     |
