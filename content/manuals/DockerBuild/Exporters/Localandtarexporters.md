+++
title = "Local and tar exporters"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/build/exporters/local-tar/](https://docs.docker.com/build/exporters/local-tar/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Local and tar exporters

The `local` and `tar` exporters output the root filesystem of the build result into a local directory. They're useful for producing artifacts that aren't container images.

- `local` exports files and directories.
- `tar` exports the same, but bundles the export into a tarball.

## Synopsis

Build a container image using the `local` exporter:



```console
$ docker buildx build --output type=local[,parameters] .
$ docker buildx build --output type=tar[,parameters] .
```

The following table describes the available parameters:

| Parameter | Type   | Default | Description           |
| --------- | ------ | ------- | --------------------- |
| `dest`    | String |         | Path to copy files to |

## Further reading

For more information on the `local` or `tar` exporters, see the [BuildKit README](https://github.com/moby/buildkit/blob/master/README.md#local-directory).
