+++
title = "Methods"
date = 2024-10-23T14:54:40+08:00
weight = 1
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文：[https://docs.docker.com/security/for-admins/enforce-sign-in/methods/](https://docs.docker.com/security/for-admins/enforce-sign-in/methods/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Ways to enforce sign-in for Docker Desktop

This page outlines the different ways you can enforce sign-in for Docker Desktop.

## Registry key method (Windows only)

> **Note**
>
> 
>
> The registry key method is available with Docker Desktop version 4.32 and later.

1. Create the registry key. Your new key should look like the following:

   

   ```console
   $ HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Docker\Docker Desktop
   ```

2. Create a multi-string value `allowedOrgs`.

   > **Important**
   >
   > 
   >
   > Only one entry for `allowedOrgs` is currently supported. If you add more than one value, sign-in enforcement silently fails.

3. As string data use your organization’s name, all lowercase.

4. Restart Docker Desktop.

5. Open Docker Desktop and when Docker Desktop starts, verify that the **Sign in required!** prompt appears.

In some cases, a system reboot may be necessary for enforcement to take effect.

> **Note**
>
> 
>
> If a registry key and a `registry.json` file both exist, the registry key takes precedence.

### Example deployment via Group Policy

The following is only an illustrative example.

There are many ways to deploy the registry key, for example using an MDM solution or with PowerShell scripting. The method you choose is dependent on your organizations infrastructure, security policies, and the administrative rights of the end-users.

1. Create the registry script. Write a script to create the `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Docker\Docker Desktop` key, add the `allowedOrgs` multi-string, and then set the value to your organization’s name.
2. Within Group Policy, create or edit a Group Policy Objective (GPO) that applies to the machines or users you want to target.
3. Within the GPO, navigate to **Computer Configuration** > **Preferences** > **Windows Settings** > **Registry**.
4. Add the registry item. Right-click on the **Registry** node, select **New** > **Registry Item**.
5. Configure the new registry item to match the registry script you created, specifying the action as **Update**. Make sure you input the correct path, value name (`allowedOrgs`), and value data (your organization’s name).
6. Link the GPO to an Organizational Unit (OU) that contains the machines you want to apply this setting to.
7. Test the GPO. Test the GPO on a small set of machines first to ensure it behaves as expected. You can use the `gpupdate /force` command on a test machine to manually refresh its group policy settings and check the registry to confirm the changes.
8. Once verified, you can proceed with broader deployment. Monitor the deployment to ensure the settings are applied correctly across the organization's computers.

## plist method (Mac only)

> **Note**
>
> 
>
> The registry key method is available with Docker Desktop version 4.32 and later.

1. Create the file `/Library/Application Support/com.docker.docker/desktop.plist`.

2. Open `desktop.plist` in a text editor and add the following content, where `myorg` is replaced with your organization’s name all lowercase:

   

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
   <plist version="1.0">
     <dict>
         <key>allowedOrgs</key>
         <array>
             <string>myorg</string>
         </array>
     </dict>
   </plist>
   ```

   > **Important**
   >
   > 
   >
   > Only one entry for `allowedOrgs` is currently supported. If you add more than one value, sign-in enforcement silently fails.

3. Modify the file permissions to ensure the file cannot be edited by any non-administrator users.

4. Restart Docker Desktop.

5. Open Docker Desktop and when Docker Desktop starts, verify that the **Sign in required!** prompt appears.

> **Note**
>
> 
>
> If a `plist` and `registry.json` file both exist, the `plist` file takes precedence.

### Example deployment

The following is only an illustrative example.

There are many ways to deploy the `.plist` file. The method you choose is dependent on your organizations infrastructure, security policies, and the administrative rights of the end-users.

MDM Shell script

------

1. Follow the steps previously outlined to create the `desktop.plist` file.
2. Use an MDM tool like Jamf or Fleet to distribute the `desktop.plist` file to `/Library/Application Support/com.docker.docker/` on target macOS devices.
3. Through the MDM tool, set the file permissions to permit editing by administrators only.

------

## registry.json method (All)

The following instructions explain how to create and deploy a `registry.json` file to a single device. There are many ways to deploy the `registry.json` file. You can follow the example deployments outlined in the `.plist` file section. The method you choose is dependent on your organization's infrastructure, security policies, and the administrative rights of the end-users.

### Option 1: Create a registry.json file to enforce sign-in

1. Ensure that the user is a member of your organization in Docker. For more details, see [Manage members]({{< ref "/manuals/Administration/Organizationadministration/Manageorganizationmembers" >}}).

2. Create the `registry.json` file.

   Based on the user's operating system, create a file named `registry.json` at the following location and make sure the file can't be edited by the user.

   | Platform | Location                                                     |
   | -------- | ------------------------------------------------------------ |
   | Windows  | `/ProgramData/DockerDesktop/registry.json`                   |
   | Mac      | `/Library/Application Support/com.docker.docker/registry.json` |
   | Linux    | `/usr/share/docker-desktop/registry/registry.json`           |

3. Specify your organization in the `registry.json` file.

   Open the `registry.json` file in a text editor and add the following contents, where `myorg` is replaced with your organization’s name. The file contents are case-sensitive and you must use lowercase letters for your organization's name.

   

   ```json
   {
   "allowedOrgs": ["myorg"]
   }
   ```

   > **Important**
   >
   > 
   >
   > Only one entry for `allowedOrgs` is currently supported. If you add more than one value, sign-in enforcement silently fails.

4. Verify that sign-in is enforced.

   To activate the `registry.json` file, restart Docker Desktop on the user’s machine. When Docker Desktop starts, verify that the **Sign in required!** prompt appears.

   In some cases, a system reboot may be necessary for the enforcement to take effect.

   > **Tip**
   >
   > 
   >
   > If your users have issues starting Docker Desktop after you enforce sign-in, they may need to update to the latest version.

### Option 2: Create a registry.json file when installing Docker Desktop

To create a `registry.json` file when installing Docker Desktop, use the following instructions based on your user's operating system.

Windows Mac

------

To automatically create a `registry.json` file when installing Docker Desktop, download `Docker Desktop Installer.exe` and run one of the following commands from the directory containing `Docker Desktop Installer.exe`. Replace `myorg` with your organization's name. You must use lowercase letters for your organization's name.

If you're using PowerShell:



```powershell
PS> Start-Process '.\Docker Desktop Installer.exe' -Wait 'install --allowed-org=myorg'
```

If you're using the Windows Command Prompt:



```console
C:\Users\Admin> "Docker Desktop Installer.exe" install --allowed-org=myorg
```

------

### Option 3: Create a registry.json file using the command line

To create a `registry.json` using the command line, use the following instructions based on your user's operating system.

Windows Mac Linux

------

To use the CLI to create a `registry.json` file, run the following PowerShell command as an administrator and replace `myorg` with your organization's name. The file contents are case-sensitive and you must use lowercase letters for your organization's name.



```powershell
PS>  Set-Content /ProgramData/DockerDesktop/registry.json '{"allowedOrgs":["myorg"]}'
```

This creates the `registry.json` file at `C:\ProgramData\DockerDesktop\registry.json` and includes the organization information the user belongs to. Make sure that the user can't edit this file, but only the administrator can:



```console
PS C:\ProgramData\DockerDesktop> Get-Acl .\registry.json


    Directory: C:\ProgramData\DockerDesktop


Path          Owner                  Access
----          -----                  ------
registry.json BUILTIN\Administrators NT AUTHORITY\SYSTEM Allow  FullControl...
```

------

## More resources

- [Video: Enforce sign-in with a registry.json](https://www.youtube.com/watch?v=CIOQ6wDnJnM)
