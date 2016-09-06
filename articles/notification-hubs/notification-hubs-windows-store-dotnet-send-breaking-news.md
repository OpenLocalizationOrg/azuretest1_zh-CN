---
ms.openlocfilehash: df3d87b2d561116dd113375314cdd1e15bca82bd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="使用通知集线器发送新闻 （Windows 通用）"
    description="使用注册标记 Azure 通知集线器到通用的 Windows 应用程序发送的新闻。"
    services="notification-hubs"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>


<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="wesmc"/>

# 使用通知集线器发送的新闻


[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##概述

本主题演示如何使用 Azure 通知集线器广播重大新闻通知到 Windows 应用商店或 Windows Phone 8.1 (非 Silverlight) 应用程序。 如果您面向 Windows Phone 8.1 Silverlight，请参阅[Windows Phone](notification-hubs-ios-send-breaking-news.md)版本。 完成后，将能够注册的重大新闻类别感兴趣，并在收到这些类别只推式通知。 这种情况下是许多应用程序的常见模式通知需要发送到组的用户以前已声明它们，例如 RSS 阅读器，为乐迷，应用程序感兴趣等。 

通过包含一个或多个_标记_，通知中心中创建登记时启用了广播的方案。 当通知被发送到一个标记时，所有已注册标记的设备将接收到通告。 标记是简单的字符串，因为它们不需要预先设置。 有关标记的详细信息，请参阅[通知集线器指南]。

##先决条件

本主题基于[入门通知集线器][入门]在创建应用程序。 之前开始学习本教程，您必须已完成[通知集线器开始][入门的]。

##向应用程序添加类别选择

第一步是将 UI 元素添加到您现有的主页面，使用户能够选择要注册的类别。 由用户选择的类别存储在该设备中。 当应用程序启动时，设备注册中创建与所选的类别通知网络集线器作为标记。

1. 打开 MainPage.xaml 项目文件，然后将下面的代码复制**网格**元素中︰

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>


2. 右键单击该**共享**项目，并添加新的类名为**通知**，将**public**修饰符添加到类定义中，然后将以下**using**语句添加到新的代码文件︰

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;

3. 将以下代码复制到新的**通知**类︰

        private NotificationHub hub;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public async Task StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            await SubscribeToCategories(categories);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }

        public async Task SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                IEnumerable<string> categories = RetrieveCategories();
            }

            await hub.RegisterNativeAsync(channel.Uri, categories);
        }

    此类使用本地存储区来存储此设备具有接收新闻的类别。 它还包含用于注册这些类别的方法。


4. 在 App.xaml.cs 项目文件中，对**应用程序**类中添加以下属性︰

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    此属性用于创建和访问的**通知**实例。

    在上面的代码中，替换`<hub name>`，`<connection string with listen access>`与您通知中心名称和之前获得的*DefaultListenSharedAccessSignature*的连接字符串的占位符。

    > [AZURE.NOTE] 分布式客户端应用程序使用的凭据不是通常的安全，因为您只应该与您的客户端应用程序分发听访问的键。 倾听 access 将启用不能修改您的应用程序的通知，但现有登记注册并不能发送通知。 完全访问键用于发送通知和更改现有登记的受保护的后端服务。

5. 您 MainPage.xaml.cs 中添加以下行︰

        using Windows.UI.Popups;

6. 在 MainPage.xaml.cs 项目文件中，添加下列方法︰

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);

            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories));
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }

    此方法创建一个类别列表，并使用**通知**类来存储本地存储区中的列表并通知中心注册相应的标记。 更改类别，注册新类别重新创建。

您的应用程序现在就可以在设备上的本地存储区中存储一组类别和注册通知中心，每当用户更改所选内容的类别。

##要通过注册获取通知

这些步骤注册通知中心在启动时将使用已存储本地存储区中的类别。

> [AZURE.NOTE] 该信道的 URI 分配由 Windows 通知服务 (WNS) 可以在任何时候更改，因为您应注册通知经常以避免通知失败。 本示例注册通知每个应用程序启动的时间。 为经常运行的应用程序，不止一次，每天可以可能跳过注册以节省带宽，如果以前的注册起已过了不少于一天。

1. 打开 App.xaml.cs 文件，并将**异步**修饰符添加到**OnLaunched**方法。

2. 在**OnLaunched**方法中，找到并替换现有的**InitNotificationsAsync**方法调用下面这行代码︰

        await notifications.SubscribeToCategories();

    这将确保，每次启动应用程序时，它从本地存储区中检索类别，并请求这些类别的注册。 **InitNotificationsAsync**方法创建的一部分 [入门通知集线器] 教程，但它不需要本主题中。

3. 在 MainPage.xaml.cs 项目文件中的*OnNavigatedTo*方法中添加以下代码︰

        var categories = ((App)Application.Current).notifications.RetrieveCategories();

        if (categories.Contains("World")) WorldToggle.IsOn = true;
        if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
        if (categories.Contains("Business")) BusinessToggle.IsOn = true;
        if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
        if (categories.Contains("Science")) ScienceToggle.IsOn = true;
        if (categories.Contains("Sports")) SportsToggle.IsOn = true;

    这将更新主页基于类别以前保存的状态。

应用程序现已完成，并可以用于每当用户更改的类别选择注册通知中心设备本地存储区中存储一组类别。 接下来，我们将定义后端，可以将类别通知发送到这个应用程序。

##从后端发送通知

[AZURE.INCLUDE [notification-hubs-back-end](../../includes/notification-hubs-back-end.md)]

##运行应用程序并生成通知

1. 在 Visual Studio 中，请按 f5 键以编译并启动应用程序。

    ![][1]

    注意应用程序用户界面提供了一套切换，您可以选择要订阅的类别。

2. 启用一个或多个类别切换，然后单击**订阅**。

    应用程序转换为标记的所选的类别和从通知中心请求为选定的标记新的设备注册。 注册的类别返回并显示在一个对话框中。

    ![][19]

4. 通过以下方式之一从后端发送新的通知︰

    + **控制台应用程序︰**启动控制台应用程序。

    + **Java/PHP:**运行您的应用程序/脚本。

    通知所选的类别的好象 toast 通知。

    ![][14]

##下一步行动

在本教程中我们学习了如何按类别广播的新闻。 完成下面的教程，突出显示其他高级的通知集线器方案之一，请考虑︰

+ [使用通知集线器以本地化的新闻广播]

    了解如何展开重大新闻应用程序启用本地化的发送通知。

+ [通知用户使用通知集线器]

    了解如何对特定的已验证身份用户推式通知。 这是一个不错的解决方案，只能向特定用户发送通知。


<!-- Anchors. -->
[向应用程序添加类别选择]: #adding-categories
[要通过注册获取通知]: #register
[从后端发送通知]: #send
[运行应用程序并生成通知]: #test-app
[下一步行动]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[入门-]: /manage/services/notification-hubs/getting-started-windows-dotnet/
[使用通知集线器以本地化的新闻广播]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[通知用户使用通知集线器]: /manage/services/notification-hubs/notify-users
[移动服务]: /develop/mobile/tutorials/get-started/
[通知集线器指南]: http://msdn.microsoft.com/library/jj927170.aspx
[为 Windows 应用商店的通知集线器操作方法]: http://msdn.microsoft.com/library/jj927172.aspx
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[对于 Windows live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Azure 的管理门户]: https://manage.windowsazure.com/
[wns 对象]: http://go.microsoft.com/fwlink/p/?LinkId=260591
