+++
title = "GitHub Actions"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/build/ci/github-actions/](https://docs.docker.com/build/ci/github-actions/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Introduction to GitHub Actions - GitHub Actions 简介

GitHub Actions is a popular CI/CD platform for automating your build, test, and deployment pipeline. Docker provides a set of official GitHub Actions for you to use in your workflows. These official actions are reusable, easy-to-use components for building, annotating, and pushing images.

​	GitHub Actions 是一个流行的 CI/CD 平台，用于自动化构建、测试和部署流程。Docker 提供了一套官方 GitHub Actions，您可以在工作流中使用它们。这些官方操作是可重用且易于使用的组件，用于构建、标注和推送镜像。

The following GitHub Actions are available:

​	以下是可用的 GitHub Actions：

- [Build and push Docker images](https://github.com/marketplace/actions/build-and-push-docker-images): build and push Docker images with BuildKit.
  - [构建和推送 Docker 镜像](https://github.com/marketplace/actions/build-and-push-docker-images)：使用 BuildKit 构建和推送 Docker 镜像。

- [Docker Login](https://github.com/marketplace/actions/docker-login): sign in to a Docker registry.
  - [Docker 登录](https://github.com/marketplace/actions/docker-login)：登录 Docker 注册表。

- [Docker Setup Buildx](https://github.com/marketplace/actions/docker-setup-buildx): initiates a BuildKit builder.
  - [Docker 设置 Buildx](https://github.com/marketplace/actions/docker-setup-buildx)：启动 BuildKit 构建器。

- [Docker Metadata action](https://github.com/marketplace/actions/docker-metadata-action): extracts metadata from Git reference and GitHub events.
  - [Docker 元数据操作](https://github.com/marketplace/actions/docker-metadata-action)：从 Git 引用和 GitHub 事件中提取元数据。

- [Docker Setup QEMU](https://github.com/marketplace/actions/docker-setup-qemu): installs [QEMU](https://github.com/qemu/qemu) static binaries for multi-arch builds.
  - [Docker 设置 QEMU](https://github.com/marketplace/actions/docker-setup-qemu)：安装用于多架构构建的 [QEMU](https://github.com/qemu/qemu) 静态二进制文件。

- [Docker Buildx Bake](https://github.com/marketplace/actions/docker-buildx-bake): enables using high-level builds with [Bake]({{< ref "/manuals/DockerBuild/Bake" >}}).
  - [Docker Buildx Bake](https://github.com/marketplace/actions/docker-buildx-bake)：支持使用高级构建工具 [Bake]({{< ref "/manuals/DockerBuild/Bake" >}})。

- [Docker Scout](https://github.com/docker/scout-action): analyze Docker images for security vulnerabilities.
  - [Docker Scout](https://github.com/docker/scout-action)：分析 Docker 镜像中的安全漏洞。


Using Docker's actions provides an easy-to-use interface, while still allowing flexibility for customizing build parameters.

​	使用 Docker 的操作提供了简单易用的接口，同时也允许灵活地自定义构建参数。

## Examples

If you're looking for examples on how to use the Docker GitHub Actions, refer to the following sections:

​	如果您正在寻找如何使用 Docker GitHub Actions 的示例，请参考以下部分：

- [Add image annotations with GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Annotations" >}})
  - [使用 GitHub Actions 添加镜像注释]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Annotations" >}})

- [Add SBOM and provenance attestations with GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Attestations" >}})

  - [使用 GitHub Actions 添加 SBOM 和来源认证]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Attestations" >}})

- [Using secrets with GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Buildsecrets" >}})

  - [在 GitHub Actions 中使用密钥]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Buildsecrets" >}})

- [GitHub Actions build summary]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Buildsummary" >}})

  - [GitHub Actions 构建总结]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Buildsummary" >}})

- [Configuring your GitHub Actions builder]({{< ref "/manuals/DockerBuild/CI/GitHubActions/BuildKitconfiguration" >}})

  - [配置 GitHub Actions 构建器]({{< ref "/manuals/DockerBuild/CI/GitHubActions/BuildKitconfiguration" >}})

- [Cache management with GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Cachemanagement" >}})

  - [GitHub Actions 的缓存管理]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Cachemanagement" >}})

- [Copy image between registries with GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Copyimagebetweenregistries" >}})

  - [使用 GitHub Actions 在注册表之间复制镜像]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Copyimagebetweenregistries" >}})

- [Export to Docker with GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions/ExporttoDocker" >}})

  - [使用 GitHub Actions 导出到 Docker]({{< ref "/manuals/DockerBuild/CI/GitHubActions/ExporttoDocker" >}})

- [Local registry with GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Localregistry" >}})

  - [GitHub Actions 的本地注册表]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Localregistry" >}})

- [Multi-platform image with GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Multi-platformimage" >}})

  - [使用 GitHub Actions 构建多平台镜像]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Multi-platformimage" >}})

- [Named contexts with GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Namedcontexts" >}})

  - [GitHub Actions 中的命名上下文]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Namedcontexts" >}})

- [Push to multiple registries with GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Pushtomultipleregistries" >}})

  - [使用 GitHub Actions 推送到多个注册表]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Pushtomultipleregistries" >}})

- [Reproducible builds with GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Reproduciblebuilds" >}})

  - [使用 GitHub Actions 进行可重现构建]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Reproduciblebuilds" >}})

- [Share built image between jobs with GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Shareimagebetweenjobs" >}})

  - [在 GitHub Actions 中共享构建的镜像]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Shareimagebetweenjobs" >}})

- [Manage tags and labels with GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Tagsandlabels" >}})

  - [使用 GitHub Actions 管理标签和标签]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Tagsandlabels" >}})

- [Test before push with GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Testbeforepush" >}})

  - [在推送之前测试]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Testbeforepush" >}})

- [Update Docker Hub description with GitHub Actions]({{< ref "/manuals/DockerBuild/CI/GitHubActions/UpdateDockerHubdescription" >}})

  - [使用 GitHub Actions 更新 Docker Hub 描述]({{< ref "/manuals/DockerBuild/CI/GitHubActions/UpdateDockerHubdescription" >}})

  

## 开始使用 GitHub Actions - Get started with GitHub Actions

This tutorial walks you through the process of setting up and using Docker GitHub Actions for building Docker images, and pushing images to Docker Hub. You will complete the following steps:

​	本教程将指导您设置并使用 Docker GitHub Actions 来构建 Docker 镜像，并将镜像推送到 Docker Hub。您将完成以下步骤：

1. Create a new repository on GitHub. 在 GitHub 上创建新存储库。
2. Define the GitHub Actions workflow. 定义 GitHub Actions 工作流。
3. Run the workflow. 运行工作流。

To follow this tutorial, you need a Docker ID and a GitHub account.

​	要完成本教程，您需要一个 Docker ID 和一个 GitHub 账户。

### 步骤一：创建存储库 Step one: Create the repository

Create a GitHub repository and configure the Docker Hub credentials.

​	在 GitHub 上创建一个存储库并配置 Docker Hub 凭据。

1. Create a new GitHub repository using [this template repository](https://github.com/dvdksn/clockbox/generate). 使用[此模板存储库](https://github.com/dvdksn/clockbox/generate)创建新的 GitHub 存储库。

   The repository contains a simple Dockerfile, and nothing else. Feel free to use another repository containing a working Dockerfile if you prefer.

   ​	该存储库包含一个简单的 Dockerfile。您也可以选择其他包含有效 Dockerfile 的存储库。

2. Open the repository **Settings**, and go to **Secrets and variables** > **Actions**. 打开存储库的 **Settings**，进入 **Secrets and variables** > **Actions**。

3. Create a new **Repository variable** named `DOCKERHUB_USERNAME` and your Docker ID as value. 创建一个名为 `DOCKERHUB_USERNAME` 的**存储库变量**，并将您的 Docker ID 作为值。

4. Create a new [personal access token](https://docs.docker.com/security/for-developers/access-tokens/#create-an-access-token) for Docker Hub. You can name this token `clockboxci`. 创建一个新的 [个人访问令牌](https://docs.docker.com/security/for-developers/access-tokens/#create-an-access-token)，可以命名为 `clockboxci`。

5. Add the Docker Hub access token as a **Repository secret** in your GitHub repository, with the name `DOCKERHUB_TOKEN`. 将 Docker Hub 访问令牌作为**存储库密钥**添加到您的 GitHub 存储库中，名称为 `DOCKERHUB_TOKEN`。

With your repository created, and credentials configured, you're now ready for action.

​	创建存储库并配置凭据后，就可以继续操作了。

### 步骤二：设置工作流 Step two: Set up the workflow

Set up your GitHub Actions workflow for building and pushing the image to Docker Hub.

​	设置 GitHub Actions 工作流，以构建镜像并将其推送到 Docker Hub。

1. Go to your repository on GitHub and then select the **Actions** tab. 在 GitHub 存储库中选择 **Actions** 标签。

2. Select **set up a workflow yourself**. 选择 **set up a workflow yourself**。

   This takes you to a page for creating a new GitHub actions workflow file in your repository, under `.github/workflows/main.yml` by default.

   ​	这将打开一个页面，默认在 `.github/workflows/main.yml` 下创建新的 GitHub actions 工作流文件。

3. In the editor window, copy and paste the following YAML configuration.

   在编辑器窗口中，复制并粘贴以下 YAML 配置。

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

   - `name`: the name of this workflow. 工作流名称。
   - `on.push.branches`: specifies that this workflow should run on every push event for the branches in the list. 指定该工作流应在每个推送事件时运行。
   - `jobs`: creates a job ID (`build`) and declares the type of machine that the job should run on. 创建一个作业 ID（`build`）并声明运行机器的类型。

For more information about the YAML syntax used here, see [Workflow syntax for GitHub Actions](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions).

​	有关此处 YAML 语法的更多信息，请参阅 [GitHub Actions 的工作流语法](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)。

### 步骤三：定义工作流步骤 Step three: Define the workflow steps

Now the essentials: what steps to run, and in what order to run them.

​	接下来定义步骤：执行哪些步骤以及执行顺序。

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

​	上面的 YAML 代码片段包含以下一系列步骤：

1. Signs in to Docker Hub, using the [Docker Login](https://github.com/marketplace/actions/docker-login) action and your Docker Hub credentials. 使用 [Docker Login](https://github.com/marketplace/actions/docker-login) 操作和您的 Docker Hub 凭证登录 Docker Hub。

2. Creates a BuildKit builder instance using the [Docker Setup Buildx](https://github.com/marketplace/actions/docker-setup-buildx) action. 使用 [Docker Setup Buildx](https://github.com/marketplace/actions/docker-setup-buildx) 操作创建一个 BuildKit 构建器实例。

3. Builds the container image and pushes it to the Docker Hub repository, using [Build and push Docker images](https://github.com/marketplace/actions/build-and-push-docker-images). 使用 [Build and push Docker images](https://github.com/marketplace/actions/build-and-push-docker-images) 操作构建容器镜像并将其推送到 Docker Hub 仓库。

   The `with` key lists a number of input parameters that configures the step:

   ​	`with` 键列出配置步骤的一些输入参数：
   
   - `push`: tells the action to upload the image to a registry after building it. 指示操作在构建后将镜像上传到注册表。
   - `tags`: tags that specify where to push the image. 指定要将镜像推送到的标签位置。

Add these steps to your workflow file. The full workflow configuration should look as follows:

​	将这些步骤添加到您的工作流文件中。完整的工作流配置应如下所示：

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

### 运行工作流 Run the workflow

Save the workflow file and run the job.

​	保存工作流文件并运行该任务。

1. Select **Commit changes...** and push the changes to the `main` branch. 选择 **Commit changes...** 并将更改推送到 `main` 分支。

   After pushing the commit, the workflow starts automatically.

   ​	推送提交后，工作流将自动启动。

2. Go to the **Actions** tab. It displays the workflow. 转到 **Actions** 选项卡。它会显示工作流。

   Selecting the workflow shows you the breakdown of all the steps.

   ​	选择工作流可以查看所有步骤的细分。

3. When the workflow is complete, go to your [repositories on Docker Hub](https://hub.docker.com/repositories). 当工作流完成后，前往您的 [Docker Hub 仓库](https://hub.docker.com/repositories)。

   If you see the new repository in that list, it means the GitHub Actions successfully pushed the image to Docker Hub.
   
   ​	如果在该列表中看到新的仓库，则说明 GitHub Actions 成功地将镜像推送到了 Docker Hub。

## Next steps

This tutorial has shown you how to create a simple GitHub Actions workflow, using the official Docker actions, to build and push an image to Docker Hub.

​	本教程展示了如何使用官方的 Docker 操作创建一个简单的 GitHub Actions 工作流，以构建并将镜像推送到 Docker Hub。

There are many more things you can do to customize your workflow to better suit your needs. To learn more about some of the more advanced use cases, take a look at the advanced examples, such as [building multi-platform images]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Multi-platformimage" >}}), or [using cache storage backends]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Cachemanagement" >}}) and also how to [configure your builder]({{< ref "/manuals/DockerBuild/CI/GitHubActions/BuildKitconfiguration" >}}).

​	您还可以进一步定制工作流以更好地满足您的需求。如需了解更多高级用例，请查看高级示例，例如 [构建多平台镜像]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Multi-platformimage" >}})、[使用缓存存储后端]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Cachemanagement" >}}) 以及如何 [配置您的构建器]({{< ref "/manuals/DockerBuild/CI/GitHubActions/BuildKitconfiguration" >}})。
