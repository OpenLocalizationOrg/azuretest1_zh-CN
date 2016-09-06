---
ms.openlocfilehash: e32fb2d6b78e85adcc149378882260366bd5dd99
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="为您的 Azure 移动应用程序 (Xamarin Android) 启用脱机同步"
    description="了解如何使用 Xamarin Android 应用程序中的缓存和同步脱机数据应用程序服务移动应用程序"
    documentationCenter="xamarin"
    authors="wesmc7777"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/22/2015"
    ms.author="wesmc"/>

# 启用脱机同步您的 Xamarin.Android 移动应用程序

[AZURE.INCLUDE [app-service-mobile-selector-offline-preview](../../includes/app-service-mobile-selector-offline-preview.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

## 概述

本教程介绍 Xamarin.Android Azure 移动应用程序的脱机同步功能。 脱机同步允许最终用户与移动应用程序，查看、 添加或修改数据--即使没有网络连接。 更改存储在本地数据库中;设备重新联机后，这些更改的同步完成与远程服务。

在本教程中，您将更新从教程[创建一个 Xamarin Android 应用程序]支持脱机功能的 Azure 移动应用程序客户端项目。 如果不使用下载快速入门服务器项目，您必须向项目添加数据访问扩展包。 有关服务器扩展软件包的详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。 

若要了解有关脱机同步功能的详细信息，请参阅主题[在 Azure 移动应用程序的脱机数据同步]。

## 要求

* Visual Studio 2013 年
* Visual Studio[扩展 Xamarin] **或** [Xamarin Studio]
* 完成本教程[创建一个 Xamarin Android 应用程序]。 本教程使用已完成的应用程序，该教程中介绍。

## 检查客户端的同步代码

当您已完成本教程[创建一个 Xamarin Android 应用程序]下载的 Xamarin 客户端项目包含支持使用一个本地的 SQLite 数据库的脱机同步的代码。 下面是教程的代码中已包含的内容的简要概述。 该功能的概念性概述，请参阅[脱机数据同步在 Azure 移动应用程序]。

* 表操作之前，必须先初始化本地存储区。 本地存储数据库初始化时`ToDoActivity.OnCreate()`执行`ToDoActivity.InitLocalStoreAsync()`。 这将创建一个新本地 SQLite 数据库使用`MobileServiceSQLiteStore`Azure 移动应用程序客户端 SDK 提供的类。 
 
    `DefineTable`方法与所提供的类型中的字段相匹配的本地存储区中创建一个表`ToDoItem`在这种情况下。 该类型不一定包括所有远程数据库中的列。 很可能来存储列的子集。  
        ToDoActivity.cs

        private async Task InitLocalStoreAsync()
        {
            // new code to initialize the SQLite store
            string path = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), localDbFilename);

            if (!File.Exists(path))
            {
                File.Create(path).Dispose();
            }

            var store = new MobileServiceSQLiteStore(path);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            // To use a different conflict handler, pass a parameter to InitializeAsync. For more details, see http://go.microsoft.com/fwlink/?LinkId=521416
            await client.SyncContext.InitializeAsync(store);
        }


* `toDoTable`的成员`ToDoActivity`的`IMobileServiceSyncTable`而不是键入`IMobileServiceTable`。 这将引导所有创建、 读取、 更新和删除 (CRUD) 到本地存储数据库的表操作。 
 
    您决定时这些更改被推送到 Azure 的移动应用程序的后端通过调用`IMobileServiceSyncContext.PushAsync()`客户端连接使用同步上下文。 同步上下文有助于跟踪和推送客户端应用程序已修改时的所有表中的更改保留表关系`PushAsync`调用。 

    提供的代码调用`ToDoActivity.SyncAsync()`每当刷新 todoitem 列表 todoitem 已添加或已完成的同步。 因此它将同步所有本地都更改同步上下文和同步表上拉上执行推后。 但是，它是一定要认识到是否纳入针对具有挂起的表执行上下文，跟踪的本地拉操作更新将自动触发上下文推第一次。 因此，在这种情况下 （刷新，添加并完成项目） 可以忽略显式`PushAsync`调用。 它是多余的。

    在提供的代码中，所有记录在远程`TodoItem`查询表，但它也是可以通过查询 id 筛选记录和查询为`PushAsync`。 有关更多信息，请参见*增量同步*[脱机数据同步在 Azure 移动应用程序]中的部分。

    <!-- Need updated conflict handling info : `InitializeAsync` uses the default conflict handler, which fails whenever there is a conflict. To provide a custom conflict handler, see the tutorial [Handling conflicts with offline support for Mobile Services].
    -->


        // ToDoActivity.cs

        private async Task SyncAsync()
        {
            try {
                await client.SyncContext.PushAsync();
                await toDoTable.PullAsync("allTodoItems", toDoTable.CreateQuery()); // query ID is used for incremental sync
            } catch (Java.Net.MalformedURLException) {
                CreateAndShowDialog (new Exception ("There was an error creating the Mobile Service. Verify the URL"), "Error");
            } catch (Exception e) {
                CreateAndShowDialog (e, "Error");
            }
        }


