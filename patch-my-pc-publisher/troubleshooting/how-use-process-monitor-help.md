---
title: "How to use Process Monitor to help diagnose publishing issues"
date: 2023-12-29
taxonomy:
    products:
        - patch-my-pc-publisher
    tech-stack:
        - configmgr
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - troubleshooting
        - log-collection-and-analysis
        - education

---

In the following article we will show you how to use Process Monitor (procmon) from Sysinternals to collect file system, registry and process events to assist a Patch My PC support engineer with diagnosing publishing issues in both WSUS and ConfigMgr.

## Overview

Sometimes we require more details to help diagnose publishing issues in your environment. [Process Monitor](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon), from Sysinternals, commonly referred to as _**Procmon**_, is an advanced monitoring tool for Windows that captures real-time file system, registry and process activity.

We often see antivirus engines locking files during publishing and a procmon trace is useful to help flag to security teams the correct antivirus configurations and exclusions to put into place.

A procmon trace is also very useful at revealing competing antivirus products installed on the same server which will exasperate file locks. We often encounter this when customers perform in-place upgrades on servers from Server 2012 > higher. Windows Defender must be explicitly disabled or set to passive mode after an in-place upgrade if you are using a third-party antivirus solution. See the following advice from Microsoft for more information [Microsoft Defender Antivirus on Windows Server | Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/microsoft-defender-antivirus-on-windows-server?view=o365-worldwide#what-happens-if-a-non-microsoft-antivirus-product-is-uninstalled)

> **Note:** Please see the following KB for the recommended antivirus exclusions when publishing apps and updates to ConfigMgr, WSUS and Intune [Patch My PC - Recommended antivirus exclusions](https://patchmypc.com/recommended-antivirus-exclusions)

## Download Procmon from Sysinternals

Procmon can either be downloaded as a zip file (1) or as an executable (2) from the Sysinternals website.

[![procmon download](/_images/procmon-download-1.png "procmon download")](https://patchmypc.com/app/uploads/2025/04/procmon-download-1.png)

Typically, we recommend downloading the zip file and extracting the contents to disk so they can be used for continuous troubleshooting when working with our engineers on a support case.

**Download** the zip file from thie website [https://learn.microsoft.com/en-us/sysinternals/downloads/procmon](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon) or directly from this link [https://download.sysinternals.com/files/ProcessMonitor.zip.](https://download.sysinternals.com/files/ProcessMonitor.zip) Once downloaded, extract the contents to a folder.

The extracted zip file will reveal the following files which we will use for troubleshooting.

[![procmon extracted](/_images/procmon-extracted-1.png "procmon extracted")](https://patchmypc.com/app/uploads/2025/04/procmon-extracted-1.png)

## Run a procmon trace to diagnose publishing issues

Unless otherwise instructed, a procmon trace is run from the same computer where the Patch My PC Publishing Service is installed.

1\. Run **procmon.exe** (1) on the same computer where the Patch My PC Publisher Serviceis installed (2)

![procmon launch](/_images/procmon-launch-1.png "procmon launch")

2\. Agree to the Sysinternals licence terms

3\. Procmon will start collecting events straight away. To avoid unnecessarily âlarge capture files, stop the capture until you are ready to reproduce the issue. Stop the trace by clicking the **Capture** icon on the toolbar.

![procmon capture button](/_images/procmon-capture-button-e1703848606988.png "procmon capture button")

4\. Clear the current trace by clicking the **Clear** icon on the taskbar.

![procmon clear button](/_images/procmon-clear-button.png "procmon clear button")

5\. Re-produce the publishing issue by selecting the appropriate products in the Publisher and from the **Sync Schedule** (1) tab click **Run Publishing Service Sync** (2).

![procmon publisher sync](/_images/procmon-publisher-sync.png "procmon publisher sync")

6\. Immediately, in procmon click the **Capture** icon on the toolbar to begin capturing the publishing events that occur during the sync.

![procmon capture button small](/_images/procmon-capture-button-small.png "procmon capture button small")

7\. Once publishing has completed, click the **Capture** icon on the toolbar again (see above) to finish the capture. You can verify the sync has completed by reviewing the following log file and observing the **\*\*\* Report** log line. This log line indicates the sync has completed and will report any success and failures.

- %PatchMyPCInstallDirectory%\\PatchMyPC.log

![sync complete](/_images/procmon-sync-complete-e1703850811274.png "sync complete")

8\. Save the capture by clicking **Save** from the **File** menu.

![procmon save](/_images/procmon-save.png "procmon save")

9\. Unless instructed to filter captured events, ensure the following options are selected:-

1. Events to save = **All events**

3. Format = **Native Process Monitor Format (PML)**

5. Path = **Make a note of the path where the .pml file will be saved**

![procmon save options](/_images/procmon-save-options.png "procmon save options")

By default, the .pml file will save into the same folder that procmon.exe was launched from and will be named **LogFile.PML.**

10\. Finally, the .pml file will compress very well. Please compress the file before sending it to Patch My PC support.

![pml file size](/_images/pml-file-size.png "pml file size")

## Send the  capture (pml) file to Patch My PC support

**IMPORTANT:** As well as sharing the capture with us, we also require the relevant log files to help us match failed publishing events with the captured events. Your support engineer will ask for the relevant log files, as listed in the KB [Collecting Log Files to Send to Support for SCCM and Intune - Patch My PC.](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-logs)

Even after compressing the .pml file, it is normally too large to send via email. Please upload the captured events **and** requested log files, in .zip format. We recommend renaming the zip file so its descriptive and can easily identified by your support engineer. For example:-

> _ProcmonCapture-for-PatchMyPC-from-ContosoLtd.zip_

Upload the file(s) to support via [https://patchmypc.com/share](https://patchmypc.com/share) 

> **Note:** The following KB has more details on how to share files with support: [Share Large Files with Patch My PC for Support Case](https://patchmypc.com/how-to-share-large-files)
