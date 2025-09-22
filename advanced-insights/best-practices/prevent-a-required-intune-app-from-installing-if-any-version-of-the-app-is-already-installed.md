# Prevent a required Intune app from installing if any version of the app is already installed

In some situations, such as during Autopilot deployments, you may need to mark a Patch My PC **Intune app** as **required** to ensure it installs during the Enrollment Status Page (ESP) phase, when blocking apps are enforced. However, after the app is installed, you might want to prevent that required targeting from pushing newer versions of the app to the device. Instead, you can use Patch My PC **Intune updates** to manage future updates using update rings. This approach lets you control the update cadence, avoiding the immediate updates that would otherwise happen due to the original required targeting.

In this article, we’ll guide you through configuring an extra requirement on Patch My PC **Intune apps** to prevent immediate app updates after the initial installation has occured.

### Scenario 1 <a href="#h-scenario-1" id="h-scenario-1"></a>

#### Install a required app during Autopilot but prevent the same, required, assignment from updating the app when a new version is published by Patch My PC. <a href="#h-install-a-required-app-during-autopilot-but-prevent-the-same-required-assignment-from-updating-the" id="h-install-a-required-app-during-autopilot-but-prevent-the-same-required-assignment-from-updating-the"></a>

In deployment scenarios like Autopilot, it’s common to designate apps as **required** to ensure they install during the ESP phase. This ensures that critical applications are in place before the device is handed over to the user. However, a challenge typically arises after the initial installation – the same **required** assignment that ensured the app was deployed can also trigger **immediate** updates when a newer version of the app becomes available. This behavior can be problematic for organizations that want to control when app updates are applied, especially when users need stability and consistency on their devices.

**Why does this occur?**

The issue arises because Patch My PC creates the initial version of the app using **Intune apps**, and admins will assign it as **required** to **All Devices** (for example) for deployment. This ensure’s the app gets installed during Autopilot when using the ESP blocking apps feature. When a new version of the app is released by the vendor, Patch My PC automatically creates and publishes the updated version while retaining the same assignments as the initial release.

This means that the new version is immediately targeted to the same devices with a required assignment, resulting in the app being pushed as soon as it’s available.

**Why is this a problem?**

This breaks any expected ringed update approach when customers have deployed Patch My PC **Intune updates**, where organizations typically want to stagger updates through phased deployments, ensuring stability and controlled testing before the app reaches all users. This behavior can lead to the new version being deployed too quickly, bypassing the gradual rollout process many customers expect.

### Scenario 2 <a href="#h-scenario-2" id="h-scenario-2"></a>

#### Install a required app for a specific set of users or devices but prevent the same, required, assignment from updating the app when a new version is published. <a href="#h-install-a-required-app-for-a-specific-set-of-users-or-devices-but-prevent-the-same-required-assign" id="h-install-a-required-app-for-a-specific-set-of-users-or-devices-but-prevent-the-same-required-assign"></a>

In many deployment scenarios, admins may choose to assign an app as required for specific users or devices to ensure it is deployed automatically. This approach ensures that critical applications are installed for a targeted group, ensuring those users or devices have the necessary apps to perform their job function. However, a challenge often arises after the initial installation – the same required assignment that ensured the app was deployed to the intended group can also trigger an immediate update when a newer version of the app is released by the vendor. This behavior can be problematic for organizations looking to maintain control over when app updates are applied.

**Why does this occur?**

The issue occurs because Patch My PC creates the initial version of the app using **Intune apps**, and admins often assign it as required for specific users or devices. This guarantees the app is installed across the specified group. When a new version of the app is released by the vendor, Patch My PC automatically generates and publishes the updated version while retaining the same assignments that were set for the initial release. As a result, the new version is immediately targeted to those same users or devices, leading to an immediate update.

**Why is this a problem?**

This disrupts any desired phased update strategy, particularly when Patch My PC **Intune updates** are in place. Organizations typically want to roll out updates gradually, ensuring that testing is performed and stability is maintained before the app reaches all users or devices. However, this behavior can result in updates being deployed too quickly, bypassing the carefully planned rollout process. Customers expecting to manage updates using rings or controlled phases may find their users or devices receiving updates sooner than anticipated, potentially introducing instability or unforeseen issues within their environments.

### Solution <a href="#h-solution" id="h-solution"></a>

### Step 1: Download and edit the Get-NotInstalledRequirement.ps1 script <a href="#h-step-1-download-and-edit-the-get-notinstalledrequirement-ps1-script" id="h-step-1-download-and-edit-the-get-notinstalledrequirement-ps1-script"></a>

