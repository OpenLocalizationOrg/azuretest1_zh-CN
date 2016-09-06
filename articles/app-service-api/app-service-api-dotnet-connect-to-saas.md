---
ms.openlocfilehash: 245990805fbf073627f9947297bdbc9eb436f60f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
    pageTitle="自定义 SaaS 连接器 ASP.NET API 的应用程序在 Azure 应用程序服务" 
    description="了解如何编写连接到 SaaS 平台 API 应用程序中的代码，以及如何从.NET 客户端调用 API 的应用程序。" 
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
    ms.topic="hero-topics" 
    ms.date="07/01/2015" 
    ms.author="tdykstra"/>

# 从 Azure 应用程序服务的 ASP.NET API 应用程序连接到一个 SaaS 平台

## 概述

本教程展示如何代码和配置连接到一个[软件作为-服务 (SaaS) 平台](../app-service/app-service-authentication-overview.md#obotosaas) [API 应用程序](app-service-api-apps-why-best-platform.md)使用[应用程序服务 API 应用程序的.NET 的 SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/)。 教程还展示如何使用[用于.NET 的应用程序服务 SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService)调用从.NET 客户端 API 应用程序。 在本教程末尾您将有.NET 控制台应用程序客户端调用.NET API 的应用程序运行在 Azure 应用程序服务。 API 的应用程序调用收存箱 API 并返回用户的收存箱帐户中文件和文件夹的列表。

为编写自定义的 API 应用程序中直接调用一个 SaaS 的 API 的替代方法，您可以调用预打包的[连接器 API 的应用程序](../app-service-logic/app-service-logic-what-are-biztalk-api-apps.md)。 有关如何执行此操作的信息，请参阅[部署和配置一个 SaaS 连接器 API 应用程序](app-service-api-connnect-your-app-to-saas-connector.md)。

本教程将引导您完成以下步骤︰


* 在 Visual Studio 中创建 API 的应用程序项目。 
* 将*apiapp.json*文件启用 API 的应用程序连接到收存箱服务配置。
* 添加代码，调用收存箱并返回结果。
* 在 Azure 创建新的 API 应用程序。
* 将项目部署到 API 的应用程序。
* 配置 API 的应用程序。
* 配置网关。
* 创建测试客户端。
* 运行测试客户端。

## 先决条件

本教程作出以下假设︰

* 已完成[创建 API 的应用程序](app-service-dotnet-create-api-app.md)和[部署 API 的应用程序](app-service-dotnet-deploy-api-app.md)的教程。
  
* 呈现在[API 的应用程序和移动应用程序的身份验证](app-service-authentication-overview.md)，您必须进行身份验证，Azure 应用程序的服务网关体系结构的一个基本的了解。

* 您知道如何使用 API 应用程序在 Azure 预览门户中，[如何定位 API 的应用程序和网关到刀片式服务器](app-service-api-manage-in-portal.md#navigate)中所述。

## 创建 API 的应用程序项目
 
说明指示您输入项目的名称，请输入*SimpleDropbox*。 

[AZURE.INCLUDE [app-service-api-create](../../includes/app-service-api-create.md)]

## 将*apiapp.json*文件配置

API 使应用程序拨出电话到 SaaS 平台，SaaS 平台都有指定的*apiapp.json*文件中。 

1. 打开*apiapp.json*文件，添加`authentication`属性，如下所示 （将还必须添加逗号前面的属性之后）︰

        "authentication": [
          {
            "type": "dropbox"
          }
        ]

    完整的 apiapp.json 文件将类似于以下示例︰ 

        {
            "$schema": "http://json-schema.org/schemas/2014-11-01/apiapp.json#",
            "id": "SimpleDropBox",
            "namespace": "microsoft.com",
            "gateway": "2015-01-14",
            "version": "1.0.0",
            "title": "SimpleDropBox",
            "summary": "",
            "author": "",
            "endpoints": {
                "apiDefinition": "/swagger/docs/v1",
                "status": null
            },
            "authentication": [
              {
                "type": "dropbox"
              }
            ]
        }

2. 保存该文件。

设置`authentication`属性有两个效果︰

* 它会使门户网站 API 应用程序刀片式服务器，使您可以输入 SaaS 平台的客户机 ID 和客户端密钥值中显示用户界面。

    ![](./media/app-service-api-dotnet-connect-to-saas/authblade.png)

* 它使 API 的应用程序时调用 SaaS 提供商的 API 使用网关从检索 SaaS 提供商的访问令牌。

`authentication`属性是一个数组，但此预览版本不支持指定多个提供程序。

有关支持的平台的列表，请参阅[获得用户同意访问其他 SaaS 平台](../app-service/app-service-authentication-overview.md#obotosaas)。

您还可以指定范围，如本示例所示︰

        "authentication": [
          {
            "type": "google",
            "scopes": ["https://www.googleapis.com/auth/userinfo.email", "https://www.googleapis.com/auth/userinfo.profile"]
          }
        ]

可用的作用域定义的每个 SaaS 提供商，可以在提供程序的开发人员门户中找到。

## 添加代码，调用收存箱

1. 在 SimpleDropbox 项目中安装[DropboxRestAPI](https://www.nuget.org/packages/DropboxRestAPI) NuGet 程序包。

    * 从**工具**菜单上，单击**NuGet 程序包管理器 > 程序包管理器控制台**。

    * 在**程序包管理器控制台**窗口中，输入以下命令︰
     
            install-package DropboxRestAPI  

1. 打开*Controllers\ValuesController.cs* ，所有文件中的代码替换为以下代码。

        using DropboxRestAPI;
        using Microsoft.Azure.AppService.ApiApps.Service;
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Threading.Tasks;
        using System.Web.Http;
        
        namespace SimpleDropBox2.Controllers
        {
            public class ValuesController : ApiController
            {
                public async Task<IEnumerable<string>> Get()
                {
                    // Retrieve the token from the gateway
                    var runtime = Runtime.FromAppSettings(Request);
                    var dropboxTokenResult = await runtime.CurrentUser.GetRawTokenAsync("dropbox");
        
                    // Create a Dropbox client object that will send the token
                    // with REST API calls to Dropbox.
                    var dropboxClient = new Client(
                        new Options
                        {
                            AccessToken = dropboxTokenResult.Properties["AccessToken"]
                        }
                    );
        
                    // Call the Dropbox API
                    var metadata = await dropboxClient.Core.Metadata.MetadataAsync("/");
        
                    // Return a list of files and folders.
                    return metadata.contents.Select(md => md.path);
                }
            }
        }

    客户端调用此方法之前，用户已经登录到收存箱并授予同意 API 应用程序可访问用户的收存箱帐户。 收存箱确认该同意的情况下通过提供对应用程序的服务网关访问令牌。 此代码检索标记从网关，包括在收存箱 API 调用步骤 6 和 7 下图中的所示。

    ![](./media/app-service-api-dotnet-connect-to-saas/saastoken.png)

2. 生成项目。

## 在 Azure 创建 API 的应用程序

在这一节中使用 Visual Studio 的**Web 发布**向导在 Azure 创建 API 的应用程序。 其中说明指示您输入 API 的应用程序的名称，请输入*SimpleDropbox*。

[AZURE.INCLUDE [app-service-api-pub-web-create](../../includes/app-service-api-pub-web-create.md)]

## 部署您的代码

您可以使用相同的**Web 发布**向导将代码部署到新的 API 应用程序。

[AZURE.INCLUDE [app-service-api-pub-web-deploy](../../includes/app-service-api-pub-web-deploy.md)]

## 配置身份验证的传入呼叫

为了允许已通过身份验证传出的呼叫 API 应用程序从 Azure 应用程序服务，API 的应用程序还必须要求来电来自已通过身份验证的用户。 这不是一般的 OAuth 2.0 要求但是目前实现的应用程序的服务网关体系结构的要求。

本节中的屏幕快照显示 ContactsList API 的应用程序，而是您在本教程中创建的 SimpleDropbox API 应用程序相同的进程。

### 配置 API 应用程序要求具有传入调用进行身份验证

[AZURE.INCLUDE [app-service-api-config-auth](../../includes/app-service-api-config-auth.md)]

### 在网关配置标识提供程序

[AZURE.INCLUDE [app-service-api-gateway-config-auth](../../includes/app-service-api-gateway-config-auth.md)]

## 为打出的电话配置身份验证

若要启用您的 API 应用程序调用收存箱 API，您必须设置 API 应用程序和收存箱开发人员网站创建一个收存箱应用程序之间交换。

### 在 Dropbox.com 网站上创建一个收存箱应用程序

[AZURE.INCLUDE [app-service-api-create-dropbox-app](../../includes/app-service-api-create-dropbox-app.md)]

### 收存箱和 API 应用程序之间交换设置

以下步骤请参阅收存箱连接器 API 的应用程序，但程序和用户界面都为您在本教程中创建的 SimpleDropbox API 应用程序相同。

> **注意︰**如果您看不到域的客户端 ID 和 SimpleDropbox API 的应用程序的**身份验证**刀片式服务器上的客户端密钥的屏幕快照中所示，确保收存箱您重新启动网关后将 API 的应用程序项目部署到 API 的应用程序的指导。 "收存箱"中的值`authentication`的*apiapp.json*文件的先前部署的属性会触发门户以显示这些字段。

[AZURE.INCLUDE [app-service-api-exchange-dropbox-settings](../../includes/app-service-api-exchange-dropbox-settings.md)]

## 创建测试客户端

在本节中，您将创建的控制台应用程序项目中使用 Visual Studio 生成客户端代码来调用 SimpleDropbox API 的应用程序。 控制台应用程序实例化一个 Windows 窗体浏览器控件来处理与网关和收存箱登录 web 页的用户交互。

### 创建项目

1. 在 Visual Studio 中，创建一个控制台应用程序项目并将其命名为*SimpleDropboxTest*。

2. 设置对 System.Windows.Forms 的引用。
 
    * 在**解决方案资源管理器**中右键单击**引用**，然后单击**添加引用**。

    * 选择**System.Windows.Forms**，左侧的复选框，然后单击**确定**。
     
    ![](./media/app-service-api-dotnet-connect-to-saas/setref.png)

    控制台应用程序将使用 Windows 窗体组件时要实例化的浏览器控件使用户能够登录到网关和收存箱所需。

### 添加生成的客户端代码

本节中的屏幕快照显示 ContactsList 项目和 API 的应用程序，但在本教程中选择 SimpleDropboxTest 项目和 SimpleDropbox API 的应用程序。

[AZURE.INCLUDE [app-service-api-dotnet-add-generated-client](../../includes/app-service-api-dotnet-add-generated-client.md)]

### 添加代码以调用 API 的应用程序

3. 打开*Program.cs* ，在其中的代码替换为下面的代码。
        
        using Microsoft.Azure.AppService;
        using Newtonsoft.Json;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Windows.Forms;
        
        namespace SimpleDropboxTest
        {
            enum Step
            {
                GatewayLogin,
                GetSaaSConsentLink,
                GetUserConsent
            }
        
            class Program
            {
                private const string GATEWAY_URL = @"{gateway url}";
                private const string URL_TOKEN = "#token=";
                private const string SAAS_URL = "dropbox.com";
                private static Form frm = new Form();
                private static string responseURL = "";
                private static Step step;
                [STAThread]
                static void Main(string[] args)
                {
                    // Create the web browser control
                    WebBrowser browser = new WebBrowser();
                    browser.Dock = DockStyle.Fill;
                    browser.Navigated += CheckResponseURL;
                    frm.Controls.Add(browser);
                    frm.Width = 640;
                    frm.Height = 480;
        
                    // Create the gateway and API app clients.
                    AppServiceClient appServiceClient = new AppServiceClient(GATEWAY_URL);
                    SimpleDropbox simpleDropboxClient = appServiceClient.CreateSimpleDropbox();
        
                    // Navigate browser to gateway login URL for configured identity provider.
                    // Identity provider for this example is Azure Active Directory.
                    step = Step.GatewayLogin;
                    browser.Navigate(string.Format(@"{0}/login/aad", GATEWAY_URL));
                    frm.ShowDialog();
                    Console.WriteLine("Logged in to gateway, response URL=" + responseURL);
        
                    // Get user ID and Zumo token from return URL, then call 
                    // the gateway URL to log in the gateway client.
                    var encodedJson = responseURL.Substring(responseURL.IndexOf(URL_TOKEN) + URL_TOKEN.Length);
                    var decodedJson = Uri.UnescapeDataString(encodedJson);
                    var result = JsonConvert.DeserializeObject<dynamic>(decodedJson);
                    string userId = result.user.userId;
                    string userToken = result.authenticationToken;
                    appServiceClient.SetCurrentUser(userId, userToken);
        
                    // Call gateway API to get consent link URL for target SaaS platform.
                    // SaaS platform for this example is Dropbox.
                    // See the tutorial for an explanation of
                    // the redirectURL parameter for GetConsentLinkAsync
                    var gatewayConsentLink = appServiceClient.GetConsentLinkAsync("SimpleDropbox", GATEWAY_URL).Result;
                    Console.WriteLine("\nGot gateway consent link, URL=" + gatewayConsentLink);
        
                    // Navigate browser to consent link URL returned from gateway.
                    // Response URL will be the SaaS logon link
                    step = Step.GetSaaSConsentLink;
                    browser.Navigate(gatewayConsentLink);
                    frm.ShowDialog();
                    Console.WriteLine("\nGot SaaS consent link response, URL=" + responseURL);
        
                    // Navigate browser to login/consent link for SaaS platform.
                    step = Step.GetUserConsent;
                    browser.Navigate(responseURL);
                    frm.ShowDialog();
                    Console.WriteLine("\nGot Dropbox login response, URL=" + responseURL);
        
                    Console.WriteLine("\nResponse from SaaS Provider");
                    var response = simpleDropboxClient.Values.Get();
                    foreach (string s in response)
                    {
                        Console.WriteLine(s);
                    }
                    Console.Read();
                }
        
                static void CheckResponseURL(object sender, WebBrowserNavigatedEventArgs e)
                {
                    if ((step == Step.GatewayLogin && e.Url.AbsoluteUri.IndexOf(URL_TOKEN) > -1)
                        || (step == Step.GetSaaSConsentLink && e.Url.AbsoluteUri.IndexOf(SAAS_URL) > -1)
                        || (step == Step.GetUserConsent && e.Url.AbsoluteUri.IndexOf(GATEWAY_URL) > -1))
                    {
                        responseURL = e.Url.AbsoluteUri;
                        frm.Close();
                    }
                }
        
            }
        }

1. {网关 url} 替换为实际的 URL 的网关。
 
    您可以从门户中**网关**刀片式服务器获得网关 URL:

    ![](./media/app-service-api-dotnet-connect-to-saas/gwurl.png)

        private const string GATEWAY_URL = @"https://sd1aeb4ae60b7cb4f3d966dfa43b660.azurewebsites.net";

    > **重要事项**︰ 请确保 URL 开头的网关`https://`，而不`http://`。 **如果从门户复制 http://，必须将其更改为 https://，当粘贴代码。**

### 代码说明

该控制台应用程序旨在使用最少的代码来说明客户端应用程序必须经过的步骤。 生产应用程序中通常不是一个控制台应用程序，并将实现错误处理和日志记录。

下面是该代码正在执行的操作的概述︰

* 已配置的标识提供程序，此案例的 Azure Active Directory 中打开浏览器，到网关的登录 URL。 
     
* 处理预期的响应 URL，用户登录后︰ 提取用户 ID 和 Zumo 标记，将它们提供给应用程序服务的客户端对象。 

* 使用应用程序服务的客户端对象检索将重定向到登录和同意的情况下收存箱链接的网关 URL。 在图中的步骤 1。

* 打开浏览器访问网关同意 URL。 浏览器被重定向到登录收存箱和同意的情况下获取链接。 在图中的步骤 2。 
     
* 用户登录，并在 Dropbox.com 中提供的同意后，请关闭浏览器。 在图中的步骤 3。 
 
* 调用此 API 的应用程序。 在图中的步骤 5。 （Dropbox.com 之间的网关，在幕后发生的第 4 步步骤 6 和 7 是从完成 API 应用程序中，不是客户端。）

![](./media/app-service-api-dotnet-connect-to-saas/saastoken.png)

其他注意事项︰

* `STAThread`特性`Main`方法需要通过 web 浏览器控件并设置或调用 API 的应用程序无关。

* 网关登录 URL 显示以`/aad`Azure 活动目录。

        browser.Navigate(string.Format(@"{0}/login/aad", GATEWAY_URL));

    下面是使用其他提供程序的值︰
    * ""microsoftaccount
    * ""facebook
    * ""twitter
    * ""google
<br/><br/>

* 第二个参数为`GetConsentLinkAsync()`方法是用户登录到收存箱后，许可服务器重定向到的回调 URL 并提供同意访问的用户帐户。 

        var gatewayConsentLink = appServiceClient.GetConsentLinkAsync("SimpleDropbox", GATEWAY_URL).Result;

    此参数通常会指定在客户端应用程序中，用户应转到下一个网页。 这段演示代码是一个控制台应用程序中，由于没有应用程序页，转和代码指定的网关 URL 一样方便的登陆页。 

    它获取指向此 URL 重定向，并且没有任何错误消息，则应验证客户端应用程序。 如果登录/同意过程失败，则重定向 URL 可能包含查询字符串中的错误消息。 有关详细信息，请参阅[疑难解答](#troubleshooting)部分。 

## 测试

1. 运行 SimpleDropboxTest 控制台应用程序。

2. 在第一个登录页面，注册使用 Azure Active Directory 的凭据 （或另一个身份标识提供程序，例如 Google 或 Twitter 如果在网关配置的凭据）。

    ![](./media/app-service-api-dotnet-connect-to-saas/aadlogon.png)

3. 在 Dropbox.com 登录页上，使用您收存箱凭据登录。

    ![](./media/app-service-api-dotnet-connect-to-saas/dblogon.png)

4. 在收存箱同意页中，提供该应用程序有权访问您的数据。

    ![](./media/app-service-api-dotnet-connect-to-saas/dbconsent.png)

    控制台应用程序然后调用 API 的应用程序并收存箱帐户中返回的文件的列表。

    ![](./media/app-service-api-dotnet-connect-to-saas/testclient.png)

## 疑难解答

本部分包含以下主题︰

* [网关登录后的 HTTP 错误 405](#405)
* [而不是收存箱登录页的 HTTP 错误 400](#400)
* [HTTP 错误 403 调用 API 的应用程序时](#403)

### <a id="405"></a> 网关登录后的 HTTP 错误 405

如果该代码将调用 GetConsentLinkAsync 时得到的 HTTP 错误 405，，验证您使用 https://，不关 url 的 http://。

![](./media/app-service-api-dotnet-connect-to-saas/http405.png)

因为客户端会尝试提出一个非 SSL HTTP POST 请求，到*https://*，网关重定向，重定向会导致 GET 请求接收到 405 不允许错误的方法。 检索的同意的情况下链接的 URL 仅接受 POST 请求。

### <a id="400"></a>而不是收存箱登录页的 HTTP 错误 400

请确保您具有正确的**客户机 ID**在 API 的应用程序的**身份验证**刀片式服务器，并确保没有前导或尾随空格。 

### <a id="403"></a> HTTP 错误 403 调用 API 的应用程序时

* 请确保，API 的应用程序的**访问级别**设置为**公共 （身份验证）**，不是**内部**。

* 请确保您具有正确的**客户端密码**API 的应用程序的**身份验证**刀片式服务器，并确保没有前导或尾随空格。

重定向 URL 收存箱登录后的看起来如下例所示︰

    https://sd1aeb4ae60b7cb4f3d966dfa43b6607f30.azurewebsites.net/?error=RmFpbGVkIHRvIGV4Y2hhbmdlIGNvZGUgZm9yIHRva2VuLiBEZXRhaWxzOiB7ImVycm9yX2Rlc2NyaXB0aW9uIjogIkludmFsaWQgY2xpZW50X2lkIG9yIGNsaWVudF9zZWNyZXQiLCAiZXJyb3IiOiAiaW52YWxpZF9jbGllbnQifQ%3d%3d

如果您删除了 %3d %3d 从结尾处`error`查询字符串值，这是一个有效的 base64 编码字符串。 解码得到的错误消息的字符串︰

    Failed to exchange code for token. Details: {"error_description": "Invalid client_id or client_secret", "error": "invalid_client"}

## 下一步行动

您已经看到如何配置连接到 SaaS 平台 API 应用程序和代码。  有关如何处理 API 应用程序中的身份验证的其他教程的链接，请参阅[身份验证 API 的应用程序和移动应用程序的下一步行动](../app-service/app-service-authentication-overview.md#next-steps)。 

[Azure 预览门户]: https://portal.azure.com/
[Azure 门户]: https://manage.windowsazure.com/

测试
