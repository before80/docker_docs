+++
title = "Integrating Docker Scout with environments"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/scout/integrations/environment/](https://docs.docker.com/scout/integrations/environment/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Integrating Docker Scout with environments

You can integrate Docker Scout with your runtime environments, and get insights for your running workloads. This gives you a real-time view of your security status for your deployed artifacts.

Docker Scout lets you define multiple environments, and assign images to different environments. This gives you a complete overview of your software supply chain, and lets you view and compare deltas between environments, for example staging and production.

How you define and name your environments is up to you. You can use patterns that are meaningful to you and that matches how you ship your applications.

## Assign to environments

Each environment contains references to a number of images. These references represent containers currently running in that particular environment.

For example, say you're running `myorg/webapp:3.1` in production, you can assign that tag to your `production` environment. You might be running a different version of the same image in staging, in which case you can assign that version of the image to the `staging` environment.

To add environments to Docker Scout, you can:

- Use the `docker scout env <environment> <image>` CLI command to record images to environments manually
- Enable a runtime integration to automatically detect images in your environments.

Docker Scout supports the following runtime integrations:

- [Docker Scout GitHub Action](https://github.com/marketplace/actions/docker-scout#record-an-image-deployed-to-an-environment)
- [CLI client]({{< ref "/manuals/DockerScout/Integrations/IntegratingDockerScoutwithenvironments/GenericCLI" >}})
- [Sysdig integration]({{< ref "/manuals/DockerScout/Integrations/IntegratingDockerScoutwithenvironments/Sysdig" >}})

> **Note**
>
> 
>
> Only organization owners can create new environments and set up integrations. Additionally, Docker Scout only assigns an image to an environment if the image [has been analyzed]({{< ref "/manuals/DockerScout/Explore/DockerScoutimageanalysis" >}}), either manually or through a [registry integration](https://docs.docker.com/scout/integrations/#container-registries).

## List environments

To see all of the available environments for an organization, you can use the `docker scout env` command.



```console
$ docker scout env
```

By default, this prints all environments for your personal Docker organization. To list environments for another organization that you're a part of, use the `--org` flag.



```console
$ docker scout env --org <org>
```

You can use the `docker scout config` command to change the default organization. This changes the default organization for all `docker scout` commands, not just `env`.



```console
$ docker scout config organization <org>
```

## Comparing between environments

Assigning images to environments lets you make comparisons with and between environments. This is useful for things like GitHub pull requests, for comparing the image built from the code in the PR to the corresponding image in staging or production.

You can also compare with streams using the `--to-env` flag on the [`docker scout compare`]({{< ref "/reference/CLIreference/docker/dockerscout/dockerscoutcompare" >}}) CLI command:



```console
$ docker scout compare --to-env production myorg/webapp:latest
```

## View images for an environment

To view the images for an environment:

1. Go to the [Images page](https://scout.docker.com/) in the Docker Scout Dashboard.
2. Open the **Environments** drop-down menu.
3. Select the environment that you want to view.

The list displays all images that have been assigned to the selected environment. If you've deployed multiple versions of the same image in an environment, all versions of the image appear in the list.

Alternatively, you can use the `docker scout env` command to view the images from the terminal.



```console
$ docker scout env production
docker/scout-demo-service:main@sha256:ef08dca54c4f371e7ea090914f503982e890ec81d22fd29aa3b012351a44e1bc
```

### Mismatching image tags

When you've selected an environment on the **Images** tab, tags in the list represent the tag that was used to deploy the image. Tags are mutable, meaning that you can change the image digest that a tag refers to. If Docker Scout detects that a tag refers to an outdated digest, a warning icon displays next to the image name.
