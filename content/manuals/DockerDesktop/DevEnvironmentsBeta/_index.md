+++
title = "Dev Environments (Beta)"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/desktop/dev-environments/](https://docs.docker.com/desktop/dev-environments/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Overview of Dev Environments

> **Important**
>
> 
>
> Dev Environments is no longer under active development.
>
> While the current functionality remains available, it may take us longer to respond to support requests.

**Beta**

The Dev Environments feature is currently in [Beta](https://docs.docker.com/release-lifecycle/#beta).

Dev Environments let you create a configurable developer environment with all the code and tools you need to quickly get up and running.

It uses tools built into code editors that allows Docker to access code mounted into a container rather than on your local host. This isolates the tools, files and running services on your machine allowing multiple versions of them to exist side by side.

You can use Dev Environments through the intuitive GUI in Docker Dashboard or straight from your terminal with the new [`docker dev` CLI plugin](https://docs.docker.com/desktop/dev-environments/dev-cli/).

## [Use Dev Environments](https://docs.docker.com/desktop/dev-environments/#use-dev-environments)

To use Dev Environments:

1. Navigate to the **Features in Development** tab in **Settings**.
2. On the **Beta** tab, select **Turn on Dev Environments**.
3. Select **Apply & restart**.

The Dev Environments tab is now visible in Docker Dashboard.

## [How does it work?](https://docs.docker.com/desktop/dev-environments/#how-does-it-work)

> **Changes to Dev Environments with Docker Desktop 4.13**
>
> Docker has simplified how you configure your dev environment project. All you need to get started is a `compose-dev.yaml` file. If you have an existing project with a `.docker/` folder this is automatically migrated the next time you launch.

Dev Environments is powered by [Docker Compose](https://docs.docker.com/compose/). This allows Dev Environments to take advantage of all the benefits and features of Compose whilst adding an intuitive GUI where you can launch environments with the click of a button.

Every dev environment you want to run needs a `compose-dev.yaml` file which configures your application's services and lives in your project directory. You don't need to be an expert in Docker Compose or write a `compose-dev.yaml` file from scratch as Dev Environments creates a starter `compose-dev.yaml` files based on the main language in your project.

You can also use the many [sample dev environments](https://github.com/docker/awesome-compose) as a starting point for how to integrate different services. Alternatively, see [Set up a dev environment](https://docs.docker.com/desktop/dev-environments/set-up/) for more information.

## [What's next?](https://docs.docker.com/desktop/dev-environments/#whats-next)

Learn how to:

- [Launch a dev environment](https://docs.docker.com/desktop/dev-environments/create-dev-env/)
- [Set up a dev environment](https://docs.docker.com/desktop/dev-environments/set-up/)
- [Distribute your dev environment](https://docs.docker.com/desktop/dev-environments/share/)
