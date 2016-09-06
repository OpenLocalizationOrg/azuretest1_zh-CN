---
ms.openlocfilehash: 3049abc25d1b27c1dd6f476dde092ec11c82ec64
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="上载图像到 Azure 存储从 Windows Phone Silverlight 应用程序 |Microsoft Azure" 
    description="了解如何使用移动服务上传到 Azure Blob 存储 Windows Phone Silverlight 应用程序中的图像。" 
    documentationCenter="windows" 
    authors="ggailey777" 
    services="mobile-services" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/21/2015" 
    ms.author="glenga"/>

# 通过使用移动服务到 Azure 存储上载图像

[AZURE.INCLUDE [mobile-services-selector-upload-data-blob-storage](../../includes/mobile-services-selector-upload-data-blob-storage.md)]

##概述

本主题演示如何使用 Azure 移动服务使您的应用程序上载和 Azure 存储中存储用户生成图像。 移动服务使用 SQL 数据库来存储数据。 但是，二进制大对象 (BLOB) 数据更有效地存储在 Azure Blob 存储服务。 

您不能安全地分发使用客户端应用程序安全地将数据上载到 Blob 存储服务所需的凭据。 相反，必须将这些凭据存储您的移动服务，并使用它们来生成共享访问签名 (SAS)，用来上传新的映像。 SAS，短到期凭据&mdash;在这种情况下 5 分钟，由安全地移动服务到客户端应用程序。 该应用程序使用此临时凭证要上载的图像。 在此示例中，从 Blob 服务下载是公共的。

在本教程中，您将添加功能到[GetStartedWithData 示例应用程序项目](mobile-services-windows-phone-get-started-data.md)来拍摄照片并将图像传到 Azure，通过使用 SAS 生成的移动服务。 


##先决条件

本教程要求如下︰

+ Microsoft Visual Studio 2012 Express 针对 Windows 8 或更高版本
+ [Windows Phone SDK 8.0]或更高版本
+ Nuget 程序包管理器已安装了 Microsoft Visual Studio。
+ [Azure 存储帐户][如何创建存储帐户。]
+ 完成教程[将添加到现有应用程序的移动服务](mobile-services-windows-phone-get-started-data.md)  


##安装 Windows Phone 应用程序存储客户端

若要能够使用 SAS 将图像传到 Blob 存储，必须先添加安装为 Windows Phone 应用程序存储客户端库的 NuGet 程序包。

1. 在**解决方案资源管理器**在 Visual Studio 中右击项目名称，然后选择**管理 NuGet 程序包**。

2. 在左窗格中，选择**联机**类别，选择**包括预发布版**、 **WindowsAzure.Storage 预览**搜索，单击**安装**在**Azure 存储**软件包，然后接受许可证协议条款。 

    ![NuGet Azure 存储中添加](./media/mobile-services-windows-phone-upload-data-blob-storage/mobile-add-storage-nuget-package-dotnet.png)

    这将 Azure 存储服务的客户端库添加到项目。

接下来，您将更新快速入门应用程序捕获和上传图像。

##更新管理门户中的注册的插入脚本


[AZURE.INCLUDE [mobile-services-configure-blob-storage](../../includes/mobile-services-configure-blob-storage.md)]

>[AZURE.NOTE]若要将新属性添加到 TodoItem 对象，您必须在您的移动服务中启用动态架构。 启用动态架构后，新的列将自动添加到 TodoItem 表映射到这些新属性。

[AZURE.INCLUDE [mobile-services-windows-phone-upload-to-blob-storage](../../includes/mobile-services-windows-phone-upload-to-blob-storage.md)]


##下一步行动

现在，您已经能够安全地将图像上载将 Blob 服务与集成在您的移动服务，签出的后端服务和集成的一些主题︰

+ [从 SendGrid 的移动服务发送电子邮件]
 
  了解如何添加到您的移动服务使用 SendGrid 的电子邮件服务的电子邮件功能。 本主题演示如何添加服务器端脚本发送邮件使用 SendGrid。

+ [计划中移动服务的后端作业]

  了解如何使用移动服务作业计划程序功能来定义服务器按照您定义的计划执行的脚本代码。

##请参见

+ [移动服务服务器脚本引用]

  使用服务器脚本来执行服务器端任务和与其他 Azure 组件和外部资源集成的参考主题。
 
+ [移动服务.NET 帮助概念参考]

  了解更多关于如何使用.NET 的移动服务
  
<!-- Images. -->

<!-- URLs. -->
[从 SendGrid 的移动服务发送电子邮件]: store-sendgrid-mobile-services-send-email-scripts.md
[计划中移动服务的后端作业]: mobile-services-schedule-recurring-tasks.md
[移动服务服务器脚本引用]: mobile-services-how-to-use-server-scripts.md
[开始使用移动服务]: ../mobile-services-windows-phone-get-started.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[如何创建存储帐户。]: ../storage-create-storage-account.md
[Azure 存储客户端库，用于存储应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=276866 
[移动服务.NET 帮助概念参考]: mobile-services-windows-dotnet-how-to-use-client-library.md
[Windows Phone SDK 8.0]: http://www.microsoft.com/download/details.aspx?id=35471


 