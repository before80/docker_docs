+++
title = "Continuous Integration (CI)"
date = 2024-10-23T14:54:40+08:00
weight = 10
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/extensions/extensions-sdk/dev/continuous-integration/](https://docs.docker.com/extensions/extensions-sdk/dev/continuous-integration/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Continuous Integration (CI)

In order to help validate your extension and ensure it's functional, the Extension SDK provides tools to help you setup continuous integration for your extension.

> **Important**
>
> 
>
> The [Docker Desktop Action](https://github.com/docker/desktop-action) and the [extension-test-helper library](https://www.npmjs.com/package/@docker/extension-test-helper) are both [experimental](https://docs.docker.com/release-lifecycle/#experimental).

## [Setup CI environment with GitHub Actions](https://docs.docker.com/extensions/extensions-sdk/dev/continuous-integration/#setup-ci-environment-with-github-actions)

You need Docker Desktop to be able to install and validate your extension. You can start Docker Desktop in GitHub Actions using the [Docker Desktop Action](https://github.com/docker/desktop-action), by adding the following to a workflow file:



```yaml
steps:
  - id: start_desktop
    uses: docker/desktop-action/start@v0.1.0
```

> **Note**
>
> 
>
> This action supports only Github Action macOS runners at the moment. You need to specify `runs-on: macOS-latest` for your end to end tests.

Once the step has executed, the next steps use Docker Desktop and the Docker CLI to install and test the extension.

## [Validating your extension with Puppeteer](https://docs.docker.com/extensions/extensions-sdk/dev/continuous-integration/#validating-your-extension-with-puppeteer)

Once Docker Desktop starts in CI, you can build, install, and validate your extension with Jest and Puppeteer.

First, build and install the extension from your test:



```ts
import { DesktopUI } from "@docker/extension-test-helper";
import { exec as originalExec } from "child_process";
import * as util from "util";

export const exec = util.promisify(originalExec);

// keep a handle on the app to stop it at the end of tests
let dashboard: DesktopUI;

beforeAll(async () => {
  await exec(`docker build -t my/extension:latest .`, {
    cwd: "my-extension-src-root",
  });

  await exec(`docker extension install -f my/extension:latest`);
});
```

Then open the Docker Dashboard and run some tests in your extension's UI:



```ts
describe("Test my extension", () => {
  test("should be functional", async () => {
    dashboard = await DesktopUI.start();

    const eFrame = await dashboard.navigateToExtension("my/extension");

    // use puppeteer APIs to manipulate the UI, click on buttons, expect visual display and validate your extension
    await eFrame.waitForSelector("#someElementId");
  });
});
```

Finally, close the Docker Dashboard and uninstall your extension:



```ts
afterAll(async () => {
  dashboard?.stop();
  await exec(`docker extension uninstall my/extension`);
});
```

## [What's next](https://docs.docker.com/extensions/extensions-sdk/dev/continuous-integration/#whats-next)

- Build an [advanced frontend](https://docs.docker.com/extensions/extensions-sdk/build/frontend-extension-tutorial/) extension.
- Learn more about extensions [architecture](https://docs.docker.com/extensions/extensions-sdk/architecture/).
- Learn how to [publish your extension](https://docs.docker.com/extensions/extensions-sdk/extensions/).
