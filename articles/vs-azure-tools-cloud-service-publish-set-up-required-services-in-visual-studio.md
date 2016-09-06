---
ms.openlocfilehash: 5c3b842032bcfd4745d722ec7750f7bdf7d750f7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="设置发布从 Visual Studio 的云服务所需的服务"
   description="了解了云和存储帐户服务安装和配置 Azure 应用程序的过程"
   services="visual-studio-online"
   documentationCenter="na"
   authors="kempb"
   manager="douge"
   editor="tglee" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/13/2015"
   ms.author="kempb" />

# 设置发布从 Visual Studio 的云服务所需的服务

##概述

您可以发布一个云服务项目之前，必须设置以下服务︰

- **云服务**在 Azure 的环境中运行您的角色

- 提供访问 Blob、 队列和表服务**存储帐户**。

使用以下过程来设置这些服务和配置应用程序


## 创建一个云服务

要发布到 Azure 的云服务，您必须首先创建一个云服务，它在 Azure 的环境中运行您的角色。 作为描述[在此处](#to-create-a-cloud-service-by-using-the-azure-management-portal)，您可以创建在 Azure 管理门户中，云服务。 当您使用发布向导，您还可以在 Visual Studio 中创建云服务。

### 若要通过使用 Visual Studio 中创建一个云服务

1. 打开的 Azure 项目的快捷菜单，然后选择**发布**。

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)

1. 如果您还没有登录，您的用户名和密码登录的 Microsoft 客户或组织有关联的 Azure 订阅的帐户。

1. 选择**下一步**按钮以转到**设置**页面。

    ![发布向导的常用设置](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)

1. 在**云服务**列表中，选择**新建**。 此时将显示**创建 Azure 服务**对话框。

1. 输入您的云服务的名称。 名称构成您的服务的 URL 的一部分，因此必须是全局唯一的。 该名称不区分大小写。

### 若要通过使用 Azure 管理门户创建云服务

