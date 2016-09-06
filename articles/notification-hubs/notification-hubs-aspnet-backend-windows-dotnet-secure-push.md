---
ms.openlocfilehash: 3427d353d1bdedbd116e8ef78fc14824dfcf4478
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="安全的 azure 通知集线器推"
    description="了解如何在 Azure 中发送安全推式通知。 用 C# 使用.NET API 编写的代码样本。"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs" 
    ms.workload="mobile"
    ms.tgt_pltfrm="windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="wesmc"/>

#安全的 azure 通知集线器推

> [AZURE.SELECTOR]
- [Windows 世界](notification-hubs-windows-dotnet-secure-push.md)
- [iOS](notification-hubs-aspnet-backend-ios-secure-push.md)
- [Android](notification-hubs-aspnet-backend-android-secure-push.md)


##概述

在 Microsoft Azure 支持推送通知使您可以访问一个易于使用、 多平台和向外扩展的推的基础结构，极大地简化了使用者和企业应用程序的移动平台的推式通知的实现。

法规或安全约束，有时应用程序可能需要在无法通过标准推式通知基础架构传输通知中包括的内容。 本教程介绍了如何通过发送敏感信息通过安全、 经身份验证的客户端设备和应用程序的后端之间连接来获得相同的体验。

在较高级别流程如下所示︰

1. 应用程序后端︰
    - 在后端数据库中的存储安全有效负载。
    - 此通知的 ID 将发送到设备 （安全信息不会发送）。
2. 该应用程序在设备上，在接收到此通知时︰
    - 设备联系人后端请求安全有效负载。
    - 应用程序可以显示为通知设备上的负载。

值得注意的是，上述流中 （并在本教程中），我们假定用户登录后，设备将在本地存储中中, 存储身份验证令牌。 这可以保证完全无缝的体验，因为该设备可以检索通知使用此令牌的安全有效负载。 如果您的应用程序不会存储在设备上，身份验证令牌，或这些标记可以到期，设备应用程序中的，在接收到此通知时应显示一般通知提示用户启动该应用程序。 应用程序对用户进行身份验证，然后显示通知负载。

本安全推教程演示如何安全地发送推式通知。 本教程基于**通知用户**教程，所以应完成的步骤，该教程中第一次。

> [AZURE.NOTE] 本教程假设您已经创建并配置通知中心[入门通知集线器 （Windows 应用商店）](notification-hubs-windows-store-dotnet-get-started.md)中所述。
另外，请注意 Windows Phone 8.1 要求 (而不是 Windows Phone) Windows 凭据和后台任务在 Windows Phone 8.0 或 Silverlight 8.1 上不起作用。 对于 Windows 应用商店应用程序，您可以收到通知通过后台任务，只有当应用程序启用锁定屏幕 （单击 Appmanifest 中的复选框）。

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## 修改 Windows Phone 项目

1. 在**NotifyUserWindowsPhone**项目中，app.xaml.cs 注册推后台任务中添加以下代码。 在末尾添加下面这行代码`OnLaunched()`方法︰

        RegisterBackgroundTask();

2. 在 App.xaml.cs 中，添加以下代码后立即`OnLaunched()`方法︰

        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();

                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }

3. 添加以下`using`App.xaml.cs 文件顶部的语句︰

        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;

4. 从 Visual Studio 中的**文件**菜单中，单击**全部保存**。

## 创建推式背景组件

下一步是创建推式背景组件。

1. 在解决方案资源管理器，右键单击解决方案 (在此情况下**解决方案 SecurePush** ) 的顶级节点，单击**添加**，然后单击**新建项目**。

2. 展开**存储应用程序**，单击**Windows Phone 应用程序**，然后单击**所需的 Windows 运行时组件 (Windows Phone)**。 名称**PushBackgroundComponent**，该项目，然后单击**确定**以创建项目。

    ![][12]

3. 在解决方案资源管理器，右键单击**(Windows Phone 8.1) PushBackgroundComponent**项目，再单击**添加**然后单击**类**。 **PushBackgroundTask.cs**的新类的名称。 单击**添加**以生成类。

4. **PushBackgroundComponent**命名空间定义的所有内容都替换为下面的代码替换该占位符`{back-end endpoint}`与后端端点部署后端时获取︰

        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }

            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";

                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store the content received from the notification so it can be retrieved from the UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;

                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);

                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);

                    ShowToast(notification);

                    deferral.Complete();
                }

                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }

5. 在解决方案资源管理器中右键单击**(Windows Phone 8.1) PushBackgroundComponent**项目，然后单击**管理 NuGet 程序包**。

6. 在左侧，单击**联机**。

7. 在**搜索**框中，键入**Http 客户端**。

8. 在结果列表中，单击**Microsoft HTTP 客户端库**，，然后单击**安装**。 完成安装。

9. 在 NuGet**搜索**框中，键入**Json.net**。 安装**Json.NET**软件包，然后关闭 NuGet 程序包管理器窗口。

10. 添加以下`using` **PushBackgroundTask.cs**文件顶部的语句︰

        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;

11. 在解决方案资源管理器中，在**NotifyUserWindowsPhone (Windows Phone 8.1)**项目中，右键单击**引用**，然后单击**添加引用...**。 在引用管理器对话框中， **PushBackgroundComponent**，旁边的复选框，然后单击**确定**。

12. 在解决方案资源管理器中双击**NotifyUserWindowsPhone (Windows Phone 8.1)**项目中的**Package.appxmanifest** 。 在**通知**，设置**祝酒能够**为**是**。

    ![][3]

13. 仍在**Package.appxmanifest**中，请单击顶部附近的**声明**菜单。 在**可用声明**下拉列表中，单击**后台任务**，然后单击**添加**。

14. 在**Package.appxmanifest**，在**属性**检查**推式通知**。

15. 在**Package.appxmanifest**，在**应用程序设置**中，键入**PushBackgroundComponent.PushBackgroundTask** **入口点**字段中。

    ![][13]

16. 从**文件**菜单上，单击**全部保存**。

## 运行应用程序

要运行该应用程序，请执行以下操作︰

1. 在 Visual Studio 中，运行**AppBackend** Web API 应用程序。 将显示 ASP.NET web 页。

2. 在 Visual Studio 中， **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone 应用程序。 Windows Phone 仿真程序运行并自动加载该应用程序。

3. **NotifyUserWindowsPhone**应用程序用户界面中输入用户名和密码。 这些可以是任何字符串，但它们必须是相同的值。

4. **NotifyUserWindowsPhone**应用程序用户界面中单击**登录和注册**。 然后单击**发送推**。

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
