---
title: "Pass Variables into Pre and Post Scripts"
date: 2022-09-27
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

This article covers the feature to pass variables into pre and post scripts using Patch My PC.

## Topic 1: Pass Variables into Pre and Post Scripts

Starting with the Patch My PC Publisher Preview Release 2.1.6.35 and Production Release 2.1.7.0, variables are now able to be passed into pre/post scripts defined in the [Add custom pre/post scripts](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#custom-scripts) feature. **These variables can be passed into the Pre/Post scripts by including them in the arguments list of the Pre/Post script.**

 **![Example of creating a post script with variables.](/_images/PostScriptwithVariables.png "Example of creating a post script with variables.") Example of creating a post script with variables.**

With release 2.1.6.35 and 2.1.7.0 the following variables can be passed into Pre/Post Scripts defined in Patch My PC:

- Vendor Name
    - The name of the Vendor as defined in Patch My PC [(in Base64)](#topic2) below

- Product Name
    - The name of the Product as defined in Patch My PC [(in Base64)](#topic2) below

- Version
    - The version of the application or update being installed

- PackageID
    - The ConfigMgr PackageID

- Return Code (Post Script Only)
    - The return code of the main installer process

## Topic 2: Using the Product and Vendor names in Pre/Post Scripts

Patch My PC passes the Product and Vendor names to scripts in Base64 to avoid issues with escape characters and long names. Therefore to use the Product or Vendor names in post scripts, they will need to be converted back to a human readable string. The following PowerShell script gives an example of a script that can convert the Pre/Post script variables to a human readable string.

> ```
> param (  $VendorName,  $ProductName,  $Version,  $PackageID,  $ReturnCode)$Vendor = [System.Text.Encoding]::UTF8.Getstring([System.Convert]::FromBase64String($VendorName))Write-Output "Vendor: $Vendor" > C:\out.txt$Product = [System.Text.Encoding]::UTF8.Getstring([System.Convert]::FromBase64String($ProductName))Write-Output "Product: $Product" >> C:\out.txtWrite-Output "Version: $Version" >> C:\out.txtWrite-Output "Package ID: $PackageID" >> C:\out.txtWrite-Output "Return Code: $ReturnCode" >> C:\out.txt
> ```