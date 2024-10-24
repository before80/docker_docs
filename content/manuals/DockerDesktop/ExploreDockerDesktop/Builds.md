+++
title = "Builds"
date = 2024-10-23T14:54:40+08:00
weight = 30
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/desktop/use-desktop/builds/](https://docs.docker.com/desktop/use-desktop/builds/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Explore the Builds view in Docker Desktop

![Builds view in Docker Desktop](Builds_img/builds-view.webp)

The **Builds** view is a simple interface that lets you inspect your build history and manage builders using Docker Desktop.

Opening the **Builds** view in Docker Desktop displays a list of completed builds. By default, the list is sorted by date, showing the most recent builds at the top. You can switch to **Active builds** to view any ongoing builds.

![Build UI screenshot active builds](Builds_img/build-ui-active-builds.webp)

If you're connected to a cloud builder through [Docker Build Cloud]({{< ref "/manuals/DockerBuildCloud" >}}), the Builds view also lists any active or completed cloud builds by other team members connected to the same cloud builder.

## Show build list

Select the **Builds** view in the Docker Dashboard to open the build list.

The build list shows your completed and ongoing builds. The **Build history** tab shows completed historical builds, and from here you can inspect the build logs, dependencies, traces, and more. The **Active builds** tab shows builds that are currently running.

The list shows builds for your active, running builders. It doesn't list builds for inactive builders: builders that you've removed from your system, or builders that have been stopped.

### Builder settings

The top-right corner shows the name of your currently selected builder, and the **Builder settings** button lets you [manage builders](https://docs.docker.com/desktop/use-desktop/builds/#manage-builders) in the Docker Desktop settings.

### Import builds

**Beta feature**

Import builds is currently in [Beta](https://docs.docker.com/release-lifecycle/#Beta).

The **Import builds** button lets you import build records for builds by other people, or builds in a CI environment. When you've imported a build record, it gives you full access to the logs, traces, and other data for that build, directly in Docker Desktop. The [build summary]({{< ref "/manuals/DockerBuild/CI/GitHubActions/Buildsummary" >}}) for the `docker/build-push-action` and `docker/bake-action` GitHub Actions includes a link to download the build records, for inspecting CI jobs with Docker Desktop.

## Inspect builds

To inspect a build, select the build that you want to view in the list. The inspection view contains a number of tabs.

The **Info** tab displays details about the build.

If you're inspecting a multi-platform build, the drop-down menu in the top-right of this tab lets you filter the information down to a specific platform:

![Platform filter](Builds_img/build-ui-platform-menu.webp)

The **Source details** section shows information about the frontend [frontend]({{< ref "/manuals/DockerBuild/BuildKit/CustomDockerfilesyntax" >}}) and, if available, the source code repository used for the build.

### Build timing

The **Build timing** section of the Info tab contains charts showing a breakdown of the build execution from various angles.

- **Real time** refers to the wall-clock time that it took to complete the build.
- **Accumulated time** shows the total CPU time for all steps.
- **Cache usage** shows the extent to which build operations were cached.
- **Parallel execution** shows how much of the build execution time was spent running steps in parallel.

![Build timing charts](Builds_img/build-ui-timing-chart.webp)

The chart colors and legend keys describe the different build operations. Build operations are defined as follows:

| Build operation      | Description                                                  |
| :------------------- | :----------------------------------------------------------- |
| Local file transfers | Time spent transferring local files from the client to the builder. |
| File operations      | Any operations that involve creating and copying files in the build. For example, the `COPY`, `WORKDIR`, `ADD` instructions in a Dockerfile frontend all incur file operations. |
| Image pulls          | Time spent pulling images.                                   |
| Executions           | Container executions, for example commands defined as `RUN` instructions in a Dockerfile frontend. |
| HTTP                 | Remote artifact downloads using `ADD`.                       |
| Git                  | Same as **HTTP** but for Git URLs.                           |
| Result exports       | Time spent exporting the build results.                      |
| SBOM                 | Time spent generating the [SBOM attestation]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations/SBOMattestations" >}}). |
| Idle                 | Idle time for build workers, which can happen if you have configured a [max parallelism limit](https://docs.docker.com/build/buildkit/configure/#max-parallelism). |

### Build dependencies

The **Dependencies** section shows images and remote resources used during the build. Resources listed here include:

- Container images used during the build
- Git repositories included using the `ADD` Dockerfile instruction
- Remote HTTPS resources included using the `ADD` Dockerfile instruction

### Arguments, secrets, and other parameters

The **Configuration** section of the Info tab shows parameters passed to the build:

- Build arguments, including the resolved value
- Secrets, including their IDs (but not their values)
- SSH sockets
- Labels
- [Additional contexts](https://docs.docker.com/reference/cli/docker/buildx/build/#build-context)

### Outputs and artifacts

The **Build results** section shows a summary of the generated build artifacts, including image manifest details, attestations, and build traces.

Attestations are metadata records attached to a container image. The metadata describes something about the image, for example how it was built or what packages it contains. For more information about attestations, see [Build attestations]({{< ref "/manuals/DockerBuild/Metadata/Buildattestations" >}}).

Build traces capture information about the build execution steps in Buildx and BuildKit. The traces are available in two formats: OTLP and Jaeger. You can download build traces from Docker Desktop by opening the actions menu and selecting the format you want to download.

#### Inspect build traces with Jaeger

Using a Jaeger client, you can import and inspect build traces from Docker Desktop. The following steps show you how to export a trace from Docker Desktop and view it in [Jaeger](https://www.jaegertracing.io/):

1. Start Jaeger UI:

   

   ```console
   $ docker run -d --name jaeger -p "16686:16686" jaegertracing/all-in-one
   ```

2. Open the Builds view in Docker Desktop, and select a completed build.

3. Navigate to the **Build results** section, open the actions menu and select **Download as Jaeger format**.

   <video controls="" style="--tw-border-spacing-x: 0; --tw-border-spacing-y: 0; --tw-translate-x: 0; --tw-translate-y: 0; --tw-rotate: 0; --tw-skew-x: 0; --tw-skew-y: 0; --tw-scale-x: 1; --tw-scale-y: 1; --tw-pan-x: ; --tw-pan-y: ; --tw-pinch-zoom: ; --tw-scroll-snap-strictness: proximity; --tw-gradient-from-position: ; --tw-gradient-via-position: ; --tw-gradient-to-position: ; --tw-ordinal: ; --tw-slashed-zero: ; --tw-numeric-figure: ; --tw-numeric-spacing: ; --tw-numeric-fraction: ; --tw-ring-inset: ; --tw-ring-offset-width: 0px; --tw-ring-offset-color: #fff; --tw-ring-color: rgb(59 130 246 / 0.5); --tw-ring-offset-shadow: 0 0 #0000; --tw-ring-shadow: 0 0 #0000; --tw-shadow: 0 0 #0000; --tw-shadow-colored: 0 0 #0000; --tw-blur: ; --tw-brightness: ; --tw-contrast: ; --tw-grayscale: ; --tw-hue-rotate: ; --tw-invert: ; --tw-saturate: ; --tw-sepia: ; --tw-drop-shadow: ; --tw-backdrop-blur: ; --tw-backdrop-brightness: ; --tw-backdrop-contrast: ; --tw-backdrop-grayscale: ; --tw-backdrop-hue-rotate: ; --tw-backdrop-invert: ; --tw-backdrop-opacity: ; --tw-backdrop-saturate: ; --tw-backdrop-sepia: ; --tw-contain-size: ; --tw-contain-layout: ; --tw-contain-paint: ; --tw-contain-style: ; box-sizing: border-box; border-width: 0px; border-style: solid; border-color: initial; display: block; vertical-align: middle; max-width: 100%; height: auto; margin: 0px;"></video>

4. Go to [http://localhost:16686](http://localhost:16686/) in your browser to open Jaeger UI.

5. Select the **Upload** tab and open the Jaeger build trace you just exported.

Now you can analyze the build trace using the Jaeger UI:

![Jaeger UI screenshot](Builds_img/build-ui-jaeger-screenshot.png)Screenshot of a build trace in the Jaeger UI

### Dockerfile source and errors

When inspecting a successful completed build or an ongoing active build, the **Source** tab shows the [frontend]({{< ref "/manuals/DockerBuild/BuildKit/CustomDockerfilesyntax" >}}) used to create the build.

If the build failed, an **Error** tab displays instead of the **Source** tab. The error message is inlined in the Dockerfile source, indicating where the failure happened and why.

![Build error displayed inline in the Dockerfile](Builds_img/build-ui-error.webp)

### Build logs

The **Logs** tab displays the build logs. For active builds, the logs are updated in real-time.

You can toggle between a **List view** and a **Plain-text view** of a build log.

- The **List view** presents all build steps in a collapsible format, with a timeline for navigating the log along a time axis.
- The **Plain-text view** displays the log as plain text.

The **Copy** button lets you copy the plain-text version of the log to your clipboard.

### Build history

The **History** tab displays statistics data about completed builds.

The time series chart illustrates trends in duration, build steps, and cache usage for related builds, helping you identify patterns and shifts in build operations over time. For instance, significant spikes in build duration or a high number of cache misses could signal opportunities for optimizing the Dockerfile.

![Build history chart](Builds_img/build-ui-history.webp)

You can navigate to and inspect a related build by selecting it in the chart, or using the **Past builds** list below the chart.

## Manage builders

The **Builder settings** view in the Docker Desktop settings lets you:

- Inspect the state and configuration of active builders
- Start and stop a builder
- Delete build history
- Add or remove builders (or connect and disconnect, in the case of cloud builders)

![Builder settings drop-down](Builds_img/build-ui-manage-builders.webp)

For more information about managing builders, see [Change settings](https://docs.docker.com/desktop/settings/#builders)
