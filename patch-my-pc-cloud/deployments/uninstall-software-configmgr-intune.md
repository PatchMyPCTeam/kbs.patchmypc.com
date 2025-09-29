---
title: "How to leverage Patch My PC to uninstall software from devices using ConfigMgr or Intune"
date: 2024-06-03
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

If you want to deploy Patch My PC Packaged software to uninstall it from managed devices, this article shows what you need to take into consideration.

## [Introduction](#introduction)

Sometimes, customers use Patch My PC Publisher to package software for deployment with uninstall intent. This means they want to remove software that shouldn't be on their corporate devices. While software packaged by our Publisher can be deployed with uninstall intent, there are some conditions for the uninstall to work effectively.

## [What do I need to know?](#whatdoineedtoknow)

The uninstall feature for software published by Patch My PC only works if the version you're deploying, or a newer one, is already installed on the targeted device. If the device has an older version installed, the uninstall won't work. This limitation is because of the comparison operators used in our PowerShell detection scripts.

To address this issue, follow the steps listed below, based on the platform you are using - either the [Patch My PC OnPrem Publisher](#patchmypcpublisherconsole) or the [Patch My PC Cloud Portal](#patchmypcpcloudportal).

### [Patch My PC Publisher console](#patchmypcpublisherconsole)

If you are working with the OnPrem Patch My PC Publisher, you'll have to follow these steps:

1. **Select the Same Software**: From the **Updates** tab (for ConfigMgr) or **Intune** **Updates** tab (for Intune), choose the software you want to uninstall from your devices.

3. **Deploy the Update**: Deploy the update to your devices to patch the software to the latest version.

5. **Deploy the App with Uninstall Intent**: At the same time, select the software from the **ConfigMgr** **Apps** or **Intune** **Apps** tab and deploy it with uninstall intent.

Once the software is patched by the update on devices that have it installed, the uninstall of the app will be triggered, and the software will be removed. Even if users manually install the software again, this process will repeat itself as long as the deployments remain active.

### [Patch My PC Cloud portal](#patchmypcpcloudportal)

If you are publishing software from the [Patch My PC cloud portal](https://portal.patchmypc.com/), you'll have to follow these steps:

1. You will have to **deploy** the software from the portal. If you are not familiar with deployments from the Cloud portal, you can find more information [here](https://docs.patchmypc.com/patch-my-pc-cloud/intune-apps-public-preview/deployments).

3. Assign the deployment with **Update only** and **Uninstall** intent and targed the group of devices / users you want to get the software uninstalled from.

![How to assign Update and Uninstall intent using the PMPC Cloud](images/SCR-20240530-jjtv-2.png)

When the software is published to Intune, you will have two win32 apps.

- One of the will be the update - it will patch the software to the latest version on devices that have the software installed.

- The other one is the app, which will uninstall the software as soon as it's patched by the update mentioned above.

If the assignments remain active, the process will be automatically repeated by Intune if the user manually installs the software again.
