---
title: "ConfigMgr Application Manager Overview"
date: 2022-09-28
taxonomy:
    products:
        - patch-my-pc-publisher
    tech-stack:
        - configmgr
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - best-practices
        - general-configuration-and-usage
---

The ConfigMgr Application Manager in the Patch My PC Publisher is a tool to help you quickly and easily manage ConfigMgr Applications created by the Patch My PC Publisher.

Open the **ConfigMgr Application Manager** in the **Options** window found in the **ConfigMgr Apps** tab in the Patch My PC Publisher.

![How to open the ConfigMgr Application Manager](images/OpeningConfigMgrAppManager.png)

## Available Actions to Perform on Published ConfigMgr Apps

There are a variety of actions that are available for published ConfigMgr Applications published by Patch My PC.

![Available Options in the ConfigMgr Application Manager Options include: Reload Applications, Extract Content, Delete Deployments, and Delete Applications](images/ConfigMgrAppManagerOptions.png)

### Extract Content...

This option will open a a dialog box that will allow you to save the ConfigMgr application content for the specified application in the selected location.

![Extract Content Feature for ConfigMgr App Manager, allows content to be extracted to the specified folder. Screenshot shows the results of extracting content with arrows pointing to the general workflow.](images/ConfigMgrAppMgrExtractContent.png)

> **Note:** Extracted application content will not automatically go into a new folder. It is recommended to extract to a destination folder instead of directly to the Desktop or similar location.

### Delete Deployments

This option will delete all deployments for the selected application(s).

### Delete Applications

This option will delete the selected application(s) from ConfigMgr. **Associated application content will also be deleted.**

[More information regarding this process can be found here](https://patchmypc.com/how-to-delete-applications-created-by-patch-my-pc-in-sccm)
