+++
title = "SDK"
date = 2024-10-23T14:54:43+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/reference/api/engine/sdk/](https://docs.docker.com/reference/api/engine/sdk/)
>
> 收录该文档的时间：`2024-10-23T14:54:43+08:00`

# Develop with Docker Engine SDKs

Docker provides an API for interacting with the Docker daemon (called the Docker Engine API), as well as SDKs for Go and Python. The SDKs allow you to efficiently build and scale Docker apps and solutions. If Go or Python don't work for you, you can use the Docker Engine API directly.

The Docker Engine API is a RESTful API accessed by an HTTP client such as `wget` or `curl`, or the HTTP library which is part of most modern programming languages.

## Install the SDKs

Use the following commands to install the Go or Python SDK. Both SDKs can be installed and coexist together.

### Go SDK



```console
$ go get github.com/docker/docker/client
```

The client requires a recent version of Go. Run `go version` and ensure that you're running a currently supported version of Go.

For more information, see [Docker Engine Go SDK reference](https://godoc.org/github.com/docker/docker/client).

### Python SDK

- Recommended: Run `pip install docker`.
- If you can't use `pip`:
  1. [Download the package directly](https://pypi.python.org/pypi/docker/).
  2. Extract it and change to the extracted directory.
  3. Run `python setup.py install`.

For more information, see [Docker Engine Python SDK reference](https://docker-py.readthedocs.io/).

## View the API reference

You can [view the reference for the latest version of the API](https://docs.docker.com/reference/api/engine/latest/) or [choose a specific version]({{< ref "/reference/APIreference/DockerEngineAPI/EngineAPIversionhistory" >}}).

## Versioned API and SDK

The version of the Docker Engine API you should use depends on the version of your Docker daemon and Docker client. See the [versioned API and SDK](https://docs.docker.com/reference/api/engine/#versioned-api-and-sdk) section in the API documentation for details.

## SDK and API quickstart

Use the following guidelines to choose the SDK or API version to use in your code:

- If you're starting a new project, use the [latest version](https://docs.docker.com/reference/api/engine/latest/), but use API version negotiation or specify the version you are using. This helps prevent surprises.
- If you need a new feature, update your code to use at least the minimum version that supports the feature, and prefer the latest version you can use.
- Otherwise, continue to use the version that your code is already using.

As an example, the `docker run` command can be implemented using the Docker API directly, or using the Python or Go SDK.

Go Python HTTP

------



```go
package main

import (
	"context"
	"io"
	"os"

	"github.com/docker/docker/api/types/container"
        "github.com/docker/docker/api/types/image"
	"github.com/docker/docker/client"
	"github.com/docker/docker/pkg/stdcopy"
)

func main() {
    ctx := context.Background()
    cli, err := client.NewClientWithOpts(client.FromEnv, client.WithAPIVersionNegotiation())
    if err != nil {
        panic(err)
    }
    defer cli.Close()

    reader, err := cli.ImagePull(ctx, "docker.io/library/alpine", image.PullOptions{})
    if err != nil {
        panic(err)
    }
    io.Copy(os.Stdout, reader)

    resp, err := cli.ContainerCreate(ctx, &container.Config{
        Image: "alpine",
        Cmd:   []string{"echo", "hello world"},
    }, nil, nil, nil, "")
    if err != nil {
        panic(err)
    }

    if err := cli.ContainerStart(ctx, resp.ID, container.StartOptions{}); err != nil {
        panic(err)
    }

    statusCh, errCh := cli.ContainerWait(ctx, resp.ID, container.WaitConditionNotRunning)
    select {
    case err := <-errCh:
        if err != nil {
            panic(err)
        }
    case <-statusCh:
    }

    out, err := cli.ContainerLogs(ctx, resp.ID, container.LogsOptions{ShowStdout: true})
    if err != nil {
        panic(err)
    }

    stdcopy.StdCopy(os.Stdout, os.Stderr, out)
}
```

------

For more examples, take a look at the [SDK examples]({{< ref "/reference/APIreference/DockerEngineAPI/SDK/Examples" >}}).

## Unofficial libraries

There are a number of community supported libraries available for other languages. They haven't been tested by Docker, so if you run into any issues, file them with the library maintainers.

| Language | Library                                                      |
| :------- | :----------------------------------------------------------- |
| C        | [libdocker](https://github.com/danielsuo/libdocker)          |
| C#       | [Docker.DotNet](https://github.com/ahmetalpbalkan/Docker.DotNet) |
| C++      | [lasote/docker_client](https://github.com/lasote/docker_client) |
| Clojure  | [clj-docker-client](https://github.com/into-docker/clj-docker-client) |
| Clojure  | [contajners](https://github.com/lispyclouds/contajners)      |
| Dart     | [bwu_docker](https://github.com/bwu-dart/bwu_docker)         |
| Erlang   | [erldocker](https://github.com/proger/erldocker)             |
| Gradle   | [gradle-docker-plugin](https://github.com/gesellix/gradle-docker-plugin) |
| Groovy   | [docker-client](https://github.com/gesellix/docker-client)   |
| Haskell  | [docker-hs](https://github.com/denibertovic/docker-hs)       |
| Java     | [docker-client](https://github.com/spotify/docker-client)    |
| Java     | [docker-java](https://github.com/docker-java/docker-java)    |
| Java     | [docker-java-api](https://github.com/amihaiemil/docker-java-api) |
| Java     | [jocker](https://github.com/ndeloof/jocker)                  |
| NodeJS   | [dockerode](https://github.com/apocas/dockerode)             |
| NodeJS   | [harbor-master](https://github.com/arhea/harbor-master)      |
| Perl     | [Eixo::Docker](https://github.com/alambike/eixo-docker)      |
| PHP      | [Docker-PHP](https://github.com/docker-php/docker-php)       |
| Ruby     | [docker-api](https://github.com/swipely/docker-api)          |
| Rust     | [bollard](https://github.com/fussybeaver/bollard)            |
| Rust     | [docker-rust](https://github.com/abh1nav/docker-rust)        |
| Rust     | [shiplift](https://github.com/softprops/shiplift)            |
| Scala    | [tugboat](https://github.com/softprops/tugboat)              |
| Scala    | [reactive-docker](https://github.com/almoehi/reactive-docker) |
| Swift    | [docker-client-swift](https://github.com/valeriomazzeo/docker-client-swift) |
