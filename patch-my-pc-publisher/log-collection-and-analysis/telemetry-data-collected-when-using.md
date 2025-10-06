---
title: Telemetry Data Collected when Using the Publisher
date: 2020-07-19T00:00:00.000Z
taxonomy:
  products:
    - patch-my-pc-publisher
  tech-stack:
    - null
  solution:
    - null
  post_tag:
    - null
  sub-solutions:
    - log-collection-and-analysis
    - education
    - best-practices
---

# Telemetry Data Collected When Using

Patch My PC will collect **basic telemetry data** when our [**Publisher software**](https://patchmypc.com/publishing-service-setup-documentation) is used to **improve our products and services**.

This article will describe in detail the specific data collected by Patch My PC and, more importantly, the **reason we collect it**.

* [The Number of Times the Catalog is Downloaded](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#topic1)
* [The Version Number of the Publisher Software](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#topic2)
* [The List of Selected Products](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#productselections)
* [.NET Version Number where the Publisher Software is installed](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#topic12)
* [Are Preview and Self-Updates Enabled](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#topic3)
* [Is ConfigMgr Application Creation Enabled](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#topic4)
* [Is Intune Application Creation Enabled](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#topic5)
* [The Error Description if an Update or Application Publishing Fails](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#topic9)
* [The Configuration Manager Site Version](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#topic6)
* [The WSUS Build Number](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#topic7)
* [The WSUS Database Type](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#WsusDbType)
* [WSUS Updates Cleanup Information](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#wsus-cleanup)
* [WSUS Maintenance Configuration](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#WsusMaintenance)
* [The Number of Products Enabled and Published](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#topic8)
* [Heartbeat](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#heartbeat)
* [Certificate Validation Errors](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#cert-errors)
* [Feature Usage](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#feature-usage)
* [Cloud Attached](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#cloud-attached)
* [Alert Configuration](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#alerts)
* [ConfigMgr Application Metadata](https://patchmypc.com/kb/telemetry-data-collected-when-using/#configmgr-application-metadata)
* [How Data is Transferred](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#topic10)
* [Data collected from our Installer(s)](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#topic17)
* [Disclaimer and More Information](https://patchmypc.com/telemetry-data-collected-when-using-the-publisher#topic11)

### The Number of Times the Catalog is Downloaded

Each time the catalog is download via an **HTTPS request**, we will increment the number of times it was downloaded. The data type for this is an **integer**. We will also report the next scheduled sync time as a **DateTime.**

> **Reason:** The download count and next sync time are collected because we believe this gives critical insights to understand if a specific customer has been able to **utilize our products and services effectively**.

### The Version Number of the Publisher Software

The **currently installed version** of the Publisher is reported as a **string** value.

> **Reason:** The **installed version** is helpful to understand how our **self-updates are progressing** and if any notifications may need to be sent based on notices for customers on a **specific-build**.

### The List of Selected Products

The list of **selected products** in the Publisher is reported as a list of product identifiers and product types.

> **Reason:** The list of **selected products** is essential to help us understand the usage of our catalog, and ensure we are able to reach out to customers regarding specific products.

### .NET Version Number where the Publisher Software is installed

The **version** of .NET where the Publisher Software is installed.

> **Reason:** The **version** of .NET where the Publisher is installed is important to help us understand if future releases of the Publisher, and any code changes, will be supported.

### Are Preview and Self-Updates Updates Enabled

The **preview update status** within the Publisher will be reported using a **boolean** type. The **self-update** status will be reported as a **boolean**.

> **Reason:** We collect this data to understand what customers have enabled **preview updates enabled** or **self-updates disabled**. This state may be helpful to notify customers of any specific actions that may need to be taken.

### Is ConfigMgr Application Creation Enabled

The Publisher will report if the checkbox to **enable the creation of ConfigMgr apps** is enabled as a **boolean**.

> **Reason:** This data is helpful to understand if a customer is making use of **all the features they may be entitled to** within their subscription.

### Is Intune Application Creation Enabled

The Publisher will report if the checkbox to **enable the creation of Intune apps** is enabled as a **boolean**.

> **Reason:** This data is helpful to understand if a customer is making use of **all the features they may be entitled to** within their subscription.

### The Error Description if an Update or Application Publishing Fails

The Publisher will report the specific error returned if an update or application fails to create. This applies to the **Updates**, **ConfigMgr Apps**, **Intune Apps**, and **Intune Updates tab.** The error description is a **string** value.

Error description reporting can be disabled with the following registry value:

*
  * REG ADD “HKLM\SOFTWARE\Patch My PC Publishing Service” /v DisableErrorReporting /t REG\_DWORD /d 1 /f

> **Reason:** This data helps us see any issues a customer is having when **applications** or **updates fail to create**. Our goal is to be able to proactively reach out to customers who are experiencing known publishing issues.

### The Configuration Manager Site Version

The Publisher will report back the **Configuration Manager site version** if the SMSProvider is defined as a **string** value.

> **Reason:** The Configuration Manager **site version** is important to know what versions of ConfigMgr are installed for compatibility and supportability. If we are aware of any **compatibility issues** with a specific Configuration Manager build, this data will allow us to contact affected customers.&#x20;

### The WSUS Build Number

The Publisher will report back the **WSUS build number** as a **string** value.

> **Reason:** This data helps us identify **WSUS API’s** we need to test, support, or depreciate.

### The WSUS Database Type

The Publisher will report back the **type of WSUS Database**, whether WID or SQL.

> **Reason:** This data helps us better understand how our customers use WSUS which can impact how our software works.

### WSUS Updates Cleanup Information

The Publisher will perform some WSUS cleanup during a sync. The **number of updates, size of the updates,** and **folder count** cleaned up are reported back as **integer** values.

> **Reason:** This data helps us better understand hard disk usage for the Publisher.

### WSUS Maintenance Configuration

The Publisher will report back the current WSUS Maintenance configuration from the respective tab in Configuration Manager, which includes the following:

*
  * Decline expired updates in WSUS According to supersedence rules
*
  * Add non-clustered indexes to the WSUS database
*
  * Remove obsolete updates from the WSUS database

> **Reason:** This data helps us better understand how our customers use WSUS which can impact how our software works.

### The Number of Products Enabled and Published

The Publisher will report the specific number of products enabled within each of the following tabs: **Updates**, **ConfigMgr Apps**, **Intune Apps**, and **Intune Updates** as an **integer**. The publisher will also report the number of updates, applications, and CVE’s published as an integer.

> **Reason:** This data helps us see the **percentage** of enabled vs. available products and how many products are being published.

### Heartbeat

The Publisher sends a heartbeat every 4 hours. This simply is used as an indication that the Patch My PC service is running.

> **Reason:** This data helps us determine if an instance of the Publisher is active. The data is used in troubleshooting failed upgrades and understanding Publisher usage.

### Certificate Validation Errors

The Publisher performs **certificate validation** when communicating with Patch My PC servers. If an error occurs, the **content of the error** is reported as a **string** value.

> **Reason:** This data helps us ensure the security and availability of the connection between the Publisher and Patch My PC servers.&#x20;

### Feature Usage

The enablement of features, such as the [Pause Feature](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#pause-product-updates), Proxy enablement other features added to the Publisher are reported back as **boolean** values.

> **Reason:** This data helps us determine the **usage of new features** within the Publisher and **helps us contact customers** in the event a bug may affect them.&#x20;

### Cloud Attached

The **status of the** [**cloud connection**](https://docs.patchmypc.com/patch-my-pc-cloud/custom-apps/integrating-publisher) is reported back as a **boolean** value indicating if there is configured connection between the Publisher and Patch My PC Cloud.

> **Reason:** This data **helps us validate the connection** between the Publisher and Patch My PC Cloud to **ensure the availability** of cloud-enabled features.&#x20;

### Alert Configuration

The **enablement** of alerts, such as [SMTP](https://patchmypc.com/how-publishing-alerts-work#topic1) alerts, or [Teams alerts](https://patchmypc.com/how-publishing-alerts-work#topic3) within the Publisher is reported back as a **boolean** value.

> **Reason:** This data helps us determine **how customers can be notified** of changes, actions or errors within the Publisher.&#x20;

### ConfigMgr Application Metadata

The list of **ConfigMgr Applications** and related **metadata** is reported back as a JSON payload. This includes information such as the installation command line, uninstallation command line, detection method, file names, file hashes, display info, and other application-related information. The collection of this information is dependent on the Publisher being cloud-connected and the Migration feature being enabled.

> **Reason:** This data is used to allow for the ConfigMgr to Patch My PC Cloud Migration feature. The data collected is analyzed to determine which applications are eligible for migration.&#x20;

### How Data is Transferred

The data listed above would be **transported** and **encrypted** using **TLS 1.2**.

### Data collected from our Installer(s)

We collect “Installer Analytics” which is part of Advanced Installer – [Installer Analytics (advancedinstaller.com)](https://www.advancedinstaller.com/analytics/)

Advanced Installer is what we use to create the installers for both Advanced Insights and the Patch My PC Publisher. The data collected is:-

*
  * Was the application installed or uninstalled?
*
  * The version of the app that is installed
*
  * Were there any exceptions during the installation?
*
  * The OS that the app was installed on
*
  * The version of .NET, IIS, Java runtimes
*
  * Is the device x64 or x86?
*
  * The geographic location of the installation (e.g. US, UK, Germany)
*
  * Were the prerequisites met?

### Disclaimer and More Information

The list above is current through **August 20, 2024**. We will try to keep this list updated, but it may not contain every instance of data collection. Please contact [**technical support**](https://patchmypc.com/technical-support) with any questions or you can review our [**Terms of Service**](https://patchmypc.com/terms-of-service).
