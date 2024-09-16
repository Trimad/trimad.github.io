
---
title: "Managing Microsoft Office Versions with OfficeC2RClient"
date: 2024-09-10
draft: false
tags: ["Microsoft Office", "OfficeC2RClient", "Version Management", "IT Admin"]
toc: true
---

Managing different versions of Microsoft Office on Windows can be critical, especially when troubleshooting compatibility issues or performing specific tasks that require a certain version. One useful tool for this is `officec2rclient.exe`, which allows you to roll back or update the installed Office version. Below, I'll cover essential steps and commands you can use to manage Microsoft Office versions efficiently.

## Rolling Back to a Specific Microsoft Office Version

Sometimes you may need to roll back to an earlier version of Office, especially if an update introduces bugs or compatibility issues. Here's how you can roll back to specific versions:

### Rolling Back to Version 2407 (Current Channel)

To roll back to version `16.0.17830.20166`:

```bash
cd "C:\Program Files\Common Files\Microsoft Shared\ClickToRun" && officec2rclient.exe /update user updatetoversion=16.0.17830.20166
```

### Rolling Back to Version 2406 (Current Channel)

For version `16.0.17726.20160`, use the following command:

```bash
cd "C:\Program Files\Common Files\Microsoft Shared\ClickToRun" && officec2rclient.exe /update user updatetoversion=16.0.17726.20160
```

> **Tip**: It's always good practice to disable automatic updates before rolling back, to prevent the system from automatically upgrading after you’ve downgraded.

## Checking the Installed Office Version

To check which version of Microsoft Office is currently installed, you can run the following PowerShell command:

```powershell
Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Office\ClickToRun\Configuration" | Select-Object -Property VersionToReport
```

Or if you're using PowerShell from the command line:

```bash
powershell.exe -c "Get-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Office\ClickToRun\Configuration' | Select-Object -Property VersionToReport"
```

## Managing the Microsoft Office Click-to-Run Service

The Click-to-Run service manages Office installations and updates. Sometimes, you'll need to stop or disable this service before performing certain actions, like rolling back or installing specific versions of Office.

### Stopping the Click-to-Run Service

To stop the service:

```bash
sc stop ClickToRunSvc
```

### Checking the Status of the Click-to-Run Service

To check whether the service is running:

```bash
sc query ClickToRunSvc
```

### Disabling the Click-to-Run Service

If you need to disable the service to prevent it from restarting:

```bash
sc config ClickToRunSvc start= disabled
```

> **Note**: Disabling this service will prevent future updates. Be sure to re-enable it when you're ready to update Office again.

## Useful Hacks and Tips

Here are some additional tips for managing Microsoft Office versions effectively:

- **Backup Your Office Settings**: Before rolling back to a previous version, it’s a good idea to backup your Office settings, especially if you’ve customized the interface or added macros.
  
- **Re-enabling Updates**: Once you're satisfied with the rollback or if you want to receive future updates, you can re-enable automatic updates by resetting the Click-to-Run service:

    ```bash
    sc config ClickToRunSvc start= demand
    sc start ClickToRunSvc
    ```

- **Monitor Version Changes**: Microsoft releases frequent updates for Office, and it's easy to lose track of which version you're on. Keep a log of the versions and dates when you update or roll back to better manage the lifecycle of your Office installations.

For more details on Office version history, visit the official [Microsoft Office version history page](https://learn.microsoft.com/en-us/officeupdates/update-history-microsoft365-apps-by-date).