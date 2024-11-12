+++
title = "可重复构建"
date = 2024-10-23T14:54:40+08:00
weight = 120
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/ci/github-actions/reproducible-builds/](https://docs.docker.com/build/ci/github-actions/reproducible-builds/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Reproducible builds with GitHub Actions - 使用 GitHub Actions 进行可重复构建

`SOURCE_DATE_EPOCH` is a [standardized environment variable](https://reproducible-builds.org/docs/source-date-epoch/) for instructing build tools to produce a reproducible output. Setting the environment variable for a build makes the timestamps in the image index, config, and file metadata reflect the specified Unix time.

​	`SOURCE_DATE_EPOCH` 是一个用于指导构建工具生成可重复输出的[标准化环境变量](https://reproducible-builds.org/docs/source-date-epoch/)。为构建设置该环境变量可以使镜像索引、配置和文件元数据中的时间戳反映指定的 Unix 时间。

To set the environment variable in GitHub Actions, use the built-in `env` property on the build step.

​	要在 GitHub Actions 中设置环境变量，可以在构建步骤中使用内置的 `env` 属性。

## Unix 纪元时间戳 Unix epoch timestamps

The following example sets the `SOURCE_DATE_EPOCH` variable to 0, Unix epoch.

​	以下示例将 `SOURCE_DATE_EPOCH` 变量设置为 0，即 Unix 纪元。

{{< tabpane text=true persist=disabled >}}

{{% tab header="docker/build-push-action" %}}

```yaml
name: ci

on:
  push:

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      
      - name: Build
        uses: docker/build-push-action@v6
        with:
          tags: user/app:latest
        env:
          SOURCE_DATE_EPOCH: 0
```

{{% /tab  %}}

{{% tab header="docker/bake-action" %}}

```yaml
name: ci

on:
  push:

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      
      - name: Build
        uses: docker/bake-action@v5
        env:
          SOURCE_DATE_EPOCH: 0
```

{{% /tab  %}}

{{< /tabpane >}}



## Git 提交时间戳 Git commit timestamps

The following example sets `SOURCE_DATE_EPOCH` to the Git commit timestamp.

​	以下示例将 `SOURCE_DATE_EPOCH` 设置为 Git 提交的时间戳。

{{< tabpane text=true persist=disabled >}}


{{% tab header="docker/build-push-action" %}}

```yaml
name: ci

on:
  push:

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      
      - name: Get Git commit timestamps
        run: echo "TIMESTAMP=$(git log -1 --pretty=%ct)" >> $GITHUB_ENV
      
      - name: Build
        uses: docker/build-push-action@v6
        with:
          tags: user/app:latest
        env:
          SOURCE_DATE_EPOCH: ${{ env.TIMESTAMP }}
```

{{% /tab  %}}

{{% tab header="docker/bake-action" %}}

```yaml
name: ci

on:
  push:

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      
      - name: Get Git commit timestamps
        run: echo "TIMESTAMP=$(git log -1 --pretty=%ct)" >> $GITHUB_ENV
      
      - name: Build
        uses: docker/bake-action@v5
        env:
          SOURCE_DATE_EPOCH: ${{ env.TIMESTAMP }}
```

{{% /tab  %}}

{{< /tabpane >}}



------

## 其他信息 Additional information

For more information about the `SOURCE_DATE_EPOCH` support in BuildKit, see [BuildKit documentation](https://github.com/moby/buildkit/blob/master/docs/build-repro.md#source_date_epoch).

​	有关 BuildKit 中 `SOURCE_DATE_EPOCH` 支持的更多信息，请参阅 [BuildKit 文档](https://github.com/moby/buildkit/blob/master/docs/build-repro.md#source_date_epoch)。
