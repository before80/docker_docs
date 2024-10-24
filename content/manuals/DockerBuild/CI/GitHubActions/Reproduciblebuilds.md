+++
title = "Reproducible builds"
date = 2024-10-23T14:54:40+08:00
weight = 120
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/build/ci/github-actions/reproducible-builds/](https://docs.docker.com/build/ci/github-actions/reproducible-builds/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Reproducible builds with GitHub Actions

`SOURCE_DATE_EPOCH` is a [standardized environment variable](https://reproducible-builds.org/docs/source-date-epoch/) for instructing build tools to produce a reproducible output. Setting the environment variable for a build makes the timestamps in the image index, config, and file metadata reflect the specified Unix time.

To set the environment variable in GitHub Actions, use the built-in `env` property on the build step.

## [Unix epoch timestamps](https://docs.docker.com/build/ci/github-actions/reproducible-builds/#unix-epoch-timestamps)

The following example sets the `SOURCE_DATE_EPOCH` variable to 0, Unix epoch.

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



## [Git commit timestamps](https://docs.docker.com/build/ci/github-actions/reproducible-builds/#git-commit-timestamps)

The following example sets `SOURCE_DATE_EPOCH` to the Git commit timestamp.

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

## [Additional information](https://docs.docker.com/build/ci/github-actions/reproducible-builds/#additional-information)

For more information about the `SOURCE_DATE_EPOCH` support in BuildKit, see [BuildKit documentation](https://github.com/moby/buildkit/blob/master/docs/build-repro.md#source_date_epoch).
