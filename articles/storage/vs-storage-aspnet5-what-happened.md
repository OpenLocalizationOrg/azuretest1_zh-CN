---
ms.openlocfilehash: cfa17539dc1ed9df5c86a4e01997094cd99a6510
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 存储 |Microsoft Azure"
    description="描述 Visual Studio ASP.NET 5 项目中创建 Azure 存储时，发生了什么变化"
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
    ms.author="patshea"/>

# 我的项目发生了什么变化？

> [AZURE.SELECTOR]
> - [入门教程](vs-storage-aspnet5-getting-started-blobs.md)
> - [发生了什么事](vs-storage-aspnet5-what-happened.md)

> [AZURE.SELECTOR]
> - [Blob](vs-storage-aspnet5-getting-started-blobs.md)
> - [队列](vs-storage-aspnet5-getting-started-queues.md)
> - [表](vs-storage-aspnet5-getting-started-tables.md)

### 我的项目发生了什么变化？

#### 添加引用

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

此外，还添加了 NuGet 程序包**Microsoft.Framework.Configuration.Json** 。

#### Azure 存储添加的连接字符串
在项目的 config.json 文件中，已使用的选定的存储帐户连接字符串和键创建一个元素。

有关详细信息，请参见[ASP.NET 5](http://www.asp.net/vnext)。
