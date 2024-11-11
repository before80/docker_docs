+++
title = "Dockerfile 发布说明"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/buildkit/dockerfile-release-notes/](https://docs.docker.com/build/buildkit/dockerfile-release-notes/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Dockerfile release notes - Dockerfile 发布说明

This page contains information about the new features, improvements, known issues, and bug fixes in [Dockerfile reference]({{< ref "/reference/Dockerfilereference" >}}).

​	此页面包含有关 [Dockerfile 参考]({{< ref "/reference/Dockerfilereference" >}}) 的新功能、改进、已知问题和错误修复的信息。

For usage, see the [Dockerfile frontend syntax]({{< ref "/manuals/DockerBuild/BuildKit/CustomDockerfilesyntax" >}}) page.

​	有关用法，请参见 [Dockerfile 前端语法]({{< ref "/manuals/DockerBuild/BuildKit/CustomDockerfilesyntax" >}}) 页面。

## 1.10.0

*2024-09-10*

The full release note for this release is available [on GitHub](https://github.com/moby/buildkit/releases/tag/dockerfile%2F1.10.0).

​	此版本的完整发行说明可在 [GitHub 上查看](https://github.com/moby/buildkit/releases/tag/dockerfile%2F1.10.0)。



```dockerfile
# syntax=docker/dockerfile:1.10.0
```

- [Build secrets](https://docs.docker.com/build/building/secrets/#target) can now be mounted as environment variables using the `env=VARIABLE` option. [moby/buildkit#5215](https://github.com/moby/buildkit/pull/5215)

  - [构建机密](https://docs.docker.com/build/building/secrets/#target) 现在可以使用 `env=VARIABLE` 选项作为环境变量挂载。[moby/buildkit#5215](https://github.com/moby/buildkit/pull/5215)

- The [`# check` directive](https://docs.docker.com/reference/dockerfile/#check) now allows new experimental attribute for enabling experimental validation rules like `CopyIgnoredFile`. [moby/buildkit#5213](https://github.com/moby/buildkit/pull/5213)

  - [`# check` 指令](https://docs.docker.com/reference/dockerfile/#check) 现在允许启用实验验证规则的新实验属性，如 `CopyIgnoredFile`。[moby/buildkit#5213](https://github.com/moby/buildkit/pull/5213)

- Improve validation of unsupported modifiers for variable substitution. [moby/buildkit#5146](https://github.com/moby/buildkit/pull/5146)

  - 改进对变量替换不支持的修饰符的验证。[moby/buildkit#5146](https://github.com/moby/buildkit/pull/5146)

- `ADD` and `COPY` instructions now support variable interpolation for build arguments for the `--chmod` option values. [moby/buildkit#5151](https://github.com/moby/buildkit/pull/5151)

  - `ADD` 和 `COPY` 指令现在支持对 `--chmod` 选项值的构建参数进行变量插值。[moby/buildkit#5151](https://github.com/moby/buildkit/pull/5151)

- Improve validation of the `--chmod` option for `COPY` and `ADD` instructions. [moby/buildkit#5148](https://github.com/moby/buildkit/pull/5148)

  - 改进 `COPY` 和 `ADD` 指令的 `--chmod` 选项验证。[moby/buildkit#5148](https://github.com/moby/buildkit/pull/5148)

- Fix missing completions for size and destination attributes on mounts. [moby/buildkit#5245](https://github.com/moby/buildkit/pull/5245)

  - 修复挂载上大小和目标属性缺失的问题。[moby/buildkit#5245](https://github.com/moby/buildkit/pull/5245)

- OCI annotations are now set to the Dockerfile frontend release image. [moby/buildkit#5197](https://github.com/moby/buildkit/pull/5197)

  - 现在，Dockerfile 前端发布镜像中设置了 OCI 注释。[moby/buildkit#5197](https://github.com/moby/buildkit/pull/5197)

  

## 1.9.0

*2024-07-11*

The full release note for this release is available [on GitHub](https://github.com/moby/buildkit/releases/tag/dockerfile%2F1.9.0).

​	此版本的完整发行说明可在 [GitHub 上查看](https://github.com/moby/buildkit/releases/tag/dockerfile%2F1.9.0)。



```dockerfile
# syntax=docker/dockerfile:1.9.0
```

- Add new validation rules: 添加新的验证规则：
  - `SecretsUsedInArgOrEnv`
  - `InvalidDefaultArgInFrom`
  - `RedundantTargetPlatform`
  - `CopyIgnoredFile` (experimental)
  - `FromPlatformFlagConstDisallowed`
- Many performance improvements for working with big Dockerfiles. [moby/buildkit#5067](https://github.com/moby/buildkit/pull/5067/), [moby/buildkit#5029](https://github.com/moby/buildkit/pull/5029/)
  - 针对处理大型 Dockerfile 的多项性能改进。[moby/buildkit#5067](https://github.com/moby/buildkit/pull/5067/), [moby/buildkit#5029](https://github.com/moby/buildkit/pull/5029/)

- Fix possible panic when building Dockerfile without defined stages. [moby/buildkit#5150](https://github.com/moby/buildkit/pull/5150/)
  - 修复在没有定义阶段的 Dockerfile 中构建可能发生的 panic。[moby/buildkit#5150](https://github.com/moby/buildkit/pull/5150/)

- Fix incorrect JSON parsing that could cause some incorrect JSON values to pass without producing an error. [moby/buildkit#5107](https://github.com/moby/buildkit/pull/5107/)
  - 修复可能导致某些错误 JSON 值通过而不会产生错误的 JSON 解析问题。[moby/buildkit#5107](https://github.com/moby/buildkit/pull/5107/)

- Fix a regression where `COPY --link` with a destination path of `.` could fail. [moby/buildkit#5080](https://github.com/moby/buildkit/pull/5080/)
  - 修复 `COPY --link` 目标路径为 `.` 时可能失败的回归问题。[moby/buildkit#5080](https://github.com/moby/buildkit/pull/5080/)

- Fix validation of `ADD --checksum` when used with a Git URL. [moby/buildkit#5085](https://github.com/moby/buildkit/pull/5085/)
  - 修复使用 Git URL 时 `ADD --checksum` 的验证问题。[moby/buildkit#5085](https://github.com/moby/buildkit/pull/5085/)


## 1.8.1

*2024-06-18*

The full release note for this release is available [on GitHub](https://github.com/moby/buildkit/releases/tag/dockerfile%2F1.8.1).



```dockerfile
# syntax=docker/dockerfile:1.8.1
```

### Bug fixes and enhancements

- Fix handling of empty strings on variable expansion. [moby/buildkit#5052](https://github.com/moby/buildkit/pull/5052/)
- Improve formatting of build warnings. [moby/buildkit#5037](https://github.com/moby/buildkit/pull/5037/), [moby/buildkit#5045](https://github.com/moby/buildkit/pull/5045/), [moby/buildkit#5046](https://github.com/moby/buildkit/pull/5046/)
- Fix possible invalid output for `UndeclaredVariable` warning for multi-stage builds. [moby/buildkit#5048](https://github.com/moby/buildkit/pull/5048/)

## 1.8.0

*2024-06-11*

The full release note for this release is available [on GitHub](https://github.com/moby/buildkit/releases/tag/dockerfile%2F1.8.0).



```dockerfile
# syntax=docker/dockerfile:1.8.0
```

- Many new validation rules have been added to verify that your Dockerfile is using best practices. These rules are validated during build and new `check` frontend method can be used to only trigger validation without completing the whole build.
- New directive `#check` and build argument `BUILDKIT_DOCKERFILE_CHECK` lets you control the behavior or build checks. [moby/buildkit#4962](https://github.com/moby/buildkit/pull/4962/)
- Using a single-platform base image that does not match your expected platform is now validated. [moby/buildkit#4924](https://github.com/moby/buildkit/pull/4924/)
- Errors from the expansion of `ARG` definitions in global scope are now handled properly. [moby/buildkit#4856](https://github.com/moby/buildkit/pull/4856/)
- Expansion of the default value of `ARG` now only happens if it is not overwritten by the user. Previously, expansion was completed and value was later ignored, which could result in an unexpected expansion error. [moby/buildkit#4856](https://github.com/moby/buildkit/pull/4856/)
- Performance of parsing huge Dockerfiles with many stages has been improved. [moby/buildkit#4970](https://github.com/moby/buildkit/pull/4970/)
- Fix some Windows path handling consistency errors. [moby/buildkit#4825](https://github.com/moby/buildkit/pull/4825/)

## 1.7.0

*2024-03-06*

### Stable



```dockerfile
# syntax=docker/dockerfile:1.7
```

- Variable expansion now allows string substitutions and trimming. [moby/buildkit#4427](https://github.com/moby/buildkit/pull/4427), [moby/buildkit#4287](https://github.com/moby/buildkit/pull/4287)
- Named contexts with local sources now correctly transfer only the files used in the Dockerfile instead of the full source directory. [moby/buildkit#4161](https://github.com/moby/buildkit/pull/4161)
- Dockerfile now better validates the order of stages and returns nice errors with stack traces if stages are in incorrect order. [moby/buildkit#4568](https://github.com/moby/buildkit/pull/4568), [moby/buildkit#4567](https://github.com/moby/buildkit/pull/4567)
- History commit messages now contain flags used with `COPY` and `ADD`. [moby/buildkit#4597](https://github.com/moby/buildkit/pull/4597)
- Progress messages for `ADD` commands from Git and HTTP sources have been improved. [moby/buildkit#4408](https://github.com/moby/buildkit/pull/4408)

### Labs



```dockerfile
# syntax=docker/dockerfile:1.7-labs
```

- New `--parents` flag has been added to `COPY` for copying files while keeping the parent directory structure. [moby/buildkit#4598](https://github.com/moby/buildkit/pull/4598), [moby/buildkit#3001](https://github.com/moby/buildkit/pull/3001), [moby/buildkit#4720](https://github.com/moby/buildkit/pull/4720), [moby/buildkit#4728](https://github.com/moby/buildkit/pull/4728), [docs](https://docs.docker.com/reference/dockerfile/#copy---parents)
- New `--exclude` flag can be used in `COPY` and `ADD` commands to apply filter to copied files. [moby/buildkit#4561](https://github.com/moby/buildkit/pull/4561), [docs](https://docs.docker.com/reference/dockerfile/#copy---exclude)

## 1.6.0

*2023-06-13*

### New

- Add `--start-interval` flag to the [`HEALTHCHECK` instruction](https://docs.docker.com/reference/dockerfile/#healthcheck).

The following features have graduated from the labs channel to stable:

- The `ADD` instruction can now [import files directly from Git URLs](https://docs.docker.com/reference/dockerfile/#adding-a-git-repository-add-git-ref-dir)
- The `ADD` instruction now supports [`--checksum` flag](https://docs.docker.com/reference/dockerfile/#verifying-a-remote-file-checksum-add---checksumchecksum-http-src-dest) to validate the contents of the remote URL contents

### Bug fixes and enhancements

- Variable substitution now supports additional POSIX compatible variants without `:`. [moby/buildkit#3611](https://github.com/moby/buildkit/pull/3611)
- Exported Windows images now contain OSVersion and OSFeatures values from base image. [moby/buildkit#3619](https://github.com/moby/buildkit/pull/3619)
- Changed the permissions for Heredocs to 0644. [moby/buildkit#3992](https://github.com/moby/buildkit/pull/3992)

## 1.5.2

*2023-02-14*

### Bug fixes and enhancements

- Fix building from Git reference that is missing branch name but contains a subdir
- 386 platform image is now included in the release

## 1.5.1

*2023-01-18*

### Bug fixes and enhancements

- Fix possible panic when warning conditions appear in multi-platform builds

## 1.5.0 (labs)

*2023-01-10*

**Experimental**

The "labs" channel provides early access to Dockerfile features that are not yet available in the stable channel.

### New

- `ADD` command now supports [`--checksum` flag](https://docs.docker.com/reference/dockerfile/#verifying-a-remote-file-checksum-add---checksumchecksum-http-src-dest) to validate the contents of the remote URL contents

## 1.5.0

*2023-01-10*

### New

- `ADD` command can now [import files directly from Git URLs](https://docs.docker.com/reference/dockerfile/#adding-a-git-repository-add-git-ref-dir)

### Bug fixes and enhancements

- Named contexts now support `oci-layout://` protocol for including images from local OCI layout structure
- Dockerfile now supports secondary requests for listing all build targets or printing outline of accepted parameters for a specific build target
- Dockerfile `#syntax` directive that redirects to an external frontend image now allows the directive to be also set with `//` comments or JSON. The file may also contain a shebang header
- Named context can now be initialized with an empty scratch image
- Named contexts can now be initialized with an SSH Git URL
- Fix handling of `ONBUILD` when importing Schema1 images

## 1.4.3

*2022-08-23*

### Bug fixes and enhancements

- Fix creation timestamp not getting reset when building image from `docker-image://` named context
- Fix passing `--platform` flag of `FROM` command when loading `docker-image://` named context

## 1.4.2

*2022-05-06*

### Bug fixes and enhancements

- Fix loading certain environment variables from an image passed with built context

## 1.4.1

*2022-04-08*

### Bug fixes and enhancements

- Fix named context resolution for cross-compilation cases from input when input is built for a different platform

## 1.4.0

*2022-03-09*

### New

- [`COPY --link` and `ADD --link`](https://docs.docker.com/reference/dockerfile/#copy---link) allow copying files with increased cache efficiency and rebase images without requiring them to be rebuilt. `--link` copies files to a separate layer and then uses new LLB MergeOp implementation to chain independent layers together
- [Heredocs](https://docs.docker.com/reference/dockerfile/#here-documents) support have been promoted from labs channel to stable. This feature allows writing multiline inline scripts and files
- Additional [named build contexts](https://docs.docker.com/reference/cli/docker/buildx/build/#build-context) can be passed to build to add or overwrite a stage or an image inside the build. A source for the context can be a local source, image, Git, or HTTP URL
- [`BUILDKIT_SANDBOX_HOSTNAME` build-arg](https://docs.docker.com/reference/dockerfile/#buildkit-built-in-build-args) can be used to set the default hostname for the `RUN` steps

### Bug fixes and enhancements

- When using a cross-compilation stage, the target platform for a step is now seen on progress output
- Fix some cases where Heredocs incorrectly removed quotes from content

## 1.3.1

*2021-10-04*

### Bug fixes and enhancements

- Fix parsing "required" mount key without a value

## 1.3.0 (labs)

*2021-07-16*

**Experimental**

The "labs" channel provides early access to Dockerfile features that are not yet available in the stable channel.

### New

- `RUN` and `COPY` commands now support [Here-document syntax](https://docs.docker.com/reference/dockerfile/#here-documents) allowing writing multiline inline scripts and files

## 1.3.0

*2021-07-16*

### New

- `RUN` command allows [`--network` flag](https://docs.docker.com/reference/dockerfile/#run---network) for requesting a specific type of network conditions. `--network=host` requires allowing `network.host` entitlement. This feature was previously only available on labs channel

### Bug fixes and enhancements

- `ADD` command with a remote URL input now correctly handles the `--chmod` flag
- Values for [`RUN --mount` flag](https://docs.docker.com/reference/dockerfile/#run---mount) now support variable expansion, except for the `from` field
- Allow [`BUILDKIT_MULTI_PLATFORM` build arg](https://docs.docker.com/reference/dockerfile/#buildkit-built-in-build-args) to force always creating multi-platform image, even if only contains single platform

## 1.2.1 (labs)

*2020-12-12*

**Experimental**

The "labs" channel provides early access to Dockerfile features that are not yet available in the stable channel.

### Bug fixes and enhancements

- `RUN` command allows [`--network` flag](https://docs.docker.com/reference/dockerfile/#run---network) for requesting a specific type of network conditions. `--network=host` requires allowing `network.host` entitlement

## 1.2.1

*2020-12-12*

### Bug fixes and enhancements

- Revert "Ensure ENTRYPOINT command has at least one argument"
- Optimize processing `COPY` calls on multi-platform cross-compilation builds

## 1.2.0 (labs)

*2020-12-03*

**Experimental**

The "labs" channel provides early access to Dockerfile features that are not yet available in the stable channel.

### Bug fixes and enhancements

- Experimental channel has been renamed to *labs*

## 1.2.0

*2020-12-03*

### New

- [`RUN --mount` syntax](https://docs.docker.com/reference/dockerfile/#run---mount) for creating secret, ssh, bind, and cache mounts have been moved to mainline channel
- [`ARG` command](https://docs.docker.com/reference/dockerfile/#arg) now supports defining multiple build args on the same line similarly to `ENV`

### Bug fixes and enhancements

- Metadata load errors are now handled as fatal to avoid incorrect build results
- Allow lowercase Dockerfile name
- `--chown` flag in `ADD` now allows parameter expansion
- `ENTRYPOINT` requires at least one argument to avoid creating broken images

## 1.1.7

*2020-04-18*

### Bug fixes and enhancements

- Forward `FrontendInputs` to the gateway

## 1.1.2 (experimental)

*2019-07-31*

**Experimental**

The "labs" channel provides early access to Dockerfile features that are not yet available in the stable channel.

### Bug fixes and enhancements

- Allow setting security mode for a process with `RUN --security=sandbox|insecure`
- Allow setting uid/gid for [cache mounts](https://docs.docker.com/reference/dockerfile/#run---mounttypecache)
- Avoid requesting internally linked paths to be pulled to build context
- Ensure missing cache IDs default to target paths
- Allow setting namespace for cache mounts with [`BUILDKIT_CACHE_MOUNT_NS` build arg](https://docs.docker.com/reference/dockerfile/#buildkit-built-in-build-args)

## 1.1.2

*2019-07-31*

### Bug fixes and enhancements

- Fix workdir creation with correct user and don't reset custom ownership
- Fix handling empty build args also used as `ENV`
- Detect circular dependencies

## 1.1.0

*2019-04-27*

### New

- `ADD/COPY` commands now support implementation based on `llb.FileOp` and do not require helper image if builtin file operations support is available
- `--chown` flag for `COPY` command now supports variable expansion

### Bug fixes and enhancements

- To find the files ignored from the build context Dockerfile frontend will first look for a file `<path/to/Dockerfile>.dockerignore` and if it is not found `.dockerignore` file will be looked up from the root of the build context. This allows projects with multiple Dockerfiles to use different `.dockerignore` definitions
