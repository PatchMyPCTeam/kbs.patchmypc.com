---
title: "What is Patch My PC ScriptRunner?"
date: 2024-09-16
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

In this article we will explore what the Patch My PC ScriptRunner is and how it enables IT administrators to automate and customize the installation of third-party applications and software updates.

## What is PatchMyPC-Scriptrunner?

**PatchMyPC-ScriptRunner.exe** is our wrapper that is used to perform base application and update installations. In addition to installing and uninstalling apps, it is also responsible for any of the customisations configured from Patch My PC Publisher or Patch My PC Cloud.

## Is PatchMyPC-ScriptRunner.exe safe?

PatchMyPC-ScriptRunner.exe is signed, ensuring its authenticity and integrity. It is signed by our EV (Extended Validation) code signing certificate.

![](/_images/scriptrunner_signed.png)

To avoid interference from security solutions, it is recommended that PatchMyPC-ScriptRunner.exe and its associated binaries be whitelisted in ASR (Attack Surface Reduction) rules for Defender or the relevant rule for other, alternative, Endpoint Detection and Response (EDR) solutions:-

- NotificationLocalization.xml

- PatchMyPC-Enum.dll

- PatchMyPC-Localization.dll

- PatchMyPC-Models.dll

- PatchMyPC-PreventStart.exe

- PatchMyPC-Sccm.dll

- PatchMyPC-ScriptRunner.exe

- PatchMyPC-UINotification.exe

> **Note:** Additionally, some directories such as CCMCache and IMECache, where PatchMyPC-ScriptRunner.exe and its associated binaries are temporarily stored during an installation or update, should also be whitelisted to ensure successful application and update deployment. All Patch My PC recommended anti-virus exclusions are listed at [Patch My PC - Recommended antivirus exclusions](https://patchmypc.com/recommended-antivirus-exclusions)

## What does PatchMyPC-ScriptRunner do?

PatchMyPC-ScriptRunner.exe plays a pivotal role in the Patch My PC solution. Its core responsibility is to install and update applications published via the Patch My PC Publisher. This involves executing installation commands, handling updates, and ensuring that the software is correctly deployed on target systems.

**Contextual Operations**  
Whether running as a standard user, an admin, or with SYSTEM-level permissions, ScriptRunner knows the difference and applies the appropriate settings.

**Command Execution  
**ScriptRunner uses a set of instructions (commands) referenced from various XML files to install or update software, ensuring the installation or update is performed as predetermined by the administrator.

**Manage Conflicting Process  
**If you use the Patch My PC feature ['](https://patchmypc.com/manage-conflicting-processes-when-updating-third-party-applications)[manage conflicting process](https://patchmypc.com/manage-conflicting-processes-when-updating-third-party-applications)[es'](https://patchmypc.com/manage-conflicting-processes-when-updating-third-party-applications), then ScriptRunner will also impersonate the user in order to launch a UI to ask the user to close the app in order for the update to be installed successfully. 

ScriptRunner checks for running processes that might interfere with the installation or update of software. If such processes are detected, ScriptRunner can prompt the user to close them, using components like **PatchMyPC-UINotification.exe** and **NotificationLocalization.xml**. ScriptRunner displays notifications to users, asking them to close the conflicting applications to proceed with the installation.

> **Note: As ScriptRunner walks the running process list on a device to understand if an application should be closed in order for the update to be successful, it will inevitabilty touch LSASS.exe (Local Security Authority Subsystem Service). Some admins might have to make an exception to the the ASR rule _**“Block credential stealing from the Windows local security authority subsystem (lsass.exe)”.**_ You can read more about how this ASR rule can interfer with the manage conflicting processes feature at [Windows Defender Exploit Guard breaks Google Chrome - Patch Tuesday Blog](https://patchtuesday.com/blog/tech-blog/windows-defender-exploit-guard-breaks-google-chrome/)**

## Available Customizations

By leveraging our own wrapper, ScriptRunner, we can perform customized installations of applications and updates, over and above what the vendor offers natively, by deploying helper files with every application or update we publish. Customizations can include deleting the desktop shortcut, adding additional parameters for the installation, prompting users to close conflicting apps, running custom scripts, etc. 

A full list of customizations that we offer within our tooling is documented at [Right-Click Options Available for Updates and Applications - Patch My PC](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications)

**Diving Deeper**

You may come across a files called **package.xml** in the installation source folder for apps and updates that have been packaged by Patch My PC. ScriptRunner will use the content of this XML file to understand how it should customize the installation.

![](/_images/package_xml.png)

In the above example, PatchMyPC-ScriptRunner.exe will call msiexec.exe and use the following parameters to customise the installation:-

> /MainFile=InventoryExtensions.msi /MainArg=REBOOT=ReallySuppressÿ"/LoggingSwitch=/lv\* {path}"ÿ/LoggingPath=C:\\Windows\\CCM\\LogsPatchMyPCInstallLogsÿ/LogRetentionDays=28

Some of the parameters above are understood by msiexec and others are understood by PatchMyPC-ScriptRunner.exe. 

**Logging**

ScriptRunner keeps logs of everything it does helping you track what happened during installations and troubleshoot any issues that arise. You can also enable verbose logs for all the MSI installers that support it and detailed logs will be generated that will definitely help in troubleshooting installation failures, or stuck deployment.

![](/_images/logging.png)

## References

- [Troubleshoot Win32 apps in Microsoft Intune | Microsoft Learn](https://learn.microsoft.com/en-us/mem/intune/apps/apps-win32-troubleshoot)

- [Right-Click Options Available for Updates and Applications - Patch My PC](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications)

- [Manage Conflicting Processes when Updating Third-Party Applications - Patch My PC](https://patchmypc.com/manage-conflicting-processes-when-updating-third-party-applications)

- [Third-Party Patch Management for ConfigMgr and Intune - Patch My PC](https://patchmypc.com/third-party-patch-management-for-sccm-and-intune)