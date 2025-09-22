# Migrate Third-Party Application Management and Patching from ConfigMgr/SCCM to Intune

This article provides a detailed step-by-step guide on how to make this transition, ensuring a seamless and efficient patch management process. &#x20;

### Pre-requisites <a href="#h-pre-requisites" id="h-pre-requisites"></a>

As a prerequisite, please ensure you have co-management configured in your environment and your devices enrolled into Intune.&#x20;

This article will not cover how to setup co-management, however, you can refer to the following tutorial on Microsoft Learn: [Tutorial: Enable co-management for existing clients – Configuration Manager | Microsoft Learn](https://learn.microsoft.com/en-us/mem/configmgr/comanage/tutorial-co-manage-clients)&#x20;

### Move the Client Apps Workload <a href="#h-move-the-client-apps-workload" id="h-move-the-client-apps-workload"></a>

The Client Apps workload is unique because it doesn’t fully transfer app management from ConfigMgr to Intune. Instead, it allows apps to be targeted by both platforms, consecutively.

To enable Patch My PC Win32 apps and updates through Intune, the Client Apps workload must be moved.

For more details, refer to [Co-management workloads – Configuration Manager | Microsoft Learn](https://learn.microsoft.com/en-us/mem/configmgr/comanage/workloads#client-apps)

**Tip:** The Company Portal app is now the cross-platform app portal experience for the Microsoft Intune family of products. By configuring co-managed devices to also use the Company Portal app, you can provide a consistent user experience on all devices. Read more at [Apps in Company Portal – Configuration Manager | Microsoft Learn](https://learn.microsoft.com/en-us/mem/configmgr/comanage/company-portal)&#x20;

To move the Client Apps workload, in the Configuration Manager console:

1.  Navigate through **Administration** -> **Cloud Services** -> **Cloud Attach**, right-click CoMgmtSettingsProd and click **Properties**.

    <figure><img src="https://patchmypc.com/app/uploads/2025/04/clientapps_move1.png" alt="" width="700"><figcaption></figcaption></figure>
2. Go to the **Workloads** and set the slider for C**lient apps** to **Pilot Intune** or **Intune**. With the Client Apps workload moved to Intune or Pilot Intune, the co-managed devices will receive applications deployed from both ConfigMgr and Intune.\

3.  If you moved the workload to the **Pilot Intune** position, you will need to now click on the tab **Staging** and choose a collection of devices. Click **OK** to save and close. Only the devices in this collection will have their Client Apps workload moved to Intune.\


    <figure><img src="https://patchmypc.com/app/uploads/2025/04/clientapps_move3.png" alt="" width="300"><figcaption></figcaption></figure>

### Reviewing CoManagementHandler.log <a href="#h-reviewing-comanagementhandler-log" id="h-reviewing-comanagementhandler-log"></a>

Connect to a client to make sure that the Client Apps workload has moved as expected. &#x20;

You can check this in two places:&#x20;

1. &#x20;“C:\Windows\CCM\Logs”. Open the **CoManagementHandler.log** and search for the string **CoManagementSettings\_Capabilities.**\

2.  Open the **Configuration Manager Properties** Control Panel applet on the client device. On the **General** tab look for the field **Co-Management capabilities**.\


    <figure><img src="https://patchmypc.com/app/uploads/2025/04/clientapps_move5.png" alt="" width="300"><figcaption></figcaption></figure>

The table below contains workload values that indicate the Client Apps workload has been moved for that device.

| 8257  | 8259  | 8261  | 8263  | 8265  | 8267  | 8269  | 8271  |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| 8273  | 8275  | 8277  | 8279  | 8281  | 8283  | 8285  | 8287  |
| 8385  | 8387  | 8389  | 8391  | 8393  | 8395  | 8397  | 8399  |
| 8401  | 8403  | 8405  | 8407  | 8409  | 8411  | 8413  | 8415  |
| 12385 | 12387 | 12389 | 12391 | 12393 | 12395 | 12397 | 12399 |
| 12401 | 12403 | 12405 | 12407 | 12409 | 12411 | 12413 | 12415 |
| 12513 | 12515 | 12517 | 12519 | 12521 | 12523 | 12525 | 12527 |
| 12529 | 12531 | 12533 | 12535 | 12537 | 12539 | 12541 | 12543 |

### Configuring Intune Integration in the Patch My PC Publisher <a href="#h-configuring-intune-integration-in-the-patch-my-pc-publisher" id="h-configuring-intune-integration-in-the-patch-my-pc-publisher"></a>

If you are already using Patch My PC Publisher today with your ConfigMgr/SCCM and you want to move your current software selection and configurations across to Intune, the process is simple.&#x20;

Firstly, you need to create an app registration in your Entra ID tenant to allow the Patch My PC Publisher to connect to Intune via the Microsoft Graph in order to create Win32 apps in Intune. &#x20;

The following documentation explains how to create the app registration and establish the connection in the Patch My PC Publisher: [Azure App Registration | Getting Started (patchmypc.com)](https://docs.patchmypc.com/installation-guides/intune/azure-app-registration)&#x20;

Once the app registration is in place, you can begin to configure Intune apps and updates. You can manually select the products and customizations, or copy your existing ConfigMgr apps and customizations from the **Updates** tab.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/clientapps_move6.png" alt=""><figcaption></figcaption></figure>

Repeat the process for the Intune Updates tab to bring across the same product selection and customizations for Intune Updates.

You can find detailed information on setting up the Patch My PC Publisher for Intune at [https://docs.patchmypc.com/installation-guides/intune](https://docs.patchmypc.com/installation-guides/intune) or you can also book a FREE Setup Call with one of our engineers at [https://patchmypc.com/schedule-setup-call](https://patchmypc.com/schedule-setup-call) and we can help you integrate the Patch My PC Publisher to Intune your environment.&#x20;

### Stop targeting updates from ConfigMgr <a href="#h-stop-targeting-updates-from-configmgr" id="h-stop-targeting-updates-from-configmgr"></a>

Co-managed devices are still able to receive applications and third-party updates from ConfigMgr. This may be undesirable if you have moved your Client apps and/or Windows Updates workload to Intune.

If you are now using Windows Update for Business Policies from Intune to control first-party (Microsoft) updates and are also updating devices using the Win32 apps created by Patch My PC, you should consider disabling the “Software Updates” client setting for those co-managed devices. This will prevent the device from reaching out to WSUS to perform compliance scans and from receiving “Software Update Deployments” from ConfigMgr.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/clientapps_move7.png" alt=""><figcaption></figcaption></figure>

Targeting third-party updates from both ConfigMgr and Intune is theoretically possible and will not cause any problems. The device will get updated **but** there is no control over which platform will install the update – leading to confusion when reviewing deployment reports from either ConfigMgr or within the Intune Admin Center.

**Further Reading**

You can read more about how ConfigMgr controls the Windows Update agent with ScanSource Policy and DualScan at [SCCM Co-management – Dual Scan and Scan Source Demystified – Patch My PC](https://patchmypc.com/sccm-co-management-dual-scan-and-scan-source-demystified).
