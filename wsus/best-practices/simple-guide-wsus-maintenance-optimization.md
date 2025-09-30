---
title: "The Simple Guide to WSUS Maintenance and Optimization in ConfigMgr"
date: 2021-10-18
taxonomy:
    products:
        - wsus
    tech-stack:
        - 
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - best-practices
        - troubleshooting
        - updates
---

In this article, we are going to break down the **key factors involved in ensuring your WSUS server and software update point are healthy and optimized** for performance. There are many excellent guides out there around WSUS maintenance, including **[Microsoft Docs](https://docs.microsoft.com/en-us/troubleshoot/mem/configmgr/wsus-maintenance-guide)**.

## Relationship Between WSUS and the ConfigMgr Software Update Point (SUP)

Before I dig into why WSUS maintenance is essential, let's get a **basic overview of how Configuration Manager [interacts with WSUS](https://docs.microsoft.com/en-us/mem/configmgr/sum/plan-design/plan-for-software-updates#BKMK_SUPInfrastructure)**.

Even if you have never opened the WSUS console, when a software update point is installed, ConfigMgr configures the top-level **WSUS server to synchronize automatically from the [Microsoft Update Catalog](https://docs.microsoft.com/en-us/windows-server/administration/windows-server-update-services/deploy/2-configure-wsus#211-connection-from-the-wsus-server-to-the-internet)**. If you have multiple software update points, any child-SUP will sync from the top-level SUP.

![WSUS Sync From the Windows Update Catalog From the Internet](/_images/WSUS-Sync-From-the-Windows-Update-Catalog-From-the-Internet-Sync.png "WSUS Sync From the Windows Update Catalog From the Internet")

When a software update point synchronization runs, ConfigMgr tells WSUS to go sync from the **[Microsoft Update Catalog](https://docs.microsoft.com/en-us/windows-server/administration/windows-server-update-services/deploy/2-configure-wsus#211-connection-from-the-wsus-server-to-the-internet)**.

![Synchronize Software Updates](/_images/Synchronize-Software-Updates.png "Synchronize Software Updates")

If you review the ConfigMgr log **[wsyncmgr.log](https://docs.microsoft.com/en-us/mem/configmgr/core/plan-design/hierarchy/log-files#BKMK_SUPLog)**, you can better see this happening in real-time. Here's an example of some **key lines in the wsyncmgr.log** when synchronization is running.

![wsyncmgr sync from WSUS to ConfigMgr SUP](/_images/wsyncmgr-sync-from-WSUS-to-ConfigMgr-SUP.png "wsyncmgr sync from WSUS to ConfigMgr SUP")

> **Note:** In the past, the Microsoft Update Catalog was relatively small, and WSUS would often work great without any type of optimization. It used to be pretty common to hear in the ConfigMgr space that you shouldn't ever touch WSUS.

## Why Performing WSUS Maintenance is Now Essential

As the [Microsoft Update Catalog grew](https://www.youtube.com/watch?v=WisuGPCM9j8&t=36s), the WSUS database did as well. As I recall, the issue of WSUS maintenance started to become a topic around 2017 when customers started to **see timeouts due to an unmaintained WSUS environment** with a large number of software updates.

The timeout error would generally show in the **wsyncmgr.log as the following error**:

Sync failed: The operation has timed out. Source: Microsoft.UpdateServices.Internal.DatabaseAccess.ApiRemotingCompressionProxy.GetWebResponse  
Sync failed. Will retry in 60 minutes

At first glance, you may think this is a networking or proxy error since the logline contains the text "**ApiRemotingCompressionProxy**", but it's generally not related to the proxy at all.

It's an API call to the **WSUS Administration** IIS website timing out when running a **SQL query to get all published updates**.

![Microsoft.UpdateServices.Internal.DatabaseAccess.ApiRemotingCompressionProxy.GetWebResponse](/_images/Microsoft-UpdateServices-Internal-DatabaseAccess-ApiRemotingCompressionProxy-GetWebResponse.png "Microsoft.UpdateServices.Internal.DatabaseAccess.ApiRemotingCompressionProxy.GetWebResponse")

This issue generally happens because SQL maintenance such as **re-indexing, [custom index creation](https://docs.microsoft.com/en-us/troubleshoot/mem/configmgr/wsus-maintenance-guide#create-custom-indexes),** and other **WSUS maintenance steps** have never been performed.

## Step 1: Check Current WSUS Performance

First, you should get a baseline of how your WSUS database performs before enabling any WSUS maintenance. The stored procedure below will retrieve all updates in WSUS. The **spSearchUpdates** stored produce is called during each software update point synchronization in ConfigMgr.

We would hope to see this query executed in **under 30 seconds in a healthy environment**. If the query takes **longer than 150 seconds**, you may have sync failures and other WSUS performance issues.

Perform the following actions in **SQL Management Studio** > **Connect to the SQL or WID Instance** > **Right-Click the SUSDB** > **New Query > Type: exec spSearchUpdates > Execute or F5**

![exec spSearchUpdates in SQL Management Studio for WSUS Performance Check](/_images/RemoteDesktopManagerFree_232HaqhWGO.gif "exec spSearchUpdates in SQL Management Studio for WSUS Performance Check")

## Step 2: Enable the Built-In WSUS Maintenance in ConfigMgr 1906 and Newer

The easiest way to ensure WSUS performs well and is optimized is to enable the [built-in cleanup and maintenance process](https://docs.microsoft.com/en-us/troubleshoot/mem/configmgr/wsus-maintenance-guide#maintain-wsus-while-supporting-configuration-manager-current-branch-version-1906-and-later-versions). To enable this, navigate to > **Administration** > **Sites** > **Right-Click the Site** > **Configure Site Components** > **Software Update Point**

![Software Update Point in Configure Site Components ConfigMgr](/_images/Software-Update-Point-in-Configure-Site-Components-ConfigMgr.png "Software Update Point in Configure Site Components ConfigMgr")

In the WSUS Maintenance tab, check the following checkboxes:

- **Decline expired updates in WSUS according to supersedence rules**

- **Add non-clustered indexes to the WSUS database**

- **Remove obsolete updates from the WSUS database**

![Decline expired updates in WSUS according to supersedence rules and Add non-clustered indexes to the WSUS database and Add non-clustered indexes to the WSUS database](/_images/Decline-expired-updates-in-WSUS-according-to-supersedence-rules-and-Add-non-clustered-indexes-to-the-WSUS-database-and-Add-non-clustered-indexes-to-the-WSUS-database.png "Decline expired updates in WSUS according to supersedence rules and Add non-clustered indexes to the WSUS database and Add non-clustered indexes to the WSUS database")

Once enabled, the **wsyncmgr** component will **perform a WSUS cleanup after successfully synchronizing** the software update point.

You can see the actions taking place in the **[wsyncmgr.log](https://docs.microsoft.com/en-us/mem/configmgr/core/plan-design/hierarchy/log-files#BKMK_SUPLog)** 

![wsyncmgr cleanup in wsyncmgr.log](/_images/wsyncmgr-cleanup-in-wsyncmgr-log_.png "wsyncmgr cleanup in wsyncmgr.log")

> **Important:** Before the next software update point sync is performed, you should ensure the **WsusPool in IIS is optimized** using the steps below **[Optimizing WSUS AppPool in IIS for Better Performance](#topic4)** to improve WSUS IIS performance.

## Step 3: Validate Non-Clustered Indexes Were Created Successfully from Step 2

Based on our research and information in [Microsoft Docs](https://docs.microsoft.com/en-us/troubleshoot/mem/configmgr/wsus-maintenance-guide#create-custom-indexes), the single most significant factor to WSUS database performance and the root cause for timeouts is not having the custom [non-clustered indexes](https://docs.microsoft.com/en-us/troubleshoot/mem/configmgr/wsus-maintenance-guide#create-custom-indexes) on the WSUS database (SUSDB).

If you successfully performed a software update point sync after **[Step 2: Enable the Built-In WSUS Maintenance in ConfigMgr](#topic3)**, the non-clustered indexes should be automatically created. However, there are a few scenarios where the automatic creation may fail:

1. The synchronization of the software update point is failing, which causes the step to [add non-clustered indexes](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/software-updates-maintenance#sql-server-permissions-for-creating-indexes) never to attempt.

3. WSUS/SUP uses a remote Windows Internal Database (WID), so [ConfigMgr can't automatically create the custom indexes](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/software-updates-maintenance#sql-server-permissions-for-creating-indexes).

5. If the WSUS database is on a remote SQL Server using a non-default port, indexes [might not be added](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/software-updates-maintenance#sql-server-permissions-for-creating-indexes).

To verify the non-clustered indexes were created automatically, copy the following **[SQL Query](https://github.com/PatchMyPCTeam/VSMUG/blob/master/2021-12/Top%2010%20Tips%20for%20Optimizing%20WSUS/SQL%20Scripts/CheckIfIndexExists.sql)** and ensure the result of the query in SQL Management Studio returns **nclLocalizedPropertyID index exists,** and **nclSupercededUpdateID index exists**

![Validate non-clustered indexes exist on SUSDB](/_images/RemoteDesktopManagerFree_jPhbBSFWsp.gif "Validate non-clustered indexes exist on SUSDB")

If the custom indexes **weren't created automatically**, you should use the following step to **create the custom indexes manually**: **[Step 7: Create Non-Clustered Indexes on SUSDB Manually (As Needed)](https://patchmypc.com/the-simple-guide-to-wsus-maintenance-and-optimization-in-configmgr#topic5)**

## Step 4: Optimizing WSUS AppPool in IIS for Better Performance

One WSUS optimization that is **not automatically enabled** is changing the WSUS Administration websites AppPool settings in IIS. Per **[Microsoft's WSUS best practice](https://docs.microsoft.com/en-us/troubleshoot/mem/configmgr/windows-server-update-services-best-practices#disable-recycling-and-configure-memory-limits)**, you should perform the following changes.

Open **Internet Information Services (IIS) Manager** > **Application Pools** > **Right Click WsusPool** > **Advanced Settings...**

![WsusPool Advanced Settings...](/_images/WsusPool-Advanced-Settings.png "WsusPool Advanced Settings...")

In the **Advanced Settings pane**, perform the following changes:

- Queue Length 2000 (up from default of 1000)

- Idle Time-out (minutes) 0 (down from the default of 20)

- Ping Enabled False (from default of True)

- Private Memory Limit (KB) 0 (unlimited, up from the default of 1,843,200 KB)

- Regular Time Interval (minutes) 0 (to prevent a recycle, and modified from the default of 1740)

![Disable recycling and configure memory limits](/_images/WSUS-App-Pool-Best-Practice-Settings.png "Disable recycling and configure memory limits")

Click **OK** to save the settings then **right-click the WsusPool** and click **Recycle...**

![Recycle WsusPool in IIS](/_images/Recycle-WsusPool-in-IIS.png "Recycle WsusPool in IIS")

## Step 5: Check for Un-Needed Products that are Enabled

We also highly recommend reviewing the **products you have enabled** to see if there are **any products that are no longer needed**. For example, it's not uncommon for us to review this with customers to find unsupported products such as the following enabled:

- Windows XP

- Windows 7

- Windows Server 2003

- etc.

To check the products enabled, navigate to > **Administration** > **Sites** > **Right-Click the Site** > **Configure Site Components** > **Software Update Point**

![Software Update Point in Configure Site Components ConfigMgr](/_images/Software-Update-Point-in-Configure-Site-Components-ConfigMgr.png "Software Update Point in Configure Site Components ConfigMgr")

**Uncheck** any **unneeded products** and click **OK**

![Uncheck Unneeded Products ConfigMgr SUP](/_images/Uncheck-Unneeded-Products-ConfigMgr-SUP.png "Uncheck Unneeded Products ConfigMgr SUP")

## Step 6: Reindex the WSUS Database (Recommended)

While still **connected to the SUSDB in SQL Server Management Studio** open a **New Query for the SUSDB**

![New Query SUSDB in SQL Management Studio](/_images/New-Query-SUSDB-in-SQL-Management-Studio.png "New Query SUSDB in SQL Management Studio")

Copy and paste the **T-SQL script** from the Microsoft Docs article **[Reindex the WSUS database](https://docs.microsoft.com/en-us/troubleshoot/mem/configmgr/reindex-the-wsus-database)** and click **Execute**.

![Reindex the WSUS database (SUSDB)](/_images/Reindex-the-WSUS-database-SUSDB.png "Reindex the WSUS database (SUSDB)")

> **Note:** The [Reindex the WSUS database T-SQL script](https://docs.microsoft.com/en-us/troubleshoot/mem/configmgr/reindex-the-wsus-database) can take some time to complete depending on how fragmented the WSUS database indexes are. We also recommend you set up a SQL maintenance plan to re-index the database on a schedule as described in the following video: [Top 10 Tips for Optimizing WSUS Performance and Reliability](https://youtu.be/WisuGPCM9j8?t=1488) 

## Step 7: Create Non-Clustered Indexes on SUSDB Manually (As Needed)

There is a scenario where the [Built-In WSUS Maintenance in ConfigMgr](#topic3) may not work if you are already in a very unhealthy WSUS state. If you recall in the step above, the WSUS cleanup and maintenance is performed by the **wsyncmgr component** and only **after a successful software update point sync**.

If you are already in an unhealthy WSUS state, you may constantly be receiving timeout errors as shown below in the **wsyncmgr.log**:

Sync failed: The operation has timed out. Source: Microsoft.UpdateServices.Internal.DatabaseAccess.ApiRemotingCompressionProxy.GetWebResponse  
Sync failed. Will retry in 60 minutes

When this happens, the **WSUS clean up task will never trigger** because it only occurs after a successful software update point sync.

If you happen to be in this unhealthy WSUS state, you will need to **[manually perform some of the tasks](https://docs.microsoft.com/en-us/troubleshoot/mem/configmgr/wsus-maintenance-guide#perform-wsus-maintenance)** generally automated by [Built-In WSUS Maintenance in ConfigMgr](#topic3). After manually performing WSUS optimization and you are able to complete a successful sync the built-in maintenance can take over.

We found performing the following two actions will generally be enough to get WSUS in a state to successfully sync.

**[Create custom indexes](https://docs.microsoft.com/en-us/troubleshoot/mem/configmgr/wsus-maintenance-guide#create-custom-indexes)**

### Create Custom Indexes Manually

To create **custom indexes manually**, perform the following steps.

Open **Microsoft SQL Server Management Studio** > **Connect to the SQL Server Running the WSUS Database (SUSDB) > Right-Click SUSDB > New Query**

![New Query SUSDB in SQL Management Studio](/_images/New-Query-SUSDB-in-SQL-Management-Studio.png "New Query SUSDB in SQL Management Studio")

Paste in the **[SQL Query from Microsoft Docs](https://docs.microsoft.com/en-us/troubleshoot/mem/configmgr/wsus-maintenance-guide#create-custom-indexes)** to create the WSUS custom indexes and click **Execute**.

![Create SQL Indexes for SUSDB for WSUS Performance](/_images/Create-SQL-Indexes-for-SUSDB-for-WSUS-Performance.png "Create SQL Indexes for SUSDB for WSUS Performance")

![  Automate 3rd Party Patching   Discover how PMPC can resolve your patching headaches  ](/_images/interactive-156207349366.png "  Automate 3rd Party Patching   Discover how PMPC can resolve your patching headaches  ")

## Video Guide: Top 10 Tips for Optimizing WSUS Performance and Reliability

If you prefer learning by video, you can check out the deep dive video we did on WSUS performance and reliability below

<iframe title="YouTube video player" src="https://www.youtube.com/embed/WisuGPCM9j8" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen" data-cookieconsent="ignore"></iframe>