+++
title = "Allowlist for Docker Desktop"
date = 2024-10-23T14:54:40+08:00
weight = 90
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/desktop/allow-list/](https://docs.docker.com/desktop/allow-list/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Allowlist for Docker Desktop

This page contains the domain URLs that you need to add to a firewall allowlist to ensure Docker Desktop works properly within your organization.

## Domain URLs to allow

| Domains                                                      | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [https://api.segment.io](https://api.segment.io/)            | Analytics                                                    |
| [https://cdn.segment.com](https://cdn.segment.com/)          | Analytics                                                    |
| [https://notify.bugsnag.com](https://notify.bugsnag.com/)    | Error reports                                                |
| [https://sessions.bugsnag.com](https://sessions.bugsnag.com/) | Error reports                                                |
| [https://auth.docker.io](https://auth.docker.io/)            | Authentication                                               |
| [https://cdn.auth0.com](https://cdn.auth0.com/)              | Authentication                                               |
| [https://login.docker.com](https://login.docker.com/)        | 1$ docker run --gpus=all -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama2$ docker exec -it ollama ollama run llama2console |
| [https://desktop.docker.com](https://desktop.docker.com/)    | Update                                                       |
| [https://hub.docker.com](https://hub.docker.com/)            | Docker Pull/Push                                             |
| [https://registry-1.docker.io](https://registry-1.docker.io/) | Docker Pull/Push                                             |
| [https://production.cloudflare.docker.com](https://production.cloudflare.docker.com/) | Docker Pull/Push                                             |
| [https://docker-pinata-support.s3.amazonaws.com](https://docker-pinata-support.s3.amazonaws.com/) | Troubleshooting                                              |
| [https://api.dso.docker.com](https://api.dso.docker.com/)    | Docker Scout service                                         |
