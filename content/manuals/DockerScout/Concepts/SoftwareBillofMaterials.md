+++
title = "Software Bill of Materials"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/scout/concepts/sbom/](https://docs.docker.com/scout/concepts/sbom/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Software Bill of Materials

A Bill of Materials (BOM) is a list of materials, parts, and the quantities of each needed to manufacture a product. For example, a BOM for a computer might list the motherboard, CPU, RAM, power supply, storage devices, case, and other components, along with the quantities of each that are needed to build the computer.

A Software Bill of Materials (SBOM) is a list of all the components that make up a piece of software. This includes open source and third-party components, as well as any custom code that has been written for the software. An SBOM is similar to a BOM for a physical product, but for software.

In the context of software supply chain security, SBOMs can help with identifying and mitigating security and compliance risks in software. By knowing exactly what components are used in a piece of software, you can quickly identify and patch vulnerabilities in your components, or determine if a component is licensed in a way that is incompatible with your project.

## [Contents of an SBOM](https://docs.docker.com/scout/concepts/sbom/#contents-of-an-sbom)

An SBOM typically includes the following information:

- The name of the software, such as the name of a library or framework, that the SBOM describes.
- The version of the software.
- The license under which the software is distributed.
- A list of other components that the software depends on.

## [How Docker Scout uses SBOMs](https://docs.docker.com/scout/concepts/sbom/#how-docker-scout-uses-sboms)

Docker Scout uses SBOMs to determine the components that are used in a Docker image. When you analyze an image, Docker Scout will either use the SBOM that is attached to the image (using [attestations](https://docs.docker.com/build/metadata/attestations/)), or it will generate an SBOM on the fly by analyzing the contents of the image.

The SBOM is cross-referenced with the [advisory database](https://docs.docker.com/scout/deep-dive/advisory-db-sources/) to determine if any of the components in the image have known vulnerabilities.

## [Additional resources](https://docs.docker.com/scout/concepts/sbom/#additional-resources)

To learn more about generating SBOMs and how SBOMs are used in Docker Scout, see:

- [Image analysis in Docker Scout](https://docs.docker.com/scout/explore/analysis/)
- [View and create SBOMs](https://docs.docker.com/scout/how-tos/view-create-sboms/)
