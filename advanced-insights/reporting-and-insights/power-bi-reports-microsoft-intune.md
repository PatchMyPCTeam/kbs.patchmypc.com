# Power BI Reports for Microsoft Intune Third-Party Update and Application Deployments

<pre class="language-yaml"><code class="lang-yaml"><strong>---
</strong>
taxonomy:
<strong>    products:
</strong>        - advanced-insights
    tech-stack:
        - intune
    post_tag:
        - education
        - best-practices
    sub-solutions:
        - test
        
---
</code></pre>

Our Power BI dashboards can be used to monitor the compliance and deployment of [**third-party updates**](https://patchmypc.com/third-party-patch-management-for-microsoft-intune) and [**applications**](https://patchmypc.com/automatically-create-and-deploy-applications-in-microsoft-intune) in Microsoft Intune.

* [**Step 1: Download the Latest Power BI Template File**](https://patchmypc.com/kb/power-bi-reports-microsoft-intune/#topic1)
* [**Step 2: Save the Latest Assignment Statistics via the Publisher**](https://patchmypc.com/kb/power-bi-reports-microsoft-intune/#topic2)
* [**Step 3: Open the Power BI Report and Import the CSV File**](https://patchmypc.com/kb/power-bi-reports-microsoft-intune/#topic3)
* [**Step 4: Explore the Power BI Report**](https://patchmypc.com/kb/power-bi-reports-microsoft-intune/#topic4)

### Step 1: Download the Latest Power BI Template File <a href="#h-step-1-download-the-latest-power-bi-template-file" id="h-step-1-download-the-latest-power-bi-template-file"></a>

The first step is to download the latest Power BI template file for our third-party update and application dashboard

* [**Patch My PC Intune Dashboard v1.0.2**](https://patchmypc.com/app/uploads/2025/06/PatchMyPC-Intune-Dashboard-v1.0.2.zip)

### Step 2: Save the Latest Assignment Statistics via the Publisher <a href="#h-step-2-save-the-latest-assignment-statistics-via-the-publisher" id="h-step-2-save-the-latest-assignment-statistics-via-the-publisher"></a>

The next step is to **export** the latest assignment statics to a **.CSV** file in bulk using our Publisher.

Perform the following actions: [Open the Intune Application Manager Utility](https://patchmypc.com/intune-application-manager-utility) > **Export**

![](/_images/IntunePowerBiReportVersion1-0-2.png)

### Step 3: Open the Power BI Report and Import the CSV File <a href="#h-step-3-open-the-power-bi-report-and-import-the-csv-file" id="h-step-3-open-the-power-bi-report-and-import-the-csv-file"></a>

**Open the Power BI file**, and navigate to the **CSV file** that contains the latest export for assignment statistics from Microsoft Intune third-party updates and applications.

After importing, you should see a dashboard for both **third-party software updates** and **Win32 application assignments** in Microsoft Intune.

![Intune Dashboard for Third-Party Updates](/_images/Intune-Dashboard-for-Third-Party-Updates-2023.png "Intune Dashboard for Third-Party Updates")

### Step 4: Explore the Power BI Report <a href="#h-step-4-explore-the-power-bi-report" id="h-step-4-explore-the-power-bi-report"></a>

There are two pages the will show you compliance statistics for [**third-party update**](https://patchmypc.com/third-party-patch-management-for-microsoft-intune) or [**third-party application**](https://patchmypc.com/automatically-create-and-deploy-applications-in-microsoft-intune) assignments

![Power BI Tabs for Intune Patching of Third-Party Updates](/_images/Power-BI-Tabs-for-Intune-Patching-of-Third-Party-Updates.png "Power BI Tabs for Intune Patching of Third-Party Updates")

#### Intune Patching Overview Tab <a href="#h-intune-patching-overview-tab" id="h-intune-patching-overview-tab"></a>

The Intune Patching Overview tab will display **compliance statistics** for any third-party update assignments within your Microsoft Intune tenant.\
Some of the charts can be **shown as a data point table** to review specific compliance data. For example, if you right-click the **Update Timeline**, you can drill into updates deployed on a specific day.

![Drill Into Specific Updates Power BI](/_images/Drill-Into-Specific-Updates-Power-BI.png "Drill Into Specific Updates Power BI")

Here’s an example of this **data point**:

![Drilled In Report for Software Update](/_images/Drilled-In-Report-for-Software-Update.png "Drilled In Report for Software Update")

#### Intune Application Overview Tab <a href="#h-intune-application-overview-tab" id="h-intune-application-overview-tab"></a>

The Intune Application Overview tab will display **compliance statistics** for any third-party applications (Win32 applications for initial deployment) within your Microsoft Intune tenant.\
![](/_images/Intune-Application-Overview.png)

#### Dashboard components explained <a href="#h-dashboard-components-explained" id="h-dashboard-components-explained"></a>

**Intune Patching Overview**

* **Applications Patching =** the total number of **Intune Updates** published using PMPC.
* **Publishers Patching** = Out of the total number of Application Patching, it informs you how many unique vendors the updates come from.
* **Assigned Patches =** The number of published **PMPC Intune Updates** with an active Intune assignment.
* **Un-assigned patches** = The number of **PMPC Intune Updates** with no Intune assignment.
* **Patches Installed** = The total number of PMPC Intune Updates installed on Intune devices.
* **Patches Pending** = The total number of PMPC Intune Updates with an active assignment, but the installation is still pending.
* **Applications Deployed** = The total number of **Intune Apps** published using PMPC.
* **Publishers deployed** = Out of the total number of Applications deployed, it informs you how many unique vendors they come from.
* **Assigned applications** = Out of the total number of Applications deployed, it informs you how many have an active Intune assignment.
* **Un-assigned Applications** = Out of the total number of Applications deployed, it informs you how many don’t have any Intune assignments.
* **Applications Installed** = Out of the total number of Applications deployed with Active Assignments, it informs you how many are installed on Intune devices.
* **Applications pending** = Out of the total number of Applications deployed with Active Assignments, it informs you on how many devices the installation is pending.