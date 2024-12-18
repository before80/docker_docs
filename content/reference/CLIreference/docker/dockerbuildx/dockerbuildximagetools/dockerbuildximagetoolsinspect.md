+++
title = "docker buildx imagetools inspect"
date = 2024-10-23T14:54:43+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/cli/docker/buildx/imagetools/inspect/](https://docs.docker.com/reference/cli/docker/buildx/imagetools/inspect/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# docker buildx imagetools inspect

| Description | Show details of an image in the registry          |
| :---------- | ------------------------------------------------- |
| Usage       | `docker buildx imagetools inspect [OPTIONS] NAME` |

## Description

Show details of an image in the registry.

​	显示注册表中镜像的详细信息。

```console
$ docker buildx imagetools inspect alpine
Name:      docker.io/library/alpine:latest
MediaType: application/vnd.docker.distribution.manifest.list.v2+json
Digest:    sha256:21a3deaa0d32a8057914f36584b5288d2e5ecc984380bc0118285c70fa8c9300

Manifests:
  Name:      docker.io/library/alpine:latest@sha256:e7d88de73db3d3fd9b2d63aa7f447a10fd0220b7cbf39803c803f2af9ba256b3
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/amd64

  Name:      docker.io/library/alpine:latest@sha256:e047bc2af17934d38c5a7fa9f46d443f1de3a7675546402592ef805cfa929f9d
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/arm/v6

  Name:      docker.io/library/alpine:latest@sha256:8483ecd016885d8dba70426fda133c30466f661bb041490d525658f1aac73822
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/arm/v7

  Name:      docker.io/library/alpine:latest@sha256:c74f1b1166784193ea6c8f9440263b9be6cae07dfe35e32a5df7a31358ac2060
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/arm64/v8

  Name:      docker.io/library/alpine:latest@sha256:2689e157117d2da668ad4699549e55eba1ceb79cb7862368b30919f0488213f4
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/386

  Name:      docker.io/library/alpine:latest@sha256:2042a492bcdd847a01cd7f119cd48caa180da696ed2aedd085001a78664407d6
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/ppc64le

  Name:      docker.io/library/alpine:latest@sha256:49e322ab6690e73a4909f787bcbdb873631264ff4a108cddfd9f9c249ba1d58e
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/s390x
```

## Options

| Option                                                       | Default           | Description                                                  |
| ------------------------------------------------------------ | ----------------- | ------------------------------------------------------------ |
| [`--format`](https://docs.docker.com/reference/cli/docker/buildx/imagetools/inspect/#format) | ``{{.Manifest}}`` | 使用给定的 Go 模板格式化输出 Format the output using the given Go template |
| [`--raw`](https://docs.docker.com/reference/cli/docker/buildx/imagetools/inspect/#raw) |                   | 显示原始的、未格式化的 JSON 清单 Show original, unformatted JSON manifest |

## Examples

### 覆盖配置的构建器实例（`--builder`） Override the configured builder instance (`--builder`)

Same as [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder).

​	与 [`buildx --builder`](https://docs.docker.com/reference/cli/docker/buildx/#builder) 相同。

### 格式化输出（`--format`） Format the output (`--format`)

Format the output using the given Go template. Defaults to `{{.Manifest}}` if unset. Following fields are available:

​	使用给定的 Go 模板格式化输出。如果未设置，则默认为 `{{.Manifest}}`。可用字段包括：

- `.Name`: provides the reference of the image 提供镜像的引用

- `.Manifest`: provides the manifest or manifest list 提供清单或清单列表
- `.Image`: provides the image config 提供镜像配置

#### `.Name`



```console
$ docker buildx imagetools inspect alpine --format "{{.Name}}"
Name: docker.io/library/alpine:latest
```

#### `.Manifest`



```console
$ docker buildx imagetools inspect crazymax/loop --format "{{.Manifest}}"
Name:      docker.io/crazymax/loop:latest
MediaType: application/vnd.docker.distribution.manifest.v2+json
Digest:    sha256:08602e7340970e92bde5e0a2e887c1fde4d9ae753d1e05efb4c8ef3b609f97f1
```



```console
$ docker buildx imagetools inspect moby/buildkit:master --format "{{.Manifest}}"
Name:      docker.io/moby/buildkit:master
MediaType: application/vnd.docker.distribution.manifest.list.v2+json
Digest:    sha256:3183f7ce54d1efb44c34b84f428ae10aaf141e553c6b52a7ff44cc7083a05a66

Manifests:
  Name:      docker.io/moby/buildkit:master@sha256:667d28c9fb33820ce686887a717a148e89fa77f9097f9352996bbcce99d352b1
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/amd64

  Name:      docker.io/moby/buildkit:master@sha256:71789527b64ab3d7b3de01d364b449cd7f7a3da758218fbf73b9c9aae05a6775
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/arm/v7

  Name:      docker.io/moby/buildkit:master@sha256:fb64667e1ce6ab0d05478f3a8402af07b27737598dcf9a510fb1d792b13a66be
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/arm64

  Name:      docker.io/moby/buildkit:master@sha256:1c3ddf95a0788e23f72f25800c05abc4458946685e2b66788c3d978cde6da92b
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/s390x

  Name:      docker.io/moby/buildkit:master@sha256:05bcde6d460a284e5bc88026cd070277e8380355de3126cbc8fe8a452708c6b1
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/ppc64le

  Name:      docker.io/moby/buildkit:master@sha256:c04c57765304ab84f4f9807fff3e11605c3a60e16435c734b02c723680f6bd6e
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/riscv64
```

#### JSON output

A `json` template function is also available if you want to render fields in JSON format:

​	如果想以 JSON 格式呈现字段，还可以使用 `json` 模板函数：

```console
$ docker buildx imagetools inspect crazymax/buildkit:attest --format "{{json .Manifest}}"
```



```json
{
  "schemaVersion": 2,
  "mediaType": "application/vnd.oci.image.index.v1+json",
  "digest": "sha256:7007b387ccd52bd42a050f2e8020e56e64622c9269bf7bbe257b326fe99daf19",
  "size": 855,
  "manifests": [
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:fbd10fe50b4b174bb9ea273e2eb9827fa8bf5c88edd8635a93dc83e0d1aecb55",
      "size": 673,
      "platform": {
        "architecture": "amd64",
        "os": "linux"
      }
    },
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:a9de632c16998489fd63fbca42a03431df00639cfb2ecb8982bf9984b83c5b2b",
      "size": 839,
      "annotations": {
        "vnd.docker.reference.digest": "sha256:fbd10fe50b4b174bb9ea273e2eb9827fa8bf5c88edd8635a93dc83e0d1aecb55",
        "vnd.docker.reference.type": "attestation-manifest"
      },
      "platform": {
        "architecture": "unknown",
        "os": "unknown"
      }
    }
  ]
}
```



```console
$ docker buildx imagetools inspect crazymax/buildkit:attest --format "{{json .Image}}"
```



```json
{
  "created": "2022-12-01T11:46:47.713777178Z",
  "architecture": "amd64",
  "os": "linux",
  "config": {
    "Env": [
      "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
    ],
    "Cmd": [
      "/bin/sh"
    ]
  },
  "rootfs": {
    "type": "layers",
    "diff_ids": [
      "sha256:ded7a220bb058e28ee3254fbba04ca90b679070424424761a53a043b93b612bf",
      "sha256:d85d09ab4b4e921666ccc2db8532e857bf3476b7588e52c9c17741d7af14204f"
    ]
  },
  "history": [
    {
      "created": "2022-11-22T22:19:28.870801855Z",
      "created_by": "/bin/sh -c #(nop) ADD file:587cae71969871d3c6456d844a8795df9b64b12c710c275295a1182b46f630e7 in / "
    },
    {
      "created": "2022-11-22T22:19:29.008562326Z",
      "created_by": "/bin/sh -c #(nop)  CMD [\"/bin/sh\"]",
      "empty_layer": true
    },
    {
      "created": "2022-12-01T11:46:47.713777178Z",
      "created_by": "RUN /bin/sh -c apk add curl # buildkit",
      "comment": "buildkit.dockerfile.v0"
    }
  ]
}
```



```console
$ docker buildx imagetools inspect moby/buildkit:master --format "{{json .Manifest}}"
```



```json
{
  "schemaVersion": 2,
  "mediaType": "application/vnd.oci.image.index.v1+json",
  "digest": "sha256:d895e8fdcf5e2bb39acb5966f97fc4cd87a2d13d27c939c320025eb4aca5440c",
  "size": 4654,
  "manifests": [
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:ac9dd4fbec9e36b562f910618975a2936533f8e411a3fea2858aacc0ac972e1c",
      "size": 1054,
      "platform": {
        "architecture": "amd64",
        "os": "linux"
      }
    },
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:0f4dc6797db467372cbf52c7236816203654a839f64a6542c9135d1973c9d744",
      "size": 1054,
      "platform": {
        "architecture": "arm",
        "os": "linux",
        "variant": "v7"
      }
    },
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:d62bb533d95afe17c4a9caf1e7c57a3b0a7a67409ccfa7af947aeb0f670ffb87",
      "size": 1054,
      "platform": {
        "architecture": "arm64",
        "os": "linux"
      }
    },
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:b4944057e0c68203cdcc3dceff3b2df3c7d9e3dd801724fa977b01081da7771e",
      "size": 1054,
      "platform": {
        "architecture": "s390x",
        "os": "linux"
      }
    },
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:825702a51eb4234904fc9253d8b0bf0a584787ffd8fc3fd6fa374188233ce399",
      "size": 1054,
      "platform": {
        "architecture": "ppc64le",
        "os": "linux"
      }
    },
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:dfb27c6acc9b9f3a7c9d47366d137089565062f43c8063c9f5e408d34c87ee4a",
      "size": 1054,
      "platform": {
        "architecture": "riscv64",
        "os": "linux"
      }
    },
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:f2fe69bccc878e658caf21dfc99eaf726fb20d28f17398c1d66a90e62cc019f9",
      "size": 1113,
      "annotations": {
        "vnd.docker.reference.digest": "sha256:ac9dd4fbec9e36b562f910618975a2936533f8e411a3fea2858aacc0ac972e1c",
        "vnd.docker.reference.type": "attestation-manifest"
      },
      "platform": {
        "architecture": "unknown",
        "os": "unknown"
      }
    },
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:9e112f8d4e383186f36369fba7b454e246d2e9ca5def797f1b84ede265e9f3ca",
      "size": 1113,
      "annotations": {
        "vnd.docker.reference.digest": "sha256:0f4dc6797db467372cbf52c7236816203654a839f64a6542c9135d1973c9d744",
        "vnd.docker.reference.type": "attestation-manifest"
      },
      "platform": {
        "architecture": "unknown",
        "os": "unknown"
      }
    },
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:09d593587f8665269ec6753eaed7fbdb09968f71587dd53e06519502cbc16775",
      "size": 1113,
      "annotations": {
        "vnd.docker.reference.digest": "sha256:d62bb533d95afe17c4a9caf1e7c57a3b0a7a67409ccfa7af947aeb0f670ffb87",
        "vnd.docker.reference.type": "attestation-manifest"
      },
      "platform": {
        "architecture": "unknown",
        "os": "unknown"
      }
    },
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:985a3f4544dfb042db6a8703f5f76438667dd7958aba14cb04bebe3b4cbd9307",
      "size": 1113,
      "annotations": {
        "vnd.docker.reference.digest": "sha256:b4944057e0c68203cdcc3dceff3b2df3c7d9e3dd801724fa977b01081da7771e",
        "vnd.docker.reference.type": "attestation-manifest"
      },
      "platform": {
        "architecture": "unknown",
        "os": "unknown"
      }
    },
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:cfccb6afeede7dc29bf8abef4815d56f2723fa482ea63c9cd519cd991c379294",
      "size": 1113,
      "annotations": {
        "vnd.docker.reference.digest": "sha256:825702a51eb4234904fc9253d8b0bf0a584787ffd8fc3fd6fa374188233ce399",
        "vnd.docker.reference.type": "attestation-manifest"
      },
      "platform": {
        "architecture": "unknown",
        "os": "unknown"
      }
    },
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:2e93733432c6a14cb57db33928b3a17d7ca298b3babe24d9f56dca2754dbde3b",
      "size": 1113,
      "annotations": {
        "vnd.docker.reference.digest": "sha256:dfb27c6acc9b9f3a7c9d47366d137089565062f43c8063c9f5e408d34c87ee4a",
        "vnd.docker.reference.type": "attestation-manifest"
      },
      "platform": {
        "architecture": "unknown",
        "os": "unknown"
      }
    }
  ]
}
```

The following command provides [SLSA](https://github.com/moby/buildkit/blob/master/docs/attestations/slsa-provenance.md) JSON output:



```console
$ docker buildx imagetools inspect crazymax/buildkit:attest --format "{{json .Provenance}}"
```



```json
{
  "SLSA": {
    "builder": {
      "id": ""
    },
    "buildType": "https://mobyproject.org/buildkit@v1",
    "materials": [
      {
        "uri": "pkg:docker/docker/buildkit-syft-scanner@stable-1",
        "digest": {
          "sha256": "b45f1d207e16c3a3a5a10b254ad8ad358d01f7ea090d382b95c6b2ee2b3ef765"
        }
      },
      {
        "uri": "pkg:docker/alpine@latest?platform=linux%2Famd64",
        "digest": {
          "sha256": "8914eb54f968791faf6a8638949e480fef81e697984fba772b3976835194c6d4"
        }
      }
    ],
    "invocation": {
      "configSource": {},
      "parameters": {
        "frontend": "dockerfile.v0",
        "locals": [
          {
            "name": "context"
          },
          {
            "name": "dockerfile"
          }
        ]
      },
      "environment": {
        "platform": "linux/amd64"
      }
    },
    "metadata": {
      "buildInvocationID": "02tdha2xkbxvin87mz9drhag4",
      "buildStartedOn": "2022-12-01T11:50:07.264704131Z",
      "buildFinishedOn": "2022-12-01T11:50:08.243788739Z",
      "reproducible": false,
      "completeness": {
        "parameters": true,
        "environment": true,
        "materials": false
      },
      "https://mobyproject.org/buildkit@v1#metadata": {}
    }
  }
}
```

The following command provides [SBOM](https://github.com/moby/buildkit/blob/master/docs/attestations/sbom.md) JSON output:

​	以下命令提供 [SBOM](https://github.com/moby/buildkit/blob/master/docs/attestations/sbom.md) JSON 输出：

```console
$ docker buildx imagetools inspect crazymax/buildkit:attest --format "{{json .SBOM}}"
```



```json
{
  "SPDX": {
    "SPDXID": "SPDXRef-DOCUMENT",
    "creationInfo": {
      "created": "2022-12-01T11:46:48.063400162Z",
      "creators": [
        "Tool: syft-v0.60.3",
        "Tool: buildkit-1ace2bb",
        "Organization: Anchore, Inc"
      ],
      "licenseListVersion": "3.18"
    },
    "dataLicense": "CC0-1.0",
    "documentNamespace": "https://anchore.com/syft/dir/run/src/core-0a4ccc6d-1a72-4c3a-a40e-3df1a2ffca94",
    "files": [...],
    "spdxVersion": "SPDX-2.2"
  }
}
```



```console
$ docker buildx imagetools inspect crazymax/buildkit:attest --format "{{json .}}"
```



```json
{
  "name": "crazymax/buildkit:attest",
  "manifest": {
    "schemaVersion": 2,
    "mediaType": "application/vnd.oci.image.index.v1+json",
    "digest": "sha256:7007b387ccd52bd42a050f2e8020e56e64622c9269bf7bbe257b326fe99daf19",
    "size": 855,
    "manifests": [
      {
        "mediaType": "application/vnd.oci.image.manifest.v1+json",
        "digest": "sha256:fbd10fe50b4b174bb9ea273e2eb9827fa8bf5c88edd8635a93dc83e0d1aecb55",
        "size": 673,
        "platform": {
          "architecture": "amd64",
          "os": "linux"
        }
      },
      {
        "mediaType": "application/vnd.oci.image.manifest.v1+json",
        "digest": "sha256:a9de632c16998489fd63fbca42a03431df00639cfb2ecb8982bf9984b83c5b2b",
        "size": 839,
        "annotations": {
          "vnd.docker.reference.digest": "sha256:fbd10fe50b4b174bb9ea273e2eb9827fa8bf5c88edd8635a93dc83e0d1aecb55",
          "vnd.docker.reference.type": "attestation-manifest"
        },
        "platform": {
          "architecture": "unknown",
          "os": "unknown"
        }
      }
    ]
  },
  "image": {
    "created": "2022-12-01T11:46:47.713777178Z",
    "architecture": "amd64",
    "os": "linux",
    "config": {
      "Env": [
        "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
      ],
      "Cmd": [
        "/bin/sh"
      ]
    },
    "rootfs": {
      "type": "layers",
      "diff_ids": [
        "sha256:ded7a220bb058e28ee3254fbba04ca90b679070424424761a53a043b93b612bf",
        "sha256:d85d09ab4b4e921666ccc2db8532e857bf3476b7588e52c9c17741d7af14204f"
      ]
    },
    "history": [
      {
        "created": "2022-11-22T22:19:28.870801855Z",
        "created_by": "/bin/sh -c #(nop) ADD file:587cae71969871d3c6456d844a8795df9b64b12c710c275295a1182b46f630e7 in / "
      },
      {
        "created": "2022-11-22T22:19:29.008562326Z",
        "created_by": "/bin/sh -c #(nop)  CMD [\"/bin/sh\"]",
        "empty_layer": true
      },
      {
        "created": "2022-12-01T11:46:47.713777178Z",
        "created_by": "RUN /bin/sh -c apk add curl # buildkit",
        "comment": "buildkit.dockerfile.v0"
      }
    ]
  }
}
```

#### 多平台 Multi-platform

Multi-platform images are supported for `.Image`, `.SLSA` and `.SBOM` fields. If you want to pick up a specific platform, you can specify it using the `index` go template function:

​	多平台镜像支持 `.Image`、`.SLSA` 和 `.SBOM` 字段。如果您想选择特定平台，可以使用 `index` Go 模板函数进行指定：



```console
$ docker buildx imagetools inspect --format '{{json (index .Image "linux/s390x")}}' moby/buildkit:master
```



```json
{
  "created": "2022-11-30T17:42:26.414957336Z",
  "architecture": "s390x",
  "os": "linux",
  "config": {
    "Env": [
      "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
    ],
    "Entrypoint": [
      "buildkitd"
    ],
    "Volumes": {
      "/var/lib/buildkit": {}
    }
  },
  "rootfs": {
    "type": "layers",
    "diff_ids": [
      "sha256:41048e32d0684349141cf05f629c5fc3c5915d1f3426b66dbb8953a540e01e1e",
      "sha256:2651209b9208fff6c053bc3c17353cb07874e50f1a9bc96d6afd03aef63de76a",
      "sha256:88577322e65f094ce8ac27435880f1a8a9baadb569258026bb141770451bafcb",
      "sha256:de8f9a790e4ed10ff1f1f8ea923c9da4f97246a7e200add2dc6650eba3f10a20"
    ]
  },
  "history": [
    {
      "created": "2021-11-24T20:41:23.709681315Z",
      "created_by": "/bin/sh -c #(nop) ADD file:cd24c711a2ef431b3ff94f9a02bfc42f159bc60de1d0eceecafea4e8af02441d in / "
    },
    {
      "created": "2021-11-24T20:41:23.94211262Z",
      "created_by": "/bin/sh -c #(nop)  CMD [\"/bin/sh\"]",
      "empty_layer": true
    },
    {
      "created": "2022-01-26T18:15:21.449825391Z",
      "created_by": "RUN /bin/sh -c apk add --no-cache fuse3 git openssh pigz xz   \u0026\u0026 ln -s fusermount3 /usr/bin/fusermount # buildkit",
      "comment": "buildkit.dockerfile.v0"
    },
    {
      "created": "2022-08-25T00:39:25.652811078Z",
      "created_by": "COPY examples/buildctl-daemonless/buildctl-daemonless.sh /usr/bin/ # buildkit",
      "comment": "buildkit.dockerfile.v0"
    },
    {
      "created": "2022-11-30T17:42:26.414957336Z",
      "created_by": "VOLUME [/var/lib/buildkit]",
      "comment": "buildkit.dockerfile.v0",
      "empty_layer": true
    },
    {
      "created": "2022-11-30T17:42:26.414957336Z",
      "created_by": "COPY / /usr/bin/ # buildkit",
      "comment": "buildkit.dockerfile.v0"
    },
    {
      "created": "2022-11-30T17:42:26.414957336Z",
      "created_by": "ENTRYPOINT [\"buildkitd\"]",
      "comment": "buildkit.dockerfile.v0",
      "empty_layer": true
    }
  ]
}
```

### 显示原始 JSON 清单（`--raw`）Show original JSON manifest (`--raw`)

Use the `--raw` option to print the raw JSON manifest.

​	使用 `--raw` 选项打印原始 JSON 清单。

```console
$ docker buildx imagetools inspect --raw crazymax/loop
```



```json
{
  "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
  "schemaVersion": 2,
  "config": {
    "mediaType": "application/vnd.docker.container.image.v1+json",
    "digest": "sha256:a98999183d2c7a8845f6d56496e51099ce6e4359ee7255504174b05430c4b78b",
    "size": 2762
  },
  "layers": [
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "digest": "sha256:8663204ce13b2961da55026a2034abb9e5afaaccf6a9cfb44ad71406dcd07c7b",
      "size": 2818370
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "digest": "sha256:f0868a92f8e1e5018ed4e60eb845ed4ff0e2229897f4105e5a4735c1d6fd874f",
      "size": 1821402
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "digest": "sha256:d010066dbdfcf7c12fca30cd4b567aa7218eb6762ab53169d043655b7a8d7f2e",
      "size": 404457
    }
  ]
}
```



```console
$ docker buildx imagetools inspect --raw moby/buildkit:master | jq
```



```json
{
  "mediaType": "application/vnd.docker.distribution.manifest.list.v2+json",
  "schemaVersion": 2,
  "manifests": [
    {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:f9f41c85124686c2afe330a985066748a91d7a5d505777fe274df804ab5e077e",
      "size": 1158,
      "platform": {
        "architecture": "amd64",
        "os": "linux"
      }
    },
    {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:82097c2be19c617aafb3c3e43c88548738d4b2bf3db5c36666283a918b390266",
      "size": 1158,
      "platform": {
        "architecture": "arm",
        "os": "linux",
        "variant": "v7"
      }
    },
    {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:b6b91e6c823d7220ded7d3b688e571ba800b13d91bbc904c1d8053593e3ee42c",
      "size": 1158,
      "platform": {
        "architecture": "arm64",
        "os": "linux"
      }
    },
    {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:797061bcc16778de048b96f769c018ec24da221088050bbe926ea3b8d51d77e8",
      "size": 1158,
      "platform": {
        "architecture": "s390x",
        "os": "linux"
      }
    },
    {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:b93d3a84d18c4d0b8c279e77343d854d9b5177df7ea55cf468d461aa2523364e",
      "size": 1159,
      "platform": {
        "architecture": "ppc64le",
        "os": "linux"
      }
    },
    {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:d5c950dd1b270d437c838187112a0cb44c9258248d7a3a8bcb42fae8f717dc01",
      "size": 1158,
      "platform": {
        "architecture": "riscv64",
        "os": "linux"
      }
    }
  ]
}
```
