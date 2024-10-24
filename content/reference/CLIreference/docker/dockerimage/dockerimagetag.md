+++
title = "docker image tag"
date = 2024-10-23T14:54:43+08:00
weight = 70
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/image/tag/](https://docs.docker.com/reference/cli/docker/image/tag/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker image tag

| Description | Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE    |
| :---------- | -------------------------------------------------------- |
| Usage       | `docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]` |
| Aliases     | `docker tag`                                             |

## Description

A full image name has the following format and components:

```
[HOST[:PORT_NUMBER]/]PATH
```

- `HOST`: The optional registry hostname specifies where the image is located. The hostname must comply with standard DNS rules, but may not contain underscores. If you don't specify a hostname, the command uses Docker's public registry at `registry-1.docker.io` by default. Note that `docker.io` is the canonical reference for Docker's public registry.

- `PORT_NUMBER`: If a hostname is present, it may optionally be followed by a registry port number in the format `:8080`.

- ```
  PATH
  ```

  : The path consists of slash-separated components. Each component may contain lowercase letters, digits and separators. A separator is defined as a period, one or two underscores, or one or more hyphens. A component may not start or end with a separator. While the

   

  OCI Distribution Specification

   

  supports more than two slash-separated components, most registries only support two slash-separated components. For Docker's public registry, the path format is as follows:

  - `[NAMESPACE/]REPOSITORY`: The first, optional component is typically a user's or an organization's namespace. The second, mandatory component is the repository name. When the namespace is not present, Docker uses `library` as the default namespace.

After the image name, the optional `TAG` is a custom, human-readable manifest identifier that's typically a specific version or variant of an image. The tag must be valid ASCII and can contain lowercase and uppercase letters, digits, underscores, periods, and hyphens. It can't start with a period or hyphen and must be no longer than 128 characters. If you don't specify a tag, the command uses `latest` by default.

You can group your images together using names and tags, and then [push]({{< ref "/reference/CLIreference/docker/dockerimage/dockerpush" >}}) them to a registry.

## Examples

### Tag an image referenced by ID

To tag a local image with ID `0e5574283393` as `fedora/httpd` with the tag `version1.0`:



```console
$ docker tag 0e5574283393 fedora/httpd:version1.0
```

### Tag an image referenced by Name

To tag a local image `httpd` as `fedora/httpd` with the tag `version1.0`:



```console
$ docker tag httpd fedora/httpd:version1.0
```

Note that since the tag name isn't specified, the alias is created for an existing local version `httpd:latest`.

### Tag an image referenced by Name and Tag

To tag a local image with the name `httpd` and the tag `test` as `fedora/httpd` with the tag `version1.0.test`:



```console
$ docker tag httpd:test fedora/httpd:version1.0.test
```

### Tag an image for a private registry

To push an image to a private registry and not the public Docker registry you must include the registry hostname and port (if needed).



```console
$ docker tag 0e5574283393 myregistryhost:5000/fedora/httpd:version1.0
```
