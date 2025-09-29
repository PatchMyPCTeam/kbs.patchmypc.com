---
title: "Site Database and Collection Options for Application Scanning"
date: 2020-05-08
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

This article will describe how to quickly find the correct **Configuration Manager database server name** and **database name**.

### Set the Configuration Manager Database Server and Database Name

To find the database server and database name, navigate to **Administration** > **Monitoring** > **System Status** > **Site Status** in your configuration manager console.

![](/_images/ConfigMgrDBScan_FindInfo.png)

Here you will find the Site database server site system that contains the details required.

### Are You Using a Custom SQL Server Instance for the Site Database?

Although less common, if you are using a custom SQL instance, use **%Server%%Instancename%** in the Server Name input box.

![](/_images/ConfigMgrDBScan_CustomInstance.png)

## Limit Scanning to a Specific Collection

By default, the device collection is not set. When empty, the scan for support products is performed against **All Systems**.

**Optionally**, you can choose a device collection by clicking the browse icon.

When a device collection is enabled, the results for detected applications will be **filtered to devices in the specified collection**.

![](/_images/ConfigMgrDBScan_LimitCollectionId.png)