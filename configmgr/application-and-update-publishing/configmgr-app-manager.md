# ConfigMgr Application Manager Overview



```yaml
---

taxonomy:
    products:
        - patch-my-pc-publisher
    solutions:
        - 
    tech-stack:
        - configmgr
    post_tag:
        - 
    sub-solutions:
        - best-practices
        - general-configuration-and-usage
        
---
```

Open the **ConfigMgr Application Manager** in the **Options** window found in the **ConfigMgr Apps** tab in the Patch My PC Publisher.

![How to open the ConfigMgr Application Manager](/_images/OpeningConfigMgrAppManager.png "How to open the ConfigMgr Application Manager")

### Available Actions to Perform on Published ConfigMgr Apps <a href="#h-available-actions-to-perform-on-published-configmgr-apps" id="h-available-actions-to-perform-on-published-configmgr-apps"></a>

There are a variety of actions that are available for published ConfigMgr Applications published by Patch My PC.

![Available Options in the ConfigMgr Application Manager Options include: Reload Applications, Extract Content, Delete Deployments, and Delete Applications](/_images/ConfigMgrAppManagerOptions.png "Available Options in the ConfigMgr Application Manager Options include: Reload Applications, Extract Content, Delete Deployments, and Delete Applications")

#### Extract Content… <a href="#h-extract-content" id="h-extract-content"></a>

This option will open a a dialog box that will allow you to save the ConfigMgr application content for the specified application in the selected location.

![Extract Content Feature for ConfigMgr App Manager, allows content to be extracted to the specified folder. Screenshot shows the results of extracting content with arrows pointing to the general workflow.](/_images/ConfigMgrAppMgrExtractContent.png "Extract Content Feature for ConfigMgr App Manager, allows content to be extracted to the specified folder. Screenshot shows the results of extracting content with arrows pointing to the general workflow.")

> **Note:** Extracted application content will not automatically go into a new folder. It is recommended to extract to a destination folder instead of directly to the Desktop or similar location.

#### Delete Deployments <a href="#h-delete-deployments" id="h-delete-deployments"></a>

This option will delete all deployments for the selected application(s).

#### Delete Applications <a href="#h-delete-applications" id="h-delete-applications"></a>

This option will delete the selected application(s) from ConfigMgr. **Associated application content will also be deleted.**

[More information regarding this process can be found here](https://patchmypc.com/how-to-delete-applications-created-by-patch-my-pc-in-sccm)

\