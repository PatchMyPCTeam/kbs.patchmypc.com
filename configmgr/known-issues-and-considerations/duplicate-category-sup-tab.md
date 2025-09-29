---
title: "Duplicate Patch My PC Category Listed in Software Update Point Products Tab"
date: 2020-11-05
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

Recently, we have seen an increase in customers having an issue where **duplicate Patch My PC categories** will appear in the software update point products tab.

## Determine if You are Affected

The most common scenario that leads to us checking for duplicate Patch My PC categories is customers opening a support case mentioning **newly published updates aren't appearing** in the **All Software Updates** node in Configuration Manager. If you are affected by this issue, you should see **multiple Patch My PC categories** listed in the Software Update Point properties.

In the ConfigMgr console, navigate to **Administration** > **Site Configuration** > **Sites** > **Configure Site Components** > **Software Update Point**. In the **Products** tab, check if there are multiple Patch My PC categories as shown below:

![Duplicate Patch My PC Category in WSUS and SUP](/_images/Duplicate-Patch-My-PC-Category-in-WSUS-and-SUP.png "Duplicate Patch My PC Category in WSUS and SUP")

## Root Cause of Duplicate Product Categories

The duplicate categories are caused by a change in WSUS on Server 2019, where it uses **SHA2 instead of SHA1 to compute a unique category hash for a vendor categor**y.

Before Server 2019, WSUS would use SHA1 to compute a unique hash for a vendor category in the database. Unfortunately, this causes a **duplicate category** for any updates published after a Server 2019 in-place upgrade.

If you connect the **SUSDB** with SQL Server Management Studio and run:

exec spGetTopLevelCategories

You can see the **duplicate categories** created

![Duplicate Company spGetTopLevelCategories](/_images/Duplicate-Company-spGetTopLevelCategories.png "Duplicate Company spGetTopLevelCategories")

> **Important:** The duplicate category is not specific to Patch My PC updates. This WSUS bug **affects any published third-party category**. For example, in the example below, you can see a duplicate Dell category when Dell drivers were published after the OS upgrade
> 
> ![Duplicate Dell Company Category in ConfigMgr Products SUP](/_images/Duplicate-Dell-Company-Category-in-ConfigMgr-Products-SUP.png "Duplicate Dell Company Category in ConfigMgr Products SUP")

## Resolution for Duplicate Patch My PC Categories

The primary issue that causes customer problems is the **new company category in the Software Update Point Products tab has not been enabled**. By not having it enabled, any newly published updates after the OS upgrade **don't sync into ConfigMgr**.

The quickest and easiest fix is to simply **enable the second Patch My PC category** in the Software Update Point Products tab

![Enable Patch My PC Category in SUP Products](/_images/Enable-Patch-My-PC-Category-in-SUP-Products.gif "Enable Patch My PC Category in SUP Products")

Once the product is enabled and you completed a **[software update point sync](https://patchmypc.com/troubleshooting-why-third-party-updates-dont-show-in-sccm#topic2)** in ConfigMgr, the new updates should appear in the All Software Updates as normal.

> **Important:** If you use an Automatic Deployment Rules that use **Vendor** or **Product** in the Software Update criteria, you will need to modify the search criteria.

In the **Software Update** tab of your automatic deployment rule, you will need to enable both **Patch My PC Vendors**. Based on our experience, although it appears both Vendor categories may be enabled when clicking the Vendor that is not the case unless the ADR Vendor shows **"Patch My PC" OR "Patch My PC"** in the search criteria.

To enable both vendors, you can **uncheck** one of the Patch My PC vendors then **recheck** it again and the criteria should then list **both vendors**. This change will allow your ADR to search updates published before the WSUS OS upgrade and after.

![Adjust ADR for Both Vendors](/_images/Adjust-ADR-for-Both-Vendors.gif "Adjust ADR for Both Vendors")