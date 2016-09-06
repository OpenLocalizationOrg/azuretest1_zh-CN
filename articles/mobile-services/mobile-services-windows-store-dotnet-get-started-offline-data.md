---
ms.openlocfilehash: bae0dd5b78a0b17b03fc99438df6f6d1c65e5989
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在通用的 Windows 应用程序中使用脱机数据 |Microsoft Azure" 
    description="了解如何使用 Azure 移动服务为通用 Windows 应用程序中的缓存和同步脱机数据。" 
    documentationCenter="mobile-services" 
    authors="lindydonna" 
    manager="dwrede" 
    editor="" 
    services="mobile-services"/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/23/2015" 
    ms.author="donnam"/>

# 使用移动服务中的脱机数据同步

[AZURE.INCLUDE [mobile-services-selector-offline](../../includes/mobile-services-selector-offline.md)]

本教程展示如何将离线支持添加到使用 Azure 移动服务世界 Windows 应用商店应用程序。 离线支持，可以与本地数据库进行交互，当您的应用程序处于脱机的情况。 一旦您的应用程序与后端数据库联机，则同步本地更改使用脱机功能。 

如果您更喜欢观看视频，右侧的剪辑将遵循本教程中的相同步骤。

> [AZURE.VIDEO build-offline-apps-with-mobile-services]

在本教程中，您可以更新[移动服务入门]教程以支持 Azure 移动服务脱机功能的通用应用程序项目。 然后将已断开连接的脱机方案中添加数据、 同步到在线的数据库，这些项目，然后登录到 Azure 管理门户网站来查看运行应用程序时所做的数据更改。

>[AZURE.NOTE] 本教程旨在帮助您更好地了解如何移动服务使您能够使用 Azure 存储和检索 Windows 应用商店应用程序中的数据。 如果这是您首次使用移动服务体验，您应该首先完成[移动服务入门]教程。
>
>Visual Studio 2012 的旧 Windows Phone 8 教程是在这里，仍然可以使用[Visual Studio 2012 的 Windows Phone 8 教程]。

##先决条件 

本教程要求如下︰

* Visual Studio 2013 年 Windows 8.1 上运行。
* [开始使用移动服务]完成。
* [Azure 移动服务 SDK 1.3.0 版 （或更高版本）][移动服务 SDK Nuget]
* [Azure 移动服务 SQLite 存储版本 1.0.0 版 （或更高版本）][SQLite 存储 nuget]
* [SQLite windows 8.1](www.sqlite.org/downloads)
* Azure 帐户。 如果您没有帐户，可以注册 Azure 的试验，并获得最多 10 个免费的移动服务，您可以继续使用您试用结束后甚至。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28)。 

## <a name="enable-offline-app"></a>更新应用程序支持脱机功能

Azure 移动服务脱机功能允许您与本地数据库进行交互，当您在脱机情况下与您的移动服务。 若要在您的应用程序中使用这些功能，您可以初始化`MobileServiceClient.SyncContext`到本地存储区。 然后，引用表通过`IMobileServiceSyncTable`接口。 在本教程中，我们使用 SQLite 的本地存储。

>[AZURE.NOTE] 可以跳过此部分，并只会收到从 GitHub 样本库中已经有离线支持，移动服务的示例项目。 与离线支持已启用的样例项目位于这里，[离线任务列表的示例]。

1. 安装 Windows 8.1 和 Windows Phone 8.1 SQLite 运行时。 

    * **Windows 8.1 运行时︰**安装[Windows 8.1 的 SQLite]。
    * **Windows Phone 8.1:**安装[Windows Phone 8.1 的 SQLite]。

    >[AZURE.NOTE] 如果您正在使用 Internet Explorer，单击链接来安装 SQLite 可能会提示您下载一个.zip 文件为.vsix。 将文件保存到您的硬盘而不是.zip 扩展名为.vsix 上的位置。 双单击.vsix 文件，在 Windows 资源管理器来运行安装程序。