1. 登录到 Microsoft 网站上的[Azure 管理门户](http://go.microsoft.com/fwlink/?LinkId=253103)。

1. （可选）若要显示已创建的云服务的列表，请选择云服务链接在页面的左侧。

1. 选择**+**图标左下角，然后在出现的菜单上选择**云服务**。 有两个选项，**快速创建**和**自定义创建**，另一个屏幕。 如果选择**快速创建**，您可以通过指定其 URL 和地区，它将实际承载创建云服务。 如果选择**创建自定义**，立即可以通过指定一个包 （.cspkg 文件）、 配置 (.cscfg) 文件和证书发布云服务。 如果您打算将云服务发布 Azure 项目中使用**发布**命令，创建自定义并不是必需。 **发布**命令是 Azure 项目快捷方式菜单上可用。

1. 选择**快速创建**以后通过使用 Visual Studio 发布您的云服务。

1. 指定您的云服务的名称。名称旁边显示的完整 URL。

1. 在列表中，选择您的用户的大部分地区。

1. 在窗口的底部，选择**创建云服务**链接。

## 创建存储帐户。

存储帐户提供访问 Blob、 队列和表格服务。 您可以通过使用 Visual Studio 或[Azure 平台管理门户](http://go.microsoft.com/fwlink/?LinkId=253103)创建存储帐户。

### 若要通过使用 Visual Studio 中创建存储帐户。

1. 在解决方案资源管理器，打开**存储**节点的快捷菜单，然后选择**创建存储帐户**。

    ![创建一个新的 Azure 存储帐户](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)

1. 选择或**创建存储帐户**对话框中输入新的存储帐户的以下信息。
    - Azure 的订阅，您要添加的存储帐户。
    - 要使用新的存储帐户的名称。
    - 地区或相关性组 （如美国西部或东南亚）。
    - 要使用存储帐户，如地理冗余的复制类型。

1. 当您完成时，选择**创建**。在服务器资源管理器的**存储**列表中将显示新的存储帐户。

### 若要通过使用 Azure 管理门户创建存储帐户。

1. 登录到[Azure 平台管理门户](http://go.microsoft.com/fwlink/?LinkId=253103)在 Microsoft 网站上。

1. （可选）若要查看您的存储帐户，请在该页的左侧面板中选择**存储**链接。

1. 在页的左下角，选择**+**图标。

1. 在出现的菜单中，选择**存储**，然后选择**快速创建**。

1. 提供存储帐户名称会导致一个唯一的 url。

1. 为云服务提供一个名称。 名称旁边显示的完整 URL。

1. 在列表区域中，选择您的用户的大部分地区。

1. 指定是否要启用地理复制。 如果您启用地理复制功能，您的数据将保存在多个物理位置，以减少损失的可能性。 此功能使存储成本更高，但您可以通过启用地理位置创建存储帐户而不是以后再添加该功能时降低成本。 有关详细信息，请参阅[地区复制](http://go.microsoft.com/fwlink/?LinkId=253108)。

1. 在窗口的底部，选择**创建存储帐户**链接。

您在创建存储帐户后，您将看到可用于访问每个 Azure 存储服务，以及为您的帐户的主要和次要访问键中的资源的 Url。 您可以使用这些密钥对存储服务的请求进行身份验证。

>[AZURE.NOTE] 辅助快捷键提供相同的访问到您的存储帐户为主要的访问键，并生成作为备份主访问键受到。 另外，建议您定期的访问密钥重新生成。 您可以修改连接字符串设置，使用辅助键时重新生成的主键，然后您可以修改它以使用重新生成的主键时重新生成辅助键。

## 配置您的应用程序使用存储帐户提供服务

您必须配置访问存储服务使用 Azure 存储服务的已创建的任何角色。 要做到这一点，可以为 Azure 项目使用多个服务配置。 默认情况下，两个是 Azure 项目中创建的。 通过使用多个服务配置，您可以在代码中，使用相同的连接字符串，但每个服务配置中具有不同值的连接字符串。 例如，可以使用来运行和调试应用程序使用 Azure 存储仿真程序在本地的一种服务配置和发布到 Azure 应用程序不同的服务配置。 服务配置有关的详细信息，请参阅[配置 Azure 项目](https://msdn.microsoft.com/library/azure/ee405486.aspx)。

### 配置应用程序以使用存储帐户提供的服务

1. 在 Visual Studio 中打开您 Azure 的解决方案。 在解决方案资源管理器，打开快捷菜单的每个角色在 Azure 项目访问存储服务并选择**属性**。 Visual Studio 编辑器中显示角色的名称的页面。 页面将显示**配置**选项卡的字段。

1. 在角色属性页中选择**设置**。

1. 在**服务配置**列表中，选择您要编辑的服务配置的名称。 如果您想要向所有为此角色的服务配置进行更改，您可以选择**所有配置**。  有关如何更新服务配置的详细信息，请参阅[管理存储帐户连接字符串](https://msdn.microsoft.com/library/azure/8cda8963-ef0e-4f64-8d29-5eac467e5f53#ConnectionStrings)。

1. 若要修改连接字符串的任何设置，请选择**...** 设置旁边的按钮。 显示**创建存储连接字符串**对话框。

1. **使用连接**，在下选择**订阅**选项。

1. 在**订阅**列表中，选择您的订购。 订阅列表中的不包含您想要的那个，如果选择**下载发布设置**链接。

1. 在**帐户名称**列表中，选择您的存储帐户名。 Azure 的工具使用的.publishsettings 文件自动获取存储帐户凭据。 若要手动指定您的存储帐户凭据，选择**手动输入的凭据**选项，然后继续本过程。 可以从[管理门户网站](http://go.microsoft.com/fwlink/p/?LinkID=213885)获取您的存储帐户名称和主键。 如果您不想指定您的存储帐户设置手动，请选择**确定**按钮以关闭该对话框。

1. 选择**输入存储帐户**凭据链接。

1. 在**帐户名**框中，输入您的存储帐户的名称。

    >[AZURE.NOTE] 管理门户网站，登录，然后选择**存储**按钮。 门户显示存储帐户的列表。 如果您选择一个帐户时，打开它的页面。 您可以从此页面复制存储帐户的名称。 如果您使用的早期版本的管理门户，门户管理**存储帐户**视图中将显示您的存储帐户的名称。 若要复制此名称，在此视图中，**属性**窗口中将其选中，然后选择 Ctrl C 键。 要将名称粘贴到 Visual Studio 中，选择**帐户名称**文本框中，然后选择 Ctrl + V 键。

1. 在**帐户密码**框中，输入为主键，或从复制和粘贴它[管理门户](http://go.microsoft.com/fwlink/?LinkID=213885)。 
    可以从管理门户复制此注册表项︰

    1. 在适当的存储帐户的页面的底部，选择**管理密钥**按钮。

    1. 在**管理密钥访问**页上，选择主要的访问键，该文本，然后选择 Ctrl + C 键。

    1. 在 Azure 工具将密钥粘贴到**帐户密码**框。

    1. 您必须选择下面的选项以确定服务将如何访问存储帐户之一︰
        - **使用 HTTP**。 这是标准的选项。 例如， `http://<account name>.blob.core.windows.net`。
        - **使用 HTTPS**的安全连接。 例如， `https://<accountname>.blob.core.windows.net`。
        - 这三项服务的每个**指定自定义终结点**。 然后可以为该特定服务的字段键入这些终结点。 

        >[AZURE.NOTE] 如果您创建自定义的终结点，则可以创建一个更复杂的连接字符串。 当您使用此字符串格式时，您可以指定存储服务终结点，包括已注册的个人存储 Blob 服务的自定义域名。 此外可以授予访问仅在单个容器通过共享的访问签名 blob 资源。 有关如何创建自定义的终结点的详细信息，请参阅[配置 Azure 存储连接字符串](https://azure.microsoft.com/documentation/articles/storage-configure-connection-string/)。

1. 若要保存这些连接字符串更改，选择**确定**按钮，然后选择工具栏上的**保存**按钮。 保存这些更改后，可以通过使用[GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx)在代码中获取此连接字符串的值。 当您发布到 Azure 应用程序时，选择包含连接字符串的 Azure 存储帐户的服务配置。 发布您的应用程序后，请验证该应用程序就能如期作用对 Azure 存储服务

## 其他资源

[从 Visual Studio 发布到 Azure 的云服务](https://msdn.microsoft.com/library/azure/ee460772.aspx)


测试
