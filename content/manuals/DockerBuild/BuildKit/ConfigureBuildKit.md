+++
title = "配置 BuildKit"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/buildkit/configure/](https://docs.docker.com/build/buildkit/configure/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Configure BuildKit - 配置 BuildKit

If you create a `docker-container` or `kubernetes` builder with Buildx, you can apply a custom [BuildKit configuration]({{< ref "/manuals/DockerBuild/BuildKit/buildkitd_toml" >}}) by passing the [`--config` flag](https://docs.docker.com/reference/cli/docker/buildx/create/#config) to the `docker buildx create` command.

​	如果您使用 Buildx 创建 `docker-container` 或 `kubernetes` 构建器，可以通过向 `docker buildx create` 命令传递 [`--config` 参数](https://docs.docker.com/reference/cli/docker/buildx/create/#config) 来应用自定义的 [BuildKit 配置]({{< ref "/manuals/DockerBuild/BuildKit/buildkitd_toml" >}})。

## 注册表镜像 Registry mirror

You can define a registry mirror to use for your builds. Doing so redirects BuildKit to pull images from a different hostname. The following steps exemplify defining a mirror for `docker.io` (Docker Hub) to `mirror.gcr.io`.

​	您可以为构建定义一个注册表镜像，这样可以将 BuildKit 重定向到不同的主机名拉取镜像。以下步骤演示了如何为 `docker.io`（Docker Hub）定义一个指向 `mirror.gcr.io` 的镜像。

1. Create a TOML at `/etc/buildkitd.toml` with the following content:

   在 `/etc/buildkitd.toml` 创建一个 TOML 文件，内容如下：

   ```toml
   debug = true
   [registry."docker.io"]
     mirrors = ["mirror.gcr.io"]
   ```

   > **Note**
   >
   > 
   >
   > `debug = true` turns on debug requests in the BuildKit daemon, which logs a message that shows when a mirror is being used.
   >
   > ​	`debug = true` 会在 BuildKit 守护进程中开启调试请求，以记录使用镜像时的日志。

2. Create a `docker-container` builder that uses this BuildKit configuration:

   创建一个使用该 BuildKit 配置的 `docker-container` 构建器：

   ```console
   $ docker buildx create --use --bootstrap \
     --name mybuilder \
     --driver docker-container \
     --config /etc/buildkitd.toml
   ```

3. Build an image:

   构建一个镜像：

   ```bash
   docker buildx build --load . -f - <<EOF
   FROM alpine
   RUN echo "hello world"
   EOF
   ```

The BuildKit logs for this builder now shows that it uses the GCR mirror. You can tell by the fact that the response messages include the `x-goog-*` HTTP headers.

​	现在，这个构建器的 BuildKit 日志会显示其使用 GCR 镜像，可以通过返回消息中包含 `x-goog-*` HTTP 头来确认。

```console
$ docker logs buildx_buildkit_mybuilder0
```



```text
...
time="2022-02-06T17:47:48Z" level=debug msg="do request" request.header.accept="application/vnd.docker.container.image.v1+json, */*" request.header.user-agent=containerd/1.5.8+unknown request.method=GET spanID=9460e5b6e64cec91 traceID=b162d3040ddf86d6614e79c66a01a577
time="2022-02-06T17:47:48Z" level=debug msg="fetch response received" response.header.accept-ranges=bytes response.header.age=1356 response.header.alt-svc="h3=\":443\"; ma=2592000,h3-29=\":443\"; ma=2592000,h3-Q050=\":443\"; ma=2592000,h3-Q046=\":443\"; ma=2592000,h3-Q043=\":443\"; ma=2592000,quic=\":443\"; ma=2592000; v=\"46,43\"" response.header.cache-control="public, max-age=3600" response.header.content-length=1469 response.header.content-type=application/octet-stream response.header.date="Sun, 06 Feb 2022 17:25:17 GMT" response.header.etag="\"774380abda8f4eae9a149e5d5d3efc83\"" response.header.expires="Sun, 06 Feb 2022 18:25:17 GMT" response.header.last-modified="Wed, 24 Nov 2021 21:07:57 GMT" response.header.server=UploadServer response.header.x-goog-generation=1637788077652182 response.header.x-goog-hash="crc32c=V3DSrg==" response.header.x-goog-hash.1="md5=d0OAq9qPTq6aFJ5dXT78gw==" response.header.x-goog-metageneration=1 response.header.x-goog-storage-class=STANDARD response.header.x-goog-stored-content-encoding=identity response.header.x-goog-stored-content-length=1469 response.header.x-guploader-uploadid=ADPycduqQipVAXc3tzXmTzKQ2gTT6CV736B2J628smtD1iDytEyiYCgvvdD8zz9BT1J1sASUq9pW_ctUyC4B-v2jvhIxnZTlKg response.status="200 OK" spanID=9460e5b6e64cec91 traceID=b162d3040ddf86d6614e79c66a01a577
time="2022-02-06T17:47:48Z" level=debug msg="fetch response received" response.header.accept-ranges=bytes response.header.age=760 response.header.alt-svc="h3=\":443\"; ma=2592000,h3-29=\":443\"; ma=2592000,h3-Q050=\":443\"; ma=2592000,h3-Q046=\":443\"; ma=2592000,h3-Q043=\":443\"; ma=2592000,quic=\":443\"; ma=2592000; v=\"46,43\"" response.header.cache-control="public, max-age=3600" response.header.content-length=1471 response.header.content-type=application/octet-stream response.header.date="Sun, 06 Feb 2022 17:35:13 GMT" response.header.etag="\"35d688bd15327daafcdb4d4395e616a8\"" response.header.expires="Sun, 06 Feb 2022 18:35:13 GMT" response.header.last-modified="Wed, 24 Nov 2021 21:07:12 GMT" response.header.server=UploadServer response.header.x-goog-generation=1637788032100793 response.header.x-goog-hash="crc32c=aWgRjA==" response.header.x-goog-hash.1="md5=NdaIvRUyfar8201DleYWqA==" response.header.x-goog-metageneration=1 response.header.x-goog-storage-class=STANDARD response.header.x-goog-stored-content-encoding=identity response.header.x-goog-stored-content-length=1471 response.header.x-guploader-uploadid=ADPycdtR-gJYwC7yHquIkJWFFG8FovDySvtmRnZBqlO3yVDanBXh_VqKYt400yhuf0XbQ3ZMB9IZV2vlcyHezn_Pu3a1SMMtiw response.status="200 OK" spanID=9460e5b6e64cec91 traceID=b162d3040ddf86d6614e79c66a01a577
time="2022-02-06T17:47:48Z" level=debug msg=fetch spanID=9460e5b6e64cec91 traceID=b162d3040ddf86d6614e79c66a01a577
time="2022-02-06T17:47:48Z" level=debug msg=fetch spanID=9460e5b6e64cec91 traceID=b162d3040ddf86d6614e79c66a01a577
time="2022-02-06T17:47:48Z" level=debug msg=fetch spanID=9460e5b6e64cec91 traceID=b162d3040ddf86d6614e79c66a01a577
time="2022-02-06T17:47:48Z" level=debug msg=fetch spanID=9460e5b6e64cec91 traceID=b162d3040ddf86d6614e79c66a01a577
time="2022-02-06T17:47:48Z" level=debug msg="do request" request.header.accept="application/vnd.docker.image.rootfs.diff.tar.gzip, */*" request.header.user-agent=containerd/1.5.8+unknown request.method=GET spanID=9460e5b6e64cec91 traceID=b162d3040ddf86d6614e79c66a01a577
time="2022-02-06T17:47:48Z" level=debug msg="fetch response received" response.header.accept-ranges=bytes response.header.age=1356 response.header.alt-svc="h3=\":443\"; ma=2592000,h3-29=\":443\"; ma=2592000,h3-Q050=\":443\"; ma=2592000,h3-Q046=\":443\"; ma=2592000,h3-Q043=\":443\"; ma=2592000,quic=\":443\"; ma=2592000; v=\"46,43\"" response.header.cache-control="public, max-age=3600" response.header.content-length=2818413 response.header.content-type=application/octet-stream response.header.date="Sun, 06 Feb 2022 17:25:17 GMT" response.header.etag="\"1d55e7be5a77c4a908ad11bc33ebea1c\"" response.header.expires="Sun, 06 Feb 2022 18:25:17 GMT" response.header.last-modified="Wed, 24 Nov 2021 21:07:06 GMT" response.header.server=UploadServer response.header.x-goog-generation=1637788026431708 response.header.x-goog-hash="crc32c=ZojF+g==" response.header.x-goog-hash.1="md5=HVXnvlp3xKkIrRG8M+vqHA==" response.header.x-goog-metageneration=1 response.header.x-goog-storage-class=STANDARD response.header.x-goog-stored-content-encoding=identity response.header.x-goog-stored-content-length=2818413 response.header.x-guploader-uploadid=ADPycdsebqxiTBJqZ0bv9zBigjFxgQydD2ESZSkKchpE0ILlN9Ibko3C5r4fJTJ4UR9ddp-UBd-2v_4eRpZ8Yo2llW_j4k8WhQ response.status="200 OK" spanID=9460e5b6e64cec91 traceID=b162d3040ddf86d6614e79c66a01a577
...
```

## 设置注册表证书 Setting registry certificates

If you specify registry certificates in the BuildKit configuration, the daemon copies the files into the container under `/etc/buildkit/certs`. The following steps show adding a self-signed registry certificate to the BuildKit configuration.

​	如果在 BuildKit 配置中指定了注册表证书，守护进程会将文件复制到容器中的 `/etc/buildkit/certs` 目录下。以下步骤展示了如何向 BuildKit 配置中添加自签名的注册表证书。

1. Add the following configuration to `/etc/buildkitd.toml`:

   在 `/etc/buildkitd.toml` 文件中添加以下配置：

   ```toml
   # /etc/buildkitd.toml
   debug = true
   [registry."myregistry.com"]
     ca=["/etc/certs/myregistry.pem"]
     [[registry."myregistry.com".keypair]]
       key="/etc/certs/myregistry_key.pem"
       cert="/etc/certs/myregistry_cert.pem"
   ```

   This tells the builder to push images to the `myregistry.com` registry using the certificates in the specified location (`/etc/certs`).

   ​	这会告诉构建器使用指定位置（`/etc/certs`）中的证书，将镜像推送到 `myregistry.com` 注册表。

2. Create a `docker-container` builder that uses this configuration:

   创建一个使用该配置的 `docker-container` 构建器：

   ```console
   $ docker buildx create --use --bootstrap \
     --name mybuilder \
     --driver docker-container \
     --config /etc/buildkitd.toml
   ```

3. Inspect the builder's configuration file (`/etc/buildkit/buildkitd.toml`), it shows that the certificate configuration is now configured in the builder.

   检查构建器的配置文件（`/etc/buildkit/buildkitd.toml`），可以看到证书配置已应用到构建器中：

   ```console
   $ docker exec -it buildx_buildkit_mybuilder0 cat /etc/buildkit/buildkitd.toml
   ```

   

   ```toml
   debug = true
   
   [registry]
   
     [registry."myregistry.com"]
       ca = ["/etc/buildkit/certs/myregistry.com/myregistry.pem"]
   
       [[registry."myregistry.com".keypair]]
         cert = "/etc/buildkit/certs/myregistry.com/myregistry_cert.pem"
         key = "/etc/buildkit/certs/myregistry.com/myregistry_key.pem"
   ```

4. Verify that the certificates are inside the container:

   验证证书是否在容器内：

   ```console
   $ docker exec -it buildx_buildkit_mybuilder0 ls /etc/buildkit/certs/myregistry.com/
   myregistry.pem    myregistry_cert.pem   myregistry_key.pem
   ```

Now you can push to the registry using this builder, and it will authenticate using the certificates:

​	现在可以使用此构建器将镜像推送到注册表，且会使用证书进行身份验证：

```console
$ docker buildx build --push --tag myregistry.com/myimage:latest .
```

## CNI networking

CNI networking for builders can be useful for dealing with network port contention during concurrent builds. CNI is [not yet](https://github.com/moby/buildkit/issues/28) available in the default BuildKit image. But you can create your own image that includes CNI support.

​	构建器的 CNI 网络功能在并发构建期间有助于解决网络端口争用问题。默认的 BuildKit 镜像中还[不支持](https://github.com/moby/buildkit/issues/28) CNI，但你可以创建包含 CNI 支持的自定义镜像。

The following Dockerfile example shows a custom BuildKit image with CNI support. It uses the [CNI config for integration tests](https://github.com/moby/buildkit/blob/master//hack/fixtures/cni.json) in BuildKit as an example. Feel free to include your own CNI configuration.

​	以下 Dockerfile 示例展示了带有 CNI 支持的自定义 BuildKit 镜像，使用了 BuildKit 中的 [CNI 配置](https://github.com/moby/buildkit/blob/master//hack/fixtures/cni.json) 作为示例。可以根据需要包含自己的 CNI 配置。

```dockerfile
# syntax=docker/dockerfile:1

ARG BUILDKIT_VERSION=v0.16.0
ARG CNI_VERSION=v1.0.1

FROM --platform=$BUILDPLATFORM alpine AS cni-plugins
RUN apk add --no-cache curl
ARG CNI_VERSION
ARG TARGETOS
ARG TARGETARCH
WORKDIR /opt/cni/bin
RUN curl -Ls https://github.com/containernetworking/plugins/releases/download/$CNI_VERSION/cni-plugins-$TARGETOS-$TARGETARCH-$CNI_VERSION.tgz | tar xzv

FROM moby/buildkit:${BUILDKIT_VERSION}
ARG BUILDKIT_VERSION
RUN apk add --no-cache iptables
COPY --from=cni-plugins /opt/cni/bin /opt/cni/bin
ADD https://raw.githubusercontent.com/moby/buildkit/${BUILDKIT_VERSION}/hack/fixtures/cni.json /etc/buildkit/cni.json
```

Now you can build this image, and create a builder instance from it using [the `--driver-opt image` option](https://docs.docker.com/reference/cli/docker/buildx/create/#driver-opt):

​	现在可以构建此镜像，并使用 [`--driver-opt image` 选项](https://docs.docker.com/reference/cli/docker/buildx/create/#driver-opt)创建一个基于该镜像的构建器实例：

```console
$ docker buildx build --tag buildkit-cni:local --load .
$ docker buildx create --use --bootstrap \
  --name mybuilder \
  --driver docker-container \
  --driver-opt "image=buildkit-cni:local" \
  --buildkitd-flags "--oci-worker-net=cni"
```

## 资源限制 Resource limiting

### 最大并行度 Max parallelism

You can limit the parallelism of the BuildKit solver, which is particularly useful for low-powered machines, using a [BuildKit configuration]({{< ref "/manuals/DockerBuild/BuildKit/buildkitd_toml" >}}) while creating a builder with the [`--config` flags](https://docs.docker.com/reference/cli/docker/buildx/create/#config).

​	可以限制 BuildKit 解析器的并行度，特别适用于低性能的机器。创建构建器时，可以通过 [`--config` 标志](https://docs.docker.com/reference/cli/docker/buildx/create/#config)应用 BuildKit 配置：

```toml
# /etc/buildkitd.toml
[worker.oci]
  max-parallelism = 4
```

Now you can [create a `docker-container` builder]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockercontainerbuilddriver" >}}) that will use this BuildKit configuration to limit parallelism.

​	现在可以 创建一个使用该 BuildKit 配置的 `docker-container` 构建器来限制并行度：

```console
$ docker buildx create --use \
  --name mybuilder \
  --driver docker-container \
  --config /etc/buildkitd.toml
```

### TCP 连接限制 TCP connection limit

TCP connections are limited to 4 simultaneous connections per registry for pulling and pushing images, plus one additional connection dedicated to metadata requests. This connection limit prevents your build from getting stuck while pulling images. The dedicated metadata connection helps reduce the overall build time.

​	TCP 连接每个注册表限制为同时 4 个连接，用于拉取和推送镜像，外加一个专用于元数据请求的连接。此连接限制防止构建在拉取镜像时卡住。专用的元数据连接有助于缩短整体构建时间。

More information: [moby/buildkit#2259](https://github.com/moby/buildkit/pull/2259)

​	更多信息请参考：[moby/buildkit#2259](https://github.com/moby/buildkit/pull/2259)
