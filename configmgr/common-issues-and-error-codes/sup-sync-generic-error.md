---
title: "An error occurred while triggering a SUP synchronization: Generic failure [-1]"
date: 2020-05-08
taxonomy:
    products:
        - 
    tech-stack:
        - configmgr
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - common-issues-and-error-codes
        - troubleshooting
        - security
---

In the **[PatchMyPC.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-logs)**, you may see the following error when the option to trigger a software update point sync is enabled.

An error occurred while triggering a SUP synchronization: Generic failure \[-1\]  
Failed to triggered a SUP..sync

**OR**

An error occurred while trying to trigger a SUP sync: Access is denied.

This error will occur when the **computer account** of the server running the publisher doesn't have the necessary permissions to sync the software update point.

## Import the Patch My PC - Base Installation Security Role

The easiest fix for this issue is to **[download our Security Role XML file](https://patchmypc.com/app/uploads/2025/06/Patch-My-PC-Base-Installations-6-26-20.zip)** for Configuration Manager. This security role will contain permissions required to **[create applications](/permissions-required-in-sccm-for-base-installation-packages-from-patch-my-pc)** as well as permissions to sync the software update point.

This security template was updated on **May 7, 2020** to include the **Read** and **Modify** permissions on the **Software Updates** class.

Import the file **Patch My PC - Base Installations.xml** from the extracted ZIP file in your console under **Administration** > **Security** > **Security Roles**

![](../../_images/security-role-software-updates-modify-and-read-in-sccm.png)

Validate the security role has **Read** and **Modify** for **Software Updates**.

## Automatically Create the Configuration manager Security Role for the Patch My PC Publisher

In build 1.8.6 or newer, the Publisher can **automatically create** the Security role with the minimum permissions for you.

![Auto create ConfigMgr security role for Patch My PC](images/Create-ConfigMgr-Security-Role-for-Publisher-Automatically.png)

After the security role is created, you will need to **assign the computer account** of the server running the publisher to it.

![Assign ConfigMgr Security Role](images/assign-configmgr-security-role-to-server.png)

## Assign the Computer the Security Role

Add the computer account of the server running the publisher in the **Administration** > **Security** > **Administrative Users** node on the Configuration Manager console.

![](../../_images/computer-account-SCCM-permissions.png)

After the computer account is added, ensure you assign the security role for Patch My PC.

![](../../_images/assign-security-role-sccm-patchmypc.png)
