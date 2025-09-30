---
title: "Verifying WSUS Server settings"
date: 2022-07-29
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
        - troubleshooting
        - best-practices
---

This topic covers typical WSUS Server settings as recommended by Microsoft.

This section covers the following issues which affect update file synchronization and download:

- [Registry Settings](#topic1)

- [Configuration Settings](#topic2)

- [IIS Settings](#topic3)

- [Permissions](#topic4)

> **Note:**These settings are configured during WSUS setup by default. They are listed here as a reference, to use as checkpoints when troubleshooting. When troubleshooting, you can verify that these settings are in place.

## Registry Settings

Following are registry settings configured during setup on the WSUS server. These settings do not store server configuration information. All configuration information is stored in the WSUS database (SUSDB.mdf).

All of the following Registry entries are within the **HKLM\\SOFTWARE\\Microsoft\\Update Services\\Server\\Setup** Registry key:

- **ContentDir** – the location under which update binaries and end user license agreement files are stored. If the user chose to install WMSDE during setup, this location also contains the database storage files and log files; for example, C:\\WSUS. Note the following:
    - **WsusContent** contains the update files
    
    - **MSSQL$WSUS** contains the database files (if WMSDE)

- **TargetDir** – the product installation location; for example, C:\\Program Files\\Update Services

- **WmsdeInstalled** – this entry specifies whether or not WMSDE was used in the original installation; for example, 1=yes, 0=no. Note: This key does not get modified if your later migrate the WMSDE database to a full SQL Server database.

- **SqlServerName** – The main registry key used under regular server operation. This is used to bootstrap the server components with the database server where the rest of data and server configuration is used. Ex: %computername%WSUS for WMSDE. Use this key to quickly figure out which SQL server the WSUS server is using (especially in the remote SQL case).

- **SqlDatabaseName** – the name of the database. For WSUS 2.0, this is always SUSDB.

- **SqlAuthenticationMode** – the authentication mode WSUS uses to talk to the database server. For WSUS 2.0, this is always WindowsAuthentication.

## Configuration settings

All of the following server configuration settings are stored inside the WSUS database (SUSDB.mdf).

| **What is configured** | **Database storage location** | **Description** |
| --- | --- | --- |
| Update Storage | _tbConfigurationA.SyncToMu tbConfigurationA.UpstreamServerName_ | The first database location specifies the update source for client computers. The values possible are: 0 – WSUS server 1 – Microsoft Update The second database location specifies the name of the upstream WSUS server, if you have chosen one as the update source. |
| Express (PSF) file download | _tbConfigurationC.DownloadExpressPackages_ | This setting controls whether or not express installation files are downloaded. The values possible are: 0 – Do not download express files (default) option. 1 – Download express files. On the WSUS console, this is configured on the Advanced Synchronization Options box. |
| Language options | _tbLanguage.Enabled_ | This setting controls which language binaries are to be downloaded. By default, all languages are enabled. On the WSUS console this is configured on the Advanced Synchronization Options box. |
| BITS download priority | _tbConfigurationC.BitsDownloadPriorityForeground_ | This internal setting specifies whether or not to use foreground priority for BITS downloads. The default is to use throttled downloads. This setting was added to handle issues with certain proxy servers that did not correctly handle HTTP 1.1 restartable downloads. |

## IIS Settings

The following virtual directories (vroots) are created in IIS (in the Default Web Site by default) for client to server synchronization, server to server synchronization, reporting, and client self-update.

| **Vroot in IIS** | **Properties** |
| --- | --- |
| ClientWebService | Directory: %ProgramFiles%\\Update Services\\WebServices\\ClientWebService Application Pool: WsusPool Security: Anonymous Access Enabled. Execute Permissions: Scripts Only |
| Content | Directory: E:\\wsus\\wsuscontent Security: Anonymous Access Enabled Execute Permissions: None |
| DssAuthWebService | Directory: %ProgramFiles%\\Update Services\\WebServices\\DssAuthWebService Application Pool: WsusPool Security: Anonymous Access Enabled. Execute Permissions: Scripts Only |
| ReportingWebService | Directory: %ProgramFiles%\\Update Services\\WebServices\\ReportingWebService Application Pool: WsusPool Security: Anonymous Access Enabled. Execute Permissions: Scripts Only |
| ServerSyncWebService | Directory: %ProgramFiles%\\Update Services\\WebServices\\ServerSyncWebService Application Pool: WsusPool Security: Anonymous Access Enabled. Execute Permissions: Scripts Only |
| SimpleAuthWebService | Directory: %ProgramFiles%\\Update Services\\WebServices\\SimpleAuthWebService Application Pool: WsusPool Security: Anonymous Access Enabled. Execute Permissions: Scripts Only |
| WSUSAdmin | Directory: %ProgramFiles%\\Update Services\\Administration Application Pool: WsusPool Security: Integrated Windows Authentication. Execute Permissions: Scripts Only |
| SelfUpdate | Directory: %ProgramFiles%\\Update Services\\SelfUpdate Security: Anonymous Access Enabled, Integrated Windows Authentication. Execute Permissions: Scripts Only |

## Permissions

The following lists permissions necessary for specific folders on the WSUS server disk and registry permissions.

**Disk**

The following permissions are configured during WSUS setup, and are important for BITS downloads to work:

- The root folder on the drive where the **WSUSContent** folder resides (for example, **<%windir%>**\\WSUS\\WsusContent) must have **Read** permissions for either the **Users** account or the **NT Authority\\Network Service** account (on Windows 2003). If this permission is not set, BITS downloads will fail. Note: this is the permission that WSUS setup does not configure, so make sure the permissions are set as described here

- The WSUS content directory, usually **<%windir%>\\WSUS\\WsusContent** must have **Full Control** permission granted to the **NT Authority\\Network Service** account. This permission is set by WSUS server setup when it creates the directory, but it is possible that your security software might reset this. permission. Not having this permission set will also cause BITS downloads to fail.

- The **NT Authority\\Network Service** account (on Windows 2003) must have **Full Control** permissions to the following folders for the WSUS console to display the pages correctly:
    - **<%windir%>\\Microsoft .NET\\Framework\\v1.1.4322\\Temporary ASP.NET Files**
    
    - **<%windir%>\\Temp**

**Registry**

The following permissions are set for the Registry during WSUS setup.

- The **Users** group must have Read access to the **HKLM\\SOFTWARE\\Microsoft\\Update Services\\Server** Registry key.

- The following accounts must have Full Control permissions to the **HKLM\\SOFTWARE\\Microsoft\\Update Services\\Server\\Setup** Registry key:
    - **ASP.NET**
    
    - **Network Service** (for Windows Server 2003)
    
    - **WSUS Administrators**
