---
title: >-
  An error occurred while publishing an update to WSUS:  failed to create cab;
  error was: 2147942512
date: 2020-08-25T00:00:00.000Z
taxonomy:
  products:
    - null
  tech-stack:
    - wsus
  solution:
    - null
  post_tag:
    - null
  sub-solutions:
    - common-issues-and-error-codes
    - troubleshooting
    - security
---

# WSUS Publish Error 2147942512

In this article, we're going to review a common error code that can occur when the drive the **WSUSContent** folder is located on has no more free space available.

### Determine if you are affected

If you are affected by this error, you will see errors similar to the errors below in the [**PatchMyPC.log**](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-logs), [SMS\_ISVUPDATES\_SYNCAGENT.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-in-console-logs), or [**SoftwareDistribution.log**](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-logs). The actual log file you see the error in may vary depending on the **Publishing method** you are using.

An error occurred while publishing an update to WSUS: failed to create cab; error was: 2147942512

Check if the drive **hosting the WSUSContent** folder has **low disk space:**

![WSUS Drive with Low Disk Space](/_images/wsus-content-drive-low-disk-space.png "WSUS Drive with Low Disk Space")

### Resolution 1: Extend the disk to have more free space

The first possible resolution is to **add more disk space to the existing drive.** This operation can be performed on virtual machines pretty easily, for example.&#x20;

### Resolution 2: Move the WSUSContent folder to a new drive

The second option is to [**move the WSUSContent folder to a completely different drive**](https://patchmypc.com/how-to-move-the-wsus-content-folder-to-a-new-location). This can be performed using the WSUSUtil.exe tool that comes with WSUS. For a complete guide on moving the WSUSContent folder see: [**How to Move the WSUS Content Folder to a New Location**](../../how-to-move-the-wsus-content-folder-to-a-new-location/)

### Resolution 3: Cleanup unneeded update files from UpdateServicePackages&#x20;

Another option is to delete unneeded updates files in the **UpdateServicePackages** folder. A full guide is available at [**How to Clean Up Third-Party Updates from the WSUS UpdateServicesPackages Folder**](https://patchmypc.com/clean-up-third-party-updates-from-the-wsus-updateservicespackages-folder). This operation can often save a lot of space on the drive hosting the WSUS content files.