The provided script will be used as a requirement check for a Win32 app in Intune, designed to evaluate if specific software is already installed on a device. The script performs the following:-

1. **Checks for Installed Applications**\
   The script looks for installed applications by searching the Windows registry for specific software titles listed in the **$appNameList** array. In the provided example in the script, it searches for ‘Cisco Secure Client’ and ‘Cisco AnyConnect’ by their “Apps & Features” DisplayName.
2. **Registry Search**\
   It examines the registry paths related to both 32-bit (Wow6432Node) and 64-bit installations, depending on the device architecture, to check for installed applications. It scans the registry hives (HKLM and HKCU) for the software names.
3. **Custom Matching**\
   The script performs a pattern match (using -like) to find software that matches the names in **$appNameList**. If a match is found, the script does not return any output, which means the requirement is not met, preventing the Win32 app from installing.
4. **Determines Applicability**\
   If the specified software is found, the script outputs **Applicable**. This indicates that the software is not installed, satisfying the requirement for the Win32 app to be installed on the device.

1\. Download the script from [https://github.com/PatchMyPCTeam/Community-Scripts/blob/main/Install/NotInstalledRequirement/Get-NotInstalledRequirement.ps1](https://github.com/PatchMyPCTeam/Community-Scripts/blob/main/Install/NotInstalledRequirement/Get-NotInstalledRequirement.ps1)

2\. Open the script in VSCode, Notepad or another text editor of your choice.

3\. Replace the contents of the array list **$appNameList** on **line 36** with the **Apps & Features / Installed Apps** name of the app you want to check for.

> **Note:** The script will search for the exact match based on what you enter in $appNameList. You can add asterisk characters (\*) if you want to use wildcards.
>
> For example: **$appNameList = @(‘7-Zip\*2\*’)** will match a DisplayName of **7-Zip 24.07 (x64)**

> **Important:** Using an overly generic name may cause the app to be falsely detected as installed.

In the following example, we replace the default entries in the **$appNameList** array with **‘Google Chrome’** as this is the DisplayName that is shown in **Apps & Features / Installed Apps** when this app is installed.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/required_apps_solution_9.jpg" alt=""><figcaption></figcaption></figure>

```
$appNameList = @('Google Chrome')
```

<figure><img src="https://patchmypc.com/app/uploads/2025/04/required_apps_solution_8.jpg" alt=""><figcaption></figcaption></figure>

&#x20;4\. Save the script.

### Step 2: Select your Publishing Method <a href="#h-step-2-select-your-publishing-method" id="h-step-2-select-your-publishing-method"></a>

The app must be published **before** we can add the requirement script in the Intune Admin Center.&#x20;

### Patch My PC Publisher <a href="#h-patch-my-pc-publisher" id="h-patch-my-pc-publisher"></a>

In the following example, we will use Google Chrome:-

1\. Open the Patch My PC Publisher.

2\. Navigate to the Intune apps tab and click **options** (the options button might look different than depicted in the image below depending on your licence level).

<figure><img src="https://patchmypc.com/app/uploads/2025/04/required_apps_solution_1-2.jpg" alt=""><figcaption></figcaption></figure>

2\. Ensure the check box is selected for [**Copy the requirements from previously created applications or updates when an updated application is created**](https://patchmypc.com/intune-application-creation-options#CopyRequirements)

<figure><img src="https://patchmypc.com/app/uploads/2025/04/required_apps_solution_2.jpg" alt=""><figcaption></figcaption></figure>

3\. Ensure your desired variant of **Google Chrome** is selected with the intent to Publish by verifying the check box is ticked on the Intune apps tab.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/required_apps_solution_3.jpg" alt=""><figcaption></figcaption></figure>

4\. After the next schedule sync cycle, or a manual sync, or as a result from being published previously, find your selected variant of Google Chrome in the Intune Admin Center.

5\. Proceed to [Step 3](https://patchmypc.com/kb/prevent-required-intune-app-from/#step3).

### Patch My PC Cloud <a href="#h-patch-my-pc-cloud" id="h-patch-my-pc-cloud"></a>

In the following example, we will use Google Chrome:-

1\. In a browser, navigate to [https://portal.patchmypc.com](https://portal.patchmypc.com/).

2\. By reviewing the **Deployments** tab, ensure the app is already deployed. If it is not, [follow the instructions here](https://docs.patchmypc.com/installation-guides/patch-my-pc-cloud/deployments/deploy-an-app) to deploy an Intune app from Patch My PC Cloud.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/required_apps_solution_12.jpg" alt=""><figcaption></figcaption></figure>

3\. Proceed to [Step 3](https://patchmypc.com/kb/prevent-required-intune-app-from/#step3).

### Step 3: Add the requirement script to the app in the Intune Admin Center <a href="#h-step-3-add-the-requirement-script-to-the-app-in-the-intune-admin-center" id="h-step-3-add-the-requirement-script-to-the-app-in-the-intune-admin-center"></a>

To address the issue where new versions of required Patch My PC apps continue to install immediately on targeted Entra ID groups after the initial deployment, a **requirement script** can be implemented. This solution leverages the ability to add custom requirements to Win32 apps in Intune, ensuring that if the app is already installed, a newer version of the Intune app will not install, regardless of a required assignment.

By using a requirement, the script checks if **any** version of the app is already present on the device, and if so, it marks the Win32 app as **not applicable**. This prevents the immediate install behavior triggered by the required assignment.

In the following example, we will continue to use Google Chrome:-

1\. Select the app in the Intune Admin Centre. In our example, we are using **Google Chrome**.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/required_apps_solution_4.jpg" alt=""><figcaption></figcaption></figure>

2\. Select **Properties** and click **Edit** in the app requirements section.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/required_apps_solution_5.jpg" alt=""><figcaption></figcaption></figure>

3\. Click **Add**.

4\. Select the Requirement type **Script**.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/required_apps_solution_7-1.jpg" alt=""><figcaption></figcaption></figure>

5\. Select the edited script you saved in [Step 1](https://patchmypc.com/kb/prevent-required-intune-app-from/#download-edit-script) and apply the following settings:-

<figure><img src="https://patchmypc.com/app/uploads/2025/04/required_apps_solution_10.jpg" alt=""><figcaption></figcaption></figure>

* Script file: **Get-NotInstalledRequirement.ps1**
* Run script as 32-bit process on 64-bit clients: **No**
* Run this script using the logged on crednetials: **No** (Unless you are deploying an app that installs in the User context, then set this to **Yes**).
* Enforce script signature check: **No**
* Select output data type: **String**
* Operator: **Equals**
* Value: **Applicable**

6\. Click **OK**.

7\. Click **Review & Save** twice and click **Save**.

8\. The Win32 app should indicate that it now has the requirement script configured.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/required_apps_solution_11.jpg" alt=""><figcaption></figcaption></figure>

### Step 4: Verify a newer version of a required Intune app will not install if an older version of the application is already installed <a href="#h-step-4-verify-a-newer-version-of-a-required-intune-app-will-not-install-if-an-older-version-of-the" id="h-step-4-verify-a-newer-version-of-a-required-intune-app-will-not-install-if-an-older-version-of-the"></a>

In the following example, **Google Chrome 114.0.5735.199** is already installed on the device.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/required_apps_solution_13.jpg" alt=""><figcaption></figcaption></figure>

We can simulate a required deployment of a newer version of the Intune app. We deployed **Google Chrome 130.0.6723.59**, as an Intune app. In the Company Portal we can verify the newer version of the Intune app did not install, it is marked as **Requirements not met**.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/required_apps_solution_14.jpg" alt=""><figcaption></figcaption></figure>

The AppWorkload.log also indicates the applicability result: ScriptRequirementRuleNotMet = **1008** (0x000003F0).

<figure><img src="https://patchmypc.com/app/uploads/2025/04/required_apps_solution_15.jpg" alt=""><figcaption></figcaption></figure>

The AppActionProcessor.log also indicates the Intune app is not applicable.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/required_apps_solution_16-1.jpg" alt=""><figcaption></figcaption></figure>

### Summary <a href="#h-summary" id="h-summary"></a>

By utilizing the script in this article as a requirement for Patch My PC Intune apps, the installation of newer versions of the app can be blocked if an older version of the app is already installed on a device. The script checks the system for the app and, if found, marks the app as “not applicable,” ensuring the required assignment does not trigger an immediate, unplanned, update of the app.

Crucially, this approach does not interfere with Intune updates managed by Patch My PC. Once the app is installed, Patch My PC Intune updates will continue to work as expected, allowing you to control when updates are deployed. This gives you the flexibility to maintain controlled rollouts, like during Autopilot, while ensuring an expected cadence of update roll-out across your organisation.
