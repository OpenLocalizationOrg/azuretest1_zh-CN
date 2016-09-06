---
ms.openlocfilehash: 7cda63afb19e00229de618c25603a1dd49132f5c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="使用移动服务将图像传到 blob 存储 (Windows Phone) |Microsoft Azure"
    description="了解如何使用移动服务将图像传到 Azure Blob 存储。"
    documentationCenter="windows"
    authors="ggailey777"
    services="mobile-services,storage"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="glenga"/>

# 通过使用移动服务到 Azure 存储上载图像

[AZURE.INCLUDE [mobile-services-selector-upload-data-blob-storage](../../includes/mobile-services-selector-upload-data-blob-storage.md)]

##概述
本主题演示如何使用 Azure 移动服务启用 Windows Phone 8 或 Windows Phone 8.1 Silverlight 应用程序上载和 Azure 存储中存储用户生成图像。 移动服务使用 SQL 数据库来存储数据。 但是，二进制大对象 (BLOB) 数据更有效地存储在 Azure Blob 存储服务。 

您不能安全地分发使用客户端应用程序安全地将数据上载到 Blob 存储服务所需的凭据。 相反，必须将这些凭据存储您的移动服务，并使用它们来生成共享访问签名 (SAS)，用来上传新的映像。 SAS，短到期凭据&mdash;在这种情况下 5 分钟，由安全地移动服务到客户端应用程序。 该应用程序使用此临时凭证要上载的图像。 在此示例中，从 Blob 服务下载是公共的。

在本教程中，您将添加功能到[GetStartedWithData 示例应用程序项目](mobile-services-dotnet-backend-windows-phone-get-started-data.md)来拍摄照片并将图像传到 Azure，通过使用 SAS 生成的移动服务。

##先决条件

本教程要求如下︰

+ Microsoft Visual Studio 2013 或更高版本
+ [Windows Phone SDK 8.0]或更高版本
+ Nuget 程序包管理器已安装了 Microsoft Visual Studio。
+ [Azure 存储帐户][如何创建存储帐户。]
+ 完成教程[将添加到现有应用程序的移动服务](mobile-services-dotnet-backend-windows-phone-get-started-data.md)  

>[AZURE.NOTE]本教程只支持 Windows Phone 8 和 Windows Phone 8.1 Silverlight 的应用程序。 它不支持 Windows Phone 商店 8.1 或通用 Windows 8.1 应用程序。

[AZURE.INCLUDE [mobile-services-dotnet-backend-configure-blob-storage](../../includes/mobile-services-dotnet-backend-configure-blob-storage.md)]

##<a name="install-storage-client"></a>安装 Windows 应用商店应用程序存储客户端

若要能够使用 SAS 上载到 Blob 存储的图像从您的应用程序，必须首先添加安装为 Windows 应用商店应用程序存储客户端库的 NuGet 程序包。

1. 在**解决方案资源管理器**在 Visual Studio 中用鼠标右键单击该客户端应用程序项目，然后选择**管理 NuGet 程序包**。

2. 在左窗格中，选择**联机**类别，选择**包括预发布版**、 **WindowsAzure.Storage 预览**搜索，单击**安装**在**Azure 存储**软件包，然后接受许可证协议条款。

    ![][2]

    这将 Azure 存储服务的客户端库添加到项目。

[AZURE.INCLUDE [mobile-services-windows-phone-upload-to-blob-storage](../../includes/mobile-services-windows-phone-upload-to-blob-storage.md)]

<!-- Anchors. -->
[安装客户端存储库]: #install-storage-client
[更新的客户端应用程序来捕获图像]: #add-select-images
[安装存储客户端的移动服务项目中]: #storage-client-server
[更新数据模型中的 TodoItem 定义]: #update-data-model
[更新表生成 SAS 控制器]: #update-scripts
[上载图像来测试应用程序]: #test
[下一步行动]:#next-steps

<!-- Images. -->
[2]: ./media/mobile-services-dotnet-backend-windows-phone-upload-data-blob-storage/mobile-add-storage-nuget-package-dotnet.png

<!-- URLs. -->
[从 SendGrid 的移动服务发送电子邮件]: store-sendgrid-mobile-services-send-email-scripts.md
[计划中移动服务的后端作业]: mobile-services-dotnet-backend-schedule-recurring-tasks.md
[开始使用移动服务]: ../mobile-services-windows-phone-get-started.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[如何创建存储帐户。]: ../storage-create-storage-account.md
[Azure 存储客户端库，用于存储应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=276866
[移动服务.NET 帮助概念参考]: mobile-services-windows-dotnet-how-to-use-client-library.md
[Windows Phone SDK 8.0]: http://www.microsoft.com/download/details.aspx?id=35471

测试
