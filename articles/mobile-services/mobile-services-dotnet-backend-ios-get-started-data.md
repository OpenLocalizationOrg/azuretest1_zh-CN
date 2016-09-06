---
ms.openlocfilehash: c9ca59781774f80d6bc362ee61510813d9b2a6db
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 iOS 中添加到现有应用程序的移动服务"
    description="了解如何开始使用移动服务利用 iOS 应用程序中的数据。"
    services="mobile-services"
    documentationCenter="ios"
    authors="krisragh"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="07/01/2015"
    ms.author="krisragh"/>

# 添加到现有应用程序的移动服务

[AZURE.INCLUDE [mobile-services-selector-get-started-data](../../includes/mobile-services-selector-get-started-data.md)]

在本教程中，您可以下载现有应用程序存储在内存中的数据并将其更改为使用 Azure 的移动服务。

在开始本教程之前完成[快速启动]是先决条件。 您将重新使用快速启动中创建移动服务。


##<a name="download-app"></a>下载 GetStartedWithData 项目

本教程是基于[GetStartedWithData iOS 应用程序]。 应用程序等同于[快速入门]，只是添加的项存储在内存中。

下载[GetStartedWithData iOS 应用程序]。 在 Xcode，打开的项目，并检查**TodoService.m**。 有八个**/ / TODO**指定的步骤，以使此应用程序中工作的意见。

##<a name="update-app"></a>更新应用程序为数据访问使用移动服务

[AZURE.INCLUDE [mobile-services-ios-enable-mobile-service-access](../../includes/mobile-services-ios-enable-mobile-service-access.md)]

##<a name="test-app"></a>测试应用程序

1. 在 Xcode，单击**运行**以启动该应用程序。 将项目添加到任务列表，键入文本并单击**+**。

2. 请验证所做的更改已保存到 Azure 中的数据库。 检查使用 Azure 管理门户或 Visual Studio SQL Server 对象资源管理器的数据库。

3. 若要检查使用门户网站的数据库，在您的移动服务仪表板页中，单击数据库名称，单击管理来管理数据库，并登录。 执行下面的查询中，但使用移动服务的名称，而不是`todolist`。

```
        SELECT * FROM [todolist].[todoitems]
```

<!-- Anchors. -->
[下载的 iOS 应用程序项目]: #download-app
[创建移动服务]: #create-service
[添加用于存储数据表格]: #add-table
[更新应用程序以使用移动服务]: #update-app
[测试针对移动服务应用程序]: #test-app
[下一步行动]:#next-steps
[下载本地服务]: #download-the-service-locally
[测试移动服务]: #test-the-service
[发布到 Azure 的移动服务]: #publish-mobile-service


<!-- Images. -->
[0]: ./media/mobile-services-dotnet-backend-ios-get-started-data/mobile-quickstart-startup-ios.png
[8]: ./media/mobile-services-dotnet-backend-ios-get-started-data/mobile-dashboard-tab.png
[9]: ./media/mobile-services-dotnet-backend-ios-get-started-data/mobile-todoitem-data-browse.png
[17]: ./media/mobile-services-dotnet-backend-ios-get-started-data/manage-sql-azure-database.png
[18]: ./media/mobile-services-dotnet-backend-ios-get-started-data/sql-azure-query.png


<!-- URLs. -->

[验证和修改数据的脚本]: /develop/mobile/tutorials/validate-modify-and-augment-data-dotnet
[开始使用移动服务]: /develop/mobile/tutorials/get-started-ios
[有关数据入门]: /develop/mobile/tutorials/get-started-with-data-ios
[开始使用身份验证]: /develop/mobile/tutorials/get-started-with-users-ios
[开始使用推式通知]: /develop/mobile/tutorials/get-started-with-push-ios
[JavaScript 后端版本]: /develop/mobile/tutorials/get-started-with-data-ios


[Azure 的管理门户]: https://manage.windowsazure.com/
[管理门户]: https://manage.windowsazure.com/
[安装 Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[移动服务 iOS SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[GitHub]:  http://go.microsoft.com/fwlink/p/?LinkId=268622
[GitHub repo]: http://go.microsoft.com/fwlink/p/?LinkId=268784

[快速入门]: mobile-services-dotnet-backend-ios-get-started.md
[GetStartedWithData iOS 应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=268622

测试
