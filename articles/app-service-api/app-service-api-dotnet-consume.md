---
ms.openlocfilehash: f67a06a45ac855f6b182264bb7184e18b3fad2a0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 API 的应用程序在 Azure 应用程序服务从.NET 客户端" 
    description="了解如何使用从.NET 客户端使用的应用程序服务 SDK API 的应用程序。" 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/30/2015" 
    ms.author="tdykstra"/>

# 使用 API 的应用程序在 Azure 应用程序服务从.NET 客户端 

## 概述

本教程展示如何使用应用程序服务 SDK 编写代码来调用[API 的应用程序](app-service-api-apps-why-best-platform.md)已配置为**公共 （匿名）**或**公共 （身份验证）**访问级别。 文中包含以下示例方案︰

- 从一个控制台应用程序中调用**（匿名） 公共**API 的应用程序
- 从 Windows 桌面应用程序**（身份验证） 的公共**API 的应用程序 

其中每个教程部分无关--您可以按照第二种方案的说明而不完成的第一个步骤。

有关如何调用 API 的**内部**应用程序的信息，请参阅[从.NET 客户端的内部 API 应用程序使用](app-service-api-dotnet-consume-internal.md)。

## 先决条件

本教程假定您熟悉如何创建项目并向其中添加代码，在 Visual Studio 中，以及如何[管理 API 在 Azure 预览门户的应用程序](app-service-api-apps-manage-in-portal.md)。

在这篇文章中的项目和代码示例基于 API 的应用程序项目中创建、 部署和保护在这些文章中︰

- [创建 API 的应用程序](app-service-dotnet-create-api-app.md)
- [部署应用程序 API](app-service-dotnet-deploy-api-app.md)
- [保护 API 的应用程序](../app-service-api-dotnet-add-authentication.md)

[AZURE.INCLUDE [install-sdk-2013-only](../../includes/install-sdk-2013-only.md)]

本教程需要 2.6 或更高版本的 Azure sdk 版本的.NET。

## 未经身份验证的调用从一个控制台应用程序

这一节中创建一个控制台应用程序项目，并将代码添加到该调用 API 应用程序不要求身份验证。 

### 设置 API 的应用程序并创建项目

1. 如果尚未执行此操作，请按照[API 的应用程序部署](app-service-dotnet-deploy-api-app.md)到 Azure 订购 API 的应用程序部署的 ContactsList 示例项目。

    该教程将指导您设置的访问权限级别在 Visual Studio 中的发布对话框为**可用于任何人**，这是**公共 （匿名）**相同的门户。 但是，如果您还没有[保护 API 应用程序](../app-service-dotnet-add-authentication.md)教程之后，访问级别已设置为**公共 （身份验证）**，并在这种情况下，您需要按照下面的步骤中的说明对其进行更改。

