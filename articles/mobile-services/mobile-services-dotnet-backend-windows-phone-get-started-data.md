---
ms.openlocfilehash: 075c1894eaca985ccf47f485e2ef2f021055e69e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="将移动服务添加到一个现有的应用程序 (Windows Phone) |Microsoft Azure"
    description="了解如何开始使用移动服务利用 Windows Phone 应用程序中的数据。"
    services="mobile-services"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/18/2015" 
    ms.author="wesmc"/>

# 添加到现有应用程序的移动服务

##概述

[AZURE.INCLUDE [mobile-services-selector-get-started-data-legacy](../../includes/mobile-services-selector-get-started-data-legacy.md)]

本主题演示如何为 Windows Phone 8.1 Silverlight 应用程序的后端数据源添加 Azure 移动服务。 在本教程中，将下载的应用程序用于将数据存储在内存中的 Visual Studio 项目、 创建新的移动服务、 移动服务相结合的应用程序，并查看运行应用程序时所做的数据更改。 若要添加到 Windows Phone 商店 8.1 应用程序移动服务，请参阅[教程的此版本](mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data.md)。

您在本教程中创建的移动服务在手机信息服务支持.NET 运行库。 这将允许您使用.NET 语言和 Visual Studio 中移动服务的服务器端业务逻辑。 若要创建可以在 JavaScript 编写服务器端业务逻辑的移动服务，请参阅此主题的[JavaScript 后端版本](mobile-services-windows-phone-get-started-data.md)。


##先决条件

本教程要求如下︰

+ Visual Studio 2013年更新 2年或更高版本。
+ 一种 Microsoft Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅<a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-windows-store-dotnet-get-started-data%2F" target="_blank">Azure 免费试用版</a>。

##下载 GetStartedWithData 项目

