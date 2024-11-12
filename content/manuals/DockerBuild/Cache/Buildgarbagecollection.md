+++
title = "构建垃圾回收"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/build/cache/garbage-collection/](https://docs.docker.com/build/cache/garbage-collection/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Build garbage collection - 构建垃圾回收

While [`docker builder prune`]({{< ref "/reference/CLIreference/docker/dockerbuilder/dockerbuilderprune" >}}) or [`docker buildx prune`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxprune" >}}) commands run at once, garbage collection runs periodically and follows an ordered list of prune policies.

​	虽然 [`docker builder prune`]({{< ref "/reference/CLIreference/docker/dockerbuilder/dockerbuilderprune" >}}) 或 [`docker buildx prune`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxprune" >}}) 命令一次性执行，但垃圾回收会定期运行，并遵循一系列的清理策略。

Garbage collection runs in the BuildKit daemon. The daemon clears the build cache when the cache size becomes too big, or when the cache age expires. The following sections describe how you can configure both the size and age parameters by defining garbage collection policies.

​	垃圾回收在 BuildKit 守护进程中运行。守护进程会在缓存大小过大或缓存年龄过期时清理构建缓存。以下部分描述了如何通过定义垃圾回收策略来配置缓存大小和缓存年龄参数。

## 配置 Configuration

Depending on the [driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}}) used by your builder instance, the garbage collection will use a different configuration file.

​	根据构建器实例所用的[驱动程序]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}})，垃圾回收将使用不同的配置文件。

If you're using the [`docker` driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockerdriver" >}}), garbage collection can be configured in the [Docker Daemon configuration](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file). file:

​	如果使用 [`docker` 驱动程序]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockerdriver" >}})，可以在 [Docker Daemon 配置文件](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file)中配置垃圾回收：



```json
{
  "builder": {
    "gc": {
      "enabled": true,
      "defaultKeepStorage": "10GB",
      "policy": [
        { "keepStorage": "10GB", "filter": ["unused-for=2200h"] },
        { "keepStorage": "50GB", "filter": ["unused-for=3300h"] },
        { "keepStorage": "100GB", "all": true }
      ]
    }
  }
}
```

For other drivers, garbage collection can be configured using the [BuildKit configuration]({{< ref "/manuals/DockerBuild/BuildKit/buildkitd_toml" >}}) file:

​	对于其他驱动程序，可以使用 [BuildKit 配置文件]({{< ref "/manuals/DockerBuild/BuildKit/buildkitd_toml" >}}) 来配置垃圾回收：



```toml
[worker.oci]
  gc = true
  gckeepstorage = 10000
  [[worker.oci.gcpolicy]]
    keepBytes = 512000000
    keepDuration = 172800
    filters = [ "type==source.local", "type==exec.cachemount", "type==source.git.checkout"]
  [[worker.oci.gcpolicy]]
    all = true
    keepBytes = 1024000000
```

## 默认策略 Default policies

Default garbage collection policies apply to all builders if not set:

​	如果未设置，默认垃圾回收策略适用于所有构建器：



```text
GC Policy rule#0:
        All:            false
        Filters:        type==source.local,type==exec.cachemount,type==source.git.checkout
        Keep Duration:  48h0m0s
        Keep Bytes:     512MB
GC Policy rule#1:
        All:            false
        Keep Duration:  1440h0m0s
        Keep Bytes:     26GB
GC Policy rule#2:
        All:            false
        Keep Bytes:     26GB
GC Policy rule#3:
        All:            true
        Keep Bytes:     26GB
```

- `rule#0`: if build cache uses more than 512MB delete the most easily reproducible data after it has not been used for 2 days.
  - `rule#0`: 如果构建缓存使用超过 512MB，则在数据未使用 2 天后删除最易再生成的数据。

- `rule#1`: remove any data not used for 60 days.
  - `rule#1`: 删除 60 天未使用的任何数据。

- `rule#2`: keep the unshared build cache under cap.
  - `rule#2`: 保持未共享的构建缓存低于限制。

- `rule#3`: if previous policies were insufficient start deleting internal data to keep build cache under cap.
  - `rule#3`: 如果先前的策略不足以降低缓存大小，则开始删除内部数据以保持构建缓存低于限制。

> **Note**
>
> 
>
> `Keep Bytes` defaults to 10% of the size of the disk. If the disk size cannot be determined, it uses 2GB as a fallback.
>
> ​	`Keep Bytes` 默认占磁盘大小的 10%。如果无法确定磁盘大小，将使用 2GB 作为回退。
