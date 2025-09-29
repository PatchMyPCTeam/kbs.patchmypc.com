---
title: "Add an MST Transform to the Adobe Acrobat Reader Base Install"
date: 2020-07-08
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

You can customize the Adobe Acrobat Reader using a custom **MST transform** file via Patch My PC using our **[custom command line feature](/custom-options-available-for-third-party-updates-and-applications#modify-command-line)**. The Adobe Acrobat Reader Base Installation uses the executable (.EXE) installer by default, not an MSI. However, it is still possible to **use a custom MST to customize the Reader installation**. Follow the instructions below to add a custom MST to the Patch My PC Base installation for Adobe Acrobat Reader.

> **Note:** Patch My PC uses the latest Adobe Acrobat Reader EXE based installer for Base installations. Because of this, the Adobe Acrobat Reader version installed via the base installation feature may not match the latest MSP patch provided by Adobe.

### Adding the MST file

1. **Generate your MST transform file** using the Acrobat Customization Wizard or similar MSI modification software.

3. Place the generated MST in a centralized location that the Patch My PC Publisher has access to.

5. Open the Patch My PC Publisher and navigate to **_ConfigMgr Apps_** \-> **_All Products_** -> **_Adobe Systems, Inc._** \-> **_Adobe Acrobat Reader DC_**

7. Right-click the application to modify and choose to **Add custom pre/post update installation scripts**.  
    ![Adobe MST Right Click Pre/Post Script](images/AdobeMST-RC-AddPrePostScript.png)
      
    

9. Click **Browse**... next to the **Additional Files** list box and select your custom **MST file**, Click **OK**.  
    ![Adobe MST Additional Files Window](images/AdobeMST-W-AdditionalFiles.png)
      
    

11. Right-click the application to modify once more and choose **_Modify Command Line_**  
     ![Adobe MST Right Click Modify Command Line](images/AdobeMST-RC-ModifyCommandLine.png)
     

13. Enter the following in the Additional Argument field and click **OK**:  
     TRANSFORMS=  
     ![Adobe MST Modify Command Line Window](images/AdobeMST-W-AdditionalArgs.png)
     

15. Click **Apply** and **run a Sync** to update the Application in ConfigMgr.

17. After the sync completes, the MST customizations should apply to **future installations** of the Adobe Reader base application.
