# Using Patch My PC Publisher with Azure Update Manager

yaml global product = Advanced Insights

yaml global solution = Reporting and Analytics

yaml global tech Stack = ConfigMgr

In this article, we discuss the high-level steps for implementation and key considerations when using Azure Update Manager (AUM) to manage third-party updates.

While Patch My PC does not integrate directly with AUM, this knowledge base article aims to assist customers already using Azure Arc and AUM in incorporating third-party patching into their existing update management workflows.

### What is Azure Update Manager (AUM) <a href="#h-what-is-azure-update-manager-aum" id="h-what-is-azure-update-manager-aum"></a>

As customers consider the new technologies and management platforms on offer from Microsoft, many will adopt Microsoft Intune to manage their user devices. This can ultimately lead them to reconsider the need for Configuration Manager for their remaining server estate. This “mindset shift to cloud” also raises questions about how to handle server patching. As customers might already be using Azure in some capacity, AUM automatically becomes very topical in these conversations as “something we could adopt”.

One common misconception is that moving to AUM eliminates the need for WSUS entirely. While this may be true for customers focusing solely on managing Microsoft first-party updates, it is important to understand that AUM provides no direct support for third-party patching outside of WSUS. Customers requiring third-party patching must still maintain their own WSUS instance, whether on-premises or hosted in Azure.

Most third-party software vendors provide updates in the form of catalogs, specifically designed to integrate with WSUS. Currently, WSUS is the only Microsoft-supported mechanism for publishing third-party catalogs and distributing those updates to endpoints. Without WSUS, AUM lacks the capability to manage third-party updates entirely, making WSUS an indispensable component for customers with hybrid or multi-cloud environments who require comprehensive patching capabilities.

The screenshot below shows first-party updates identified as required during a scan of Microsoft Update, and third-party updates identified as required during a scan of a local WSUS instance.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/AUM_Configuration2-1.jpg" alt=""><figcaption></figcaption></figure>

### Scenarios <a href="#h-scenarios" id="h-scenarios"></a>

While AUM is an excellent tool for managing Microsoft first-party updates, customers requiring third-party patching must still incorporate WSUS into their update strategy. Below are two common scenarios where AUM and WSUS work together.

#### 1. Using AUM with WSUS for All Updates <a href="#h-1-using-aum-with-wsus-for-all-updates" id="h-1-using-aum-with-wsus-for-all-updates"></a>

While AUM provides powerful orchestration and compliance capabilities, it does not eliminate the need for WSUS. AUM has no native support for third-party updates outside of WSUS, making WSUS a critical component for any organization requiring comprehensive patch management. Customers should plan accordingly to maintain their WSUS infrastructure and understand its role in supporting both first-party and third-party updates in hybrid or multi-cloud environments.

In this scneario, WSUS is deployed either on-premises or in Azure and serves as the source for both Microsoft first-party and third-party updates. AUM leverages Azure Arc to manage update compliance and patch orchestration for on-premises servers, while Patch My PC Publisher remains responsible for publishing third-party updates to the WSUS instance.

For customers transitioning from Configuration Manager, it is important to recognize that WSUS updates now require approval, either manually or with automatic approvals, ensuring that both Microsoft and third-party updates are available for deployment.

**How does it work?**

1. **Patch My PC Publisher** publishes third-party updates to WSUS.
2. **WSUS** Synchronizes first-party updates from Microsoft Update and serves as the distribution point for third-party updates, hosting the necessary metadata and content.
3. Updates are **approved** for devices (either manually or using automatic approval rules in WSUS).
4. **AUM** connects to Azure Arc-enabled servers and orchestrates the installation of updates, while WSUS ensures both Microsoft and third-party updates and content are accessible.

**Who is it for?**

Organizations that want a single WSUS instance to handle all update management and maintain full control over patch approval and distribution.

This option **must** also be used for customers wishing to install third-party updates on servers with an operating system **older than Windows Server 2022**. This is because Scan Source is not supported on older operating systems, meaning the devices cannot scan both Microsoft Update and WSUS simultaneously.

