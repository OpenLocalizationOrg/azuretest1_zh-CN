---
ms.openlocfilehash: c84767ec5b706d7bc938ce8c4a7089be89e9147b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
本主题演示如何创建一个具有两个移动的应用程序和 web 客户端。 您将创建一个移动应用程序和 web 应用程序并使用两个相同的基础数据库。

首先，您将创建新的移动应用程序后端和一个简单*的待办事项列表*的应用程序将应用程序数据存储在新移动应用程序的后端。 移动应用程序的后端服务器端业务逻辑使用受支持的.NET 语言。 客户端应用程序可以使用任何客户端平台，支持移动应用程序，包括 iOS，Windows，Xamarin iOS 和 Xamarin Android。

然后，您将创建一个 web 应用程序中，为您的移动应用程序中使用相同的数据库。 在本教程结束时，您必须在 web 客户端和移动客户端使用相同的数据。

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## 创建一个新移动应用程序的后端和客户端

* 在本教程[创建一个移动应用程序]来创建移动应用程序的后端和客户端中执行的步骤。 您可以使用任何客户端平台，支持移动应用程序，包括 iOS，Windows，Xamarin iOS 和 Xamarin Android。

* 确保您已到 Azure 部署移动应用程序端和移动客户端应用程序到托管后端连接。 移动应用程序的代码项目使用实体框架代码优先，并从移动客户端应用程序的第一个 REST 请求之后初始化数据库。

## 从 Visual Studio 任务列表 Web API 发布

在本节中，您将创建新 web 应用程序使用一个示例 Web 应用程序解决方案。 您将修改此示例使用移动应用程序与数据库架构名称相同和相同的连接字符串。

>[AZURE.NOTE] 在完成这些步骤之前, 应确保，已将客户端连接到它来初始化您移动应用程序的后端数据库。 否则，使 web 应用程序将不能连接到同一数据库。

1. 在[Azure 门户](https://portal.azure.com/)，创建新的 web 应用程序，使用同一资源组和承载计划为您的移动应用程序。

2. 下载示例解决方案[MultiChannelToDo] ，并在 Visual Studio 中打开。 该解决方案包含 web 客户端用户界面的 Web API 项目和 Web 应用程序项目。

3. 在 Web API 项目，编辑 MultiChannelToDoContext.cs。 在`OnModelCreating`，更新为您的移动应用程序名称相同的架构名称︰

        modelBuilder.HasDefaultSchema("your_mobile_app"); // your service name, replacing dashes with underscore

4. 接下来，我们将会从 Azure 门户移动应用程序的连接字符串︰

    - 在门户中选择您的移动应用程序，然后单击部件标记为**用户代码**。

    - 打开刀片式服务器，在选择**的所有设置**，再选择**应用程序设置**。

    - 在**连接字符串**下单击**显示连接字符串**。 复制设置**MS_TableConnectionString**的值。 这是由您的移动应用程序用来连接到 SQL 数据库的连接字符串。

5. 在 Visual Studio 中，右击 Web API 项目并选择**发布**。 作为发布目标，选择**Azure Web 应用程序**，然后选择上面创建的 web 应用程序。 直到到达 Web 发布向导的**设置**部分，单击**下一步**。

6. 在**数据库**部分中，粘贴移动应用程序的连接字符串的值为**MultiChannelToDoContext**。 选择仅**使用在运行时此连接字符串**。

7. 一旦 Web API 已成功发布到 Azure，您将看到一个确认页。 复制发布的服务的 URL。

## 发布任务列表 web 客户端从 Visual Studio 的 UI

在本节中，您将使用示例 web 客户端应用程序使用 AngularJS 实现。 然后，您将使用 Visual Studio 将项目发布到新宿主应用程序服务 web 应用程序在 Azure 中。

1. 在 Visual Studio 中，打开项目**MultiChannelToDo.Web**。 编辑文件， `js/service/ToDoService.js`，将 URL 添加到刚才发布 Web API:

        var apiPath = "https://your-web-api-site-name.azurewebsites.net";

2. 右键单击项目**MultiChannelToDo.Web** ，然后选择**发布**。

3. 在**发布**向导中，选择作为发布目标的**Azure Web 应用程序**和创建新 web 应用程序而无需数据库。

4. 一旦您的项目已被成功发布，您将看到在浏览器中的 web 用户界面。

## 测试移动电话和 web 应用程序

1. 在 web 用户界面中添加或编辑一些 todo 项。

    ![在浏览器中的 Web 应用程序的视图](./media/app-service-mobile-dotnet-backend-web-and-mobile/web-app-in-browser.png)

2. 运行您在[创建一个移动应用程序]教程中创建的移动应用程序。 您将看到相同的 todo 项，如下所示的 web 应用程序。

    ![Xamarin 移动应用程序的视图](./media/app-service-mobile-dotnet-backend-web-and-mobile/xamarin-ios-quickstart-device.png)

## 下一步行动

在此示例中我们介绍了如何让应用程序具有一个网站和移动客户端使用相同的基础数据库。 在这里，我们没有任何业务逻辑中我们想要重复使用跨两个客户端，这样已足够，只是共享同一个数据库的后端。 在以后的教程中，您将学习如何将业务逻辑添加到您的 Web API 和重用您移动应用程序的后端代码中的逻辑。

## 会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)
* 旧的门户与新门户的更改的指南，请参阅︰[用于预览门户导航的引用](http://go.microsoft.com/fwlink/?LinkId=529715)

<!-- Links -->

[MultiChannelToDo]: https://github.com/Azure/mobile-services-samples/tree/web-mobile/MultiChannelToDo
[创建一个移动应用程序]: ../article/app-service-mobile/app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview.md
