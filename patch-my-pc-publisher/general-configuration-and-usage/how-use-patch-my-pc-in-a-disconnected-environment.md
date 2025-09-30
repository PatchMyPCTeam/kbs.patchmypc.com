---
title: "How to Use Patch My PC in a Disconnected Environment without Internet"
date: 2020-09-03
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
        - general-configuration-and-usage
        - connectivity-and-proxy-issues
        - best-practices
---

We often get asked if Patch My PC supports publishing third-party updates to WSUS/SCCM in a **totally offline environment with no internet access**. Offline environments are a supported scenario, and luckily the process is similar to how you would export and import normal **[Microsoft updates in a disconnected WSUS environment](https://docs.microsoft.com/en-us/mem/configmgr/sum/get-started/synchronize-software-updates-disconnected)**.

## Example Environment Setup

In our example, we have the following WSUS servers:

- **DEMO1** = This WSUS server is independent on the offline environment and used to sync regular Microsoft updates

- **DEMO2** = A fully offline WSUS server used for client scanning in the disconnected environment. This server has no internet access.

> **[Note from Microsoft Docs](https://docs.microsoft.com/en-us/mem/configmgr/sum/get-started/synchronize-software-updates-disconnected):** When the software update point at the top-level site is **disconnected from the Internet**, you must use the **export** and **import** functions of the **WSUSUtil** tool to synchronize software updates metadata and content. You can choose an existing WSUS server not in your Configuration Manager hierarchy as the synchronization source.

## Step 1: Publish Updates from Internet-Connected WSUS Server

Similar to how Microsoft updates need to be synced from a **[standalone internet-connected WSUS server](https://docs.microsoft.com/en-us/mem/configmgr/sum/get-started/synchronize-software-updates-disconnected)**, you will need to publish third-party updates to an **internet-connected WSUS server first**.

In this example, we have **[installed and configured](https://docs.microsoft.com/en-us/mem/configmgr/sum/get-started/synchronize-software-updates-disconnected)** the Publisher software on DEMO1. We have published 7-Zip and Google Chrome to the WSUS on DEMO1. We also confirmed the updates have been published to WSUS using the **[Modify Published Updates](/modify-published-third-party-updates-wizard)** wizard from the **Advanced** tab.

![Modify Published Updates to Confirm Published Updates for Offline WSUS](images/Modify-Published-Updates-to-Confirm-Published-Updates-for-Offline-WSUS.png)

Once updates are published to the online WSUS server, you can proceed to the **export** step.

## Step 2: Export the WSUS Catalog from the Online-WSUS Server

After publishing updates to the online-WSUS server, you need to **export** the WSUS catalog that includes the metadata for those published updates. The exported file will then be **imported** into the WSUS server in the offline environment.

To [perform the export update metadata](https://docs.microsoft.com/en-us/mem/configmgr/sum/get-started/synchronize-software-updates-disconnected#to-export-software-updates-metadata-from-the-wsus-database-on-the-export-server) from WSUS perform the following:

- Open a **command prompt** as **administrator**

- In the command prompt window, change the directory to the WSUS tools directory. Generally, this will be: **cd "C:\\Program Files\\Update Services\\Tools"**

- Once there, run the **WSUSUtil.exe** export command to export the catalog.
    - **wsusutil.exe export** packagename logfile
    
    - In our example, we ran: **WsusUtil.exe export "C:\\WSUS-Export-2020-09-02.xml.gz" "C:\\WSUS-Export-2020-09-02.log"**
        - **
            ![WSUSUtil export WSUS Catalog](images/WSUSUtil-export-WSUS-Catalog.png)
            **

## Step 3: Import the Exported File on the Offline WSUS Server

Copy the **.xml.gz** from the online WSUS server to the offline WSUS server. On the offline WSUS server, perform the following steps to import the file:

- Open a **command prompt** as **administrator**

- In the command prompt window, change the directory to the WSUS tools directory. Generally, this will be: **cd "C:\\Program Files\\Update Services\\Tools"**

- Once there, run the **WSUSUtil.exe****import** command to export the catalog.
    - In our example, we ran: **WsusUtil.exe import "C:\\WSUS-Export-2020-09-02.xml.gz" "C:\\WSUS-Import-2020-09-02.log"**
        - **
            ![WSUSUtil import WSUS Catalog](images/WSUSUtil-import-WSUS-Catalog.png)
            **

Optionally, after the import, you can run the modify published updates wizard on the offline WSUS server to verify the published third-party updates from the online-WSUS where imported:

![View Published Updates on Offline WSUS](images/View-Published-Updates-on-Offline-WSUS.png)

## Step 4: Copy the WSUSContent and UpdateServicesPackages to the Offline WSUS Server

Once the WSUS metadata has been exported and imported, the last step is to ensure the WSUS content folders are **copied to the offline environment**.

In our scenario, a local network connection exists between **DEMO1** and **DEMO2**, so we used **robocopy** to copy new content in the DEMO1 WSUS folder to DEMO2.

The command in our example was: **robocopy "J:\\WSUS" "\\\\demo2\\j$\\WSUS" /E**

![Copy WSUS Content Folder to Offline WSUS](images/Copy-WSUS-Content-Folder-to-Offline-WSUS.png)

> **Important:** If you do not copy the WSUSContent folder to the offline environment, third-party updates will be unable to download on clients or into a ConfigMgr deployment package. This is because the third-party updates are signed using your [WSUS signing certificate](/wsus-signing-certificate-options-for-third-party-updates-in-configuration-manager) on the online-WSUS server.

**Search terms**: air gap, air-gapped