#### 2. Using AUM with Windows Update for First-Party Updates and WSUS for Third-Party Updates <a href="#h-2-using-aum-with-windows-update-for-first-party-updates-and-wsus-for-third-party-updates" id="h-2-using-aum-with-windows-update-for-first-party-updates-and-wsus-for-third-party-updates"></a>

In this scenario, Patch My PC Publisher manages the publishing of third-party updates to a WSUS instance, while first-party updates are obtained directly from Microsoft Update using the “scan source” configuration. AUM continues to orchestrate the assessment and deployment of updates, leveraging Azure Arc to manage on-premises servers. This approach significantly minimizes reliance on WSUS, as it is configured solely for third-party patching rather than handling the entire breadth of Microsoft first-party updates.

By avoiding the need to configure WSUS for first-party updates, you drastically reduce the number of updates managed within the WSUS database. This optimization addresses some of WSUS’s most notorious challenges, including bloated metadata, increased maintenance overhead, and performance degradation that can occur when managing thousands of updates. In this streamlined setup, WSUS is reserved exclusively for third-party updates, allowing it to operate more efficiently while still enabling AUM to handle patch compliance and orchestration for both first-party and third-party updates.

**How does it work?**

1. **Patch My PC Publisher**Â publishes third-party updates to WSUS.
2. **WSUS**Â serves as the distribution point for third-party updates, hosting the necessary metadata and content.
3. Updates are **approved** for devices (either manually or using automatic approval rules in WSUS).
4. **Scan Source** configuration allows endpoints to:
   * Retrieve Microsoft first-party updates directly from Microsoft Update.
   * Retrieve third-party updates from WSUS which were published by Patch My PC Publisher.
5. **AUM**Â connects to Azure Arc-enabled servers to orchestrate update compliance and installation for both first-party and third-party updates.

**Who is it for?**

Customers transitioning to a cloud-first model but still requiring robust third-party patching capabilities. This approach is ideal for those looking to reduce on-premises content storage requirements by shifting first-party update handling to Microsoft Update.

This option can **only** be used for customers wishing to install third-party updates on servers with an operating system version of **Windows Server 2022 or higher**. This is because Scan Source is not supported on older operating systems, meaning the device cannot scan both Microsoft Update and WSUS simultaneously.

> **Note:** By downloading first-party updates directly from Microsoft Update, organizations can significantly reduce the storage requirements for WSUS, as first-party update content no longer needs to be stored locally. However, since each server downloads its first-party updates directly from the internet, organizations must plan for potential bandwidth impacts. Unlike Configuration Manager, which can efficiently distribute content via on-premises distribution points, this approach may lead to higher network usage for large deployments.

### OS Requirements <a href="#h-os-requirements" id="h-os-requirements"></a>

The following Windows operating systems are supported for use with AUM:-

* Windows Server 2012 R2 and higher (including Server Core) **\***

Windows 10 and 11 clients are not supported by AUM. If customers are seeking a cloud only solution for client patch management, Microsoft Intune is recommended to manage and orchestrate update workloads using the Win32 app model.

