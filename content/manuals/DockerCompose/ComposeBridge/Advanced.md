+++
title = "高级"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/compose/bridge/advanced-integration/](https://docs.docker.com/compose/bridge/advanced-integration/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Advanced integration - 高级集成

**Experimental 实验性功能**

Compose Bridge is an [Experimental](https://docs.docker.com/release-lifecycle/#experimental) product.

​	Compose Bridge 是一个[实验性](https://docs.docker.com/release-lifecycle/#experimental)产品。

Compose Bridge can also function as a `kubectl` plugin, allowing you to integrate its capabilities directly into your Kubernetes command-line operations. This integration simplifies the process of converting and deploying applications from Docker Compose to Kubernetes.

​	Compose Bridge 也可以作为一个 `kubectl` 插件，使您能够直接在 Kubernetes 命令行操作中集成其功能。此集成简化了将应用程序从 Docker Compose 转换并部署到 Kubernetes 的过程。

## 将 `compose-bridge` 用作 `kubectl` 插件 Use `compose-bridge` as a `kubectl` plugin

To use the `compose-bridge` binary as a `kubectl` plugin, you need to make sure that the binary is available in your PATH and the name of the binary is prefixed with `kubectl-`.

​	要使用 `compose-bridge` 二进制文件作为 `kubectl` 插件，您需要确保该二进制文件在您的 PATH 中，并且二进制文件的名称以 `kubectl-` 为前缀。

1. Rename or copy the `compose-bridge` binary to `kubectl-compose_bridge`:

   将 `compose-bridge` 二进制文件重命名或复制为 `kubectl-compose_bridge`：

   ```console
   $ mv /path/to/compose-bridge /usr/local/bin/kubectl-compose_bridge
   ```

2. Ensure that the binary is executable:

   确保该二进制文件是可执行的：

   ```console
   $ chmod +x /usr/local/bin/kubectl-compose_bridge
   ```

3. Verify that the plugin is recognized by `kubectl`:

   验证插件是否被 `kubectl` 识别：

   ```console
   $ kubectl plugin list
   ```

   In the output, you should see `kubectl-compose_bridge`.

   ​	在输出中，您应该能看到 `kubectl-compose_bridge`。

4. Now you can use `compose-bridge` as a `kubectl` plugin:

   现在您可以将 `compose-bridge` 作为一个 `kubectl` 插件使用：

   ```console
   $ kubectl compose-bridge [command]
   ```

Replace `[command]` with any `compose-bridge` command you want to use.

​	将 `[command]` 替换为您想要使用的任何 `compose-bridge` 命令。
