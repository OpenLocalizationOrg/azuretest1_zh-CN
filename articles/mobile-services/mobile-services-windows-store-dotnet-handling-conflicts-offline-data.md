---
ms.openlocfilehash: 484f799a8ae363a2eac1233c3eca4262deb5d59b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="处理冲突与通用的 Windows 应用程序中脱机数据 |Microsoft Azure" 
    description="了解如何在同步脱机数据通用 Windows 应用程序时使用 Azure 移动服务处理冲突" 
    documentationCenter="windows" 
    authors="wesmc7777" 
    manager="dwrede" 
    editor="" 
    services="mobile-services"/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/23/2015" 
    ms.author="glenga"/>


# 处理与中移动服务脱机数据同步冲突

[AZURE.INCLUDE [mobile-services-selector-offline-conflicts](../../includes/mobile-services-selector-offline-conflicts.md)]

##概述

本主题演示如何同步数据和处理冲突时使用 Azure 移动服务的脱机功能。

如果您更喜欢观看视频，剪辑下面遵循本教程中的相同步骤。

> [AZURE.VIDEO build-offline-apps-azure-mobile-services]

在本教程中，您可以下载一个通用 Windows C# 解决方案的应用程序支持脱机同步冲突处理。 您将移动服务相结合的应用程序，并运行 Windows 商店 8.1 和 Windows Phone 8.1 客户端生成同步冲突并解决它。

本教程中的步骤，并[开始使用脱机数据]上一教程中的示例应用程序为基础。 在开始本教程之前，应首先完成[开始使用脱机数据]。


##先决条件

本教程需要运行在 Windows 8.1 的 Visual Studio 2013年。


##下载样例项目

![][0]

本教程是一个演练，该演练的[Todo 脱机移动服务示例]处理如何同步本地脱机存储区和 Azure 中的手机信息服务数据库之间的冲突。

1. [移动服务示例 GitHub 资料库]下载 zip 文件并将其提取到工作目录。 

2. 如果您还没有安装 SQLite 的 Windows 8.1 和[离线数据入门]教程中所述的 Windows Phone 8.1，，安装两个运行时。

3. 在 Visual Studio 2013，打开*mobile-services-samples\TodoOffline\WindowsUniversal\TodoOffline-Universal.sln*解决方案文件。 按**F5**键以重新生成并运行该项目。 验证还原 NuGet 程序包和引用都已正确设置。

    >[AZURE.NOTE] 您可能需要删除任何旧的 SQLite 运行时参考，并将它们替换为更新的参考，[离线数据入门]教程中所述。

4. 在应用程序中中**插入 TodoItem**，请, 键入一些文本，然后单击**保存**，将一些 todo 项添加到本地存储。 然后关闭该应用程序。

请注意，应用程序不是尚未连接到任何移动的服务，因此**推送**和**拉入**按钮将引发异常。




##测试应用程序对您的移动服务

现在是时间来测试对移动服务的应用程序。

1. 在 Azure 管理门户中，通过单击命令栏上的**仪表板**选项卡**管理密钥**中查找您的移动服务的应用程序键。 复制**应用程序注册表项**。

2. 在解决方案资源管理器中的 Visual Studio，打开客户端示例项目中的 App.xaml.cs 文件。 更改的**MobileServiceClient**使用移动服务 URL 初始化和应用程序键︰

         public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://your-mobile-service.azure-mobile.net/",
            "Your AppKey"
        );

3. 在 Visual Studio 中，请按**F5**键以生成并再次运行该应用程序。

    ![][0]


##更新创建冲突后端中的数据

在实际方案中，当一个应用程序将更新推送到数据库中的记录，然后另一个应用程序尝试将更新推送到使用过时的版本字段在该记录中的同一条记录时将发生同步冲突。 如果您还记得从[开始使用脱机数据]，支持进行同步的脱机功能需要的版本系统属性。 与每个数据库更新检查，此版本信息。 如果试图更新记录使用的是过时的版本应用程序的一个实例，冲突将会出现，并且被捕获为`MobileServicePreconditionFailedException`应用程序中。 如果应用程序不能捕获`MobileServicePreconditionFailedException`然后`MobileServicePushFailedException`被最终将引发描述遇到多少同步错误。

>[AZURE.NOTE] 若要支持同步脱机数据同步已删除的记录，您应该启用[软删除](mobile-services-using-soft-delete.md)。 否则，您必须手动删除记录，在本地存储区或调用`IMobileServiceSyncTable::PurgeAsync()`清除本地存储区。


下面的步骤演示了 Windows Phone 8.1 和 Windows 商店 8.1 客户端运行在相同的时间导致或解决冲突使用示例。

