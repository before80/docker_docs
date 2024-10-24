+++
title = "docker buildx bake"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/reference/cli/docker/buildx/bake/](https://docs.docker.com/reference/cli/docker/buildx/bake/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx bake

| Description | Build from a file                          |
| :---------- | ------------------------------------------ |
| Usage       | `docker buildx bake [OPTIONS] [TARGET...]` |
| Aliases     | `docker buildx f`                          |

## [Description](https://docs.docker.com/reference/cli/docker/buildx/bake/#description)

Bake is a high-level build command. Each specified target runs in parallel as part of the build.

Read [High-level build options with Bake](https://docs.docker.com/build/bake/) guide for introduction to writing bake files.

> **Note**
>
> `buildx bake` command may receive backwards incompatible features in the future if needed. We are looking for feedback on improving the command and extending the functionality further.

## [Options](https://docs.docker.com/reference/cli/docker/buildx/bake/#options)

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| `--allow`                                                    |         | Allow build to access specified resources                    |
| [`--call`](https://docs.docker.com/reference/cli/docker/buildx/bake/#call) | `build` | Set method for evaluating build (`check`, `outline`, `targets`) |
| [`--check`](https://docs.docker.com/reference/cli/docker/buildx/bake/#check) |         | Shorthand for `--call=check`                                 |
| [`-f, --file`](https://docs.docker.com/reference/cli/docker/buildx/bake/#file) |         | Build definition file                                        |
| `--load`                                                     |         | Shorthand for `--set=*.output=type=docker`                   |
| [`--metadata-file`](https://docs.docker.com/reference/cli/docker/buildx/bake/#metadata-file) |         | Write build result metadata to a file                        |
| [`--no-cache`](https://docs.docker.com/reference/cli/docker/buildx/bake/#no-cache) |         | Do not use cache when building the image                     |
| [`--print`](https://docs.docker.com/reference/cli/docker/buildx/bake/#print) |         | Print the options without building                           |
| [`--progress`](https://docs.docker.com/reference/cli/docker/buildx/bake/#progress) | `auto`  | Set type of progress output (`auto`, `plain`, `tty`, `rawjson`). Use plain to show container output |
| [`--provenance`](https://docs.docker.com/reference/cli/docker/buildx/bake/#provenance) |         | Shorthand for `--set=*.attest=type=provenance`               |
| [`--pull`](https://docs.docker.com/reference/cli/docker/buildx/bake/#pull) |         | Always attempt to pull all referenced images                 |
| `--push`                                                     |         | Shorthand for `--set=*.output=type=registry`                 |
| [`--sbom`](https://docs.docker.com/reference/cli/docker/buildx/bake/#sbom) |         | Shorthand for `--set=*.attest=type=sbom`                     |
| [`--set`](https://docs.docker.com/reference/cli/docker/buildx/bake/#set) |         | Override target value (e.g., `targetpattern.key=value`)      |

## [Examples](https://docs.docker.com/reference/cli/docker/buildx/bake/#examples)

### [Override the configured builder instance (--builder)](https://docs.docker.com/reference/cli/docker/buildx/bake/#builder)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).

### [Invoke a frontend method (--call)](https://docs.docker.com/reference/cli/docker/buildx/bake/#call)

Same as [`build --call`](https://docs.docker.com/reference/cli/docker/buildx/build/#call).

#### [Call: check (--check)](https://docs.docker.com/reference/cli/docker/buildx/bake/#check)

Same as [`build --check`](https://docs.docker.com/reference/cli/docker/buildx/build/#check).

### [Specify a build definition file (-f, --file)](https://docs.docker.com/reference/cli/docker/buildx/bake/#file)

Use the `-f` / `--file` option to specify the build definition file to use. The file can be an HCL, JSON or Compose file. If multiple files are specified, all are read and the build configurations are combined.

You can pass the names of the targets to build, to build only specific target(s). The following example builds the `db` and `webapp-release` targets that are defined in the `docker-bake.dev.hcl` file:



```hcl
# docker-bake.dev.hcl
group "default" {
  targets = ["db", "webapp-dev"]
}

target "webapp-dev" {
  dockerfile = "Dockerfile.webapp"
  tags = ["docker.io/username/webapp"]
}

target "webapp-release" {
  inherits = ["webapp-dev"]
  platforms = ["linux/amd64", "linux/arm64"]
}

target "db" {
  dockerfile = "Dockerfile.db"
  tags = ["docker.io/username/db"]
}
```



```console
$ docker buildx bake -f docker-bake.dev.hcl db webapp-release
```

See the [Bake file reference](https://docs.docker.com/build/bake/reference/) for more details.

### [Write build results metadata to a file (--metadata-file)](https://docs.docker.com/reference/cli/docker/buildx/bake/#metadata-file)

Similar to [`buildx build --metadata-file`](https://docs.docker.com/reference/cli/docker/buildx/build/#metadata-file) but writes a map of results for each target such as:



```hcl
# docker-bake.hcl
group "default" {
  targets = ["db", "webapp-dev"]
}

target "db" {
  dockerfile = "Dockerfile.db"
  tags = ["docker.io/username/db"]
}

target "webapp-dev" {
  dockerfile = "Dockerfile.webapp"
  tags = ["docker.io/username/webapp"]
}
```



```console
$ docker buildx bake --load --metadata-file metadata.json .
$ cat metadata.json
```



```json
{
  "buildx.build.warnings": {},
  "db": {
    "buildx.build.provenance": {},
    "buildx.build.ref": "mybuilder/mybuilder0/0fjb6ubs52xx3vygf6fgdl611",
    "containerimage.config.digest": "sha256:2937f66a9722f7f4a2df583de2f8cb97fc9196059a410e7f00072fc918930e66",
    "containerimage.descriptor": {
      "annotations": {
        "config.digest": "sha256:2937f66a9722f7f4a2df583de2f8cb97fc9196059a410e7f00072fc918930e66",
        "org.opencontainers.image.created": "2022-02-08T21:28:03Z"
      },
      "digest": "sha256:19ffeab6f8bc9293ac2c3fdf94ebe28396254c993aea0b5a542cfb02e0883fa3",
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "size": 506
    },
    "containerimage.digest": "sha256:19ffeab6f8bc9293ac2c3fdf94ebe28396254c993aea0b5a542cfb02e0883fa3"
  },
  "webapp-dev": {
    "buildx.build.provenance": {},
    "buildx.build.ref": "mybuilder/mybuilder0/kamngmcgyzebqxwu98b4lfv3n",
    "containerimage.config.digest": "sha256:9651cc2b3c508f697c9c43b67b64c8359c2865c019e680aac1c11f4b875b67e0",
    "containerimage.descriptor": {
      "annotations": {
        "config.digest": "sha256:9651cc2b3c508f697c9c43b67b64c8359c2865c019e680aac1c11f4b875b67e0",
        "org.opencontainers.image.created": "2022-02-08T21:28:15Z"
      },
      "digest": "sha256:6d9ac9237a84afe1516540f40a0fafdc86859b2141954b4d643af7066d598b74",
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "size": 506
    },
    "containerimage.digest": "sha256:6d9ac9237a84afe1516540f40a0fafdc86859b2141954b4d643af7066d598b74"
  }
}
```

> **Note**
>
> Build record [provenance](https://docs.docker.com/build/metadata/attestations/slsa-provenance/#provenance-attestation-example) (`buildx.build.provenance`) includes minimal provenance by default. Set the `BUILDX_METADATA_PROVENANCE` environment variable to customize this behavior:
>
> - `min` sets minimal provenance (default).
> - `max` sets full provenance.
> - `disabled`, `false` or `0` does not set any provenance.

> **Note**
>
> Build warnings (`buildx.build.warnings`) are not included by default. Set the `BUILDX_METADATA_WARNINGS` environment variable to `1` or `true` to include them.

### [Don't use cache when building the image (--no-cache)](https://docs.docker.com/reference/cli/docker/buildx/bake/#no-cache)

Same as `build --no-cache`. Don't use cache when building the image.

### [Print the options without building (--print)](https://docs.docker.com/reference/cli/docker/buildx/bake/#print)

Prints the resulting options of the targets desired to be built, in a JSON format, without starting a build.



```console
$ docker buildx bake -f docker-bake.hcl --print db
{
  "group": {
    "default": {
      "targets": [
        "db"
      ]
    }
  },
  "target": {
    "db": {
      "context": "./",
      "dockerfile": "Dockerfile",
      "tags": [
        "docker.io/tiborvass/db"
      ]
    }
  }
}
```

### [Set type of progress output (--progress)](https://docs.docker.com/reference/cli/docker/buildx/bake/#progress)

Same as [`build --progress`](https://docs.docker.com/reference/cli/docker/buildx/build/#progress).

### [Create provenance attestations (--provenance)](https://docs.docker.com/reference/cli/docker/buildx/bake/#provenance)

Same as [`build --provenance`](https://docs.docker.com/reference/cli/docker/buildx/build/#provenance).

### [Always attempt to pull a newer version of the image (--pull)](https://docs.docker.com/reference/cli/docker/buildx/bake/#pull)

Same as `build --pull`.

### [Create SBOM attestations (--sbom)](https://docs.docker.com/reference/cli/docker/buildx/bake/#sbom)

Same as [`build --sbom`](https://docs.docker.com/reference/cli/docker/buildx/build/#sbom).

### [Override target configurations from command line (--set)](https://docs.docker.com/reference/cli/docker/buildx/bake/#set)



```text
--set targetpattern.key[.subkey]=value
```

Override target configurations from command line. The pattern matching syntax is defined in https://golang.org/pkg/path/#Match.



```console
$ docker buildx bake --set target.args.mybuildarg=value
$ docker buildx bake --set target.platform=linux/arm64
$ docker buildx bake --set foo*.args.mybuildarg=value # overrides build arg for all targets starting with 'foo'
$ docker buildx bake --set *.platform=linux/arm64     # overrides platform for all targets
$ docker buildx bake --set foo*.no-cache              # bypass caching only for targets starting with 'foo'
```

You can override the following fields:

- `args`
- `cache-from`
- `cache-to`
- `context`
- `dockerfile`
- `labels`
- `load`
- `no-cache`
- `no-cache-filter`
- `output`
- `platform`
- `pull`
- `push`
- `secrets`
- `ssh`
- `tags`
- `target`
