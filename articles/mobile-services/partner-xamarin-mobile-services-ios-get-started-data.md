---
ms.openlocfilehash: cf9662ba0437accd0f53755d515acf89ea733aea
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="添加到现有的应用程序 (Xamarin.iOS) 的移动服务 |Microsoft Azure"
    description="了解如何存储和访问数据从 Azure 服务 Xamarin.iOS 移动应用程序。"
    documentationCenter="xamarin"
    authors="ggailey777"
    manager="dwrede"
    services="mobile-services"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/18/2015"
    ms.author="ggailey777"/>

# 添加到现有应用程序的移动服务

[AZURE.INCLUDE [mobile-services-selector-get-started-data](../../includes/mobile-services-selector-get-started-data.md)]

本主题演示如何使用 Azure 移动服务利用 Xamarin.iOS 应用程序中的数据。 在本教程中，您将下载的应用程序用于将数据存储在内存中创建一个新的移动服务、 移动服务相结合的应用程序，然后登录到 Azure 管理门户网站来查看运行应用程序时所做的数据更改。

> [AZURE.NOTE] 本教程旨在帮助您更好地了解如何移动服务使您能够使用 Azure 存储和检索 Xamarin.iOS 应用程序中的数据。 在这种情况下，本主题还演示了很多的步骤，将为您完成移动服务快速入门中。 如果这是您首次使用移动服务体验，请考虑首先完成[移动服务入门](/develop/mobile/tutorials/get-started-xamarin-ios)教程。

本教程将引导您完成以下基本步骤︰

1. [下载 Xamarin.iOS 应用程序项目][GitHub]
2. [创建移动服务]
3. [添加用于存储数据表格]
4. [更新应用程序以使用移动服务]
5. [测试针对移动服务应用程序]

本教程需要[Azure 移动服务组件]、 [XCode 6.0][安装 Xcode]， [Xamarin.iOS]，和 iOS 7.0 或更高版本。

> [AZURE.IMPORTANT] 若要完成本教程，您需要一个 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started-with-data-xamarin-ios%2F"%20target="_blank)。

## <a name="download-app"></a>下载 GetStartedWithData 项目

本教程是基于[GetStartedWithData app][GitHub]，即 Xamarin.iOS 应用程序。 此应用程序的用户界面与应用程序生成的移动服务 Xamarin.iOS 快速入门，只是添加项目存储在本地内存中。

1. 下载[GetStartedWithData app][GitHub]。

2. 在 Xamarin Studio 中打开下载的项目并检查**TodoService**类。

    请注意，有几个**/ / TODO**指定步骤的注释必须采取以使此应用程序中使用您的移动服务。

3. 转到**运行**菜单中，选择**不调试启动**以启动该应用程序。

4. 在应用程序中，键入某些文本框中的文本，然后单击**+**按钮。

    ![][0]  

    注意到已保存的文本将显示在下面的列表中。

## <a name="create-service"></a>管理门户中创建新的移动服务

[AZURE.INCLUDE [mobile-services-create-new-service-data](../../includes/mobile-services-create-new-service-data.md)]

## <a name="add-table"></a>将新表添加到移动服务

若要能够将应用程序数据存储在新的移动服务，必须首先在相关联的 SQL 数据库实例中创建新表。

1. 在管理门户中，单击**移动服务**，然后单击您刚刚创建的移动服务。

2. 单击**数据**选项卡，然后单击**+ 创建**。

    ![][5]

    这将显示对话框中**创建新表**。

3. 在**表名称**键入_TodoItem_，然后单击检查按钮。

    ![][6]

    这将使用默认的权限集，这意味着应用程序的任何用户都可以访问和更改表中的数据创建新存储表**TodoItem** 。

    > [AZURE.NOTE] 在移动服务快速入门中使用相同的表名称。 但是，特定于给定的移动服务的架构中创建的每个表。 这是为了避免数据冲突时多个移动服务使用相同的数据库。

4. 单击新的**TodoItem**表，请检查不有任何数据行。

5. 单击**列**选项卡，验证只有一个**id**列，自动为您创建。

      这是一个表中移动服务的最低要求。

    > [AZURE.NOTE] 在您的移动服务启用动态架构时，新列将 JSON 对象发送到移动服务的插入或更新操作时自动创建。

现在您可以将新的移动服务用作数据存储应用程序。

## <a name="update-app"></a>更新应用程序来使用用于数据访问的移动服务

现在，您的移动服务已准备就绪，可以更新应用程序而不是本地集合中移动服务中存储的项目。

1. 如果没有**Azure 移动服务**在组件文件夹中列出，可以通过右键单击**组件**，选择**获取更多的组件**然后搜索**Azure 移动服务**来获取它。

2. 在 Xamarin Studio 中的解决方案视图中，打开的**TodoService**类，请取消注释以下`using`语句︰

        using Microsoft.WindowsAzure.MobileServices;

3. 在管理门户中，单击**移动服务**，然后单击您刚刚创建的移动服务。

4. 单击**仪表板**选项卡和记下**网站 URL**，然后单击**管理密钥**并记下**应用程序注册表项**。

    ![][8]

5. 在**常量**类中，取消注释下列常量︰

        public const string ApplicationURL = @"AppUrl";
        public const string ApplicationKey = @"AppKey";

6. 替换为**AppUrl**和**AppKey**在上述常量值上面找到管理门户中。

7. 在**TodoService**类中，取消注释以下代码行︰

        private MobileServiceClient client;

    这将创建一个表示连接到服务 MobileServiceClient 属性

