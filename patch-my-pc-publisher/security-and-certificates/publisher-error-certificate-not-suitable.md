---
title: "Publisher Error: Certificate is not Suitable for Code Signing"
date: 2021-04-29
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

In certain situations, the Publisher may throw an error during the publishing of Apps or Updates when signing a PowerShell script fails. Patch My PC Publisher version 2.0.2.0 introduced this error, before the 2.0.2.0 release, a failure to sign PowerShell scripts would still result in a successfully published application, however the script included with the application (pre/post script or detection method script) would not be code signed. 

## Determine if You are Affected

This error will occur when the Publisher attempts to code sign a PowerShell script, but the Code Signing certificate used was created incorrectly. When this issue occurs you may see one of the following errors in the PatchMyPC.log:

- An error occurred while signing the PowerShell detection script, but there is no error message to display. CertManager  
    An error occurred while copying Pre/Post Script: Error creating detection method PowerShell script because code-signing is enabled, but there is no code-signing certificate available. Please review the following article for a resolution https://patchmypc.com/ui-doc-no-signing-certificate-for-intune \[\[System.Exception\]\]

- An error occurred while signing the file: Cannot sign code. The specified certificate is not suitable for code signing. \[\[System.Management.Automation.PSArgumentException\]\]. Please see kb: [https://patchmypc.com/ui-doc-certificate-not-suitable-for-code-signing](https://patchmypc.com/ui-doc-certificate-not-suitable-for-code-signing)  
    An error occurred while copying Pre/Post Script: while signing the file: Cannot sign code. The specified certificate is not suitable for code signing. \[\[PatchMyPC\_Certificate.AuthenticodeSignatureException\]\]

To confirm if you are affected by this issue, the following steps may be performed

1. On the system with the Patch My PC Publisher installed, run certlm.msc

3. Navigate to the WSUS -> Certificates store

5. Double-Click the Code Signing Certificate

7. Navigate to the "Details" tab

9. Under the "Details" tab, the following Properties should exist:
    - Enhanced Key Usage - Code Signing
    
    - Key Usage - Digital Signature

11. Click "Edit Properties..." ensure that Code Signing is listed and enabled under the list of "Certificate purposes"  
     ![Image of Certificate and Certificate Properties window showing the proper properties for a code signing certificate](images/CodeSigningCertificateProperties.png)
     

 If any of the properties listed above do not exist or contain the values listed, the certificate cannot be used to sign PowerShell scripts

## Solution: Recreate Code Signing Certificate

The full solution for this error is to recreate the Code Signing Certificate that is used for signing updates, and republish updates using the new code signing certificate.

See the following KB articles for information on creating a new Code Signing Certificate and republishing updates:

- [What is the WSUS Signing Certificate and How to Create It](https://patchmypc.com/pki-certificate-for-third-party-update-code-signing-in-sccm)

- [When and How to Republish Patch My PC Third-Party Updates](https://patchmypc.com/when-and-how-to-republish-third-party-updates)

## Workaround: Disable Code-Signing in the Publisher and disable any Patch My PC provided Pre/Post Scripts

If recreating the Code Signing Certificate is not possible, the following steps will work around the issue until a new Code Signing Certificate can be created.

1. Open the Patch My PC Publisher

3. Navigate to ConfigMgr Apps tab

5. Click Options...

7. Deselect the option to "Code-sign the PowerShell detection method script using the WSUS Signing Certificate" and click "OK"  
    ![](../../_images/DisableCodeSigningPublisher.png)
    

9. Click Apply to apply the changes

The Publisher also attempts to code sign any Patch My PC provided scripts by default (custom pre/post scripts are not resigned), any application utilizing a Patch My PC provided script will need to be disabled and optionally converted to a custom pre/post script in order for publishing to complete successfully. The following process can be used on each of the effected apps to perform this conversion.

1. Open the Patch My PC Publisher

3. Navigate to an application with a Patch My PC defined script (Oracle Java is a popular example)

5. Right click on the Product and select "Patch My PC defined pre/post scripts"  
    ![](../../_images/DefinedPatchMyPCScript.png)
    

7. Select the Option to "Disable the Patch My PC recommended pre/post-update script for this product."  
    ![](../../_images/DisablePatchMyPCScript.png)
    

9. Click on View Script, this will open the provided script in a web browser

11. Save the script as a ".ps1" file in a location that the Publisher has read access, close the web browser  
     ![](../../_images/SaveDefinedScript.png)
     

13. Click OK in the "Patch My PC Defined Script(s)" window

15. Right click the Product in the Publisher again

17. Select "Add Custom Pre/Post Scripts"

19. Select "Browse..." next to the appropraite pre/post script option

21. Navigate and select the script that was just saved  
     ![](../../_images/UpdateCustomScript.png)
     

23. Click OK, Click Apply

25. The Update or Application should successfully publish on the next Publisher Sync
