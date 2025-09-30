---
title: "Application Creation Bug if PowerShell Execution Policy is Set via GPO"
date: 2021-06-17
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
        - troubleshooting
---

As of June 16, 2021, the Patch My PC team was made aware of a bug in the Publisher that affected **approximately 116 customers**.

At this time the team has released **[build 2.0.6](https://docs.patchmypc.com/release-history/production-releases#2-0-6-2021-06-16)** of the Publisher which resolves this issue. The article below describes the scope and remediation steps.

**Bug Timeline**

| **Event** | **Time** |
| --- | --- |
| Build 2.0.5 posted to self-update channel | June 14, 2021 - 09:00 MST |
| Bug discovery | June 16, 2021 - 10:00 MST |
| Diagnosis completed | June 16, 2021 - 10:18 MST |
| Development of fix completed | June 16, 2021 - 11:24 MST |
| Resolved Build 2.0.6 posted to self-update channel | June 16, 2021 - 11:50 MST |
| This article posted | June 16, 2021 - 13:44 MST |
| 116 impacted customers emailed | June 16, 2021 - 14:17 MST |

## What Caused the Issue:

In [build 2.0.5](https://docs.patchmypc.com/release-history/production-releases#2-0-5-2021-06-08) (released to self-update channel June 14), a change was made that could cause application creation to fail if the PowerShell execution policy is set via GPO. The error in the **[PatchMyPC.log](https://docs.patchmypc.com/get-help/log-reference-guide#patchmypc-log)** would show:

An error occurred while signing the file: Security error.

This issue would cause applications that attempted to update in place to fail. For example, if **Google Chrome 91.0.4472.101** was upgrading to **Google Chrome 91.0.4472.106,** the creation of the newer application would fail.

This issue would also cause the content source folder for the previous version of **Google Chrome 91.0.4472.101** to be deleted.

## When was the Error Fixed

In **[build 2.0.6](https://docs.patchmypc.com/release-history/production-releases#2-0-6-2021-06-16)** (released to self-update channel June 16), we resolved the issue where code-signing of our detection method scripts would fail.

## What Issue Needs to be Manually Resolved

If an application failed to update from build 2.0.4.1, it would cause any future application updates created with build 2.0.6 to lose the association.

![Duplicate Application PowerShell Security Error Bug](images/Duplicate-Application-PowerShell-Security-Error-Bug.png)

This means the new version will be created as a new application rather than **[updating the existing application in place](https://patchmypc.com/base-install-update-options-explained)**.

> **Manual Resolution:** We recommend that you copy any assignments (if any) from the old version to the newer version of the application.
> 
> You would also need to replace the older application with the newer application in any task sequences. Once completed, we recommend deleting the older duplicate version of the application.

## Remediation Steps and Tools for this Issue

The following applications were released after posting build 2.0.5 which may result in duplicate applications if your PowerShell execution policy is set via GPO.

| **Old Application** | **New Application** |
| --- | --- |
| Cisco WebEx Recorder and Player 41.6.3.42 | Cisco WebEx Recorder and Player 41.6.4.8 |
| Google Chrome 91.0.4472.101 (x64/x86) | Google Chrome 91.0.4472.106 (x64/x86) |
| Jabra Direct 5.4.33519 | Jabra Direct 5.5.37716 |
| Microsoft Azure CLI 2.24.2 | Microsoft Azure CLI 2.25.0 |
| Node.js 14.17.0 LTS (x64/x86) | Node.js 14.17.1 LTS (x64/x86) |
| Opera 77.0.4054.64 (x64/x86) | Opera 77.0.4054.80 (x64/x86) |
| Zscaler Client Connector 3.4.0.124 | Zscaler Client Connector 3.4.1.4 |

We also made a **[PowerShell script](https://github.com/PatchMyPCTeam/CustomerTroubleshooting/blob/Release/PowerShell/Get-PMPCAppsWithEmptyContent.ps1)** available to help you identify possible duplicate applications if this scenario applies to you.

> **Info:** The script needs to be run on the SMSProvider or you can provide the $ProviderMachineName parameter to the script and specify the SMS Provider name.