## 运行客户端应用程序

运行客户端应用程序至少一次以填充本地存储数据库。 在下一部分中，将模拟脱机的情况下，修改本地存储中的数据，该应用程序处于脱机状态时。

## 更新的客户端应用程序的同步行为

在本节中，您将修改客户端应用程序通过您的后端使用无效的应用程序的 URL 来模拟脱机的情况。 当添加或更改数据的项目时，这些更改将保存在本地存储，但不是同步到后端数据存储区，直到重新建立连接时。

1. 顶部的`ToDoActivity.cs`，更改初始化`applicationURL`和`gatewayURL`为指向无效的 Url:

        const string applicationURL = @"https://your-service.azurewebsites.xxx/"; 
        const string gatewayURL = @"https://your-gateway.azurewebsites.xxx";


2. 更新`ToDoActivity.SyncAsync`，`MobileServicePushFailedException`被捕获，并且完全忽略假定您处于脱机状态。

        private async Task SyncAsync()
        {
            try {
                await client.SyncContext.PushAsync();
                await toDoTable.PullAsync("allTodoItems", toDoTable.CreateQuery()); // query ID is used for incremental sync
            } catch (Java.Net.MalformedURLException) {
                CreateAndShowDialog (new Exception ("There was an error creating the Mobile Service. Verify the URL"), "Error");
            } catch (MobileServicePushFailedException)
            {
                // Not reporting this exception. Assuming the app is offline for now
            }
            catch (Exception e) {
                CreateAndShowDialog (e, "Error");
            }
        }


3. 生成并运行该应用程序。 如果您获得异常对话框报告名称解析异常关闭它。 添加一些新的 todo 项，并注意应用程序的行为就像它连接因为`MobileServicePushFailedException`处理而不显示对话框。

4. 直到他们可以推送到移动的后端，仅在本地存储中存在您添加的新项。 关闭应用程序并重新启动它以验证您所创建的新项目将保存到本地存储区。

5. （可选）使用 Visual Studio 可以查看 SQL Azure 数据库表格，查看后端数据库中的数据未发生更改。 

   在 Visual Studio 中，打开**服务器资源管理器**。 导航到您的数据库在**Azure**->**SQL 数据库**。 右击数据库并选择**SQL Server 对象资源管理器中打开**。 现在您可以浏览 SQL 数据库表及其内容。

6. （可选）使用其他工具，如 Fiddler 或把邮递员弄来查询您移动的后端，在窗体中使用 GET 查询`https://your-mobile-app-backend-name.azurewebsites.net/tables/TodoItem`。 


## 更新的客户端应用程序重新连接您移动的后端

在本节中将重新连接到模拟回来到联机状态的应用程序的移动后端应用程序。 在执行刷新笔势时，将向您移动的后端同步数据。

1. 打开`ToDoActivity.cs`。 更正`applicationURL`和`gatewayURL`为指向正确的 Url。

2. 重新生成并运行该应用程序。 应用程序尝试启动后与 Azure 的移动应用程序的后端同步。 验证创建无异常对话框了。

3. （可选）查看已更新的数据使用 SQL Server 对象资源管理器或类似 Fiddler 其余工具。 请注意数据已 Azure 的移动应用程序的后端数据库和本地存储之间同步。

    注意在数据库和本地存储之间同步数据包含在脱机的情况下添加的项。

## 其他资源

* [在 Azure 的移动应用程序的脱机数据同步]

* [云盖︰ Azure 的移动服务在脱机同步]\(注︰ 视频移动服务但脱机同步以类似的方式在 Azure 移动应用程序中的工作\)

<!-- ##Summary

[AZURE.INCLUDE [mobile-services-offline-summary-csharp](../../includes/mobile-services-offline-summary-csharp.md)]

## Next steps

* [Handling conflicts with offline support for Mobile Services]

* [How to use the Xamarin Component client for Azure Mobile Services]
 -->

<!-- Images -->

<!-- URLs. -->
[创建一个 Xamarin Android 应用程序]: ../app-service-mobile-dotnet-backend-xamarin-android-get-started-preview.md
[在 Azure 的移动应用程序的脱机数据同步]: ../app-service-mobile-offline-data-sync-preview.md

[如何为 Azure 移动服务使用 Xamarin 组件的客户端]: ../partner-xamarin-mobile-services-how-to-use-client-library.md

[Xamarin Studio]: http://xamarin.com/download
[Xamarin 扩展名]: http://xamarin.com/visual-studio

[在移动服务 Azure 云盖︰ 脱机同步]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri

测试
