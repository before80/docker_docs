+++
title = "CLI release notes"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/scout/release-notes/cli/](https://docs.docker.com/scout/release-notes/cli/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Docker Scout CLI release notes

This page contains information about the new features, improvements, known issues, and bug fixes in the Docker Scout [CLI plugin](https://github.com/docker/scout-cli/) and the `docker/scout-action` [GitHub Action](https://github.com/docker/scout-action).

## [1.13.0](https://docs.docker.com/scout/release-notes/cli/#1130)

*2024-08-05*

### [New](https://docs.docker.com/scout/release-notes/cli/#new)

- Add `--only-policy` filter option to the `docker scout quickview`, `docker scout policy` and `docker scout compare` commands.
- Add `--ignore-suppressed` filter option to `docker scout cves` and `docker scout quickview` commands to filter out CVEs affected by [exceptions](https://docs.docker.com/scout/explore/exceptions/).

### [Bug fixes and enhancements](https://docs.docker.com/scout/release-notes/cli/#bug-fixes-and-enhancements)

- Use conditional policy name in checks.

- Add support for detecting the version of a Go project set using linker flags, for example:

  

  ```console
  $ go build -ldflags "-X main.Version=1.2.3"
  ```

## [1.12.0](https://docs.docker.com/scout/release-notes/cli/#1120)

*2024-07-31*

### [New](https://docs.docker.com/scout/release-notes/cli/#new-1)

- Only display vulnerabilities from the base image:

  CLI

  

  ```console
  $ docker scout cves --only-base IMAGE
  ```

  GitHub Action

  

  ```yaml
  uses: docker/scout-action@v1
  with:
    command: cves
    image: [IMAGE]
    only-base: true
  ```

- Account for VEX in `quickview` command.

  CLI

  

  ```console
  $ docker scout quickview IMAGE --only-vex-affected --vex-location ./path/to/my.vex.json
  ```

  GitHub Action

  

  ```yaml
  uses: docker/scout-action@v1
  with:
    command: quickview
    image: [IMAGE]
    only-vex-affected: true
    vex-location: ./path/to/my.vex.json
  ```

- Account for VEX in `cves` command (GitHub Actions).

  GitHub Action

  

  ```yaml
  uses: docker/scout-action@v1
  with:
    command: cves
    image: [IMAGE]
    only-vex-affected: true
    vex-location: ./path/to/my.vex.json
  ```

### [Bug fixes and enhancements](https://docs.docker.com/scout/release-notes/cli/#bug-fixes-and-enhancements-1)

- Update `github.com/docker/docker` to `v26.1.5+incompatible` to fix CVE-2024-41110.
- Update Syft to 1.10.0.

## [1.11.0](https://docs.docker.com/scout/release-notes/cli/#1110)

*2024-07-25*

### [New](https://docs.docker.com/scout/release-notes/cli/#new-2)

- Filter CVEs listed in the CISA Known Exploited Vulnerabilities catalog.

  CLI

  

  ```console
  $ docker scout cves [IMAGE] --only-cisa-kev
  
  ... (cropped output) ...
  ## Packages and Vulnerabilities
  
  0C     1H     0M     0L  io.netty/netty-codec-http2 4.1.97.Final
  pkg:maven/io.netty/netty-codec-http2@4.1.97.Final
  
  ✗ HIGH CVE-2023-44487  CISA KEV  [OWASP Top Ten 2017 Category A9 - Using Components with Known Vulnerabilities]
    https://scout.docker.com/v/CVE-2023-44487
    Affected range  : <4.1.100
    Fixed version   : 4.1.100.Final
    CVSS Score      : 7.5
    CVSS Vector     : CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H
  ... (cropped output) ...
  ```

  GitHub Action

  

  ```yaml
  uses: docker/scout-action@v1
  with:
    command: cves
    image: [IMAGE]
    only-cisa-kev: true
  ```

- Add new classifiers:

  - `spiped`
  - `swift`
  - `eclipse-mosquitto`
  - `znc`

### [Bug fixes and enhancements](https://docs.docker.com/scout/release-notes/cli/#bug-fixes-and-enhancements-2)

- Allow VEX matching when no subcomponents.
- Fix panic when attaching an invalid VEX document.
- Fix SPDX document root.
- Fix base image detection when image uses SCRATCH as the base image.

## [1.10.0](https://docs.docker.com/scout/release-notes/cli/#1100)

*2024-06-26*

### [Bug fixes and enhancements](https://docs.docker.com/scout/release-notes/cli/#bug-fixes-and-enhancements-3)

- Add new classifiers:

  - `irssi`
  - `Backdrop`
  - `CrateDB CLI (Crash)`
  - `monica`
  - `Openliberty`
  - `dumb-init`
  - `friendica`
  - `redmine`

- Fix whitespace-only originator on package breaking BuildKit exporters

- Fix parsing image references in SPDX statement for images with a digest

- Support `sbom://` prefix for image comparison:

  CLI

  

  ```console
  $ docker scout compare sbom://image1.json --to sbom://image2.json
  ```

  GitHub Action

  

  ```yaml
  uses: docker/scout-action@v1
  with:
    command: compare
    image: sbom://image1.json
    to: sbom://image2.json
  ```

## [1.9.3](https://docs.docker.com/scout/release-notes/cli/#193)

*2024-05-28*

### [Bug fix](https://docs.docker.com/scout/release-notes/cli/#bug-fix)

- Fix a panic while retrieving cached SBOMs.

## [1.9.1](https://docs.docker.com/scout/release-notes/cli/#191)

*2024-05-27*

### [New](https://docs.docker.com/scout/release-notes/cli/#new-3)

- Add support for the [GitLab container scanning file format](https://docs.gitlab.com/ee/development/integrations/secure.html#container-scanning) with `--format gitlab` on `docker scout cves` command.

  Here is an example pipeline:

  

  ```yaml
     docker-build:
    # Use the official docker image.
    image: docker:cli
    stage: build
    services:
      - docker:dind
    variables:
      DOCKER_IMAGE_NAME: $CI_REGISTRY_IMAGE:$CI_COMMIT_REF_SLUG
    before_script:
      - docker login -u "$CI_REGISTRY_USER" -p "$CI_REGISTRY_PASSWORD" $CI_REGISTRY
  
      # Install curl and the Docker Scout CLI
      - |
        apk add --update curl
        curl -sSfL https://raw.githubusercontent.com/docker/scout-cli/main/install.sh | sh -s --
        apk del curl
        rm -rf /var/cache/apk/*      
      # Login to Docker Hub required for Docker Scout CLI
      - echo "$DOCKER_HUB_PAT" | docker login --username "$DOCKER_HUB_USER" --password-stdin
  
    # All branches are tagged with $DOCKER_IMAGE_NAME (defaults to commit ref slug)
    # Default branch is also tagged with `latest`
    script:
      - docker buildx b --pull -t "$DOCKER_IMAGE_NAME" .
      - docker scout cves "$DOCKER_IMAGE_NAME" --format gitlab --output gl-container-scanning-report.json
      - docker push "$DOCKER_IMAGE_NAME"
      - |
        if [[ "$CI_COMMIT_BRANCH" == "$CI_DEFAULT_BRANCH" ]]; then
          docker tag "$DOCKER_IMAGE_NAME" "$CI_REGISTRY_IMAGE:latest"
          docker push "$CI_REGISTRY_IMAGE:latest"
        fi      
    # Run this job in a branch where a Dockerfile exists
    rules:
      - if: $CI_COMMIT_BRANCH
        exists:
          - Dockerfile
    artifacts:
      reports:
        container_scanning: gl-container-scanning-report.json
  ```

### [Bug fixes and enhancements](https://docs.docker.com/scout/release-notes/cli/#bug-fixes-and-enhancements-4)

- Support single-architecture images for `docker scout attest add` command
- Indicate on the `docker scout quickview` and `docker scout recommendations` commands if image provenance was not created using `mode=max`. Without `mode=max`, base images may be incorrectly detected, resulting in less accurate results.

## [1.9.0](https://docs.docker.com/scout/release-notes/cli/#190)

*2024-05-24*

Discarded in favor of [1.9.1](https://docs.docker.com/scout/release-notes/cli/#191).

## [1.8.0](https://docs.docker.com/scout/release-notes/cli/#180)

*2024-04-25*

### [Bug fixes and enhancements](https://docs.docker.com/scout/release-notes/cli/#bug-fixes-and-enhancements-5)

- Improve format of EPSS score and percentile.

  Before:

  

  ```text
  EPSS Score      : 0.000440
  EPSS Percentile : 0.092510
  ```

  After:

  

  ```text
  EPSS Score      : 0.04%
  EPSS Percentile : 9th percentile
  ```

- Fix markdown output of the `docker scout cves` command when analyzing local filesystem. [docker/scout-cli#113](https://github.com/docker/scout-cli/issues/113)

## [1.7.0](https://docs.docker.com/scout/release-notes/cli/#170)

*2024-04-15*

### [New](https://docs.docker.com/scout/release-notes/cli/#new-4)

- The [`docker scout push` command](https://docs.docker.com/reference/cli/docker/scout/push/) is now fully available: analyze images locally and push the SBOM to Docker Scout.

### [Bug fixes and enhancements](https://docs.docker.com/scout/release-notes/cli/#bug-fixes-and-enhancements-6)

- Fix adding attestations with `docker scout attestation add` to images in private repositories

- Fix image processing for images based on the empty `scratch` base image

- A new `sbom://` protocol for Docker Scout CLI commands let you read a Docker Scout SBOM from standard input.

  

  ```console
  $ docker scout sbom IMAGE | docker scout qv sbom://
  ```

- Add classifier for Joomla packages

## [1.6.4](https://docs.docker.com/scout/release-notes/cli/#164)

*2024-03-26*

### [Bug fixes and enhancements](https://docs.docker.com/scout/release-notes/cli/#bug-fixes-and-enhancements-7)

- Fix epoch handling for RPM-based Linux distributions

## [1.6.3](https://docs.docker.com/scout/release-notes/cli/#163)

*2024-03-22*

### [Bug fixes and enhancements](https://docs.docker.com/scout/release-notes/cli/#bug-fixes-and-enhancements-8)

- Improve package detection to ignore referenced but not installed packages.

## [1.6.2](https://docs.docker.com/scout/release-notes/cli/#162)

*2024-03-22*

### [Bug fixes and enhancements](https://docs.docker.com/scout/release-notes/cli/#bug-fixes-and-enhancements-9)

- EPSS data is now fetched via the backend, as opposed to via the CLI client.
- Fix an issue when rendering markdown output using the `sbom://` prefix.

### [Removed](https://docs.docker.com/scout/release-notes/cli/#removed)

- The `docker scout cves --epss-date` and `docker scout cache prune --epss` flags have been removed.

## [1.6.1](https://docs.docker.com/scout/release-notes/cli/#161)

*2024-03-20*

> **Note**
>
> 
>
> This release only affects the `docker/scout-action` GitHub Action.

### [New](https://docs.docker.com/scout/release-notes/cli/#new-5)

- Add support for passing in SBOM files in SDPX or in-toto SDPX format

  

  ```yaml
  uses: docker/scout-action@v1
  with:
      command: cves
      image: sbom://alpine.spdx.json
  ```

- Add support for SBOM files in `syft-json` format

  

  ```yaml
  uses: docker/scout-action@v1
  with:
      command: cves
      image: sbom://alpine.syft.json
  ```

## [1.6.0](https://docs.docker.com/scout/release-notes/cli/#160)

*2024-03-19*

> **Note**
>
> 
>
> This release only affects the CLI plugin, not the GitHub Action

### [New](https://docs.docker.com/scout/release-notes/cli/#new-6)

- Add support for passing in SBOM files in SDPX or in-toto SDPX format

  

  ```console
  $ docker scout cves sbom://path/to/sbom.spdx.json
  ```

- Add support for SBOM files in `syft-json` format

  

  ```console
  $ docker scout cves sbom://path/to/sbom.syft.json
  ```

- Reads SBOM files from standard input

  

  ```console
  $ syft -o json alpine | docker scout cves sbom://
  ```

- Prioritize CVEs by EPSS score

  - `--epss` to display and prioritise the CVEs
  - `--epss-score` and `--epss-percentile` to filter by score and percentile
  - Prune cached EPSS files with `docker scout cache prune --epss`

### [Bug fixes and enhancements](https://docs.docker.com/scout/release-notes/cli/#bug-fixes-and-enhancements-10)

- Use Windows cache from WSL2

  When inside WSL2 with Docker Desktop running, the Docker Scout CLI plugin now uses the cache from Windows. That way, if an image has been indexed for instance by Docker Desktop there's no need anymore to re-index it on WSL2 side.

- Indexing is now blocked in the CLI if it has been disabled using [Settings Management](https://docs.docker.com/security/for-admins/hardened-desktop/settings-management/configure/) feature.

- Fix a panic that would occur when analyzing a single-image `oci-dir` input

- Improve local attestation support with the containerd image store

## [Earlier versions](https://docs.docker.com/scout/release-notes/cli/#earlier-versions)

Release notes for earlier versions of the Docker Scout CLI plugin are available on [GitHub](https://github.com/docker/scout-cli/releases).
