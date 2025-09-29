# Adding Third-Party Software Update Catalog Fails with Error 12045 / 0x80072f0d in SCCM

```yaml
---

taxonomy:
    products:
        - 
    solutions:
        - 
    tech-stack:
        - configmgr
    post_tag:
        - 
    sub-solutions:
        - troubleshooting
        - connectivity-and-proxy-issues
        - security
        
---
```

We have seen an increase of customers receiving failures when attempting to subscribe to our third-party update catalog in the third-party software updates catalog node of the Configuration Manager console.

In this scenario, the console will return an error that says something similar to **Unable to create the subscription. The console failed to download from because of error code 12045. For more information, see SmsAdminUI logfile.**

![DownloadContentFiles() failed with hr=0x80072f0d](/_images/DownloadContentFiles-failed-with-hr0x80072f0d.png "DownloadContentFiles() failed with hr=0x80072f0d")

When you click the option to **Subscribe to Catalog** in the Configuration Manager, the console will attempt to download the catalog file from the URL you entered in the catalog URL. To better understand the error 12045, we need to review the patchdownloader.log within the %temp% folder of the user running the Configuration Manager console.

If you review the **patchdownloader.log**, and you see errors similar to that shown below:

![](/_images/HttpSendRequest-failed-12045.png)

HttpSendRequest failed 12045, can still rety for 3 times\
FileHash value is NULL. Hash verification for this file will not be performed.\
ERROR: DownloadContentFiles() failed with hr=**0x80072f0d**

In this scenario, the **error code 12045** is actually a generic code from patchdownloader.log. The actual information that will provide value for troubleshooting is the logline: **ERROR: DownloadContentFiles() failed with hr=0x80072f0d**

In the CMTrace.exe tool, you can click Control + L to perform an error code lookup. Error code 0x80072f0d translates to Winhttp error: **The certificate authority is invalid or incorrect**

![](/_images/winhttp-0x80072f0d.png)

On the server running Configuration Manager Console or the Patch My PC Publisher tool, open up **Internet Explorer** and navigate to [https://patchmypc.com/](https://patchmypc.com/). In the address bar, check the certificate lock to see if the SSL certificate is trusted.

Click the **lock icon** then click **View certificate**

![SSL View Certificate IE](/_images/SSL-View-Certificate-IE.png "SSL View Certificate IE")

Click the Certificate Path and validate the certificate for patchmypc.com is trusted.

![SSL View Certification Path](/_images/SSL-View-Certification-Path.png "SSL View Certification Path")

The Winhttp error **0x80072f0d,** generally means the root certificate that issued the SSL certificate for the website the HTTP download connection is being made to is not trusted. For **patchmypc.com**, we use a common public certificate authority **DigiCert**. If this certificate isn’t trusted, you will need to import all the issuing certificates into the **Trusted Root** certificate store on the computer,