2. 在 Visual Studio 中打开[移动服务入门]教程中完成该项目。 安装 Windows 8.1 运行时，Windows Phone 8.1 项目的**WindowsAzure.MobileServices.SQLiteStore** NuGet 程序包。

    * **Windows 8.1:**在解决方案资源管理器中，右击 Windows 8.1 项目，然后单击**管理 Nuget 程序包**运行 NuGet 程序包管理器。 搜索安装的**SQLiteStore** `WindowsAzure.MobileServices.SQLiteStore`软件包。
    * **Windows Phone 8.1:**右击 Windows Phone 8.1 项目然后单击**管理 Nuget 程序包**添加运行 NuGet 程序包管理器。 搜索安装的**SQLiteStore** `WindowsAzure.MobileServices.SQLiteStore`软件包。

    >[AZURE.NOTE] 如果安装创建 SQLite 的较旧版本的引用，就可以删除该重复的引用。 

    ![][2]

2. 在解决方案资源管理器中，右击 Windows 8.1 运行时，Windows Phone 8.1 平台项目的**引用**并确保 SQLite，位于**扩展**部分的参考。 

    ![][1]
    </br>

    **Windows 8.1 运行时**

    ![][11]
    </br>

    **Windows Phone 8.1**

3. SQLite 运行时要求您更改正在生成的项目向**x86**、 **x64**或**ARM**的处理器体系结构。 不支持**任何 CPU** 。 在解决方案资源管理器，单击顶部的解决方案，然后将处理器体系结构下拉框更改为支持您想要测试的设置之一。

    ![][13]

5. 在解决方案资源管理器中的共享项目，打开 MainPage.cs 文件。 取消注释下列文件的顶部使用语句︰

        using Microsoft.WindowsAzure.MobileServices.SQLiteStore;  // offline sync
        using Microsoft.WindowsAzure.MobileServices.Sync;         // offline sync

6. 在 MainPage.cs 中，注释的定义`todoTable`和取消注释以下行调用一个`MobileServicesClient.GetSyncTable()`:

        //private IMobileServiceTable<TodoItem> todoTable = App.MobileService.GetTable<TodoItem>();
        private IMobileServiceSyncTable<TodoItem> todoTable = App.MobileService.GetSyncTable<TodoItem>(); // offline sync


7. 在标记区域中的 MainPage.cs， `Offline sync`，请取消注释方法`InitLocalStoreAsync`， `SyncAsync`。 方法`InitLocalStoreAsync`初始化 SQLite 存储与客户端同步上下文。 

        private async Task InitLocalStoreAsync()
        {
            if (!App.MobileService.SyncContext.IsInitialized)
            {
                var store = new MobileServiceSQLiteStore("localstore.db");
                store.DefineTable<TodoItem>();
                await App.MobileService.SyncContext.InitializeAsync(store);
            }

            await SyncAsync();
        }

        private async Task SyncAsync()
        {
            await App.MobileService.SyncContext.PushAsync();
            await todoTable.PullAsync("todoItems", todoTable.CreateQuery());
        }

8. 在`OnNavigatedTo`事件处理程序取消注释调用`InitLocalStoreAsync`:

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            await InitLocalStoreAsync(); // offline sync
            await RefreshTodoItems();
        }

9. Uncommment 3 对`SyncAsync`的方法中`InsertTodoItem`， `UpdateCheckedTodoItem`，和`ButtonRefresh_Click`:

        private async Task InsertTodoItem(TodoItem todoItem)
        {
            await todoTable.InsertAsync(todoItem);
            items.Add(todoItem);

            await SyncAsync(); // offline sync
        }

        private async Task UpdateCheckedTodoItem(TodoItem item)
        {
            await todoTable.UpdateAsync(item);
            items.Remove(item);
            ListItems.Focus(Windows.UI.Xaml.FocusState.Unfocused);

            await SyncAsync(); // offline sync
        }

        private async void ButtonRefresh_Click(object sender, RoutedEventArgs e)
        {
            ButtonRefresh.IsEnabled = false;

            await SyncAsync(); // offline sync
            await RefreshTodoItems();

            ButtonRefresh.IsEnabled = true;
        }

