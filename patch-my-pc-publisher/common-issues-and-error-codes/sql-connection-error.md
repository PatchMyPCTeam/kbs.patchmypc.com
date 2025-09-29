---
title: "A network-related or instance-specific error occurred while establishing a connection to sql server."
date: 2020-09-15
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

The error **a network-related or instance-specific error occurred while establishing a connection to sql server** has been an error we have been tracking via our **[proactive customer care program](/proactive-customer-care)**.

## Determine if You are Affected

When affected, you will see an error that starts with the error below in the **[PatchMyPC.log](40 - Could not open a connection to SQL Server\))**.

A network-related or instance-specific error occurred while establishing a connection to SQL Server.

> **Note:** The will likely be another sentence or two in the log error line that isn't always unique. Each of these errors will have similar troubleshooting steps. A few examples we have seen are:
> 
> - the server was not found or was not accessible. verify that the instance name is correct and that sql server is configured to allow remote connections. (provider: named pipes provider, error: 40 - could not open a connection to sql server)
> 
> - the server was not found or was not accessible. verify that the instance name is correct and that sql server is configured to allow remote connections. (provider: sql network interfaces, error: 26 - error locating server/instance specified)
> 
> - the server was not found or was not accessible. verify that the instance name is correct and that sql server is configured to allow remote connections. (provider: tcp provider, error: 0 - the remote computer refused the network connection.)
> 
> - the server was not found or was not accessible. verify that the instance name is correct and that sql server is configured to allow remote connections. (provider: tcp provider, error: 0 - the wait operation timed out.)

## Troubleshooting Step 1: Is the MSSQLSERVER Service Running?

The first step is to validate the **MSSQLSERVER** service is **running** on the SQL Server hosting the WSUS database. In our example image below, we can see it's in a **stopped** state. If you are using a **WID (Windows Internal Database)** you can perform the same check on your WSUS Server where the WID is installed. 

![MSSQLSERVER in stopped state on WSUS Server](/_images/MSSQLSERVER-in-stopped-state-on-WSUS-Server.png "MSSQLSERVER in stopped state on WSUS Server")

![](/_images/WID.png)

## Troubleshooting Step 2: Is the SQL Server DNS Name Resolvable?

You can receive the **SQL Server DNS name** from the WSUS server registry with the following **RegValue**: HKEY\_LOCAL\_MACHINE\\Software\\Microsoft\\Update Services\\Server**\\Setup:SqlServerName**

In our example below, we can see the WSUS server is **unable to resolve the DNS name of the SQL Server** defined for the database server.

![WSUS SqlerverName not resolvable DNS](/_images/WSUS-SqlerverName-not-resolvable-DNS.png "WSUS SqlerverName not resolvable DNS")

## Troubleshooting Step 3: Validate SQL Permissions for WSUS Computer Account

The next step if to connect to SQL Server using **SQL Server Management Studio**. Check the **SUSDB** > **Security** > **Users** and ensure the computer account of the **WSUS server exist** and it's been assigned the **WebService** database role membership.

![Validate WSUS Server has ebService SQL Membership Role](/_images/Validate-WSUS-Server-has-ebService-SQL-Membership-Role.png "Validate WSUS Server has ebService SQL Membership Role")

## Troubleshooting Step 4: Check if SQL Ports are Blocked

If all the steps above are correct, the next thing to check is **firewalls may be blocking the SQL port** used to connect to the SQL Server from the WSUS Server.

You can download use the Microsoft tool **[PortQryUI](https://www.microsoft.com/en-us/download/details.aspx?id=24009)** to run a port query check.

On the **WSUS server**, open **PortQryUI** and enter the **SQL Server hostname/IP** and add the **SQL server port number** (default is **1433**) and check the result.

![SQL Server Port Filtered from WSUS Server](/_images/SQL-Server-Port-Filtered-from-WSUS-Server.png "SQL Server Port Filtered from WSUS Server")