---
title: "Customize Content Download and Log Save Location"
date: 2020-05-12
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

In this article, we are to review the option c**ustomize content download and log save location** in the Advanced tab of the publisher.

This feature was requested on our UserVoice portal [Option to Change Default Temporary Download from %WinDir%temp to a Custom Folder](https://ideas.patchmypc.com/ideas/PATCHMYPC-I-519) 

## Use a Custom Folder for Temporary Downloads

By default, content files are downloaded temporarily to **%temp%** during publishing operations. Since the service runs under system context, **%temp%** generally will translate to **C:\\Windows\\Temp**. After the publishing operations are completed, these files will be automatically deleted.

Optionally, you can define a custom folder in the **Advanced tab** for where these content files are downloaded.

![](/_images/custom-download-folder-publisher-settings.png)

When this setting is configured, the temporary content downloads will take place in the random subfolder of the folder defined. 

![](/_images/custom-folder-download-location.png)

> **Use Case:** The option to define a custom folder for temporary content downloads can be helpful in the following situations.
> 
> - You need to make anti-virus exclusions and prefer not to exclude the **C:\\Windows\\Temp** directory. Instead, the defined folder can be excluded.
> 
> - Low disk space concerns on the system drive

## Use a Custom Folder for the PatchMyPC.log

By default, the PatchMyPC.log file is stored in the **%InstallDir%** of the Publisher. This configuration is generally recommended, but there may be some cases where you may want to define a custom folder for the log file for collection and monitoring reasons.

If you set a custom folder, the computer account of the device running the publisher will need permissions to write to the folder.