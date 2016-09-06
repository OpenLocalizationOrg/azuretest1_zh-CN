---
ms.openlocfilehash: 3c41316314c2c4439a3a1f91b8faaab513d11413
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="连接到使用混合连接本地 SQL Server 的 Azure 的移动应用程序 |Microsoft Azure"
    description="了解如何连接到内部部署 SQL Server 中使用混合连接的应用程序服务移动应用程序"
    services="app-service\mobile"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="07/30/2015"
    ms.author="glenga"/>


# 从使用混合连接的移动应用程序连接到本地 SQL Server

当企业过渡到云，您可能不能马上将您的资产的所有迁移到 Azure。 混合连接，可以安全地连接到您的内部资产的 Azure 应用程序服务中的移动应用程序功能。 这种方式，您可以使内部数据移动客户端可以访问使用 Azure。 支持的资产包括运行在一个静态的 TCP 端口，包括 Microsoft SQL Server，MySQL，HTTP Web Api 和大多数自定义 web 服务的任何资源。 混合连接使用共享访问签名 (SAS) 授权保护混合连接到您的移动服务和内部混合连接管理器的连接。 有关详细信息，请参阅[混合连接概述](../integration-hybrid-connection-overview.md)。

在本教程中，您将学习如何修改移动应用程序的.NET 后端使用本地的内部部署 SQL Server 数据库而不是默认的 Azure SQL 数据库与您的服务提供。

## 先决条件 ##

本教程要求您具有下列项目︰

- **现有的手机应用程序后端** <br/>按照要创建并下载新的.NET 后端移动应用程序从[Azure 门户][快速入门教程](app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-preview.md)。

[AZURE.INCLUDE [hybrid-connections-prerequisites](../../includes/hybrid-connections-prerequisites.md)]

## 安装 SQL Server Express，启用 TCP/IP，并创建一个 SQL Server 数据库内部

[AZURE.INCLUDE [hybrid-connections-create-on-premises-database](../../includes/hybrid-connections-create-on-premises-database.md)]

## 创建一个混合连接

您需要创建一个新的混合连接和 BizTalk 服务为您的移动应用程序端，这是 web 应用程序的代码部分。

1. 在[Azure 门户]中，浏览到您的移动应用程序，请单击 web 应用程序的后端按钮。

    ![导航到 web 应用程序](./media/app-service-mobile-dotnet-backend-hybrid-connections-get-started-preview/mobile-app-link-to-web-app-backend.png)

    您的移动应用程序，这是您的移动应用程序的名称为您的 web 应用程序实现后端代码此采用跟`-code`。

2. 向 web 应用程序的刀片下滚动并单击**混合连接**。

    ![混合连接](./media/app-service-mobile-dotnet-backend-hybrid-connections-get-started-preview/start-hybrid-connection.png)

2. 混合连接刀片式服务器，请单击**添加**，然后**新的混合连接**。

3. **创建混合连接刀片**上, 混合连接提供**名称**和**主机名**和**端口**设置为`1433`。

    ![创建一个混合连接](./media/app-service-mobile-dotnet-backend-hybrid-connections-get-started-preview/create-hybrid-connection.png)

4. 单击**Biz 交谈服务**为 BizTalk 服务输入一个名称并单击**确定**两次。

    本教程使用**mobile1**。 您将需要提供新的 BizTalk 服务的唯一名称。

    在过程完成后，**通知**区域会闪烁绿色的**成功**和**混合连接**刀片式服务器显示为**未连接**状态的新混合连接。

    ![创建一个混合连接](./media/app-service-mobile-dotnet-backend-hybrid-connections-get-started-preview/hybrid-connection-created.png)

此时，您已完成云混合连接基础结构的重要组成部分。 接下来，您将创建相应的内部部分。

## 安装内部混合连接管理器才能完成连接

[AZURE.INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

## 配置移动应用程序的后端项目连接到 SQL Server 数据库

在此步骤中，定义内部数据库的连接字符串，并将修改为使用此连接的移动应用程序后端。

1. 在 Visual Studio 2013，打开定义了移动应用程序端的项目。

    若要了解如何下载您的.NET 后端项目、[快速入门教程](app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-preview.md)，请参阅。

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

## 更新 Azure 使用内部连接字符串

接下来，您需要添加此新的连接字符串的应用程序设置，以便可以从 Azure 使用它。  

1. 回在[Azure 门户网站]在您的移动应用程序的 web 应用程序后端代码，请单击**所有设置**，然后**应用程序设置**。

3. 在**Web 应用程序设置**刀片式服务器，向下滚动到**连接字符串**并添加名为新的**SQL Server**连接字符串`OnPremisesDBConnection`类似的值与`Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>`。

    更换`<**secure_password**>`与本地数据库的安全密码。

    ![内部数据库的连接字符串](./media/app-service-mobile-dotnet-backend-hybrid-connections-get-started-preview/set-sql-server-database-connection.png)

2. 按**保存**以保存的混合连接和您刚创建的连接字符串。

## 发布和测试移动应用程序的后端在 Azure 中

最后，您需要发布到 Azure 的移动应用程序的后端，并验证它使用混合连接来将数据存储在内部数据库中。

3. 在 Visual Studio 中，右击该项目，单击**发布**，然后单击**发布网站**中的**Microsoft Azure 网站**。

    而不是使用 Visual Studio 中，您还可以[使用 Git 发布您的后端](mobile-services-dotnet-backend-store-code-source-control.md)。

2. 使用 Azure 的凭据登录，然后**选择现有网站**中选择您的服务。

    Visual Studio 将下载您发布直接从 Azure 的设置。

3. 最后，单击**发布**。

    发布完成后，会显示在服务重新启动和后端的起始页。

4. 位于开始页之前使用任一**立即尝试**按钮或使用连接到您的移动应用程序，客户端应用程序调用生成数据库更改某些操作。

    >[AZURE.NOTE]当您使用**立即尝试**按钮启动帮助 API 页面时，请务必提供您应用程序的密钥的密码 （使用空的用户名）。

4. 在 SQL Server 管理 Studio 中，连接到 SQL Server 实例打开对象资源管理器中，展开的**OnPremisesDB**数据库，展开**表**。

5. 用鼠标右键单击**hybridService1.TodoItems**表，然后选择**选择前 1000年行**以查看结果。

    请注意，生成的客户端应用程序中的更改保存到内部部署数据库使用混合连接您移动应用程序后端。

## 请参见 ##

+ [混合连接 web 站点](../../services/biztalk-services/)
+ [混合连接概述](../integration-hybrid-connection-overview.md)
+ [BizTalk 服务︰ 仪表板、 显示器、 比例、 配置和混合连接选项卡](../biztalk-dashboard-monitor-scale-tabs.md)
+ [如何使数据模型更改为.NET 后端移动服务](../mobile-services-dotnet-backend-how-to-use-code-first-migrations.md)

<!-- IMAGES -->


<!-- Links -->
[Azure 门户]: https://portal.azure.com/
[Azure 的管理门户]: http://go.microsoft.com/fwlink/p/?linkid=213885
[开始使用移动服务]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started.md

测试
