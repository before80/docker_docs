+++
title = "Distribute your dev environment"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/desktop/dev-environments/share/](https://docs.docker.com/desktop/dev-environments/share/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Distribute your dev environment

> **Important**
>
> 
>
> Dev Environments is no longer under active development.
>
> While the current functionality remains available, it may take us longer to respond to support requests.

The `compose-dev.yaml` config file makes distributing your dev environment easy so everyone can access the same code and any dependencies.

### [Distribute your dev environment](https://docs.docker.com/desktop/dev-environments/share/#distribute-your-dev-environment)

When you are ready to share your environment, simply copy the link to the Github repo where your project is stored, and share the link with your team members.

You can also create a link that automatically starts your dev environment when opened. This can then be placed on a GitHub README or pasted into a Slack channel, for example.

To create the link simply join the following link with the link to your dev environment's GitHub repository:

```
https://open.docker.com/dashboard/dev-envs?url=
```

The following example opens a [Compose sample](https://github.com/docker/awesome-compose/tree/master/nginx-golang-mysql), a Go server with an Nginx proxy and a MariaDB/MySQL database, in Docker Desktop.

https://open.docker.com/dashboard/dev-envs?url=https://github.com/docker/awesome-compose/tree/master/nginx-golang-mysql

### [Open a dev environment that has been distributed to you](https://docs.docker.com/desktop/dev-environments/share/#open-a-dev-environment-that-has-been-distributed-to-you)

To open a dev environment that has been shared with you, select the **Create** button in the top right-hand corner, select source **Existing Git repo**, and then paste the URL.
