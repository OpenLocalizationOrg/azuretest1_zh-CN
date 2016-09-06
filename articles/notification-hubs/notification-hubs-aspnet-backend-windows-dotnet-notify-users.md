---
ms.openlocfilehash: 769b0ea3afbdf987b462453b36deb21a7fc48e0f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure 通知集线器通知用户"
    description="了解如何在 Azure 中发送安全推式通知。 用 C# 使用.NET API 编写的代码样本。"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    services="notification-hubs"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="wesmc"/>

#Azure 通知集线器通知用户

[AZURE.INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]


##概述

在 Azure 支持推送通知使您可以访问一个易于使用、 多平台和向外扩展的推的基础结构，极大地简化了使用者和企业应用程序的移动平台的推式通知的实现。 本教程展示如何使用 Azure 通知集线器上特定设备的特定应用程序用户发送推式通知。 ASP.NET WebAPI 后端用于客户端身份验证。 使用经过身份验证的客户端用户和标记将自动添加由后端到通知的注册。 此标记用于发送由后端生成针对特定用户的通知。 注册使用应用程序的后端的通知的详细信息，请参阅[注册您的应用程序的后端](http://msdn.microsoft.com/library/dn743807.aspx)的指导主题。 本教程建立在通知中心和在[有关通知集线器入门]教程中创建的项目。

本教程也是[保护推]教程的先决条件。 您已经完成本教程中的步骤之后，就可以继续[保护推]教程，说明如何修改安全地发送推式通知本教程中的代码。


##先决条件

在开始本教程之前，您必须已经完成这些移动服务指南︰

+ [开始使用通知集线器]<br/>创建通知中心和保留应用程序名称和注册在本教程中接收通知。 本教程假设您已经完成了这些步骤。 如果不是，请按照[入门通知集线器 （Windows 应用商店）](notification-hubs-windows-store-dotnet-get-started.md);具体而言，部分[注册为 Windows 应用商店应用程序](notification-hubs-windows-store-dotnet-get-started.md#register-your-app-for-the-windows-store)和[配置通知中心](notification-hubs-windows-store-dotnet-get-started.md#configure-your-notification-hub)。 特别是，请确保已经在门户中，通知中心的**配置**选项卡中输入了**包的 SID** ，而且**客户端密钥**值。 [通知中心配置](notification-hubs-windows-store-dotnet-get-started.md#configure-your-notification-hub)一节中描述此配置过程。 这是一个重要的步骤︰ 如果在门户网站上的凭据不匹配所指定应用程序名称的选择，推式通知将不会成功。




> [AZURE.NOTE] 如果使用的作为后端服务的移动服务，请参阅本教程的[移动服务版本](../mobile-services-javascript-backend-windows-store-dotnet-push-notifications-app-users.md)。




[AZURE.INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## 更新客户端项目的代码

在本节中，您将更新[通知集线器入门]教程完成项目中的代码。 已应与存储相关联并通知集线器配置。 在本节中，您将添加代码以调用新的 WebAPI 后端并将其用于注册和发送通知。

1. 在 Visual Studio，打开[通知集线器入门]教程创建的解决方案。

2. 在解决方案资源管理器中右键单击**(Windows 8.1)**项目，然后单击**管理 NuGet 程序包**。

3. 在左侧，单击**联机**。

4. 在**搜索**框中，键入**Http 客户端**。

5. 在结果列表中，单击**Microsoft HTTP 客户端库**，，然后单击**安装**。 完成安装。

6. 在 NuGet**搜索**框中，键入**Json.net**。 安装**Json.NET**软件包，并关闭 NuGet 程序包管理器窗口。

7. 重复上述步骤来安装 Windows Phone 项目**JSON.NET** NuGet 程序包**(Windows Phone 8.1)**项目。


8. 在解决方案资源管理器中，在**(Windows 8.1)**项目中，双击**MainPage.xaml**在 Visual Studio 编辑器中打开它。

9. 在**MainPage.xaml**的 XML 代码，替换`<Grid>`部分中的使用下列代码。 此代码将添加用户将使用进行身份验证的用户名和密码文本框。 它还添加了文本框的邮件通知，并应在收到通知的用户名标记︰

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>

            <TextBlock Grid.Row="0" Text="Notify Users" HorizontalAlignment="Center" FontSize="48"/>

            <StackPanel Grid.Row="1" VerticalAlignment="Center">
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                    </Grid.ColumnDefinitions>
                    <TextBlock Grid.Row="0" Grid.ColumnSpan="3" Text="Username" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="UsernameTextBox" Grid.Row="1" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
                    <TextBlock Grid.Row="2" Grid.ColumnSpan="3" Text="Password" FontSize="24" Margin="20,0,20,0" />
                    <PasswordBox Name="PasswordTextBox" Grid.Row="3" Grid.ColumnSpan="3" Margin="20,0,20,0"/>

                    <Button Grid.Row="4" Grid.ColumnSpan="3" HorizontalAlignment="Center" VerticalAlignment="Center"
                                Content="1. Login and register" Click="LoginAndRegisterClick" Margin="0,0,0,20"/>

                    <ToggleButton Name="toggleWNS" Grid.Row="5" Grid.Column="0" HorizontalAlignment="Right" Content="WNS" IsChecked="True" />
                    <ToggleButton Name="toggleGCM" Grid.Row="5" Grid.Column="1" HorizontalAlignment="Center" Content="GCM" />
                    <ToggleButton Name="toggleAPNS" Grid.Row="5" Grid.Column="2" HorizontalAlignment="Left" Content="APNS" />

                    <TextBlock Grid.Row="6" Grid.ColumnSpan="3" Text="Username Tag To Send To" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="ToUserTagTextBox" Grid.Row="7" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <TextBlock Grid.Row="8" Grid.ColumnSpan="3" Text="Enter Notification Message" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="NotificationMessageTextBox" Grid.Row="9" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <Button Grid.Row="10" Grid.ColumnSpan="3" HorizontalAlignment="Center" Content="2. Send push" Click="PushClick" />
                </Grid>
            </StackPanel>
        </Grid>


10. 在解决方案资源管理器中，在**(Windows Phone 8.1)**项目中，打开**MainPage.xaml** ，然后替换 Windows Phone 8.1`<Grid>`与上面的相同代码一节。 接口应类似于如下所示的是什么。

    ![][13]

11. 在解决方案资源管理器中打开**MainPage.xaml.cs**文件**(Windows 8.1)**和**(Windows Phone 8.1)**项目。 添加以下`using`语句，在这两个文件的顶部︰

        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Windows.Networking.PushNotifications;
        using Windows.UI.Popups;
        using System.Threading.Tasks;

12. 在**MainPage.xaml.cs** **(Windows 8.1)**和**(Windows Phone 8.1)**项目，添加以下成员`MainPage`类。 一定要替换`<Enter Your Backend Endpoint>`与以前获得实际的后端端点。 例如， `http://mybackend.azurewebsites.net`。

        private static string BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";



13. 将下面的代码添加到主页中**MainPage.xaml.cs** **(Windows 8.1)**和**(Windows Phone 8.1)**项目的类。

    `PushClick`方法是**发送推**按钮单击处理程序。 它调用后端触发到具有匹配的用户名标记的所有设备的通知`to_tag`参数。 作为 JSON 请求正文内容发送通知消息。

    `LoginAndRegisterClick`方法是单击处理程序**登录和注册**按钮。 它存储基本身份验证令牌在本地存储 （请注意，这表示身份验证方案使用任何标记），然后使用`RegisterClient`注册使用后端的通知。


        private async void PushClick(object sender, RoutedEventArgs e)
        {
            if (toggleWNS.IsChecked.Value)
            {
                await sendPush("wns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleGCM.IsChecked.Value)
            {
                await sendPush("gcm", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleAPNS.IsChecked.Value)
            {
                await sendPush("apns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);

            }
        }

        private async Task sendPush(string pns, string userTag, string message)
        {
            var POST_URL = BACKEND_ENDPOINT + "/api/notifications?pns=" +
                pns + "&to_tag=" + userTag;

            using (var httpClient = new HttpClient())
            {
                var settings = ApplicationData.Current.LocalSettings.Values;
                httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                try
                {
                    await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                        System.Text.Encoding.UTF8, "application/json"));
                }
                catch (Exception ex)
                {
                    MessageDialog alert = new MessageDialog(ex.Message, "Failed to send " + pns + " message");
                    alert.ShowAsync();
                }
            }
        }

        private async void LoginAndRegisterClick(object sender, RoutedEventArgs e)
        {
            SetAuthenticationTokenInLocalStorage();

            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            // The "username:<user name>" tag gets automatically added by the message handler in the backend.
            // The tag passed here can be whatever other tags you may want to use.
            try
            {
                await new RegisterClient(BACKEND_ENDPOINT).RegisterAsync(channel.Uri, new string[] { "myTag" });

                var dialog = new MessageDialog("Registered as: " + UsernameTextBox.Text);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
                SendPushButton.IsEnabled = true;
            }
            catch (Exception ex)
            {
                MessageDialog alert = new MessageDialog(ex.Message, "Failed to register with RegisterClient");
                alert.ShowAsync();
            }
        }

        private void SetAuthenticationTokenInLocalStorage()
        {
            string username = UsernameTextBox.Text;
            string password = PasswordTextBox.Password;

            var token = Convert.ToBase64String(System.Text.Encoding.UTF8.GetBytes(username + ":" + password));
            ApplicationData.Current.LocalSettings.Values["AuthenticationToken"] = token;
        }



14. 在解决方案资源管理器中**共享**项目下打开**App.xaml.cs**文件。 找到对的调用`InitNotificationsAsync()`在`OnLaunched()`事件处理程序。 注释掉或删除调用`InitNotificationsAsync()`。 在前面添加按钮处理程序将初始化通知登记。


        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
            //InitNotificationsAsync();


15. 在解决方案资源管理器中，用鼠标右键单击该**共享**项目，然后单击**添加**，然后单击**类**。 名类**RegisterClient.cs**，然后单击**确定**以生成类。

    此类将与应用程序的后端，以推式通知注册所需的其余部分调用自动换行。 它也是本地存储创建通知详见[从您的应用程序的后端的注册](http://msdn.microsoft.com/library/dn743807.aspx)中心的*registrationIds* 。 注意，它使用存储在本地存储区中，当您单击授权标记**登录和注册**按钮。


16. 添加以下`using`RegisterClient.cs 文件顶部的语句︰

        using Windows.Storage;
        using System.Net;
        using System.Net.Http;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using System.Threading.Tasks;
        using System.Linq;

17. 添加下面的代码在`RegisterClient`类定义。

        private string POST_URL;

        private class DeviceRegistration
        {
            public string Platform { get; set; }
            public string Handle { get; set; }
            public string[] Tags { get; set; }
        }

        public RegisterClient(string backendEndpoint)
        {
            PostURL = backendEndpoint + "/api/register";
        }

        public async Task RegisterAsync(string handle, IEnumerable<string> tags)
        {
            var regId = await RetrieveRegistrationIdOrRequestNewOneAsync();

            var deviceRegistration = new DeviceRegistration
            {
                Platform = "wns",
                Handle = handle,
                Tags = tags.ToArray<string>()
            };

            var statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);

            if (statusCode == HttpStatusCode.Gone)
            {
                // regId is expired, deleting from local storage & recreating
                var settings = ApplicationData.Current.LocalSettings.Values;
                settings.Remove("__NHRegistrationId");
                regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
                statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
            }

            if (statusCode != HttpStatusCode.Accepted)
            {
                // log or throw
            }
        }

        private async Task<HttpStatusCode> UpdateRegistrationAsync(string regId, DeviceRegistration deviceRegistration)
        {
            using (var httpClient = new HttpClient())
            {
                var settings = ApplicationData.Current.LocalSettings.Values;
                httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string) settings["AuthenticationToken"]);

                var putUri = POST_URL + "/" + regId;

                string json = JsonConvert.SerializeObject(deviceRegistration);
                                var response = await httpClient.PutAsync(putUri, new StringContent(json, Encoding.UTF8, "application/json"));
                return response.StatusCode;
            }
        }

        private async Task<string> RetrieveRegistrationIdOrRequestNewOneAsync()
        {
            var settings = ApplicationData.Current.LocalSettings.Values;
            if (!settings.ContainsKey("__NHRegistrationId"))
            {
                using (var httpClient = new HttpClient())
                {
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                    var response = await httpClient.PostAsync(POST_URL, new StringContent(""));
                    if (response.IsSuccessStatusCode)
                    {
                        string regId = await response.Content.ReadAsStringAsync();
                        regId = regId.Substring(1, regId.Length - 2);
                        settings.Add("__NHRegistrationId", regId);
                    }
                    else
                    {
                        throw new Exception();
                    }
                }
            }
            return (string)settings["__NHRegistrationId"];

        }

18. 保存所有更改。


## 测试应用程序

1. 启动应用程序在 Windows 8.1 和 Windows Phone 8.1。 对于 Windows Phone 8.1 可以使用实际的设备仿真程序中运行的实例。

2. 在 Windows 8.1 实例的应用程序中，输入**用户名**和**密码**，如下面的屏幕中所示。 它应不同的用户名称和密码在 Windows Phone 输入。


3. 单击**登录和注册**和验证对话框显示您已登录。 这也将使**发送推**按钮。

    ![][14]

4. 在 Windows Phone 8.1 实例的**用户名**和**密码**字段中输入用户名称的字符串，然后单击**登录和注册**。
5. 然后在**收件人的用户名标记**字段中，输入在 Windows 8.1 上注册的用户名称。 输入一条通知消息并单击**发送推送**。

    ![][16]

6. 只有匹配的用户名标记中已注册的设备接收通知消息。

    ![][15]

## 下一步行动

* 如果您想要段由有兴趣的集团用户，请参阅[使用通知集线器发送的新闻]。
* 若要了解有关如何使用通知集线器的更多信息，请参见[通知集线器指南]。



[9]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push9.png
[10]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push10.png
[11]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push11.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-ui.png
[14]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-windows-instance-username.png
[15]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-notification-received.png
[16]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-send-message.png



<!-- URLs. -->
[开始使用通知集线器]: notification-hubs-windows-store-dotnet-get-started.md
[安全的推]: notification-hubs-aspnet-backend-windows-dotnet-secure-push.md
[使用通知集线器发送的新闻]: notification-hubs-windows-store-dotnet-send-breaking-news.md
[通知集线器指南]: http://msdn.microsoft.com/library/jj927170.aspx
