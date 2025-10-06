---
title: Manage Conflicting Processes when Updating Third-Party Applications
date: 2021-02-24T00:00:00.000Z
taxonomy:
  products:
    - patch-my-pc-publisher
  tech-stack:
    - null
  solution:
    - null
  post_tag:
    - null
  sub-solutions:
    - best-practices
    - troubleshooting
    - application-and-update-publishing
---

# Manage Conflicting Processes When Updating

When updating third-party applications, there may be cases where you need to **close an application** that is currently open by an end-user. This guide will explain the option "**Notify the user to close the application**" within the [custom right-click option](../../custom-options-available-for-third-party-updates-and-applications/) **Manage conflicting processes**.

![](/_images/Manage-Conflicting-Process-17.png)

### Video Walkthrough of Notify the user to close the application feature

If you prefer to watch a video guide of how this feature works, please check out the video below.

### Configuration Options for Notify the user to close the application

There are a variety of options available for this feature. Below you can find detailed explanations of each option.

* **Notify Timeout Configuration**
  * This option controls how long the user notification will remain open on the screen.
  * Time out after... - maximum value depends on the platform. See below for details.
    * **While the user notification is displayed, the update or application install is considered to be running.** The run time here would contribute to the max run time of your applications and updates, and consume time during a maintenance window, for example.
    * Platform Specific Maximums: Max runtimes account for a 15-minute buffer to allow the application to install after a notification has timed out or closed. (Note: There are newer features in ConfigMgr to default this to a higher value that Patch My PC will investigate)
      * Updates Max: 5 minutes
        * Third-party updates in ConfigMgr can have a few default values for Maximum Runtime depending on your version of ConfigMgr. We default to assuming the maximum runtime of third-party updates will be 15 minutes. Because of this, we have the maximum set to 5 minutes, allowing 10 minutes for the update to run before it would exceed the update maximum run time. See the 'use maximum run time' info below for a workaround to this.
      * ConfigMgr App Max: 705 minutes
        * This is the maximum configurable run time for a ConfigMgr application minus a 15-minute buffer.
      * Intune (Applications and Updates) Max: 1425 minutes
        * Intune has a non-configurable run time of 1440 minutes (1 day) for Win32 applications. The notification max is this 1440 minutes minus a 15-minute buffer.
  * Option to use ‘maximum run time’ from the respective update or app.
    * ConfigMgr Update Max: Will use configured update ‘max run time’ as configured in ConfigMgr for the update.
      * Note: Update max run time must be edited before the update is deployed for a client to recognize the change.
      * If either the maximum run time in ConfigMgr, or the Notify Timeout Configuration, is under 15 minutes, the timeout notification will default to 5 minutes. This is because a 15-minute buffer is applied. For example, a 60-minute max run time will have a 45-minute UI timeout.
    * ConfigMgr App Max: Will use the configured deployment time ‘max run time.’
    * Intune App/Update Max: Will use the maximum run time of an Intune Win32 app (60 minutes minus the 15-minute buffer).
  * Once the timeout is reached, the options defined in the 'Defer Policy' section described below will either increment or start the day deferral count, depending on your settings.
