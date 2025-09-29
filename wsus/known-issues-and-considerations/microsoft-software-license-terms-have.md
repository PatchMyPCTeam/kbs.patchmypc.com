---
title: "The Microsoft Software License Terms have not been completely downloaded and cannot be accepted"
date: 2024-07-05
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

Microsoft Configuration Manager can throw the error in the title when the Software Update Point tries to synchronise new updates from WSUS and the update it is trying to supersede no longer has its End User Licensing Agreement (EULA) available on disk.

EULAs are stored in the WsusContent directory as .txt files for software updates which come with any. For example, the Windows Malicious Software Removal Tool for some older versions of Windows come with EULA .txt files saved in WsusContent.

Before you can deploy any updates which come with a EULA, the agreement must first be accepted. An example of this can be seen below in Configuration Manager:

It is also possible to automatically accept all EULAs without any manual acceptance when using Automatic Deployment Rules (ADR):

## Determine if you are affected

If affected, you will see the below log line in [wsyncmgr.log](/collecting-log-files-for-patch-my-pc-support#publishing-service-logs):

Failed to sync update 8b1ca2c2-e002-4dc6-a5d6-0168006fb369. Error: The Microsoft Software License Terms have not been completely downloaded and~~cannot be accepted. Source: Microsoft.UpdateServices.Internal.BaseApi.LicenseAgreement.GetById

The GUID would be the WSUS update ID of the new update failing to synchronise into Configuration Manager.

You may also see the **error code 0x80131500** in the Configuration Manager console under MonitoringOverviewSoftware Update Point Synchronization Status:

In [SoftwareDistribution.log](/collecting-log-files-for-patch-my-pc-support#publishing-service-logs) you may also see the reference to specific .txt files missing from your WsusContent directory:

AdminDataAccess.ExecuteSPGetEulaFileLicenseAgreement file does not exist: J:\\WSUS\\WsusContent2314D19C27B28CC3990260D7191F6E0FF6C7483623.txt

Finally, when trying to query WSUS for the update using PowerShell, it will return a similar error message as [wsyncmgr.log](/collecting-log-files-for-patch-my-pc-support#publishing-service-logs):

## Step 1: Validate the permissions on the WsusContent and UpdateServicesPackages shares

To verify the NTFS and SMB permissions are correct for your **WsusContent** and **UpdateServicesPackages** folders, review **Step 2: Validate the Permissions on the WsusContent and UpdateServicesPackages Shares** in the following article:

- [CreateDirectory failed Error when Publishing Third-Party Updates](/an-error-occurred-while-publishing-an-update-to-wsus-createdirectory-failed)  
    

  
If any permission changes were made, perform a sync of your Software Update Point and validate if the issue still exists. If it does, move on to step 2. 

## Step 2: Restore missing EULA .txt files in the WsusContent share

If the NTFS and SMB permissions are correct for your WSUS content directories, it's likely the EULA .txt files have been deleted from the WsusContent directory.

To restore the missing EULA .txt files back to the WsusContent directory, you need to perform a reset of WSUS by using the WsusUtil.exe utility.

> **Note:** you may have multiple Software Update Points in your environment, and some of them may be throwing this error. You will need to run the same remediation steps below on each Software Update Point impacted. You can see which SUP fails to sync and its errors by reading wsyncmgr.log.

This is achieved by running the below command:

```
C:\Program Files\Update Services\Tools\WsusUtil.exe reset
```

Performing a "reset" is not as significant as it sounds.

As per the help command, the reset parameter "Checks that every metadata row in the database has corresponding content stored in the file system. If content is missing or corrupted, WSUS downloads the content again. This is useful after restoring a database."

Microsoft have documentation on this issue and also recommend the same action:

- [Synchronization fails because of issues with the EULA - Configuration Manager | Microsoft Learn](https://learn.microsoft.com/en-us/troubleshoot/mem/configmgr/update-management/troubleshoot-software-update-synchronization#synchronization-fails-because-of-issues-with-the-eula)  
    

After running the **WsusUtil.exe reset** command, you can monitor its progress in **[SoftwareDistribution.log](/collecting-log-files-for-patch-my-pc-support#publishing-service-logs)**. At the beginning of the reset process, in the log you will see the below log lines printed:

```
SusEventDispatcher.DispatchManagerWorkerThreadProcDispatchManager Worker Thread Processing NotificationEvent: StateMachineReset
SusEventDispatcher.DispatchManagerWorkerThreadProcDispatchManager Worker Thread Processing NotificationEvent: ContentSyncAgent
EventLogEventReporter.ReportEventEventId=361,Type=Information,Category=Synchronization,Message=Content synchronization started
```

  
You can still publish new third-party updates to WSUS while it is experiencing a reset, and you can also still synchronise your Software Update Point, too - it is not necessary to disable the Patch My PC Publisher or Software Update Point scheduled sync.

During this process, you will see the activity of WSUS enumerating every update, validating its content. For any updates missing content, e.g. missing EULA's, it will redownload them from the Internet.

> **Important:** make sure your proxy or firewall is not blocking access to Microsoft to redownload these EULA .txt files.

Below is an example snippet from SoftwareDistribution.log, shortly after the reset process starting, redownloading a EULA .txt file.

Depending on how many updates you have in WSUS requiring content, especially third-party updates, the reset process can take a long time. WSUS will enumerate every update in serial to validate its content.

Once the reset process is complete, you will see the two log lines below in [SoftwareDistribution.log](/collecting-log-files-for-patch-my-pc-support#publishing-service-logs):

```
ContentSyncAgent.WakeUpWorkerThreadProcContentSyncAgent found no more Jobs, thread exittingContentSyncAgent.WakeUpWorkerThreadProcServerHealth: Updating Server Health for Component: ContentSyncAgent, Marking as Not Running
```

You can confirm an update has its EULAs restored by querying the same update as perform with PowerShell, and this time it shouldn't throw an error:

At this point, all missing EULA .txt files should be redownloaded into the WsusContent directory. Try synchronizing your Software Update Point again to validate the issue is resolved.

## Video

https://www.youtube.com/watch?v=2cGMSJyxaw8