---
title: "How to Remove Already Published Third-Party Software Updates"
date: 2020-08-19
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

In some cases, you may want to remove third-party updates that have already been published to WSUS from Patch My PC. To perform this task, you will need to use the [modify published updates wizard](https://patchmypc.com/modify-published-third-party-updates-wizard) in the [Publisher](/publishing-service-setup-documentation).

## Unchecking a Product doesn't Remove Previously Published Updates

If you uncheck a product in the **Updates** tab, it will cause no future updates to be published for that specific product, but it will **not cause existing updates** that have been already published to be removed.

![Unchecked Third-Party Update Patch My PC](/_images/UnChecking-Product-PatchMyPC-Updates.gif "Unchecked Third-Party Update Patch My PC")

> **Note:**  If you want to remove already published updates for the product, you will need to **[decline the existing updates](#topic2)**.

## Show and Decline Only Updates for Not Enabled Products

The **All Enabled Status** dropdown allows you to select **Not Enabled** to show any updates published from the **Patch My PC's catalog** that are not currently enabled in the **Updates** tab.

![](/_images/Not-enabled-products.png)

> **Note**: The **Not Enabled** dropdown option can be a great option to **decline** or **delete** old updates for products not currently enabled.

## Decline the Third-Party Updates Based on Filters

To remove existing updates already published, you will need to **decline** the updates.

Within the Publisher, go to the "**Updates**" tab, click "**Options**", then click "**Run Wizard**" in the **Modify Published Updates** section.

Select the updates you would like to remove from WSUS/SCCM and click the "**Decline Selected Updates**".

![Filter and Remove Third-Party Update](/_images/filter-and-decline-update.png "Filter and Remove Third-Party Update")

After the updates are declined in the Publisher, you can manually **sync your software update point** in SCCM for the changes to occur immediately.

![sync sccm software update point for declined updates](/_images/sync-sccm-software-update-point-for-declined-updates.png "sync sccm software update point for declined updates")

After the software update point sync is complete, you should see the **declined updates show as expired** in SCCM and will no longer be deployed. In the example below, you can see the declined 7-Zip updates show expired.

![updates marked as expired in sccm](/_images/updates-marked-as-expired-in-sccm.png "updates marked as expired in sccm")

> **Note:** Expired updates will be automatically purged from the Configuration Manager after **7 days**.

## Removing/Filtering Specific Updates from Automatic Deployment Rules

In some scenarios, you may simply want to remove **specific updates** from an automatic deployment rule. You can use the **title filter** to exclude the specific update title from the ADR search criteria.

Here's an example of an ADR that will deploy **all Patch My PC updates** with no filters:

![SCCM ADR with No Filters](/_images/no-adr-title-filter.png "SCCM ADR with No Filters")

Here's how you can add a **title filter** if you wanted to exclude **any 7-Zip update** from your ADR. **Title: -7-Zip**

![ADR Exclude Title Filter SCCM](/_images/ADR-Exclude-Title-Filter-SCCM.png "ADR Exclude Title Filter SCCM")

A full list of update title names for all product can be found at [Excluding Specific Products from ADRs in Microsoft SCCM with Patch My PC](/filtering-specific-third-party-product-from-adrs-in-microsoft-sccm-patch-my-pc-update-catalog#titlenames)