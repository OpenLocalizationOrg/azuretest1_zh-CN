---
ms.openlocfilehash: fc39a7e1ab6abeeb366be572528e55e7952bdba6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="访问使用 HTML 和 JavaScript Azure API 应用程序" 
    description="了解如何访问 API 的应用程序后端使用 HTML 和 JavaScript。" 
    services="app-service\api" 
    documentationCenter=".net"
    authors="bradygaster"
    manager="mohisri" 
    editor="tdykstra"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="JavaScript" 
    ms.topic="article" 
    ms.date="07/31/2015" 
    ms.author="bradygaster"/>

# 使用 Azure API 应用程序使用 HTML 和 JavaScript

## 概述

本文介绍如何在[Azure 应用程序服务](/documentation/services/app-service/)创建[API 的应用程序](app-service-api-apps-why-best-platform.md)的 HTML 和 JavaScript 的客户端。 文章假设使用知识的 HTML 和 JavaScript，并针对 REST 调用 API 应用程序使用[AngularJS](https://angularjs.org/) JavaScript 框架。 

有些文章经过之前这可帮助您入门。 

1. 在[创建 API 的应用程序](app-service-dotnet-create-api-app.md)，您将创建 API 的应用程序项目。
2. 在[部署 API 的应用程序](app-service-dotnet-deploy-api-app.md)，部署到 Azure 订购 API 的应用程序。
3. 在[调试 API 的应用程序](../app-service-dotnet-remotely-debug-api-app.md)，可以使用 Visual Studio 远程调试的代码，虽然它在 Azure 中运行。

本文将基于这些先前的文章展现 HTML 应用程序如何使用 JavaScript 来访问后端 API 应用程序。 

## 启用 CORS

通常情况下，将从不同的主机比 API 本身提供的 HTML 应用程序中需要 CORS （跨原始资源共享）。 API 的应用程序，没有用于启用 CORS 至少两个选项。 本节将介绍这些选项。 

### 启用了 CORS API 应用程序网关

可以配置 API 应用程序网关，以便 CORS 使用 Azure 预览门户。 通过添加**MS_CrossDomainOrigins** *设置*可以指定允许哪些 Url 调用 API 的应用程序。 本部分将介绍如何使用此*设置*来启用 CORS API 网关级别。 

1. 导航到 Azure 预览门户刀片式服务器 API 应用程序希望 CORS 启用。 一次您 API 的应用程序，单击*网关*图标。 

    ![单击 API 应用程序网关按钮](./media/app-service-api-javascript-client/19-api-app-blade.png)

1. 单击门户刀片式服务器中的**网关主机**链接。 

    ![单击此 API 的应用程序网关主机的链接](./media/app-service-api-javascript-client/20-gateway-host-blade.png)

1. 单击门户刀片式服务器中的**所有设置**链接。 

    ![所有的网关设置链接](./media/app-service-api-javascript-client/21-gateway-blade-all-settings.png)

1. 单击门户刀片式服务器中的**应用程序设置**按钮。

    ![网关应用程序设置](./media/app-service-api-javascript-client/22-gateway-app-settings-blade.png)

1. 添加**MS_CrossDomainOrigins**应用程序设置。 将设置的值以逗号分隔的要为您 API 的应用程序提供访问权限的 HTTP 主机列表。 如果您想要提供对多个主机访问权限，*设置*的值可以设置为类似于下面的代码中。 

        http://foo.azurewebsites.net, https://foo.azurewebsites.net, http://contactlistwebapp.azurewebsites.net

    下面的部分将介绍创建包含简单的 HTML 页所在的 Azure 资源组 API 应用程序中运行 Web 应用程序从调用 API 的应用程序的单独的 web 应用程序的过程。 因为这是唯一允许访问 API 的应用程序的主机，将设置设置为包含一台主机。 

        http://contactlistwebapp.azurewebsites.net

    下面的截屏演示后在 Azure 预览门户已保存此设置应外观。 

    ![](./media/app-service-api-javascript-client/23-app-settings-set.png)

在[Azure 移动服务.NET 更新](http://azure.microsoft.com/blog/2014/07/28/azure-mobile-services-net-updates/)的博客文章中详细讨论了**MS_CrossDomainOrigins**应用程序设置，因此检查这篇文章有关的设置的详细信息。

### 启用 Web API 代码 CORS

ASP.NET 文章[在 ASP.NET Web API 2 启用跨原始请求](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api)中深度启用 Web API 中的 CORS 的过程进行了说明。 API 应用程序使用 ASP.NET Web API 构建的过程是完全相同的但会总结如下。 跳过此部分，如果您已经为您 API 的应用程序启用 CORS，应该已经被正确设置。 

1. [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet 程序包提供 CORS 的功能。 它通过打开安装在**程序包管理器控制台**，并执行以下 PowerShell 脚本。 

        Install-Package Microsoft.AspNet.WebApi.Cors

1. 一旦该命令执行时，程序包管理器控制台和**packages.config**将反映新的 NuGet 程序包的加法。 

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/01-cors-installed.png)

1. 打开*App_Start/WebApiConfig.cs*文件。 将下面的代码行添加到文件中的**WebApiConfig**类的**注册**方法。 

        config.EnableCors();

    一旦更新了文件，代码应如下所示︰

        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
                
                config.EnableCors(); /* -- NEW CODE -- */
    
                // Web API routes
                config.MapHttpAttributeRoutes();
    
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }

1. 若要启用 CORS 的最后一步是界限要启用单个操作方法。 访问每个方法中的或在整个控制器上，添加**EnableCors**属性，如下面的代码所示。 

    > **注意**︰ 使用正则表达式使用 EnableCors 属性的参数的所有仅用于演示目的，并将打开您所有来源和所有 HTTP 请求的 API。 请谨慎使用此特性，其含义有所了解。

        using ContactList.Models;
        using System.Collections.Generic;
        using System.Web.Http;
        using System.Web.Http.Cors;
    
        namespace ContactList.Controllers
        {
            [EnableCors(origins:"*", headers:"*", methods: "*")] /* -- NEW CODE -- */
            public class ContactsController : ApiController
            {
                [HttpGet]
                public IEnumerable<Contact> Get()
                {
                    return new Contact[]
                    {
                        new Contact { Id = 1, EmailAddress = "barney@contoso.com", Name = "Barney Poland"},
                        new Contact { Id = 2, EmailAddress = "lacy@contoso.com", Name = "Lacy Barrera"},
                        new Contact { Id = 3, EmailAddress = "lora@microsoft.com", Name = "Lora Riggs"}
                    };
                }
        
                [HttpPost]
                public Contact Post([FromBody] Contact contact)
                {
                    return contact;
                }
            }
        }
1. 如果已经有部署到 Azure 之前添加 CORS 支持 API 的应用程序，重新部署代码以便 CORS 将启用您 Azure 托管 API 中。 

## 创建一个 Web 应用程序使用 API 的应用程序

在本节中，将创建一个新的空 Web 应用程序、 安装和执行它，请使用 AngularJS 并绑定到 API 的应用程序的一个简单的 HTML 前端。 会将花费很多 Web 应用程序部署到 Azure 应用程序服务。 使 HTML Web 应用程序将绑定到显示从 API 应用程序中，检索到的数据和联系人 api 为用户提供简单的用户界面。 

1. 右键单击您在前面部分中[创建 API 的应用程序](app-service-dotnet-create-api-app.md)，创建解决方案并选择**添加-> 新建项目**

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/02-add-project.png)