本教程是基于[GetStartedWithMobileServices 的应用程序](https://code.msdn.microsoft.com/Add-Azure-Mobile-to-a-8b906f72)，它是用于 Visual Studio 2013年的 Windows Phone Silverlight 8.1 应用程序项目。  

1. 从[开发人员代码示例网站](https://code.msdn.microsoft.com/Add-Azure-Mobile-to-a-8b906f72)下载 C# 版本的 GetStartedWithMobileServices 示例应用程序。

    >[AZURE.NOTE]若要创建一个 Windows Phone Silverlght 8.1 应用程序，只需到 Windows Phone 8.1 更改目标操作系统下载 Windows Phone Silverlight 8 应用程序项目中。 要创建一个 Windows Phone 存储应用程序，请下载 GetStartedWithData 示例应用程序项目的[Windows Phone 存储应用程序版本](http://go.microsoft.com/fwlink/p/?LinkId=397372)。

2. 使用管理权限运行 Visual Studio，通过右击 Visual Studio，然后单击**以管理员身份运行**。

3. 在 Visual Studio，打开下载的项目并检查 MainPage.xaml.cs 文件。

    请注意，添加**TodoItem**对象存储在内存中**ObservableCollection&lt;TodoItem&gt;**。

4. 在 Visual Studio 中，选择该应用程序的部署目标。 您可以部署到 Windows Phone 设备或仿真程序是 Windows Phone SDK 中包含的一个。 在本教程中，我们演示部署到仿真程序。

5. 按**F5**键。 这将生成、 部署和启动应用程序以进行调试。

6. 在应用程序中，在文本框中，键入一些文本，然后单击**保存**以保存在应用程序中的几项在内存中。

    ![使用内存中存储的应用程序](./media/mobile-services-dotnet-backend-windows-phone-get-started-data/app-view.png)

    请注意，每个文字`TodoItem`所示刷新按钮和一个复选框，允许您将标记项已完成。

##创建新的移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]


##下载的移动服务项目并将其添加到解决方案中

1. 如果您还没有这样做，下载并安装[Visual Studio 专业版 2013年](https://go.microsoft.com/fwLink/p/?LinkID=391934)或更高版本。

2. 在[Azure 管理门户](https://manage.windowsazure.com/)中，单击您的新移动服务，在快速启动页上单击**Windows**平台上，然后下**开始**展开**连接现有 Windows 或 Windows Phone 应用程序**。

    ![下载的移动服务项目](./media/mobile-services-dotnet-backend-windows-phone-get-started-data/download-service-project.png)

4. 在**下载并发布到云服务**，单击**下载**。

    这会将下载的 Visual Studio 项目的实现移动服务。

4. 解压缩下载的个性化的服务初学者解决方案和 zip 文件到相同的**C#**目录开始使用数据解决方案文件 (.sln) 所在的位置中的文件夹复制。 这使得更容易为 NuGet 程序包管理器，以使所有包保持同步。

5. Visual Studio 解决方案资源管理器中，右在您的解决方案的数据 Windows 应用商店应用程序入门知识。 单击**添加**，然后单击**现有项目**。

6. 在**添加现有项目**对话框中，导航到您移动到**C#**目录的移动服务项目文件夹，选择 C# 项目中的文件 (.csproj) 服务的子目录，然后单击**打开**将该项目添加到您的解决方案。

7. 在解决方案资源管理器中的 Visual Studio，右键单击刚添加的服务项目，并单击**生成**以验证它生成，没有错误。 在生成期间，NuGet 程序包管理器可能需要还原的项目中引用一些 NuGet 程序包。

8. 再次右键单击该服务项目。 这一次**调试**上下文菜单下单击**启动新实例**。

    Visual Studio 会打开默认 web 页为您的服务。 您可以单击**立即尝试**着从默认 web 页移动服务中测试方法。

    ![移动服务尝试它现在页](./media/mobile-services-dotnet-backend-windows-phone-get-started-data/service-welcome-page.png)

    Visual Studio 托管默认情况下 IIS Express 在本地移动服务。 这可以通过右击任务栏图标的 IIS Express 在任务栏上可以看到。


##更新 Windows Phone 应用程序要使用移动服务

在本节中将更新 Windows Phone 应用程序作为应用程序的后端服务使用移动服务。


1. 在解决方案资源管理器在 Visual Studio 中，右击 Windows Phone 应用程序项目，然后单击**管理 NuGet 程序包**。

2. 在管理 NuGet 程序包对话框中，搜索在线包集合中的**WindowsAzure.MobileServices**并单击以将 Azure 移动服务 Nuget 程序包安装。 然后关闭对话框。

    ![安装 NuGet 程序包](./media/mobile-services-dotnet-backend-windows-phone-get-started-data/manage-nuget-packages.png)

3. 回在 Azure 管理门户中，查找标有**连接您的应用程序并将数据存储在您的服务**的步骤。 将复制的代码段的创建`MobileServiceClient`连接。

    ![连接您的应用程序的代码段](./media/mobile-services-dotnet-backend-windows-phone-get-started-data/copy-mobileserviceclient-snippet.png)

4. 在 Visual Studio 中，打开 App.xaml.cs。 将代码段粘贴在开始处`App`类定义。 此外将添加以下`using`顶部的语句中的那个文件和保存文件。

        using Microsoft.WindowsAzure.MobileServices;

5. 在 Visual Studio，打开 MainPage.xaml.cs，添加 using 语句在文件的顶部︰

        using Microsoft.WindowsAzure.MobileServices;

6. 在 MainPage.xaml.cs 中的 Visual Studio，替换`MainPage`类定义中的添加以下定义并保存该文件。

    此代码使用移动服务 SDK 来启用应用程序所提供的服务而不存储表中存储数据的本地内存中。 主要的三种方法是`InsertTodoItem`， `RefreshTodoItems`，和`UpdateCheckedTodoItem`。 这三种方法，可以异步插入查询和更新到 Azure 中的表的数据收集。

        public sealed partial class MainPage : PhoneApplicationPage
        {
            private MobileServiceCollection<TodoItem, TodoItem> items;
            private IMobileServiceTable<TodoItem> todoTable =
                App.MobileService.GetTable<TodoItem>();
            public MainPage()
            {
                this.InitializeComponent();
            }
            private async void InsertTodoItem(TodoItem todoItem)
            {
                await todoTable.InsertAsync(todoItem);
                items.Add(todoItem);
            }
            private async void RefreshTodoItems()
            {
                items = await todoTable
                    .ToCollectionAsync();
                ListItems.ItemsSource = items;
            }
            private async void UpdateCheckedTodoItem(TodoItem item)
            {
                await todoTable.UpdateAsync(item);
            }
            private void ButtonRefresh_Click(object sender, RoutedEventArgs e)
            {
                RefreshTodoItems();
            }
            private void ButtonSave_Click(object sender, RoutedEventArgs e)
            {
                var todoItem = new TodoItem { Text = InputText.Text };
                InsertTodoItem(todoItem);
            }
            private void CheckBoxComplete_Checked(object sender, RoutedEventArgs e)
            {
                CheckBox cb = (CheckBox)sender;
                TodoItem item = cb.DataContext as TodoItem;
                item.Complete = (bool)cb.IsChecked;
                UpdateCheckedTodoItem(item);
            }
            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
                RefreshTodoItems();
            }
        }


##测试 Windows Phone 应用程序与本地承载的服务</h2>

在本部分中将使用 Visual Studio 开发工作站上本地测试的应用程序和手机信息服务。 为了测试 Windows Phone 设备或其中的 Windows Phone 仿真程序的本地承载于 IIS Express 的移动服务，您必须配置 IIS Express 和工作站，以允许连接到工作站的 IP 地址和端口。 以非本地网络客户端的 Windows Phone 设备和仿真程序的连接。

#### 配置 IIS Express 允许远程连接

[AZURE.INCLUDE [mobile-services-how-to-configure-iis-express](../../includes/mobile-services-how-to-configure-iis-express.md)]

#### 在 IIS Express 测试针对移动服务应用程序

6. 在 Visual Studio 中打开 App.xaml.cs 文件和注释掉`MobileService`最近粘贴到该文件的定义。 添加一个新的定义，使根据 IP 地址和端口，您在工作站上配置的连接。 然后保存该文件。 代码看起来应类似于下面...

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "http://192.168.111.11:54321");

        //public static MobileServiceClient MobileService = new MobileServiceClient(
        //    "https://todolist.azure-mobile.net/",
        //    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
        //);


7. 在 Visual Studio，按 F7 键或单击生成菜单来构建 Windows Phone 应用程序和移动服务中**生成解决方案**。 验证这两个项目生成，没有错误在 Visual Studio 的输出窗口

8. 在 Visual Studio，按 F5 键或单击**开始调试**从调试菜单来运行应用程序和主机在本地 IIS Express 的移动服务。

    >[AZURE.NOTE] 请您在未运行 Visual Studio 使用**以管理员身份运行**选项。 否则，IIS Express 可能无法加载 applicationhost.config 更改。

9. 输入新 todoitem 的文本。 然后单击**保存**。 这移动在 IIS Express 本地承载的服务创建的数据库中插入新的 todoItem。 单击复选框以将其标记为已完成的项目之一。

10. 在 Visual Studio 中停止调试应用程序。 打开服务器资源管理器中，展开数据连接的后端服务创建的数据库中，可以查看所做的更改。 右击下**MS_TableConnectionString**的 TodoItems 表，单击**显示表格数据**

    ![在表中显示数据]([14]: ./media/mobile-services-dotnet-backend-windows-phone-get-started-data/vs-show-local-table-data.png)

11. 一旦完成了与本地承载移动服务与测试后，删除您创建的 Windows 防火墙规则打开您的工作站上的端口。


##发布到 Azure 的移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]


##<a name="test-azure-hosted"></a>测试发布到 Azure 的移动服务

1. 在 Visual Studio 中，打开 App.xaml.cs。  注释掉的代码，创建`MobileServiceClient`连接到本地承载的移动服务。 取消注释创建的代码， `MobileServiceClient` ，连接到您的 Azure 服务。 保存对文件所做的更改。

        sealed partial class App : Application
        {
            //public static MobileServiceClient MobileService = new MobileServiceClient(
            //          "http://192.168.111.11:54321");

            // Use this constructor instead after publishing to the cloud
            public static MobileServiceClient MobileService = new MobileServiceClient(
                 "https://todolist.azure-mobile.net/",
                 "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
            );
            ....

2. 在 Visual Studio，按 F5 键或单击调试菜单中的**开始调试**。 这将导致与以前的更改运行应用程序来连接到远程宿主在 Azure 中的移动服务之前需要重新生成该应用程序。

3. 输入一些新的 todoitems，并单击**保存**为每个。 单击复选框来完成的一些新项目。 每个新的 todoItem 并存储在以前为您的移动服务在 Azure 管理门户配置 SQL 数据库中更新。

    您可以重新启动该应用程序以查看所做的更改已保存到 Azure 中的数据库。 您还可以检查使用 Azure 管理门户或 Visual Studio SQL Server 对象资源管理器的数据库。 在接下来的两个步骤将使用 Azure 管理门户数据库中查看所做的更改。


4. 在 Azure 管理门户中，单击与您的移动服务关联的数据库管理。

    ![管理 SQL 数据库](./media/mobile-services-dotnet-backend-windows-phone-get-started-data/manage-sql-azure-database.png)

5. 管理门户中执行一个查询，以查看该应用程序所做的更改。 您的查询将会类似于下面的查询，但使用数据库的名称而不是`todolist`。

        SELECT * FROM [todolist].[todoitems]

    ![在 SQL 数据库中查询](./media/mobile-services-dotnet-backend-windows-phone-get-started-data/sql-azure-query.png)

**有关数据入门**教程到此结束。

##下一步行动

本教程说明了启用 Windows Phone 8 应用程序中使用.Net 运行时构建的移动服务的数据处理的基础知识。 接下来，尝试下列任一其他教程︰

* [开始使用身份验证]
  <br/>了解如何对您的应用程序的用户进行身份验证。

* [将推式通知添加到您的应用程序]()
  <br/>了解如何将非常基本的推式通知发送到您的应用程序。

* [移动服务.NET 帮助概念参考]
  <br/>了解更多关于如何使用.NET 的移动服务。



<!-- Images. -->

<!-- URLs. -->
[开始使用移动服务]: ../mobile-services-dotnet-backend-windows-phone-get-started.md
[有关数据入门]: mobile-services-dotnet-backend-windows-phone-get-started-data.md
[开始使用身份验证]: mobile-services-dotnet-backend-windows-phone-get-started-users.md
[开始使用推式通知]: mobile-services-dotnet-backend-windows-phone-get-started-push.md

[Windows Phone 8 SDK]: http://go.microsoft.com/fwlink/p/?linkid=268374
[Azure 的管理门户]: https://manage.windowsazure.com/
[管理门户]: https://manage.windowsazure.com/
[移动服务 SDK]: http://go.microsoft.com/fwlink/p/?LinkId=257545
[开发人员代码示例站点]:  https://code.msdn.microsoft.com/Add-Azure-Mobile-to-a-8b906f72
[移动服务.NET 帮助概念参考]: mobile-services-windows-dotnet-how-to-use-client-library.md
[MobileServiceClient 类]: http://go.microsoft.com/fwlink/p/?LinkId=302030
[如何添加新的 Windows 防火墙端口规则]:  http://go.microsoft.com/fwlink/?LinkId=392240

测试
