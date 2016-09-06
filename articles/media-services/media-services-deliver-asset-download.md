---
ms.openlocfilehash: 1dc07bee29e8edc89e2f4e3a7c7cca2dfcbd0ead
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="下载的媒体资产" 
    description="了解要下载到您的计算机的资产。 代码示例编写的 C# 和用于.NET 的媒体服务 SDK。" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2015" 
    ms.author="juliako"/>

#如何︰ 提供下载的资产

本主题将讨论提供媒体资产上载到介质服务的选项。 在很多应用程序方案中，可以提供媒体服务的内容。 可以下载的媒体资产，或通过使用定位程序中访问它们。 到另一个应用程序或其他内容提供商，您可以发送媒体内容。 为了提高的性能和可扩展性，您还可以使用内容传递网络 (CDN) 提供的内容。

此示例演示如何从介质服务中的媒体资产下载到本地计算机。 该代码查询作业 id 与介质服务帐户相关联的作业并访问其**OutputMediaAssets**集合 （这是通过运行作业而产生的一个或多个输出媒体资产集）。 此示例演示如何输出媒体资产从一项工作，但是您可以应用同样的方法下载其他资产。

    
    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 
    
        // Get a reference to the job. 
        IJob job = GetJob(jobId);
    
        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);
    
        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };
    
        var downloadTasks = new List&lt;Task&gt;();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;
    
            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);
    
            Console.WriteLine("File download path:  " + localDownloadPath);
    
            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));
    
            outputFile.DownloadProgressChanged -= DownloadProgress;
        }
    
        Task.WaitAll(downloadTasks.ToArray());
    
        return outputAsset;
    }
    
    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }
   
##请参见 

[提供流式内容](media-services-deliver-streaming-content.md)


测试
