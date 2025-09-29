---
title: "Intune Global Options"
date: 2022-12-07
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

There is an option in the Patch My PC Publisher to store the encryption keys used to create the Intune package files (.intunewin). This is configurable in the advanced tab.

With the keys stored, you can use the [Intune Application Manager](https://patchmypc.com/intune-application-manager-utility) to download and extract the content of the Patch My PC published Intune applications and updates.

## Enable the option to track the encryption keys used to create Intune package files

Navigate to the **Advanced** tab in the Patch My PC Publisher and under **Intune Global Options** check the box labelled **When publishing an Intune package, locally keep track of encryption information to allow later extraction**

![](/_images/IntunePackageExtraction_1-1.png)

After you click **Apply,** this new setting will be reflected in the **IntuneTenants RetainCryptoInfo** element in settings.xml. It will change from **False** to **True**

![](/_images/IntunePackageExtraction_2.png)

> **Note:** The default location for settings.xml is C:\\Program Files\\Patch My PCPatch My PC Publishing Service

## Using the Intune Application manager to download and extract applications and updates published by Patch My PC

From either the Intune Apps or Intune Updates tab, open the [Intune Application Manager](https://patchmypc.com/intune-application-manager-utility)

![](/_images/IntunePackageExtraction_3.png)

Right click an application and select **Extract Content**

![](/_images/IntunePackageExtraction_4.png)

> **Note:** If the Extract Content option is greyed out, it means that the encryption keys were not gathered when the app was published. This is most likely due to the feature being enabled "after" the app or update had been published.

Either enter an **Output Path** manually or click **Browse** to choose the output directory.

![](/_images/IntunePackageExtraction_5.png)

Click **Extract**

![](/_images/IntunePackageExtraction_7.png)

Explorer will open the window to the path specified and display the extracted content

![](/_images/IntunePackageExtraction_8.png)