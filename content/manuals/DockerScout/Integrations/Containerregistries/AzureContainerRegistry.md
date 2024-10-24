+++
title = "Azure Container Registry"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/scout/integrations/registry/acr/](https://docs.docker.com/scout/integrations/registry/acr/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Integrate Docker Scout with Azure Container Registry

Integrating Docker Scout with Azure Container Registry (ACR) lets you view image insights for images hosted in ACR repositories. After integrating Docker Scout with ACR and activating Docker Scout for a repository, pushing an image to the repository automatically triggers image analysis. You can view image insights using the Docker Scout Dashboard, or the `docker scout` CLI commands.

## How it works

To help you integrate your Azure Container Registry with Docker Scout, you can use a custom Azure Resource Manager (ARM) template that automatically creates the necessary infrastructure in Azure for you:

- An EventGrid Topic and Subscription for Image push and delete events.
- A read-only authorization token for the registry, used to list repositories, and ingest the images.

When the resources have been created in Azure, you can enable the integration for image repositories in the integrated ACR instance. Once you've enabled a repository, pushing new images triggers image analysis automatically. The analysis results appear in the Docker Scout Dashboard.

If you enable the integration on a repository that already contains images, Docker Scout pulls and analyzes the latest image version automatically.

### ARM template

The following table describes the configuration resources.

> **Note**
>
> 
>
> Creating these resources incurs a small, recurring cost on the Azure account. The **Cost** column in the table represents an estimated monthly cost of the resources, when integrating an ACR registry that gets 100 images pushed per day.
>
> The Egress cost varies depending on usage, but it’s around $0.1 per GB, and the first 100 GB are free.

| Azure                   | Resource                                                     | Cost                                              |
| ----------------------- | ------------------------------------------------------------ | ------------------------------------------------- |
| Event Grid system topic | Subscribe to Azure Container Registry events (image push and image delete) | Free                                              |
| Event subscription      | Send Event Grid events to Scout via a Webhook subscription   | $0.60 for every 1M messages. First 100k for free. |
| Registry Token          | Read-only token used for Scout to list the repositories, and pull images from the registry | Free                                              |

The following JSON document shows the ARM template Docker Scout uses to create the Azure resources.

## Integrate a registry

1. Go to [ACR integration page](https://scout.docker.com/settings/integrations/azure/) on the Docker Scout Dashboard.

2. In the **How to integrate** section, enter the **Registry hostname** of the registry you want to integrate.

3. Select **Next**.

4. Select **Deploy to Azure** to open the template deployment wizard in Azure.

   You may be prompted to sign in to your Azure account if you're not already signed in.

5. In the template wizard, configure your deployment:

   - **Resource group**: enter the same resource group as you're using for the container registry. The Docker Scout resources must be deployed to the same resource group as the registry.
   - **Registry name**: the field is pre-filled with the subdomain of the registry hostname.

6. Select **Review + create**, and then **Create** to deploy the template.

7. Wait until the deployment is complete.

8. In the **Deployment details** section click on the newly created resource of the type **Container registry token**. Generate a new password for this token.

   Alternatively, use the search function in Azure to navigate to the **Container registry** resource that you're looking to integrate, and generate the new password for the created access token.

9. Copy the generated password and head back to the Docker Scout Dashboard to finalize the integration.

10. Paste the generated password into the **Registry token** field.

11. Select **Enable integration**.

After selecting **Enable integration**, Docker Scout performs a connection test to verify the integration. If the verification was successful, you're redirected to the Azure registry summary page, which shows you all your Azure integrations for the current organization.

Next, activate Docker Scout for the repositories that you want to analyze in [Repository settings](https://scout.docker.com/settings/repos/).

After activating repositories, images that you push are analyzed by Docker Scout. The analysis results appear in the Docker Scout Dashboard. If your repository already contains images, Docker Scout pulls and analyzes the latest image version automatically.

## Remove an integration

> **Important**
>
> 
>
> Removing the integration in the Docker Scout Dashboard doesn't automatically remove the resources created in Azure.

To remove an ACR integration:

1. Go to the [ACR integration page](https://scout.docker.com/settings/integrations/azure/) on the Docker Scout Dashboard.
2. Find the ACR integration that you want to remove, and select the **Remove** button.
3. In the dialog that opens, confirm by selecting **Remove**.
4. After removing the integration in the Docker Scout Dashboard, also remove the Azure resources related to the integration:
   - The `docker-scout-readonly-token` token for the container registry.
   - The `docker-scout-repository` Event Grid System Topic.
