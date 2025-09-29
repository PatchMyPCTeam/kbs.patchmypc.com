---
title: "Unable to find the Assembly: AdminUI.WqlQueryEngine, Version=5.0.0.0, Culture=neutral"
date: 2021-03-23
taxonomy:
    products:
        - 
    tech-stack:
        - 
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - 
---

The error message "_**Could not load file or assembly 'AdminUI.WqlQueryEngine, Version=5.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.**_" occurs when the **[Configuration Manager console isn't installed](https://docs.microsoft.com/en-us/mem/configmgr/core/servers/deploy/install/install-consoles)** on the device running the **[Patch My PC Publisher](https://patchmypc.com/docs)**. We use the assembly files within the Configuration Manager console to connect to ConfigMgr to perform the following actions:

- **Check the build number**

- **Trigger a software update point synchronization**

- **Create and manage ConfigMgr applications**

## Determine if You are Affected

There are a variety of different error messages that will occur depending on the action that occurred. Here's an example of errors you will see in the **[PatchMyPC.log](/collecting-log-files-for-patch-my-pc-support#publishing-service-app-logs-intune)** file.

An error occurred while trying to trigger a SUP sync: Unable to find the Assembly: AdminUI.WqlQueryEngine, Version=5.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35

An error occurred while connecting to SCCM: Unable to find the Assembly: AdminUI.WqlQueryEngine, Version=5.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35 \]

An error occurred while connecting to Sms provider as System: Unable to find the Assembly: AdminUI.WqlQueryEngine, Version=5.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35 \]

## How to Resolve this Error

To resolve this error, you need to **install the ConfigMgr console** using the steps documented here **[Install console - Configuration Manager | Microsoft](https://docs.microsoft.com/en-us/mem/configmgr/core/servers/deploy/install/install-consoles) [Docs](https://docs.microsoft.com/en-us/mem/configmgr/core/servers/deploy/install/install-consoles)**.

> **Important:** After the ConfigMgr console is installed, you will need to perform the following steps to ensure the **console assemblies are loaded within the Patch My PC Publisher**.

Once the console is installed, **perform the following steps**:

1. **Close** the Patch My PC Publisher UI

3. Re-open the Publisher

5. Navigate to the **About tab** and click **Restart service****
    ![Could not load file or assembly 'AdminUI.WqlQueryEngine, Version=5.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.](/_images/Could-not-load-file-or-assembly-AdminUI-WqlQueryEngine.png "Could not load file or assembly 'AdminUI.WqlQueryEngine, Version=5.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.")
    **

7. These actions should **resolve the issue**

> **Note:** In rare cases, if the console assemblies still aren't detected, you may need to restart the computer for the assemblies to be detected.