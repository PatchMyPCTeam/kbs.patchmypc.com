---
title: "Windows Server Update Services not installed"
date: 2024-06-03
taxonomy:
    products:
        - 
    tech-stack:
        - wsus
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - common-issues-and-error-codes
        - troubleshooting
        - deployments
---

This article discusses the "Windows Server Update Services is not installed" error message you get when installing the Patch My PC Publisher.

## [Introduction](#introduction)

When you install the Patch My PC Publisher on a Windows 10 / 11 / Windows Server, you get the error message “Windows Server Update Services is not installed”.

![Windows Server Update Services is not installed error message.](/_images/01_WSUS-is-not-installed.png "Windows Server Update Services is not installed error message.")

Even for the Intune-only integration, the Patch My PC Publisher can only be installed if the WSUS RSAT tools are installed. These tools are needed to be installed to parse the Patch My PC catalog XML.

You can find more details on necessary requirements on [this page](https://docs.patchmypc.com/installation-guides/intune/requirements).

## [How to fix it](#howtofixit)

If you are installing the Patch My PC Publisher on a Windows OS, you'll need to install the WSUS RSAT tools to fix this issue.

> **Note:** You can only install the WSUS RSAT tools on Pro or Enterprise Operating Systems.

If you plan on installing the Publisher on a Windows Server OS, you’ll need to install the UpdateServices-API.

> **Note:** It’s not mandatory to install the full WSUS role, the API is sufficient.

### [How to fix the issue on Windows 10 and Windows 11 OS](#howtofixW10andW11)

To install the WSUS RSAT tool on a Windows Desktop machine, please follow these steps:

1. Open an elevated PowerShell window.

3. Enter this command:

> Add-WindowsCapability -Online -Name Rsat.WSUS.Tools~~~~0.0.1.0

You can verify if this capability installed successfully by running the following PowerShell command.

> Get-WindowsCapability -Online -Name Rsat.WSUS.Tools~~~~0.0.1.0

You should get a result similar to this:

![Check if WSUS RSAT Tools installed successfully](/_images/02-Get-WSUSTools-installed.png "Check if WSUS RSAT Tools installed successfully")

### [How to fix the issue on a Windows Server OS](#howtofixwindowsserveros)

If you plan hosting the Patch My PC Publisher on a Windows Server, please follow these steps to install the UpdateServices API:

1. Open an elevated PowerShell window.

3. Enter this command:

> Install-WindowsFeature UpdateServices-API

3. You can verify if this feature installed successfully by running the following PowerShell command:

> Get-WindowsFeature UpdateServices-API

You should get a result similar to this:

![Verify if the UpdateServices API is successfully installed](/_images/03_GetWSUSinstalled-on-Server.png "Verify if the UpdateServices API is successfully installed")