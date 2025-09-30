---
title: "How to generate a code signing certificate and sign PowerShell .ps1 script"
date: 2022-06-10
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
        - security-and-certificates
        - education
        - best-practices
---

This guide will show you how to issue a code signing certificate from your internal Certificate Authority, and how to use it to sign your code.

## Trying to sign code with no certificate available

If you would like to sign your own files, and this is done on a machine where no code signing certificate is present, you will get an error.

1. If you are using [SignTool](https://docs.microsoft.com/en-us/windows/win32/seccrypto/signtool) to automatically select the best signing certificate, the verbose output will say:  
    SignTool Error: No certificates were found that met all the given criteria.  
      
    ![](/_images/01-codesign.png)
    

## How to get a code signing certificate

One option would be to purchase a code signing certificate online from authorities such as [DigiCert](https://www.digicert.com/signing/code-signing-certificates).

Another one would be to issue one from your internal Certificate Authority, which you can do by following these steps:

**Create and issue a signing certificate**

1. Open **Certification Authority** (**certsrv.msc**) on a machine where you have installed the certification authority.

3. Expand the name of the **Certification Authority**, then right-click on **Certificate Templates** and choose **Manage**  
    ![](/_images/03-codesign.png)
    

5. Right-Click on **Code Signing** under the **Template Display Name** column and choose **Duplicate template**.  
    ![](/_images/04-codesign.png)
    

7. On the properties of the new template, click on the **General** tab and give it the name you want. Also, choose the validity period. Save change with **Apply**.  
    ![](/_images/05-codesign.png)
    

9. Go on the **Request Handling** tab, and make sure **Allow private key to be exported** is enabled.  
    ![](/_images/06-codesign.png)
    

11. On the **Subject Name** tab, set the **Subject name format** to **Common Name**.  
     ![](/_images/07-codesign.png)
     

13. On the **Extensions** tab, make sure that the description of **Key Usage** contains **Digital Signature**.  
     ![](/_images/08-codesign.png)
     

15. On the Security tab, ensure that “**Authenticated Users**” have **Read** and **Enroll** permissions.  
     ![](/_images/09-codesign.png)
     

> **Note:** You might not want all Authenticated Users to be able to Enroll the certificate, you can add your own custom security group which should have these permissions.

9. You can now click OK and then close the Certificate Templates Console.

11. Back to **Certification Authority**, right click **Certificate Templates**, choose **New** and then select **Certificate Template to Issue**.
     ![](/_images/10-codesign.png)
     

13. From the **Enable Certificate Templates list**, select your certificate template and confirm with **OK**.  
     ![](/_images/11-codesign.png)
     

## Requesting a certificate on a machine to use it for code signing

1. On a domain joined machine, open **mmc.exe**.

3. Click **File** and then **Add/Remove Snap-in**.

5. Select **Certificates** and then click on **Add**.

7. In the dialog box which appears, select **My user account**, then confirm with **Finish**.

9. In the console, expand **Certificates – Current User**, then expand **Personal**.

11. Right-click **Certificates**, then go to **All Tasks** and select **Request New Certificate**.  
     ![](/_images/12-codesign.png)
     

13. In the **Certificate Enrollment** window, click **Next** until you see a list of certificates to request. The issues certificate should appear in this list. Select it and confirm with **Enroll**.  
     ![](/_images/13-codesign.png)
     

15. The certificate should now be enrolled on that device and can be used for code signing.

## How to sign your code

In terms of signing, you can either use [SignTool](https://docs.microsoft.com/en-us/windows/win32/seccrypto/signtool) or the [Set-AuthenticodeSignature](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-authenticodesignature) PowerShell cmdlet.

**Example 1**: SignTool

![](/_images/14-codesign.png)

In this example, we used the following arguments:

- sign = Digitally signs files. Digital signatures protect files from tampering, and enable users to verify the signer based on a signing certificate.

- /a = Automatically selects the best signing certificate. Sign Tool will find all valid certificates that satisfy all specified conditions and select the one that is valid for the longest time. In our case, it automatically selected the code signing certificate we enrolled.

- /v = Displays verbose output

- /fd = specifies the digest algorithm. In our case, we went with SHA256.

**Example 2**: PowerShell cmdlet [Set-AuthenticodeSignature](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-authenticodesignature)

![](/_images/15-codesign.png)

In this example, on the server where the Patch My PC Publishing service is installed, from an elevated PowerShell ISE instance:

- On line 1, we get the code signing certificate we enrolled.

- On line 3, we used the Set-AuthenticodeSignature cmdlet to sign our **C:test.ps1** file using the WSUS code signing certificate.