1. 在 Visual Studio 中，右击 Windows Phone 8.1 项目，然后单击**设为启动项目**。 然后按**Ctrl + F5**键来运行 Windows Phone 8.1 客户端而不调试。 一旦有 Windows Phone 8.1 客户端并运行在仿真器中，单击**拉出**按钮来同步本地存储与数据库的当前状态。
 
    ![][3]
 
   
2. 在 Visual Studio 中，右击 Windows 8.1 运行时项目，然后单击**设为启动项目**可将其设置回项目启动。 然后按**f5 键**以运行它。 一旦有 Windows 存储 8.1 客户端并运行，请单击**拉出**按钮来同步本地存储与数据库的当前状态。

    ![][4]
 
3. 在此点点两个客户端与数据库同步。 这两个客户端的代码还使用增量同步，以便它们就只需要同步未完成 todo 项。 已完成的 todo 项将被忽略。 选择一个项目并编辑中为不同的值的两个客户端的同一项的文本。 单击**推送**按钮来同步这两个更改与服务器上的数据库。

    ![][5]

    ![][6]


4. 其强制执行客户端最后一次遇到冲突，并允许用户决定要提交到数据库的值。 该异常提供了用于解决冲突的正确版本值。

    ![][7]



##用于处理同步冲突的代码审阅

为对移动服务使用脱机功能，必须在版本列包含在您的本地数据库和数据传输对象。 这可以通过更新`TodoItem`类的下列成员︰

        [Version]
        public string Version { get; set; }

`__version`中的本地数据库中包含该列`OnNavigatedTo()`方法在`TodoItem`类用于定义本地存储区。

来处理代码中的脱机同步冲突，您可以创建一个实现类`IMobileServiceSyncHandler`。 这种类型的对象传递到调用中`MobileServiceClient.SyncContext.InitializeAsync()`。 这也发生在`OnNavigatedTo()`的示例方法。

     await App.MobileService.SyncContext.InitializeAsync(store, new SyncHandler(App.MobileService));

此类`SyncHandler`在**SyncHandler.cs**实现`IMobileServiceSyncHandler`。 方法`ExecuteTableOperationAsync`每个推送操作发送到服务器时，将调用。 如果类型的异常`MobileServicePreconditionFailedException`抛出时，这意味着，没有一个项目的本地和远程版本之间的冲突。

要解决冲突，而本地项目，只是应该重试该操作。 一旦发生冲突，本地项版本将被更新以匹配服务器版本，因此执行此操作会覆盖本地更改与服务器更改︰

    await operation.ExecuteAsync(); 

要解决冲突，而服务器项，只需返回从`ExecuteTableOperationAsync`。 将丢弃并从服务器的值替换对象的本地版本。

若要停止推送操作 （但仍保留排队的更改），使用该方法`AbortPush()`:

    operation.AbortPush();

这将停止当前的推送操作，但将保留所有挂起的更改，包括当前的操作，如果`AbortPush`从调用`ExecuteTableOperationAsync`。 下一次的`PushAsync()`被调用，这些更改将被发送到服务器。 

取消后推，`PushAsync`将引发`MobileServicePushFailedException`，和 exception 属性`PushResult.Status`的值为`MobileServicePushStatus.CancelledByOperation`。 



<!-- Images -->
[0]: ./media/mobile-services-windows-store-dotnet-handling-conflicts-offline-data/mobile-services-handling-conflicts-app-run1.png
[1]: ./media/mobile-services-windows-store-dotnet-handling-conflicts-offline-data/javascript-backend-database.png
[2]: ./media/mobile-services-windows-store-dotnet-handling-conflicts-offline-data/dotnet-backend-database.png
[3]: ./media/mobile-services-windows-store-dotnet-handling-conflicts-offline-data/wp81-view.png
[4]: ./media/mobile-services-windows-store-dotnet-handling-conflicts-offline-data/win81-view.png
[5]: ./media/mobile-services-windows-store-dotnet-handling-conflicts-offline-data/wp81-edit-text.png
[6]: ./media/mobile-services-windows-store-dotnet-handling-conflicts-offline-data/win81-edit-text.png
[7]: ./media/mobile-services-windows-store-dotnet-handling-conflicts-offline-data/conflict.png




<!-- URLs -->
[处理冲突的代码的示例]: http://go.microsoft.com/fwlink/?LinkId=394787
[开始使用移动服务]: ../mobile-services-windows-store-get-started.md
[开始使用脱机数据]: mobile-services-windows-store-dotnet-get-started-offline-data.md
[SQLite windows 8.1]: http://go.microsoft.com/fwlink/?LinkId=394776
[Azure 的管理门户]: https://manage.windowsazure.com/
[处理数据库冲突]: mobile-services-windows-store-dotnet-handle-database-conflicts.md#test-app
[移动服务示例 GitHub 存储库]: http://go.microsoft.com/fwlink/?LinkId=512865
[托多离线移动服务示例]: http://go.microsoft.com/fwlink/?LinkId=512866
 