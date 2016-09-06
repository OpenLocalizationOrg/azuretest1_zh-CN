---
ms.openlocfilehash: f25d03143fd0475bbd2c959b15cb71ca855c18dd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="连接到本地 SQL Server 从 Azure 移动服务使用混合连接-Azure 移动服务" 
    description="了解如何连接到内部部署 SQL Server 从 Azure 移动服务使用混合连接" 
    services="mobile-services" 
    documentationCenter="" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="06/16/2015" 
    ms.author="glenga"/>

  
# 连接到本地 SQL Server 从 Azure 使用混合连接的移动服务 

当企业过渡到云，您可能不能马上将您的资产的所有迁移到 Azure。 混合连接，可以安全地连接到您的内部资产的 Azure 移动服务。 这种方式，您可以使内部数据移动客户端可以访问使用 Azure。 支持的资产包括运行在一个静态的 TCP 端口，包括 Microsoft SQL Server，MySQL，HTTP Web Api 和大多数自定义 web 服务的任何资源。 混合连接使用共享访问签名 (SAS) 授权保护混合连接到您的移动服务和内部混合连接管理器的连接。 有关详细信息，请参阅[混合连接概述](../integration-hybrid-connection-overview.md)。

在本教程中，您将学习如何修改.NET 后端移动服务以使用内部部署本地 SQL Server 数据库，而不是默认的 Azure SQL 数据库与您的服务提供。 此外支持 JavaScript 后端移动服务，如[本文](http://blogs.msdn.com/b/azuremobile/archive/2014/05/12/connecting-to-an-external-database-with-node-js-backend-in-azure-mobile-services.aspx)中所述混合连接。

##先决条件##

本教程要求您具有下列项目︰ 

- **现有的.NET 后端移动服务** <br/>按照本教程[开始使用移动服务]创建并下载新的.NET 后端移动服务从[Azure 管理门户]。

[AZURE.INCLUDE [hybrid-connections-prerequisites](../../includes/hybrid-connections-prerequisites.md)]

## 安装 SQL Server Express，启用 TCP/IP，并创建一个 SQL Server 数据库内部

[AZURE.INCLUDE [hybrid-connections-create-on-premises-database](../../includes/hybrid-connections-create-on-premises-database.md)]

## 创建一个混合连接

[AZURE.INCLUDE [hybrid-connections-create-new](../../includes/hybrid-connections-create-new.md)]

## 安装内部混合连接管理器才能完成连接

[AZURE.INCLUDE [hybrid-connections-install-connection-manager](../../includes/hybrid-connections-install-connection-manager.md)]

## 配置要连接到 SQL Server 数据库的移动服务项目

在此步骤中，定义内部数据库的连接字符串，并将修改移动服务以使用此连接。 

1. 在 Visual Studio 2013，打开定义.NET 后端移动服务的项目。 

    若要了解如何下载您的.NET 后端项目，请参阅[开始使用移动服务](mobile-services-dotnet-backend-windows-store-dotnet-get-started.md)。

2. 在解决方案资源管理器，打开 Web.config 文件，找到**connectionStrings**部分，添加一个新的 SqlClient 条目，如下所示，指向本地 SQL Server 数据库︰ 
    
        <add name="OnPremisesDBConnection" 
         connectionString="Data Source=OnPremisesServer,1433;
         Initial Catalog=OnPremisesDB;
         User ID=HybridConnectionLogin;
         Password=<**secure_password**>;
         MultipleActiveResultSets=True"
         providerName="System.Data.SqlClient" />

    请记住，替换`<**secure_password**>` *HbyridConnectionLogin*为您创建的密码与此字符串中。
    
3. 单击**保存**保存 Web.config 文件的 Visual Studio 中。

    > [AZURE.NOTE]在本地计算机上运行时使用此连接的设置。 Azure 中运行时，此设置将覆盖在门户中定义的连接设置。

4. 展开**模型**文件夹，然后打开数据模型文件中，在*Context.cs*中结束。

6. 修改**DbContext**实例构造函数传递值`OnPremisesDBConnection`基**DbContext**构造函数类似于下面的代码段︰

        public class hybridService1Context : DbContext
        {
            public hybridService1Context()
                : base("OnPremisesDBConnection")
            {
            }
        }

    服务现在将使用 SQL Server 数据库的新连接。
 
##测试本地数据库连接

之前发布到 Azure 并且使用混合连接，是一个好主意，确保数据库连接本地运行时能够正常工作。 通过这种方式可以更容易地诊断并解决任何连接问题之前重新发布并开始使用混合连接。

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service-api-documentation](../../includes/mobile-services-dotnet-backend-test-local-service-api-documentation.md)]

## 更新 Azure 使用内部连接字符串

现在，您已验证的数据库连接，您需要添加此新的连接字符串的应用程序设置，以便它可以从 Azure 使用和发布到 Azure 的移动服务。  

1. 在[Azure 管理门户]中，浏览到您的移动服务。
  
1. 单击**配置**选项卡，并找到**连接字符串**部分。 

    ![内部数据库的连接字符串](./media/mobile-services-dotnet-backend-hybrid-connections-get-started/11.png)

2. 添加新的连接**SQL Server**字符串命名为`OnPremisesDBConnection`的值如下所示︰

        Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>


    更换`<**secure_password**>`与*HybridConnectionLogin*的安全密码。

2. 按**保存**以保存的混合连接和您刚创建的连接字符串。

3. 使用 Visual Studio，将已更新的移动服务项目发布到 Azure。

    显示服务的起始页。

4. 位于开始页之前使用任一**立即尝试**按钮或使用连接到您的移动服务，客户端应用程序调用生成数据库更改某些操作。 

    >[AZURE.NOTE]当您使用**立即尝试**按钮启动帮助 API 页面时，请务必提供您应用程序的密钥的密码 （使用空的用户名）。

4. 在 SQL Server 管理 Studio 中，连接到 SQL Server 实例打开对象资源管理器中，展开的**OnPremisesDB**数据库，展开**表**。 

5. 用鼠标右键单击**hybridService1.TodoItems**表，然后选择**选择前 1000年行**以查看结果。

    请注意，在您的应用程序中生成的更改已保存到内部部署数据库使用混合连接您移动服务。

##请参见##
 
+ [混合连接 web 站点](../../services/biztalk-services/)
+ [混合连接概述](../integration-hybrid-connection-overview.md)
+ [BizTalk 服务︰ 仪表板、 显示器、 比例、 配置和混合连接选项卡](../biztalk-dashboard-monitor-scale-tabs.md)
+ [如何使数据模型更改为.NET 后端移动服务](mobile-services-dotnet-backend-how-to-use-code-first-migrations.md)

<!-- IMAGES -->
 

<!-- Links -->
[Azure 的管理门户]: http://manage.windowsazure.com
[开始使用移动服务]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md 
测试t