10. 添加异常处理程序在`SyncAsync`方法︰

        private async Task SyncAsync()
        {
            String errorString = null;

            try
            {
                await App.MobileService.SyncContext.PushAsync();
                await todoTable.PullAsync("todoItems", todoTable.CreateQuery()); // first param is query ID, used for incremental sync
            }

            catch (MobileServicePushFailedException ex)
            {
                errorString = "Push failed because of sync errors: " +
                  ex.PushResult.Errors.Count + " errors, message: " + ex.Message;
            }
            catch (Exception ex)
            {
                errorString = "Pull failed: " + ex.Message +
                  "\n\nIf you are still in an offline scenario, " +
                  "you can try your Pull again when connected with your Mobile Serice.";
            }

            if (errorString != null)
            {
                MessageDialog d = new MessageDialog(errorString);
                await d.ShowAsync();
            }
        }

    我们在此示例中，检索所有的记录在远程`todoTable`，但它也是可以通过传递查询来筛选记录。 第一个参数为`PullAsync`用于增量同步，使用的查询 id`UpdatedAt`以获取仅记录自上次同步以来修改的时间戳。 查询 ID 应该是唯一的应用程序中每个逻辑查询的描述性字符串。 若要退出的增量同步，传递`null`作为查询 id。 此命令会检索所有的记录在每个请求操作，这是有可能效率低下。

    >[AZURE.NOTE] * 到移动服务数据库中删除后从设备的本地存储区中删除记录，则应启用[软删除]。 否则，您的应用程序应定期调用`IMobileServiceSyncTable.PurgeAsync()`清除本地存储区。

    请注意，`MobileServicePushFailedException`的推和拉的操作可能会出现。 它可以进行拉由于抽取操作内部执行的按下，以确保所有表之间的任何关系以及保持一致。 下一步的教程中，[处理冲突与离线支持移动服务]，说明如何处理这些同步相关的异常。

11. 在 Visual Studio 中，请按**F5**键以重新生成并运行该应用程序。 因为它未脱机同步更改，因为它只会在插入，同步操作才能更新和刷新操作的应用程序将行为相同。

## <a name="update-sync"></a>更新应用程序的同步行为

在本节中，将修改应用程序，以便它不会不同步的插入和更新操作，但只有在**刷新**按钮被按下时。 然后，将中断与以模拟脱机的情况下移动服务的应用程序连接。 添加数据项时，它们会保存在本地存储，但不是会同步到移动服务。

1. 打开共享项目中的 MainPage.cs。 编辑方法`InsertTodoItem`和`UpdateCheckedTodoItem`为注释掉到调用`SyncAsync`。

2. 编辑共享项目中的 App.xaml.cs。 注释掉**MobileServiceClient**的初始化，然后添加以下行，使用无效的移动服务 URL:

         public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://your-mobile-service.azure-mobile.xxx/",
            "AppKey"
        );

3. 在`InitLocalStoreAsync()`，注释掉对`SyncAsync()`，以便应用程序不会执行同步启动。

4. 按**f5 键**以生成并运行应用程序。 输入一些新的 todo 项，每个中单击**保存**。 直到它们可被推送到手机信息服务，仅在本地存储区中存在新的 todo 项。 客户端应用程序的行为就像它连接到支持所有移动服务的创建、 读取、 更新、 删除 (CRUD) 操作。

5. 关闭应用程序并重新启动它以验证您所创建的新项目将保存到本地存储区。

## <a name="update-online-app"></a>更新应用程序重新连接您的移动服务

在这一节中您重新连接到移动服务的应用程序。 这模拟应用程序将从脱机状态移动到联机状态的移动服务。 按刷新按钮时，数据将被同步到您的移动服务。

1. 打开共享项目中的 App.xaml.cs。 取消注释您上次初始化`MobileServiceClient`来重新添加正确的移动服务 URL 和应用程序键。

2. 按**F5**键以重新生成并运行该应用程序。 请注意数据查找用相同作为脱机的情况下，即使该应用程序现在连接到移动服务。 这是因为此应用程序中始终适用于`IMobileServiceSyncTable`指向本地存储区。

