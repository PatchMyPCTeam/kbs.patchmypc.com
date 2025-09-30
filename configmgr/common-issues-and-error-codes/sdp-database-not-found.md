---
title: "An error occurred while getting the SDP: The specified item could not be found in the database"
date: 2023-05-08
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
        - common-issues-and-error-codes
        - troubleshooting
---

This article will describe an error you may see in the **[PatchMyPC.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-app-logs-intune)** after migrating to a new WSUS server and not performing a full WSUS backup and restore to the new WSUS server **[per this guide](https://www.youtube.com/watch?v=bBcJY8_uHCQ&t=2433s)**.

## Determine if You are Affected

If you are affected, you will see the following error line in the **[PatchMyPC.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-app-logs-intune)**.

An error occurred while getting the SDP: The specified item could not be found in the database.

## Why Does this Logging Error Line Occur?

When migrating to a new Software Update Point (WSUS Server), companies will often add a new SUP that initially synchronizes with the original SUP.

Once the new software update point is synchronized, the original SUP is removed from the site, and **the new SUP is set as the primary SUP.**

![Removing Primary Software Update Point](/_images/Removing-Primary-Software-Update-Point.png "Removing Primary Software Update Point")

A small subset of metadata for third-party updates doesn't **synchronize during this process**, resulting in this error line in the **PatchMyPC.log** when we query already published updates.

> **Important:** The third-party updates will still scan and install successfully on client devices. This log line can be ignored.

## How to Resolve the Error by Declining and Republishing All Patch My PC Updates

Although this error can be safely ignored, there is a way to resolve the error if you prefer. This process can take multiple hours to decline and republish all updates.

Perform the steps below **on the new WSUS server using the Patch My PC Publisher**.

**Decline** all "**Patch My PC**" updates using the [Modify Updates Wizard](https://patchmypc.com/modify-published-third-party-updates-wizard#decline) as shown in the GIF below.

![Decline All Patch My PC Updates](/_images/Decline-All-PatchMyPC-Software-Updates.gif "Decline All Patch My PC Updates")

**Next, Republish** all Patch My PC updates at the **All Products** level. Choose **Yes** to both of the prompts.

Next, trigger **Publisher Service Sync** in the **Sync Schedule** tab.

![Republish All Patch My PC Updates](/_images/Republish-All-Software-Updates.gif "Republish All Patch My PC Updates")

Monitor the **[PatchMyPC.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-app-logs-intune)** for the sync to complete.

> **Note:** Since you are republishing all third-party updates, this process can take many hours to complete depending on the number of products you enabled.