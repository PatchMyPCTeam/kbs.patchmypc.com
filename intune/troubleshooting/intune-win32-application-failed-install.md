---
title: "Intune Win32 Application Failed to install: Powershell script is failed to execute"
date: 2020-11-19
taxonomy:
    products:
        - 
    tech-stack:
        - intune
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - troubleshooting
        - deployments
        - common-issues-and-error-codes
---

Intune applications created by Patch My PC may give the error **Failed to install** in some scenarios if the code-signing certificate used to sign the PowerShell detection method script isn't trusted on the client device.

## Determine if You are Affected

If affected, you will see the error **Failed to install** in Company Portal if the application assignment is available. In some cases, the application may actually be installed successfully, as shown below.

![Intune Application Failed to Install Powershell script is failed to execute](/_images/Intune-Application-Failed-to-Install-Powershell-script-is-failed-to-execute.png "Intune Application Failed to Install Powershell script is failed to execute")

If you review the **[Intune Management Extention](https://docs.microsoft.com/en-us/mem/intune/apps/apps-win32-app-management#prerequisites)** log file **[AgentExecutor.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#application-troubleshooting-client-logs)**, you will see errors similar to below:

Powershell script is failed to execute  
write output done. output = , error = File C:\\Program Files (x86)\\Microsoft Intune Management ExtensionContentDetectionScripts.ps1 cannot be loaded. A certificate chain processed, but terminated in a root certificate which is not trusted by the trust provider. + CategoryInfo : SecurityError: (:) , ParentContainsErrorRecordException + FullyQualifiedErrorId : UnauthorizedAccess

This error results from having the option **[Digitally sign the detection method script and enforce signature checking on the application in Intune](/intune-application-creation-options#topic1)** enabled and **not deploying** the certificate to the **Trusted Root** and **Trusted Publishers** certificate store on the client.

![Intune Application Options](/_images/intune-application-options-in-patchmypc.png "Intune Application Options")

## Resolution: Deploy the Code-Signing Certificate to Client Devices

You have two options to deploy the code signing certificate with Intune:

1. Using a [profile with custom settings in Intune](https://docs.microsoft.com/en-gb/mem/intune/configuration/custom-settings-configure) and an OMA-URI to apply a setting from the RootCATrustedCertificates CSP.

3. Using a [PowerScript in Intune](https://docs.microsoft.com/en-us/mem/intune/apps/intune-management-extension), which itself will be unsigned and contain the code signing certificate used to install it on target machines.

To read more about option 1, using a profile with custom settings in Intune, see this Microsoft TechCommunity article by Jason Sandys: **[Adding a Certificate to Trusted Publishers using Intune](https://techcommunity.microsoft.com/t5/intune-customer-success/adding-a-certificate-to-trusted-publishers-using-intune/ba-p/1974488)**.

For options 2, using a PowerShell script deployed in Intune, contiune reading. Below is a step by step showing how an Intune Script can be created using **[the script attached here](/app/uploads/2025/06/Script_Register-CodeSigningCertificate.zip)**. Additionally, there are steps to help gather the required information.

- [Gathering Info](#GatherInfo)

- [Step 1: Create a New Script](#CreateScript)

- [Step 2: Assign the Script](#AssignScript)

### Gathering Info

 The script itself will need to be edited with your specific Thumbprint and Base64 encoded certificate string. The function contained within the script discusses how to gather these values, but I’ll also note them below. Also, if you are comfortable with Configuration Items within Configuration Manager, you can simply grab the script, adjust for your environment, and deploy away!

- **Certificate Thumbprint****:**
    - Within the properties of your Code Signing certificate, go to the Details tab and scroll to down to the thumbnail area. This thumbprint will help us uniquely identify the certificate.
        -  
            ![certificate properties, details, thumbprint field](/_images/cert-details.png "certificate properties, details, thumbprint field")
            
    
    - This can also be copied out of the Patch My PC Publisher, as shown below.
        - ![General Tab, Show Certificate, Thumbprint field](/_images/publisher-show-certificate.png "General Tab, Show Certificate, Thumbprint field")
            

- **Base64 Encoded Certificate String****:** This can be acquired a handful of ways, but a simple way is to get a copy of the certificate as a file and run a PowerShell command similar to the below
    - Set-Clipboard -Value (::ToBase64String((Get-Content -Path .ExportedCert.cer -Encoding Byte)))
        

### Step 1: Create a New Script

Make a local copy of **[the script attached here](/app/uploads/2025/06/Script_Register-CodeSigningCertificate.zip)** and ensure you edit the script based on the information you gathered above. We will later upload this .ps1 file to Intune.

Navigate to [Intune](https://endpoint.microsoft.com) and locate the **Scripts** node under **Devices**.

![Intune  Devices  Scripts](/_images/IntuneDeviceScripts.png "Intune  Devices  Scripts")

Within this node, you can create **Add** a new **Windows 10** script.

![Add Script - Windows 10](/_images/IntuneDeviceScriptsAddScript.png "Add Script - Windows 10")

Basic information can be input for the Name and Description.

![](/_images/IntuneDeviceScriptsNameDescription.png)

We now will make sure our local copy of the [script](/app/uploads/2025/06/Script_Register-CodeSigningCertificate.zip) has been **edited for our environment**.

1. Ensure **$Remediate = $true**

3. Set **$CodeSigningCertificateThumbprint** to your thumbprint gathered earlier.

5. Set **$EncodedCertString** to the Base64 string representation of your code signing certificate gathered earlier.

7. Save the script!

![](/_images/CodeSigningEditScript.png)

With our script updated and saved we can:

1. **Upload it to Intune**

3. Ensure we set **'Run script in 64 bit PowerShell Host'**

![](/_images/CodeSigningScriptSettings.png)

> **Note:** It is important that you **do not 'Enforce script signature check'** on this script. We are deploying this script to ensure we can enforce signature checking on other scripts.

### Step 2: Assign the Script

With all the above steps complete we can assign our script! The target of the assignment is up to you, but in our case, we will target all devices.

![Assign To All Devices](/_images/CodeSigningAssignScript.png "Assign To All Devices")