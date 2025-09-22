# Windows Server Update Services not installed

This article discusses the “Windows Server Update Services is not installed” error message you get when installing the Patch My PC Publisher.

### [Introduction](https://patchmypc.com/kb/wsus-not-installed/#introduction) <a href="#h-introduction" id="h-introduction"></a>

When you install the Patch My PC Publisher on a Windows 10 / 11 / Windows Server, you get the error message “Windows Server Update Services is not installed”.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/01_WSUS-is-not-installed.png" alt="Windows Server Update Services is not installed error message."><figcaption></figcaption></figure>

Even for the Intune-only integration, the Patch My PC Publisher can only be installed if the WSUS RSAT tools are installed. These tools are needed to be installed to parse the Patch My PC catalog XML.

You can find more details on necessary requirements on [this page](https://docs.patchmypc.com/installation-guides/intune/requirements).

### [How to fix it](https://patchmypc.com/kb/wsus-not-installed/#howtofixit) <a href="#h-how-to-fix-it" id="h-how-to-fix-it"></a>

If you are installing the Patch My PC Publisher on a Windows OS, you’ll need to install the WSUS RSAT tools to fix this issue.

> **Note:** You can only install the WSUS RSAT tools on Pro or Enterprise Operating Systems.

If you plan on installing the Publisher on a Windows Server OS, you’ll need to install the UpdateServices-API.

> **Note:** It’s not mandatory to install the full WSUS role, the API is sufficient.

#### [How to fix the issue on Windows 10 and Windows 11 OS](https://patchmypc.com/kb/wsus-not-installed/#howtofixW10andW11) <a href="#h-how-to-fix-the-issue-on-windows-10-and-windows-11-os" id="h-how-to-fix-the-issue-on-windows-10-and-windows-11-os"></a>

To install the WSUS RSAT tool on a Windows Desktop machine, please follow these steps:

1. Open an elevated PowerShell window.
2. Enter this command:

> Add-WindowsCapability -Online -Name Rsat.WSUS.Tools\~\~\~\~0.0.1.0

You can verify if this capability installed successfully by running the following PowerShell command.

> Get-WindowsCapability -Online -Name Rsat.WSUS.Tools\~\~\~\~0.0.1.0

You should get a result similar to this:

<figure><img src="https://patchmypc.com/app/uploads/2025/04/02-Get-WSUSTools-installed.png" alt="Check if WSUS RSAT Tools installed successfully"><figcaption></figcaption></figure>

#### [How to fix the issue on a Windows Server OS](https://patchmypc.com/kb/wsus-not-installed/#howtofixwindowsserveros) <a href="#h-how-to-fix-the-issue-on-a-windows-server-os" id="h-how-to-fix-the-issue-on-a-windows-server-os"></a>

If you plan hosting the Patch My PC Publisher on a Windows Server, please follow these steps to install the UpdateServices API:

1. Open an elevated PowerShell window.
2. Enter this command:

> Install-WindowsFeature UpdateServices-API

3. You can verify if this feature installed successfully by running the following PowerShell command:

> Get-WindowsFeature UpdateServices-API

You should get a result similar to this:

<figure><img src="https://patchmypc.com/app/uploads/2025/04/03_GetWSUSinstalled-on-Server.png" alt="Verify if the UpdateServices API is successfully installed"><figcaption></figcaption></figure>
