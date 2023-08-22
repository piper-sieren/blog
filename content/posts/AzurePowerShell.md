+++
author = "Piper Williams"
title = "Use a Windows VM system-assigned managed identity to access Azure Storage via a SAS credential"
date = "2021-02-10"
description = "Use a Windows VM system-assigned managed identity to access Azure Storage via a SAS credential"
tags = [
    "PowerShell",
    "Azure",
]
categories = [
    "Azure",
    "PowerShell",
]
series = ["Azure"]
draft = false
+++

## Prerequisites
 
1. Assigning TLS1.2 to Storage Account and VM

2. Adding the VM to the firewall subnet inclusions

3. Create a System Managed Identity and assign to VM

4. Give that System Managed Identity RBAC over the storage account 



## Azure PowerShell Modules
 {{<highlight html>}}
 Install-Module AzureRM 

 Install-Module Azure -AllowClobber 

 Install-Module Az -AllowClobber

#AllowClobber flag removes the overlap between AzureRM, Az, and Azure PS Modules’ conflicting command names. 
{{</highlight>}}



## PowerShell Script
 
#### TLS1.2 can be set it the portal settings as well – following command must have Admin privs.

{{<highlight html>}}
  [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
{{</highlight>}}

**The purpose of these self-assigned link-local addresses is to facilitate communication with other hosts within the subnet even in the absence of external address configuration (via manual input or DHCP). Unlike in IPv6, implementation of IPv4 link-local addresses is recommended only in the absence of a normal, routable address.**
{{<highlight html>}}
$response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}

$content = $response.Content | ConvertFrom-Json

$ArmToken = $content.access_token


$params = @{canonicalizedResource="/<container name>/<storage account>/<blob name>";signedResource="c";signedPermission="rcw";signedProtocol="https";signedExpiry="<signed expiry>"}

$jsonParams = $params | ConvertTo-Json


$sasResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<subscription>/resourceGroups/<resource group> /providers/Microsoft.Storage/storageAccounts/<storage account>/listServiceSas/?api-version=2017-06-01 -Method POST -Body $jsonParams -Headers @{Authorization="Bearer $ArmToken"}


$sasContent = $sasResponse.Content | ConvertFrom-Json

$sasCred = $sasContent.serviceSasToken



$ctx = New-AzStorageContext -StorageAccountName a3dvcseburpstr01 -SasToken $sasCred

{{</highlight >}}
#### The following function uploads a blank test file to the blob container 
 {{<highlight html>}}
Set-AzStorageBlobContent -File test.txt -Container <container name> -Blob <blob name>-Context $ctx

{{</highlight>}}

## Terminology
 
**SAS Credential:** A Service SAS provides the ability to grant limited access to objects in a storage account, for limited time and a specific service (in our case, the blob service), without exposing an account access key.

**PowerShell Module:** A module is a package that contains PowerShell members, such as cmdlets, providers, functions, workflows, variables, and aliases. The members of this package can be implemented in a PowerShell script, a compiled DLL, or a combination of both. These files are usually grouped together in a single directory.

**Managed Identities:** On Azure, managed identities eliminate the need for developers having to manage credentials by providing an identity for the Azure resource in Azure AD and using it to obtain Azure Active Directory (Azure AD) tokens. This also helps accessing Azure Key Vault where developers can store credentials in a secure manner. Managed identities for Azure resources solve this problem by providing Azure services with an automatically managed identity in Azure AD.
#### TLS1.2: Why use TLS 1.2 with Configuration Manager?

TLS 1.2 is more secure than the previous cryptographic protocols such as SSL 2.0, SSL 3.0, TLS 1.0, and TLS 1.1. Essentially, TLS 1.2 keeps data being transferred across the network more secure.

## Reference Documents & Videos
 
1. [Getting started with PowerShell for Azure](https://www.youtube.com/watch?v=3Q0jG1Doa-s&t=596s)

2. [Tutorial: Use a Windows VM system-assigned managed identity to access Azure Storage via a SAS credential](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-storage-sas)