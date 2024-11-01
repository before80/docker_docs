+++
title = "分发您的开发环境"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/dev-environments/share/](https://docs.docker.com/desktop/dev-environments/share/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Distribute your dev environment - 分发您的开发环境

> **Important**
>
> 
>
> Dev Environments is no longer under active development.
>
> ​	开发环境不再进行活跃开发。
>
> While the current functionality remains available, it may take us longer to respond to support requests.
>
> ​	尽管当前功能仍然可用，但我们可能会对支持请求的响应时间更长。

The `compose-dev.yaml` config file makes distributing your dev environment easy so everyone can access the same code and any dependencies.

​	`compose-dev.yaml` 配置文件让您轻松分发开发环境，使每个人都可以访问相同的代码和所有依赖项。

### 分发您的开发环境 Distribute your dev environment

When you are ready to share your environment, simply copy the link to the Github repo where your project is stored, and share the link with your team members.

​	当您准备共享您的环境时，只需复制项目存放的 GitHub 仓库链接，并将该链接分享给您的团队成员。

You can also create a link that automatically starts your dev environment when opened. This can then be placed on a GitHub README or pasted into a Slack channel, for example.

​	您还可以创建一个自动启动开发环境的链接，例如将该链接放在 GitHub README 或 Slack 频道中。

To create the link simply join the following link with the link to your dev environment's GitHub repository:

​	要创建此链接，只需将以下链接与您的开发环境 GitHub 仓库的链接拼接即可：

```
https://open.docker.com/dashboard/dev-envs?url=
```

The following example opens a [Compose sample](https://github.com/docker/awesome-compose/tree/master/nginx-golang-mysql), a Go server with an Nginx proxy and a MariaDB/MySQL database, in Docker Desktop.

​	以下示例打开一个 [Compose 示例](https://github.com/docker/awesome-compose/tree/master/nginx-golang-mysql)，包含一个带 Nginx 代理的 Go 服务器和一个 MariaDB/MySQL 数据库：

[https://open.docker.com/dashboard/dev-envs?url=https://github.com/docker/awesome-compose/tree/master/nginx-golang-mysql](https://open.docker.com/dashboard/dev-envs?url=https://github.com/docker/awesome-compose/tree/master/nginx-golang-mysql)

### 打开共享的开发环境Open a dev environment that has been distributed to you

To open a dev environment that has been shared with you, select the **Create** button in the top right-hand corner, select source **Existing Git repo**, and then paste the URL.

​	要打开共享给您的开发环境，选择右上角的 **创建** 按钮，选择源为 **已有 Git 仓库**，然后粘贴 URL。
