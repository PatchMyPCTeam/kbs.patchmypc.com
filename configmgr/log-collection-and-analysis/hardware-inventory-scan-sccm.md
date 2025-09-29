---
title: "Understanding the Hardware Inventory Scan Cycle in Configuration Manager"
date: 2024-07-05
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

Over a year ago, we published an [article](https://patchtuesday.com/blog/tech-blog/mastering-configmgr-client-actions/) on the PatchTuesday tech blog discussing the various  Configuration Manager (or simply ConfigMgr) Client actions, including the one we'll discuss today - the Hardware Inventory scan cycle. The article detailed what each does, when it’s useful to trigger a specific action, and the relevant logs to monitor on the client side. The article was written because the information was highly relevant to many people at the time.

That article provided a general overview of all the available actions. In this article, we’ll examine one of the most powerful actions: the Hardware Inventory Scan Cycle. We’ll discuss the data it collects, how to configure its automatic execution, and the various methods to trigger it manually.

## [What is the Hardware Inventory Scan Cycle?](#WhatIsTheHinvCycle)

Before answering this question, we need to know what the hardware inventory is.  
Briefly put, it is a collection of detailed information about client devices in your environment. The scan cycle triggers the collection of data from your devices.  
By default, the hardware inventory data collection is disabled in Configuration Manager. You will have to enable it by following the steps [provided by Microsoft](https://learn.microsoft.com/en-us/mem/configmgr/core/clients/manage/inventory/introduction-to-hardware-inventory).

## [What does it do, and what data does it collect?](#WhatDoesItDoAndCollect)

The Hardware Inventory Scan Cycle collects a wide range of data about the hardware configuration of client devices, including:

- **Processor details**: Type, speed, and number of cores.

- **Memory information**: Total physical memory and available memory.

- **Storage details**: Disk drives, partitions, and available space.

- **Network information**: Network adapters, IP addresses, and MAC addresses.

- **Peripheral devices**: Monitors, keyboards, and mice.

- **System details**: BIOS version, manufacturer, and model.

- Last but not least, **Installed software**: A list of installed programs and updates.

> **Note:**Yes, the software inventory data is collected by the Hardware Inventory Cycle, not by Software Scan Cycle.

## [Configuring Automatic Hardware Inventory Scan Cycle](#ConfiguringAutomaticHardwareInventoryScanCycle)

To configure the Hardware Inventory Scan Cycle to run automatically, follow these steps:

1. **Open the ConfigMgr console** and navigate to **Administration > Client Settings**.

3. Edit the **Default Client Settings** or create a new, custom client settings policy.

5. Right-click on your client settings, select **Properties**, and go to the **Hardware Inventory** section.

7. Enable hardware inventory and configure the schedule for the inventory scan cycle (e.g., every 7 days).

9. Apply the settings and deploy the client settings policy to the desired collection.  
    **Note**: It's not needed to deploy the client settings if you edit the Default Client Settings.

This setup ensures the Hardware Inventory Scan Cycle runs automatically according to the specified schedule.

![](/_images/01_hinv.png)

## [Ways to manually invoke the Hardware Inventory Scan Cycle](#HowToManuallyInvokeIt)

In some cases, you might need to trigger the Hardware Inventory Scan Cycle manually instead of waiting for the automated sync. Here are several methods to do so:

### 1\. From the Client Side

One method is by using the Control Panel **Configuration Manager** Applet:

1. Open the **Control Panel** on the client device.

3. Go to **System and Security** > Configuration Manager.

5. In the Actions tab, select Hardware Inventory Cycle and click Run Now.

The other method of manually invoking the scan cycle from a client is by using PowerShell:

1. Open PowerShell as Administrator

3. Run this command:  
    
    > Invoke-WmiMethod -Namespace "rootCCM" -Class SMS\_Client -Name TriggerSchedule -ArgumentList "{00000000-0000-0000-0000-000000000001}"
    

### 2\. Using PowerShell Remoting

PowerShell remoting may or may not be enabled in your organization. Some orgs prefer to disable it, as they consider it a security risk.  
Truth be told, it can be a security risk if not properly configured. But that’s not the scope of this article.

Here is how to invoke the scan cycle on a remote computer within your network:

1. Open PowerShell as Admin with an account that has PowerShell Remoting privileges.

3. Run this code:  
    
    > Invoke-Command -ComputerName EnterComputerName -ScriptBlock {  
    > Invoke-WmiMethod -Namespace "rootccm" -Class "sms\_client" -Name "TriggerSchedule" -ArgumentList "{00000000-0000-0000-0000-000000000001}"  
    > }
    

###  3. From the SCCM Console

1. Open the ConfigMgr console and navigate to **Assets and Compliance** > **Devices**.

3. Locate the device and right-click on it.

5. In the **Client Notification** section, choose **Collect** **Hardware Inventory**.

### 4\. Using Support Center Client Tools

This is also a client-sided tool, but I wanted to allocate an additional section to it.

This tool can be distributed to your devices with the MSI that resides in your “%ConfigMgr Install Directory%toolsSupport Center”.

With the MSI installed, you will find an app named **Configuration Manager Support Center Client Tools**.

This tool will give you more information about the Configuration Manager agent on the device, such as:

- Client information (assigned Management Point, Site Code, Client Version, etc),

- Policy details

- Content details, deployments, cache information, etc

- Troubleshooting

- Log viewer

- The most important, however, is the **Inventory**.

It can give you information on when the last hardware inventory was performed:

![](/_images/02_hinv.png)

If you’re curious about the information stored in the WMI collected by the Hardware Inventory Scan Cycle, this tool can give you that insight as well:

![](/_images/03_hinv.png)

This option can be useful if you’re setting additional classes to be collected by the hardware inventory, and you’re curious to know if it’s collected, it’s one option to check.

But back to the topic. If you wish to trigger a Hardware Inventory cycle, do this:

- Open Configuration Manager Support Center Client Tools

- Navigate to the **Inventory** tab

- Click on the **Invoke trigger** dropdown

- Select **Hardware inventory cycle** if you want to run a Delta sync.  
    OR

Select  **Advanced** and then **Hardware inventory cycle (full resynchronization)** to run a full sync.  
![](/_images/04_hinv.png)

### 5\. Using Advanced Insights

If you’re unaware, [Advanced Insights](https://patchmypc.com/advanced-insights/overview) is an innovative reporting and patch compliance solution for Configuration Manager offered by Patch My PC.

Among the very useful information this tool offers (and I encourage you to check it out, as it might be something your org can benefit from), it allows you to trigger some client actions, including the cycle we are discussing today.

To trigger the cycle, follow these steps:

- Access the Advanced Insights reporting website

- Navigate to Resources à Devices à Managed devices and locate your device using the Quick Search option.

From the **Actions** dropdown, select the **Hardware Inventory** option.

![](/_images/05_hinv.png)

## [Viewing the data using Resource Explorer](#ResourceExplorer)

Once the data is collected, you probably want to view it. Here’s how:

- Open the ConfigMgr console and navigate to **Assets and Compliance** > **Devices**.

- Locate and select the desired device.

- Right-click on the device and choose **Start** > **Resource Explorer**.

In the Resource Explorer Window, you can browse the various categories to see the information collected from clients, such as Installed applications, Operating System, Network adapters, and more.  
This information can be used to create a dynamic collection, for example.

![](/_images/06_hinv.png)

## [Relevant logs to monitor on the client side](#RelevantLogs)

If you want to monitor the evaluation of this action, it can be done using these logs located on the client side in the %WinDir%\\CCM\\Logs folder:

- **InventoryAgent.log** – Records activities of hardware inventory, software inventory, and heartbeat discovery actions on the client.

- **InventoryProvider.log** – More details about hardware inventory, software inventory, and heartbeat discovery actions on the client.

If you trigger a **Delta** hardware inventory scan, this is how the InventoryAgent.log will look like:

![](/_images/07_hinvV2.png)

> **Note:** Don’t worry about the Warning messages. If a class the hardware inventory scan cycle is supposed to collect data for, but the class doesn’t exist on the device and there is no data to collect, you’ll see the warning.

However, if you trigger a full hardware inventory sync using the Support Center Client Tools, this is how the log is going to look like:

![](/_images/08_hinvV2.png)

Every time a delta hardware inventory sync is performed, the minor version is incremented (e.g., from 6.1 to 6.2). When a full hardware inventory sync is performed, the major version is incremented, and the minor version resets (e.g., from 6.2 to 7.0).

When the site server receives a new report from the client, the client is instructed to perform a delta or full inventory sync if the version does not match what the server expects.

## [Conclusion](#Conclusion)

In conclusion, the Hardware Inventory Scan Cycle in ConfigMgr plays an important part in keeping a current list of your organization's hardware. Knowing how to set it up and run it manually, as well as which logs to check, helps IT administrators manage and troubleshoot hardware inventory. The Resource Explorer makes it easy to view and analyze the collected data, leading to better asset management, compliance, and efficiency in the IT infrastructure.