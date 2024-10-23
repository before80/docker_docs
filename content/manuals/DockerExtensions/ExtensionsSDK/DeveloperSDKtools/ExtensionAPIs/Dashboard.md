+++
title = "Dashboard"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/extensions/extensions-sdk/dev/api/dashboard/](https://docs.docker.com/extensions/extensions-sdk/dev/api/dashboard/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Dashboard

## [User notifications](https://docs.docker.com/extensions/extensions-sdk/dev/api/dashboard/#user-notifications)

Toasts provide a brief notification to the user. They appear temporarily and shouldn't interrupt the user experience. They also don't require user input to disappear.

### [success](https://docs.docker.com/extensions/extensions-sdk/dev/api/dashboard/#success)

▸ **success**(`msg`): `void`

Use to display a toast message of type success.



```typescript
ddClient.desktopUI.toast.success("message");
```

### [warning](https://docs.docker.com/extensions/extensions-sdk/dev/api/dashboard/#warning)

▸ **warning**(`msg`): `void`

Use to display a toast message of type warning.



```typescript
ddClient.desktopUI.toast.warning("message");
```

### [error](https://docs.docker.com/extensions/extensions-sdk/dev/api/dashboard/#error)

▸ **error**(`msg`): `void`

Use to display a toast message of type error.



```typescript
ddClient.desktopUI.toast.error("message");
```

For more details about method parameters and the return types available, see [Toast API reference](https://docs.docker.com/reference/api/extensions-sdk/Toast/).

> Deprecated user notifications
>
> These methods are deprecated and will be removed in a future version. Use the methods specified above.



```typescript
window.ddClient.toastSuccess("message");
window.ddClient.toastWarning("message");
window.ddClient.toastError("message");
```

## [Open a file selection dialog](https://docs.docker.com/extensions/extensions-sdk/dev/api/dashboard/#open-a-file-selection-dialog)

This function opens a file selector dialog that asks the user to select a file or folder.

▸ **showOpenDialog**(`dialogProperties`): `Promise`< [`OpenDialogResult`](https://docs.docker.com/reference/api/extensions-sdk/OpenDialogResult/)>:

The `dialogProperties` parameter is a list of flags passed to Electron to customize the dialog's behaviour. For example, you can pass `multiSelections` to allow a user to select multiple files. See [Electron's documentation](https://www.electronjs.org/docs/latest/api/dialog) for a full list.



```typescript
const result = await ddClient.desktopUI.dialog.showOpenDialog({
  properties: ["openDirectory"],
});
if (!result.canceled) {
  console.log(result.paths);
}
```

## [Open a URL](https://docs.docker.com/extensions/extensions-sdk/dev/api/dashboard/#open-a-url)

This function opens an external URL with the system default browser.

▸ **openExternal**(`url`): `void`



```typescript
ddClient.host.openExternal("https://docker.com");
```

> The URL must have the protocol `http` or `https`.

For more details about method parameters and the return types available, see [Desktop host API reference](https://docs.docker.com/reference/api/extensions-sdk/Host/).

> Deprecated user notifications
>
> This method is deprecated and will be removed in a future version. Use the methods specified above.



```typescript
window.ddClient.openExternal("https://docker.com");
```

## [Navigation to Dashboard routes](https://docs.docker.com/extensions/extensions-sdk/dev/api/dashboard/#navigation-to-dashboard-routes)

From your extension, you can also [navigate](https://docs.docker.com/extensions/extensions-sdk/dev/api/dashboard-routes-navigation/) to other parts of the Docker Desktop Dashboard.
