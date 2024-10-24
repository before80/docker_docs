+++
title = "Continuous Integration"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/scout/integrations/ci/](https://docs.docker.com/scout/integrations/ci/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Using Docker Scout in continuous integration

You can analyze Docker images in continuous integration pipelines as you build them using a GitHub action or the Docker Scout CLI plugin.

Available integrations:

- [GitHub Actions](https://docs.docker.com/scout/integrations/ci/gha/)
- [GitLab](https://docs.docker.com/scout/integrations/ci/gitlab/)
- [Microsoft Azure DevOps Pipelines](https://docs.docker.com/scout/integrations/ci/azure/)
- [Circle CI](https://docs.docker.com/scout/integrations/ci/circle-ci/)
- [Jenkins](https://docs.docker.com/scout/integrations/ci/jenkins/)

You can also add runtime integration as part of your CI/CD pipeline, which lets you assign an image to an environment, such as `production` or `staging`, when you deploy it. For more information, see [Environment monitoring](https://docs.docker.com/scout/integrations/environment/).
