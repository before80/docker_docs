+++
title = "Sample apps"
date = 2024-10-23T14:54:40+08:00
weight = 20
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/compose/support-and-feedback/samples-for-compose/](https://docs.docker.com/compose/support-and-feedback/samples-for-compose/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Sample apps with Compose

The following samples show the various aspects of how to work with Docker Compose. As a prerequisite, be sure to [install Docker Compose](https://docs.docker.com/compose/install/) if you have not already done so.

## [Key concepts these samples cover](https://docs.docker.com/compose/support-and-feedback/samples-for-compose/#key-concepts-these-samples-cover)

The samples should help you to:

- Define services based on Docker images using [Compose files](https://docs.docker.com/reference/compose-file/): `compose.yml` and `docker-stack.yml`
- Understand the relationship between `compose.yml` and [Dockerfiles](https://docs.docker.com/reference/dockerfile/)
- Learn how to make calls to your application services from Compose files
- Learn how to deploy applications and services to a [swarm](https://docs.docker.com/engine/swarm/)

## [Samples tailored to demo Compose](https://docs.docker.com/compose/support-and-feedback/samples-for-compose/#samples-tailored-to-demo-compose)

These samples focus specifically on Docker Compose:

- [Quickstart: Compose and ELK](https://github.com/docker/awesome-compose/tree/master/elasticsearch-logstash-kibana/README.md) - Shows how to use Docker Compose to set up and run ELK - Elasticsearch-Logstash-Kibana.
- [Quickstart: Compose and Django](https://github.com/docker/awesome-compose/tree/master/official-documentation-samples/django/README.md) - Shows how to use Docker Compose to set up and run a simple Django/PostgreSQL app.
- [Quickstart: Compose and Rails](https://github.com/docker/awesome-compose/tree/master/official-documentation-samples/rails/README.md) - Shows how to use Docker Compose to set up and run a Rails/PostgreSQL app.
- [Quickstart: Compose and WordPress](https://github.com/docker/awesome-compose/tree/master/official-documentation-samples/wordpress/README.md) - Shows how to use Docker Compose to set up and run WordPress in an isolated environment with Docker containers.

## [Awesome Compose samples](https://docs.docker.com/compose/support-and-feedback/samples-for-compose/#awesome-compose-samples)

The Awesome Compose samples provide a starting point on how to integrate different frameworks and technologies using Docker Compose. All samples are available in the [Awesome-compose GitHub repo](https://github.com/docker/awesome-compose) and are ready to run with `docker compose up`.
