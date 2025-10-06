---
title: Understanding the Machine Policy Retrieval &amp; Evaluation Cycle in SCCM
date: 2024-12-19T00:00:00.000Z
taxonomy:
  products:
    - null
  tech-stack:
    - configmgr
  solution:
    - null
  post_tag:
    - null
  sub-solutions:
    - log-collection-and-analysis
    - troubleshooting
    - known-issues-and-considerations
---

# Machine Policy Eval SCCM

The **Machine Policy Retrieval & Evaluation Cycle** is a critical process in SCCM that ensures clients receive and apply the latest policies from the management server. This article provides a breakdown of what the policy does, how to invoke it and which logs to use for troubleshooting.

### What is the Machine Policy Retrieval & Evaluation Cycle?

The Machine Policy Retrieval & Evaluation Cycle in ConfigMgr is a process through which the SCCM client devices check for new or updated policies and evaluate their applicability. This cycle determines what actions the client needs to perform based on the policies assigned by the ConfigMgr server. &#x20;

### [What does it do?](machine-policy-eval-sccm.md#WhatDoesItDo)

* The Machine Policy Retrieval & Evaluation Cycle is essential for keeping SCCM client settings up to date. It helps enforce configuration policies, software deployments, compliance settings, and other management tasks.&#x20;
* The cycle enables the client to periodically communicate with the SCCM server to download any new policies or updates, apply those policies, and determine any required actions.&#x20;
* This process ensures that the client remains aligned with IT policies and software distribution schedules set by administrators.&#x20;

### How the cycle works

The SCCM client initiates the Machine Policy Retrieval & Evaluation Cycle on a predefined schedule (default is every 60 minutes).

During each cycle, the client:

1. **Contacts the SCCM Management Point** The client requests updated policy information from the SCCM management point.
2. **Downloads Policies** If there are new or modified policies, the client downloads them. These policies may include settings, configurations, deployments, or compliance rules.
3. **Evaluates Policies** After downloading, the client evaluates the policies to see if there are any specific actions it needs to take. For example
   * Software Installations or Updates: Determine if new software or updates are required.
   * Configuration Baselines: Check if configuration items match the baseline defined by the organization.
   * Compliance and Security Settings: Apply new security configurations or compliance rules.
4. **Performs Actions as Needed** Based on the evaluation, the client takes actions such as installing software, enforcing configurations, or generating compliance reports.

### Ways to manually invoke the Machine Policy Retrieval & Evaluation Cycle

Although the cycle runs automatically, administrators can also manually trigger it on individual clients to speed up policy application. There are numerous ways this can be invoked manually.

#### 1. From the Client Side

One method is by using the Control Panel **Configuration Manager** Applet:

1. Open the **Control Panel** on the client device.
2. Go to **System and Security** **> Configuration Manager**.
3. In the Actions tab, select **Machine Policy Retrieval & Evaluation Cycle** and click **Run Now**.

![](/_images/MPREC_1.jpg)

#### 2. Using WMI

The other method of manually invoking the scan cycle from a client is by using PowerShell:

1. Open PowerShell as Administrator
2.  Run this command:

    > Invoke-WmiMethod -Namespace "rootCCM" -Class SMS\_Client -Name TriggerSchedule -ArgumentList "{00000000-0000-0000-0000-000000000021}"

#### &#x20;3. From the SCCM Console

1. Open the ConfigMgr console and navigate to **Assets and Compliance** > **Devices**.
2. Locate the device and right-click on it.
3. In the **Client Notification** section, choose **Download Computer Policy**.

![](/_images/MPREC_2.jpg)

#### 4. Using Advanced Insights

[Advanced Insights](https://patchmypc.com/advanced-insights/overview) is an innovative reporting and patch compliance solution for SCCM offered by Patch My PC.

To trigger the cycle from Advanced Insights, follow these steps:

* Access the Advanced Insights reporting website
* Navigate to **Resources > Devices > Managed** **devices** and locate your device using the Quick Search option.

From the **Actions** dropdown, select the **Download Computer Policy** option.

![](/_images/MPREC_3.jpg)

### Relevant logs to monitor on the client side

If you want to monitor the evaluation of this action, it can be done using these logs located on the client side in the %WinDir%\CCM\Logs folder:

* **PolicyAgent.log**:Records the actions of the client processing and applying policies received from the SCCM server. This includes policy requests, downloads, and enforcement activities related to software deployments, compliance settings, and other client configuration tasks.
* **ClientLocation.log**:Tracks the client's process of locating and selecting a management point or distribution point in the SCCM hierarchy. This log is useful for troubleshooting issues related to client site assignment and communication with management points.