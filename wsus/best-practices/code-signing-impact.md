---
title: "How The Publisher is Affected by Code Signing Certificate Changes"
date: 2023-10-25
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

Today I want to talk a bit about the Public CA code signing certificate changes that were introduced in June 2023. But first, I want to point out that this only applies to Patch My PC customers that are using code signing certificates issued from Public CAs. Most customers will remain unaffected by these changes I am outlining below.

That being said, let’s get into it!

If you use the Publisher in conjunction with WSUS/Configuration Manager, you are required to have a trusted code-signed certificate so that Patch My PC can publish third-party updates in your environment.

![](/_images/how_the_publisher.jpg)

Unfortunately, this process has become more complicated as of June 1st, 2023. [Industry standards](https://knowledge.digicert.com/generalinformation/new-private-key-storage-requirement-for-standard-code-signing-certificates-november-2022.html) have changed to require private keys for standard code signing certificates to be stored on HSMs (hardware security modules). To use a certificate on an HSM, you need to have access to the HSM and the appropriate credentials to use the certificate stored on it. Since Patch My PC and the Publisher cannot access your private HSM, that means that we cannot use that certificate to publish updates in your environment.

So, if you cannot use these public CA code signing certificates, what can you do? You have two options:

1. You can transfer to a self-signed certificate (which you can read more about here: [What is the WSUS Signing Certificate and How to Create It - Patch My PC](https://patchmypc.com/wsus-signing-certificate-options-for-third-party-updates-in-configuration-manager)).  
    ![](/_images/you_can_transfer.jpg)

3. If your organization has an internal CA, you can request and use a code signing certificate from within [your own organization](https://www.techtarget.com/searchsecurity/definition/private-CA-private-PKI).

Hopefully this gives you an idea of how to work with the changes we’ve seen to Public CA certificates earlier this year. If you need further assistance, please feel free to reach out to [support@patchmypc.com](mailto:support@patchmypc.com).