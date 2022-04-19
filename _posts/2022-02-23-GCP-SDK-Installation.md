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

3. In your browser, log in to your Google user account when prompted and click Allow to grant permission to access Google Cloud resources.

4. Select a Google Cloud project from the list of projects

```
Pick cloud project to use:
 [1] project-123456
 [2] Create a new project
Please enter numeric choice or text value (must exactly match list item):
```

5. Run core commands to view information about your gcloud CLI installation

```
# Account information
gcloud auth list

# gcloud configuration property
gcloud config list

# gcloud information
gcloud info

```

6. kubectl installation

```
# components list
gcloud components list

# install kubectl
gcloud components install kubectl

```

7. set-up configuration

```
  125  gcloud config list
  126  kubectl
  127  gcloud components install kubectl
  128  kubectl config view
  129  gcloud container clusters get-credentials ###-kube
  130  gcloud config set compute/zone asia-northeast3-a
  131  kubectl config view
  132  gcloud container clusters get-credentials ###-kube
  133  kubectl get node
```









