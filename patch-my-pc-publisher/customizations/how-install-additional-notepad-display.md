---
title: "How to install additional Notepad++ display languages with Patch My PC"
date: 2024-08-23
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

If you want to deploy Notepad++ in your organization on your managed devices,and would like to also install additional Display Languages during the unattended installation, follow this guide.

At the time of writing this article, the Notepad++ installer does not support any parameters that will allow you to install additional languages.

1\. Download the Notepad++ installer and run the installation manually.

2\. Click through the wizard until you get to the **Components** section.

3\. Select the languages you would like to deploy:

![Notepad++ components](/_images/notepadplusplus1.png "Notepad++ components")

In today's example, we chose English (included by default), German, and French.

4\. Proceed with the installation.

5\. Once you installed the app, go to the installation directory.

In my case, I installed the 64-bit version, so I went to: "_C:\\Program FilesNotepad++localization_".

6\. Copy these files to your desktop, they will be used later.

7\. Save the script lines below as a .ps1 file.

```
## specify the language XML files you will copy using the script (the ones on your desktop)
[array]$myNotepadPlusPlusLangs = @('english.xml','german.xml','french.xml')
[string]$notepadInstallDir = "$env:ProgramFiles\Notepad++\localization"

## If using Notepad++ 32-bit, use this instead:
# $notepadInstallDir = "$env:ProgramFiles(x86)\Notepad++\localization"

#######################################################
## DO NOT EDIT THE SCRIPT FROM HERE ON OUT
#######################################################
Foreach ($lang in $myNotepadPlusPlusLangs){
[string]$xmlLangPath = "$PSScriptRoot\$lang"
#Write-Host $xmlLangPath
If(Test-Path $xmlLangPath){
Copy-Item $xmlLangPath $notepadInstallDir
}
}
```

> **Note:**
> 
> LEGAL DISCLAIMER
> 
> The PowerShell script provided is shared with the community as-is  
> The author and co-author(s) make no warranties or guarantees regarding its functionality, reliability, or suitability for any specific purpose  
> Please note that the script may need to be modified or adapted to fit your specific environment or requirements  
> It is recommended to thoroughly test the script in a non-production environment before using it in a live or critical system  
> The author and co-author(s) cannot be held responsible for any damages, losses, or adverse effects that may arise from the use of this script  
> You assume all risks and responsibilities associated with its usage

8\. Edit row in the script and give the name of the XLS files you have on your Desktop.

![](/_images/notepadplusplus3.png)

9\. Copy the ps1 script as well as the XML files on the server hosting the PMPC Publisher Console and save them in a location they won't be accidentally deleted.

10\. Open the PMPC Publisher Console, right-click on Notepad++ (64-bit), and choose the "[Add custom pre/post scripts](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#custom-scripts)" right-click option.

11\. Specify the ps1 file as a post-install script, as well as your files, as shown in the screenshot:

![](/_images/notepadplusplus4.png)

12\. Run a sync to publish Notepad++ (64-bit). Make sure to test before a global rollout.

> **Note:** If the latest version of Notepad++ is already published through the Patch My PC Publisher, you will have to republish it with the updated configuration

> **Note:** By following this guid, you will only install the DisplayLanguages. Users then be able to choose the Display (Localization) language from the Notepad++ app –> Settings menu –> Preferences.
> 
> This specific setting is specific to each user profile and can be set by creating/modifying this XML: %appdata%\\Notepad++\\nativelangs.xml. More info [here](https://npp-user-manual.org/docs/preferences/#general).
> 
> Given ConfigMgr / Intune runs the installation as the SYSTEM account, if you aim to modify the file with a script, you will have to create one that takes into account all user profiles.