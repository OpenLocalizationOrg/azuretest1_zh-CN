---
ms.openlocfilehash: 68d255c57e287e334dec5ae7956ef137048301bd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="要开始使用 Azure 存储"
    description="描述 Visual Studio 云服务项目中使用 Azure 存储时，会发生什么情况"
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
    ms.date="07/22/2015"
    ms.author="patshea123"/>

# 我的项目发生了什么变化？

> [AZURE.SELECTOR]
> - [入门教程](vs-storage-cloud-services-getting-started-blobs.md)
> - [发生了什么事](vs-storage-cloud-services-what-happened.md)

###我的项目发生了什么变化？

###### 添加引用

Azure 存储 NuGet 程序包添加到 Visual Studio 项目。  
此包中添加以下.NET 引用︰

- `Microsoft.Data.Edm`
- `Microsoft.Data.OData`
- `Microsoft.Data.Services.Client`
- `Microsoft.WindowsAzure.Configuration`
- `Microsoft.WindowsAzure.Storage`
- `Newtonsoft.Json`
- `System.Data`
- `System.Spatial`

######Azure 存储添加的连接字符串
使用选定的存储帐户连接字符串和密钥创建元素。 以下文件做了修改︰

- `ServiceDefinition.csdef`
- `ServiceConfiguration.Cloud.cscfg`
- `ServiceConfiguration.Local.cscfg`
