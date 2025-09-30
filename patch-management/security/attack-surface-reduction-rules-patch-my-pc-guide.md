---
title: "A guide to using Attack Surface Reduction Rules (ASR) with Patch My PC"
date: 2025-09-01
taxonomy:
    products:
        - patch-management
    tech-stack:
        - 
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - security
        - best-practices
        - education
---

Microsoft Defender’s **Attack Surface Reduction (ASR) rules** are designed to limit the actions of applications and reduce the overall attack surface on Windows devices. While these rules are effective at blocking common attack techniques, they can sometimes interfere with legitimate enterprise tools that must operate across both the **SYSTEM** and **USER** contexts.

One ASR rule we frequently encounter in customer environments is **Block credential stealing from the Windows local security authority subsystem (lsass.exe)**

This rule prevents processes from accessing the memory of lsass.exe, blocking a common credential theft method.

## Why does this affect Patch My PC?

Most applications and updates are installed in the **SYSTEM context**, but some actions must also be performed in the **USER context** to patch applications successfully. For example:-

- **Process enumeration**  
    PatchMyPC-ScriptRunner.exe will enumerate running processes for all users to detect if any conflicting applications need to be closed before an update can proceed. These processes often run in the user context and may even be active for multiple users simultaneously.

- **User notifications**  
    To display the [Manage Conflicting Processes](https://patchmypc.com/kb/manage-conflicting-processes-when-updating/) pop-up in the user’s notification area, Patch My PC must temporarily switch from SYSTEM to USER context. When doing so, PatchMyPC-ScriptRunner.exe calls the CreateProcessAsUser function from Advapi32.dll, a common and legitimate approach for creating processes under the current user’s token.

These actions, while legitimate, can trigger the LSASS ASR rule because they involve obtaining a handle to the user’s primary token. To be clear **Patch My PC executables are not dumping the contents of LSASS**, as the ASR rule description might suggest. Instead, they are performing controlled operations required to install updates reliably.

Because Microsoft security baselines often enforce this ASR rule in _block mode_ by default, you may need to configure exceptions to ensure Patch My PC functionality is not disrupted.

While **PatchMyPC-ScriptRunner.exe** is the primary process you’ll see interacting with this rule, there are two other executables that can be found in the installer cache location:-

- `PatchMyPC-ScriptRunner.exe`

- `PatchMyPC-UINotification.exe`

- `PatchMyPC-PreventStart.exe`

These executables are all **code-signed by Patch My PC**, so you can trust their integrity.

## Exclusion Options

Unfortunately, this ASR rule only supports **path or folder exclusions**, publisher or certificate-based exclusions are not available. Depending on how strict you want to be with path matching, there are 3 levels of exclusions you can configure. The right option depends on your organization’s security posture and operational tolerance. Most customers will find the stricter option offers the best balance.

Additionally, per [https://learn.microsoft.com/en-us/defender-endpoint/configure-extension-file-exclusions-microsoft-defender-antivirus#use-wildcards-in-the-file-name-and-folder-path-or-extension-exclusion-lists](https://learn.microsoft.com/en-us/defender-endpoint/configure-extension-file-exclusions-microsoft-defender-antivirus#use-wildcards-in-the-file-name-and-folder-path-or-extension-exclusion-lists) we can use wildcards to simplify the rule for both the cache location and executable name. This is useful as both the ccmcache directory and IMECache directory have the same 3-level depth. For example:-

- C:\\Windows\\ccmcache\\3y\\PatchMyPC-ScriptRunner.exe

- C:\\Windows\\IMECache\\52dfd2fc-2384-4757-af76-744a7c458f13\_1\\PatchMyPC-ScriptRunner.exe

![folder depth](images/image.png)

**Note:** An asterisk `*` in a folder exclusion stands in place for a single folder. Use multiple instances of `\*\` to indicate multiple nested folders with unspecified names.

### 1\. Relaxed (simplest, easiest to maintain)

Covers any Patch My PC executable across the Windows Update, Configuration Manager and Intune cache locations in a few lines.

```
C:\Windows\*\*\PatchMyPC-*.exe
C:\Windows\SoftwareDistribution\Download\Install\PatchMyPC-*.exe
```

### 2\. Stricter (explicit, one line per EXE)

More explicit, listing the Patch My PC executable by name.

```
C:\Windows\*\*\PatchMyPC-ScriptRunner.exe
C:\Windows\SoftwareDistribution\Download\Install\PatchMyPC-ScriptRunner.exe
```

### 3\. Very Strict (explicit per cache location and EXE)

The most explicit option, defining the Patch My PC executable by name **and** cache location

```
C:\Windows\ccmcache\*\PatchMyPC-ScriptRunner.exe
C:\Windows\IMECache\*\PatchMyPC-ScriptRunner.exe
C:\Windows\SoftwareDistribution\Download\Install\PatchMyPC-ScriptRunner.exe
```

## Further Reading

- [https://learn.microsoft.com/en-us/defender-endpoint/configure-extension-file-exclusions-microsoft-defender-antivirus#use-wildcards-in-the-file-name-and-folder-path-or-extension-exclusion-lists](https://learn.microsoft.com/en-us/defender-endpoint/configure-extension-file-exclusions-microsoft-defender-antivirus#use-wildcards-in-the-file-name-and-folder-path-or-extension-exclusion-lists)

- [https://learn.microsoft.com/en-us/defender-endpoint/configure-extension-file-exclusions-microsoft-defender-antivirus](https://learn.microsoft.com/en-us/defender-endpoint/configure-extension-file-exclusions-microsoft-defender-antivirus)

- [https://patchmypc.com/kb/manage-conflicting-processes-when-updating](https://patchmypc.com/kb/manage-conflicting-processes-when-updating)

- [https://patchmypc.com/kb/patch-my-pc-recommended-antivirus/](https://patchmypc.com/kb/patch-my-pc-recommended-antivirus/)

- [https://patchtuesday.com/blog/tech-blog/windows-defender-exploit-guard-breaks-google-chrome/](https://patchtuesday.com/blog/tech-blog/windows-defender-exploit-guard-breaks-google-chrome/)
