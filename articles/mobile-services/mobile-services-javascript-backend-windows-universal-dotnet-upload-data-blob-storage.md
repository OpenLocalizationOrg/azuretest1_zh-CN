---
ms.openlocfilehash: 722194f2ae26d63a6e2f3bd4186e31ee0490687e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="上载图像到 Azure Blob 存储从通用的 Windows 应用程序 |Microsoft Azure" 
    description="了解如何使用 JavaScript 后端移动服务来将图像传到 Azure Blob 存储和访问通用的 Windows 应用程序中的图像。" 
    services="mobile-services,storage" 
    documentationCenter="windows" 
    authors="ggailey777" 
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

# 将图像传到 Azure Blob 存储，使用移动服务

[AZURE.INCLUDE [mobile-services-selector-upload-data-blob-storage](../../includes/mobile-services-selector-upload-data-blob-storage.md)]

##概述 

本主题演示如何使用 Azure 移动服务使您的应用程序上载和 Azure 存储中存储用户生成图像。 移动服务使用 SQL 数据库来存储数据。 但是，二进制大对象 (BLOB) 数据更有效地存储在 Azure Blob 存储服务。 

您不能安全地分发使用客户端应用程序安全地将数据上载到 Blob 存储服务所需的凭据。 相反，必须将这些凭据存储在您的移动服务，并使用它们来生成共享访问签名 (SAS)，用来上传新的映像。 SAS，短到期凭据&mdash;在这种情况下 5 分钟，由安全地移动服务到客户端应用程序。 该应用程序使用此临时凭证要上载的图像。 在此示例中，从 Blob 服务下载是公共的。

在本教程中向拍摄照片并将图像传到 Azure，使用移动服务生成 SAS 的移动服务快速入门应用程序中添加功能。 

##先决条件

本教程要求如下︰

+ Microsoft Visual Studio 2013年更新 3 或更高版本
+ [Azure 存储帐户](../storage-create-storage-account.md)
+ 照相机或其他图像捕获设备连接到计算机。

本教程基于移动服务快速入门。 在开始本教程之前，您必须首先完成[开始使用移动服务]。 

##更新管理门户中的注册的插入脚本

[AZURE.INCLUDE [mobile-services-configure-blob-storage](../../includes/mobile-services-configure-blob-storage.md)]

[AZURE.INCLUDE [mobile-services-windows-universal-dotnet-upload-to-blob-storage](../../includes/mobile-services-windows-universal-dotnet-upload-to-blob-storage.md)]

##下一步行动

现在，您已经能够安全地将图像上载将 Blob 服务与集成在您的移动服务，签出的后端服务和集成的一些主题︰

+ [计划中移动服务的后端作业]

    了解如何使用移动服务作业计划程序功能来定义服务器按照您定义的计划执行的脚本代码。

+ [移动服务服务器脚本引用]

    使用服务器脚本来执行服务器端任务和与其他 Azure 组件和外部资源集成的参考主题。
 
+ [移动服务.NET 帮助概念参考]

    了解更多关于如何使用.NET 的移动服务
  
 
<!-- Anchors. -->
[安装客户端存储库]: #install-storage-client
[更新的客户端应用程序来捕获图像]: #add-select-images
[更新生成 SAS 的插入脚本]: #update-scripts
[上载图像来测试应用程序]: #test
[下一步行动]:#next-steps

<!-- Images. -->

[2]: ./media/mobile-services-windows-store-dotnet-upload-data-blob-storage/mobile-add-storage-nuget-package-dotnet.png


<!-- URLs. -->
[从 SendGrid 的移动服务发送电子邮件]: store-sendgrid-mobile-services-send-email-scripts.md
[计划中移动服务的后端作业]: mobile-services-schedule-recurring-tasks.md
[向 Windows 应用商店应用程序从一个.NET 后端使用服务总线发送推式通知]: http://go.microsoft.com/fwlink/?LinkId=277073&clcid=0x409
[移动服务服务器脚本引用]: mobile-services-how-to-use-server-scripts.md
[开始使用移动服务]: mobile-services-javascript-backend-windows-store-dotnet-get-started.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[如何创建存储帐户。]: ../storage-create-storage-account.md
[Azure 存储客户端库，用于存储应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=276866 
[移动服务.NET 帮助概念参考]: mobile-services-windows-dotnet-how-to-use-client-library.md
[应用程序设置]: http://msdn.microsoft.com/library/windowsazure/b6bb7d2d-35ae-47eb-a03f-6ee393e170f7
 
测试
