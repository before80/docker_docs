+++
title = "docker buildx bake"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/buildx/bake/](https://docs.docker.com/reference/cli/docker/buildx/bake/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx bake

| Description | Build from a file                          |
| :---------- | ------------------------------------------ |
| Usage       | `docker buildx bake [OPTIONS] [TARGET...]` |
| Aliases     | `docker buildx f`                          |

## Description

Bake is a high-level build command. Each specified target runs in parallel as part of the build.

​	Bake 是一个高级构建命令。每个指定的目标作为构建的一部分并行运行。

Read [High-level build options with Bake]({{< ref "/manuals/DockerBuild/Bake" >}}) guide for introduction to writing bake files.

​	阅读[使用 Bake 的高级构建选项]({{< ref "/manuals/DockerBuild/Bake" >}})指南，以了解如何编写 Bake 文件。

> **Note**
>
> `buildx bake` command may receive backwards incompatible features in the future if needed. We are looking for feedback on improving the command and extending the functionality further.
>
> ​	`buildx bake` 命令未来可能会包含不向后兼容的功能。我们期待您提供反馈，以帮助改进该命令并进一步扩展其功能。

## Options

| Option                                                       | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| `--allow`                                                    |         | 允许构建访问指定的资源 Allow build to access specified resources |
| [`--call`](https://docs.docker.com/reference/cli/docker/buildx/bake/#call) | `build` | 设置用于求值构建的方法 (`check`, `outline`, `targets`) Set method for evaluating build (`check`, `outline`, `targets`) |
| [`--check`](https://docs.docker.com/reference/cli/docker/buildx/bake/#check) |         | `--call=check` 的简写 Shorthand for `--call=check`           |
| [`-f, --file`](https://docs.docker.com/reference/cli/docker/buildx/bake/#file) |         | 构建定义文件 Build definition file                           |
| `--load`                                                     |         | `--set=*.output=type=docker` 的简写 Shorthand for `--set=*.output=type=docker` |
| [`--metadata-file`](https://docs.docker.com/reference/cli/docker/buildx/bake/#metadata-file) |         | 将构建结果的元数据写入文件 Write build result metadata to a file |
| [`--no-cache`](https://docs.docker.com/reference/cli/docker/buildx/bake/#no-cache) |         | 构建镜像时不使用缓存 Do not use cache when building the image |
| [`--print`](https://docs.docker.com/reference/cli/docker/buildx/bake/#print) |         | 打印选项但不进行构建 Print the options without building      |
| [`--progress`](https://docs.docker.com/reference/cli/docker/buildx/bake/#progress) | `auto`  | 设置进度输出的类型 (`auto`, `plain`, `tty`, `rawjson`) Set type of progress output (`auto`, `plain`, `tty`, `rawjson`). Use plain to show container output |
| [`--provenance`](https://docs.docker.com/reference/cli/docker/buildx/bake/#provenance) |         | `--set=*.attest=type=provenance` 的简写 Shorthand for `--set=*.attest=type=provenance` |
| [`--pull`](https://docs.docker.com/reference/cli/docker/buildx/bake/#pull) |         | 始终尝试拉取所有引用的镜像 Always attempt to pull all referenced images |
| `--push`                                                     |         | `--set=*.output=type=registry` 的简写 Shorthand for `--set=*.output=type=registry` |
| [`--sbom`](https://docs.docker.com/reference/cli/docker/buildx/bake/#sbom) |         | `--set=*.attest=type=sbom` 的简写 Shorthand for `--set=*.attest=type=sbom` |
| [`--set`](https://docs.docker.com/reference/cli/docker/buildx/bake/#set) |         | 覆盖目标值（例如 `targetpattern.key=value`） Override target value (e.g., `targetpattern.key=value`) |

## Examples

### 覆盖已配置的构建实例 (`--builder`) Override the configured builder instance (`--builder`)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).

​	与 [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder) 相同。

### 调用前端方法 (`--call`) Invoke a frontend method (`--call`)

Same as [`build --call`](https://docs.docker.com/reference/cli/docker/buildx/build/#call).

​	与 [`build --call`](https://docs.docker.com/reference/cli/docker/buildx/build/#call) 相同。

#### Call: check (`--check`)

Same as [`build --check`](https://docs.docker.com/reference/cli/docker/buildx/build/#check).

​	与 [`build --check`](https://docs.docker.com/reference/cli/docker/buildx/build/#check) 相同。

### 指定构建定义文件 (`-f`, `--file`) Specify a build definition file (`-f`, `--file`)

Use the `-f` / `--file` option to specify the build definition file to use. The file can be an HCL, JSON or Compose file. If multiple files are specified, all are read and the build configurations are combined.

​	使用 `-f` / `--file` 选项指定要使用的构建定义文件。文件可以是 HCL、JSON 或 Compose 格式。如果指定多个文件，所有文件将被读取，构建配置将被合并。

You can pass the names of the targets to build, to build only specific target(s). The following example builds the `db` and `webapp-release` targets that are defined in the `docker-bake.dev.hcl` file:

​	可以传递要构建的目标名称，仅构建特定的目标。例如，以下示例构建定义在 `docker-bake.dev.hcl` 文件中的 `db` 和 `webapp-release` 目标：



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

See the [Bake file reference]({{< ref "/manuals/DockerBuild/Bake/Bakefilereference" >}}) for more details.

​	有关更多详情，请参阅 [Bake 文件参考]({{< ref "/manuals/DockerBuild/Bake/Bakefilereference" >}})。

### 将构建结果元数据写入文件 (`--metadata-file`) Write build results metadata to a file (`--metadata-file`)

Similar to [`buildx build --metadata-file`](https://docs.docker.com/reference/cli/docker/buildx/build/#metadata-file) but writes a map of results for each target such as:

​	类似于 [`buildx build --metadata-file`](https://docs.docker.com/reference/cli/docker/buildx/build/#metadata-file)，但为每个目标写入结果的映射，如下所示：

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
> ​	构建记录 [provenance](https://docs.docker.com/build/metadata/attestations/slsa-provenance/#provenance-attestation-example) (`buildx.build.provenance`) 默认包含最小的溯源信息。可以通过设置 `BUILDX_METADATA_PROVENANCE` 环境变量来自定义此行为：
>
> - `min` sets minimal provenance (default).
>   - `min` 表示最小溯源（默认）。
> - `max` sets full provenance.
>   - `max` 表示完整溯源。
> - `disabled`, `false` or `0` does not set any provenance.
>   - `disabled`, `false` 或 `0` 不设置溯源信息。

> **Note**
>
> Build warnings (`buildx.build.warnings`) are not included by default. Set the `BUILDX_METADATA_WARNINGS` environment variable to `1` or `true` to include them.
>
> ​	构建警告（`buildx.build.warnings`）默认不包括在内。将 `BUILDX_METADATA_WARNINGS` 环境变量设置为 `1` 或 `true` 以包含它们。

### 构建镜像时不使用缓存 (`--no-cache`) Don't use cache when building the image (`--no-cache`)

Same as `build --no-cache`. Don't use cache when building the image.

​	与 `build --no-cache` 相同。构建镜像时不使用缓存。

### 打印选项但不进行构建 (`--print`) Print the options without building (`--print`)

Prints the resulting options of the targets desired to be built, in a JSON format, without starting a build.

​	以 JSON 格式打印所需构建目标的最终选项，而不开始构建。

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

### 设置进度输出类型 (`--progress`) Set type of progress output (`--progress`)

Same as [`build --progress`](https://docs.docker.com/reference/cli/docker/buildx/build/#progress).

​	与 [`build --progress`](https://docs.docker.com/reference/cli/docker/buildx/build/#progress) 相同。

### 创建溯源声明 (`--provenance`) Create provenance attestations (`--provenance`)

Same as [`build --provenance`](https://docs.docker.com/reference/cli/docker/buildx/build/#provenance).

​	与 [`build --provenance`](https://docs.docker.com/reference/cli/docker/buildx/build/#provenance) 相同。

### 始终尝试拉取最新版本的镜像 (`--pull`) Always attempt to pull a newer version of the image (`--pull`)

Same as `build --pull`.

​	与 `build --pull` 相同。

### 创建 SBOM 声明 (`--sbom`) Create SBOM attestations (`--sbom`)

Same as [`build --sbom`](https://docs.docker.com/reference/cli/docker/buildx/build/#sbom).

​	与 [`build --sbom`](https://docs.docker.com/reference/cli/docker/buildx/build/#sbom) 相同。

### 从命令行覆盖目标配置 (`--set`) Override target configurations from command line (`--set`)



```text
--set targetpattern.key[.subkey]=value
```

Override target configurations from command line. The pattern matching syntax is defined in https://golang.org/pkg/path/#Match.

​	从命令行覆盖目标配置。模式匹配语法定义在 https://golang.org/pkg/path/#Match。

```console
$ docker buildx bake --set target.args.mybuildarg=value
$ docker buildx bake --set target.platform=linux/arm64
$ docker buildx bake --set foo*.args.mybuildarg=value # overrides build arg for all targets starting with 'foo'
$ docker buildx bake --set *.platform=linux/arm64     # overrides platform for all targets
$ docker buildx bake --set foo*.no-cache              # bypass caching only for targets starting with 'foo'
```

You can override the following fields:

​	您可以覆盖以下字段：

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
