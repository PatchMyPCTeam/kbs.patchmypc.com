---
title: "Configuring Standalone WSUS Mode"
date: 2020-05-10
taxonomy:
    products:
        - 
    tech-stack:
        - wsus
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - general-configuration-and-usage
        - best-practices
        - troubleshooting
---

## Description

This article will show you how the Patch My PC Publisher can be set up to publish updates in a **standalone WSUS** environment. A full guide on how to configure WSUS standalone can be found here: [WSUS Standalone - Getting Started (patchmypc.com).](https://docs.patchmypc.com/installation-guides/wsus-standalone)

# Configuring Standalone WSUS Mode

You can publish third-party updates to **standalone WSUS** without SCCM. To allow updates to appear **directly in WSUS**, follow the instructions in this article.

Standalone WSUS mode is **only required when you are not using Configuration Manager** for software updates and are **only using WSUS**. When enabled, published third-party updates will appear directly in the WSUS console.

## Enable Standalone WSUS Mode

To enable standalone WSUS mode, go to the **Updates** tab and select **Options**. In the **WSUS options** window check the box **Make updates appear in the WSUS console. This option isn’t needed if using Configuration Manager.**

![](/_images/standalone-WSUS-2.png)

If your WSUS database is using a **Windows Internal Database** (WID), the **database connection options will be read-only**.

When using a **SQL Server for the WSUS database**, you will need to choose either to connect to the database using the **SYSTEM account** or define a user name and password using **SQL authentication**.

![Connect as SYSTEM for WSUS](/_images/connection-options-wsus-standalone-mode.png "Connect as SYSTEM for WSUS")

## Verify Third-Party Updates Appear in the WSUS Console

Any updates published after this setting was enabled should appear directly in the WSUS console.

![](/_images/standalone-WSUS-3.png)

You can now deploy the third-party updates just like a Microsoft update **directly from the WSUS console**.

If updates were published **before this setting was enabled**, they would **not appear in the WSUS console automatically**.

For updates published before WSUS Standalone mode was enabled, use the **[Modify Published Updates Wizard](/modify-published-third-party-updates-wizard)** to make those updates appear in the WSUS console.

## SQL permissions required to publish update information to the database

When the Patch My PC Publishing Sync runs, if the SUSDB is remote from the WSUS Standalone server, you would have to grant specific permissions to the computer account where PMPC is installed for it to be able to update information.

The script below can be used to grant those permissions. Make sure to replace the computer account values with the ones appropriate to your environment.

The code can be edited, and run as a SQL query to grant the permissions.

```
USE SUSDBGO-- Replace CONTOSO\ServerName$ with the appropriate value for your environmentDECLARE @UserName nvarchar(128) = 'CONTOSO\ServerName$'DECLARE @QuotedUserToGrant nvarchar(128) = QUOTENAME(@UserName);IF NOT EXISTS(SELECT principal_id FROM sys.server_principals WHERE name = @UserName) BEGINDECLARE @LoginSQL as varchar(500);SET @LoginSQL = 'CREATE LOGIN '+ @QuotedUserToGrant + ' FROM WINDOWS';EXEC (@LoginSQL);ENDIF NOT EXISTS(SELECT principal_id FROM sys.database_principals WHERE name = @UserName) BEGINDECLARE @UserSQL as varchar(500);SET @UserSQL = 'CREATE USER ' + @QuotedUserToGrant + ' FOR LOGIN ' + @QuotedUserToGrant;EXEC (@UserSQL);ENDDECLARE @PermissionsSQL as varchar(500);SET @PermissionsSQL = 'GRANT UPDATE ON [dbo].[tbUpdate] ([IsLocallyPublished]) TO ' + @QuotedUserToGrant +'GRANT SELECT ON [dbo].[tbUpdate] ([UpdateID]) TO ' + @QuotedUserToGrant;EXEC (@PermissionsSQL);
```