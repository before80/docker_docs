+++
title = "GitHub Actions"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/build/ci/github-actions/](https://docs.docker.com/build/ci/github-actions/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Introduction to GitHub Actions

GitHub Actions is a popular CI/CD platform for automating your build, test, and deployment pipeline. Docker provides a set of official GitHub Actions for you to use in your workflows. These official actions are reusable, easy-to-use components for building, annotating, and pushing images.

The following GitHub Actions are available:

- [Build and push Docker images](https://github.com/marketplace/actions/build-and-push-docker-images): build and push Docker images with BuildKit.
- [Docker Login](https://github.com/marketplace/actions/docker-login): sign in to a Docker registry.
- [Docker Setup Buildx](https://github.com/marketplace/actions/docker-setup-buildx): initiates a BuildKit builder.
- [Docker Metadata action](https://github.com/marketplace/actions/docker-metadata-action): extracts metadata from Git reference and GitHub events.
- [Docker Setup QEMU](https://github.com/marketplace/actions/docker-setup-qemu): installs [QEMU](https://github.com/qemu/qemu) static binaries for multi-arch builds.
- [Docker Buildx Bake](https://github.com/marketplace/actions/docker-buildx-bake): enables using high-level builds with [Bake](https://docs.docker.com/build/bake/).
- [Docker Scout](https://github.com/docker/scout-action): analyze Docker images for security vulnerabilities.

Using Docker's actions provides an easy-to-use interface, while still allowing flexibility for customizing build parameters.

## [Examples](https://docs.docker.com/build/ci/github-actions/#examples)

If you're looking for examples on how to use the Docker GitHub Actions, refer to the following sections:

- [Add image annotations with GitHub Actions](https://docs.docker.com/build/ci/github-actions/annotations/)
- [Add SBOM and provenance attestations with GitHub Actions](https://docs.docker.com/build/ci/github-actions/attestations/)
- [Using secrets with GitHub Actions](https://docs.docker.com/build/ci/github-actions/secrets/)
- [GitHub Actions build summary](https://docs.docker.com/build/ci/github-actions/build-summary/)
- [Configuring your GitHub Actions builder](https://docs.docker.com/build/ci/github-actions/configure-builder/)
- [Cache management with GitHub Actions](https://docs.docker.com/build/ci/github-actions/cache/)
- [Copy image between registries with GitHub Actions](https://docs.docker.com/build/ci/github-actions/copy-image-registries/)
- [Export to Docker with GitHub Actions](https://docs.docker.com/build/ci/github-actions/export-docker/)
- [Local registry with GitHub Actions](https://docs.docker.com/build/ci/github-actions/local-registry/)
- [Multi-platform image with GitHub Actions](https://docs.docker.com/build/ci/github-actions/multi-platform/)
- [Named contexts with GitHub Actions](https://docs.docker.com/build/ci/github-actions/named-contexts/)
- [Push to multiple registries with GitHub Actions](https://docs.docker.com/build/ci/github-actions/push-multi-registries/)
- [Reproducible builds with GitHub Actions](https://docs.docker.com/build/ci/github-actions/reproducible-builds/)
- [Share built image between jobs with GitHub Actions](https://docs.docker.com/build/ci/github-actions/share-image-jobs/)
- [Manage tags and labels with GitHub Actions](https://docs.docker.com/build/ci/github-actions/manage-tags-labels/)
- [Test before push with GitHub Actions](https://docs.docker.com/build/ci/github-actions/test-before-push/)
- [Update Docker Hub description with GitHub Actions](https://docs.docker.com/build/ci/github-actions/update-dockerhub-desc/)

## [Get started with GitHub Actions](https://docs.docker.com/build/ci/github-actions/#get-started-with-github-actions)

This tutorial walks you through the process of setting up and using Docker GitHub Actions for building Docker images, and pushing images to Docker Hub. You will complete the following steps:

1. Create a new repository on GitHub.
2. Define the GitHub Actions workflow.
3. Run the workflow.

To follow this tutorial, you need a Docker ID and a GitHub account.

### [Step one: Create the repository](https://docs.docker.com/build/ci/github-actions/#step-one-create-the-repository)

Create a GitHub repository and configure the Docker Hub credentials.

1. Create a new GitHub repository using [this template repository](https://github.com/dvdksn/clockbox/generate).

   The repository contains a simple Dockerfile, and nothing else. Feel free to use another repository containing a working Dockerfile if you prefer.

2. Open the repository **Settings**, and go to **Secrets and variables** > **Actions**.

3. Create a new **Repository variable** named `DOCKERHUB_USERNAME` and your Docker ID as value.

4. Create a new [personal access token](https://docs.docker.com/security/for-developers/access-tokens/#create-an-access-token) for Docker Hub. You can name this token `clockboxci`.

5. Add the Docker Hub access token as a **Repository secret** in your GitHub repository, with the name `DOCKERHUB_TOKEN`.

With your repository created, and credentials configured, you're now ready for action.

### [Step two: Set up the workflow](https://docs.docker.com/build/ci/github-actions/#step-two-set-up-the-workflow)

Set up your GitHub Actions workflow for building and pushing the image to Docker Hub.

1. Go to your repository on GitHub and then select the **Actions** tab.

2. Select **set up a workflow yourself**.

   This takes you to a page for creating a new GitHub actions workflow file in your repository, under `.github/workflows/main.yml` by default.

3. In the editor window, copy and paste the following YAML configuration.

   

   ```yaml
   name: ci
   
   on:
     push:
       branches:
         - "main"
   
   jobs:
     build:
       runs-on: ubuntu-latest
   ```

   - `name`: the name of this workflow.
   - `on.push.branches`: specifies that this workflow should run on every push event for the branches in the list.
   - `jobs`: creates a job ID (`build`) and declares the type of machine that the job should run on.

For more information about the YAML syntax used here, see [Workflow syntax for GitHub Actions](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions).

### [Step three: Define the workflow steps](https://docs.docker.com/build/ci/github-actions/#step-three-define-the-workflow-steps)

Now the essentials: what steps to run, and in what order to run them.



```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      -
        name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      -
        name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      -
        name: Build and push
        uses: docker/build-push-action@v6
        with:
          push: true
          tags: ${{ vars.DOCKERHUB_USERNAME }}/clockbox:latest
```

The previous YAML snippet contains a sequence of steps that:

1. Signs in to Docker Hub, using the [Docker Login](https://github.com/marketplace/actions/docker-login) action and your Docker Hub credentials.

2. Creates a BuildKit builder instance using the [Docker Setup Buildx](https://github.com/marketplace/actions/docker-setup-buildx) action.

3. Builds the container image and pushes it to the Docker Hub repository, using [Build and push Docker images](https://github.com/marketplace/actions/build-and-push-docker-images).

   The `with` key lists a number of input parameters that configures the step:

   - `push`: tells the action to upload the image to a registry after building it.
   - `tags`: tags that specify where to push the image.

Add these steps to your workflow file. The full workflow configuration should look as follows:



```yaml
name: ci

on:
  push:
    branches:
      - "main"

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      -
        name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      -
        name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      -
        name: Build and push
        uses: docker/build-push-action@v6
        with:
          push: true
          tags: ${{ vars.DOCKERHUB_USERNAME }}/clockbox:latest
```

### [Run the workflow](https://docs.docker.com/build/ci/github-actions/#run-the-workflow)

Save the workflow file and run the job.

1. Select **Commit changes...** and push the changes to the `main` branch.

   After pushing the commit, the workflow starts automatically.

2. Go to the **Actions** tab. It displays the workflow.

   Selecting the workflow shows you the breakdown of all the steps.

3. When the workflow is complete, go to your [repositories on Docker Hub](https://hub.docker.com/repositories).

   If you see the new repository in that list, it means the GitHub Actions successfully pushed the image to Docker Hub.

## [Next steps](https://docs.docker.com/build/ci/github-actions/#next-steps)

This tutorial has shown you how to create a simple GitHub Actions workflow, using the official Docker actions, to build and push an image to Docker Hub.

There are many more things you can do to customize your workflow to better suit your needs. To learn more about some of the more advanced use cases, take a look at the advanced examples, such as [building multi-platform images](https://docs.docker.com/build/ci/github-actions/multi-platform/), or [using cache storage backends](https://docs.docker.com/build/ci/github-actions/cache/) and also how to [configure your builder](https://docs.docker.com/build/ci/github-actions/configure-builder/).
