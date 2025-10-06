---
title: What is the WSUS Signing Certificate and How to Create It
date: 2020-05-10T00:00:00.000Z
taxonomy:
  products:
    - null
  tech-stack:
    - wsus
  solution:
    - null
  post_tag:
    - null
  sub-solutions:
    - security-and-certificates
    - education
    - best-practices
---

# What WSUS Signing Certificate How

In this article, we're going to break down what the **WSUS Signing Certificate** is with regard to third-party software updates, why you need one, and what you need to know when deciding what type of **code-signing certificate** is best for your organization.

#### Why Do I Need a Code-Signing Certificate?

In order to create third-party software updates in WSUS or ConfigMgr, they must be signed using a [code-signing certificate](https://en.wikipedia.org/wiki/Code_signing). Furthermore, your devices must trust the same certificate used to sign updates for the Windows Update service to install them.

Without a code-signing certificate, you cannot publish third-party software updates into WSUS or ConfigMgr.

By default, updates in WSUS are from Microsoft and client Windows devices inherently trust updates published by Microsoft. By using your own code-signing certificate, you enable your devices to successfully install third-party updates by trusting them.&#x20;

If your devices do not trust the code-signing certificate used to sign the updates in WSUS, they will not successfully install.

#### The Basics of the WSUS Signing Certificate

At the most basic level, the **WSUS Signing Certificate** is simply a **code-signing certificate**. The purpose of the certificate is to code-sign any content files for third-party updates during the publishing operation from the WSUS API.

Before any third-party software update is installed on a device, it will check to ensure the third-party update content was **signed using a certificate the device trusts**.

If you click the properties of a **CAB file** for a published update in the **WSUSContent** folder, you will be able to see the WSUS code-signing certificate that was used.

![Third-Party Software Update CAB file WSUS Signing Certificate](/_images/third-party-update-cab-file-code-signed-from-wsus-signing-certficate.png "Third-Party Software Update CAB file WSUS Signing Certificate")

If you open the CAB file, you will be able to see the **binaries from the original update** from the vendor. For example, we can see the **GoogleChromeStandaloneEnterprise64.msi** within the CAB file in the image below.

![](/_images/cab-file-extracted-for-third-party-software-update.png)

In the event the **CAB file was modified**, it would no longer be trusted on a device that attempts to install the third-party update.

The WSUS Signing Certificate can be viewed in the WSUS certificate store on the local machine using **certlm.msc**.

![](/_images/wsus-certificate-certlm-local-machine.png)

#### Should You Choose a Self-Signed or PKI Based WSUS Certificate?

We get this question a lot from our customers. Unfortunately, there is no one-size-fits-all answer to this because it depends. Each option has its pros and cons. We have discovered the most common option used by our [**customers**](../../customer-testimonials/) is self-signed.

Here's a list of some **Pros** and **Cons** for each option:

**Self-Signed WSUS Signing Certificate:**

* **Pros**
  * **It's free** to generate a self-signed code-signing certificate
  * **It's simple** and doesn't require a [PKI](https://en.wikipedia.org/wiki/Public_key_infrastructure)
  * It can be **created immediately**
  * Configuration Manager can [automatically deploy the certificate to clients](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#automatically-manage-the-wsus-signing-certificate)
* **Cons**
  * Self-signed certificates [can't be revoked](https://en.wikipedia.org/wiki/Self-signed_certificate#Security_issues) using a certificate revocation list if the private key is compromised. You would need to use a script via Configuration Manager or another method to remove the certificate from the Trusted Root certificate store if it was compromised.

**PKI Based WSUS Signing Certificate:**

* **Pros**
  * The certificate can be [revoked](https://en.wikipedia.org/wiki/Certificate_revocation_list) if compromised
  * You can issue a [code-signing certificate for WSUS](../../pki-certificate-for-third-party-update-code-signing-in-sccm/pki-certificate-for-third-party-update-code-signing-in-sccm/) from an internal PKI such as [Active Directory Certificate Services](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831740\(v=ws.11\)) for free
  * Configuration Manager can [automatically deploy the certificate to clients](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#automatically-manage-the-wsus-signing-certificate)
* **Cons**
  * A code-signing certificate needs to be issued from a [PKI](https://en.wikipedia.org/wiki/Public_key_infrastructure)
  * There will be a cost if you use a [public certificate authority](https://en.wikipedia.org/wiki/Certificate_authority#Providers) to issue the code-signing certificate

#### Options for Creating a Self-Signed WSUS Signing Certificate

If you decide to use a self-signed certificate, you have **three common ways** to **generate the self-signed certificate**.

* Patch My PC's [Publisher](../../publishing-service-setup-documentation/)
  * An easy option if you are using the Patch My PC Publisher
  * This option allows you to customize the expiration date, subject name, key length, and export the private key
  * You can create a self-signed certificate or import a PKI based certificate
* Configuration Manager Console ([**Automatically manage the WSUS Signing Certificate**](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#automatically-manage-the-wsus-signing-certificate))
  * A good option if you are using the ConfigMgr third-party software update catalog feature directly in the ConfigMgr console
  * It only supports creating a self-signed certificate
* System Center Updates Publisher ([SCUP](https://techcommunity.microsoft.com/t5/configuration-manager-archive/system-center-updates-publisher-signing-certificate-requirements/ba-p/272954))
  * If you are using legacy SCUP on our Basic subscription, this will be a good option.
  * You can create a self-signed certificate or import a PKI based certificate

&#x20;Each tool uses the same WSUS API when generating the self-signed WSUS signing certificate.

#### Creating the WSUS Signing Certificate Using Patch My PC's Publisher

After [installing our publisher](../../publishing-service-setup-documentation/), if there is no certificate detected, you can click **Generate a Self-Signed Certificate**.

![](/_images/generate-self-signed-certificate-patchmypc-publisher.png)

In the **Certificate Options** dialog, you have the ability to adjust the **subject name**, **validity**, and **key length**.

![](/_images/certificate-creation-options-wsus-signing-certificate.png)

Once the certificate is generated, you can click the **Show Certificate** button in our Publisher to view more details. You can also view the certificate in the WSUS store using **certlm.msc**.

![](/_images/wsus-signing-certificate-show-certificate-details.png)

#### Import a PKI Based Certificate Using Patch My PC's Publisher

If you decide to use a **PKI** based code-signing certificate, you have the ability to **import** the exported code-signing certificate (**PFX File**). We will not cover the process of **obtaining a PKI based code-signing certificate in this guide,** as that process can vary depending on the certificate authority you use.

We do have a separate article that will cover [**creating a certificate template and requesting the certificate using Active Directory Certificate Services**](../../pki-certificate-for-third-party-update-code-signing-in-sccm/). Once you have the PFX file for the PKI based code-signing certificate, you can click the button to **Import PFX Certificate**.

![](/_images/import-pfx-wsus-signing-certficate.png)

> Please be aware that if you use a PKI based code-signing certificate, the PFX file can only be imported using [**Patch My PC's Publisher**](../../publishing-service-setup-documentation/) or [System Center Updates Publisher](https://www.microsoft.com/en-us/download/details.aspx?id=55543). You can't import a PFX certificate directly in the Configuration Manager console, although there is a [UserVoice for this feature](https://configurationmanager.uservoice.com/forums/300492-ideas/suggestions/37129744-manage-pki-certificate-for-3rd-party-update-signin). Once the PKI certificate is imported, the option for configuration manager to [**Automatically manage the WSUS Signing certificate**](what-wsus-signing-certificate-how.md#configuration-manager-automatically-manage-the-wsus-signing-certificate) will still work and can be used to distribute that PKI based certificate to clients' **Trusted Root** and **Trusted Publishers** certificate stores automatically.&#x20;

#### Set Configuration Manager to Automatically Manage the WSUS Signing Certificate

If you enabled the option to **Automatically manage the WSUS Signing Certificate in the Configuration Manager console**, it will automatically generate a self-signed certificate in the event **one doesn't already exist**.

To enable this option, navigate to **Administration** > **Site Configuration** > **Sites** > **Right-click the site** > **Configure Site Components** > **Software Update Point**

![](/_images/configuration-manager-automatically-manage-the-wsus-signing-certficaite.png)

Note that there are two options here. The difference is explained below.

* **Configuration Manager manages the certificate**
  * If the SUP is remote from the Site Server, please review the [additional requirements when the SUP is remote](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#additional-requirements-when-the-sup-is-remote-from-the-top-level-site-server).
  * If the WSUS code-signing certificate is not found or has expired, then Configuration Manager **will generate a new certificate** and store it in the database.
  * If there is an existing WSUS code-signing certificate associated with WSUS, then it will be grabbed and stored in the database.&#x20;
  * Configuration Manager will automatically distribute this certificate to clients that have the client settings for 'Enable third party software updates' set to 'Yes'.
* **Manually manage the certificate**
  * If the WSUS code-signing certificate is not found or has expired, then Configuration Manager **will take NO action. A certificate must already exist as you have set it to be manually managed.**
  * If there is an existing WSUS code-signing certificate associated with WSUS, then it will be grabbed and stored in the database.&#x20;
  * Configuration Manager will automatically distribute this certificate to clients that have the client settings for 'Enable third party software updates' set to 'Yes'.

After enabling these options, **trigger a software update point synchronization** from the Configuration Manager console.

![](/_images/sync-sccm-software-update-point-for-declined-updates.png)

You can view the **wsyncmgr.log** on the site server. This log file will show the WSUS certificate **being created** or **imported** if one already existed.

![](/_images/wsyncmgr-creates-certificate-for-wsus.png)

> If the WSUS Signing Certificate **already existed** before setting the 'Enable third-party software updates' option, it will automatically **import the existing certificate,** and it will allow that certificate to be automatically deployed to client devices.
>
> This feature will be helpful if you want to use Patch My PC's Publisher to create the certificate and benefit from the **additional options** or **import a PKI based certificate.** Configuration Manager can still manage the certificate and automatically deploy it to devices rather than using a GPO.

After the sync, the certificate details should appear in the **Software Update Point Component Properties** > **Third Party Updates** tab

![](/_images/wsus-singing-certficaite-details-sccm-console.png)

You will want to ensure the client settings to **Enable third party software updates** is set to **Yes**. This should be enabled on the **Default Client Settings** as well as any **custom client device settings** targeted to devices that will install third-party software updates.

![](/_images/client-settings-enabled-third-party-software-updates-sccm.png)

When this client setting is enabled, the device will **automatically** install the WSUS Signing Certificate to the **Trusted Root** and **Trusted Publishers**. If the **third party updates tab is not configured on your Software Update Point** or the **client settings is set to No**, you will need to [deploy the WSUS Signing Certificate using group policy](../../scupcatalog/documentation/CertificateAndGPODeploymentGuide.pdf).

> If your software update point is **remote** from your **top-level site server** and the software update point is **not configured for SSL**, the option within Configuration Manager to **Automatically manage the WSUS Signing Certificate** will not work and you will receive an error in wsyncmgr.log [**Remote WSUS connection is not HTTPS. This prevents the software update point from getting the signing certificate for third-party updates**](../../remote-wsus-connection-is-not-https-this-prevents-software-update-point-from-getting-the-signing-certificate-for-third-party-updates/). For more details about this scenario, please see the following Microsoft Doc: [**Additional requirements when the SUP is remote from the top-level site server**](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#additional-requirements-when-the-sup-is-remote-from-the-top-level-site-server)**.**
>
> If your software update point is remote from your site server and the software update point is not in SSL, you can [**use group policy to deploy the WSUS Signing Certificate**](../../scupcatalog/documentation/CertificateAndGPODeploymentGuide.pdf) instead of the option in Configuration Manager to **Automatically manage the WSUS Signing Certificate**.

#### How to Deploy the WSUS Signing Certificate for Third-Party Software Updates

Once you have the **WSUS Signing Certificate** created, it needs to be deployed to all your devices for the third-party software updates to be trusted. If you need more details on the options available for certificate deployment, please review this knowledge base article [**How to Deploy the WSUS Signing Certificate for Third-Party Software Updates**](../../how-to-deploy-the-wsus-signing-certificate-for-third-party-software-updates/)