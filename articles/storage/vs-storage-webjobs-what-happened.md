---
ms.openlocfilehash: b23dea33807634b7590d8b06e641c1dd1f347381
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="要开始使用 Azure 存储" 
    description="描述 Visual Studio Azure WebJob 项目中创建 Azure 存储时，发生了什么变化" 
    services="storage" 
    documentationCenter="" 
    authors="patshea123" 
    manager="douge" 
    editor="tglee"/>

<tags 
    ms.service="storage" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-what-happened" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/13/2015" 
    ms.author="patshea123"/>

# 我的项目发生了什么变化？

> [AZURE.SELECTOR]
> - [入门教程](vs-storage-webjobs-getting-started-blobs.md)
> - [发生了什么事](vs-storage-webjobs-what-happened.md)

###<span id="whathappened">我的项目发生了什么变化？</span>

##### 添加引用

Azure 存储 NuGet 程序包添加或更新 Visual Studio 项目中。  
此包中添加以下.NET 引用︰

- `Microsoft.Data.Edm`
- `Microsoft.Data.OData`
- `Microsoft.Data.Services.Client`
- `Microsoft.WindowsAzure.ConfigurationManager`
- `Microsoft.WindowsAzure.Storage`
- `Newtonsoft.Json`
- `System.Data`
- `System.Spatial`

#####Azure 存储添加的连接字符串 
您的项目的 App.config 文件中`AzureWebJobsStorage`，`AzureWebJobsDashboard`与选定的存储帐户连接字符串和密钥更新了。 

有关详细信息，请参阅[Azure WebJobs 推荐资源](http://go.microsoft.com/fwlink/?linkid=390226)。 