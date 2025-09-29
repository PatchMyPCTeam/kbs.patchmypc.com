---
title: "Applications Fail in Task Sequence Error: AuthorizationManager check failed 0x87d00327"
date: 2019-08-08
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

Since releasing our new **[application creation and management feature](https://patchmypc.com/application-patch-management#base-installations)** a few weeks back, we have noticed some customers opening cases with regards to trouble getting the applications to install during an operating system deployment **task sequence**.

The most common reason applications may fail during a task sequence is the code-signing certificate isn't properly deployed ahead of the **Install Applications** task sequence step.

## A Little Background - Our Applications and PowerShell Detection Method

When our service creates applications in SCCM, we use a Powershell script for the detection method of the deployment type, as shown below.

![Detection Method Script SCCM](images/DetectionMethod-Script-SCCM-Applications.png)

We also code-sign the Powershell detection method script by default using the WSUS Signing certificate.

![Code-Signed Detection Method Scipt in SCCM PatchMyPC](images/Code-Signed-Detection-Method-Scipt-in-SCCM-PatchMyPC.png)

## Determine if You are Affected

When the device gets to the **install application step**, the device may fail to execute the detection method script if you are using a self-signed WSUS Signing certificate and the Powershell execution policy is **not ByPass**.

![Install Application Step SCCM OSD Task Sequence](images/Install-Application-Step-SCCM-OSD-Task-Sequence.png)

If the detection method script fails, it's because the WSUS Signing certificate used to code-sign it isn't trusted during the operating system deployment on the client-side. This trust failure is due to the fact that the device wouldn't have received **[group policy](https://patchmypc.com/scupcatalog/documentation/CertificateAndGPODeploymentGuide.pdf)** or the [client policy](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#enable-third-party-updates-on-the-sup) if you are deploying the certificate **[using SCCM](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#enable-third-party-updates-on-the-clients)** to add the certificate to the **Trusted Root** and **Trusted Publishers** certificate stores.

In the C:\\Windows\\CCM\\Logs\\**AppDiscovery.log**, you will also be able to see the failure when the SCCM agent tries to call the detection method script. It will contain lines similar to the image below.

![AppDiscoveryLog SCCM Error 0x87d00327](images/AppDiscoveryLog-SCCM-Error-0x87d00327.png)

## The Resolution - Install WSUS Signing Certificate as a Task Sequence Step

Now that we see the cause, the resolution is pretty straight forward. You need to ensure the WSUS Signing Certificate you are using for third-party update publishing that is also used for signing the scripts is trusted before the **install application** step in SCCM.

The first thing to do is **export your WSUS Signing Certificate** from the publishing service. In our example, we saved the file as **WSUSSigningCertificate.cer**

![Export WSUS Signing Certificate](images/Export-WSUS-Signing-Certficate.png)

Now that you have the certificate used for signing the detection method scripts, we need to import it on the client-side during the task sequence before the install applications step.

Luckily this process is pretty easy, and we'll show you how. First, you can download our script **[Import-Certificates.cmd](https://patchmypc.com/scupcatalog/downloads/scripts/Import-Certificates.zip)** that can be used for importing (.CER) files into the **Trusted Root** and **Trusted Publishers**.

You will want to save the **Import-Certificates.cmd** file in the same UNC folder as the exported (.CER) file. In our example, we used the folder path **\\\\sccm\\sources\\Scripts\\Install-Certificates**

![Install Certificate Source Content Structure](images/Install-Certificate-Source-Content-Structure.png)

The batch script is very straight forward. It will import **all** (.CER) certificates that are in the same folder as the script to the **Trusted Root** and **Trusted Publishers** certificate store. The REG ADD command **on line 5 is optional**, but it can be helpful if you want to also install third-party software updates using the Install Software Updates step in the same task sequence.

![Import Certificates Script Lines](images/Import-Certificates-Script-Lines.png)

Next, you need to create a legacy package in SCCM (No programs required if you only want to install the certificates via task sequence). If you're going to deploy this using software distribution to existing clients outside a task sequence, you can create a new program for the package the runs **Import-Certificates.cmd** as the **command line**.

When creating the package, use the UNC path that you saved your **(.CER) certificate** to as well as the **Import-Certificates.cmd**. In our example, we used the path mentioned above. This package is used for accessing the content in the task sequence.

![SCCM Package For Certificate Import TrustedRoot](images/SCCM-Package-For-Certificate-Import-TrustedRoot.png)

Lastly, you need to add a **Run Command Line** step before the **Install Application** step. Within this step, be sure to set the command line to **Import-Certificates.cmd** and enable the **Package** option and reference the package you created in the previous step.

![SCCM Run Command Line to Import Certificates](images/SCCM-Run-Command-Line-to-Import-Certificates.png)

**That's it!** Now that the certificate chain is trusted before the Install Applications step, you shouldn't receive the trust error when the detection method script is executed.

> **Note:** This process can also resolve the error **0x800b0109** during the **Install Software Updates** step of a task sequence because the default certificate deployment options such as **[group policy](https://patchmypc.com/how-to-deploy-the-wsus-signing-certificate-for-third-party-software-updates#topic2)** or [ConfigMgr client settings](/how-to-deploy-the-wsus-signing-certificate-for-third-party-software-updates#topic1) won't apply until after the task sequence is completed.

 **Search Terms:**

In-line script returned error output: & : AuthorizationManager check failed.  
Script Execution returned error message: & : AuthorizationManager check failed.  
Script Execution Returned :1, Error Message: & : AuthorizationManager check failed.  
CScriptHandler::DiscoverApp failed (0x87d00327).  
Deployment type detection failed with error 0x87d00327. 
Failed to perform detection of app deployment type , revision 1) for system. Error 0x87d00327  
Applications in OSD  
Install Applications OSD
