---
title: "Scan Microsoft Intune for Supported Third-Party Products for Patching"
date: 2020-06-19
taxonomy:
    products:
        - patch-my-pc-publisher
    tech-stack:
        - 
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - general-configuration-and-usage
        - education
        - best-practices
---

# Scan Microsoft Intune for Supported Third-Party Products for Patching

The Patch My PC Publisher can **scan Microsoft Intune inventory** to detect products that are supported for Win32 application creation. You can auto-enable products detected.

This article discusses the **Intune scanning feature**. This article includes what features are available, existing limitations, and the planned features. The ability to scan Intune for products that are supported by Patch My PC is actively under development and will continue to change.

Topics covered in this article:

- [Current Features](#CurrentFeatures)
- [Limitations](#Limitations)

### Current Features

The intent with this feature was to provide some parity to our Configuration Manager scan feature shown [here](https://patchmypc.com/automatically-enroll-products-based-on-sccm-inventory-scans). The Configuration Manager database contains a monumental amount of data allowing us to provide a good representation of what our product can patch in your environment.

The Intune scanning UI can be seen below.

![Scan Intune Utility](/_images/ScanIntune1.png "Scan Intune Utility")

![](/_images/ScanIntune.png)

**Intune Connection:**

- This Intune Connection will share the configuration of the other Intune based features in the product. The configuration for API permissions can be found [here](https://patchmypc.com/intune-authentication-using-azure-app-registration).

[![More Info](/_images/more-info-icon.svg "More Info")](https://patchmypc.com/app/uploads/2025/05/more-info-icon.svg)**There are additional permissions required with this feature beyond what is required for creating applications. Please review the [article](https://patchmypc.com/intune-authentication-using-azure-app-registration) to ensure your permissions are configured correctly.**

**Auto Publishing Rules:**

- If this feature is enabled (checkbox in a checked stated) the Intune scanning will occur every time a **Publisher synchronization** occurs. Based on the results of the scan Win32 Intune Applications, or Intune Updates will be published to Intune if the count threshold is met.
    - The number specified will act as a threshold. When the scan is performed, any software found on **at least** that number of devices will be automatically selected to be published as an Intune application.
        - ie. The threshold set to 30, Zoom Meetings found on 55 devices, Zoom Meetings will be automatically selected for publishing
    - The dropdown "1 selected..." box lets you configure certain options for the published applications, such as disabling the self-updater if supported

[![More Info](/_images/more-info-icon.svg "More Info")](https://patchmypc.com/app/uploads/2025/05/more-info-icon.svg)If you have [Manage assignments](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#ManageAssignments) configured at the product or vendor level, and a product becomes enabled via auto-publishing rules, the newly enabled product will not inherit the assignments. This is by design to prevent customers experiencing an [Intune policy size limit](https://patchmypc.com/intune-policy-limit-considerations) issue.

**Filter:**

- The table of data can be filtered based on four fields
    - Product
    - Vendor
    - Count greater than...
    - Already enabled
        - The radio buttons to the right of the filters provide the ability to include, or exclude products which are already enabled as Win32 Intune Applications
- The filter is cosmetic only, it does not affect the scans or the Export to CSV feature

**Export to CSV**

- The data table can be exported out to a CSV file. This can be useful to pass to management or your cybersecurity team. Keep in mind the filters do not apply to the resulting CSV file.

**Click OK**

- - Intune Connection will be saved to your settings
    - Auto-publishing rules will be saved to your settings
        - Auto-publishing will occur during the next Publisher synchronization.
    - Changes in product selection in the data table will be saved to your settings.
        - Newly selected applications will be published during the next Publisher synchronization.

### Limitations:

Because this feature is using the [Microsoft Graph API](https://docs.microsoft.com/en-us/graph/api/resources/intune-graph-overview?view=graph-rest-1.0), we are subject to any limitations presented by the API. These limitations are discussed below.

- **Inventoried Applications:**
    - Intune currently keeps inventory of a subset of the applications installed on a device
    - Microsoft details this limitation in [this docs page](https://docs.microsoft.com/en-us/mem/intune/apps/app-discovered-apps#details-of-discovered-apps)
        - Windows 10 Win32 Apps: Only MSI based apps on company-owned devices
    - This limits our Graph API queries to returning MSI based products that we support, as opposed to the Configuration Manager scans which will detail all our supported products based on the database query results.
    - Counts may not directly reflect the exact count of instances of installed software. This is the result of the data structure in Graph, where each version of the software is stored as an individual record for a device. The result is some applications being counted multiple times for a device.
- **Graph API** 
    - Microsoft Graph API currently does not allow a query to return a full list of detected applications.
        - The [/devicemanagement/detectedApps](https://docs.microsoft.com/en-us/graph/api/intune-devices-detectedapp-list?view=graph-rest-1.0) endpoint does exist, but it only lists Modern apps
    - To list MSI based inventory data each device must be queried directly.
        - The [detectedApps](https://docs.microsoft.com/en-us/graph/api/resources/intune-devices-manageddevice?view=graph-rest-beta#relationships) relationship can be expanded to get a devices MSI based detected apps
    - Because each device must be queried individually there is a lot of queries to perform
        - Graph batches are used to increase performance
            - Graph batches are [limited to 20 queries per batch](https://docs.microsoft.com/en-us/graph/known-issues#limit-on-batch-size)
            - Graph batches [cannot be nested](https://docs.microsoft.com/en-us/graph/known-issues#no-nested-batch)
        - For the quickest Intune scanning, the Publisher will group devices into batches of 20 and perform the Graph API queries in parallel