1. 选择**ASP.NET Web 应用程序**模板。 

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/03-new-web-project.png)

1. 从一个 ASP.NET 对话框中选择**空**模板。

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/04-empty-web.png)

1. 使用任何一个**程序包管理器**或通过使用**管理 NuGet 程序包**的上下文菜单项，安装[AngularJS](https://www.nuget.org/packages/angularjs) NuGet 程序包。

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/05-install-angular.png)

1. 使用任何一个**程序包管理器**或通过使用**管理 NuGet 程序包**的上下文菜单项，安装[引导](https://www.nuget.org/packages/bootstrap)NuGet 程序包。

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/05-install-bootstrap.png) 

1. 向 Web 应用程序项目中添加新的 HTML 文件。 

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/06-new-html-file.png) 

1. *Index.html*文件的名称。 

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/07-index-html.png)

1. 将引导数据库的 CSS 和 AngularJS JavaScript 文件添加到 HTML 页中，以及使用一个简单的引导数据库模板 ([此类](http://getbootstrap.com/examples/starter-template/)) 和创建空脚本标记页面做准备。 
    
    > 注意︰ 下面的 HTML 和 JavaScript 代码中的注释是 preludes 到此节中的后续步骤。  

        <!DOCTYPE html>
        <html xmlns="http://www.w3.org/1999/xhtml">
        <head>
            <title>Contacts HTML Client</title>
            <link href="Content/bootstrap.css" rel="stylesheet" />
            <style type="text/css">
                body {
                    margin-top: 60px;
                }
            </style>
        </head>
        <body>
        
            <nav class="navbar navbar-inverse navbar-fixed-top">
                <div class="container">
                    <div class="navbar-header">
                        <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar" aria-expanded="false" aria-controls="navbar">
                            <span class="sr-only">Toggle navigation</span>
                            <span class="icon-bar"></span>
                            <span class="icon-bar"></span>
                            <span class="icon-bar"></span>
                        </button>
                        <a class="navbar-brand" href="#">Contacts</a>
                    </div>
                    <div id="navbar" class="collapse navbar-collapse">
                        <ul class="nav navbar-nav"></ul>
                    </div>
                </div>
            </nav>
        
            <div class="container">
                <!-- contacts ui here -->
            </div>
        
            <script src="Scripts/angular.js" type="text/javascript"></script>
            <script type="text/javascript">
                /* client javascript code here */
            </script>
        
        </body>
        </html>

1. 添加到 HTML 中的**容器** *div*元素如下所示的 HTML 表代码。

        <!-- contacts ui here -->
        <table class="table table-striped" ng-app="myApp" ng-controller="contactListCtrl">
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Name</th>
                    <th>Email</th>
                    <th></th>
                </tr>
            </thead>
            <tbody>
                <tr ng-repeat="con in contacts">
                    <td>[[con.Id]]</td>
                    <td>[[con.Name</td>
                    <td>[[con.EmailAddress]]</td>
                    <td></td>
                </tr>
            </tbody>
            <tfoot>
                <tr>
                    <th>Create a new Contact</th>
                    <th colspan="2">API Status: [[status]]</th>
                    <th><button class="btn btn-sm btn-info" ng-click="refresh()">Refresh</button></th>
                </tr>
                <tr>
                    <td><input type="text" placeholder="ID" ng-model="newId" /></td>
                    <td><input type="text" placeholder="Name" ng-model="newName" /></td>
                    <td><input type="text" placeholder="Email Address" ng-model="newEmail" /></td>
                    <td><button class="btn btn-sm btn-success" ng-click="create()">Create</button></td>
                </tr>
            </tfoot>
        </table>

1. 在`tbody`，`tfoot`元素替换每个 [与 {和每个] 与}。 （此站点是当前无法显示在代码块中的双大括号的表达式。

2. 右键单击*index.html*文件，然后单击**设为起始页**。 

3. 右键单击*index.html*文件，然后单击**在浏览器中的查看**。 

    请注意模板车中的 HTML 输出。 您将数据绑定使用 AngularJS 的下一步这些 HTML 元素。 

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/09-template-ui.png)

1. 下面的 JavaScript 代码中加入*index.html*文件调用的 API 和数据绑定 API 输出到 HTML 用户界面。 

        /* client javascript code here */
        angular.module('myApp', []).controller('contactListCtrl', function ($scope, $http) {
            $scope.baseUrl = 'http://localhost:1578';

            $scope.refresh = function () {
                $scope.status = "Refreshing Contacts...";
                $http({
                    method: 'GET',
                    url: $scope.baseUrl + '/api/contacts',
                    headers: {
                        'Content-Type': 'application/json'
                    }
                }).success(function (data) {
                    $scope.contacts = data;
                    $scope.status = "Contacts loaded";
                }).error(function (data, status) {
                    $scope.status = "Error loading contacts";
                });
            };

            $scope.create = function () {
                $scope.status = "Creating a new contact";

                $http({
                    method: 'POST',
                    url: $scope.baseUrl + '/api/contacts',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    data: {
                        'Id': $scope.newId,
                        'Name': $scope.newName,
                        'EmailAddress': $scope.newEmail
                    }
                }).success(function (data) {
                    $scope.status = "Contact created";
                    $scope.refresh();
                    $scope.newId = 0;
                    $scope.newName = '';
                    $scope.newEmail = '';
                });
            };

            $scope.refresh();
        });

1、 您刚刚添加到 index.html，代码中的替换的基 URL 中的端口号 (`http://localhost:1578`) 与 API 项目的实际端口号。

    > **Note** Don't use the port number of the HTML client project. You can right-click the API project and click **Debug > Start New Instance** to get a browser window that shows the port number.

1. 请确保您运行 HTML 客户端，或 JavaScript HTML 将无法正常工作时也运行 API 的应用程序项目。 右击解决方案并选择**属性**。 将 Web 项目添加到**不调试的情况下启动**，并首先运行 API 项目。 

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/10-run-both-web-projects.png)

1. 运行解决方案和 HTML/JavaScript 客户端连接到并显示 API 的应用程序项目中的数据。 

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/11-web-client-running.png)

## 部署 Web 应用程序

在本节中将作为应用程序服务 Web 应用程序部署 HTML/JavaScript 客户端。 部署完成后，您将更改正在使用 JavaScript 来使用部署的 API 应用程序的 URL。 

> 注意︰ 本节假定您已经阅读并完成[部署 API 的应用程序](app-service-dotnet-deploy-api-app.md)项目或您以前已经部署了 API 应用程序。 

1. 在 Azure 预览门户打开 API 的应用程序的刀片式服务器。 单击刀片式服务器中的 URL，以在浏览器中打开它。 一旦打开，将复制出 API 的应用程序浏览器地址栏中的 URL。 

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/12-open-api-app-from-blade.png)

