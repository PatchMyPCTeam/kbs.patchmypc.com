---
title: "PowerShell Script Requirement Rule is not met"
date: 2024-06-03
taxonomy:
    products:
        - 
    tech-stack:
        - intune
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - scripts
        - troubleshooting
        - automation
---

This article discusses the "PowerShell script requirement rule is not met" status you get when deploying Intune Updates through the Patch My PC Publisher.

## [Introduction](#introduction)

When you deploy [Intune Updates](https://patchmypc.com/intune-apps-vs-intune-updates) published by the Patch My PC Publishing Service for some devices the Intune deployment statistics might say **Not Applicable**.

The status details in such cases will state **Powershell script requirement rule is not met**.

![PowerShell script requirement rule is not met.](/_images/Requirements-not-met.png "PowerShell script requirement rule is not met.")

## [Additional information](#additionalinformation)

Each Intune Update published by Patch My PC contains in its Properties a requirement script.

Intune runs this requirement script to determine if the update you targeted the device with applies or not.  
Therefore, before installing the update, Intune invokes the script, which returns:

- True, if an older version is installed on the device.

- False, if an older version isn’t installed on the device.

> **Note:**Intune Updates will only work if the version already on the device matches the type of installer (like EXE or MSI) and the system architecture (like x64 or x86) of the update you're sending out.

## [Recommendations](#recommendations)

If you want to update software on your devices, like Google Chrome, you can pick all the Google Chrome updates from the **Intune Updates** section and deploy them. Then, Intune takes care of installing the necessary updates on the devices.

We don’t recommend selecting every software available in the Patch My PC Publisher or it's Cloud counterpart, however, as you might run into the Intune policy size limit issue documented [here](https://patchmypc.com/intune-policy-limit-considerations).

## [Troubleshooting](#troubleshooting)

If you find that Intune Updates aren't being applied even though an older version is installed, you'll need to troubleshoot the issue. Here's what we need:

1. The name of the update that is not installing (e.g. Update for Google Chrome, or Update for Notepad++)

3. From the device on which the update is not applying, collect [these logs](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#application-troubleshooting-client-logs-intune). Make sure to include the rolled-over logs as well.

5. Additionally, run [this script](https://github.com/PatchMyPCTeam/CustomerTroubleshooting/blob/Release/PowerShell/Export-PMPCInstalledSoftware.ps1) to export a list of the software installed on the device.  
    This step is very important, as this export will allow us to verify the applicability rules of the update.

7. Once you've collected the necessary files, please [open a support case](https://patchmypc.com/technical-support), if you haven't already.