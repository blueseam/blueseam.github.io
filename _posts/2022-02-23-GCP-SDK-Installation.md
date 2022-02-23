---
layout: post
title:  "Google SDK Installation for windows"
categories: [ blueseam, tutorial ]
image: assets/images/h3.png
---

# Google SDK Installation Notes

1. Download the Google Cloud CLI installer.<br/>
   (https://dl.google.com/dl/cloudsdk/channels/rapid/GoogleCloudSDKInstaller.exe)

Alternatively, open a PowerShell terminal and run the following PowerShell commands:
```
(New-Object Net.WebClient).DownloadFile("https://dl.google.com/dl/cloudsdk/channels/rapid/GoogleCloudSDKInstaller.exe", "$env:Temp\GoogleCloudSDKInstaller.exe")

& $env:Temp\GoogleCloudSDKInstaller.exe
```

![Install GCloud SDK](/assets/images/z1.png)


2. Initialize the gcloud

```
gcloud init
```



