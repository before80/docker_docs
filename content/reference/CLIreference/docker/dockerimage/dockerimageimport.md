+++
title = "docker image import"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/image/import/](https://docs.docker.com/reference/cli/docker/image/import/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker image import

| Description | Import the contents from a tarball to create a filesystem image |
| :---------- | ------------------------------------------------------------ |
| Usage       | `docker image import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]` |
| Aliases     | `docker import`                                              |

## Description

You can specify a `URL` or `-` (dash) to take data directly from `STDIN`. The `URL` can point to an archive (.tar, .tar.gz, .tgz, .bzip, .tar.xz, or .txz) containing a filesystem or to an individual file on the Docker host. If you specify an archive, Docker untars it in the container relative to the `/` (root). If you specify an individual file, you must specify the full path within the host. To import from a remote location, specify a `URI` that begins with the `http://` or `https://` protocol.

The `--change` option applies `Dockerfile` instructions to the image that is created. Supported `Dockerfile` instructions: `CMD`|`ENTRYPOINT`|`ENV`|`EXPOSE`|`ONBUILD`|`USER`|`VOLUME`|`WORKDIR`

## Options

| Option          | Default | Description                                                |
| --------------- | ------- | ---------------------------------------------------------- |
| `-c, --change`  |         | Apply Dockerfile instruction to the created image          |
| `-m, --message` |         | Set commit message for imported image                      |
| `--platform`    |         | API 1.32+ Set platform if server is multi-platform capable |

## Examples

### Import from a remote location

This creates a new untagged image.



```console
$ docker import https://example.com/exampleimage.tgz
```

### Import from a local file

Import to docker via pipe and `STDIN`.



```console
$ cat exampleimage.tgz | docker import - exampleimagelocal:new
```

Import with a commit message.



```console
$ cat exampleimage.tgz | docker import --message "New image imported from tarball" - exampleimagelocal:new
```

Import to docker from a local archive.



```console
$ docker import /path/to/exampleimage.tgz
```

### Import from a local directory



```console
$ sudo tar -c . | docker import - exampleimagedir
```

### Import from a local directory with new configurations



```console
$ sudo tar -c . | docker import --change "ENV DEBUG=true" - exampleimagedir
```

Note the `sudo` in this example – you must preserve the ownership of the files (especially root ownership) during the archiving with tar. If you are not root (or the sudo command) when you tar, then the ownerships might not get preserved.

