---
ms.openlocfilehash: fab060657ee0aad54f7712a6fe48fcd961d1f8d8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="部署在 Azure 应用程序服务 API 的应用程序 " 
    description="了解如何将一个 API 的应用程序项目部署到 Azure 订购。" 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="bradygaster" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/14/2015" 
    ms.author="tdykstra"/>

# 部署在 Azure 应用程序服务 API 的应用程序 

## 概述

在本教程中，您将部署新[API 的应用程序](app-service-api-apps-why-best-platform.md)与[前面的教程](app-service-dotnet-create-api-app.md)中创建的 Web API 项目。 在[Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md)创建 API 的应用程序资源，并将 Web API 代码部署到 Azure API 的应用程序，您将使用 Visual Studio。 

### 其他部署选项

有许多其他方式来部署 API 的应用程序。 API 的应用程序是[web 应用程序](../app-service-web/app-service-web-overview.md)与承载 web 服务的额外功能，还可以与 API 应用程序使用所有[可用的 web 应用程序的部署方法](../app-service-web/web-sites-deploy.md)。 Web 应用程序主机 API 的应用程序在 Azure 预览门户中，调用 API 的应用程序主机，并且您可以通过使用 API 的应用程序主机门户刀片配置部署。 有关 API 的应用程序主机刀片式服务器的信息，请参阅[管理 API 的应用程序](app-service-api-manage-in-portal.md)。

API 的应用程序基于 web 的应用程序这一事实也意味着您可以部署为非 ASP.NET 平台 API 应用程序编写的代码。 使用 Git 到 API 的应用程序部署 Node.js 代码示例，请参阅[创建 Node.js API 的应用程序在 Azure 应用程序服务](app-service-api-nodejs-api-app.md)。
 
## <a id="provision"></a>在 Azure 创建 API 的应用程序 

在本节中，您使用 Visual Studio **Web 发布**向导在 Azure 创建 API 的应用程序。 其中说明指示您输入 API 的应用程序的名称，请输入*ContactsList*。

[AZURE.INCLUDE [app-service-api-pub-web-create](../../includes/app-service-api-pub-web-create.md)]

## <a id="deploy"></a>将代码部署到新的 Azure API 应用程序

您可以使用相同的**Web 发布**向导将代码部署到新的 API 应用程序。

[AZURE.INCLUDE [app-service-api-pub-web-deploy](../../includes/app-service-api-pub-web-deploy.md)]

## Azure API 应用程序调用 

由于您在前面的教程中启用 Swagger 用户界面，可以使用来验证 API 的应用程序正在运行在 Azure。

1. 在[Azure 预览门户网站](https://portal.azure.com)，请转到**API 应用程序**刀片式服务器 API 应用程序部署。

2. 单击 API 的应用程序的 URL。

    ![请单击该 URL](./media/app-service-dotnet-deploy-api-app/clickurl.png)

    将显示"已成功创建 API 应用程序"页。

3. 添加"/ swagger"到浏览器地址栏中的 URL 末尾。

4. 在 Swagger 页中显示，请单击**联系人 > 获取 > 试一下**。

    ![试一下](./media/app-service-dotnet-deploy-api-app/swaggerui.png)

## 在门户中查看 API 定义

1. 在[Azure 预览门户](https://portal.azure.com)，回到**API 应用程序**刀片式服务器 API 应用程序部署。

4. 单击**API 定义**。 
 
    应用程序的**API 定义**刀片式服务器显示时创建该应用程序定义的 API 操作列表中。 

    ![API 定义](./media/app-service-dotnet-deploy-api-app/29-api-definition-v3.png)

下一步，将对 API 定义进行更改并在门户中反映出来的变化，请参阅。

5. 返回到项目中，Visual Studio 并将下面的代码添加到**ContactsController.cs**文件。   

        [HttpPost]
        public HttpResponseMessage Post([FromBody] Contact contact)
        {
            // todo: save the contact somewhere
            return Request.CreateResponse(HttpStatusCode.Created);
        }

    此代码将添加可用于新张贴的**发布**方法`Contact`api 的实例。

    现在联系人类的代码类似于下面的示例。

        public class ContactsController : ApiController
        {
            [HttpGet]
            public IEnumerable<Contact> Get()
            {
                return new Contact[]{
                            new Contact { Id = 1, EmailAddress = "barney@contoso.com", Name = "Barney Poland"},
                            new Contact { Id = 2, EmailAddress = "lacy@contoso.com", Name = "Lacy Barrera"},
                            new Contact { Id = 3, EmailAddress = "lora@microsoft.com", Name = "Lora Riggs"}
                        };
            }
        
            [HttpPost]
            public HttpResponseMessage Post([FromBody] Contact contact)
            {
                // todo: save the contact somewhere
                return Request.CreateResponse(HttpStatusCode.Created);
            }
        }

7. 在**解决方案资源管理器**中右键单击项目并选择**发布**。 

9. 单击**预览**选项卡

10. 单击**开始预览**以查看哪些文件将被复制到 Azure。  

    ![发布 Web 对话框](./media/app-service-dotnet-deploy-api-app/39-re-publish-preview-step-v2.png)

11. 单击**发布**。

6. 就像第一次发布，请重新启动该网关。

12. 发布过程完成后，请返回到门户，并关闭并重新打开**API 定义**刀片式服务器。 您将看到新的 API 端点刚创建和部署直接到 Azure 订购。

    ![API 定义](./media/app-service-dotnet-deploy-api-app/38-portal-with-post-method-v4.png)

## 下一步行动

您已经看到如何在 Visual Studio 中的直接部署功能方便地循环访问和快速部署和测试您的 API 能够正常工作。 在[下一步的教程](../app-service-dotnet-remotely-debug-api-app.md)中，您将看到如何在 Azure 中运行时调试 API 的应用程序。
 

测试
