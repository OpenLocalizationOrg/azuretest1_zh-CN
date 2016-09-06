---
ms.openlocfilehash: 690d6f3295a8079d3e9863b3da4f20c233ff9c70
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="复制到介质服务资产的现有的斑点" 
    description="本主题演示如何将现有的 blob 复制到介质服务资产。" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="08/11/2015"
    ms.author="juliako"/>

#复制到介质服务资产的现有的斑点

本主题演示如何将 blob 存储帐户复制到新的 Microsoft Azure 媒体服务资产。

您 blob 可能存在于介质服务帐户或不与媒体服务帐户相关联的存储帐户相关联的存储帐户。 本主题演示如何将 blob 存储帐户复制到介质服务资产。 请注意，您还可以复制跨数据中心。 但是，可能会通过这种方式所产生费用。 有关定价的详细信息，请参阅[数据传输](http://azure.microsoft.com/pricing/#header-11)。

>[AZURE.NOTE] 您不应尝试更改所带来的媒体服务，而无需使用介质服务 Api 的 blob 容器的内容。

##先决条件

- 在新的或现有的 Azure 订阅中的两个介质服务帐户。 请参阅[如何创建介质服务帐户](media-services-create-account.md)主题。
- 操作系统︰ Windows 7，Windows 2008 R2 或 Windows 8。
- .NET framework 4.5。
- Visual Studio 2013年，Visual Studio 2012 或 Visual Studio 2010 SP1 （专业、 特优、 终极，或速成版）。

##设置项目

在本节中将创建并设置 C# 控制台应用程序项目。

1. 使用 Visual Studio 2012 或 Visual Studio 2010 SP1 创建新的解决方案，其中包含 C# 控制台应用程序项目。 
2. CopyExistingBlobsIntoAsset 输入名称，然后单击确定。
1. 使用 Nuget 将引用添加到介质服务相关的 Dll。 在 Visual Studio 主菜单中，选择工具-> 库程序包管理器-> 程序包管理器控制台。 在控制台窗口类型安装软件包 windowsazure.mediaservices 按输入。
1. 添加所需的此项目的其他引用︰ System.Configuration。
1. 在使用已添加到 Programs.cs 文件中默认情况下，使用下面的语句替换︰
        
        using System;
        using System.Linq;
        using System.Configuration;
        using System.IO;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using System.Collections.Generic;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure;
        using System.Web;
        using Microsoft.WindowsAzure.Storage.Blob;
        using Microsoft.WindowsAzure.Storage.Auth;

1. .Config 文件中添加的 appSettings 部分和更新基于您的媒体服务和存储密钥的值和名称值。 

        <appSettings>
          <add key="MediaServicesAccountName" value="Media-Services-Account-Name"/>
          <add key="MediaServicesAccountKey" value="Media-Services-Account-Key"/>
          <add key="MediaServicesStorageAccountName" value="Media-Services-Storage-Account-Name"/>
          <add key="MediaServicesStorageAccountKey" value="Media-Services-Storage-Account-Key"/>
          <add key="ExternalStorageAccountName" value="External-Storage-Account-Name"/>
          <add key="ExternalStorageAccountKey" value="External-Storage-Account-Key"/>
        </appSettings>


##复制到介质服务资产的存储帐户中的 Blob

下面的代码示例执行以下任务︰

1. 创建 CloudMediaContext 实例。 
1. 创建 CloudStorageAccount 实例︰ _sourceStorageAccount 和 _destinationStorageAccount。
1. 将平滑流式处理的文件从本地目录上载到位于 _sourceStorageAccount 的 blob 容器。 
1. 创建新的资产。 对于这项资产，它创建 blob 容器位于 _destinationStorageAccount。 
1. 使用 Azure 存储 SDK 将指定的 blob 复制到与该资产相关的容器。

>[AZURE.NOTE]复制操作定位程序已经过期时不引发异常。

1. 设置.ism 文件的主文件。
1. 为与该资产相关联的 OnDemandOrigin 定位器创建平滑流式传输的 URL。 
        
        class Program
        {
            // Read values from the App.config file. 
            static string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
            static string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];
            static string _storageAccountName = ConfigurationManager.AppSettings["MediaServicesStorageAccountName"];
            static string _storageAccountKey = ConfigurationManager.AppSettings["MediaServicesStorageAccountKey"];
            static string _externalStorageAccountName = ConfigurationManager.AppSettings["ExternalStorageAccountName"];
            static string _externalStorageAccountKey = ConfigurationManager.AppSettings["ExternalStorageAccountKey"];
        
            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;
        
            private static CloudStorageAccount _sourceStorageAccount = null;
            private static CloudStorageAccount _destinationStorageAccount = null;
        
            static void Main(string[] args)
            {
                _cachedCredentials = new MediaServicesCredentials(
                                _accountName,
                                _accountKey);
                // Use the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
        
                // In this example the storage account from which we copy blobs is not 
                // associated with the Media Services account into which we copy blobs.
                // But the same code will work for coping blobs from a storage account that is 
                // associated with the Media Services account.
                //
                // Get a reference to a storage account that is not associated with a Media Services account
                // (an external account).  
                StorageCredentials externalStorageCredentials =
                    new StorageCredentials(_externalStorageAccountName, _externalStorageAccountKey);
                _sourceStorageAccount = new CloudStorageAccount(externalStorageCredentials, true);
        
                //Get a reference to the storage account that is associated with a Media Services account. 
                StorageCredentials mediaServicesStorageCredentials =
                    new StorageCredentials(_storageAccountName, _storageAccountKey);
                _destinationStorageAccount = new CloudStorageAccount(mediaServicesStorageCredentials, false);
        
                // Upload Smooth Streaming files into a storage account.
                string localMediaDir = @"C:\supportFiles\streamingfiles";
                CloudBlobContainer blobContainer =
                    UploadContentToStorageAccount(localMediaDir);
        
                // Create a new asset and copy the smooth streaming files into 
                // the container that is associated with the asset.
                IAsset asset = CreateAssetFromExistingBlobs(blobContainer);
        
                // Get the streaming URL for the smooth streaming files 
                // that were copied into the asset.   
                string urlForClientStreaming = CreateStreamingLocator(asset);
                Console.WriteLine("Smooth Streaming URL: " + urlForClientStreaming);
        
                Console.ReadLine();
            }
        
            /// <summary>
            /// Uploads content from a local directory into the specified storage account.
            /// In this example the storage account is not associated with the Media Services account.
            /// </summary>
            /// <param name="localPath">The path from which to upload the files.</param>
            /// <returns>The container that contains the uploaded files.</returns>
            static public CloudBlobContainer UploadContentToStorageAccount(string localPath)
            {
                CloudBlobClient externalCloudBlobClient = _sourceStorageAccount.CreateCloudBlobClient();
                CloudBlobContainer externalMediaBlobContainer = externalCloudBlobClient.GetContainerReference("streamingfiles");
        
                externalMediaBlobContainer.CreateIfNotExists();
        
                // Upload files to the blob container.  
                DirectoryInfo uploadDirectory = new DirectoryInfo(localPath);
                foreach (var file in uploadDirectory.EnumerateFiles())
                {
                    CloudBlockBlob blob = externalMediaBlobContainer.GetBlockBlobReference(file.Name);
        
                    blob.UploadFromFile(file.FullName, FileMode.Open);
                }
        
                return externalMediaBlobContainer;
            }
        
            /// <summary>
            /// Creates a new asset and copies blobs from the specifed storage account.
            /// </summary>
            /// <param name="mediaBlobContainer">The specified blob container.</param>
            /// <returns>The new asset.</returns>
            static public IAsset CreateAssetFromExistingBlobs(CloudBlobContainer mediaBlobContainer)
            {
                // Create a new asset. 
                IAsset asset = _context.Assets.Create("Burrito_" + Guid.NewGuid(), AssetCreationOptions.None);
        
                IAccessPolicy writePolicy = _context.AccessPolicies.Create("writePolicy", TimeSpan.FromHours(24), AccessPermissions.Write);
                ILocator destinationLocator = _context.Locators.CreateLocator(LocatorType.Sas, asset, writePolicy);
        
                CloudBlobClient destBlobStorage = _destinationStorageAccount.CreateCloudBlobClient();
        
                // Get the asset container URI and Blob copy from mediaContainer to assetContainer. 
                string destinationContainerName = (new Uri(destinationLocator.Path)).Segments[1];
        
                CloudBlobContainer assetContainer = destBlobStorage.GetContainerReference(destinationContainerName);
        
                if (assetContainer.CreateIfNotExists())
                {
                    assetContainer.SetPermissions(new BlobContainerPermissions
                    {
                        PublicAccess = BlobContainerPublicAccessType.Blob
                    });
                }
        
                var blobList = mediaBlobContainer.ListBlobs();
                foreach (var sourceBlob in blobList)
                {
                    var assetFile = asset.AssetFiles.Create((sourceBlob as ICloudBlob).Name);
                    CopyBlob(sourceBlob as ICloudBlob, assetContainer);
                    assetFile.ContentFileSize = (sourceBlob as ICloudBlob).Properties.Length;
                    assetFile.Update();
                }
        
                destinationLocator.Delete();
                writePolicy.Delete();
        
                // Since we copied a set of Smooth Streaming files, 
                // set the .ism file to be the primary file. 
                SetISMFileAsPrimary(asset);
        
                return asset;
            }
        
            /// <summary>
            /// Creates the OnDemandOrigin locator in order to get the streaming URL.
            /// </summary>
            /// <param name="asset">The asset that contains the smooth streaming files.</param>
            /// <returns>The streaming URL.</returns>
            static public string CreateStreamingLocator(IAsset asset)
            {
                var ismAssetFile = asset.AssetFiles.ToList().
                    Where(f => f.Name.EndsWith(".ism", StringComparison.OrdinalIgnoreCase)).First();
        
                // Create a 30-day readonly access policy. 
                IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
                    TimeSpan.FromDays(30),
                    AccessPermissions.Read);
        
                // Create a locator to the streaming content on an origin. 
                ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
                    policy,
                    DateTime.UtcNow.AddMinutes(-5));
        
                return originLocator.Path + ismAssetFile.Name + "/manifest";
            }
        
            /// <summary>
            /// Copies the specified blob into the specified container.
            /// </summary>
            /// <param name="sourceBlob">The source container.</param>
            /// <param name="destinationContainer">The destination container.</param>
            static private void CopyBlob(ICloudBlob sourceBlob, CloudBlobContainer destinationContainer)
            {
                var signature = sourceBlob.GetSharedAccessSignature(new SharedAccessBlobPolicy
                {
                    Permissions = SharedAccessBlobPermissions.Read,
                    SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
                });
        
                ICloudBlob destinationBlob = destinationContainer.GetBlockBlobReference(sourceBlob.Name);
        
                if (destinationBlob.Exists())
                {
                    Console.WriteLine(string.Format("Destination blob '{0}' already exists. Skipping.", destinationBlob.Uri));
                }
                else
                {
                    try
                    {
                        Console.WriteLine(string.Format("Copy blob '{0}' to '{1}'", sourceBlob.Uri, destinationBlob.Uri));
                        destinationBlob.StartCopyFromBlob(new Uri(sourceBlob.Uri.AbsoluteUri + signature));
                    }
                    catch (Exception ex)
                    {
                        Console.WriteLine(string.Format("Error copying blob '{0}': {1}", sourceBlob.Name, ex.Message));
                    }
                }
            }
        
            /// <summary>
            /// Sets a file with the .ism extension as a primary file.
            /// </summary>
            /// <param name="asset">The asset that contains the smooth streaming files.</param>
            static private void SetISMFileAsPrimary(IAsset asset)
            {
                var ismAssetFiles = asset.AssetFiles.ToList().
                    Where(f => f.Name.EndsWith(".ism", StringComparison.OrdinalIgnoreCase)).ToArray();
        
                if (ismAssetFiles.Count() != 1)
                    throw new ArgumentException("The asset should have only one, .ism file");
        
                ismAssetFiles.First().IsPrimary = true;
                ismAssetFiles.First().Update();
            }
        }
 
测试
