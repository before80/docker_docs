+++
title = "Include"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/compose/how-tos/multiple-compose-files/include/](https://docs.docker.com/compose/how-tos/multiple-compose-files/include/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Include

Introduced in Docker Compose version [2.20.3](https://docs.docker.com/compose/releases/release-notes/#2203)

​	在 Docker Compose 版本 [2.20.3](https://docs.docker.com/compose/releases/release-notes/#2203) 引入

With `include`, you can incorporate a separate `compose.yaml` file directly in your current `compose.yaml` file. This makes it easy to modularize complex applications into sub-Compose files, which in turn enables application configurations to be made simpler and more explicit.

​	使用 `include`，可以在当前的 `compose.yaml` 文件中直接包含单独的 `compose.yaml` 文件。这样可以将复杂的应用程序模块化为多个子 Compose 文件，从而简化和明确应用配置。

The [`include` top-level element]({{< ref "/reference/Composefilereference/Include" >}}) helps to reflect the engineering team responsible for the code directly in the config file's organization. It also solves the relative path problem that [`extends`]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Extend" >}}) and [merge]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Merge" >}}) present.

​	[`include` 顶级元素]({{< ref "/reference/Composefilereference/Include" >}}) 有助于将负责代码的工程团队结构直接反映在配置文件的组织中。它还解决了 [`extends`]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Extend" >}}) 和 [merge]({{< ref "/manuals/DockerCompose/How-tos/UsemultipleComposefiles/Merge" >}}) 所带来的相对路径问题。

Each path listed in the `include` section loads as an individual Compose application model, with its own project directory, in order to resolve relative paths.

​	`include` 中列出的每个路径都作为一个独立的 Compose 应用模型加载，具有自己的项目目录，以解决相对路径问题。

Once the included Compose application loads, all resources are copied into the current Compose application model.

​	加载包含的 Compose 应用后，所有资源都将复制到当前 Compose 应用模型中。

> **Note**
>
> 
>
> `include` applies recursively so an included Compose file which declares its own `include` section, results in those other files being included as well.
>
> ​	`include` 是递归的，因此包含的 Compose 文件如果声明了自己的 `include` 部分，也会导致这些其他文件被包含。

## Example



```yaml
include:
  - my-compose-include.yaml  #with serviceB declared
services:
  serviceA:
    build: .
    depends_on:
      - serviceB #use serviceB directly as if it was declared in this Compose file 直接使用 serviceB 就像它在此 Compose 文件中声明的一样
```

`my-compose-include.yaml` manages `serviceB` which details some replicas, web UI to inspect data, isolated networks, volumes for data persistence, etc. The application relying on `serviceB` doesn’t need to know about the infrastructure details, and consumes the Compose file as a building block it can rely on.

​	`my-compose-include.yaml` 管理 `serviceB`，包括一些副本、用于数据检查的 Web UI、独立的网络、用于数据持久化的卷等。依赖 `serviceB` 的应用无需了解其基础架构细节，直接将该 Compose 文件作为可依赖的构建模块。

This means the team managing `serviceB` can refactor its own database component to introduce additional services without impacting any dependent teams. It also means that the dependent teams don't need to include additional flags on each Compose command they run.

​	这意味着负责管理 `serviceB` 的团队可以自由重构自己的数据库组件以引入其他服务，而不会影响任何依赖它的团队。同时，这也意味着依赖团队不需要在每个 Compose 命令中添加额外的标志。

## Include and overrides

Compose reports an error if any resource from `include` conflicts with resources from the included Compose file. This rule prevents unexpected conflicts with resources defined by the included compose file author. However, there may be some circumstances where you might want to tweak the included model. This can be achieved by adding an override file to the include directive:

​	如果 `include` 中的任何资源与包含的 Compose 文件的资源冲突，Compose 将报告错误。这一规则可防止与包含文件定义的资源发生意外冲突。但在某些情况下，可能需要对包含的模型进行调整。这可以通过在 include 指令中添加覆盖文件来实现：



```yaml
include:
  - path : 
      - third-party/compose.yaml
      - override.yaml  # local override for third-party model 第三方模型的本地覆盖
```

The main limitation with this approach is that you need to maintain a dedicated override file per include. For complex projects with multiple includes this would result into many Compose file

​	使用此方法的主要限制是需要为每个包含的文件维护一个单独的覆盖文件。对于具有多个包含的复杂项目，这将导致多个 Compose 文件。

The other option is to use a `compose.override.yaml` file. While conflicts will be rejected from the file using `include` when same resource is declared, a global Compose override file can override the resulting merged model, as demonstrated in following example:

​	另一种选择是使用 `compose.override.yaml` 文件。尽管在声明相同资源时，从使用 `include` 的文件中会拒绝冲突，但全局的 Compose 覆盖文件可以覆盖合并后的模型，如下例所示：

Main `compose.yaml` file:

​	主 `compose.yaml` 文件：

```yaml
include:
  - team-1/compose.yaml # declare service-1
  - team-2/compose.yaml # declare service-2
```

Override `compose.override.yaml` file:

​	覆盖 `compose.override.yaml` 文件：

```yaml
services:
  service-1:
    # override included service-1 to enable debugger port 覆盖包含的 service-1 以启用调试端口
    ports:
      - 2345:2345

  service-2:
    # override included service-2 to use local data folder containing test data 覆盖包含的 service-2 以使用包含测试数据的本地数据文件夹
    volumes:
      - ./data:/data
```

Combined together, this allows you to benefit from third-party reusable components, and adjust the Compose model for your needs.

​	结合使用这些文件，可以利用第三方可重用组件，并根据需求调整 Compose 模型。

## 参考信息 Reference information

[`include` top-level element `include` 顶级元素]({{< ref "/reference/Composefilereference/Include" >}})
