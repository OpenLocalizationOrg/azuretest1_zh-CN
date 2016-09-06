---
ms.openlocfilehash: d070b28f6336ccceeef5c3d6f1be214d346d85dd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
  pageTitle="设置和检索属性和元数据的存储资源 |Microsoft Azure" 
  description="了解如何设置和检索属性和 Azure 存储资源的元数据。" 
  services="storage" 
  documentationCenter="" 
  authors="tamram" 
  manager="adinah" 
  editor=""/>

<tags 
  ms.service="storage" 
  ms.workload="storage" 
  ms.tgt_pltfrm="na" 
  ms.devlang="na" 
  ms.topic="article" 
  ms.date="08/04/2015" 
  ms.author="tamram"/>


# 设置和检索属性和元数据 #

## 概述

在 Azure 存储支持系统属性和用户定义的元数据，以及它们包含的数据的对象︰

*   **系统属性。** 系统属性存在于每个存储资源。 它们中的一些可以读取或设置，其他的是只读的。 在幕后，一些系统属性对应于某些标准的 HTTP 标头。 Azure 存储客户端库维护这些为您。  

*   **用户定义的元数据。** 用户定义的元数据是元数据，您可以在给定的资源，请在名称-值对的形式指定。 您可以使用元数据中存储其他值与存储资源;这些值只是为了自己，并不会影响资源的行为。  

## 设置和检索属性

检索存储资源的属性和元数据值是一个两步过程。 可以读取这些值之前，您必须通过调用**FetchAttributes**或**FetchAttributesAsync**方法显式获取它们。

> [AZURE.IMPORTANT] 除非您调用一个**FetchAttributes**方法才填充存储资源的属性和元数据值。 

要在一个 blob 设置属性，指定属性值，然后调用**SetProperties**或**SetPropertiesAsync** 。

下面的代码示例创建一个容器，并将属性值写入到控制台窗口中︰

    //Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));
    
    //Create the service client object for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container. 
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it does not already exist.
    container.CreateIfNotExists();

    // Fetch container properties and write out their values.
    container.FetchAttributes();
    Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
    Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
    Console.WriteLine("ETag: {0}", container.Properties.ETag);
    Console.WriteLine();

## 设置和检索元数据

可以为资源 blob 或容器上的一个或多个名称-值对指定元数据。 要设置的元数据，请将名称 / 值对添加到**元数据**集合资源，然后调用**SetMetadata**方法将值保存到该服务。

> [AZURE。请注意]: The name of your metadata must conform to the naming conventions for C# identifiers.
 
要检索的元数据，请调用**FetchAttributes**方法在 blob 或容器来填充该**元数据**集合，然后读取值。

下面的代码示例创建一个新容器并设置元数据，然后读取返回的元数据值︰

         
    // Account name and key.  Modify for your account.
    <span style="color:Blue;">string accountName = <span style="color:#A31515;">"myaccount";
    <span style="color:Blue;">string accountKey = <span style="color:#A31515;">"SzlFqgzqhfkj594cFoveYqCuvo8v9EESAnOLcTBeBIo31p16rJJRZx/5vU/oY3ZsK/VdFNaVpm6G8YSD2K48Nw==";

    // Get a reference to the storage account and client with authentication credentials.
    StorageCredentials credentials = <span style="color:Blue;">new StorageCredentials(accountName, accountKey);
    CloudStorageAccount storageAccount = <span style="color:Blue;">new CloudStorageAccount(credentials, <span style="color:Blue;">true);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container. 
    CloudBlobContainer container = blobClient.GetContainerReference(<span style="color:#A31515;">"mycontainer");

    // Create the container if it does not already exist.
    container.CreateIfNotExists();

    // Set metadata for the container.
    container.Metadata[<span style="color:#A31515;">"category"] = <span style="color:#A31515;">"images";
    container.Metadata[<span style="color:#A31515;">"owner"] = <span style="color:#A31515;">"azure";
    container.SetMetadata();

    // Get container metadata.
    container.FetchAttributes();
    <span style="color:Blue;">foreach (<span style="color:Blue;">string key <span style="color:Blue;">in container.Metadata.Keys)
    {
       Console.WriteLine(<span style="color:#A31515;">"Metadata key: " + key);
       Console.WriteLine(<span style="color:#A31515;">"Metadata value: " + container.Metadata[key]);
    }

    //Clean up.
    container.Delete();

## 请参见  

- [Azure 存储客户端库参考](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)
- [开始使用 Blob 存储.net](storage-dotnet-how-to-use-blobs.md)  
 