> _**\* IMPORTANT:** While AUM supports Server 2012 R2 and higher, customers intending to adopt the scan source approach described in_ [_Scenario 2_](https://patchmypc.com/kb/using-patch-my-pc-publisher/#scenario2) _must use Server 2022 or higher for the servers they wish to manage. This is because 2022 and later versions of Windows Server support the Windows Update client to scan both WSUS and Windows Update simultaneously._

### Policy Requirements <a href="#h-policy-requirements" id="h-policy-requirements"></a>

The following policy settings should be reviewed to ensure devices are able to assess and install third-party updates orchestrated by AUM.

#### 1. Configure the WSUS Server Location (Required) <a href="#h-1-configure-the-wsus-server-location-required" id="h-1-configure-the-wsus-server-location-required"></a>

The client will need pointing to the WSUS instance for compliance reporting and to know where to get update copntent from. If you are moving from ConfigMgr for patching, these settings may already be configured in the local policy.

**Using the Registry Editor**

Set the **WUServer** registry value to your **http(s)** **WSUS instance (REG\_SZ)**

`WUServer = https://MyWSUSServer.contoso.com:8531`\
`WUStatusServer = https://MyWSUSServer.contoso.com:8531`

`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate`

**Note:** The **WUStatusServer** is not strictly required but it will be set if you use GPO or Local Policy Editor to confiure the WUServer location.

**Using the ADMX Template**

Computer Configuration > Administrative Templates > Windows Components > Windows > Windows Update\
**Specify intranet Microsoft update service location**

Set the intranet update service for detecting updates = https://MyWSUSServer.contoso.com:8531\
Set the intranet statistics server = https://MyWSUSServer.contoso.com:8531

<figure><img src="https://patchmypc.com/app/uploads/2025/04/admx_1.jpg" alt=""><figcaption></figcaption></figure>

#### 2. Configure the UseWUServer Policy (Required) <a href="#h-2-configure-the-usewuserver-policy-required" id="h-2-configure-the-usewuserver-policy-required"></a>

**Using the Registry Editor**

The **UseWUServer** policy setting specifies whether the device should get its updates from a WSUS server or directly from Microsoft Update.

Set the **UseWUServer**  registry value to **1 (DWORD)**

`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU`\
`UseWUServer = 1` — –

**Using the ADMX Template**

When you configure **specify the intranet Microsoft update service location** using the ADMX tempalte, this policy is automatically configured and set to 1.

#### 3. Configure an Automatic Approval Rule in WSUS (Required) <a href="#h-3-configure-an-automatic-approval-rule-in-wsus-required" id="h-3-configure-an-automatic-approval-rule-in-wsus-required"></a>

Configure an automatic approval rule in WSUS to ensure that third-party updates, are downloaded and available for deployment by AUM.

This is essential because AUM relies on WSUS to host the necessary update metadata and content for the devices it orchestrates patching for.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/AUM_Approval.jpg" alt=""><figcaption></figcaption></figure>

#### 4. Accept Trusted Publisher Certifcates (Required) <a href="#h-4-accept-trusted-publisher-certifcates-required" id="h-4-accept-trusted-publisher-certifcates-required"></a>

The **AcceptTrustedPublisherCerts** policy allows you to specify whether the Windows update client accepts and processes updates signed by entities other than Microsoft, like Patch My PC, when the update is found on an intranet Microsoft update service location.

**Using the Registry Editor**

Set the **AcceptTrustedPublisherCerts** registry value to **1 (DWORD)**

`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate`\
`AcceptTrustedPublisherCerts = 1`

**Using the ADMX Template**

Computer Configuration > Administrative Templates > Windows Components > Windows > Windows Update\
**Allow signed updates from an intranet Microsoft update service location** = Enabled

<figure><img src="https://patchmypc.com/app/uploads/2025/04/admx_2.jpg" alt=""><figcaption></figcaption></figure>

> **Note:** By downloading first-party updates directly from Microsoft Update, organizations can significantly reduce the storage requirements for WSUS, as first-party update content no longer needs to be stored locally. However, since each server downloads its first-party updates directly from the internet, organizations must plan for potential bandwidth impacts. Unlike Configuration Manager, which can efficiently distribute content via on-premises distribution points, this approach may lead to higher network usage for large deployments.

#### 5. Disable Automatic Updates (Optional) <a href="#h-5-disable-automatic-updates-optional" id="h-5-disable-automatic-updates-optional"></a>

To ensure that the Windows Update agent does not take independent actions, such as downloading or installing updates outside of AUM’s control, configure the system to disable automatic updates. This setting ensures AUM retains full control over the patching process.

**Using the Registry Editor**

Set the **NoAutoUpdate** registry value to 1 **(DWORD)**

`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU`\
`NoAutoUpdate = 1`

**Using the ADMX Template**

Computer Configuration > Administrative Templates > Windows Components > Windows > Windows Update\
**Configure Automatic Updates** = Disabled

<figure><img src="https://patchmypc.com/app/uploads/2025/04/admx_3.jpg" alt=""><figcaption></figcaption></figure>

### Scan Source Requirements (for Scenario 2) <a href="#h-scan-source-requirements-for-scenario-2" id="h-scan-source-requirements-for-scenario-2"></a>

If your intent is to use WSUS **only** for third-party updates as detailed in [Scenario 2](https://patchmypc.com/kb/using-patch-my-pc-publisher/#scenario2), you will need to configure Scan Source policy to instruct the Windows Update agent to scan both WSUS and the Windows Update service.

**Note:** Utilizing Scan Source to use Windows Update for Business and WSUS together is very well documented at:- [https://learn.microsoft.com/en-us/windows/deployment/update/wufb-wsus](https://learn.microsoft.com/en-us/windows/deployment/update/wufb-wsus)

The following Scan Source settings are documented well at:-\
[https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-update](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-update)

Scan Source controls which update source the Windows Update agent will scan and honor for Feature, Driver, Quality and Other Updates (Note: Other does not mean third-party in this context). If your intent is for all Windows first-party updates to come from Windows Update, and all third-party updates to come from WSUS, configure all Scan Source policies to zero.

**Using the Registry Editor**

Set the following registry values to **0 (DWORD)**

`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate`

`SetPolicyDrivenUpdateSourceForFeatureUpdates = 0`\
`SetPolicyDrivenUpdateSourceForDriverUpdates = 0`\
`SetPolicyDrivenUpdateSourceForOtherUpdates = 0`\
`SetPolicyDrivenUpdateSourceForQualityUpdates = 0`

Additionally, set the UseUpdateClassPolicySource registry key to complete scan source configuration. Set the **UseUpdateClassPolicySource** registry value to **1 (DWORD)**

`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU`\
`UseUpdateClassPolicySource = 1`

**Using the ADMX Template**

Computer Configuration > Administrative Templates > Windows Components > Windows > Windows Update\
**Specify source service for specific classes of Windows Updates** = Windows Update

<figure><img src="https://patchmypc.com/app/uploads/2025/04/admx_4.jpg" alt=""><figcaption></figcaption></figure>

**Note:**\
There is no scan source class specifically for third-party updates. “Other Updates” does not mean third-party updates.\
\
**Scan source Key to Value:**\
Windows Update = 0\
WSUS = 1

> **Important:** Scan Source only works for Windows Server 2022 + even though AUM is supported on Windows Server 2012R2 +

### Install on Demand <a href="#h-install-on-demand" id="h-install-on-demand"></a>

Currently, AUM’s “install on demand” functionality, specifically for third-party updates from the Patch My PC catalog, has certain limitations. AUM relies on update IDs that follow a specific format traditionally used by Microsoft’s updates, which differs from the `PMPC-year-month-day` format used in the Patch My PC catalog. This incompatibility can prevent AUM from targeting specific third-party updates for on-demand installation, as the update IDs do not align with the expectations of AUM’s mechanisms.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/AUM_Configuration2.jpg" alt=""><figcaption></figcaption></figure>

Patch My PC is exploring whether it is feasible to adopt a more compatible KB ID format for updates in our catalog to improve targeting capabilities. However, this effort is still in the exploratory phase, and no confirmed timeline is available for implementation. Customers should take this into account when planning third-party update orchestration with AUM, especially if they require precise on-demand installation functionality.

### Maintenance Configuration <a href="#h-maintenance-configuration" id="h-maintenance-configuration"></a>

A commonly overlooked configuration when using AUM to orchestrate third-party updates is ensuring that the “Updates” classification is included in the maintenance configuration.

Ensure the **Updates** classification is included for **Windows** operating systems in your maintenance configuration.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/AUM_Configuration.jpg" alt=""><figcaption></figcaption></figure>

### Useful Logs <a href="#h-useful-logs" id="h-useful-logs"></a>

The following logs are useful to understand the assessment and orchestration of updates usin AUM.

* **Windows Update Logs:**
  * This log provides details about update scans, downloads, and installations. It also allows you to check which update source the device is scanning against.
  * Location: Use `Get-WindowsUpdateLog` to generate this log file.
* **Event Viewer Logs:**
  * **Path:** `Applications and Services Logs > Microsoft > Windows > WindowsUpdateClient`
  * This event log is useful for tracking update installation progress and errors
* **Windows Update Extension Log:**
  * **Path:** `C:\ProgramData\GuestConfig\extension_logsMicrosoft.SoftwareUpdateManagement.WindowsOsUpdateExtensionWindowsUpdateExtension.log`
  * This log is specifically used for troubleshooting **update orchestration and management** by the **Azure Update Manager** (AUM) or other Azure-based update mechanisms

### Troubleshooting <a href="#h-troubleshooting" id="h-troubleshooting"></a>

AUM plays a crucial role in the assessment and orchestration of Windows Updates, primarily focusing on determining update applicability and managing the scheduling of patch deployments. However, it relies heavily on the underlying Windows Update Agent to execute the actual update scan, download, and installation processes. As such, most troubleshooting steps for AUM overlap with traditional troubleshooting approaches for WSUS and Windows Update configurations.

Microsoft has a detailed troubleshooting article for AUM at [https://learn.microsoft.com/en-us/azure/update-manager/troubleshoot?tabs=azure-machines](https://learn.microsoft.com/en-us/azure/update-manager/troubleshoot?tabs=azure-machines)

**Azure Arc Problems:**

Ensure the Azure Arc agent is functioning correctly and reporting health status as expected. Verify that the agent is installed, connected, and synchronized with Azure. Additionally, check the Azure Update Manager for any configuration issues, errors, or failed update tasks that may indicate problems with update orchestration or deployment

**Policy Conflicts:**

Ensure there are no conflicting group policies or local policies affecting the update scan or deployment. For example, verify that settings like Specify Intranet Microsoft Update Service Location and Configure Automatic Updates are correctly configured. Setting AUOptions may interfer with AUM orchestration as outlined at [https://learn.microsoft.com/en-us/azure/update-manager/configure-wu-agent#pre-download-updates](https://learn.microsoft.com/en-us/azure/update-manager/configure-wu-agent#pre-download-updates)

> Pre-download of updates isn’t supported in Azure Update Manager. Don’t use pre-download functionality through AUOptions while using Azure Update Manager default/advanced patching mechanisms which sets NoAutoUpdate=1

**Certificate Issues:**

For third-party updates, confirm that the code-signing certificate is correctly deployed to the target devices.\
Ensure the certificate is placed in the Trusted Publishers store, and if it’s self-signed, also in the Trusted Root Certification Authorities store.

See this article for more information on deploying the code-signing certificate used for third-party updates [https://patchmypc.com/how-to-deploy-the-wsus-signing-certificate-for-third-party-software-updates](https://patchmypc.com/how-to-deploy-the-wsus-signing-certificate-for-third-party-software-updates)&#x20;

**Network and Connectivity Problems:**

Ensure the server can reach the configured Windows Update source (e.g., WSUS or Microsoft Update). Check for firewalls, proxies, or network rules blocking access to required update endpoints.

**Scan Source Configuration:**

Below is an export from a server in the lab that was configured for Scan Source. This configuration allowed the server to “dual scan” both WSUS and Microsoft Update.

```
"WUServer"="https://MyWSUSServer.contoso.com:8531"
"WUStatusServer"="https://MyWSUSServer.contoso.com:8531"
"UpdateServiceUrlAlternate"=""
"SetProxyBehaviorForUpdateDetection"=dword:00000000
"AcceptTrustedPublisherCerts"=dword:00000001
"SetPolicyDrivenUpdateSourceForFeatureUpdates"=dword:00000000
"SetPolicyDrivenUpdateSourceForQualityUpdates"=dword:00000000
"SetPolicyDrivenUpdateSourceForDriverUpdates"=dword:00000000
"SetPolicyDrivenUpdateSourceForOtherUpdates"=dword:00000000


"NoAutoUpdate"=dword:00000001
"UseWUServer"=dword:00000001
"UseUpdateClassPolicySource"=dword:00000001
```
