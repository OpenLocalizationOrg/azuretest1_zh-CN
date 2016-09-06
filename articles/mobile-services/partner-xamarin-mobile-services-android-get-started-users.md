---
ms.openlocfilehash: 2990e6693f1ff47e4cc768d48e739fb0f02effe7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用身份验证 (Xamarin.Android) 的移动服务"
    description="了解如何为 Xamarin.Android Azure 移动服务应用程序中使用身份验证。"
    services="mobile-services"
    documentationCenter="xamarin"
    manager="dwrede"
    authors="lindydonna"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/18/2015" 
    ms.author="donnam"/>

# 添加到您的移动服务应用程序的身份验证

[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

<p>本主题演示如何在 Azure 移动服务从您的 Xamarin.Android 应用程序的用户进行身份验证。 在本教程中，您添加到快速入门项目使用标识提供程序支持移动服务的身份验证。 后被成功通过身份验证和授权的移动服务，将显示用户 ID 值。</p>

本教程将引导您完成以下基本步骤，以便您的应用程序中的身份验证︰

1. [注册您的应用程序进行身份验证和配置移动服务]
2. [限制为经过身份验证的用户的表权限]
3. [向应用程序添加验证]

本教程基于移动服务快速入门。 第一次还必须完成本教程 [开始使用移动服务]。

学完本教程需要 Xamarin.Android 和 Android SDK 4.2 或更高版本。

##<a name="register"></a>注册您的应用程序进行身份验证和配置移动服务

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

##<a name="permissions"></a>限制对已验证身份的用户的权限


[AZURE.INCLUDE [mobile-services-restrict-permissions-javascript-backend](../../includes/mobile-services-restrict-permissions-javascript-backend.md)]


3. 日蚀式，在打开项目时您完成本教程创建 [开始使用移动服务]。

4. 从**运行**菜单，然后单击**运行**以启动该应用程序;验证应用程序启动后引发未处理的异常状态代码 401 （未经授权）。

     这是因为该应用程序试图访问移动服务作为未经身份验证的用户，但_TodoItem_表现在要求进行身份验证。

接下来，您将更新应用程序验证用户身份之前从移动服务中请求的资源。

##<a name="add-authentication"></a>向应用程序添加验证

1. **ToDoActivity**类中添加以下属性︰

        private MobileServiceUser user;

2. 将下面的方法添加到**ToDoActivity**类中︰

        private async Task Authenticate()
        {
            try
            {
                user = await client.LoginAsync(this, MobileServiceAuthenticationProvider.MicrosoftAccount);
                CreateAndShowDialog(string.Format("you are now logged in - {0}", user.UserId), "Logged in!");
            }
            catch (Exception ex)
            {
                CreateAndShowDialog(ex, "Authentication failed");
            }
        }

    这将创建一个新的方法来处理身份验证过程。 通过使用 Microsoft 帐户登录，用户进行身份验证。 将显示一个对话框，其中显示已经过身份验证的用户的 ID。 您不能继续没有积极的身份验证。

    > [AZURE.NOTE] 如果您正在使用非 Microsoft 身份标识提供程序，更改传递给**login**方法上面到以下任一值︰ _Facebook_、 _Google_、_使用 Twitter_，或_WindowsAzureActiveDirectory_。

3. 在**OnCreate**方法中，实例化的代码之后添加以下代码行`MobileServiceClient`对象。

        await Authenticate();

    此调用启动身份验证过程，并等待它以异步方式。

4. 移动后的剩余代码`await Authenticate();`在**OnCreate**方法为新的**CreateTable**方法中，其中如下所示︰

        private async Task CreateTable()
        {

            await InitLocalStoreAsync();

            // Get the Mobile Service Table instance to use
            toDoTable = client.GetTable<ToDoItem>();

            textNewToDo = FindViewById<EditText>(Resource.Id.textNewToDo);

            // Create an adapter to bind the items with the view
            adapter = new ToDoItemAdapter(this, Resource.Layout.Row_List_To_Do);
            var listViewTodo = FindViewById<ListView>(Resource.Id.listViewToDo);
            listViewTodo.Adapter = adapter;

            // Load the items from the Mobile Service
            await RefreshItemsFromTableAsync();
        }

5. 然后在步骤 2 中添加**身份验证**调用后**OnCreate**中调用新的**CreateTable**方法︰

        await CreateTable();


6. 从**运行**菜单，然后单击**运行**以启动该应用程序并使用您选定的标识提供商登录。

    成功登录后，应用程序应该运行错误，并应能为移动服务的查询，并对数据进行更新。

## 获取已完成的示例
下载[已完成的示例项目]。 请务必 Azure 设置更新的**applicationURL**和**applicationKey**变量。

## <a name="next-steps"></a>下一步行动

在下一步的教程中，[有脚本的授权用户]，将采用基于身份验证的用户的移动服务提供的用户 ID 值，并使用它来筛选移动服务返回的数据。

<!-- Anchors. -->
[注册您的应用程序进行身份验证和配置移动服务]: #register
[限制为经过身份验证的用户的表权限]: #permissions
[向应用程序添加验证]: #add-authentication
[下一步行动]:#next-steps

<!-- Images. -->
[4]: ./media/partner-xamarin-mobile-services-android-get-started-users/mobile-services-selection.png
[5]: ./media/partner-xamarin-mobile-services-android-get-started-users/mobile-service-uri.png

[13]: ./media/partner-xamarin-mobile-services-android-get-started-users/mobile-identity-tab.png
[14]: ./media/partner-xamarin-mobile-services-android-get-started-users/mobile-portal-data-tables.png
[15]: ./media/partner-xamarin-mobile-services-android-get-started-users/mobile-portal-change-table-perms.png

<!-- URLs. -->
[授权用户使用的脚本]: mobile-services-javascript-backend-service-side-authorization.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[已完成的示例项目]: http://go.microsoft.com/fwlink/p/?LinkId=331328
