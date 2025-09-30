---
title: "Could not find a part of the path 'C:Program FilesUpdate ServicesSchemabaseapplicabilityrules.xsd'"
date: 2020-12-24
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

---

The error message **_An error occurred while parsing XML node of the catalog: Could not find a part of the path 'C:\\Program Files\\Update Services\\Schema\\baseapplicabilityrules.xsd'_** will occur when the **Schema** folder has been deleted from the WSUS installation directory.

## Determine if You are Affected

If you are affected by this error, you will see the following errors in the **[PatchMyPC.log](/collecting-log-files-for-patch-my-pc-support#publishing-service-app-logs-intune)** file.

An error occurred while parsing XML node of the catalog: Could not find a part of the path 'C:\\Program Files\\Update Services\\Schema\\baseapplicabilityrules.xsd'. \]

## Root Cause for Missing Schema Folder Error

This error can occur when the **[Schema](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/bb972752\(v=vs.85\))** folder is deleted from the **WSUS installation directory** (Default path: C:\\Program Files\\Update Services). This error doesn't occur often, and we don't have any root cause of what may have caused the deletion of the folder for affected customers. In the example below, you can see there is **no Schema folder** in the Update Services folder.

![WSUS-schema-folder-missing-Could not find a part of the path C-Program Files-Update Services-Schema-baseapplicabilityrules.xsd](images/WSUS-schema-folder-missing-Could-not-find-a-part-of-the-path-C-Program-Files-Update-Services-Schema-baseapplicabilityrules.xsd_.png)

## Resolution for Missing Schema Folder Error

The resolution for this error is to restore the **Schema** folder to **C:\\Program Files\\Update Services** from a valid WSUS installation. For easy access, we have attached the Schema folders from a few different versions of Windows Server.

- **[WSUS Schema Folder - Windows Server 2019 Download](/app/uploads/2025/06/WSUS-Schema-Server-2019.zip)**

- [WSUS Schema Folder - Windows Server 2012 R2 Download](/app/uploads/2025/06/WSUS-Schema-Server-2012-R2.zip)

Once the folder is copied to the WSUS server, **restart the WSUS Service** and **PatchMyPCService**.