* **Notification behavior if the application is running and focus assist is enabled**
  * This option will configure whether the notification should appear when [**focus assist**](https://support.microsoft.com/en-us/windows/turn-focus-assist-on-or-off-in-windows-10-5492a638-b5a3-1ee0-0c4f-5ae044450e09) is enabled on the machine and whether it should impact deferral counts.
    * Discard the notification
    * Always show the notification
    * Show the notification if the deferral policy is reached
* **Allow the user to defer the installation**
  * Note that when a user selects to defer, the application will 'fail' to install.
  * This option will control whether the user can defer the installation to a later time. The following options are available when this setting is enabled:
    * **Indefinitely**: The user can defer the update indefinitely.
      * Keep in mind this may result in updates never being installed.
    * **Up to 5 Times**: The user can defer up to the specified number of times.
      * The installation will try again during the next application or update evaluation cycle, which will depend on the platform that is being used, ConfigMgr Software Updates, ConfigMgr Apps, or Intune Win32 applications.
      * This has a minimum value of 1 and a maximum value of 100.
    * **First notification displayed + 5 days:** The user can delay the software installation up to the specified number of days after the first notification was displayed, or the first notification would have been displayed based on focus assist settings described above.
      * The installation will try again during the next application or update evaluation cycle, which will depend on the platform that is being used, ConfigMgr Software Updates, ConfigMgr Apps, or Intune Win32 applications.
      * This has a minimum value of 1 and a maximum value of 15.
* **If the timeout expired and no action is taken...**
  * There are two options for when the user does not provide any response to a conflicting process notification within the timeout period. The below event will happen if the notification is left to expire without any user interaction.
    * **Defer the installation on behalf of the user (Default)**:
      * The notification will timeout and be closed and count towards deferral, whether that is a count-based deferral or a time-based deferral.
      * **Note:** With this option enabled, the **software will force close when the deferral count is met**, such as having the count set to 5, and the user missing 5 notifications. The application would close and update after the 5 notification timeout.
    * **Close the application and perform the update**:
      * The notification will timeout, the application will be force-closed, and the update for the application will begin to install.
      * **Note**: With this option enabled, the **software will force close** if someone is not present to dismiss the notification, such as being at the lock screen or otherwise not present.
* **Proceed with the installation even if the application is still running after the countdown (Auto close application)**:
  * This option will show a countdown matching your selected timeout setting and force close the application if the user does not choose to close and update before the timer is completed.
* **Prevent the end-user from opening an application while the application is updating**
  * If the installation is running and a user attempts to start a known conflicting process, they will receive a message box letting them know the application's startup is blocked.
  * This option can only be set at the product level. If an installer launches a conflicting process during installation, it can cause failures.&#x20;
  * Refer to the '[Update in progress...](manage-conflicting-processes-when-updating.md#h-update-in-progress)' section for more detailed information.

![Update in progress user notification](/_images/UpdateInProgress.png "Update in progress user notification")

> **Note:** If an update is snoozed, or if the notification times out the software will return a 1602 exit code.

### Manage Process List

The **Mange process list** option allows you to add and remove apps that need to close in order for the update to process (This applies only if you have Auto-close or Notify option enabled).

![](/_images/Manage-Conflicting-Process-18.png)

The UI notification for Conflicting Processes now lists all processes which are conflicting in a dropdown. This is to make it more clear what software will be closed.

![](/_images/Manage-Conflicting-Process-22-2.gif)

### Add Custom Branding for Notifications

In the **Manage default settings** options, you can configure a customer banner image and company name.

![](/_images/Manage-Conflicting-Process-19.png)

You can also preview how it will appear for an end-user by clicking **Preview how it will** **appear**.

![Patch My PC Close and Update UI Popup](/_images/Patch-My-PC-Close-and-Update-UI-Popup.png "Patch My PC Close and Update UI Popup")

### Custom Banner Images for Custom Usage

We have various banner images available below. You can use these images to customize the experience of the popup or use your company banner.

Download our [default JPG banner image](https://patchmypc.com/app/uploads/2025/04/UI-Notification-Banner.jpg) below.

![Default Notification Banner](/_images/UI-Notification-Banner.jpg "Default Notification Banner")

Download the [light-theme GIF image](https://patchmypc.com/app/uploads/2025/04/Patch-My-PC-Close-Application-GIF-370x100-1.gif) below to show an application close and update completed.

![Patch My PC - Close Application GIF - 370x100](/_images/Patch-My-PC-Close-Application-GIF-370x100-1.gif "Patch My PC - Close Application GIF - 370x100")

Download the [dark-theme GIF image](https://patchmypc.com/app/uploads/2025/04/Patch-My-PC-Close-and-Update-UI-Popup-Dark-Theme.gif) below to show an application close and update completed.

![Patch My PC Close and Update UI Popup - Dark Theme](/_images/Patch-My-PC-Close-and-Update-UI-Popup-Dark-Theme.gif "Patch My PC Close and Update UI Popup - Dark Theme")

### Customize Notification

In the **Mange default settings** option, you can customize the notification and add multiple languages under the **Localization** section.

![](/_images/Manage-Conflicting-Process-20.png)

You can add languages by selecting **Add/Remove** under **Localization** and then fill in the fields for those unique languages and their requirements. The language that is then chosen is based on the language of the machine in question at the time of the UI prompt opening.

![](/_images/Manage-Conflicting-Process-21.png)

![](/_images/Manage-Conflicting-Processes-22.gif)

> **Note:** Keep in mind that you **MUST** provide content for each selected language in each possible intended state. This means cycling through the drop downs under **Selected Language** and **Intent**. Otherwise you won’t be able to select the **OK** button.

> **Note:** You can add additional blank lines when modifying text in these boxes using

### When to enable: Notify the user to close the application

This option can help third-party products whose **installers will fail if the application is currently running** in the background by an end-user. The option to **notify the user to close the application** can be less impactful than the option to [**auto close application process before installation**](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#close-apps)**.**

This option is only needed for products that have issues updating while in use. The majority of products can update while in use, but may just require a reboot to fully apply.

Below is a list of products we are aware of that will generally fail to update while in use.

\[table id=22 /]

### Update in progress...

When the option is set to '**Prevent the end-user from opening an application while the application is updating**' we use the [Image File Execution Options](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/xperf/image-file-execution-options) feature to stop the application from opening. Example usage of this can be seen below.

![Image File Execution Options debugger](/_images/IFEO-Debugger.png "Image File Execution Options debugger")

The user would expect to see a message box that says '**An update is currently being installed on your computer. Please, do not try to start** ' or in some cases **'The requested operation requires elevation'** depending on the version of PatchMyPC-Scriptrunner.exe.

This message box would appear if the user attempts to **launch the software while it is being updated**. In some cases, **these registry entries may be left behind** if the Scriptrunner process is forcefully closed or otherwise closes unexpectedly. If this happens you may see the above messages even though no application installation or update is occurring. If this happens you can **delete the registry key named after the process**, such as the highlighted notepad++.exe registry key in the above image.

> **Note:** You will want to check both registry hives below depending on the architecture of the Operating System and the process which started ScriptRunner.
>
> HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\
> HKEY\_LOCAL\_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\Image File Execution Options

The following PowerShell code can be used to find and delete all Patch My PC created process blocking registry entries:

Get-Childitem "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options" -ea SilentlyContinue | Where {if($\_.Property -contains "Debugger"){($\_|Get-ItemProperty).Debugger -like "\*PreventStart\*"\}} | Remove-Item -Force -Recurse;\
Get-ChildItem "HKLM:\SOFTWARE\Microsoft\WOW6432Node\Windows NT\CurrentVersion\Image File Execution Options" -ea SilentlyContinue | Where {if ($\_.Property -contains "Debugger"){($\_|Get-ItemProperty).Debugger -like "\*PreventStart\*"\}} | Remove-Item -Force -Recurse;