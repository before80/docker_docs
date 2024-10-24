+++
title = "Extensions SDK"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/extensions/extensions-sdk/](https://docs.docker.com/extensions/extensions-sdk/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Overview of the Extensions SDK

The resources in this section help you create your own Docker extension.

The Docker CLI tool provides a set of commands to help you build and publish your extension, packaged as a specially formatted Docker image.

At the root of the image filesystem is a `metadata.json` file which describes the content of the extension. It's a fundamental element of a Docker extension.

An extension can contain a UI part and backend parts that run either on the host or in the Desktop virtual machine. For further information, see [Architecture]({{< ref "/manuals/DockerExtensions/ExtensionsSDK/Architecture" >}}).

You distribute extensions through Docker Hub. However, you can develop them locally without the need to push the extension to Docker Hub. See [Extensions distribution]({{< ref "/manuals/DockerExtensions/ExtensionsSDK/ParttwoPublish/Packageandreleaseyourextension" >}}) for further details.

> Already built an extension?
>
> Let us know about your experience using the [feedback form](https://survey.alchemer.com/s3/7184948/Publishers-Feedback-Form).



The build and publish process

Understand the process for building and publishing an extension.



Quickstart guide

Follow the quickstart guide to create a basic Docker extension quickly.



View the design guidelines

Ensure your extension aligns to Docker's design guidelines and principles.



Publish your extension

Understand how to publish your extension to the Marketplace.



Interacting with Kubernetes

Find information on how to interact indirectly with a Kubernetes cluster from your Docker extension.



Multi-arch extensions

Build your extension for multiple architectures.
