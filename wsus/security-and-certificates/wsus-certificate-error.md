---
title: 'An error occurred while extracting the certificate from WSUS: Access is denied'
date: 2021-12-03T00:00:00.000Z
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
    - troubleshooting
    - common-issues-and-error-codes
---

# WSUS Certificate Error

### Determine if You are Affected

Publishing may fail on Server 2012 / 2016 with the following error message in the [**PatchMyPC.log**](../../collecting-log-files-for-patch-my-pc-support/#publishing-service-app-logs-intune):

An error occurred while extracting the certificate from WSUS: C:\Usersusername\AppData\Local\Temp\PMP-tempfolderrandomfolder.tmp : Access is denied\
Failed to extract the certificate from WSUS Message

This error can be caused by a lack of permissions on the WSUSCertServer Service.

### Workaround

1. Add the Computer account of the Server that the Patch My PC Publisher is installed to the "WSUS Administrators" Group
2. Open **Regedit** and navigate to "**HKEY\_CLASSES\_ROOT\AppID\\{8F5D3447-9CCE-455C-BAEF-55D42420143B}**"
3. Right Click that key, select Permissions, then click Advanced.
4. At the top of the Advanced Permissions window, change the Owner to "WSUS Administrators" and Click OK
5. While in this properties window, also ensure that Administrations and SYSTEM have full control of that registry key
   1. In order to complete this step "Administrators" may need to be made the owner before adding the "Full Control" permissions. Once the "Full Control" permissions are added, set the owner back to "WSUS Administrators"\
      ![Registry Key Permissions required to adjust DCOM permissions on WSusCertServer](/_images/RegistryKeyPermissions.png "Registry Key Permissions required to adjust DCOM permissions on WSusCertServer")
6. Start dcomcnfg.exe as Administrator
7. In the Tree select "**Component Services**" ->"**My Computer**" -> "**DCOM Config**"
8. Scroll down and right click on WSusCertServer and click "Properties", and navigate to the "Security" Tab
9. Click "Customize" under "**Launch and Activation Permissions**" then click "Edit"
   1. Ensure "Local Launch", "Remote Launch", "Local Activation", and "Remote Activation" are enabled for the following accounts: "**WSUS Administrators**", "**SYSTEM**", and "**Administrators**"\
      ![dcomcnfg Launch and activation permissions for WSusCertServer](/_images/dcomcnfglaunchandactivationpermissions.png "dcomcnfg Launch and activation permissions for WSusCertServer")
10. Click "**Customize**" under "**Access Permissions**" then click "**Edit**"
    1. Ensure "**Local Access**" and "**Remote Access**" are enabled for the following accounts:  "**WSUS Administrators**", "**SYSTEM**", and "**Administrators**"\
       ![dcomconf access permissions configuration](/_images/dcomcnfgaccesspermissions.png "dcomconf access permissions configuration")
11. Click "**Customize**" under "**Configuration Permissions**" then click "**Edit**"
    1. Ensure the following accounts have "Full Control": "WSUS Administrators", "SYSTEM", and "Administrators"\
       ![dcomcnfg Configuration Permissions](/_images/dcomcnfgconfigurationpermissions.png "dcomcnfg Configuration Permissions")
12. Restart the WSUSCertServer Service from Services.msc