+++
title = "Hardened Docker Desktop"
date = 2024-10-23T14:54:40+08:00
weight = 50
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/security/for-admins/hardened-desktop/](https://docs.docker.com/security/for-admins/hardened-desktop/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Overview of Hardened Docker Desktop

> **Note**
>
> 
>
> Hardened Docker Desktop is available to Docker Business customers only.

Hardened Docker Desktop is a group of security features, designed to improve the security of developer environments with minimal impact on developer experience or productivity.

It lets administrators enforce strict security settings, preventing developers and their containers from bypassing these controls, either intentionally or unintentionally. Additionally, you can enhance container isolation, to mitigate potential security threats such as malicious payloads breaching the Docker Desktop Linux VM and the underlying host.

Hardened Docker Desktop moves the ownership boundary for Docker Desktop configuration to the organization, meaning that any security controls administrators set cannot be altered by the user of Docker Desktop.

It is for security conscious organizations who:

- Don’t give their users root or administrator access on their machines
- Would like Docker Desktop to be within their organization’s centralized control
- Have certain compliance obligations

### [How does it help my organization?](https://docs.docker.com/security/for-admins/hardened-desktop/#how-does-it-help-my-organization)

Hardened Desktop features work independently but collectively to create a defense-in-depth strategy, safeguarding developer workstations against potential attacks across various functional layers, such as configuring Docker Desktop, pulling container images, and running container images. This multi-layered defense approach ensures comprehensive security. It helps mitigate against threats such as:

- Malware and supply chain attacks: Registry Access Management and Image Access Management prevent developers from accessing certain container registries and image types, significantly lowering the risk of malicious payloads. Additionally, ECI restricts the impact of containers with malicious payloads by running them without root privileges inside a Linux user namespace.
- Lateral movement: Air-Gapped Containers lets administrators configure network access restrictions for containers, thereby preventing malicious containers from performing lateral movement within the organization's network.
- Insider threats: Settings Management configures and locks various Docker Desktop settings so administrators can enforce company policies and prevent developers from introducing insecure configurations, intentionally or unintentionally.



Settings Management

Learn how Settings Management can secure your developers' workflows.



Enhanced Container Isolation

Understand how Enhanced Container Isolation can prevent container attacks.



Registry Access Management

Control the registries developers can access while using Docker Desktop.



Image Access Management

Control the images developers can pull from Docker Hub.



Air-Gapped Containers

Restrict containers from accessing unwanted network resources.
