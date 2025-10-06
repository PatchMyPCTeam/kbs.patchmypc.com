---
title: How to Rollback or Uninstall Third-Party Software Updates in ConfigMgr
date: 2020-11-25T00:00:00.000Z
taxonomy:
  products:
    - null
  tech-stack:
    - configmgr
  solution:
    - null
  post_tag:
    - null
  sub-solutions:
    - workarounds
    - updates
    - best-practices
---

# How Rollback Uninstall Third Party Update In ConfigMgr

This article describes how to **rollback/uninstall a third-party software update in Configuration Manager (ConfigMgr)**. We get asked: _how do I rollback a third-party update if there are issues in my environment_. This guide will describe your options and some limitations that exist.

### No Native Support to Rolling Back Any Updates in ConfigMgr (Including Windows Updates)

It's important to understand that the [**software update feature**](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/deploy-software-updates) in Configuration Manager doesn't natively support the uninstall/rollback of any updates, including Microsoft updates.

If you needed to uninstall a Microsoft update, you would need to create a task sequence or package that runs the [**wusa.exe**](https://support.microsoft.com/en-us/help/934307/description-of-the-windows-update-standalone-installer-in-windows) uninstall command manually. An example of this process is available at [**Uninstall Windows Update Using SCCM**](https://systemcenterdudes.com/sccm-uninstall-windows-update/).

### How to Rollback a Third-Party Update to a Previous Version in ConfigMgr

To rollback a third-party software update, you can use the [**application management feature**](../../automating-application-packaging-in-microsoft-sccm/) available in Patch My PC. You can use [**application supersedence**](https://docs.microsoft.com/en-us/mem/configmgr/apps/deploy-use/revise-and-supersede-applications#application-supersedence) in ConfigMgr to **revert an application to a previous version.**

In the example below, we will show how to **revert Google Chrome 87.0.4280.66 (x64)** to **Google Chrome 84.0.4147.105 (x64)**. To perform a rollback, you will need to have an application you want to rollback to as well as the version you want to revert from (the software update that was installed).

![Multiple Google Chrome Versions to Rollback Third-Party Update in SCCM](/_images/Multiple-Google-Chrome-Versions-to-Rollback-Third-Party-Update-in-SCCM.png "Multiple Google Chrome Versions to Rollback Third-Party Update in SCCM")

> **Note:** If you are using the [**Update existing application’s metadata, deployment type, detection method, and content files (Default)**](https://patchmypc.com/base-install-update-options-explained#topic1), you can enable the [retain up to X previously created applications](https://patchmypc.com/base-install-update-options-explained#topic3) in the [**ConfigMgr application options**](https://patchmypc.com/base-install-update-options-explained) to provide you with a rollback option. Otherwise, previous application versions would not be available from Patch My PC.
>
> ![](/_images/Rollback-1-2.png)

To revert to an older version, go to the **properties** of the application you want to revert to and perform the following actions: Click the **Supersedence** tab > click **Add...** > Click **Browse**... > Select the **latest application** your want to uninstall > Select the deployment type in the **New Deployment Type** drop-down > and Check the **Uninstall** checkbox.

![Supersede Application for Third-Party Update Rollback in SCCM](/_images/Supercede-Application-for-Third-Party-Update-Rollback-in-SCCM.gif "Supersede Application for Third-Party Update Rollback in SCCM")

You can now [**deploy the application**](https://docs.microsoft.com/en-us/mem/configmgr/apps/deploy-use/deploy-applications) you want to revert to, and the deployment will automatically uninstall the latest update before the rollback.

> **Note:** These rollback instructions apply to a **required** deployment. It is not recommended to apply these steps to an **available** rollback deployment. All the Patch My PC detection methods check for software versions greater than or equal to the desired version. The **old** application will return detected when the **new** version is installed. This can cause supersedence relationships to be unexpectedly evaluated during an available deployment.&#x20;

### Perform an Uninstall of a Third-Party Application in ConfigMgr

If you want to remove a third-party **application entirely**, you can use the [**uninstall feature**](https://docs.microsoft.com/en-us/mem/configmgr/apps/deploy-use/uninstall-applications#uninstall-an-application) of the [application management feature](https://docs.microsoft.com/en-us/mem/configmgr/apps/understand/introduction-to-application-management). Almost all applications created by Patch My PC will have the uninstall program defined in the applications [**deployment type**](https://docs.microsoft.com/en-us/mem/configmgr/apps/deploy-use/create-applications#bkmk_create-dt).

![Uninstall Program for SCCM Application Deployment Type](/_images/Uninstall-Program-for-SCCM-Application-Deployment-Type.png "Uninstall Program for SCCM Application Deployment Type")

You can deploy the application you want to remove as an [**uninstall deployment**](https://docs.microsoft.com/en-us/mem/configmgr/apps/deploy-use/deploy-applications#bkmk_deploy-settings) to remove it from a device collection.