1. 粘贴 API 的应用程序的 URL 来覆盖的 JavaScript 代码中的**$scope.baseUrl**属性以前的值。 

        $scope.baseUrl = 'https://microsoft-apiappf7e042ba8e5233ab4312021d2aae5d86.azurewebsites.net';

    请注意 URL 指定 HTTPS。  使用 HTTPS 不是可选的。 API 的应用程序不支持 HTTP。

1. 用鼠标右键单击 HTML/JavaScript Web 项目并选择**发布**上下文菜单项。

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/13-publish-web-app.png)

1. 在发布网站对话框中选择**Microsoft Azure Web 应用程序**选项。 

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/14-publish-web-dialog.png)

1. 单击**新建**按钮以创建新的 Web 应用程序。

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/15-new-web-app.png)

1. 选择同一计划承载的应用程序和资源组中的 API 应用程序已在运行。

    > **注意**︰ 这不是一种要求，但出于演示的目的它便于您 Azure 时清理资源以后一切都包含在一个资源组。

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/16-new-web-app-creation-dialog.png)

1. 完成将 HTML/JavaScript 客户端部署到应用程序服务 Web 应用程序的 Web 发布步骤。 
1. 一旦部署 Web 应用程序它应自动在您的 Web 浏览器中打开并显示 API 应用程序中的数据。 

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/17-web-app-running-in-ie.png)

1. 在这种情况下，如果您浏览到该资源组将看到新 API 的应用程序一起运行的 Web 应用程序。 

    ![apiapp.json 和解决方案资源管理器中的元数据](./media/app-service-api-javascript-client/18-web-app-visible-in-resource-group.png)

## 下一步行动 

此示例演示了如何使用 AngularJS 作为 JavaScript 平台访问 API 的应用程序的后端。 您可以更改要使用任何其他 JavaScript 框架的其余部分访问功能。 

此示例演示未经身份验证的访问 API 的应用程序。 在应用程序服务的身份验证信息，请参阅[API 的应用程序和移动应用程序的身份验证](../app-service/app-service-authentication-overview.md)。

测试
