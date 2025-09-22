# How to package Visual Studio 2022 as a Custom App

In this article, we will show you how you can prepare a package for Visual Studio 2022 and deploy it as a Custom App using Patch My PC Cloud or publish it as a Custom App using Patch My PC Publisher.

With its modular installation model, Visual Studio allows you to select specific workloads based on development needs. However, this flexibility also means that packaging Visual Studio for deployment requires careful planning, especially when managing installation size, dependencies, and update strategies.

The following guide will focus on the **core editor workload** only for the **Community Edition**.

> This article serves as a guide to illustrate how Visual Studio can be packaged as a custom application. However, due to the many variables involved in configuring workloads and application settings, Patch My PC Support is unable to provide assistance with tailoring the package to meet specific customization needs.
>
> For detailed guidance on customizing the offline installer, including available command-line options, please refer to the [official documentation](https://learn.microsoft.com/en-us/visualstudio/install/use-command-line-parameters-to-install-visual-studio?view=vs-2022)

### Prepare a package for Visual Studio 2022 <a href="#h-prepare-a-package-for-visual-studio-2022" id="h-prepare-a-package-for-visual-studio-2022"></a>

The steps below apply to the Community, Professional, and Enterprise editions of Visual Studio. To ensure you always install the latest version of the Current Channel for Visual Studio 2022, download the appropriate bootstrapper from the table below:-

| **Edition**                     | **Download URL**                                                                                      |
| ------------------------------- | ----------------------------------------------------------------------------------------------------- |
| Visual Studio 2022 Professional | [https://aka.ms/vs/17/release/vs\_professional.exe](https://aka.ms/vs/17/release/vs_professional.exe) |
| Visual Studio 2022 Enterprise   | [https://aka.ms/vs/17/release/vs\_enterprise.exe](https://aka.ms/vs/17/release/vs_enterprise.exe)     |
| Visual Studio 2022 Community    | [https://aka.ms/vs/17/release/vs\_community.exe](https://aka.ms/vs/17/release/vs_community.exe)       |

In the following example, we will prepare a package for the **Community** edition.

#### Step 1: Download the Stub Installer <a href="#h-step-1-download-the-stub-installer" id="h-step-1-download-the-stub-installer"></a>

1. Download the Visual Studio Community 2022 stub installer from:-\
   [`https://aka.ms/vs/17/release/vs_community.exe`](https://aka.ms/vs/17/release/vs_community.exe)
2. Save it to a location where you have write permissions.

#### Step 2: Create the Local Layout <a href="#h-step-2-create-the-local-layout" id="h-step-2-create-the-local-layout"></a>

It is necessary to create a local layout which downloads the necessary workload modules and files using the stub installer. Additionally, the layout will set the language for the package. The only workload and component we will use for this example package is `Microsoft.VisualStudio.Component.CoreEditor` and `Microsoft.VisualStudio.Workload.CoreEditor.`For a full list of componenets and workloads for each edition, follow the links at [https://learn.microsoft.com/en-us/visualstudio/install/workload-and-component-ids?view=vs-2022](https://learn.microsoft.com/en-us/visualstudio/install/workload-and-component-ids?view=vs-2022)

Additionally, we will use the **en-US** language locale. For a full list of language locales, see [https://learn.microsoft.com/en-us/visualstudio/install/use-command-line-parameters-to-install-visual-studio?view=vs-2022#list-of-language-locales](https://learn.microsoft.com/en-us/visualstudio/install/use-command-line-parameters-to-install-visual-studio?view=vs-2022#list-of-language-locales)

1. Open a Command Prompt or PowerShell window as Administrator.
2. Navigate to the directory where you saved the stub installer.
3.  Run the following command to create a minimal local layout:

    `.\vs_community.exe --layout .localVSlayout --add Microsoft.VisualStudio.Component.CoreEditor --add Microsoft.VisualStudio.Workload.CoreEditor --lang en-US`
4.  Wait for the download to complete. This may take some time depending on your internet connection.

    <figure><img src="https://patchmypc.com/app/uploads/2025/04/vs2022_1.png" alt="" height="319" width="718"><figcaption></figcaption></figure>

#### Step 3: Prepare the Layout Package <a href="#h-step-3-prepare-the-layout-package" id="h-step-3-prepare-the-layout-package"></a>

1. Locate the `localVSlayout` folder created in Step 2.
2. Compress the following files and folders into a ZIP archive:
   * Select all folders and files within `localVSlayout` **except** `vs_setup.exe`
   * Right-click and select “Compress to ZIP file” or use a tool like 7-Zip
   * Name the archive `localVSlayout.zip`
3. Delete the original files and folders from `.localVSlayout` after confirming the ZIP file was created successfully.
4.  The compressed zip file should contain the following files and folders (the exact files and folders will vary depending on the edition you used and the workloads you specified in your layout).\


    ### &#x20;<a href="#heading-0" id="heading-0"></a>
5.  The localVSlayout folder should now only contain **localVSlayout.zip** and **vs\_setup.exe**

    <figure><img src="https://patchmypc.com/app/uploads/2025/04/vs2022_3.png" alt="" height="118" width="655"><figcaption></figcaption></figure>

### Create the Custom App <a href="#h-create-the-custom-app" id="h-create-the-custom-app"></a>

We now have the necessary files to create the custom app in Patch My PC Cloud.

1. Navigate to [https://portal.patchmypc.com/app-catalog/upload](https://portal.patchmypc.com/app-catalog/upload).
2. Click **Add Primary Install File** and select **vs\_setup.exe.**\

3. Click **Add Files** and select **localVSlayout.zip** (The zip file is quite large so may take a short while to upload, you can still proceed through the wizard during the upload).![](https://patchmypc.com/app/uploads/2025/04/vs2022_5.png)
4. Click **Next**.
5.  On the **General Information** tab, enter the following information:-

    | **Field**   | **Data**                                                                                                                                                                                                                            |
    | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | App Icon    | Provide a suitable icon for the app                                                                                                                                                                                                 |
    | App Name    | Visual Studio Community 2022                                                                                                                                                                                                        |
    | Vendor      | Microsoft Corporation                                                                                                                                                                                                               |
    | Description | Visual Studio Community 2022 is Microsoft’s free, fully-featured IDE for students, open-source contributors, and individual developers. It offers a comprehensive development environment for building applications across platform |
6. Click **Next**.
7.  On the **Configuration** tab, enter the following information:-

    | **Field**                 | **Data**                     |
    | ------------------------- | ---------------------------- |
    | Install Context           | System                       |
    | Architecture              | 64-bit                       |
    | Version                   | 17.3.1                       |
    | Language                  | English                      |
    | Apps & Features Name      | Visual Studio Community 2022 |
    | Conflicting processes     | devenv.exe                   |
    | Silent Install Parameters | ––quiet ––wait ––norestart   |
8.  Click **Create**. (You may not be able to click Create if the localVSlayout.zip has not finsihed uploading yet).\


    <figure><img src="https://patchmypc.com/app/uploads/2025/04/vs2022_8.png" alt="" height="147" width="529"><figcaption></figcaption></figure>

> **Important:** You must use the correct **Apps & Features Name** and **Version** for the edition and version of Visual Studio 2022 you are creating as a Custom App. If you do not, the application will fail to be detected after it has installed.
>
> Patch My PC recommends to manually install Visual Studio 2022 using the stub installer to visually check the **Name** and **Version** that should be used when creating the Custom App. This can be done very easily be examining the **Name** and **Version** in the Control Panel.
>
> <img src="https://patchmypc.com/app/uploads/2025/04/vs2022_9.png" alt="" data-size="original">

### Deploy Visual Studio 2022 with Patch My PC Cloud <a href="#h-deploy-visual-studio-2022-with-patch-my-pc-cloud" id="h-deploy-visual-studio-2022-with-patch-my-pc-cloud"></a>

Once the Custom App has been created, it can be deployed using Patch My PC Cloud to Intune.

1. Navigate to the Patch My PC portal [https://portal.patchmypc.com/app-catalog](https://portal.patchmypc.com/app-catalog).
2.  Search for **Visual Studio** and select the Custom App that was created for Visual Studio.\


    <figure><img src="https://patchmypc.com/app/uploads/2025/04/vs2022_11.png" alt="" height="556" width="895"><figcaption></figcaption></figure>
3.  Click **Deploy**.\


    <figure><img src="https://patchmypc.com/app/uploads/2025/04/vs2022_12.png" alt="" height="220" width="730"><figcaption></figcaption></figure>
4.  On the **Configurations** tab, select **Scripts** and click **Add +** to add a **Pre Install** script.\


    <figure><img src="https://patchmypc.com/app/uploads/2025/04/vs2022_13.png" alt="" height="549" width="885"><figcaption></figcaption></figure>
5.  The pre-script is going to expand the zip archive so all the required files and folders are accesible for the main installer file **vs\_setup.exe**. Paste the following script block into the script body window, name the script **Expand-Archive** and click **Save**.\
    \
    `try {`\
    `Get-ChildItem -Path $PSScriptRoot -Filter "*.zip" | ForEach-Object {`\
    `Expand-Archive -Path $_.FullName -DestinationPath $PSScriptRoot -Force -ErrorAction Stop }`\
    `} catch {`\
    `exit 1`\
    `}`\
    \


    ![](https://patchmypc.com/app/uploads/2025/04/vs2022_14.png)
6. Click **Next** to add your desired **Assignments**.
7. Click **Deploy**\


### Publish Visual Studio 2022 with Patch My PC Publisher <a href="#h-publish-visual-studio-2022-with-patch-my-pc-publisher" id="h-publish-visual-studio-2022-with-patch-my-pc-publisher"></a>

Once the Custom App has been created, it can be Published using Patch My PC Publisher.

1. Open Patch My PC Publishing Settings.
2.  Select either the **ConfigMgr Apps**, **Intune Apps** or **Intune Updates** tab and find the Custom App (You may need to click the “Refresh the list of products” button indicated below).\


    <figure><img src="https://patchmypc.com/app/uploads/2025/04/vs2022_16.png" alt="" height="549" width="708"><figcaption></figcaption></figure>
3. Select the check-box beside the app to enable it for publishing.
4.  Right-Click the app and click **Add custom pre/post scripts**.\


    <figure><img src="https://patchmypc.com/app/uploads/2025/04/vs2022_17.png" alt="" height="714" width="713"><figcaption></figcaption></figure>
5. A pre-script is required to expand the zip archive so all the required files and folders are accesible for the main installer file **vs\_setup.exe**
6. Copy the following code into a file and name the file **Expand-Archive.ps1**.\
   `try { Get-ChildItem -Path $PSScriptRoot -Filter "*.zip" | ForEach-Object { Expand-Archive -Path $_.FullName -DestinationPath $PSScriptRoot -Force -ErrorAction Stop }; } catch { exit 1 }`![](https://patchmypc.com/app/uploads/2025/04/vs2022_18.png)
7.  Click the **Browse** button for the **Pre Script** and select the **Expand-Archive.ps1** file that we just created. Additionally, select the check box name **Don’t attempt software update if the pre script returns an exit code other than 0 or 3010**.\


    <figure><img src="https://patchmypc.com/app/uploads/2025/04/vs2022_20.png" alt="" height="393" width="629"><figcaption></figcaption></figure>
8. Click **OK**.
9. The app is now ready to be published.
