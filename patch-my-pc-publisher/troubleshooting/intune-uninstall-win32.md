---
title: "Intune Win32 app \"Allow Available Uninstall\""
date: 2024-05-20
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
        - troubleshooting
        - deployments
        - application-and-update-publishing
---

In this KB we will cover the expected behavior of the "Available Uninstall" feature for Win32 apps published by Patch My PC. You can configure this feature within the Patch My PC Publisher before publishing an Intune app, or manually from the Intune Admin Centre after publishing an app. We will explore both options in this guide.

## [What is "Available Uninstall"](#WhatIsAvailableUninstall)

When creating or configuring a Win32 app, there is a setting to provide the uninstall option for the app for users in the Company Portal.

![](/_images/Available-Uninstall.png)

When this setting is enabled, set to Yes, the Primary user of the device should see the option to uninstall the app. The Uninstall option will only appear if the app has already been evaluated as installed, using the detection method set on the Win32 app.

![](/_images/Available-Uninstall-2.png)

## [Configure "Available Uninstall" in the Patch My PC Publisher](#ConfigureAvailableUninstallPublisher)

You can enable the "Available Uninstall" option either at the Global level, where the setting will be applied to all Intune Apps or at the vendor/product level.

**Option 1: Globally Set the Available Uninstall option**

1\. From the **Intune Apps** tab, click the **Options** button  
2\. Check the **Enable 'Allow available uninstall'** check box and click **OK**

![](/_images/EnableAllowAvailableUninstall.png)

**Option 2: Set the Available Uninstall option at the vendor/product level**

1\. From the **Intune Apps** tab, right click a vendor or product and select **Override Win32 application options**

![](/_images/EnableAllowAvailableUninstallProductLevel.png)

2\. Check the **Enable 'Allow available uninstall'** check box and click **OK**

![](/_images/EnableAllowAvailableUninstallProductLevelOption.png)

> **Note:** When the Patch My PC Publisher replaces an application in Intune, the new application will inherit the "Allow available uninstall" value if it had previously been set, even if the setting to enable "Allow available uninstall" in the Patch My PC Publisher has since been toggled off at either the global or product level.

## [Configure "Available Uninstall" in the Intune Admin Centre](#ConfigureAvailableUninstallIntune)

If the application has already been published, you can toggle the available uninstall option from the Intune Admin Centre.

1\. Navigate to [http://intune.microsoft.com](http://intune.microsoft.com)  
2\. Navigate to Apps > Windows and select a Win32 app to edit  
3\. Select **Properties**

![](/_images/Available-Uninstall-5.png)

4\. Scroll down to the **Program** section and click **Edit**

![](/_images/Available-Uninstall-6.png)

5\. Toggle **Allow available uninstall** to **Yes**

![](/_images/Available-Uninstall-7.png)

6\. Click **Review and Save** and **Save**

## [Company Portal Expected Behavior](#AvailableUninstallCompanyPortal)

When the user clicks on the "Uninstall" button, they are presented with a confirmation dialogue.

![](/_images/Available-Uninstall-3.png)

Clicking "Uninstall" again will invoke a policy sync using the Company Portal Intune Management Extension Bridge. The app will be removed and the user will be presented with the following option to "Reinstall" the app. This screen will also indicate if the uninstall was successful.

![](/_images/Available-Uninstall-4.png)

> **Note:** The 'Uninstall' button may not be visible, even if the 'Allow available uninstall' parameter has been configured if the device has never evaluated detection for the application currently deployed as 'Available'. This can occur if an earlier version of the application deployed by Patch My PC was used to initially install the application and the device has subsequently installed an update. Applications and Updates from Patch My PC have different App Ids. If the uninstall option is expected for an application visible in the Company Portal but it is not available, simply click 'Install'. This will invoke detection to run and if the application is detected as installed, the 'Install' button will change to 'Uninstall'.