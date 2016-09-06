---
ms.openlocfilehash: 005ce47497596cb2d7fdb9a9906289c2b35b55ac
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用脱机数据移动服务 (Xamarin iOS) |Microsoft Azure" 
    description="了解如何使用 Xamarin iOS 应用程序中的缓存和同步脱机数据到 Azure 移动服务" 
    documentationCenter="xamarin" 
    authors="lindydonna" 
    editor="wesmc" 
    manager="dwrede" 
    services="mobile-services"/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/01/2015"
    ms.author="donnam"/>

# 使用移动服务中的脱机数据同步

[AZURE.INCLUDE [mobile-services-selector-offline](../../includes/mobile-services-selector-offline.md)]

本主题介绍 todo 列表快速启动应用程序的脱机同步功能的 Azure 移动服务。 脱机同步允许您轻松地创建应用程序甚至当最终用户无法访问网络时都可用。

脱机同步具有多种潜在的用途︰

* 通过缓存服务器本地设备上的数据来提高应用程序响应能力
* 使应用程序的弹性对间歇性网络连接
* 允许最终用户可以创建和修改数据，即使没有网络访问，与很少或没有连接支持方案
* 在多个设备之间同步数据和两台设备修改同一个记录时检测冲突

>[AZURE.NOTE] 若要完成本教程，您需要一个 Azure 帐户。 如果您没有帐户，可以注册 Azure 的试验，并获得最多 10 个免费的移动服务，您可以继续使用您试用结束后甚至。 有关详细信息，请参阅<a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=AE564AB28" target="_blank">Azure 免费试用版</a>。 
>
> 如果这是您首次使用移动服务体验，您应该先完成[开始使用移动服务]。

本教程将引导您完成以下基本步骤︰

1. [检查移动服务的同步代码]
2. [更新应用程序的同步行为]
3. [更新应用程序重新连接您的移动服务]

本教程要求如下︰

* Visual Studio 具有[Xamarin 扩展名] **或** [Xamarin Studio]在 OS X 上
* XCode 4.5 和 iOS 6.0 （或更高版本） 
* 完成的[移动服务入门]教程

## <a name="review-offline"></a>检查移动服务的同步代码

Azure 移动服务脱机同步允许最终用户无法访问网络时与本地数据库进行交互。 若要在您的应用程序中使用这些功能，您可以初始化`MobileServiceClient.SyncContext`到本地存储区。 然后，引用表通过`IMobileServiceSyncTable`接口。 本部分将通过脱机同步中的相关的代码`QSTodoService.cs`。

1. 在 Visual Studio 中，打开[移动服务入门]教程中已完成的项目。 打开的文件`QSTodoService.cs`。

2. 请注意该类型的成员`todoTable`是`IMobileServiceSyncTable`。 脱机同步使用此同步表接口而不是`IMobileServiceTable`。 使用同步表时，所有操作转到本地存储，并只与使用显式的推送和请求操作的远程服务同步。

    要获取到同步表中，此方法的引用`GetSyncTable()`使用。 若要删除脱机同步功能，而是可以使用`GetTable()`。

3. 表操作之前，必须先初始化本地存储区。 这是`InitializeStoreAsync`方法︰

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }

    这将创建使用类的本地存储区`MobileServiceSQLiteStore`，这移动服务 SDK 中提供。 此外可以通过实现来提供不同的本地存储实现`IMobileServiceLocalStore`。

    `DefineTable`方法与所提供的类型中的字段相匹配的本地存储区中创建一个表`ToDoItem`在这种情况下。 该类型不一定包括所有列在远程数据库中--就可以存储列的子集。

    此重载`InitializeAsync`使用默认冲突处理程序，出现故障时有冲突。 若要提供自定义的冲突处理程序，请参阅本教程[处理离线移动服务支持的冲突]。

4. 方法`SyncAsync`触发实际的同步操作︰

        public async Task SyncAsync()
        {
            try
            {
                await client.SyncContext.PushAsync();
                await todoTable.PullAsync("allTodoItems", todoTable.CreateQuery()); // query ID is used for incremental sync
            }

            catch (MobileServiceInvalidOperationException e)
            {
                Console.Error.WriteLine(@"Sync Failed: {0}", e.Message);
            }
        }

    首先，没有对`IMobileServiceSyncContext.PushAsync()`。 此方法是的成员`IMobileServicesSyncContext`而不是在同步表，因为它将会使跨所有表的更改。 将会以某种方式 （通过 CUD 操作） 本地已修改的记录发送到服务器。

    接下来，该方法将调用`IMobileServiceSyncTable.PullAsync()`拉到应用程序服务器上的表的数据。 请注意，是否有任何挂起的更改的同步上下文中，拉总是发出推第一次。 这是为了确保关系以及本地存储区中的所有表都都一致。 在这种情况下，我们必须显式调用推。

    我们在此示例中，检索所有的记录在远程`TodoItem`表中，但它也可以是通过传递查询来筛选记录。 第一个参数为`PullAsync()`用于增量同步，使用的查询 id`UpdatedAt`以获取那些自上次同步以来修改过的记录的时间戳。 查询 ID 应该是唯一的应用程序中每个逻辑查询的描述性字符串。 若要退出的增量同步，传递`null`作为查询 id。 此命令会检索所有的记录在每个请求操作，这是有可能效率低下。

    >[AZURE.NOTE] 手机信息服务数据库中删除后，请从设备的本地存储区删除记录，则应启用[软删除]。 否则，您的应用程序应定期调用`IMobileServiceSyncTable.PurgeAsync()`清除本地存储区。

    请注意，`MobileServicePushFailedException`的推和拉的操作可能会出现。 下一步的教程中，[处理冲突与离线支持移动服务]，说明如何处理这些同步相关的异常。

