---
ms.openlocfilehash: 2426f68a88af2a20a38b843ae0a797b8134281e4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="处理冲突与离线数据中移动服务 (Windows Phone) |Microsoft Azure" 
    description="了解如何使用 Azure 移动服务处理冲突时同步脱机 Windows 手机应用程序中的数据" 
    documentationCenter="windows" 
    authors="wesmc7777" 
    manager="dwrede" 
    editor="" 
    services="mobile-services"/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/15/2015" 
    ms.author="wesmc"/>


# 处理与中移动服务脱机数据同步冲突

[AZURE.INCLUDE [mobile-services-selector-offline-conflicts](../../includes/mobile-services-selector-offline-conflicts.md)]

##概述

本主题演示如何同步数据和处理冲突时使用 Azure 移动服务的脱机功能。 在本教程中，您将下载脱机将同时支持应用程序和在线数据，相结合的移动服务的应用程序，然后登录到 Azure 管理门户网站来查看和更新数据库，运行应用程序时。

本教程中的步骤，并[开始使用脱机数据]上一教程中的示例应用程序为基础。 在开始本教程之前，您必须首先完成[开始使用脱机数据]。


##先决条件

本教程需要 Visual Studio 2012 和[Windows Phone 8 SDK]。


##下载样例项目



本教程是基于[处理冲突的代码示例]，它是用于 Visual Studio 2012 的 Windows Phone 8 项目。  
此应用程序的用户界面类似于在教程中[开始使用脱机数据]，应用程序只是没有对每个 TodoItem 中的新日期列。

![][0]


1. 下载 Windows Phone 版本的[处理冲突的代码示例]。 

2. 如果未安装，请安装[针对 Windows Phone 8 SQLite] 。

3. 在 Visual Studio 2012，打开下载的项目。 将引用添加到**Windows Phone**在**SQLite 的 Windows Phone**  > **扩展**。

4. 在 Visual Studio 2012，请按**F5**键以生成并运行该应用程序在调试器中。
 
5. 在应用程序中，键入一些文字来自某些新的 todo 项，然后单击**保存**以保存每个。 您还可以修改添加 todo 项目的截止日期。


请注意，应用程序不是尚未连接到任何移动的服务，因此**推送**和**拉入**按钮将引发异常。


##向数据模型中添加列

在本节中，您将更新您移动服务包括到期的 TodoItem 表的数据库日期列。 该应用程序允许您更改在运行时项目的截止日期，以便可以在本教程后面的一节中生成同步冲突。 

`TodoItem`在 MainPage.xaml.cs 中定义此示例中的类。 请注意类具有以下特性的将目标针对此表的同步操作。

        [DataTable("TodoWithDate")]

更新以包含此表的数据库。

###<a name="dotnet-backend"></a>正在更新数据库的.NET 后端移动服务 

如果您使用.NET 后端的移动服务，请按照以下步骤更新您的数据库的架构。

1. 在 Visual Studio 中打开您的.NET 后端的移动服务项目。
2. 在解决方案资源管理器中的 Visual Studio，在服务项目中，展开**模型**文件夹，然后打开 ToDoItem.cs。 添加`DueDate`，如下所示的属性。

          public class TodoItem : EntityData
          {
            public string Text { get; set; }
            public bool Complete { get; set; }
            public System.DateTime DueDate { get; set; }
          }


3. 在解决方案资源管理器中的 Visual Studio，展开**App_Start**文件夹，并打开 WebApiConfig.cs 文件。 

    在 WebApiConfig.cs 文件中，请注意您默认数据库初始值设定项的类从派生`DropCreateDatabaseIfModelChanges`类。 这意味着对该模型的任何更改将导致表会删除并重新创建，以适应新的模型。 因此，表中的数据将会丢失，表格将重新植入。 修改数据库初始值设定项的种子的方法，以便`Seed()`的初始化函数，如下所示来初始化新的 DueDate 列。 将 WebApiConfig.cs 文件保存。

    >[AZURE.NOTE] 时使用的默认数据库初始值设定项，实体框架将除去并重新创建数据库，在第一个代码模型定义中检测到数据模型更改时。 若要使此数据模型更改和维护数据库中的现有数据，则必须使用代码第一个迁移。 有关详细信息，请参阅[如何使用代码第一个迁移来更新数据模型](mobile-services-dotnet-backend-how-to-use-code-first-migrations.md)。


        new TodoItem { Id = "1", Text = "First item", Complete = false, DueDate = DateTime.Today },
        new TodoItem { Id = "2", Text = "Second item", Complete = false, DueDate = DateTime.Today },

          

