+++
title = "启用 GPU 支持"
date = 2024-10-23T14:54:40+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/compose/how-tos/gpu-support/](https://docs.docker.com/compose/how-tos/gpu-support/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Enable GPU access with Docker Compose - 启用 GPU 访问与 Docker Compose

Compose services can define GPU device reservations if the Docker host contains such devices and the Docker Daemon is set accordingly. For this, make sure you install the [prerequisites](https://docs.docker.com/engine/containers/resource_constraints/#gpu) if you haven't already done so.

​	如果 Docker 主机上有 GPU 设备且 Docker Daemon 已正确设置，Compose 服务可以定义 GPU 设备的预留。为此，确保您已安装[先决条件](https://docs.docker.com/engine/containers/resource_constraints/#gpu)，如果尚未安装，请先进行安装。

The examples in the following sections focus specifically on providing service containers access to GPU devices with Docker Compose. You can use either `docker-compose` or `docker compose` commands. For more information, see [Migrate to Compose V2]({{< ref "/manuals/DockerCompose/Releases/MigratetoComposeV2" >}}).

​	以下各节中的示例专注于通过 Docker Compose 为服务容器提供 GPU 设备的访问权限。您可以使用 `docker-compose` 或 `docker compose` 命令。有关更多信息，请参阅 [迁移到 Compose V2]({{< ref "/manuals/DockerCompose/Releases/MigratetoComposeV2" >}})。

## 为服务容器启用 GPU 访问 Enabling GPU access to service containers

GPUs are referenced in a `compose.yml` file using the [device](https://docs.docker.com/reference/compose-file/deploy/#devices) attribute from the Compose Deploy specification, within your services that need them.

​	在 `compose.yml` 文件中，使用 Compose 部署规范的 [device](https://docs.docker.com/reference/compose-file/deploy/#devices) 属性来引用 GPU，指定在需要 GPU 的服务内。

This provides more granular control over a GPU reservation as custom values can be set for the following device properties:

​	这可以更精细地控制 GPU 预留，并可设置以下设备属性的自定义值：

- `capabilities`. This value specifies as a list of strings (eg. `capabilities: [gpu]`). You must set this field in the Compose file. Otherwise, it returns an error on service deployment.
  - `capabilities`：该值指定为字符串列表（例如 `capabilities: [gpu]`）。您必须在 Compose 文件中设置此字段，否则会在服务部署时返回错误。

- `count`. This value, specified as an integer or the value `all`, represents the number of GPU devices that should be reserved (providing the host holds that number of GPUs). If `count` is set to `all` or not specified, all GPUs available on the host are used by default.
  - `count`：此值可以为整数或 `all`，表示要预留的 GPU 设备数量（前提是主机上具有足够数量的 GPU）。如果 `count` 设置为 `all` 或未指定，默认会使用主机上的所有 GPU。

- `device_ids`. This value, specified as a list of strings, represents GPU device IDs from the host. You can find the device ID in the output of `nvidia-smi` on the host. If no `device_ids` are set, all GPUs available on the host are used by default.
  - `device_ids`：此值指定为字符串列表，表示主机上的 GPU 设备 ID。您可以在主机的 `nvidia-smi` 输出中找到设备 ID。如果未设置 `device_ids`，默认会使用主机上的所有 GPU。

- `driver`. This value is specified as a string, for example `driver: 'nvidia'`
  - `driver`：此值指定为字符串，例如 `driver: 'nvidia'`。

- `options`. Key-value pairs representing driver specific options.
  - `options`：表示驱动程序特定选项的键值对。


> **Important**
>
> 
>
> You must set the `capabilities` field. Otherwise, it returns an error on service deployment.
>
> ​	必须设置 `capabilities` 字段，否则在服务部署时会返回错误。
>
> `count` and `device_ids` are mutually exclusive. You must only define one field at a time.
>
> ​	`count` 和 `device_ids` 是互斥的，您只能同时定义其中一个字段。

For more information on these properties, see the [Compose Deploy Specification](https://docs.docker.com/reference/compose-file/deploy/#devices).

​	有关这些属性的更多信息，请参阅 [Compose 部署规范](https://docs.docker.com/reference/compose-file/deploy/#devices)。

### 使用 GPU 设备运行服务的 Compose 文件示例 Example of a Compose file for running a service with access to 1 GPU device



```yaml
services:
  test:
    image: nvidia/cuda:12.3.1-base-ubuntu20.04
    command: nvidia-smi
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: 1
              capabilities: [gpu]
```

Run with Docker Compose:

​	使用 Docker Compose 运行：

```console
$ docker compose up
Creating network "gpu_default" with the default driver
Creating gpu_test_1 ... done
Attaching to gpu_test_1    
test_1  | +-----------------------------------------------------------------------------+
test_1  | | NVIDIA-SMI 450.80.02    Driver Version: 450.80.02    CUDA Version: 11.1     |
test_1  | |-------------------------------+----------------------+----------------------+
test_1  | | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
test_1  | | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
test_1  | |                               |                      |               MIG M. |
test_1  | |===============================+======================+======================|
test_1  | |   0  Tesla T4            On   | 00000000:00:1E.0 Off |                    0 |
test_1  | | N/A   23C    P8     9W /  70W |      0MiB / 15109MiB |      0%      Default |
test_1  | |                               |                      |                  N/A |
test_1  | +-------------------------------+----------------------+----------------------+
test_1  |                                                                                
test_1  | +-----------------------------------------------------------------------------+
test_1  | | Processes:                                                                  |
test_1  | |  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
test_1  | |        ID   ID                                                   Usage      |
test_1  | |=============================================================================|
test_1  | |  No running processes found                                                 |
test_1  | +-----------------------------------------------------------------------------+
gpu_test_1 exited with code 0
```

On machines hosting multiple GPUs, the `device_ids` field can be set to target specific GPU devices and `count` can be used to limit the number of GPU devices assigned to a service container.

​	在拥有多 GPU 的主机上，可以设置 `device_ids` 字段以定位特定的 GPU 设备，也可以使用 `count` 限制分配给服务容器的 GPU 数量。

You can use `count` or `device_ids` in each of your service definitions. An error is returned if you try to combine both, specify an invalid device ID, or use a value of count that’s higher than the number of GPUs in your system.

​	在服务定义中，可以使用 `count` 或 `device_ids`。如果尝试同时使用它们、指定无效的设备 ID 或使用超过系统 GPU 数量的 `count` 值，则会返回错误。

```console
$ nvidia-smi   
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 450.80.02    Driver Version: 450.80.02    CUDA Version: 11.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  Tesla T4            On   | 00000000:00:1B.0 Off |                    0 |
| N/A   72C    P8    12W /  70W |      0MiB / 15109MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   1  Tesla T4            On   | 00000000:00:1C.0 Off |                    0 |
| N/A   67C    P8    11W /  70W |      0MiB / 15109MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   2  Tesla T4            On   | 00000000:00:1D.0 Off |                    0 |
| N/A   74C    P8    12W /  70W |      0MiB / 15109MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   3  Tesla T4            On   | 00000000:00:1E.0 Off |                    0 |
| N/A   62C    P8    11W /  70W |      0MiB / 15109MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
```

## 访问特定设备 Access specific devices

To allow access only to GPU-0 and GPU-3 devices:

​	要仅允许访问 GPU-0 和 GPU-3 设备：

```yaml
services:
  test:
    image: tensorflow/tensorflow:latest-gpu
    command: python -c "import tensorflow as tf;tf.test.gpu_device_name()"
    deploy:
      resources:
        reservations:
          devices:
          - driver: nvidia
            device_ids: ['0', '3']
            capabilities: [gpu]
```
