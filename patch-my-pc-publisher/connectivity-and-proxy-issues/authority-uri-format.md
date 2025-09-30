---
title: "'authority' should be in uri format"
date: 2020-09-15
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
        - connectivity-and-proxy-issues
        - security
        - general-configuration-and-usage
---

The error **'authority' should be in uri format** can occur when you configured an invalid authority URL.

## Determine if You are Affected

If affected, you will see the following error in the **[PatchMyPC.log](/collecting-log-files-for-patch-my-pc-support#publishing-service-app-logs-intune)** file.

'authority' should be in Uri format Parameter name: authority

## Resolve Error: authority' should be in Uri format

This error happens when the URL in the [Intune tenant settings](https://patchmypc.com/intune-authentication-using-azure-app-registration#topic4) is invalid. We often see customers removed the **[https://login.windows.net/](https://login.windows.net)** prefix, as shown below:

!['authority' should be in Uri format](images/authority-should-be-in-Uri-format.png)

If you do not know your Intune tenant domain, navigate to the **[tenant status page](https://devicemanagement.microsoft.com/#blade/Microsoft_Intune_DeviceSettings/TenantAdminMenu/tenantStatus)** in your Intune tenant, and look at the property for **Tenant name**.

![tenant status page in intune tenant](images/tenant-status.png)

Now, it is time to go to the **Patch My PC Publisher** and input the **Authority**, **Application ID**, and **Application Secret** into the **Intune Options** window of the Publisher. It's important you leave the **https://login.windows.net/** prefix and only change the part

![inputting authoridy, application id, and secret into intune options in publisher](images/input-authority-ID-secret.png)

Replace with the **Tenant name** you found in your Intune tenant's **tenant status page**.  Paste the **Application ID** and **Application Secret** that was saved from earlier.  Click Test to validate that the **Publisher** can connect to your Intune tenant if you get a dialog box that says “Successfully connected to Intune”, congratulations!  You can now begin to publish applications to your Intune tenant.
