+++
author = "Piper Williams"
title = "Azure Blob Storage + .NET Client Library"
date = "2021-02-21"
description = "Azure Blob Storage + .NET Client Library"
tags = [
    ".Net",
    "Azure",
    "Blob Storage"
]
series = ["Azure"]
+++

Azure offers Blob Storage as the cloud storage solution. Blob Storage is a way of storing substantial amounts of unstructured data: data that doesn't adhere to a specific data model or definition- data that can't go into rows and columns. For example, video, audio, image, log files, text files, etc. Azure Blob Storage has three types of resources to offer: 

1. The Storage Account 
2. Container(s) within the storage account 
3. Blob(s) within the Container

## Prerequisites

1. Azure Subscription 
2. Azure Storage Account 
3. The current .Net Core SDK for your OS

First, you will need to set up a .NET Core project. Do this with whichever method you please. I typically use Visual Studios, but some prefer doing this directly in their Terminal of choice. 


Next, you will need to connect your Storage Account to your .NET Core project. Do this by generating an Access Key inside your Azure Storage Account. Copy the "Connection String" value generated for key1. 

Then, set this value as an environment variable named "AZURE_STORAGE_CONNECTION_STRING". An easy way to set an environment variable without navigating through your system settings is to use your OS Command Prompt. For windows, that would be the CMD with the following syntax: 

{{<highlight html>}}
set AZURE_STORAGE_CONNECTION_STRING <yourconnectionstring>
{{</highlight>}}



![image info](https://static.wixstatic.com/media/6a8247_b6f9eef8b5844201a844e0a4d3e8ba06~mv2.png/v1/fill/w_998,h_262,al_c,lg_1,q_85,enc_auto/6a8247_b6f9eef8b5844201a844e0a4d3e8ba06~mv2.png)

.Net classes to interact with Azure Storage Resources

**BlobServiceClient:** manipulate Azure Storage resources and blob containers

**BlobContainerClient:** manipulate Azure Storage containers and their blobs

**BlobClient:** manipulate Azure Storage Blobs

**BlobDownloadInfo:** Represent the properties and content returned from downloading a Blob

#### Tip: 
Retrieve the Azure Storage Account's Connection String inside your .NET

 project- From your previously set environment variables. 

{{<highlight html>}}
string connectionString = Environment.GetEnvironmentVariable("AZURE_STORAGE_CONNECTION_STRING");
{{</highlight>}}


#### Tip: 
Add the Azure Blob Storage .Net Client Library with the using statement. 
{{<highlight html>}}
using Azure.Storage;

using Azure.Storage.Blobs;

using Azure.Storage.Blobs.Models;
{{</highlight>}}


## Code Examples:

#### Retrieve the Azure Store Account’s Connection String – Environment Variable: 
{{<highlight html>}}
string connectionString = Environment.GetEnvironmentVariable("AZURE_STORAGE_CONNECTION_STRING");
{{</highlight>}}

#### List Blobs in a Container: 
{{<highlight html>}}
BlobContainerClient blobContainerClient = new BlobContainerClient(connectionString, "burpgrid");


Console.WriteLine("Listing blobs...");

var blobnames = new List<string>();

// List all blobs in the container

await foreach (BlobItem blobItem in blobContainerClient.GetBlobsAsync())

{

Console.WriteLine("\t" + blobItem.Name);

blobnames.Add(blobItem.Name);

}
{{</highlight>}}


#### Download Contents from each Blob in a Container to Local: 
{{<highlight html>}}
foreach (string blobname in blobnames)

{

 

string localFilePath = Path.Combine(localPath, blobname);

// Connect to each blob client

BlobClient blobClient = blobContainerClient.GetBlobClient(blobname);


// Download the blob's contents and save it to a file

BlobDownloadInfo download = await blobClient.DownloadAsync();


using (FileStream downloadFileStream = File.OpenWrite(localFilePath))

{

await download.Content.CopyToAsync(downloadFileStream);

downloadFileStream.Close();

}

}
{{</highlight>}}

#### Clear Local File Path: 
{{<highlight html>}}
//Delete all files in local file path

string localPath = “/.data/”

string[] filePaths = Directory.GetFiles(localPath);

foreach (string filePath in filePaths)

File.Delete(filePath);
{{</highlight>}}
#### Open and Read File Contents to Class: 
{{<highlight html>}}
//Open and read files 

foreach (string blobname in blobnames)

{

string localFilePath = Path.Combine(localPath, blobname);

string text = System.IO.File.ReadAllText(localFilePath);

 //Convert to saved JSON

 var scan = JsonConvert.DeserializeObject<ScanResults>(text);

 Console.WriteLine("issue counts: " + scan.issue_counts.total);

 Console.WriteLine("site name: " + scan.site_name);
}
{{</highlight>}}


## Additional resources:
1. [API reference documentation](https://learn.microsoft.com/en-us/dotnet/api/azure.storage.blobs?view=azure-dotnet)

2. [Library source code](https://github.com/Azure/azure-sdk-for-net/tree/main/sdk/storage/Azure.Storage.Blobs)

3. [Package (NuGet)](https://www.nuget.org/packages/Azure.Storage.Blobs)

4. [Samples](https://learn.microsoft.com/en-us/azure/storage/common/storage-samples-dotnet?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json#blob-samples)

5. [Microsoft Documentation](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-dotnet)

