---
title: "ConfigApi is not responding. An unhandled exception occurred"
date: 2024-10-10
taxonomy:
    products:
        - patch-my-pc-publisher
    tech-stack:
        - configmgr
    solution:
        - 
    post_tag:
        - configapi
    sub-solutions:
        - troublsheshooting
        - common-issues-and-error-codes 
        - connectivity-and-proxy-issues
---

In this article we troubleshoot why this error might occur when launching the Patch My PC Publisher UI and how to resolve it.

## Determine if You are Affected

When you launch the Publisher, you might receive the following error.

The PatchMyPCService is running, but the ConfigApi is not responding. Cloud features will not be available. Please see PatchMyPC.log for more information

![](/_images/ConfigApi_Error.png)

When you click OK, you get a further warning.

An unhandled exception occurred: :9001\]

![](/_images/ConfigApi_Error_UnhandledException.png)

Both of these errors can be found in the %ProgramFiles%Patch My PCPatch My PC Publishing ServicePatchMyPC.log

![](/_images/ConfigApi_Error_Log-1.png)

Run the following command to verify you are missing a configuration setting on the host which means the Publisher cannot bind to the ConfigApi on the loopback adapter.

**netsh http show iplisten**

![](/_images/ConfigApi_Error_netsh.png)

If 127.0.0.1 is not present in the list, this will cause the error listed in this article.

## Workaround

The ConfigApi is bound to the loopback adapter address 127.0.0.1 or the DNS name LOCALHOST. A legacy misconfiguration can result in the IP Listener configuration for 127.0.0.1 being removed from the host.

Run the following command to add 127.0.0.1 back as an IP Listener.

**netsh http add iplisten 127.0.0.1**

Verify the addition of 127.0.0.1 was successful by running the following command.

**netsh http show iplisten**

![](/_images/ConfigApi_Error_netshFix.png)
