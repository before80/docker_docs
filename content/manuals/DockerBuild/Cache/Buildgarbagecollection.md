+++
title = "Build garbage collection"
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

# Build garbage collection

While [`docker builder prune`]({{< ref "/reference/CLIreference/docker/dockerbuilder/dockerbuilderprune" >}}) or [`docker buildx prune`]({{< ref "/reference/CLIreference/docker/dockerbuildx/dockerbuildxprune" >}}) commands run at once, garbage collection runs periodically and follows an ordered list of prune policies.

Garbage collection runs in the BuildKit daemon. The daemon clears the build cache when the cache size becomes too big, or when the cache age expires. The following sections describe how you can configure both the size and age parameters by defining garbage collection policies.

## Configuration

Depending on the [driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers" >}}) used by your builder instance, the garbage collection will use a different configuration file.

If you're using the [`docker` driver]({{< ref "/manuals/DockerBuild/Builders/Builddrivers/Dockerdriver" >}}), garbage collection can be configured in the [Docker Daemon configuration](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file). file:



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

## Default policies

Default garbage collection policies apply to all builders if not set:



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
- `rule#1`: remove any data not used for 60 days.
- `rule#2`: keep the unshared build cache under cap.
- `rule#3`: if previous policies were insufficient start deleting internal data to keep build cache under cap.

> **Note**
>
> 
>
> `Keep Bytes` defaults to 10% of the size of the disk. If the disk size cannot be determined, it uses 2GB as a fallback.
