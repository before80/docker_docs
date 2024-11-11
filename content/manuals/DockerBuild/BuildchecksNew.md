+++
title = "Build checks New"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/build/checks/](https://docs.docker.com/build/checks/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Checking your build configuration - 检查构建配置

Introduced in Buildx version 0.15.0

​	在 Buildx 版本 0.15.0 中引入

Build checks are a feature introduced in Dockerfile 1.8. It lets you validate your build configuration and conduct a series of checks prior to executing your build. Think of it as an advanced form of linting for your Dockerfile and build options, or a dry-run mode for builds.

​	构建检查是 Dockerfile 1.8 中引入的一项功能。它允许您在执行构建之前验证构建配置并进行一系列检查。可以将其视为 Dockerfile 和构建选项的高级代码检查或构建的模拟运行模式。

You can find the list of checks available, and a description of each, in the [Build checks reference]({{< ref "/reference/Buildchecks" >}}).

​	您可以在[构建检查参考]({{< ref "/reference/Buildchecks" >}})中找到可用检查的列表及其描述。

## 构建检查的工作原理 How build checks work

Typically, when you run a build, Docker executes the build steps in your Dockerfile and build options as specified. With build checks, rather than executing the build steps, Docker checks the Dockerfile and options you provide and reports any issues it detects.

​	通常，当您运行构建时，Docker 会按照指定的 Dockerfile 和构建选项执行构建步骤。使用构建检查时，Docker 不会执行构建步骤，而是检查提供的 Dockerfile 和选项并报告发现的问题。

Build checks are useful for:

​	构建检查的用途包括：

- Validating your Dockerfile and build options before running a build.
  - 在运行构建之前验证您的 Dockerfile 和构建选项。

- Ensuring that your Dockerfile and build options are up-to-date with the latest best practices.
  - 确保您的 Dockerfile 和构建选项符合最新的最佳实践。

- Identifying potential issues or anti-patterns in your Dockerfile and build options.
  - 识别 Dockerfile 和构建选项中的潜在问题或反模式。


## 带有检查的构建 Build with checks

Build checks are supported in:

​	构建检查适用于：

- Buildx version 0.15.0 and later
  - Buildx 版本 0.15.0 及更高版本

- [docker/build-push-action](https://github.com/docker/build-push-action) version 6.6.0 and later
  - [docker/build-push-action](https://github.com/docker/build-push-action) 版本 6.6.0 及更高版本

- [docker/bake-action](https://github.com/docker/bake-action) version 5.6.0 and later
  - [docker/bake-action](https://github.com/docker/bake-action) 版本 5.6.0 及更高版本


Invoking a build runs the checks by default, and displays any violations in the build output. For example, the following command both builds the image and runs the checks:

​	运行构建时默认会执行检查，并在构建输出中显示任何违规情况。例如，以下命令既构建镜像又执行检查：

```console
$ docker build .
[+] Building 3.5s (11/11) FINISHED
...

1 warning found (use --debug to expand):
  - Lint Rule 'JSONArgsRecommended': JSON arguments recommended for CMD to prevent unintended behavior related to OS signals (line 7)
```

In this example, the build ran successfully, but a [JSONArgsRecommended]({{< ref "/reference/Buildchecks/JSONArgsRecommended" >}}) warning was reported, because `CMD` instructions should use JSON array syntax.

​	在此示例中，构建成功完成，但报告了一个 [JSONArgsRecommended]({{< ref "/reference/Buildchecks/JSONArgsRecommended" >}}) 警告，因为 `CMD` 指令应使用 JSON 数组语法。

With the GitHub Actions, the checks display in the diff view of pull requests.

​	在 GitHub Actions 中，检查会在拉取请求的差异视图中显示。

```yaml
name: Build and push Docker images
on:
  push:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Build and push
        uses: docker/build-push-action@v6.6.0
```

![GitHub Actions build check annotations](BuildchecksNew_img/gha-check-annotations.png)

### 更详细的输出 More verbose output

Check warnings for a regular `docker build` display a concise message containing the rule name, the message, and the line number of where in the Dockerfile the issue originated. If you want to see more detailed information about the checks, you can use the `--debug` flag. For example:

​	对于常规 `docker build`，检查警告会显示包含规则名称、消息和问题源自的 Dockerfile 行号的简要信息。如果您希望看到更详细的检查信息，可以使用 `--debug` 标志。例如：

```console
$ docker --debug build .
[+] Building 3.5s (11/11) FINISHED
...

 1 warning found:
 - JSONArgsRecommended: JSON arguments recommended for CMD to prevent unintended behavior related to OS signals (line 4)
JSON arguments recommended for ENTRYPOINT/CMD to prevent unintended behavior related to OS signals
More info: https://docs.docker.com/go/dockerfile/rule/json-args-recommended/
Dockerfile:4
	JSONArgsRecommended：建议使用 JSON 参数来防止与操作系统信号相关的意外行为（第 4 行）
建议在 ENTRYPOINT/CMD 中使用 JSON 参数以防止与操作系统信号相关的意外行为
更多信息：https://docs.docker.com/go/dockerfile/rule/json-args-recommended/
Dockerfile:4
--------------------
   2 |
   3 |     FROM alpine
   4 | >>> CMD echo "Hello, world!"
   5 |
--------------------
```

With the `--debug` flag, the output includes a link to the documentation for the check, and a snippet of the Dockerfile where the issue was found.

​	使用 `--debug` 标志时，输出包括检查文档的链接以及发现问题的 Dockerfile 片段。

## 检查构建而不实际构建 Check a build without building

To run build checks without actually building, you can use the `docker build` command as you typically would, but with the addition of the `--check` flag. Here's an example:

​	要在不实际构建的情况下运行构建检查，您可以像往常一样使用 `docker build` 命令，但添加 `--check` 标志。例如：

```console
$ docker build --check .
```

Instead of executing the build steps, this command only runs the checks and reports any issues it finds. If there are any issues, they will be reported in the output. For example:

​	此命令不执行构建步骤，只运行检查并报告发现的任何问题。如果有问题，它们会在输出中报告。例如：

Output with `--check`

​	使用 `--check` 输出

```text
[+] Building 1.5s (5/5) FINISHED
=> [internal] connecting to local controller
=> [internal] load build definition from Dockerfile
=> => transferring dockerfile: 253B
=> [internal] load metadata for docker.io/library/node:22
=> [auth] library/node:pull token for registry-1.docker.io
=> [internal] load .dockerignore
=> => transferring context: 50B
JSONArgsRecommended - https://docs.docker.com/go/dockerfile/rule/json-args-recommended/
JSON arguments recommended for ENTRYPOINT/CMD to prevent unintended behavior related to OS signals
Dockerfile:7
--------------------
5 |
6 |     COPY index.js .
7 | >>> CMD node index.js
8 |
--------------------
```

This output with `--check` shows the [verbose message](https://docs.docker.com/build/checks/#more-verbose-output) for the check.

​	此 `--check` 输出显示了检查的[详细信息](https://docs.docker.com/build/checks/#more-verbose-output)。

Unlike a regular build, if any violations are reported when using the `--check` flag, the command exits with a non-zero status code.

​	与常规构建不同，使用 `--check` 标志时，如果报告了任何违规情况，命令会以非零状态码退出。

## 违反检查失败构建 Fail build on check violations

Check violations for builds are reported as warnings, with exit code 0, by default. You can configure Docker to fail the build when violations are reported, using a `check=error=true` directive in your Dockerfile. This will cause the build to error out when after the build checks are run, before the actual build gets executed.

​	默认情况下，构建检查违规会作为警告报告，退出代码为 0。您可以通过在 Dockerfile 中使用 `check=error=true` 指令来配置 Docker，在报告违规时使构建失败。这将在构建检查运行后、实际构建执行之前导致构建错误。

Dockerfile

```dockerfile
# syntax=docker/dockerfile:1
# check=error=true

FROM alpine
CMD echo "Hello, world!"
```

Without the `# check=error=true` directive, this build would complete with an exit code of 0. However, with the directive, build check violation results in non-zero exit code:

​	如果没有 `# check=error=true` 指令，此构建会以退出代码 0 完成。但有该指令时，构建检查违规会导致非零退出码：

```console
$ docker build .
[+] Building 1.5s (5/5) FINISHED
...

 1 warning found (use --debug to expand):
 - JSONArgsRecommended: JSON arguments recommended for CMD to prevent unintended behavior related to OS signals (line 5)
Dockerfile:1
 - JSONArgsRecommended：建议使用 JSON 参数来防止与操作系统信号相关的意外行为（第 5 行）
Dockerfile:1
--------------------
   1 | >>> # syntax=docker/dockerfile:1
   2 |     # check=error=true
   3 |
--------------------
ERROR: lint violation found for rules: JSONArgsRecommended
$ echo $?
1
```

You can also set the error directive on the CLI by passing the `BUILDKIT_DOCKERFILE_CHECK` build argument:

​	您也可以通过传递 `BUILDKIT_DOCKERFILE_CHECK` 构建参数在 CLI 上设置错误指令：

```console
$ docker build --check --build-arg "BUILDKIT_DOCKERFILE_CHECK=error=true" .
```

## 跳过检查 Skip checks

By default, all checks are run when you build an image. If you want to skip specific checks, you can use the `check=skip` directive in your Dockerfile. The `skip` parameter takes a CSV string of the check IDs you want to skip. For example:

​	默认情况下，构建镜像时会运行所有检查。如果要跳过特定检查，可以在 Dockerfile 中使用 `check=skip` 指令。`skip` 参数接受要跳过的检查 ID 的 CSV 字符串。例如：

Dockerfile



```dockerfile
# syntax=docker/dockerfile:1
# check=skip=JSONArgsRecommended,StageNameCasing

FROM alpine AS BASE_STAGE
CMD echo "Hello, world!"
```

Building this Dockerfile results in no check violations.

​	构建此 Dockerfile 不会出现检查违规。

You can also skip checks by passing the `BUILDKIT_DOCKERFILE_CHECK` build argument with a CSV string of check IDs you want to skip. For example:

​	您也可以通过传递 `BUILDKIT_DOCKERFILE_CHECK` 构建参数，使用 CSV 字符串跳过检查。例如：

```console
$ docker build --check --build-arg "BUILDKIT_DOCKERFILE_CHECK=skip=JSONArgsRecommended,StageNameCasing" .
```

To skip all checks, use the `skip=all` parameter:

​	要跳过所有检查，请使用 `skip=all` 参数：

Dockerfile



```dockerfile
# syntax=docker/dockerfile:1
# check=skip=all
```

## 组合错误和跳过参数用于检查指令 Combine error and skip parameters for check directives

To both skip specific checks and error on check violations, pass both the `skip` and `error` parameters separated by a semi-colon (`;`) to the `check` directive in your Dockerfile or in a build argument. For example:

​	要同时跳过特定检查并在检查违规时失败，可以将 `skip` 和 `error` 参数用分号（`;`）分隔传递给 Dockerfile 中的 `check` 指令或构建参数。例如：

Dockerfile



```dockerfile
# syntax=docker/dockerfile:1
# check=skip=JSONArgsRecommended,StageNameCasing;error=true
```

Build argument

​	构建参数

```console
$ docker build --check --build-arg "BUILDKIT_DOCKERFILE_CHECK=skip=JSONArgsRecommended,StageNameCasing;error=true" .
```

## 实验性检查 Experimental checks

Before checks are promoted to stable, they may be available as experimental checks. Experimental checks are disabled by default. To see the list of experimental checks available, refer to the [Build checks reference]({{< ref "/reference/Buildchecks" >}}).

​	在检查被提升为稳定版之前，它们可能作为实验性检查提供。实验性检查默认禁用。要查看可用的实验性检查列表，请参阅[构建检查参考]({{< ref "/reference/Buildchecks" >}})。

To enable all experimental checks, set the `BUILDKIT_DOCKERFILE_CHECK` build argument to `experimental=all`:

​	要启用所有实验性检查，请将 `BUILDKIT_DOCKERFILE_CHECK` 构建参数设置为 `experimental=all`：

```console
$ docker build --check --build-arg "BUILDKIT_DOCKERFILE_CHECK=experimental=all" .
```

You can also enable experimental checks in your Dockerfile using the `check` directive:

​	您还可以在 Dockerfile 中使用 `check` 指令启用实验性检查：

Dockerfile



```dockerfile
# syntax=docker/dockerfile:1
# check=experimental=all
```

To selectively enable experimental checks, you can pass a CSV string of the check IDs you want to enable, either to the `check` directive in your Dockerfile or as a build argument. For example:

​	要有选择地启用实验性检查，可以传递要启用的检查 ID 的 CSV 字符串给 Dockerfile 中的 `check` 指令或构建参数。例如：

Dockerfile



```dockerfile
# syntax=docker/dockerfile:1
# check=experimental=JSONArgsRecommended,StageNameCasing
```

Note that the `experimental` directive takes precedence over the `skip` directive, meaning that experimental checks will run regardless of the `skip` directive you have set. For example, if you set `skip=all` and enable experimental checks, the experimental checks will still run:

​	请注意，`experimental` 指令的优先级高于 `skip` 指令，这意味着即使您设置了 `skip` 指令，实验性检查也会运行。例如，如果您设置了 `skip=all` 并启用了实验性检查，实验性检查仍会执行：

Dockerfile



```dockerfile
# syntax=docker/dockerfile:1
# check=skip=all;experimental=all
```
