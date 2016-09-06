---
ms.openlocfilehash: bb3a2a9761f59a9a14c6f8e25ececcda56a7c18f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将移动服务添加到现有的通用 Windows 应用商店应用程序 |Microsoft Azure" 
    description="了解如何开始使用移动服务利用 Windows 应用商店应用程序中的数据。" 
    services="mobile-services" 
    documentationCenter="windows" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/21/2015" 
    ms.author="glenga"/>

# 添加到现有应用程序的移动服务

[AZURE.INCLUDE [mobile-services-selector-get-started-data](../../includes/mobile-services-selector-get-started-data.md)]

##概述

本主题演示如何为 Windows 应用商店应用程序作为后端数据源使用 Azure 移动服务。 在本教程中，将下载的应用程序用于将数据存储在内存中的 Visual Studio 2013年项目、 创建新的移动服务、 移动服务相结合的应用程序，并查看运行应用程序时所做的数据更改。

您将在本教程中创建的移动服务是.NET 后端移动服务。 .NET 后端使您可以使用.NET 语言和 Visual Studio 可在移动服务中，服务器端业务逻辑，您可以运行并在您的本地计算机上调试您的移动服务。 若要创建可以在 JavaScript 编写服务器端业务逻辑的移动服务，请参阅本主题的 JavaScript 后端版本。

>[AZURE.NOTE]本主题演示如何使用 Visual Studio 专业版 2013 更新 3 中工具连接到一个通用的 Windows 应用程序的一种新的移动服务。 可以使用相同的步骤来连接到 Windows 应用商店或 Windows Phone 商店 8.1 的应用程序的移动服务。 要连接到一个 Windows Phone 8.0 或 Windows Phone Silverlight 8.1 的应用程序的移动服务，请参阅[开始使用 Windows Phone 的数据](mobile-services-dotnet-backend-windows-phone-get-started-data.md)。

> 如果您无法升级到 Visual Studio 专业版 2013年更新 3 或您更喜欢手动将您的移动服务项目添加到 Windows 应用商店应用程序解决方案，请参阅本主题的[此版本](../mobile-services-dotnet-backend-windows-store-dotnet-get-started-data.md)。

##先决条件

若要完成本教程，您需要以下各项︰

* 活动的 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-windows-universal-dotnet-get-started-data%2F)。
* <a href="https://go.microsoft.com/fwLink/p/?LinkID=391934" target="_blank">Visual Studio 2013年</a>（更新 3 或更高版本）。 

##下载 GetStartedWithData 项目

[AZURE.INCLUDE [mobile-services-windows-universal-dotnet-download-project](../../includes/mobile-services-windows-universal-dotnet-download-project.md)]

##从 Visual Studio 中创建新的移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service-vs2013](../../includes/mobile-services-dotnet-backend-create-new-service-vs2013.md)]

&nbsp;&nbsp;7.在解决方案资源管理器中，在 GetStartedWithData.Shared 项目文件夹中，打开 App.xaml.cs 文件，并注意到 Windows 应用商店应用程序条件编译块，其外观类似于下面的示例**应用程序**类添加新的静态字段︰ 

    public static Microsoft.WindowsAzure.MobileServices.MobileServiceClient 
        todolistClient = new Microsoft.WindowsAzure.MobileServices.MobileServiceClient(
            "https://todolist.azure-mobile.net/",
            "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX");
        

&nbsp;&nbsp;此代码使用[MobileServiceClient](http://go.microsoft.com/fwlink/p/?LinkId=302030)类的一个实例可以访问您在您的应用程序中的新的移动服务。 创建客户端提供的 URI 和应用程序键的新的移动服务。 此静态字段可供您的应用程序中的所有页面。

&nbsp;&nbsp;8.右击 Windows Phone 应用程序项目**添加**单击**连接服务...**，选择刚创建的移动服务和，然后单击**确定**。 相同的代码将添加到共享的 App.xaml.cs 文件，但这一次 Windows Phone 应用程序条件编译块内。

在这种情况下，Windows 应用商店和 Windows Phone 存储应用程序连接到新的移动服务。 下一步是测试新的移动服务项目。


##本地移动服务项目进行测试，

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service-api-documentation](../../includes/mobile-services-dotnet-backend-test-local-service-api-documentation.md)]


##更新应用程序以使用移动服务

在本节中将更新通用的 Windows 应用程序可以作为应用程序的后端服务使用移动服务。 您只需要对 GetStartedWithData.Shared 项目文件夹中的 MainPage.xaml.cs 项目文件进行更改。 

[AZURE.INCLUDE [mobile-services-windows-dotnet-update-data-app](../../includes/mobile-services-windows-dotnet-update-data-app.md)]


##发布到 Azure 的移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]


##测试移动在 Azure 中承载的服务

现在，我们可以测试针对移动服务承载 Azure 中通用的 Windows 应用程序的两个版本。

[AZURE.INCLUDE [mobile-services-windows-universal-test-app](../../includes/mobile-services-windows-universal-test-app.md)]

##查看存储在 SQL 数据库中的数据

[AZURE.INCLUDE [mobile-services-dotnet-backend-view-sql-data](../../includes/mobile-services-dotnet-backend-view-sql-data.md)]
 
本教程到此结束。

##下一步行动

本教程说明了启用通用的 Windows 应用程序项目来处理数据移动服务中的基础知识。 接下来，考虑研究其中一种的其他主题︰

* [开始使用身份验证]
  <br/>了解如何对您的应用程序的用户进行身份验证。

* [开始使用推式通知] 
  <br/>了解如何将非常基本的推式通知发送到您的应用程序。

* [移动服务 C# 帮助概念参考](mobile-services-windows-dotnet-how-to-use-client-library.md)
  <br/>了解更多关于如何使用.NET 的移动服务。


<!-- Images. -->



<!-- URLs. -->
[验证和修改数据的脚本]: /develop/mobile/tutorials/validate-modify-and-augment-data-dotnet
[优化查询分页]: /develop/mobile/tutorials/add-paging-to-data-dotnet
[开始使用移动服务]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md
[开始使用身份验证]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-users.md
[开始使用推式通知]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-push.md

[开始使用脱机数据同步]: mobile-services-windows-store-dotnet-get-started-offline-data.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[管理门户]: https://manage.windowsazure.com/
[移动服务 SDK]: http://go.microsoft.com/fwlink/p/?LinkId=257545
[开发人员代码示例站点]:  http://go.microsoft.com/fwlink/p/?LinkID=510826
[移动服务.NET 帮助概念参考]: mobile-services-windows-dotnet-how-to-use-client-library.md
[MobileServiceClient 类]: http://go.microsoft.com/fwlink/p/?LinkId=302030
  
测试
