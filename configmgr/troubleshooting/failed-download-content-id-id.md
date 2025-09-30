---
title: "Failed to download content id <id>. Error: Access is Denied."
date: 2021-02-23
taxonomy:
    products:
        - 
    tech-stack:
        - configmgr
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - troubleshooting
        - common-issues-and-error-codes
        - updates
---

Error **0X87D20417**, **0x80070005**, or **5**, generally occurs when attempting to download software updates (whether third party updates or Microsoft updates) into a deployment package. When attempting to download an update into a deployment package in the SCCM console, you receive the error message: **Failed to download content id Error: Access is denied**.

## Determine if You are Affected

Whenever a software update is being downloaded, regardless of whether it’s a Microsoft or third-party update, you receive the following error using the Download Software Updates wizard in the console:

![Failed to download content access denied - download wizard](/_images/FailedToDownloadContentAccessDenied-1.png "Failed to download content access denied - download wizard")

Looking in the **[PatchDownloader.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#deployment-package-download-logs)** file we can also see an error code which resolves to access denied (**0x80070005**):

Download destination = hostnameSourcesSoftware UpdatesPatch My PC Updates2f57c5d1-aab5-46a3-bbed-fdfec4101f23.18364431e-00b7-4f9b-8551-de6aa3dbbcec\_1.cab  
Download destination = hostnameSourcesSoftware UpdatesPatch My PC Updates2f57c5d1-aab5-46a3-bbed-fdfec4101f23.18364431e-00b7-4f9b-8551-de6aa3dbbcec\_1.cab  
Contentsource = http://hostname:8530/Content/48/AE431DC3B6D7F1FC4AC7C80FC6D0570F4B166C48.cab  
Failed to create directory hostnameSourcesSoftware UpdatesPatch My PC Updates2f57c5d1-aab5-46a3-bbed-fdfec4101f23.18364431e-00b7-4f9b-8551-de6aa3dbbcec\_1.cab, error 5  
CreateLinkToExistingFile() failed for ContentID 16777459. hRes = 0x80070005  
Downloading content for ContentID = 16777459,  FileName = 8364431e-00b7-4f9b-8551-de6aa3dbbcec\_1.cab  
Proxy is enabled for download, using registry settings or defaults  
Failed to create directory hostnameSourcesSoftware UpdatesPatch My PC Updates2f57c5d1-aab5-46a3-bbed-fdfec4101f23.1, error 5  
ERROR: DownloadUpdateContent() failed with hr=0x80070005

If using Automatic Deployment Rules (ADR), the error code will be different and unfortunately a little more generic (**0X87D20417**):

![Failed to download content access denied - ADR](/_images/FailedToDownloadContentAccessDenied-3.png "Failed to download content access denied - ADR")

Looking at the [ruleengine.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#automatic-deployment-rules-logs) log file for ADRs, we can find a more useful error code which also resolves to access denied (**5**):

Failed to download the update content with ID 16777460 from internet. Error = 5  
Failed to download ContentID 16777460 for UpdateID 16777951. Error code = 5  
Failed to download any update  
Failed to download update contents.

## Root cause

Depending on whether you're using the SCCM console to download the updates, or using ADR, it's going to by either the user running the SCCM console or the SYSTEM account of the site server missing SMB/NTFS permissions on the Deployment Package's source path.

The solution is to **review the SMB and NTFS permissions** of the source path used for the Deployment Package.

If you're seeing this error while using ADRs, **verify the SYSTEM account of the site server has the necessary read/write permissions**.

If you're seeing this error while using the SCCM console, **verify the user running the console has the necessary read/write permissions**.

## Solution: Verify SMB and NTFS Permissions on Deployment Package Source Path

First you need to find out what the source path is for your Deployment Package.

**Right click the Deployment Package** you, or your ADR, is trying to download to and choose **Properties**:

![Failed to download content access denied - Deployment Package properties](/_images/FailedToDownloadContentAccessDenied-5.png "Failed to download content access denied - Deployment Package properties")

Go to the directory in File Explorer and **verify the user or computer object has at least Modify permission** in the **Security** tab of the folder's Properties:

![Failed to download content access denied - NTFS permissions](/_images/FailedToDownloadContentAccessDenied-6.png "Failed to download content access denied - NTFS permissions")

> **Note:** It's not necessary to directly add the user or computer object here. It is possible to use Active Directory groups to contain users or computer objects.

If the NTFS permissions look fine, next you will need to verify SMB permissions on the shared folder itself.

For example, in the above screenshots the Deployment Package source path is **demo3SourcesSoftware UpdatesPatch My PC Updates**. We need to verify the SMB permission on the **Sources** shared folder.

The instructions to verify SMB permissions will vary between platforms, i.e. it's possible to create a shared folder from Synology or NetApp systems. The below detail is specific to a shared folder hosted on a Windows operating systeml.

Open up the **Computer Management** mmc snap in (protip: right click on the start menu and choose **Computer Management**), expand **Shared Folders** and select **Shares**:

![Failed to download content access denied - Shared folders](/_images/FailedToDownloadContentAccessDenied-7.png "Failed to download content access denied - Shared folders")

Right click on the shared folder in the centre pane and choose **Properties**.

In this case I will right click on **Sources**. Go to the **Share Permissions** tab and **verify the user or computer object has at least Change permission**.

![Failed to download content access denied - SMB permissions](/_images/FailedToDownloadContentAccessDenied-8.png "Failed to download content access denied - SMB permissions")

As you can see from the above, only the **Everyone** object is in the Access Control List (ACL) with only the **Read** permission. This will need changing to allow the user or computer object access to the shared folder with the **Change** permission.

Whether you remove the **Everyone** object and use specific identity objects is your discretion.

In any case, you would need to make sure the needed user or computer object is granted Change or Modify permissions, for both the SMB and NTFS permissions respectively.

> **Note:** It's not necessary to directly add the user or computer object here. It is possible to use Active Directory groups to contain users or computer objects.
