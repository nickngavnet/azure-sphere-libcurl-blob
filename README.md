﻿# Upload file to Azure Blob using libcurl

This sample demonstrates uploading a text file to Azure Blob service from Azure Sphere using libcurl to initiate [Put Blob](https://docs.microsoft.com/en-us/rest/api/storageservices/put-blob) REST API with [SAS authorization](https://docs.microsoft.com/en-us/rest/api/storageservices/delegate-access-with-shared-access-signature). 

## Configure Azure Blob and generate service SAS

1. To run this demo you need have a [storage account](https://docs.microsoft.com/en-us/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal) and create a [blob container](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal)
2. Download [Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/) and login with your Azure AD
3. In Storage Explorer, navigate to your container icon and right click to **Get Shared Access Signature**, create a Ad-hoc Service SAS with at least *Create* and *Write* permission. Copy **Query String**

## To build and run the sample

### Prep your device

1. Ensure that your Azure Sphere device is connected to your PC, and your PC is connected to the internet.
2. Even if you've performed this set up previously, ensure that you have Azure Sphere SDK version 19.09 or above. In an Azure Sphere Developer Command Prompt, run **azsphere show-version** to check. Download and install the [latest SDK](https://aka.ms/AzureSphereSDKDownload) as needed.
3. Open Azure Sphere Developer Command Prompt and issue the following command:

   ```
   azsphere dev prep-debug
   ```


### Build and deploy the application

1. Start Visual Studio.
2. Select the file azure-sphere-libcurl-blob.sln and then click **Open**. 
3. In main.c file, replace FileURI string with your **stroage account name** and SASToken string with your **Query String** created.
4. In app_manifest.json file, fill your own **stroage account name** in AllowedConnections capability. 
5. Press **F5** to build and debug the application
6. In **Device Output** window, you will observe below logs:
   
   ```
    Example to upload a file to Azure Blob Service by using libcurl and REST API
    *   Trying 52.239.128.36...
    * TCP_NODELAY set

    * Connected to <your-storage-account>.blob.core.windows.net (52.239.128.36) port 443 (#0)
    * successfully set certificate verify locations:
    *   CAfile: none
    CApath: /etc/certs/
    * ALPN, offering http/1.1

    * ALPN, server did not agree to a protocol
    * SSL connection using TLSv1.2 / ECDHE-RSA-AES256-GCM-SHA384
    > PUT /img/hello4.txt?se=2019-10-31T14%3A32%3A00Z&sp=w&sv=2018-03-28&sr=c&sig=eOHVGesikAYMDeJfysweiSdySbTILa12EowRJuEJbkuk%3D HTTP/1.1
    Host: <your-storage-account>.blob.core.windows.net
    Accept: */*
    x-ms-blob-type:BlockBlob
    Content-Length: 11
    Expect: 100-continue

    < HTTP/1.1 100 Continue
    * We are completely uploaded and fine

    < HTTP/1.1 201 Created
    < Content-Length: 0
    < Content-MD5: sQqNsWTgdUEFt6mb5y4/5Q==
    < Last-Modified: Wed, 23 Oct 2019 14:47:53 GMT
    < ETag: "0x8D757C7FC0FA29E"
    < Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
    < x-ms-request-id: a2a830e8-801e-0014-51b0-894e2f000000
    < x-ms-version: 2018-03-28
    < x-ms-request-server-encrypted: true
    < Date: Wed, 23 Oct 2019 14:47:52 GMT
    < 
    * Connection #0 to host <your-storage-account>.blob.core.windows.net left intact
   ```

7. Go to Azure Storage Explorer and double click your container, a *hello.txt* will be listed. Double click the file to open and inspect the content: **Hello World**