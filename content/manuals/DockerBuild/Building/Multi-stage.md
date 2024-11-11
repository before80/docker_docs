+++
title = "多阶段"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/building/multi-stage/](https://docs.docker.com/build/building/multi-stage/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Multi-stage builds - 多阶段构建

Multi-stage builds are useful to anyone who has struggled to optimize Dockerfiles while keeping them easy to read and maintain.

​	多阶段构建对那些在优化 Dockerfile 的同时希望保持其可读性和易维护性的人非常有用。

## 使用多阶段构建 Use multi-stage builds

With multi-stage builds, you use multiple `FROM` statements in your Dockerfile. Each `FROM` instruction can use a different base, and each of them begins a new stage of the build. You can selectively copy artifacts from one stage to another, leaving behind everything you don't want in the final image.

​	通过多阶段构建，您可以在 Dockerfile 中使用多个 `FROM` 语句。每个 `FROM` 指令可以使用不同的基础镜像，并且每个指令都开始一个新的构建阶段。您可以选择性地将某个阶段的构建成果复制到另一个阶段，从而在最终镜像中保留所需内容，移除不需要的部分。

The following Dockerfile has two separate stages: one for building a binary, and another where the binary gets copied from the first stage into the next stage.

​	以下 Dockerfile 包含两个独立阶段：一个用于构建二进制文件，另一个将二进制文件从第一个阶段复制到下一个阶段。





```dockerfile
# syntax=docker/dockerfile:1
FROM golang:1.23
WORKDIR /src
COPY <<EOF ./main.go
package main

import "fmt"

func main() {
  fmt.Println("hello, world")
}
EOF
RUN go build -o /bin/hello ./main.go

FROM scratch
COPY --from=0 /bin/hello /bin/hello
CMD ["/bin/hello"]
```

You only need the single Dockerfile. No need for a separate build script. Just run `docker build`.

​	只需要一个 Dockerfile，不需要单独的构建脚本。只需运行 `docker build`。



```console
$ docker build -t hello .
```

The end result is a tiny production image with nothing but the binary inside. None of the build tools required to build the application are included in the resulting image.

​	最终结果是一个只包含二进制文件的微小生产镜像，不包括构建应用所需的任何构建工具。

How does it work? The second `FROM` instruction starts a new build stage with the `scratch` image as its base. The `COPY --from=0` line copies just the built artifact from the previous stage into this new stage. The Go SDK and any intermediate artifacts are left behind, and not saved in the final image.

​	工作原理是：第二个 `FROM` 指令以 `scratch` 镜像为基础开始一个新的构建阶段。`COPY --from=0` 行仅从前一个阶段复制构建的二进制文件到这个新阶段中。Go SDK 及中间构建产物都被舍弃，没有被保存在最终镜像中。

## 为构建阶段命名 Name your build stages

By default, the stages aren't named, and you refer to them by their integer number, starting with 0 for the first `FROM` instruction. However, you can name your stages, by adding an `AS <NAME>` to the `FROM` instruction. This example improves the previous one by naming the stages and using the name in the `COPY` instruction. This means that even if the instructions in your Dockerfile are re-ordered later, the `COPY` doesn't break.

​	默认情况下，阶段没有名称，您可以通过整数编号来引用它们，第一个 `FROM` 指令的编号为 0。但是，您可以通过在 `FROM` 指令中添加 `AS <NAME>` 来命名阶段。以下示例对前面的例子进行了改进，为每个阶段命名，并在 `COPY` 指令中使用该名称。这意味着即使 Dockerfile 的指令顺序后来被更改，`COPY` 仍然可以正常工作。





```dockerfile
# syntax=docker/dockerfile:1
FROM golang:1.23 AS build
WORKDIR /src
COPY <<EOF /src/main.go
package main

import "fmt"

func main() {
  fmt.Println("hello, world")
}
EOF
RUN go build -o /bin/hello ./main.go

FROM scratch
COPY --from=build /bin/hello /bin/hello
CMD ["/bin/hello"]
```

## 在特定构建阶段停止 Stop at a specific build stage

When you build your image, you don't necessarily need to build the entire Dockerfile including every stage. You can specify a target build stage. The following command assumes you are using the previous `Dockerfile` but stops at the stage named `build`:

​	构建镜像时，不一定需要构建整个 Dockerfile 中的每个阶段。可以指定一个目标构建阶段。以下命令使用前述 Dockerfile，但在名为 `build` 的阶段停止：





```console
$ docker build --target build -t hello .
```

A few scenarios where this might be useful are:

​	一些适用的场景包括：

- Debugging a specific build stage
  - 调试特定的构建阶段

- Using a `debug` stage with all debugging symbols or tools enabled, and a lean `production` stage
  - 使用启用了调试符号或工具的 `debug` 阶段和简化的 `production` 阶段

- Using a `testing` stage in which your app gets populated with test data, but building for production using a different stage which uses real data
  - 使用一个 `testing` 阶段将应用填充测试数据，而生产构建使用不同的阶段加载真实数据


## 使用外部镜像作为阶段 Use an external image as a stage

When using multi-stage builds, you aren't limited to copying from stages you created earlier in your Dockerfile. You can use the `COPY --from` instruction to copy from a separate image, either using the local image name, a tag available locally or on a Docker registry, or a tag ID. The Docker client pulls the image if necessary and copies the artifact from there. The syntax is:

​	在使用多阶段构建时，您不限于从 Dockerfile 中之前创建的阶段中复制内容。可以使用 `COPY --from` 指令从其他镜像中复制，无论是本地镜像名称、在本地或 Docker 注册表中的标签、或标签 ID。Docker 客户端会在必要时拉取镜像并复制所需内容，语法如下：





```dockerfile
COPY --from=nginx:latest /etc/nginx/nginx.conf /nginx.conf
```

## 将之前的阶段作为新阶段使用 Use a previous stage as a new stage

You can pick up where a previous stage left off by referring to it when using the `FROM` directive. For example:

​	您可以通过在 `FROM` 指令中引用先前的阶段来从该阶段停止的地方继续。例如：



```dockerfile
# syntax=docker/dockerfile:1

FROM alpine:latest AS builder
RUN apk --no-cache add build-base

FROM builder AS build1
COPY source1.cpp source.cpp
RUN g++ -o /binary source.cpp

FROM builder AS build2
COPY source2.cpp source.cpp
RUN g++ -o /binary source.cpp
```

## 传统构建器与 BuildKit 之间的差异 Differences between legacy builder and BuildKit

The legacy Docker Engine builder processes all stages of a Dockerfile leading up to the selected `--target`. It will build a stage even if the selected target doesn't depend on that stage.

​	传统 Docker 引擎构建器会处理 Dockerfile 中的所有阶段，直至选定的 `--target` 阶段，即使所选目标不依赖于该阶段，它也会被构建。

[BuildKit]({{< ref "/manuals/DockerBuild/BuildKit" >}}) only builds the stages that the target stage depends on.

​	[BuildKit]({{< ref "/manuals/DockerBuild/BuildKit" >}}) 仅构建目标阶段所依赖的阶段。

For example, given the following Dockerfile:

​	例如，以下 Dockerfile：



```dockerfile
# syntax=docker/dockerfile:1
FROM ubuntu AS base
RUN echo "base"

FROM base AS stage1
RUN echo "stage1"

FROM base AS stage2
RUN echo "stage2"
```

With [BuildKit enabled](https://docs.docker.com/build/buildkit/#getting-started), building the `stage2` target in this Dockerfile means only `base` and `stage2` are processed. There is no dependency on `stage1`, so it's skipped.

​	启用 [BuildKit](https://docs.docker.com/build/buildkit/#getting-started) 后，构建此 Dockerfile 中的 `stage2` 目标时，只处理 `base` 和 `stage2`。没有依赖 `stage1`，因此它被跳过。





```console
$ DOCKER_BUILDKIT=1 docker build --no-cache -f Dockerfile --target stage2 .
[+] Building 0.4s (7/7) FINISHED                                                                    
 => [internal] load build definition from Dockerfile                                            0.0s
 => => transferring dockerfile: 36B                                                             0.0s
 => [internal] load .dockerignore                                                               0.0s
 => => transferring context: 2B                                                                 0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                                0.0s
 => CACHED [base 1/2] FROM docker.io/library/ubuntu                                             0.0s
 => [base 2/2] RUN echo "base"                                                                  0.1s
 => [stage2 1/1] RUN echo "stage2"                                                              0.2s
 => exporting to image                                                                          0.0s
 => => exporting layers                                                                         0.0s
 => => writing image sha256:f55003b607cef37614f607f0728e6fd4d113a4bf7ef12210da338c716f2cfd15    0.0s
```

On the other hand, building the same target without BuildKit results in all stages being processed:

​	相反，在不使用 BuildKit 的情况下构建相同目标会处理所有阶段：





```console
$ DOCKER_BUILDKIT=0 docker build --no-cache -f Dockerfile --target stage2 .
Sending build context to Docker daemon  219.1kB
Step 1/6 : FROM ubuntu AS base
 ---> a7870fd478f4
Step 2/6 : RUN echo "base"
 ---> Running in e850d0e42eca
base
Removing intermediate container e850d0e42eca
 ---> d9f69f23cac8
Step 3/6 : FROM base AS stage1
 ---> d9f69f23cac8
Step 4/6 : RUN echo "stage1"
 ---> Running in 758ba6c1a9a3
stage1
Removing intermediate container 758ba6c1a9a3
 ---> 396baa55b8c3
Step 5/6 : FROM base AS stage2
 ---> d9f69f23cac8
Step 6/6 : RUN echo "stage2"
 ---> Running in bbc025b93175
stage2
Removing intermediate container bbc025b93175
 ---> 09fc3770a9c4
Successfully built 09fc3770a9c4
```

The legacy builder processes `stage1`, even if `stage2` doesn't depend on it.

​	传统构建器会处理 `stage1`，即使 `stage2` 不依赖于它。
