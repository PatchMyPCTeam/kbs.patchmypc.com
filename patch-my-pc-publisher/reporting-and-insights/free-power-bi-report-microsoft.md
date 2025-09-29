---
title: "Free Power BI Report for Microsoft and Third-Party Updates in Configuration Manager"
date: 2021-07-24
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

The Patch My PC Power BI dashboard can monitor the compliance and deployment of **third-party and Microsoft security updates through Configuration Manager**.

- [Video Guide for Power Bi Dashboard](#video-guide)

- [Step 1: Requirements](#topic1)

- [Step 2: Download and Configure the Report](#topic2)

- [Step 3: Exploring the Power BI Report](#topic3)

- [Step 4: Updating your Data](#topic4)

## Video Guide for Power Bi Dashboard

Optionally, you can also check out the video guide below to learn more about the setup and usage of the Power BI Software Update Dashboard for ConfigMgr.

<iframe title="YouTube video player" src="https://www.youtube.com/embed/OAKwkZLrClY" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen" data-cookieconsent="ignore"></iframe>

## Step 1: Requirements

To utilize this report, you will need to meet the following requirements.

**Required**

- Have Power BI Desktop (Free) Installed

- Have the DB Reader Role on the ConfigMgr database
    - Our report does not leverage the RBAC views, so Configuration Manager roles will not provide access.

- A copy of the Power BI template, included in this article.

**Optional**

- Power BI Pro License

- Power BI Gateway server installed and configured

## Step 2: Download and Configure the Report

To get started, download the latest Power BI template file for software updates.

**[Download the Dashboard: PatchMyPC-ConfigMgr-1.0.9](https://patchmypc.com/app/uploads/2025/06/PatchMyPC-ConfigMgr-1.0.9.zip)**

**Updated 12/6/2021**

| **Version Number** | **Notes** |
| --- | --- |
| 1.0.9 |   - Fixed Collection Filter - Fixed Internal Data Model Map - Updated Company Logo   |
| 1.0.6 |   - Initial Public Version   |

We present the file in a zipped format, so you will need to **extract the template file** once downloaded.

Once you've downloaded the file, go ahead and open it to get started. Opening the file will present you with three fields to fill in.

1. Â MEMCM SQL Server Name

3. MEMCM SQL DB Name

5. Collection Filter

![Power BI Template Details](images/Snag_1b93d6aa.png)

Below is an example of what you might put in each field.

![Filled in Template](images/Snag_1b9668fd.png)

> **Note:** The Percent sign here is used as a wild card to return all machines, related to all collections. You can use this to only return machines from specific collections as needed.

When you first run this report, you will need to **authorize the SQL queries to run**, assuming that you trust the content of those queries.

You can find the contents of these queries publicly available, along with an explanation behind their logic, on our **[learning website](https://learn.patchmypc.com/patch-my-pc-extras/patch-my-pc-power-bi-queries)**.

![Allow Native Database Query](images/Snag_385497.png)

The security setting under the options governs the default behavior of Power BI and, we do not recommend unchecking the pictured option.

![Power BI Native Database Queries](images/Snag_3222a7.png)

Once you have approved all of the queries, the report will load, keep in mind it **may take some time based on the size of your environment**.

## Step 3: Exploring the Power BI Report

The Patch My PC PowerBI dashboard is broken up into three different components. You can navigate through these components by using the tabs located at the bottom of the report.

### Compliance Overview

![Compliance Overview](images/Snag_464707.png)

1. Filters - This section can be used to filter the report, only to show information about patches released over X date range, patches in a specific software update group, or to only grade based on deployed updates.

3. Workstation and server Patch compliance, with a total ring that sets a target of 90%

5. Configuration Manager Agent Compliance. This helps find Gaps in your environment for devices that might be missing the agent, therefore not reporting their associated risk.

### Workstation Compliance

This report tab is filtered specifically to workstation-only information.

![Workstation Compliance](images/Snag_4e4e63.png)

1. Filters allow you to filter down to the criteria you care about.

3. Currently selected patches you are grading against.

5. Running tally of the device's name and the number of patches missing vs. Installed.

7. Detailed information on what that patch is and its current state.

Please note this report was written with the idea of being actionable, and as a result, 'Unknown' or 'fuzzy' data is largely excluded.

### Server Compliance

This report tab is filtered specifically to server-only information.

![Server Compliance](images/Snag_a6be57.png)

1. Filters allow you to filter down to the criteria you care about.

3. Currently selected patches you are grading against.

5. Running tally of the device's name and the number of patches missing vs. Installed.

7. Detailed information on what that patch is and its current state.

## Step 4: Updating your Data

This dataset can be updated in one of two ways. First, you can at any time click '**refresh**' from the ribbon in the Power BI desktop app. This report has also been tested with a Power BI gateway and does support **automatic refresh** if you have a gateway properly set up and configured.
