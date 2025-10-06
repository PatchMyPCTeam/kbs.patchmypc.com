---
title: >-
  Power BI Reports for Microsoft Intune Third-Party Update and Application
  Deployments
date: 2020-08-31T00:00:00.000Z
taxonomy:
  products:
    - advanced-insights
  tech-stack:
    - null
  solution:
    - null
  post_tag:
    - null
  sub-solutions:
    - reporting-and-insights
    - education
    - best-practices
---

# Power BI Reports Microsoft Intune

Our Power BI dashboards can be used to monitor the compliance and deployment of [**third-party updates**](../../third-party-patch-management-for-microsoft-intune/) and [**applications**](../../automatically-create-and-deploy-applications-in-microsoft-intune/) in Microsoft Intune.

* [**Step 1: Download the Latest Power BI Template File**](power-bi-reports-microsoft-intune.md#topic1)
* [**Step 2: Save the Latest Assignment Statistics via the Publisher**](power-bi-reports-microsoft-intune.md#topic2)
* [**Step 3: Open the Power BI Report and Import the CSV File**](power-bi-reports-microsoft-intune.md#topic3)
* [**Step 4: Explore the Power BI Report**](power-bi-reports-microsoft-intune.md#topic4)

### Step 1: Download the Latest Power BI Template File

The first step is to download the latest Power BI template file for our third-party update and application dashboard

* [**Patch My PC Intune Dashboard v1.0.2**](https://patchmypc.com/app/uploads/2025/06/PatchMyPC-Intune-Dashboard-v1.0.2.zip)

### Step 2: Save the Latest Assignment Statistics via the Publisher

The next step is to **export** the latest assignment statics to a **.CSV** file in bulk using our Publisher.

Perform the following actions: [**Open the Intune Application Manager Utility**](https://patchmypc.com/intune-application-manager-utility) > **Export**

![](/_images/IntunePowerBiReportVersion1-0-2.png)

### Step 3: Open the Power BI Report and Import the CSV File

**Open the Power BI file**, and navigate to the **CSV file** that contains the latest export for assignment statistics from Microsoft Intune third-party updates and applications.

After importing, you should see a dashboard for both **third-party software updates** and **Win32 application assignments** in Microsoft Intune.

![Intune Dashboard for Third-Party Updates](/_images/Intune-Dashboard-for-Third-Party-Updates-2023.png "Intune Dashboard for Third-Party Updates")

### Step 4: Explore the Power BI Report

There are two pages the will show you compliance statistics for [**third-party update**](../../third-party-patch-management-for-microsoft-intune/) or [**third-party application**](../../automatically-create-and-deploy-applications-in-microsoft-intune/) assignments

![Power BI Tabs for Intune Patching of Third-Party Updates](/_images/Power-BI-Tabs-for-Intune-Patching-of-Third-Party-Updates.png "Power BI Tabs for Intune Patching of Third-Party Updates")

#### Intune Patching Overview Tab

The Intune Patching Overview tab will display **compliance statistics** for any third-party update assignments within your Microsoft Intune tenant.\
Some of the charts can be **shown as a data point table** to review specific compliance data. For example, if you right-click the **Update Timeline**, you can drill into updates deployed on a specific day.

![Drill Into Specific Updates Power BI](/_images/Drill-Into-Specific-Updates-Power-BI.png "Drill Into Specific Updates Power BI")

Here's an example of this **data point**:

![Drilled In Report for Software Update](/_images/Drilled-In-Report-for-Software-Update.png "Drilled In Report for Software Update")

#### Intune Application Overview Tab

The Intune Application Overview tab will display **compliance statistics** for any third-party applications (Win32 applications for initial deployment) within your Microsoft Intune tenant.\
![](/_images/Intune-Application-Overview.png)

#### Dashboard components explained

**Intune Patching Overview**

* **Applications Patching =**  the total number of **Intune Updates** published using PMPC.
* **Publishers Patching** = Out of the total number of Application Patching, it informs you how many unique vendors the updates come from.
* **Assigned Patches =** The number of published **PMPC Intune Updates** with an active Intune assignment.
* **Un-assigned patches** = The number of **PMPC Intune Updates** with no Intune assignment.
* **Patches Installed** = The total number of PMPC Intune Updates installed on Intune devices.
* **Patches** **Pending** = The total number of PMPC Intune Updates with an active assignment, but the installation is still pending.
* **Applications Deployed** = The total number of **Intune Apps** published using PMPC.
* **Publishers deployed** = Out of the total number of Applications deployed, it informs you how many unique vendors they come from.
* **Assigned applications** = Out of the total number of Applications deployed, it informs you how many have an active Intune assignment.
* **Un-assigned Applications** = Out of the total number of Applications deployed, it informs you how many don't have any Intune assignments.
* **Applications Installed** = Out of the total number of Applications deployed with Active Assignments, it informs you how many are installed on Intune devices.
* **Applications pending** = Out of the total number of Applications deployed with Active Assignments, it informs you on how many devices the  installation is pending.