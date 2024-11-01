+++
title = "Docker Desktop 允许列表"
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

# Allowlist for Docker Desktop - Docker Desktop 允许列表

This page contains the domain URLs that you need to add to a firewall allowlist to ensure Docker Desktop works properly within your organization.

​	本页面列出了需要添加到防火墙允许列表的域名 URL，以确保 Docker Desktop 在组织中正常运行。

## 需要允许的域名 URL - Domain URLs to allow

| Domains                                                      | Description              |
| ------------------------------------------------------------ | ------------------------ |
| [https://api.segment.io](https://api.segment.io/)            | 分析 Analytics           |
| [https://cdn.segment.com](https://cdn.segment.com/)          | 分析 Analytics           |
| [https://notify.bugsnag.com](https://notify.bugsnag.com/)    | 错误报告 Error reports   |
| [https://sessions.bugsnag.com](https://sessions.bugsnag.com/) | 错误报告 Error reports   |
| [https://auth.docker.io](https://auth.docker.io/)            | 认证 Authentication      |
| [https://cdn.auth0.com](https://cdn.auth0.com/)              | 认证 Authentication      |
| [https://login.docker.com](https://login.docker.com/)        | 认证 Authentication      |
| [https://desktop.docker.com](https://desktop.docker.com/)    | Update                   |
| [https://hub.docker.com](https://hub.docker.com/)            | Docker Pull/Push         |
| [https://registry-1.docker.io](https://registry-1.docker.io/) | Docker Pull/Push         |
| [https://production.cloudflare.docker.com](https://production.cloudflare.docker.com/) | Docker Pull/Push         |
| [https://docker-pinata-support.s3.amazonaws.com](https://docker-pinata-support.s3.amazonaws.com/) | 故障排除 Troubleshooting |
| [https://api.dso.docker.com](https://api.dso.docker.com/)    | Docker Scout service     |

