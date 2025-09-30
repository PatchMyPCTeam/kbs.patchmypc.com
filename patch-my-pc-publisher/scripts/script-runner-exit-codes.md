---
title: "PatchMyPC-ScriptRunner - Known Exit Codes"
date: 2024-04-30
taxonomy:
    products:
        - patch-my-pc-publisher
    tech-stack:
        - 
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - scripts
        - automation
        - education
---

A short summary of the issue including keywords.

## [Brief introduction](#introduction)

This article will explain the exit codes managed by Patch My PC, using its custom installation wrapper called "PatchMyPC-ScriptRunner.exe."

"PatchMyPC-ScriptRunner.exe" is a tool used to install software selected through the PatchMyPC Publisher on client devices, typically managed by ConfigMgr/Intune agents. Besides installing software, it can also carry out various customizations based on your preferences. These customizations may include tasks like removing desktop shortcuts, disabling self-updaters, executing custom scripts, and more.

The behavior of ScriptRunner can vary, particularly in cases of installation failure. In such instances, the failure exit code from the software installer will also be returned by ScriptRunner. However, there are predefined exit codes within ScriptRunner that may appear in specific scenarios. These are detailed below.

## [PatchMyPC-ScriptRunner Exit Codes](#scriptrunnerexitcodes)

**Exit code 0** - means that the execution of your script / the installation of the software completed successfully.

**Exit code 3010** - the software installed successfully, but a soft reboot is required to complete the installation

**Exit code 1602** - this exit code is returned when the user is presented with the "conflicting processes" notification from the [Manage conflicting processes](https://patchmypc.com/manage-conflicting-processes-when-updating-third-party-applications#topic2) feature and presses **Snooze installation**.  
By default, this exit code is considered an "installation failure" by both Intune and ConfigMgr agents.

- Intune will retry the installation 24+ hours later. More information about this can be found [here](https://patchtuesday.com/blog/tech-blog/win32app-retry-interval/).

- ConfigMgr will retry the installation based on
    - your Software Updates Deployment Evaluation Cycle client settings - for updates
    
    - your Application Deployment Evaluation Cycle client settings - for applications.

**Exit code 1618** - this exit code is returned when the [Manage Conflicting Processes](https://patchmypc.com/manage-conflicting-processes-when-updating-third-party-applications) option is set to "Skip the installation" or when the installation is skipped because focus assist is enabled. This exit code is marked in the return codes section for both Win32 apps in Intune and in the application deployment type for ConfigMgr.  
This means that:

- Intune will retry the installation 3 times every 5 minutes. If those attempts are deferred by the user too, it will retry 24+ hours later.

- For ConfigMgr, the installation will be retried 2 hours, for a total of 10 times.

> **Note:** This exit code will also be returned if the installation of a different software is already in progress.

**Exit code 32768** - This exit code is returned when the recommended prescript fails.

Some installers, such as the Oracle Java one, will not uninstall previous versions.  
To fix this, Patch My PC specified a recommended Pre-Install script that will remove older versions before the latest one you deploy is installed.

This script can be found in the [right-click options](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications) of that particular software.

By default, if this script fails to run successfully for any reason, the PatchMyPC-ScriptRunner will no longer proceed with the installation of the latest update and exit with code 32768.

**Exit code 32767** - This exit code is returned if the [custom pre-script specified by the customer](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#custom-scripts) does not run successfully **and** if the option "Don't attempt software update if the pre script returns an exit code other than 0 or 3010."

![](../../_images/custompreinstallscriptfailureV2.png)

## [Honorable Mentions](#honorablementions)

The exit codes documented above are specific to Patch My PC's "PatchMyPC-ScriptRunner" executable.

If an MSI installation fails for reasons other than those listed, the ScriptRunner will pass the exit code from the MSI installer through. In such cases, you can refer to Microsoft's [documentation](https://learn.microsoft.com/en-us/windows/win32/msi/error-codes) for msiexec.exe error messages.

For failed EXE installations, you should consult the vendor's installer documentation page for information on the error code.