3. 登录到 Microsoft Azure 管理门户并查看您的移动服务的数据库。 如果您的服务使用 JavaScript 后端的移动服务，则可以浏览移动服务的**数据**选项卡中的数据。 

    如果您移动的服务，为使用.NET 后端在 Visual Studio 转到**服务器资源管理器** -> **Azure** -> **SQL 数据库**。 右击数据库并选择**SQL Server 对象资源管理器中打开**。

    注意到尚未在数据库和本地存储之间同步数据。

    ![][6]

4. 在应用程序中，请按**刷新**按钮。 这会导致应用程序调用`PushAsync`， `PullAsync`。 该压入操作将本地存储项发送到移动的服务，然后从移动服务中检索新的数据。

    推送操作执行关闭`MobileServiceClient.SyncContext`而不是`IMobileServicesSyncTable`，将更改与该同步上下文相关联的所有表上。 这是为了涵盖方案表之间有关系。

    ![][7]

5. 在应用程序中，单击以完成它们在本地存储区中的几个项目旁边的复选框。 

    ![][8]

6. 按**刷新**按钮，这会导致`SyncAsync`调用。 `SyncAsync` 调用推和拉，但在这种情况下我们可能已经删除调用`PushAsync`。 这是因为，**总是拉 does 推第一次**。 这是为了确保关系以及本地存储区中的所有表都保持一致。

    ![][10] 
  

##摘要

[AZURE.INCLUDE [mobile-services-offline-summary-csharp](../../includes/mobile-services-offline-summary-csharp.md)]

## 下一步行动

* [处理脱机支持移动服务与冲突]

* [使用软中移动服务中删除][软删除]

<!-- Anchors. -->
[更新应用程序支持脱机功能]: #enable-offline-app
[更新应用程序的同步行为]: #update-sync
[更新应用程序重新连接您的移动服务]: #update-online-app
[下一步行动]:#next-steps

<!-- Images -->
[0]: ./media/mobile-services-windows-store-dotnet-get-started-data-vs2013/mobile-todoitem-data-browse.png
[1]: ./media/mobile-services-windows-store-dotnet-get-started-offline-data/mobile-services-add-reference-sqlite-dialog.png
[2]: ./media/mobile-services-windows-store-dotnet-get-started-offline-data/mobile-services-sqlitestore-nuget.png
[6]: ./media/mobile-services-windows-store-dotnet-get-started-offline-data/mobile-data-browse.png
[7]: ./media/mobile-services-windows-store-dotnet-get-started-offline-data/mobile-data-browse2.png
[8]: ./media/mobile-services-windows-store-dotnet-get-started-offline-data/mobile-services-online-app-run2.png
[10]: ./media/mobile-services-windows-store-dotnet-get-started-offline-data/mobile-data-browse3.png
[11]: ./media/mobile-services-windows-store-dotnet-get-started-offline-data/mobile-services-add-wp81-reference-sqlite-dialog.png
[12]: ./media/mobile-services-windows-store-dotnet-get-started-offline-data/new-synchandler-class.png
[13]: ./media/mobile-services-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[处理脱机支持移动服务与冲突]: mobile-services-windows-store-dotnet-handling-conflicts-offline-data.md 
[离线任务列表的示例]: http://go.microsoft.com/fwlink/?LinkId=394777
[开始使用移动服务]: /develop/mobile/tutorials/get-started/#create-new-service
[入门教程]: ../mobile-services-dotnet-backend-windows-phone-get-started.md
[有关数据入门]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-data.md
[开始使用移动服务]: ../mobile-services-windows-store-get-started.md
[SQLite windows 8.1]: http://go.microsoft.com/fwlink/?LinkId=394776
[SQLite 的 Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkId=397953
[Visual Studio 2012 的 Windows Phone 8 教程]: mobile-services-windows-phone-get-started-offline-data.md
[软删除]: mobile-services-using-soft-delete.md


[移动服务 SDK Nuget]: http://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.0
[SQLite 商店 nuget]: http://www.nuget.org/packages/WindowsAzure.MobileServices.SQLiteStore/1.0.0
 