4. 在解决方案资源管理器中的 Visual Studio，展开**控制器**文件夹，并打开 ToDoItemController.cs。 重命名`TodoItemController`类为`TodoWithDateController`。 这将更改表操作的 REST 端点。 

        public class TodoWithDateController : TableController<TodoItem>
    

5. 对于 Visual Studio 解决方案资源管理器，右键单击您的.NET 后端的移动服务项目并单击**发布**来发布您的更改。


### <a name="javascript-backend"></a>正在更新数据库的 JavaScript 后端移动服务

对于 JavaScript 后端移动服务，您将添加一个名为**TodoWithDate**的新表。 若要添加 JavaScript 后端移动服务的**TodoWithDate**表，请执行以下步骤。

  1. 登录到[Azure 的管理门户]。 

  2. 导航到您的移动服务的**数据**选项卡。 

  3. 单击**创建**页面底部并创建一个名为**TodoWithDate**的新表。 


##测试应用程序对您的移动服务

现在是时间来测试对移动服务的应用程序。

1. 在 Azure 管理门户中，通过单击命令栏上的**仪表板**选项卡**管理密钥**中查找您的移动服务的应用程序键。 复制**应用程序注册表项**。

2. 在解决方案资源管理器中的 Visual Studio，打开客户端示例项目中的 App.xaml.cs 文件。 更改的**MobileServiceClient**使用移动服务 URL 初始化和应用程序键︰

         public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://your-mobile-service.azure-mobile.net/",
            "Your AppKey"
        );

3. 在 Visual Studio 中，请按**F5**键以生成并运行该应用程序。

4. 像以前一样，在文本框中，键入文本，然后单击**保存**以保存某些新的 todo 项。 这会将数据保存到本地同步表中，但不是到服务器。

    ![][0]

5. 若要查看当前状态的数据库中，登录到[Azure 管理门户]，单击**移动服务**，然后单击移动服务。

  * 如果您使用 JavaScript 后端的移动服务，请单击**数据**选项卡，然后单击**TodoWithDate**表。 单击**浏览**以查看以来我们已不推送更改从应用程序到服务器仍将空，表。.

        ![][1]

  *  如果您使用.NET 后端的移动服务，单击**配置**选项卡，然后单击 SQL 数据库。 单击**管理**在屏幕的底部以登录到 SQL Azure 管理门户网站来查看您的数据库运行类似下面的 SQL 查询。
    
            SELECT * FROM todolist.todowithdate

        ![][2]

     

7. 返回在应用程序中，单击**推送**。

8. 在管理门户中， **TodoItem**表上单击**刷新**。 您现在应该看到您在您的应用程序中输入的数据。

    ![][3]

9. 将为下一节您将两个仿真程序来生成冲突中运行该应用程序的启动并运行**仿真器 WVGA 512 MB** 。

##更新创建冲突后端中的数据

在实际方案中，当那个应用程序将更新推送到数据库中的记录，然后另一个应用程序尝试将更改推送到使用过时的版本字段在该记录中的同一条记录时将发生同步冲突。 如果实例的应用程序尝试更新相同的记录，而无需更新记录中拉出，冲突将会出现，并且被捕获为`MobileServicePreconditionFailedException`应用程序中。  

本节中将在生成冲突的同时运行两个应用程序实例。 


1. 如果**仿真程序 WVGA 512 MB**不仍然启动并运行，请按**Ctrl + F5**以重新启动它。

2. 在 Visual Studio 中，输出设备改为**仿真程序 WVGA**并按**F5**新仿真程序中运行的应用程序的另一个实例。
 
    ![][5]
 
   
3. 在应用程序的第二个实例中，请单击**拉**同步与手机信息服务数据库的本地存储区。 这两个应用程序实例应具有相同的数据。
 
    ![][6]

4. 在应用程序的第二个实例中，请单击该复选框可以完成的一个项目，然后单击**推**，以将更改推送到远程数据库。 在下面的屏幕快照中，**领取 James**已完成指示 James 已领取。 第一个实例的应用程序现在已过期的记录。

    ![][9]

