---
title: "Intune Application Manager Utility"
date: 2021-04-01
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

The Intune Application Manager Utility provides you with a live view of all Win32 apps currently in your Intune tenant.

It can help you:

- **Export a list of your Intune Win32 apps to .csv**, so you can use our **[Power BI template](https://patchmypc.com/power-bi-reports-for-microsoft-intune-third-party-updates "Power BI Reports for Microsoft Intune Third-Party Update and Application Deployments")** to report on third party patching in Intune

- **Manage live assignments** for all of your existing Win32 apps in Intune

- **Multi-select and delete** Intune Win32 apps or their assignments

## Opening the Intune Application Manager Utility

The utility can be found within the Publisher in either the **Intune Apps** or **Intune Updates** tab, on the right-hand side as a cloud icon with a magnifying glass:

![Intune Application Manager Utility](/_images/IntuneApplicationUtility.png "Intune Application Manager Utility")

Below is an example screenshot of what the utility looks like in a tenant with some Win32 apps. This is a **live view of all Win32 apps** currently in your tenant.

![Live view of apps using the Intune Application Manager Utility](/_images/intune-app-manager-2.png "Live view of apps using the Intune Application Manager Utility")

From the above you can see the following details about your Intune Win32 apps:

- Name

- Description

- Installed

- Install Pending

- Not Applicable

- Classification

- Publisher

- Modified Date

- Assigned

- Purpose

Right-clicking on the column headers enables you to add additional columns to this view:

![Adding more columns to the Intune Application Manager Utility view](/_images/IntuneApplicationUtility-2.png "Adding more columns to the Intune Application Manager Utility view")

## Export a list of your Intune Win32 apps to .csv

Clicking the **Export** button at the bottom will prompt you to save this list of data to a .csv.

This .csv file is used as a data source for our **[Power BI template](https://patchmypc.com/power-bi-reports-for-microsoft-intune-third-party-updates "Power BI Reports for Microsoft Intune Third-Party Update and Application Deployments")** to help you report on third party patch compliance in Intune.

## Manage assignments

Selecting a single app in the list and clicking the **Manage assignments** button at the bottom will produce a dialogue where you can manage the live assignments of your existing Win32 apps currently in Intune.

The UI for managing the assignments here is a similar experience to using the **[right-click Manage assignments](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#ManageAssignments "Right click option for Manage assignments")** option, however, this differs by being a live modification of assignments for existing app. Whereas the right-click option configures the Publisher what assignments to apply to newly created apps or updates.

![Manage live assignments for existing Win32 apps in Intune using the Intune Application Manager Utility](/_images/IntuneApplicationUtility-3.png "Manage live assignments for existing Win32 apps in Intune using the Intune Application Manager Utility")

## Delete assignments

Selecting one or more apps in the list and choosing the **Delete assignments** button at the bottom deletes all of the assignments associated with all the select apps. This is a helpful function because doing such a task in the Intune console today involves more UI interaction, whereas here we can do that for multiple Win32 apps in fewer clicks.

![Deleting assignments using the Intune Application Manager Utility](/_images/IntuneApplicationUtility.gif "Deleting assignments using the Intune Application Manager Utility")

## Delete applications

Similar to the **Delete assignments** action, selecting one or more apps in the list and choosing the **Delete applications** button at the bottom deletes all of the selected Win32 apps from your tenant. This is a helpful function because doing such a task in the Intune console today involves more UI interaction, whereas here we can do that for multiple Win32 apps in fewer clicks.

![Deleting applications using the Intune Application Manager Utility](/_images/IntuneApplicationUtility2.gif "Deleting applications using the Intune Application Manager Utility")