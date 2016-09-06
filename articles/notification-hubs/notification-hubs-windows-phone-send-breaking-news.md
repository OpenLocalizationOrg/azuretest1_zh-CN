---
ms.openlocfilehash: 95dbb15a1ff741cb5c0ac6305ef81652d08ba54f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="使用通知集线器发送新闻 (Windows Phone)"
    description="使用 Azure 通知集线器在登记中使用标记发送到 Windows Phone 应用程序的重大新闻。"
    services="notification-hubs"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/18/2015" 
    ms.author="wesmc"/>

# 使用通知集线器发送的新闻

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##概述

本主题演示如何使用 Azure 通知集线器广播通知 Windows Phone 8.0/8.1 Silverlight 应用程序的重大新闻。 如果您面向 Windows 应用商店或 Windows Phone 8.1 应用程序，请参阅[Windows 通用](notification-hubs-windows-store-dotnet-send-breaking-news.md)版本。 完成后，将能够注册的重大新闻类别感兴趣，并在收到这些类别只推式通知。 这种情况下是许多应用程序的常见模式通知需要发送到以前已声明它们，例如 RSS 阅读器、 音乐风扇等应用程序感兴趣的用户组。

通过包含一个或多个_标记_，通知中心中创建登记时启用了广播的方案。 当通知被发送到一个标记时，所有已注册标记的设备将接收到通告。 标记是简单的字符串，因为它们不需要预先设置。 有关标记的详细信息，请参阅[通知集线器指南]。

##先决条件

本主题基于您在[开始使用通知集线器]中创建的应用程序。 在开始学习本教程之前, 您必须已完成[通知集线器入门]。

##向应用程序添加类别选择

第一步是将 UI 元素添加到您现有的主页面，使用户能够选择要注册的类别。 由用户选择的类别存储在该设备中。 当应用程序启动时，设备注册中创建与所选的类别通知网络集线器作为标记。

1. 打开 MainPage.xaml 项目文件，然后替换**网格**元素名为`TitlePanel`，`ContentPanel`与下面的代码︰

        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>

        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>

2. 在项目中创建新的类名为**通知**，将**public**修饰符添加到类定义中，然后将以下**using**语句添加到新的代码文件︰

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;

3. 将以下代码复制到新的**通知**类︰

        private NotificationHub hub;

        public Notifications()
        {
            hub = new NotificationHub("<hub name>", "<connection string with listen access>");
        }

        public async Task StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();

            await SubscribeToCategories(categories);
        }

        public async Task SubscribeToCategories(IEnumerable<string> categories)
        {
            var channel = HttpNotificationChannel.Find("MyPushChannel");

            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
            }

            await hub.RegisterNativeAsync(channel.ChannelUri.ToString(), categories);
        }

    此类使用本地存储区来存储此设备具有接收新闻的类别。 它还包含用于注册这些类别的方法。

4. 在上面的代码中，替换`<hub name>`，`<connection string with listen access>`与您通知中心名称和之前获得的*DefaultListenSharedAccessSignature*的连接字符串的占位符。

    > [AZURE.NOTE] 分布式客户端应用程序使用的凭据不是通常的安全，因为您只应该与您的客户端应用程序分发听访问的键。 倾听 access 将启用不能修改您的应用程序的通知，但现有登记注册并不能发送通知。 完全访问键用于发送通知和更改现有登记的受保护的后端服务。

4. 在 App.xaml.cs 项目文件中，对**应用程序**类中添加以下属性︰

        public Notifications notifications = new Notifications();

    此属性用于创建和访问的**通知**实例。

5. 您 MainPage.xaml.cs 中添加以下行︰

        using Windows.UI.Popups;

6. 在 MainPage.xaml.cs 项目文件中，添加下列方法︰

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldCheckBox.IsChecked == true) categories.Add("World");
            if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
            if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
            if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
            if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
            if (SportsCheckBox.IsChecked == true) categories.Add("Sports");

            await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);

            MessageBox.Show("Subscribed to: " + string.Join(",", categories));
        }

    此方法创建一个类别列表，并使用**通知**类来存储本地存储区中的列表并通知中心注册相应的标记。 更改类别，注册新类别重新创建。

您的应用程序现在就可以在设备上的本地存储区中存储一组类别和注册通知中心，每当用户更改所选内容的类别。

##要通过注册获取通知

这些步骤注册通知中心在启动时将使用已存储本地存储区中的类别。

> [AZURE.NOTE] 因为该信道的 URI 分配由 Microsoft 推送通知服务 (MPNS) 可以在任何时间的机会，您应注册通知经常以避免通知失败。 本示例注册通知每个应用程序启动的时间。 为经常运行的应用程序，不止一次，每天可以可能跳过注册以节省带宽，如果以前的注册起已过了不少于一天。

1. 将以下代码添加到**通知**类︰

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }

    此方法返回的类中定义的类别。

1. 打开 App.xaml.cs 文件，并将**异步**修饰符添加到**Application_Launching**方法。

2. 在**Application_Launching**方法中，找到并使用下面这行代码添加[入门通知集线器]中的通知集线器注册代码替换︰

        await notifications.SubscribeToCategories(notifications.RetrieveCategories());

    这将确保，每次启动应用程序时，它从本地存储区中检索类别，并请求这些类别的注册。

3. 在 MainPage.xaml.cs 项目文件中添加下面的代码实现的**OnNavigatedTo**方法︰

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
        }

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

    ![][2]

4. 通过以下方式之一从后端发送新的通知︰

    + **控制台应用程序︰**启动控制台应用程序。

    + **Java/PHP:**运行您的应用程序/脚本。

    通知所选的类别的好象 toast 通知。

    ![][3]

您已完成本主题。

<!--##Next steps

In this tutorial we learned how to broadcast breaking news by category. Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs to broadcast localized breaking news]

    Learn how to expand the breaking news app to enable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how to push notifications to specific authenticated users. This is a good solution for sending notifications only to specific users.
-->

<!-- Anchors. -->
[向应用程序添加类别选择]: #adding-categories
[要通过注册获取通知]: #register
[从后端发送通知]: #send
[运行应用程序并生成通知]: #test-app
[下一步行动]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[开始使用通知集线器]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[使用通知集线器以本地化的新闻广播]: ../breakingnews-localized-wp8.md
[通知用户使用通知集线器]: /manage/services/notification-hubs/notify-users/
[移动服务]: /develop/mobile/tutorials/get-started
[通知集线器指南]: http://msdn.microsoft.com/library/jj927170.aspx
[对于 Windows Phone 通知集线器操作方法]: ??

[Azure 的管理门户]: https://manage.windowsazure.com/
