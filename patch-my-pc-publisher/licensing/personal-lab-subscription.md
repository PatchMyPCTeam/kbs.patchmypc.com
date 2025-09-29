---
title: "Personal Lab Subscription"
date: 2021-06-07
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

We offer a Personal Lab subscription for those that want to use our **[enterprise Patch My PC Publisher](https://patchmypc.com/application-patch-management)** in their personal Configuration Manager or Intune lab environment.

## Overview of the Personal Lab Subscription

A personal lab subscription is intended for **ConfigMgr/Intune enthusiasts in a personal lab**.

The personal lab will only work for environments with fewer than 25 devices and is subject to approval.

You are **not permitted to use a personal lab subscription in a commercial or production environment.** The personal lab is subject to **additional requirements** defined in our terms of service.

> **Note:** If your **company has purchased** Patch My PC and uses it in a lab in your corporate environment, you can generally use your production subscription for this use. Please see the following FAQ: **[Can I Use the Same Catalog Subscription in Multiple Environments](https://patchmypc.com/frequently-asked-questions#multiple-environments)**.

## Requirements and Limitations

The **maximum count of 25 devices** is evaluated by the Patch My PC Publisher at the beginning of each sync. It checks your device count in WSUS, ConfigMgr, and Intune. It is acceptable to have up to 25 devices on each platform. The **functionality to publish third-party applications or updates will be disabled** if you exceed the 25 device count in any of the respective platforms.

### How is the device count determined in WSUS?

Total device count.

![Personal Lab Subscription device count exceeded in WSUS console](/_images/HomeLabExceededCount-WSUS.png "Personal Lab Subscription device count exceeded in WSUS console")

Exceeding the device count in WSUS will cause the Updates tab to stop functioning, and products from this tab will no longer publish to WSUS.

If this is the only platform you have to exceed in the device count, other tabs and platforms will continue to function and publish.

### How is the device count determined in Configuration Manager?

All Desktop and Servers Clients device collection.

![Personal Lab Subscription device count exceeded in SCCM console](/_images/HomeLabExceededCount-SCCM.png "Personal Lab Subscription device count exceeded in SCCM console")

### How is the device count determined in Intune?

Windows devices that checked in in the last 7 days

## Purchase a Personal Lab Subscription

To purchase the Personal Lab Subscription, submit a quote request here: **[Personal Lab Quote Request](personal-lab-quote)**.

> **Note:** If your **company has purchased** Patch My PC and uses it in a lab in your corporate environment, you can generally use your production subscription for this use. Please see the following FAQ: **[Can I Use the Same Catalog Subscription in Multiple Environments](https://patchmypc.com/frequently-asked-questions#multiple-environments)**.

## Installation for Personal Lab Subscription

For guidance on how to install the Publisher in your environment, please refer to our [documentation](https://docs.patchmypc.com).

> **Note:** Technical support is included in our personal lab subscriptions, but we don't offer our **[free setup calls](https://patchmypc.com/schedule-setup-call)** for **personal lab subscriptions**.

## What Happens if the Device Count is Exceeded

Exceeding the device count in **one platform will not cause other tabs to stop working or publishing to the other platforms**. For example, exceeding the device count in WSUS still enables you to publish with the ConfigMgr Apps, Intune Apps, and Intune Updates tabs as long as the device count isn't exceeded in the other areas.

If you exceed the number of allowed devices in any platform, you will receive an error similar to the below in [PatchMyPC.log](/collecting-log-files-for-patch-my-pc-support#publishing-service-app-logs-intune):

![Personal Lab Subscription device count exceeded log entry in PatchMyPC.log](/_images/HomeLabExceededCount2.png "Personal Lab Subscription device count exceeded log entry in PatchMyPC.log")

You will be **unable to publish new applications or updates** if the device count is exceeded.