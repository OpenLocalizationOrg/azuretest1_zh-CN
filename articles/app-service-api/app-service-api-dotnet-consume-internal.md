---
ms.openlocfilehash: e0028fdc9ce23735693b85801ac2b608d3f9f370
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用内部 API 的应用程序在 Azure 应用程序服务从.NET 客户端" 
    description="了解如何使用从.NET API 的应用程序在同一个资源组中，使用应用程序服务 SDK API 的应用程序。" 
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
    ms.date="07/31/2015" 
    ms.author="bradyg"/>

# 使用内部 API 的应用程序在 Azure 应用程序服务从.NET 客户端 

## 概述

本教程演示如何编写代码的**内部**访问级别调用另一个 API 应用程序配置 ASP.NET [API 的应用程序](app-service-api-apps-why-best-platform.md)。 这两种 API 的应用程序必须在同一个资源组中。  可以使用相同的代码从[移动应用程序](../app-service-mobile/app-service-mobile-value-prop-preview.md)调用 API 的应用程序内部。

您将构建一个 ContactNames Web API。 Web API 的 Get 方法将调用 ContactsList API 应用程序并返回刚刚从 ContactsList API 的应用程序所提供的联系人信息的名称。 这里是 ContactNames Get 方法成功调用 Swagger UI 屏幕。

![](./media/app-service-api-dotnet-consume-internal/tryitout.png)

有关如何为**公共 （匿名）**或**公共 （身份验证）**访问级别调用 API 的应用程序配置的信息，请参阅[从 Azure 应用程序服务中的.NET 客户端 API 应用程序使用](app-service-api-dotnet-consume.md)。

## 先决条件

本教程假定您熟悉如何创建项目并向其中添加代码，在 Visual Studio 中，以及如何[管理 API 在 Azure 预览门户的应用程序](../app-service-api-apps-manage-in-portal.md)。

在这篇文章中的项目和代码示例基于所创建和部署这些文章中的 API 的应用程序项目︰

- [创建 API 的应用程序](app-service-dotnet-create-api-app.md)
- [部署应用程序 API](app-service-dotnet-deploy-api-app.md)

[AZURE.INCLUDE [install-sdk-2013-only](../../includes/install-sdk-2013-only.md)]

本教程需要 2.6 或更高版本的 Azure sdk 版本的.NET。

### 设置目标 API 应用程序

1. 如果尚未执行此操作，请按照[部署 API 应用程序](app-service-dotnet-deploy-api-app.md)教程到 Azure 订购 API 的应用程序部署的 ContactsList 示例项目。