2. 在[Azure 预览门户](https://portal.azure.com/)，在**API 应用程序**刀片式服务器 API 应用程序调用时，您要转到**设置 > 应用程序设置**，并将**访问级别**设置为**公共 （匿名）**。

    ![](./media/app-service-api-dotnet-consume/setpublicanon.png)
 
2. 在 Visual Studio 中，将创建一个控制台应用程序项目。
 
### <a id="addclient"></a>添加应用程序服务 SDK 生成客户端代码

[AZURE.INCLUDE [app-service-api-dotnet-add-generated-client](../../includes/app-service-api-dotnet-add-generated-client.md)]

### 添加代码以调用 API 的应用程序

若要调用 API 的应用程序，您所要做是创建一个客户端对象，并调用方法，如本示例所示︰

        var client = new ContactList();
        var contacts = client.Contacts.Get();

1. 打开*Program.cs*，并添加下面的代码在`Main`方法。

        var client = new ContactsList();
        
        // Send GET request.
        var contacts = client.Contacts.Get();
        foreach (var c in contacts)
        {
            Console.WriteLine("{0}: {1} {2}",
                c.Id, c.Name, c.EmailAddress);
        }
        
        // Send POST request.
        client.Contacts.Post(new Models.Contact
        {
            EmailAddress = "lkahn@contoso.com",
            Name = "Loretta Kahn",
            Id = 4
        });
        Console.WriteLine("Finished");
        Console.ReadLine();

3. 按 CTRL + F5 以运行应用程序。

    ![生成已完成](./media/app-service-api-dotnet-consume/consoleappoutput.png)

## 从 Windows 桌面应用程序身份验证的调用

这一节中创建的 Windows 桌面应用程序项目，并将代码添加到该调用 API 应用程序要求身份验证。 

### 设置 API 的应用程序并创建项目

1. 按照[保护 API 应用程序](../app-service-dotnet-add-authentication.md)教程以设置 API 的应用程序**（身份验证） 的公共**访问级别。

1. 在 Visual Studio 中，将创建 Windows 窗体的桌面项目。

2. 在窗体设计器中，添加下列控件︰

    * 按钮控件
    * 文本框控件
    * Web 浏览器控件

3. 设置为多行的文本框控件。

    窗体应类似于下面的示例。

    ![](./media/app-service-api-dotnet-consume/form.png)

### 添加应用程序服务 SDK 生成客户端代码

3. 在**解决方案资源管理器**中右击项目 （而不是解决方案），并选择**添加 > Azure API 应用程序客户端**。 

3. 在**添加 Azure API 应用程序客户端**对话框中，单击**下载从 Azure API 应用程序**。 

5. 从下拉列表中，选择您想要调用时，该 API 应用程序，然后单击**确定**。 

### 添加代码以调用 API 的应用程序

5. 在 Azure 预览门户中，复制 API 应用程序网关的 URL。  下一步中，您将使用此值。

    ![](./media/app-service-api-dotnet-consume/gatewayurl.png)

4. *Form1.cs*源代码中添加以下代码之前`Form1()`GATEWAY_URL 为使用您在上一步中复制的值替换值的构造函数。  请确保包含尾部反斜杠 （/）。 

        private const string GATEWAY_URL = "https://resourcegroupnameb4f3d966dfa43b6607f30.azurewebsites.net/";
        private const string URL_TOKEN = "#token=";

4. 在窗体设计器中，双击该按钮以添加一个 click 处理程序中，然后处理程序方法中添加代码，例如转到该网关的登录 URL:

        webBrowser1.Navigate(string.Format(@"{0}login/[authprovider]", GATEWAY_URL));

    例如在网关配置的标识服务提供商、"aad"、"使用 twitter"、"google"、"microsoftaccount"或"facebook"的代码替换"[authprovider]"。 例如︰

        webBrowser1.Navigate(string.Format(@"{0}login/aad", GATEWAY_URL));

7. 添加`DocumentCompleted`事件处理程序的 web 浏览器控件，并将以下代码添加到处理程序方法。

        if (e.Url.AbsoluteUri.IndexOf(URL_TOKEN) > -1)
        {
            var encodedJson = e.Url.AbsoluteUri.Substring(e.Url.AbsoluteUri.IndexOf(URL_TOKEN) + URL_TOKEN.Length);
            var decodedJson = Uri.UnescapeDataString(encodedJson);
            var result = JsonConvert.DeserializeObject<dynamic>(decodedJson);
            string userId = result.user.userId;
            string userToken = result.authenticationToken;
        
            var appServiceClient = new AppServiceClient(GATEWAY_URL);
            appServiceClient.SetCurrentUser(userId, userToken);
        
            var contactsListClient = appServiceClient.CreateContactsList();
            var contacts = contactsListClient.Contacts.Get();
            foreach (Contact contact in contacts)
            {
                textBox1.Text += contact.Name + " " + contact.EmailAddress + System.Environment.NewLine;
            }
            //appServiceClient.Logout();
            //webBrowser1.Navigate(string.Format(@"{0}login/aad", GW_URL));
        }

    您已经添加的代码运行在使用 web 浏览器控件中的用户登录后。 成功登录后，响应 URL 包含用户 ID 和密码。 该代码从 URL 中提取这些值，它们提供给应用程序服务的客户端对象，然后使用该对象创建一个 API 的应用程序的客户端对象。 您可以通过调用此 API 的应用程序客户端对象上的方法调用 API。

8. 按 CTRL + F5 以运行应用程序。

9. 单击按钮，并浏览器控件将显示一个登录页面，输入被授权调用 API 的应用程序的用户的凭据。

    Azure 验证，并在应用程序调用 API 的应用程序，并显示响应。  

    ![](./media/app-service-api-dotnet-consume/formaftercall.png)

### <a id="client-flow"></a>服务器与客户端流的流

示例应用程序阐释了[服务器流量](../app-service/app-service-authentication-overview.md#server-flow)，这意味着该网关获取标识提供程序的访问令牌。 对于[客户端流](../app-service/app-service-authentication-overview.md#client-flow)，在其中您的客户端应用程序直接从标识提供程序获取访问令牌并将其发送到网关，则调用`LoginAsync`而不是`SetCurrentUser`。 

下面的代码示例假定您有身份标识提供程序的访问令牌中一个名为的字符串变量`providerAccessToken`和标识提供程序中的指示器 （"aad"、"microsoftaccount"、"google"、"使用 twitter"或"facebook"） 一个名为的字符串变量`idProvider`:

        var appServiceClient = new AppServiceClient(GATEWAY_URL);
        var providerAccessTokenJSON = new JObject();
        providerAccessTokenJSON["access_token"] = providerAccessToken;
        var appServiceUser = await appServiceClient.LoginAsync(idProvider, providerAccessTokenJSON);

        var contactsListClient = appServiceClient.CreateContactsList();
        var contacts = contactsListClient.Contacts.Get();
        foreach (Contact contact in contacts)
        {
            textBox1.Text += contact.Name + " " + contact.EmailAddress + System.Environment.NewLine;
        }

## 下一步行动

这篇文章表明如何使用 API 的应用程序从.NET 客户端 API 应用程序设置为**公共 （身份验证）**和**公众 （匿名）**的访问级别。 

有关其他示例从.NET 客户端调用 API 的应用程序的代码中，下载[Azure 卡](https://github.com/Azure-Samples/API-Apps-DotNet-AzureCards-Sample)示例应用程序。

有关如何使用身份验证 API 应用程序中的信息，请参阅[API 的应用程序和移动应用程序在 Azure 应用程序服务的身份验证](../app-service/app-service-authentication-overview.md)。
 

测试
