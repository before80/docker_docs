+++
title = "Jenkins"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/scout/integrations/ci/jenkins/](https://docs.docker.com/scout/integrations/ci/jenkins/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Integrate Docker Scout with Jenkins

You can add the following stage and steps definition to a `Jenkinsfile` to run Docker Scout as part of a Jenkins pipeline. The pipeline needs a `DOCKER_HUB` credential containing the username and password for authenticating to Docker Hub. It also needs an environment variable defined for the image and tag.



```groovy
pipeline {
    agent {
        // Agent details
    }

    environment {
        DOCKER_HUB = credentials('jenkins-docker-hub-credentials')
        IMAGE_TAG  = 'myorg/scout-demo-service:latest'
    }

    stages {
        stage('Analyze image') {
            steps {
                // Install Docker Scout
                sh 'curl -sSfL https://raw.githubusercontent.com/docker/scout-cli/main/install.sh | sh -s -- -b /usr/local/bin'

                // Log into Docker Hub
                sh 'echo $DOCKER_HUB_PSW | docker login -u $DOCKER_HUB_USR --password-stdin'

                // Analyze and fail on critical or high vulnerabilities
                sh 'docker-scout cves $IMAGE_TAG --exit-code --only-severity critical,high'
            }
        }
    }
}
```

This installs Docker Scout, logs into Docker Hub, and then runs Docker Scout to generate a CVE report for an image and tag. It only shows critical or high-severity vulnerabilities.

> **Note**
>
> 
>
> If you're seeing a `permission denied` error related to the image cache, try setting the [`DOCKER_SCOUT_CACHE_DIR`]({{< ref "/manuals/DockerScout/How-tos/DockerScoutenvironmentvariables" >}}) environment variable to a writable directory. Or alternatively, disable local caching entirely with `DOCKER_SCOUT_NO_CACHE=true`.
