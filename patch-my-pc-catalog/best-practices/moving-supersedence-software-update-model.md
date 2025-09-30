---
title: "Moving to Supersedence Software Update Model"
date: 2018-12-29
taxonomy:
    products:
        - patch-my-pc-catalog
    tech-stack:
        - 
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - best-practices
        - updates
---

Starting **December 28, 2018**, we are moving to a **supersedence model** for software updates in our third-party software update catalog. Before this change, we would mark previous versions of software updates as expired.

## Why are We Making this Change?

With the **Third-Party Software Update Catalogs feature in [Configuration Manager 1806+](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates)**, all third-party updates in a catalog are published. When we expired previous updates in our catalog, those previous versions were immediately marked expired when a new update is released. When an update is marked as expired, it can't be deployed in Configuration Manager. The immediate expiring of updates could cause issues where customers don't have enough time to fully deploy updates through all testing cycles for products where new updates are released frequently.

## What's the Supersedence Model Look Like?

By moving to a supersedence model, **you will have complete control over how long you want to keep previous versions available** for deployment that have been superseded.

You can control whether you want superseded updates to be immediately expired or delay expiring superseded updates for a certain number of months using the **[Supersedence Rules](https://docs.microsoft.com/en-us/sccm/sum/plan-design/plan-for-software-updates#BKMK_SupersedenceRules)** option on your software update point. In the example below, configuration manager won't expire superseded updates until they have been superseded for 1 month.

![Supersedence Rules SCCM](images/Supersedence-Rules-SCCM.png)

In today's catalog update in our lab, we can see the previous versions of applications are superseded and not expired. This change will allow the superseded application updates to continue being deployed for an additional month in our configuration.

![Superseded Updates After Publishing New Updates](images/Superseded-Updates-After-Publishing-New-Updates.png)

If you are not expiring superseded immediately and using automatic deployment rules, you will want to ensure that you **exclude superseded updates** in your criteria to ensure your ADR's only deploy the latest updates.

![ADR Exclude Superseded Third-Party Updates](images/ADR-Exclude-Superseded-Third-Party-Updates.png)
