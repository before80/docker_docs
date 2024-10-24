+++
title = "Slack"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/scout/integrations/team-collaboration/slack/](https://docs.docker.com/scout/integrations/team-collaboration/slack/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Integrate Docker Scout with GitHub

**Beta feature**

The GitHub integration is currently in [Beta](https://docs.docker.com/release-lifecycle/#Beta).

The GitHub app integration for Docker Scout grants Docker Scout access to your source code repository on GitHub. This improved visibility into how your image gets created means Docker Scout can give you automated and contextual remediation advice.

## [How it works](https://docs.docker.com/scout/integrations/source-code-management/github/#how-it-works)

When you enable the GitHub integration, Docker Scout can make a direct link between the image analysis results and the source.

When analyzing your image, Docker Scout checks for [provenance attestations](https://docs.docker.com/build/metadata/attestations/slsa-provenance/) to detect the location of the source code repository for the image. If the source location is found, and you've enabled the GitHub app, Docker Scout parses the Dockerfile used to create the image.

Parsing the Dockerfile reveals the base image tag used to build the image. By knowing the base image tags used, Docker Scout can detect whether the tag is outdated, meaning it's been changed to a different image digest. For example, say you're using `alpine:3.18` as your base image, and at a later point in time, the image maintainers release a patch version for version `3.18`, containing security fixes. The `alpine:3.18` tag you've been using becomes out-of-date; the `alpine:3.18` you're using is no longer the latest.

When this happens, Docker Scout detects the discrepancy and surfaces it through the [Up-to-Date Base Images policy](https://docs.docker.com/scout/policy/#up-to-date-base-images-policy). When the GitHub integration's enabled, you'll also get automated suggestions on how to update your base image. For more information about how Docker Scout can help you automatically improve your supply chain conduct and security posture, see [Remediation](https://docs.docker.com/scout/policy/remediation/).

## [Setup](https://docs.docker.com/scout/integrations/source-code-management/github/#setup)

To integrate Docker Scout with your GitHub organization:

1. Go to [GitHub integration](https://scout.docker.com/settings/integrations/github/) on the Docker Scout Dashboard.

2. Select the **Integrate GitHub app** button to open GitHub.

3. Select the organization that you want to integrate.

4. Select whether you want to integrate all repositories in the GitHub organization or a manual selection of repositories.

5. Select **Install & Authorize** to add the Docker Scout app to the organization.

   This redirects you back to the Docker Scout Dashboard, which lists your active GitHub integrations.

The GitHub integration is now active.