5. 在类中`QSTodoService`，该方法`SyncAsync()`后修改数据，这些操作将调用`InsertTodoItemAsync()`， `CompleteItemAsync`。 它也被称为从`RefreshDataAsync()`，以便用户获取最新数据，只要它们执行的刷新笔势。 应用程序还执行一个同步启动，因为`QSTodoListViewController.ViewDidLoad()`调用`RefreshDataAsync()`。

    因为`SyncAsync()`只要修改数据，则会调用此应用程序假定用户处于联机状态，只要他们正在编辑的数据。 在下一部分中，我们将更新应用程序，以便用户可以编辑甚至当他们处于脱机状态。

## <a name="update-sync"></a>更新应用程序的同步行为

在本节中，您将修改该应用程序以便在应用程序启动或插入和更新操作上不同步，但只有在执行刷新笔势时。 然后，将中断与以模拟脱机的情况下移动服务的应用程序连接。 添加数据项时，它们会保存在本地存储，但不是立即同步到移动服务。

1. 打开`QSTodoService.cs`。 注释掉到调用`SyncAsync()`在下列方法︰

    - `InsertTodoItemAsync`
    - `CompleteItemAsync`
    - `RefreshAsync`

    现在，`RefreshAsync()`将仅加载数据从本地存储区，但不是会连接到后端应用程序。

2. 在`QSTodoService.cs`，注释掉的成员定义`applicationURL`， `applicationKey`。 添加以下行，引用无效移动服务 URL:

        const string applicationURL = @"https://your-mobile-service.azure-mobile.xxx/";
        const string applicationKey = @"AppKey";

3. 若要确保数据同步执行的刷新笔势时，编辑方法`QSTodoListViewController.RefreshAsync()`。 添加对的调用`SyncAsync()`调用之前`RefreshDataAsync()`:

        private async Task RefreshAsync ()
        {
            RefreshControl.BeginRefreshing ();

            await todoService.SyncAsync();
            await todoService.RefreshDataAsync (); // add this line

            RefreshControl.EndRefreshing ();

            TableView.ReloadData ();
        }

4. 生成并运行该应用程序。 添加一些新的 todo 项目。 直到它们可被推送到移动服务，仅在本地存储区中存在这些新项目。 客户端应用程序的行为就象连接到移动服务支持，全部创建、 读取、 更新、 删除 (CRUD) 操作。

5. 关闭应用程序并重新启动它以验证您所创建的新项目将保存到本地存储区。

## <a name="update-online-app"></a>更新应用程序重新连接您的移动服务

在本节中将重新连接到移动服务的应用程序。 这模拟应用程序将从脱机状态移动到联机状态的移动服务。 在执行刷新笔势时，数据会同步到您的移动服务。

1. 打开`QSTodoService.cs`。 删除无效的移动服务 URL 并重新添加了正确的 URL 和应用程序项。

2. 重新生成并运行该应用程序。 请注意数据查找用相同作为脱机的情况下，即使该应用程序现在连接到移动服务。 这是因为此应用程序总是使用`IMobileServiceSyncTable`指向本地存储区。

3. 登录到 Microsoft Azure 管理门户并查看您的移动服务的数据库。 如果您的服务使用 JavaScript 后端，您可以浏览移动服务的**数据**选项卡中的数据。 

    如果您移动的服务，为使用.NET 后端在 Visual Studio 转到**服务器资源管理器** -> **Azure** -> **SQL 数据库**。 右击数据库并选择**SQL Server 对象资源管理器中打开**。

    请注意数据*尚未在数据库和本地存储之间同步*。

4. 在应用程序中，可以通过拉下的项列表执行刷新笔势。 这会导致应用程序调用`RefreshDataAsync()`，后者又调用`SyncAsync()`。 这将执行推送和请求操作，首先将本地存储项发送到移动服务，然后从服务检索新的数据。

5. 请检查您的移动服务，以确认更改已同步的数据库。

##摘要

[AZURE.INCLUDE [mobile-services-offline-summary-csharp](../../includes/mobile-services-offline-summary-csharp.md)]

## 下一步行动

* [处理脱机支持移动服务与冲突]

* [如何为 Azure 移动服务使用 Xamarin 组件的客户端]

<!-- Anchors. -->
[检查移动服务的同步代码]: #review-offline
[更新应用程序的同步行为]: #update-sync
[更新应用程序重新连接您的移动服务]: #update-online-app

<!-- Images -->

<!-- URLs. -->
[处理脱机支持移动服务与冲突]: ../mobile-services-xamarin-ios-handling-conflicts-offline-data.md 
[有关数据入门]: mobile-services-ios-get-started-data.md
[开始使用移动服务]: mobile-services-ios-get-started.md
[如何为 Azure 移动服务使用 Xamarin 组件的客户端]: partner-xamarin-mobile-services-how-to-use-client-library.md
[软删除]: mobile-services-using-soft-delete.md

[Xamarin Studio]: http://xamarin.com/download
[Xamarin 扩展名]: http://xamarin.com/visual-studio
 
