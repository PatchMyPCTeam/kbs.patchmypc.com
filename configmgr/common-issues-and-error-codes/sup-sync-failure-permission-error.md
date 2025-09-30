---
title: "Software Update Point Sync Failure - Request for Principal Permission failed (0x80131500 / 0x8013150A)"
date: 2024-09-18
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
        - education
        - known-issues-and-considerations
        - best-practices
---

We recently encountered this error when running a sync on a remote Software Update Point.

![](/_images/SUP_SYNC_0x80131500_or_0x8013150A-2.jpg)

In this example, we are using **CM1.corp.contoso.com** as the primary site server and **SP1.corp.contoso.com** as the remote site system with the software update point role configured.

# 
![](/_images/SUP_SYNC_0x80131500_or_0x8013150A-3-1.jpg)

## Determine if You are Affected

You will see the following error in **[wsyncmgr.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-logs)**:

MessageSync failed: Request for principal permission failed. Source: Microsoft.UpdateServices.Administration.AdminProxy.CreateUpdateServer

STATMSG: ID=6703 SEV=E LEV=M SOURCE="SMS Server" COMP="SMS\_WSUS\_SYNC\_MANAGER" SYS=CM1.corp.contoso.com SITE=CHQ PID=2836 TID=9640 GMTDATE=Fri Aug 02 20:54:26.571 2024 ISTR0="Microsoft.UpdateServices.Administration.AdminProxy.CreateUpdateServer" ISTR1="Request for principal permission failed" ISTR2="" ISTR3="" ISTR4="" ISTR5="" ISTR6="" ISTR7="" ISTR8="" ISTR9="" NUMATTRS=0 LE=0X8013150a

Sync failed. Will retry in 60 minutes

![](/_images/SUP_SYNC_0x80131500_or_0x8013150A-1.jpg)

## Understanding 0X8013150A or 0X80131500

The **0x80131500** error typically means that you are unable to connect to the Software Update Point. This can be due to several different reasons. (Another reason can be found here: [The Microsoft Software License Terms have not been completely downloaded and cannot be accepted (0x80131500) - Patch My PC](https://patchmypc.com/the-microsoft-software-license-terms-have-not-been-completely-downloaded-and-cannot-be-accepted-0x80131500).) The primary logs we will look at during this sync are [wsyncmgr.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-logs) and **[WCM.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-logs)**.

Unfortunately, the CMTrace error lookup feature (Ctrl + L) cannot help with the error code.

![](/_images/SUP_SYNC_0x80131500_or_0x8013150A-4-1.jpg)

> **Note:** This error message is different from a previous build of ConfigMgr. When setting up this scenario, we were running the latest build of ConfigMgr. Configuration Manager 2403. In older builds, the error was 0x80131500 instead of 0x8013150A.

## Troubleshooting Steps

Merge **[WCM.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-logs)** and [wsyncmgr.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-logs) in **CMTrace.**

![](/_images/SUP_SYNC_0x80131500_or_0x8013150A-6.jpg)

Review the connecting account. In this case, the credentials for **CORPService-SCCM** are being used for the network connection.

![](/_images/SUP_SYNC_0x80131500_or_0x8013150A-7.jpg)

This account is unable to connect to the remote WSUS server.

The account used to connect to the Software Update Point is found under **Administration** > **Servers and Site System Roles** > (Your Software Update Point) > **Properties** > **Proxy and Account Settings**

![](/_images/SUP_SYNC_0x80131500_or_0x8013150A-8-1.jpg)

## Resolution

The **minimum permissions** required for this account is to be in **WSUS Administrators** on the Software Update Point Site Server (in this example, we added SRVC\_SCCM to the WSUS Administrators on SP1.corp.contoso.com).

![](/_images/SUP_SYNC_0x80131500_or_0x8013150A-9.jpg)

If you were to uncheck the box for "**Use Credentials to connect to the WSUS Server**" the computer name of your primary site server would need to be added to the **WSUS Administrators** group on the Software Update Point.

This scenario may have occured as a result of the Software Update Point (SP1.corp.contoso.com) being set up when the service account (SRVC\_SCCM) was a Domain Admin or had higher privileges. The account was then removed from Domain Admins, and the SUP began to fail.

Once the appropriate permissions were added. The Software Update Point synced correctly.

![](/_images/SUP_SYNC_0x80131500_or_0x8013150A-10.jpg)

> **Note:** Minimum permissions for all ConfigMgr accounts can be found here: [https://learn.microsoft.com/en-us/mem/configmgr/core/plan-design/hierarchy/accounts](https://learn.microsoft.com/en-us/mem/configmgr/core/plan-design/hierarchy/accounts)
