---
title: "How to Resolve Download Errors During PatchMyPC SCUP Catalog Download\_- Exception Message: The request was aborted: Could not create SSL/TLS secure channel\_-2146233079"
date: 2020-05-30T00:00:00.000Z
taxonomy:
  products:
    - null
  tech-stack:
    - configmgr
  solution:
    - null
  post_tag:
    - null
  sub-solutions:
    - connectivity-and-proxy-issues
    - troubleshooting
    - security-and-certificates
---

# How To Resolve Download Errors During Patchmypc Scup Catalog Download Exception Message The Request Was Aborted Could Not Create Ssl Tls Secure Channel 2146233079

In this article, we will look at the cause and resolution for error **-2146233079** when trying to import our catalog into **System Center Update Publisher**.

### Issue: SCUP Uses TLS1.0, Not TLS1.1/TLS1.2 by Default

Currently, the System Center Update Publisher tool has native support for TLS1.0. It is a .NET Application which targets .NET Framework v4.5. Any connections which require a newer version of TLS, such as to the PatchMyPC.com website, will not connect successfully. The PatchMyPC.com website currently allows TLS1.1 and TLS1.2 connections.

This specific error will appear in the UpdatesPublisher.log file for SCUP as shown below.

Download file: 'https://patchmypc.com/scupcatalog/apis/subscriber\_download.php?id=12345678901234567890' failed.\
\==================== Exception Detail Start =======================\
Exception type: WebException\
Exception HRESULT: -2146233079\
Exception Message: The request was aborted: Could not create SSL/TLS secure channel.\
Exception source UpdatesPublisher.BaseServices\
Exception TargetSite Void MoveNext()\
EStack: at Microsoft.UpdatesPublisher.BaseServices.WebClientWrapper.d\_\_17.MoveNext()\
— End of stack trace from previous location where exception was thrown —\
at System.Runtime.ExceptionServices.ExceptionDispatchInfo.Throw()\
at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)\
at Microsoft.UpdatesPublisher.BaseServices.FileDownloader.d\_\_11.MoveNext()\
\===================== Exception Detail End ========================

### Resolution 1: Configure .NET to 'Use Strong Crypto' (Preferred)

* Perform the registry edits which are detailed in the [Microsoft docs](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls#configuring-security-via-the-windows-registry) regarding TLS support for applications targeting older versions of .NET Framework
* Specifically, SCUP is a 64-bit application which targets .NET Framework v4.5. For this scenario, the below registry edits are needed on the server which has SCUP installed
  * HKEY\_LOCAL\_MACHINE\Software\Microsoft.NET\Framework\v4.0.30319
    * "SystemDefaultTlsVersions"=dword:00000001&#x20;
    * "SchUseStrongCrypto"=dword:00000001

A couple of lines of PowerShell can be used to set these values in the registry as well. The code is below.

Set-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\\.NETFramework\v4.0.30319 -Name SystemDefaultTlsVersions -Value 1 -Type DWord\
Set-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\\.NETFramework\v4.0.30319 -Name SchUseStrongCrypto -Value 1 -Type DWord

### Resolution 2: Switch the Catalog to HTTP:// (Alternative)

* Change your SCUP Catalog URL to **HTTP**://... instead of **HTTPS**://...
  * If you request the catalog over HTTP, you are no longer using TLS of any form, and the catalog will download successfully.
