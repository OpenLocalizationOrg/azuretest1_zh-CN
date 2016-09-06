---
ms.openlocfilehash: dd77b7e0d0ac80d1fff684ca321c69eecdd2231c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何管理介质服务中的资产" 
    description="了解如何管理介质服务的资产。 您还可以管理作业、 任务、 访问策略、 定位器，和更多。 代码示例编写的 C# 和用于.NET 的媒体服务 SDK。" 
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


#如何︰ 管理资产存储中


创建媒体资产后，可以访问和管理服务器上的资产。 您还可以管理其他服务器上的对象包括作业、 任务、 访问策略、 定位器，和更多的媒体服务的一部分。

下面的示例演示如何查询 assetId 的资产。 

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query to get an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference the asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();
    
        return asset;
    }

要列出服务器上可用的所有资产，可以使用下面的方法可循环资源集合，并显示有关每个资产的详细信息。
    
    static void ListAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IAsset asset in _context.Assets)
        {
            // Display the collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");
    
            // Display the files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

下面的代码段从介质服务帐户中删除所有的资产。 请注意，是否某项资产是与某个程序关联，您首先必须删除程序。

    foreach (IAsset asset in _context.Assets)
    {
        asset.Delete();
    }

 
测试