8. 在**TodoService**类中，取消注释以下代码行︰

        private IMobileServiceTable<TodoItem> todoTable;  

    这将创建移动服务表的属性表示形式。

9. 请取消注释以下行在**TodoService**构造函数︰

        // TODO:: Uncomment these lines to use Mobile Services
        client = new MobileServiceClient(Constants.ApplicationURL, Constants.ApplicationKey).WithFilter(this);
        todoTable = client.GetTable<TodoItem>();

    这创建了移动服务客户端的实例，并创建 TodoItem 表实例。

10. 取消注释以下行**TodoService**的**RefreshDataAsync**方法中

            // TODO:: Uncomment these lines to use Mobile Services
            try
        {
                // This code refreshes the entries in the list view by querying the TodoItems table.
                // The query excludes completed TodoItems
                Items = await todoTable
                    .Where (todoItem => todoItem.Complete == false).ToListAsync();
            }
        catch (MobileServiceInvalidOperationException e)
        {
                Console.Error.WriteLine (@"ERROR {0}", e.Message);
                return null;
            }

    这将创建一个查询以返回所有尚未完成的任务。

11. 找到的**InsertTodoItemAsync**方法，并请取消注释以下行︰

        await todoTable.InsertAsync(todoItem);

    此代码将插入请求发送到移动服务。

13. 找到的**CompleteItemAsync**方法，并请取消注释以下行︰

        await todoTable.UpdateAsync(item);

    此代码将删除 TodoItems 之后它们被标记为已完成。

现在，该应用程序已更新为使用移动服务的后端存储，就可以测试对移动服务的应用程序。

## <a name="test-app"></a>测试应用程序对您的新移动服务

1. Xamarin Studio 中选择仿真程序或设备部署从一个主要的组合框，然后转到**运行**菜单并选择**启动 WithoutDebugging**启动该应用程序。

    这将执行 Azure 移动服务客户端，使用 Xamarin.iOS，查询从您的移动服务项目构建的。

2. 因为在这之前，在文本框中，键入文本，然后单击**+**按钮。.

    将新项插入发送给移动服务。

3. 在[管理门户网站]中，单击**移动服务**，然后单击移动服务。

4. 单击**数据**选项卡，然后单击**浏览**。

    ![][9]

    请注意**TodoItem**表现在移动服务所生成的 id 值与包含数据，以及列有已自动添加到要匹配的应用程序中的 TodoItem 类的表。

此教程到此结束**入门数据**为 Xamarin.iOS。

## 获取已完成的示例
下载[已完成的示例项目]。 请务必 Azure 设置更新的**applicationURL**和**applicationKey**变量。

## <a name="next-steps"> </a>下一步行动

本教程说明了启用 iOS 应用程序使用的数据移动服务中的基础知识。

接下来，请考虑完成下面的教程将基于您在本教程中创建的 GetStartedWithData 应用程序之一︰

* [验证和修改数据的脚本]
  <br/>了解有关使用移动服务在服务器脚本验证和更改您的应用程序发送数据。

* [优化查询分页]
  <br/>了解如何为控件中的单个请求处理的数据量的查询中使用分页。

一旦您已完成的数据系列，请尝试以下其他 iOS 教程︰

* [开始使用身份验证]
    <br/>了解如何对您的应用程序的用户进行身份验证。

* [开始使用推式通知]
  <br/>了解如何将非常基本的推式通知发送到您的移动服务的应用程序。

<!-- Anchors. -->

[获取 Windows 应用商店应用程序]: #download-app
[创建移动服务]: #create-service
[添加用于存储数据表格]: #add-table
[更新应用程序以使用移动服务]: #update-app
[测试针对移动服务应用程序]: #test-app
[下一步行动]:#next-steps

<!-- Images. -->
[0]: ./media/partner-xamarin-mobile-services-ios-get-started-data/mobile-quickstart-startup-ios.png




[5]: ./media/partner-xamarin-mobile-services-ios-get-started-data/mobile-data-tab-empty.png
[6]: ./media/partner-xamarin-mobile-services-ios-get-started-data/mobile-create-todoitem-table.png
[8]: ./media/partner-xamarin-mobile-services-ios-get-started-data/mobile-dashboard-tab.png
[9]: ./media/partner-xamarin-mobile-services-ios-get-started-data/mobile-todoitem-data-browse.png

<!-- URLs. TODO:: update download link, github link, and completed example project with new Xamarin.iOs projects -->
[验证和修改数据的脚本]: /develop/mobile/tutorials/validate-modify-and-augment-data-xamarin-ios
[优化查询分页]: /develop/mobile/tutorials/add-paging-to-data-xamarin-ios
[开始使用移动服务]: /develop/mobile/tutorials/get-started-xamarin-ios
[有关数据入门]: /develop/mobile/tutorials/get-started-with-data-xamarin-ios
[开始使用身份验证]: /develop/mobile/tutorials/get-started-with-users-xamarin-ios
[开始使用推式通知]: /develop/mobile/tutorials/get-started-with-push-xamarin-ios

[Azure 的管理门户]: https://manage.windowsazure.com/
[管理门户]: https://manage.windowsazure.com/
[安装 Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[移动服务 iOS SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331302
[GitHub repo]: http://go.microsoft.com/fwlink/p/?LinkId=268784
[Azure 的移动服务组件]: http://components.xamarin.com/view/azure-mobile-services/

[已完成的示例项目]: http://go.microsoft.com/fwlink/p/?LinkId=331302
[Xamarin.iOS]: http://xamarin.com/download
