---
ms.openlocfilehash: 12a425056248ffd0ebbf8830503d5a6374affc71
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="连接到在 Azure 应用程序服务 API 的应用程序的 web 应用程序" 
   description="本教程展示如何使用 API 应用程序中承载在 Azure 应用程序服务的 ASP.NET web 应用程序。" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="syntaxc4" 
   manager="yochayk" 
   editor="jimbe"/>

<tags
   ms.service="app-service-web"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="web" 
   ms.date="03/24/2015"
   ms.author="cfowler"/>

# 连接到在 Azure 应用程序服务 API 的应用程序的 web 应用程序

本教程展示如何使用 API 应用程序中承载[的应用程序](../app-service.md)服务的 ASP.NET web 应用程序。

## 先决条件

本教程为基础从 API 应用程序教程系列︰

1. [创建 Azure API 应用程序](../app-service-dotnet-create-api-app)
3. [Azure API 应用程序部署](../app-service-dotnet-deploy-api-app)
4. [调试 API Azure 应用程序](../app-service-dotnet-remotely-debug-api-app)

## 使可公开访问的 API 的应用程序

在[Azure 预览门户网站](http://go.microsoft.com/fwlink/?LinkId=529715)，请选择 API 的应用程序。 单击命令栏中的**设置**按钮。 在**应用程序设置**刀片式服务器，更改为**公共 （匿名）**的**访问级别**。

![](./media/app-service-web-connect-web-app-to-saas-api/4-5-Change-Access-Level-To-Public.png)

## 在 Visual Studio 中创建一个 ASP.NET MVC 应用程序

1. 打开 Visual Studio。 使用**新建项目**对话框来添加一个新的**ASP.NET Web 应用程序**。 单击**确定**。

    ![文件 > 新建 > Web > ASP.NET Web 应用程序](./media/app-service-web-connect-web-app-to-saas-api/1-Create-New-MVC-App-For-Consumption.png)

1. 选择的**MVC**模板。 单击**更改身份验证**，然后选择**无身份验证**，然后单击**确定**两次。

    ![新的 ASP.NET 应用程序](./media/app-service-web-connect-web-app-to-saas-api/2-Change-Auth-To-No-Auth.png)

1. 在解决方案资源管理器中，用鼠标右键单击新创建的 Web 应用程序项目并选择**添加 Azure 应用程序引用**。

    ![添加 Azure 应用程序 Api...](./media/app-service-web-connect-web-app-to-saas-api/3-Add-Azure-API-App-SDK.png)

1. 在**现有的 API 应用程序**下拉列表中，选择您想要连接到该 API 应用程序。

    ![选择现有 API 的应用程序](./media/app-service-web-connect-web-app-to-saas-api/4-Add-Azure-API-App-SDK-Dialog.png)

    >[AZURE.NOTE] 用于连接到 API 的应用程序的客户端代码将自动生成从 Swagger API 端点。

1. 利用生成的 API 代码，打开 HomeController.cs 文件并替换`Contact`具有以下操作︰

        public async Task<ActionResult> Contact()
        {
            ViewBag.Message = "Your contact page.";
    
            var contacts = new ContactsList();
            var response = await contacts.Contacts.GetAsync();
            var contactList = response.Body;
    
            return View(contactList);
        }

    ![HomeController.cs 代码更新](./media/app-service-web-connect-web-app-to-saas-api/5-Write-Code-Which-Leverages-Swagger-Generated-Code.png)

1. 更新`Contact`视图，以反映动态联系人列表中使用下面的代码︰  
    <pre>添加到最顶部的视图文件 @model IList&lt;MyContactsList.Web.Models.Contact&gt;
    
    默认电子邮件地址替换为以下&lt;h3&gt;公共联系人&lt;/h3&gt;
    &lt;ul&gt;
    @foreach （var 模型中的联系人） { &lt;li&gt;&lt;href =&quot;mailto:@contact。电子邮件地址&quot;&gt;@contact。名称&amp;lt;@contact。电子邮件地址&amp;gt;&lt;/a&gt;&lt;/li&gt;
        } &lt;/ul&gt; 
    </pre>

    ![Contact.cshtml 代码更新](./media/app-service-web-connect-web-app-to-saas-api/6-Update-View-To-Reflect-Changes.png)

## Web 应用程序部署到 Web 应用程序在应用程序服务

按照[如何部署 Azure 的 web 应用程序](web-sites-deploy.md)提供的说明进行操作。

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## 会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)
* 旧的门户与新门户的更改的指南，请参阅︰[用于预览门户导航的引用](http://go.microsoft.com/fwlink/?LinkId=529715)
 
测试
