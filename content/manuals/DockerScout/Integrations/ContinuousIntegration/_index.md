+++
title = "Continuous Integration"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/scout/integrations/ci/](https://docs.docker.com/scout/integrations/ci/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Using Docker Scout in continuous integration

You can analyze Docker images in continuous integration pipelines as you build them using a GitHub action or the Docker Scout CLI plugin.

Available integrations:

- [GitHub Actions]({{< ref "/manuals/DockerScout/Integrations/ContinuousIntegration/GitHubActions" >}})
- [GitLab]({{< ref "/manuals/DockerScout/Integrations/ContinuousIntegration/GitLabCICD" >}})
- [Microsoft Azure DevOps Pipelines]({{< ref "/manuals/DockerScout/Integrations/ContinuousIntegration/AzureDevOpsPipelines" >}})
- [Circle CI]({{< ref "/manuals/DockerScout/Integrations/ContinuousIntegration/CircleCI" >}})
- [Jenkins]({{< ref "/manuals/DockerScout/Integrations/ContinuousIntegration/Jenkins" >}})

You can also add runtime integration as part of your CI/CD pipeline, which lets you assign an image to an environment, such as `production` or `staging`, when you deploy it. For more information, see [Environment monitoring]({{< ref "/manuals/DockerScout/Integrations/IntegratingDockerScoutwithenvironments" >}}).
