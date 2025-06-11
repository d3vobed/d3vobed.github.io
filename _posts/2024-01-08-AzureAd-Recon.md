---
layout: post
title: "AD Recon with Azure AD "
description: "Collecting information in Entra ID with AzureADRecon and hunting with Microsoft Sentinel!"
tags: [azure, scripting, AD, threat-hunting]
image: https://github.com/tomwechsler/Active_Directory_Advanced_Threat_Hunting/raw/main/Azure_Active_Directory/Images/sen_5.png
---

## AzureADRecon

> **Note: The AzureADRecon tool is provided by Prashant Mahajan (@prashant3535), thanks for that!**  
https://github.com/adrecon/AzureADRecon

**Installing:** 

Download the tool, the easiest way is to save the .zip file right away.  

![Download](/assets/aad_0.png)

> Note: **Attention: It is possible that the antimalware program reacts during the download**  

If you have git installed, you can start by cloning the repository:  

git clone https://github.com/adrecon/AzureADRecon.git

If you downloaded the tool using a zip file, extract the zip file and place it in a location that you can easily find again. If you cloned the repository, a folder was created directly.
Now launch PowerShell or Windows Terminal, whichever you prefer, and navigate to the extract/clone folder.

![Download](/assets/wt_1.png)

In order to get started we need one more prerequisite, in my case the PowerShell AzureAD module. However, you are welcome to work with the Microsoft Graph, but this requires additional preparations afterwards.

```
Install-Module AzureAD -Verbose -Force -Allowclobber
```
![Download](/assets/wt_2.png)

Don't forget we need to adjust the execution policy in PowerShell!

```
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
```
![Download](/assets/wt_3.png)


> **Note: In order to work with this tool, you need to work with an account that has sufficient rights in Entra ID.**  

**To run AzureADRecon (will prompt for credentials)**  

```
PS C:\AzureADRecon-master> .\AzureADRecon.ps1
```

or

```
PS C:\AzureADRecon-master> $username = "your user principal name"
PS C:\AzureADRecon-master> $passwd = ConvertTo-SecureString "your password" -AsPlainText -Force
PS C:\AzureADRecon-master> $creds = New-Object System.Management.Automation.PSCredential ($username, $passwd)
PS C:\AzureADRecon-master> .\AzureADRecon.ps1 -Credential $creds
```

> **Note: To get the report as a spreadsheet, Excel must be installed on the system.**  

**The report is created in the same folder**  
![Download](/assets/wt_4.png)


**Now open the report and start the investigation and analysis**  
![Download](/assets/aad_1.png)


**User Stats**
![Download](/assets/aad_2.png)


**Users**  
![Download](/assets/aad_3.png)


**Directory Roles**  
![Download](/assets/aad_4.png)


**Directory Roles Members**  
![Download](/assets/aad_5.png)


**Devices**  
![Download](/assets/aad_6.png)


## Hunting with Microsoft Sentinel

Now we have detailed information from the Microsoft client. The information was not collected just like that, but because there was a suspicion. Now we continue with advanced hunting in Microsoft Sentinel.

In Microsoft Sentinel, we can directly access the incidents from the overview.

![Download](/assets/sen_1.png)

**List of incidents**
![Download](/assets/sen_2.png)


**View full incident details**
![Download](/assets/sen_3.png)


**Now the deep dive into the incident**
![Download](/assets/sen_4.png)


**Investigate each incident**
![Download](/assets/sen_5.png)


---
## *HAPPY INVESTIGATING!*
---