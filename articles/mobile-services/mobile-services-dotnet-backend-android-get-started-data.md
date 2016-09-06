---
ms.openlocfilehash: e075d394dbeda6ad01b2ba29bf4bacdd581e2bb6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="开始使用数据 (Android) |Microsoft Azure" 
    description="了解如何开始使用移动服务利用 Android 应用程序中的数据。" 
    services="mobile-services" 
    documentationCenter="android" 
    authors="RickSaling" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="java" 
    ms.topic="article" 
    ms.date="07/29/2015" 
    ms.author="ricksal"/>

# 添加到现有应用程序的移动服务

[AZURE.INCLUDE [mobile-services-selector-get-started-data](../../includes/mobile-services-selector-get-started-data.md)]

本主题演示如何使用 Azure 移动服务作为后端数据源的 Android 应用程序。 在本教程中，将创建新的移动服务、 下载 Eclipse Android 的应用程序用于将数据存储在内存中的项目、 移动服务相结合的应用程序，并查看对数据运行应用程序时所做的更改。

您在本教程中创建的移动服务在手机信息服务支持.NET 运行库。 这允许您使用.NET 语言和 Visual Studio 中移动服务的服务器端业务逻辑。 若要创建可以在 JavaScript 编写服务器端业务逻辑的移动服务，请参阅此主题的[JavaScript 后端版本](mobile-services-android-get-started-data.md)。

> [AZURE.NOTE] 如果您想要查看本教程的 Eclipse 版本，请转到︰[入门 (Eclipse) 的数据]。

若要完成本教程，您需要以下各项︰

+ <a href="https://go.microsoft.com/fwLink/p/?LinkID=391934" target="_blank">Visual Studio 2013年</a>（更新 3 或更高版本）。 

+ Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-android-get-started-data%2F)。 

##<a name="create-service"></a>创建新的移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

##<a name="download-the-service"></a>下载到本地计算机上的服务

[AZURE.INCLUDE [mobile-services-download-service-locally](../../includes/mobile-services-download-service-locally.md)]

##<a name="test-the-service"></a>测试移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service](../../includes/mobile-services-dotnet-backend-test-local-service.md)]

##<a name="publish-the-service"></a>发布到 Azure 的移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]

##<a name="download-app"></a>下载 GetStartedWithData 项目

###获取示例代码

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/download-android-sample-code.md)]

###验证 Android SDK 版本

[AZURE.INCLUDE [mobile-services-verify-android-sdk-version](../../includes/mobile-services-verify-android-sdk-version.md)]


###检查并运行该代码示例

[AZURE.INCLUDE [mobile-services-android-run-sample-code](../../includes/mobile-services-android-run-sample-code.md)]

##<a name="update-app"></a>更新应用程序来使用用于数据访问的移动服务

[AZURE.INCLUDE [mobile-services-android-getting-started-with-data](../../includes/mobile-services-android-getting-started-with-data.md)]

##<a name="test-app"></a>测试针对手机信息发布服务应用程序

现在，该应用程序已更新为使用移动服务的后端存储，可以对移动服务，使用 Android 模拟器或 Android 电话对其进行测试。

1. 从**运行**菜单上，单击**运行应用程序**以启动项目。

    这将执行您的应用程序，Android sdk，使用客户端库将发送一个查询，返回的项目，从您的移动服务生成。

5. 像以前一样，键入有意义的文本，然后单击**添加**。

    将新项插入发送给移动服务。

    您可以重新启动该应用程序以查看所做的更改已保存到 Azure 中的数据库。 您还可以检查使用 Azure 管理门户数据库︰ 在接下来的两个步骤执行此操作以查看数据库中所做的更改。

4. 在 Azure 管理门户中，单击与您的移动服务关联的数据库管理。

    ![](./media/mobile-services-dotnet-backend-android-get-started-data/manage-sql-azure-database.png)

5. 管理门户中执行查询以查看 Windows 应用商店应用程序所做的更改。 您的查询将会类似于下面的查询，但使用数据库的名称而不是`todolist`。

        SELECT * FROM [todolist].[todoitems]

    ![](./media/mobile-services-dotnet-backend-android-get-started-data/sql-azure-query.png)

Android 为到此结束**有关数据入门**教程。


## <a name="next-steps"> </a>下一步行动

本教程演示了使 Android 应用程序处理数据移动服务的基本知识。 

接下来，尝试下列任一其他教程︰

* [开始使用身份验证]
  <br/>了解如何对您的应用程序的用户进行身份验证。

* [开始使用推式通知] 
  <br/>了解如何将非常基本的推式通知发送到您的应用程序。

* [移动服务 Android 操作方法的概念引用](mobile-services-android-how-to-use-client-library.md)
  <br/>了解更多关于如何使用 Android 的移动服务。
  
<!-- Anchors. -->

[创建新的移动服务]: #create-service
[下载本地服务]: #download-the-service-locally
[测试移动服务]: #test-the-service
[下载 GetStartedWithData 项目]: #download-app
[更新应用程序来使用用于数据访问的移动服务]: #update-app
[测试 Android 应用程序针对本地承载的服务]: #test-locally-hosted
[发布到 Azure 的移动服务]: #publish-mobile-service
[Android 应用程序承载于 Azure 服务测试]: #test-azure-hosted
[测试针对手机信息发布服务应用程序]: #test-app
[下一步行动]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[开始使用数据 (Eclipse)]: mobile-services-dotnet-backend-android-get-started-data-ec.md
[开始使用移动服务]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md
[开始使用身份验证]: mobile-services-dotnet-backend-android-get-started-users.md
[开始使用推式通知]: mobile-services-dotnet-backend-android-get-started-push.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[管理门户]: https://manage.windowsazure.com/
[移动服务 SDK]: http://go.microsoft.com/fwlink/p/?LinkId=257545
[开发人员代码示例站点]:  http://go.microsoft.com/fwlink/p/?LinkId=328660
   

测试
