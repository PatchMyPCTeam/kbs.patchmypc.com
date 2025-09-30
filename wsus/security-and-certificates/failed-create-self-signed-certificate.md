---
title: "Failed to create a self-signed certificate: Cannot save configuration because the server is still processing a previous configuration change"
date: 2023-07-17
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
        - security-and-certificates
        - troubleshooting
        - common-issues-and-error-codes
---

In this article, we will review why the error above is received when you attempt to generate a self-signed certificate with the Publisher.

## Determine if you are affected

You may receive the following dialogue box when you attempt to generate a self-signed certificate using the Patch My PC Publisher.

![](/_images/error-2.png)

**[PatchMyPC.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-logs)** will indicate:-

An error occured while extracting the certificate from WSUS: C:\\UsersAppData\\Local\\TempPMP-xxxxxxxxxxxxxxx.tmp : The system cannot find the file specified

Failed to extract the certificate from WSUS

[SoftwareDistribution.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-logs) will indicate:-

Discarding stack trace for user DOMAIN, IP Address ::1, exception System.ComponentModel.Win32Exception (0x80004005): The system cannot find the file specified

SqlException occurred. Number 50000 and message spSetConfiguration was called while a Reset Process was Needed/InProgress cannot change Configuration at this time

Discarding stack trace for user DOMAIN, IP Address ::1, exception System.InvalidOperationException: Cannot save configuration because the server is still processing a previous

![](/_images/errorsoftwaredistributionlog.png)

## Reason

This error generally occurs when WSUS is "busy" running another stored procedure.

The excerpt below is the result of a recent **wsusutil movecontent** command being run. [SoftwareDistribution.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-logs) indicates that WSUS is attempting to validate the calculated hash for each update in the database. Until this reconciliation has completed, we cannot use the Publisher to generate a self-signed certificate.

![](/_images/wsusmovecontent.png)

## Workaround

The Publisher utilises the WSUS API to create a self-signed certificate with an exportable private key. If WSUS is running a stored procedure, the Publisher won't be able to create a self-signed certifiacte with an exportable key until the current stored procedure has completed.

### **Option 1**

If you have recently attempted to run a **wsusutil** **reset**, or **wsusutil movecontent** command, you may need to wait for that stored procedure to complete before you can run other commands against WSUS. You can check the [SoftwareDistribution.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-logs) to understand what stored procedures are running against the WSUS database.

### **Option 2**

If you do not want to wait for the current stored procedure to complete you can still use the Publisher to generate a self-signed code signing certificate but you would need to disable the private key export. There is not the same reliance on the WSUS API when generating a self-signed certificate without an exportable private key.

To genereate a self-signed certificate, from the Publisher, without marking the private key as exportable, you should:-

1. On the **General** tab in the Publisher, click **Generate a Self-Signed Certificate**

3. Check the **Disable Private Key Export** check-box

5. Click **Generate Certificate**

![](/_images/exportprivatekeydisable.png)