5. 在应用程序的第一个实例，尝试更改过期记录的日期，然后单击**推送**来尝试使用过时的记录更新远程数据库。 在下面屏幕抓图，我们试着安排 James 挑选在**5/10/2014**。

    ![][7]

6. 当单击**推送**按钮以提交日期更改时，您将看到一个对话框框，表明已检测到冲突。 您将被要求如何解决冲突。 选择其中一个选项来解决冲突。

    在下面所示的方案中，James 有已经领取。 因此，没有必要计划在**5/10/2014**为他挑选。 **使用服务器版本**选项将被选中，以便该应用程序的第一个实例必须使用来自服务器的记录更新该记录。 

    ![][8]

##用于处理同步冲突的代码审阅

为设置脱机功能来检测冲突，必须在版本列包含在您的本地数据库和数据传输对象。 此类`TodoItem`具有以下成员︰

        [Version]
        public string Version { get; set; }

列`__version`还在本地数据库中设置中指定`OnNavigatedTo()`方法。

来处理代码中的脱机同步冲突，您可以创建一个实现类`IMobileServiceSyncHandler`。 这种类型的对象传递到调用中`InitializeAsync`:

     await App.MobileService.SyncContext.InitializeAsync(store, new SyncHandler(App.MobileService));

此类`SyncHandler`在**MainPage.xaml.cs**实现`IMobileServiceSyncHandler`。 方法`ExecuteTableOperationAsync`每个推送操作发送到服务器时，将调用。 如果类型的异常`MobileServicePreconditionFailedException`抛出时，这意味着，没有一个项目的本地和远程版本之间的冲突。

要解决冲突，而本地项目，只是应该重试该操作。 一旦发生冲突，本地项版本将被更新以匹配服务器版本，因此执行此操作会覆盖本地更改与服务器更改︰

    await operation.ExecuteAsync(); 

要解决冲突，而服务器项，只需返回从`ExecuteTableOperationAsync`。 将丢弃并从服务器的值替换对象的本地版本。

若要停止推送操作 （但仍保留排队的更改），使用该方法`AbortPush()`:

    operation.AbortPush();

这将停止当前的推送操作，但将保留所有挂起的更改，包括当前的操作，如果`AbortPush`从调用`ExecuteTableOperationAsync`。 下一次的`PushAsync()`被调用，这些更改将被发送到服务器。 

取消后推，`PushAsync`将引发`MobileServicePushFailedException`，和 exception 属性`PushResult.Status`的值为`MobileServicePushStatus.CancelledByOperation`。 




<!-- Images -->
[0]: ./media/mobile-services-windows-phone-handling-conflicts-offline-data/mobile-services-handling-conflicts-app-run1.png
[1]: ./media/mobile-services-windows-phone-handling-conflicts-offline-data/mobile-services-todowithdate-empty.png
[2]: ./media/mobile-services-windows-phone-handling-conflicts-offline-data/mobile-services-todowithdate-empty-sql.png
[3]: ./media/mobile-services-windows-phone-handling-conflicts-offline-data/mobile-services-todowithdate-push1.png
[5]: ./media/mobile-services-windows-phone-handling-conflicts-offline-data/vs-emulator-wvga.png
[6]: ./media/mobile-services-windows-phone-handling-conflicts-offline-data/two-emulators-synced.png
[7]: ./media/mobile-services-windows-phone-handling-conflicts-offline-data/two-emulators-date-change.png
[8]: ./media/mobile-services-windows-phone-handling-conflicts-offline-data/two-emulators-conflict-detected.png
[9]: ./media/mobile-services-windows-phone-handling-conflicts-offline-data/two-emulators-item-completed.png



<!-- URLs -->
[处理冲突的代码的示例]: http://go.microsoft.com/fwlink/?LinkId=398257
[开始使用移动服务]: ../mobile-services-windows-phone-get-started.md
[开始使用脱机数据]: mobile-services-windows-phone-get-started-offline-data.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[Windows Phone 8 SDK]: http://go.microsoft.com/fwlink/p/?linkid=268374
[SQLite 的 Windows Phone 8]: http://go.microsoft.com/fwlink/?LinkId=397953
[有关数据入门]: mobile-services-windows-phone-get-started-data.md
 