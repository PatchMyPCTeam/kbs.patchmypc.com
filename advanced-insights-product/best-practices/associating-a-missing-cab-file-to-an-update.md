# Associating a missing cab file to an Update

```yaml
---

taxonomy:
    products:
        - advanced-insights
    solutions:
        - application-management
    tech-stack:
        - itune
    post_tag:
        - guid
    sub-solutions:
        - test
        
---
```

In this article we outline how to associate a missing cab file from the WSUS Content folder with an Update in WSUS/ConfigMgr. Often an ADR will display the error code 0x87D20417 after evaluation and missing content is often the cause. Alternatively, manually trying to download missing update content in ConfigMgr might present the error “Error: Failed to download content id xxxxxx. Error: The cloud file provider exited unexpectedly”.

It might be a single update that is missing content that will cause the rule engine to throw this error. Identifying the update and declining or re-publishing it is the way to resolve the error. This article will help you identify which update your should decline or re-publish.

> **Note:** Additional troubleshooting help for error code 0X87D20417 and 0X80070194 can be found in the following KB [Automatic Deployment Rule Error 0X87D20417 or 0x80070194 – Patch My PC](https://patchmypc.com/automatic-deployment-rule-adr-error-0x87d20417-or-0x80070194)

### Determine if you are affected <a href="#h-determine-if-you-are-affected" id="h-determine-if-you-are-affected"></a>

When you attempt to download update content in ConfigMgr, you may encounter the following error.

**Scenario 1: Using an Automatic Deployment Rule (ADR)**

The ADR will display the error code 0X87D20417 “Auto Deployment Rule download failed”.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/missing_wsus_content_1.png" alt=""><figcaption></figcaption></figure>

Additionally, RuleEngine.log will display the following error.

Failed to download the update content with ID xxxxxxxx from internet. Error = 404\
Failed to download ContentID xxxxxxxx for UpdateID xxxxxxxx. Error code = 404\
Failed to download any update\
Failed to download update contents.\
Failed to run the DownloadAction for the AutoDeployment.\
STATMSG: ID=8706 SEV=E LEV=M SOURCE=”SMS Server” COMP=”SMS\_RULE\_ENGINE” SYS=CM.constoso.LOCAL SITE=CM1 PID=3520 TID=8116 GMTDATE=Mon Oct 14 08:21:46.082 2024 ISTR0=”SMS Rule Engine” ISTR1=”Failed to download one or more content files” ISTR2=”” ISTR3=”” ISTR4=”” ISTR5=”” ISTR6=”” ISTR7=”” ISTR8=”” ISTR9=”” NUMATTRS=0 LE=0X0

<figure><img src="https://patchmypc.com/app/uploads/2025/04/missing_wsus_content_2.png" alt=""><figcaption></figcaption></figure>

Additionally, PatchDownloader.log will display the following.

HttpSendRequest failed HTTP\_STATUS\_NOT\_FOUND\
ERROR: DownloadUpdateContent() failed with hr=0x80070194

<figure><img src="https://patchmypc.com/app/uploads/2025/04/missing_wsus_content_3.png" alt=""><figcaption></figcaption></figure>

**Scenario 2: Manually downloading the Software update**

The download software updates wizard will display the following message “Error: Failed to download content id xxxxxx. Error: The cloud file provider exited unexpectedly.”

<figure><img src="https://patchmypc.com/app/uploads/2025/04/missing_wsus_content_4.png" alt=""><figcaption></figcaption></figure>

Additionally, PatchDownloader.log will display the following.

HttpSendRequest failed HTTP\_STATUS\_NOT\_FOUND\
ERROR: DownloadUpdateContent() failed with hr=0x80070194

<figure><img src="https://patchmypc.com/app/uploads/2025/04/missing_wsus_content_3.png" alt=""><figcaption></figcaption></figure>

### Identify a missing update file name <a href="#h-identify-a-missing-update-file-name" id="h-identify-a-missing-update-file-name"></a>

PatchDownloader.log reveals the name of the content that was not found.

Look for a line containg “**Contentsource =”** just before the log line where the download fails.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/missing_wsus_content_5.png" alt=""><figcaption></figcaption></figure>

In the example above, the missing cab file is called **316611FD021A120AE9FC6A3534A56DF7656F61F5.cab**

We can verify if the content is missing by browsing to the URL for the content and getting the same 404 status message.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/missing_wsus_content_6.png" alt=""><figcaption></figcaption></figure>

### Manually match the missing content to an Update <a href="#h-manually-match-the-missing-content-to-an-update" id="h-manually-match-the-missing-content-to-an-update"></a>

PatchDownloader.log reveals the name of the content that was not found.

Look for a line containg “**Download destination =”** just before the log line where the download fails.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/missing_wsus_content_13.png" alt=""><figcaption></figcaption></figure>

In the example above, the **Download destination =**

**cm2.lab2.localPackagesPMPC\_Updatescdbaf35-3ffd-4e3a-a8d9-c14ebea7173b.132b7c379-3210-4b61-abe4-cdcfb04284ff\_1.cab**

ConfigMgr attempts to download the update content into a sub folder in the package root directory. The sub folder name is also the Update ID. In the example above, the Update ID is:-

_cm2.lab2.localPackagesPMPC\_Updates**0cdbaf35-3ffd-4e3a-a8d9-c14ebea7173b**.132b7c379-3210-4b61-abe4-cdcfb04284ff\_1.cab_

Note: Revision information is suffixed to the folder name and this can be ignored. The Update ID from the example above is:-

**0cdbaf35-3ffd-4e3a-a8d9-c14ebea7173b**

> **Note:** You should identify all the missing .cab files referenced in PatchDownloader.log and record the ContentSource URL. We will use the URL(s) and .cab name(s) in the following steps.

### Use SQL Server Management Studio to match the missing content to an update <a href="#h-use-sql-server-management-studio-to-match-the-missing-content-to-an-update" id="h-use-sql-server-management-studio-to-match-the-missing-content-to-an-update"></a>

Once you have gathered a list of the missing cab file(s) in the previous step, you can run a query in SQL Server Management Studio (SSMS) to match the content to an update in the WSUS database.

1\. Open SSMS

2\. Select “New Query”

3\. Enter the following query:-

> ```
> -- Create a temporary table to store the CAB file names as strings
> CREATE TABLE #TempCabFiles (FileDigestString NVARCHAR(100));
>
> -- Insert CAB file names
> INSERT INTO #TempCabFiles (FileDigestString)
> VALUES
> ('316611FD021A120AE9FC6A3534A56DF7656F61F5.cab'),
> ('F2186E510EA4BD76B93FFBBD686D50D9769F3351.cab');
>
> -- Create another temporary table to store the binary representation
> CREATE TABLE #TempCabFilesBinary (FileDigest VARBINARY(20));
>
> -- Insert values into the binary table, dynamically removing .cab and converting to varbinary
> INSERT INTO #TempCabFilesBinary (FileDigest)
> SELECT CONVERT(VARBINARY(20), REPLACE(FileDigestString, '.cab', ''), 2)
> FROM #TempCabFiles;
>
> -- Use the SUSDB database (assuming this is where your data is stored)
> USE SUSDB;
>
> -- Query to match the CAB files and display binary values as hex strings
> SELECT pu.DefaultTitle,
> u.UpdateID,
> f.FileDigest,
> sys.fn_varbintohexstr(f.FileDigest) AS FileDigestHex,
> f.FileName
> FROM tbFile f
> INNER JOIN tbFileForRevision fr ON f.FileDigest = fr.FileDigest
> INNER JOIN tbRevision r ON fr.RevisionID = r.RevisionID
> INNER JOIN tbUpdate u ON r.LocalUpdateID = u.LocalUpdateID
> INNER JOIN PUBLIC_VIEWS.vUpdate pu ON pu.UpdateId = u.UpdateID
> WHERE f.FileDigest IN (SELECT FileDigest FROM #TempCabFilesBinary);
>
> -- Drop the temporary tables
> DROP TABLE #TempCabFiles;
> DROP TABLE #TempCabFilesBinary;
> ```

If querying a single .cab file, the single list item should have a semi-colon appended, indicating the end of the list of values. e.g.(‘**F2186E510EA4BD76B93FFBBD686D50D9769F3351.cab**‘);

If querying multiple .cab files, each list item should be seperate by a comma and the last list item should have a semi-colon appended, indicating the end of the list of values. e.g.\
(‘**316611FD021A120AE9FC6A3534A56DF7656F61F5.cab**‘),\
(‘**F2186E510EA4BD76B93FFBBD686D50D9769F3351.cab**‘);

The Results pane will indicate which updates were associated with the missing cab file(s).

<figure><img src="https://patchmypc.com/app/uploads/2025/04/missing_wsus_content_7.png" alt=""><figcaption></figcaption></figure>

### Use ConfigMgr to match the missing content to an Update <a href="#h-use-configmgr-to-match-the-missing-content-to-an-update" id="h-use-configmgr-to-match-the-missing-content-to-an-update"></a>

Once you have gathered a list of missing cab file(s) in the previous step, you can run the following ConfigMgr SDK cmdlet to match the missing content to an update.

1\. From the ConfigMgr console, choose to **Connect via Windows PowerShell**

<figure><img src="https://patchmypc.com/app/uploads/2025/04/missing_wsus_content_8.png" alt=""><figcaption></figcaption></figure>

2\. Define a PowerShell list containg the URL(s) of the missing content referenced in PatchDownloader.log. e.g.

> $sourceURLs = @(\
> ‘http://sus01.lab2.local:8530/Content/F5/316611FD021A120AE9FC6A3534A56DF7656F61F5.cab’, ‘http://sus01.lab2.local:8530/Content/51/F2186E510EA4BD76B93FFBBD686D50D9769F3351.cab’\
> )

Run the following snippet (This is a single line PowerShell command).

> $updateIds = Get-CMSoftwareUpdate -fast | Where-Object { $\_.LocalizedCategoryInstanceNames -match “SCUP Updates” } | Select-Object -ExpandProperty CI\_ID; $CMPSSuppressFastNotUsedCheck = $true; $updateIds | ForEach-Object { $fullUpdate = Get-CMSoftwareUpdate -Id $\_; $sdmPackageXml = $fullUpdate.SDMPackageXML; $updateId = $fullUpdate.CI\_UniqueID; $sourceUrl = $sdmPackageXml.DesiredConfigurationDigest.SoftwareUpdate.ConfigurationMetadata.Provider.Operation.Content.FileContent.SourceURL; if ($sourceURLs -contains $sourceUrl) { $fullUpdate | Select-Object LocalizedDisplayName, @{Name=’UpdateId’;Expression={$updateId\}}, @{Name=’SourceURL’;Expression={$sourceUrl\}} | Format-List } }; $CMPSSuppressFastNotUsedCheck = $false

The output will list the Update(s) and Update Id(s) for the missing content.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/missing_wsus_content_9-1.png" alt=""><figcaption></figcaption></figure>

### Find the Update and Decline or Re-Publish <a href="#h-find-the-update-and-decline-or-re-publish" id="h-find-the-update-and-decline-or-re-publish"></a>

Now that you have identified the update(s) with missing content, you can find them in the ConfigMgr console by searching for the Unique Update ID.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/missing_wsus_content_10.png" alt=""><figcaption></figcaption></figure>

If this is the latest version of the update available in the Patch My PC catalog, you can simply [re-publish the update](https://patchmypc.com/when-and-how-to-republish-third-party-updates) to restore the missing content in the WSUS Content folder.

If this is an older version of an update or you no longer need it because it is the reason your ADR is failing, you should decline it from WSUS via the Patch My PC Publisher using the following steps.

1\. Open the Publisher.

2\. From the “Updates” tab, click “Options”.

3\. Click “Run Wizard”.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/missing_wsus_content_11.png" alt=""><figcaption></figcaption></figure>

4\. Search for and select the update(s) with missing content and click **Decline**.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/missing_wsus_content_12.png" alt=""><figcaption></figcaption></figure>

5\. Perform a Software update Polint Sync for the update to be marked as expired in ConfigMgr.

6\. Re-run the failed ADR(s).