2. 在[Azure 预览门户](https://portal.azure.com/)， **API 应用程序**刀片式服务器的前面，部署 ContactsList API 应用程序中单击**设置 > 应用程序设置**和**访问级别**设置为**内部**，然后单击**保存**。

    ![](./media/app-service-api-dotnet-consume-internal/setinternal.png)
 
## 创建新的 API 应用程序将调用现有的 API 应用程序

- 在 Visual Studio 中，创建名为 ContactNames，使用 Azure API 的应用程序项目模板的 API 的应用程序项目。

    这是您在[创建应用程序 API](app-service-dotnet-create-api-app.md)，遵循相同的过程，但将在本教程后面部分不同的代码添加到项目。
 
## 添加应用程序服务 SDK 生成客户端代码

以下步骤将[使用从 Azure 应用程序服务中的.NET 客户端 API 应用程序](app-service-api-dotnet-consume.md)中更详细地介绍。 

3. 在**解决方案资源管理器**中右击项目 （而不是解决方案），并选择**添加 > Azure API 应用程序客户端**。 

3. 在**添加 Azure API 应用程序客户端**对话框中，单击**下载从 Azure API 应用程序**。 

5. 从下拉列表中，选择您想要调用的 API 应用程序。 本教程中选择 ContactsList API 应用程序之前创建。

7. 单击**确定**。 

## 启用 Swagger 用户界面

默认情况下，API 的应用程序项目将启用自动[Swagger]与(http://swagger.io/ "Swagger 官方信息")元数据生成，但 Azure API 应用程序新项目模板禁用 API 测试页。 在本节中，您启用测试页。

1. 打开*App_Start/SwaggerConfig.cs*文件，然后搜索为**EnableSwaggerUI**:

2. 取消注释下面的代码行︰

            })
        .EnableSwaggerUi(c =>
            {

## 创建控制器

5. 用鼠标右键单击**控制器**文件夹，并选择**添加 > 控制器**。 

6. 在**添加构架**对话框中，选择**Web API 2 控制器-空**选项，单击**添加**。 

7. 命名的控制器**ContactNamesController**，然后单击**添加**。 

## 添加代码以调用 API 的应用程序

若要调用已受其访问级别设置为**内部**API 应用程序，您必须将内部身份验证标头添加到 HTTP 请求。 这些头文件通知目标 API 应用程序调用的源是一个对等 API 应用程序从调用同一资源组中。 

应用程序服务 SDK 生成简化编写调用 API 的应用程序的代码的客户端类。 要调用**（匿名） 公共**API 的应用程序所要做是创建客户端对象，并调用方法，如本示例所示︰

        var client = new ContactsList();
        var contacts = await client.Contacts.GetAsync();

但是，若要添加身份验证标头需要访问`HttpRequestMessage`对象，并且您没有来这里。 有权访问该请求并添加标头，则必须创建`DelegatingHandler`类，并在它的实例生成的客户端的构造函数中传递。

1. 向项目中添加类文件命名为*InternalCredentialHandler.cs*，和模板代码替换为以下代码。

        using Microsoft.Azure.AppService.ApiApps.Service;
        using System.Net.Http;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace ContactNames
        {
            public class InternalCredentialHandler : DelegatingHandler
            {
                protected override Task<HttpResponseMessage> SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
                {
                    Runtime.FromAppSettings(request).SignHttpRequest(request);
                    return base.SendAsync(request, cancellationToken);
                }
            }
        }

    此代码调用`SignHttpRequest`添加到发送生成的客户端类的每个请求的身份验证标头︰

        Runtime.FromAppSettings(this.Request).SignHttpRequest

1. 在*ContactNamesController.cs*中，用下面的代码替换模板代码。

        using ContactNames.Models;
        using Microsoft.Azure.AppService.ApiApps.Service;
        using System;
        using System.Collections.Generic;
        using System.Net.Http;
        using System.Net.Http.Headers;
        using System.Threading.Tasks;
        using System.Web.Http;
        
        namespace ContactNames.Controllers
        {
            public class ContactNamesController : ApiController
            {
                [HttpGet]
                public async Task<IEnumerable<string>> Get()
                {
                    var names = new List<string>();

                    var client = new ContactsList(new DelegatingHandler[] { new InternalCredentialHandler() });
                    var contacts = await client.Contacts.GetAsync();
        
                    foreach (Contact contact in contacts)
                    {
                        names.Add(contact.Name);
                    }       
                    return names;
                }
            }
        }

    此代码将在处理程序传递给生成的客户端类的构造函数︰

        var client = new ContactsList(new DelegatingHandler[] { new InternalCredentialHandler() });

### 部署

您不能通过在本地运行测试。  您必须将代码部署并运行它在 Azure API 应用程序中;否则您将无法添加适当的身份验证标头，并将会一直拒绝呼叫。

[部署一个 API 应用程序](app-service-dotnet-deploy-api-app.md)中更详细地说明以下的部署步骤。 

1. 创建一个 ContactNames API 的应用程序。

    * 在**解决方案资源管理器**中右击项目 （而不是解决方案），然后单击**发布**。 

    * 单击**配置文件**选项卡，然后单击**Microsoft Azure API 的应用程序**。 

    * 单击**新建**设置 Azure 订购新 API 应用程序。

    * 在**创建 API 应用程序**对话框中，输入作为**API 的应用程序名称**ContactNames。 

    * **API 的应用程序**对话框中的其他值，请输入为[API 的应用程序部署](app-service-dotnet-deploy-api-app.md)之前输入相同的值。 

        最重要的是，请确保您要调用的 API 应用程序所在的资源组中创建新的 API 应用程序。

    * 单击**确定**。 

2. 将代码部署到新的 API 应用程序中。

    * 一旦配置 API 的应用程序，请右击**解决方案资源管理器**中的项目，单击**发布**以重新打开发布对话框中。

    * 在**发布网站**对话框中，单击**发布**着手部署过程。 

### 测试

在这一节中使用 Swagger UI 测试新的 API 应用程序并验证它可以调用 API 的应用程序之前创建。

1. 打开浏览器指向新 API 应用程序的 URL。

    使用默认的发布设置，Visual Studio 完成发布过程时它会自动打开浏览器访问 API 的应用程序的 URL。  如果没有发生，或者您关闭该浏览器窗口，按照以下步骤获得相同的 URL:

    * 在 Azure 预览门户，转到新的 ContactsName API 应用程序 API 应用程序刀片式服务器。

    * 请单击**URL**。 

        ![](./media/app-service-api-dotnet-consume-internal/clickurl.png)
  
5. 在浏览器地址栏中，添加`/swagger`到结尾的 URL，然后按回车键。 

    例如，生成的 URL 将如下所示︰

        https://microsoft-apiapp214f26e673e5449a214f26e673e5449a.azurewebsites.net/swagger

1. 在 Swagger 用户界面页中，单击**ContactNames > 获取 > 试一下 ！**

    ![](./media/app-service-api-dotnet-consume-internal/tryitout.png)
  
    该页显示在响应正文部分中，验证 ContactNames API 应用程序成功地从 ContactsList API 应用程序检索数据的联系人姓名。 

    如果您想要验证的**内部**设置了保护 ContactsList API 的应用程序，注释掉`SignHttpRequest`调用在*ContactNamesController.cs*，重新布置，Swagger**立即尝试**再次运行，并且您将收到错误消息。


## 添加代码以调用 API 应用程序使用 HttpClient
 
应用程序服务 SDK 取决于 Swagger API 定义，以生成客户端类。 如果您想要调用尚未实现 Swagger API 定义的 API 应用程序，您可以使用来完成`HttpClient`。 在这种情况下，仍然使用`SignHttpRequest`方法，但您调用直接上`HttpRequestMessage`对象。

1. 在*ContactNamesController.cs*，添加以下代码之前`return names;`语句中的`Get`方法。

        var httpClient = new HttpClient();
        HttpRequestMessage httpRequest = new HttpRequestMessage();
        httpRequest.Method = HttpMethod.Get;
        httpRequest.RequestUri = new Uri("https://{yourapiappurl}/api/contacts");
        Runtime.FromAppSettings(this.Request).SignHttpRequest(httpRequest);
        var response = await httpClient.SendAsync(httpRequest); 
        var contacts2 = await response.Content.ReadAsAsync<List<Contact>>();
        foreach (Contact contact in contacts2)
        {
            names.Add(contact.Name);
        }

    这段代码通过将传入请求对象中`SignHttpRequest`签署传出的请求对象的方法︰

        Runtime.FromAppSettings(this.Request).SignHttpRequest(httpRequest);

2. 在 Azure 预览门户中，转到 ContactsList API 应用程序，应用程序 API 刀片式服务器和复制的 URL。

    ![](./media/app-service-api-dotnet-consume-internal/clurl.png)
 
4. 实际的 URL，在控制器代码中替换占位符字符串"{yourapiappurl}"。 当您完成时，这行代码将类似下面的示例。

        httpRequest.RequestUri = new Uri("https://microsoft-apiappd99e684d99e684d99e684d99e684.azurewebsites.net/api/contacts");

### 部署和测试

1. 相同的步骤您先前未部署 API 的应用程序代码。

    **提示︰**可以绕过**发布站点**对话框中，通过单击一个按钮在工具栏上重新部署。  在 Visual Studio 中，单击**视图 > 工具栏**，并启用**Web 一键式发布**工具栏。  
 
2. 按照您以前未使用 Swagger 用户界面的过程。

    留在 HttpClient 代码中时，由于输出这一次显示一组重复的名称。

    ![](./media/app-service-api-dotnet-consume-internal/dupnames.png)
  
## 下一步行动

这篇文章已演示了如何使用.NET 客户端从一个内部 API 应用程序。 有关如何使用 API 的应用程序设置为**公共 （身份验证）**和**公共 （匿名）**的信息的访问级别，请参阅[从 Azure 应用程序服务中的.NET 客户端 API 应用程序使用](app-service-api-dotnet-consume.md)。  

有关其他示例从.NET 客户端调用 API 的应用程序的代码中，下载[Azure 卡](https://github.com/Azure-Samples/API-Apps-DotNet-AzureCards-Sample)示例应用程序。

在应用程序服务的身份验证信息，请参阅[API 的应用程序和移动应用程序的身份验证](../app-service/app-service-authentication-overview.md)。
 

测试
