---
ms.openlocfilehash: 90669b93953fa0150ff0a9c6ca8fce630945443a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="通知集线器本地化重大新闻教程"
    description="了解如何使用 Azure 服务总线通知集线器发送本地化的重大新闻通知。"
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
    ms.date="08/18/2015" 
    ms.author="wesmc"/>

# 使用通知集线器发送本地化的新闻

> [AZURE.SELECTOR]
- [Windows 应用商店 C#](notification-hubs-windows-store-dotnet-send-localized-breaking-news.md)
- [iOS](notification-hubs-ios-send-localized-breaking-news.md)

##概述

本主题演示如何使用**模板**功能的 Azure 通知集线器广播已本地化的语言和设备的重大新闻通知。 在本教程中开始[使用通知集线器发送新闻]中创建的 Windows 应用商店应用程序。 完成后，您将能够为您感兴趣的类别注册，指定用来接收通知，并接收该语言只推式通知的所选类别的语言。


有这种情况下的两个部分︰

- Windows 应用商店应用程序允许客户端设备指定一种语言，以及订阅不同重大新闻类别;

- 后端上广播通知，使用 Azure 通知集线器**标记**和**模板**feautres。



##先决条件

您必须已完成[使用通知集线器发送新闻]教程和具有的代码可用，因为本教程建立直接在该代码的基础。

您还需要 Visual Studio 2012。


##模板概念

[使用通知集线器发送的新闻]在您生成**标记**用于订阅通知不同的新闻类别的应用程序。
许多应用程序，但是，多个市场为目标，并且需要本地化。 这意味着，通知本身的内容必须本地化和传送到正确的设备组。
本主题中，我们将显示如何使用通知集线器的**模板**功能可以方便地实现本地化的重大新闻通知。

注意︰ 将本地化的通知发送的一种方法是创建多个版本的每个标记。 例如，若要支持英语、 法语和普通话，需要在三个不同的标记世界新闻:"world_en"，"world_fr"，和"world_ch"。 我们不得不将世界新闻的本地化的版本发送到其中每个标记。 本主题中我们使用模板来避免大量标记和发送多个消息的要求。

在高级别上，模板是一种方法来指定特定的设备应如何接收通知。 该模板指定确切有效负载格式通过引用应用程序后端发送的消息的一部分的属性。 在我们的例子中，我们将发送包含所有受支持的语言的区域设置无关消息︰

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

然后，我们将确保设备注册与正确的属性引用的模板。 例如，Windows 应用商店应用程序希望接收简单祝酒消息将注册下列模板︰

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



模板是非常强大的功能，您可以了解更多有关我们[通知集线器指南]文章中。 模板表达式语言引用是我们[通知集线器操作方法为 Windows 应用商店]中。


##应用程序用户界面

现在，我们将修改[使用通知集线器发送的新闻]将发送使用模板的本地化的新闻的主题中创建的新新闻应用程序。


为了适应您的客户端应用程序进行本地化的消息，您必须替换模板注册*本机*登记 （即您的登记指定模板）。


在 Windows 应用商店应用程序︰

更改您的 MainPage.xaml 包括区域设置组合框︰

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="Button_Click" />
    </Grid>

##构建 Windows 应用商店的客户端应用程序

1. 在通知类中，对*StoreCategoriesAndSubscribe*和*SubscribeToCateories*方法中添加区域设置参数。

        public async Task StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            await SubscribeToCategories(locale, categories);
        }

        public async Task SubscribeToCategories(string locale, IEnumerable<string> categories)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
            var template = String.Format(@"<toast><visual><binding template=""ToastText01""><text id=""1"">$(News_{0})</text></binding></visual></toast>", locale);

            await hub.RegisterTemplateAsync(channel.Uri, template, "newsTemplate", categories);
        }

    请注意，而不是调用*RegisterNativeAsync*方法我们调用*RegisterTemplateAsync*︰ 我们注册的模板取决于区域设置的特定通知格式。 因为我们可能想要注册多个模板 （例如一个 toast 通知），另一个用于拼块，以便可以更新或删除它们，对它们进行命名，我们需要我们还会提供模板 ("newsTemplate") 的名称。

    请注意，是否设备注册多个模板相同的标记，标记将导致多个通知传入消息目标传递给设备 （一个用于每个模板）。 相同的逻辑消息已导致多个视觉通知，例如在 Windows 应用商店应用程序中显示徽章和祝酒时，这种行为很有用。

2. 添加下面的方法来检索存储的区域设置︰

        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }

3. 在您的 MainPage.xaml.cs，更新您的按钮单击处理程序通过检索区域设置组合框的当前值并向调用通知类，提供其所示︰

         var locale = (string)Locale.SelectedItem;

         var categories = new HashSet<string>();
         if (WorldToggle.IsOn) categories.Add("World");
         if (PoliticsToggle.IsOn) categories.Add("Politics");
         if (BusinessToggle.IsOn) categories.Add("Business");
         if (TechnologyToggle.IsOn) categories.Add("Technology");
         if (ScienceToggle.IsOn) categories.Add("Science");
         if (SportsToggle.IsOn) categories.Add("Sports");

         await ((App)Application.Current).Notifications.StoreCategoriesAndSubscribe(locale, categories);

         var dialog = new MessageDialog(String .Format("Locale: {0}; Subscribed to: {1}", locale, string.Join(",", categories)));
         dialog.Commands.Add(new UICommand("OK"));
         await dialog.ShowAsync();

4. 最后，在 App.xaml.cs 文件中，请确保更新通知单一实例中的*OnLaunched*方法调用︰

        Notifications.SubscribeToCategories(Notifications.RetrieveLocale(), Notifications.RetrieveCategories());


##从后端发送本地化的通知

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]





## 下一步行动

使用模板的详细信息，请参阅[通知用户使用通知集线器︰ ASP.NET]，[通知用户使用通知集线器︰ 移动服务]还查看[通知集线器指南]。 模板表达式语言的参考是[通知集线器操作方法为 Windows 应用商店]。

<!-- Anchors. -->
[模板概念]: #concepts
[应用程序用户界面]: #ui
[构建 Windows 应用商店的客户端应用程序]: #building-client
[从后端发送通知]: #send
[下一步行动]:#next-steps

<!-- Images. -->





















<!-- URLs. -->
[移动服务]: /develop/mobile/tutorials/get-started
[通知用户采用通知中枢︰ ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[通知用户采用通知中枢︰ 移动服务]: /manage/services/notification-hubs/notify-users
[使用通知集线器发送的新闻]: /manage/services/notification-hubs/breaking-news-dotnet

[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[对于 Windows live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[开始使用移动服务]: /develop/mobile/tutorials/get-started/#create-new-service
[有关数据入门]: /develop/mobile/tutorials/get-started-with-data-dotnet
[开始使用身份验证]: /develop/mobile/tutorials/get-started-with-users-dotnet
[开始使用推式通知]: /develop/mobile/tutorials/get-started-with-push-dotnet
[推式通知给应用程序用户]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[授权用户使用的脚本]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript 和 HTML]: /develop/mobile/tutorials/get-started-with-push-js

[Azure 的管理门户]: https://manage.windowsazure.com/
[wns 对象]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[通知集线器指南]: http://msdn.microsoft.com/library/jj927170.aspx
[如何为 iOS 的集线器通知]: http://msdn.microsoft.com/library/jj927168.aspx
[为 Windows 应用商店的通知集线器操作方法]: http://msdn.microsoft.com/library/jj927172.aspx
