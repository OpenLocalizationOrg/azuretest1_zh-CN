---
ms.openlocfilehash: 3ab349399156553d28895840b2f5aa76c6481b81
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="上载图像到 Azure Blob 存储从通用的 Windows 应用程序 |Microsoft Azure" 
    description="了解如何使用.NET 后端移动服务来将图像传到 Azure Blob 存储和访问通用的 Windows 应用程序中的图像。" 
    documentationCenter="windows" 
    authors="ggailey777" 
    services="mobile-services,storage" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/13/2015" 
    ms.author="glenga"/>

# 通过使用移动服务到 Azure 存储上载图像

[AZURE.INCLUDE [mobile-services-selector-upload-data-blob-storage](../../includes/mobile-services-selector-upload-data-blob-storage.md)]

##概述
本主题演示如何使用 Azure 移动服务使您的应用程序上载和 Azure 存储中存储用户生成图像。 移动服务使用 SQL 数据库来存储数据。 但是，二进制大对象 (BLOB) 数据更有效地存储在 Azure Blob 存储服务。 

您不能安全地分发使用客户端应用程序安全地将数据上载到 Blob 存储服务所需的凭据。 相反，必须将这些凭据存储在您的移动服务，并使用它们来生成共享访问签名 (SAS)，用来上传新的映像。 SAS，短到期凭据&mdash;在这种情况下 5 分钟，由安全地移动服务到客户端应用程序。 该应用程序使用此临时凭证要上载的图像。 在此示例中，从 Blob 服务下载是公共的。

在本教程中向拍摄照片并将图像传到 Azure，使用移动服务生成 SAS 的移动服务快速入门应用程序中添加功能。 

##先决条件

本教程要求如下︰

+ Microsoft Visual Studio 2013年更新 3 或更高版本。
+ [Azure 存储帐户](../storage-create-storage-account.md)
+ 照相机或其他图像捕获设备连接到计算机。

本教程基于移动服务快速入门。 在开始本教程之前，您必须首先完成[开始使用移动服务]。 

[AZURE.INCLUDE [mobile-services-dotnet-backend-configure-blob-storage](../../includes/mobile-services-dotnet-backend-configure-blob-storage.md)]

[AZURE.INCLUDE [mobile-services-windows-universal-dotnet-upload-to-blob-storage](../../includes/mobile-services-windows-universal-dotnet-upload-to-blob-storage.md)]

##下一步行动

现在，您已经能够安全地将图像上载将 Blob 服务与集成在您的移动服务，签出的后端服务和集成的一些主题︰

+ [计划中移动服务的后端作业](../mobile-services-dotnet-backend-schedule-recurring-tasks.md)

     了解如何使用移动服务作业计划程序功能来定义服务器按照您定义的计划执行的脚本代码。

+ [移动服务.NET 帮助概念参考](../mobile-services-windows-dotnet-how-to-use-client-library.md)

     了解更多关于如何使用.NET 的移动服务
 
<!-- Anchors. -->
[安装客户端存储库]: #install-storage-client
[更新的客户端应用程序来捕获图像]: #add-select-images
[安装存储客户端的移动服务项目中]: #storage-client-server
[更新数据模型中的 TodoItem 定义]: #update-data-model
[更新表生成 SAS 控制器]: #update-scripts
[上载图像来测试应用程序]: #test
[下一步行动]:#next-steps

<!-- Images. --> 

<!-- URLs. -->
[开始使用移动服务]: ../mobile-services-windows-store-dotnet-get-started.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[如何创建存储帐户。]: ../storage-create-storage-account.md
[Azure 存储客户端库，用于存储应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=276866 
 
测试
