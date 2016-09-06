---
ms.openlocfilehash: ab4ebc8a190568035743e167058917c1b451aa61
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="发布 Azure 应用程序向导"
   description="发布 Azure 应用程序向导"
   services="visual-studio-online"
   documentationCenter="na"
   authors="kempb"
   manager="douge"
   editor="tlee" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2015"
   ms.author="kempb" />

# 发布 Azure 应用程序向导

## 概述

开发 web 应用程序在 Visual Studio 中的后，您可以通过**发布 Azure 应用程序**向导发布更方便地到 Azure 的云服务的应用程序。 第一节介绍的步骤使用向导，和其余章节解释向导的功能前必须完成。

>[AZURE.NOTE] 本主题是有关部署到云服务，不到 web 站点。 有关部署到 web 站点的信息，请参阅[如何部署一个 Azure 的 Web 站点](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false)。

## 先决条件

发布到 Azure web 应用程序之前，需要有 Microsoft 帐户和 Azure 的订阅，并且必须将 web 应用程序关联的 Azure 的云服务。 如果您已经完成了这些任务，您可以跳过下一节。

1. 获得 Microsoft 帐户和 Azure 的订阅。 您可以尝试免费一个月的免费 Azure 订阅[此处](https://azure.microsoft.com/pricing/free-trial/)

1. 创建 Azure 云服务和存储帐户。 您可以从服务器资源管理器在 Visual Studio 中，或通过使用[Azure 管理门户](http://go.microsoft.com/fwlink/?LinkID=213885)来执行此操作。 有关如何设置 Azure 环境的详细信息，请参阅[设置了服务发布所需从 Visual Studio 的云服务](vs-azure-tools-publish-azure-application-wizard)。

1. 启用 web 应用程序的 Azure。 若要使 web 应用程序发布到 Azure Visual Studio 中，您需要将其与在 Visual Studio 中 Azure 的云服务项目。 若要创建相关联的云服务项目，打开 web 应用程序项目的快捷菜单，然后选择转换，**转换到 Azure 云服务项目**。

1. 云服务项目添加到解决方案后，再次打开相同的快捷菜单，然后选择**发布**。 有关如何启用 Azure 应用程序的详细信息，请参阅[如何︰ 迁移和发布 Web 应用程序到 Azure 云服务从 Visual Studio](https://msdn.microsoft.com/library/azure/hh420322.aspx)。

>[AZURE.NOTE] 请务必使用管理员凭据 （以管理员身份运行） 启动 Visual Studio。

1. 发布您的应用程序准备就绪时，打开 Azure 的云服务项目的快捷菜单，然后选择**发布**。 下面的步骤演示了发布 Azure 应用程序向导。

## 选择您的订购

### 选择订阅

1. 第一次使用该向导之前，您必须登录。 选择**登录**链接。 登录提示时，Azure 的门户网站，提供 Azure 用户名称和密码。 

    ![这是一个发布向导屏幕](./media/vs-azure-tools-publish-azure-application-wizard/IC799159.png)

    与您的帐户相关联的订阅订阅列表中的填充。 您还可能看到从以前导入的任何订阅文件订阅。

1. 中**选择您的订阅**列表中，选择要用于此部署订阅。

   如果选择**<...管理 >**，出现**管理订阅**对话框，并且您可以选择您想要使用的订阅和用户帐户。 **帐户**选项卡显示所有帐户，并且**订阅**选项卡显示所有与帐户相关联的订阅。 您还可以选择在其中使用 Azure 的资源，以及创建或从 Azure 门户导入证书为您的订阅的区域。 如果您从订阅文件导入任何订阅，相关联的证书将显示在**证书**选项卡下。 当您完成时，选择**关闭**按钮。

    ![管理订阅](./media/vs-azure-tools-publish-azure-application-wizard/IC799160.png)

    >[AZURE.NOTE] 订阅文件可以包含多个订阅。

1. 选择**下一步**按钮继续。 

    如果您的订阅中没有任何的云服务，则需要在 Azure 来承载您的项目中创建一个云服务。 此时将显示**创建云服务和存储帐户**对话框。

    指定在云服务的新名称。 该名称必须是唯一在 Azure 中。 然后指定地区或接近于您或您的客户端的大部分数据中心的关联组。 此名称还用于 Azure 的云服务创建一个新的存储帐户。

1. 修改任何设置要为这一部署，然后将它发布通过选择**发布**按钮 （下一节提供了有关各种设置的更多详细信息）。 若要查看发布之前的设置，请选择**下一步**按钮。

    >[AZURE.NOTE] 如果在此步骤中选择了发布，则可以监视此部署在 Visual Studio 中的状态。

您可以通过**发布 Azure 应用程序**向导修改公共和高级部署设置。 例如，您可以选择设置部署到测试环境中应用程序之前释放它。 下图显示了 Azure 部署的**常见设置**选项卡。

![通用设置](./media/vs-azure-tools-publish-azure-application-wizard/IC749013.png)

## 配置您的发布设置

### 若要配置发布设置

1. 在**云服务**列表中，执行以下几组步骤之一︰

   1. 在下拉列表框中，选择一个现有的云服务。 该服务的数据中心位置出现。 应该记下该位置，并确保您的存储帐户位置位于同一个数据中心。

    1. 选择**创建新**创建承载 Azure 的云服务。 在**创建云服务**对话框中，为服务提供一个名称，然后指定地区或相关性组来指定您想要主持此云服务的数据中心的位置。 该名称必须是唯一在 Azure 中。

1. 在**环境**列表中，选择**生产**或**临时**。 如果要部署到测试环境中的应用程序，请选择暂存环境。 您可以移动稍后应用到生产环境。

1. 在**生成配置**列表中，选择**调试**或**发布**。

1. 在**服务配置**列表中，选择**云**或**本地**。

    如果您希望能够远程连接到该服务，请选择**所有角色启用远程桌面**复选框。 此选项主要用于故障排除。 当选中此复选框时，将出现**远程桌面配置**对话框。 选择设置链接来更改配置。

    选择**启用所有的 web 角色部署 Web**复选框以启用 web 服务的部署。 必须启用远程桌面，才能使用此功能。 有关详细信息，请参阅[[发布云服务使用 Azure 工具](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx)。 有关 Web 部署的详细信息，请参阅[[发布云服务使用 Azure 工具](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx)。

1. 选择**高级设置**选项卡。 在**部署标签**字段中，接受默认名称，或者输入您选择的名称。 要追加到部署标签的日期，将选中该复选框。

    ![发布向导的第三个屏幕](./media/vs-azure-tools-publish-azure-application-wizard/IC749014.png)

1. 在**存储帐户**列表中，选择要用于此部署的存储帐户。 进行比较的数据中心，云服务和您的存储帐户的位置。 理想情况下，这些位置应该是相同的。

    >[AZURE.NOTE] Azure 存储帐户存储的应用程序部署的包。 部署应用程序之后，从存储帐户删除软件包。

1. 如果您要部署更新后的组件，请选中**部署的更新**复选框。 这种部署可以更完整的部署。 选择**设置**链接可打开**部署更新设置**对话框中，在下图中所示。 

    ![部署设置](./media/vs-azure-tools-publish-azure-application-wizard/IC617060.png)

    您可以选择更新部署，增量或同时进行的两个选项。 增量部署更新一次，一个已部署的实例，以便您的应用程序保持在线且可供用户使用。 同时部署一次更新所有已部署的实例。 同时更新比增量更新，速度快，但如果您选择此选项，您的应用程序可能不能在更新过程中。

    如果不能更新部署，应选择复选框如果您希望完全部署更新部署失败时将执行自动进行全面部署。 整个部署将重置为云服务的虚拟 IP (VIP) 地址。 有关详细信息，请参阅[如何︰ 为云服务保持一个恒定的虚拟 IP 地址](https://msdn.microsoft.com/library/azure/jj614593.aspx)。


1. 若要调试服务，选择**启用 IntelliTrace**复选框，或者如果您正在部署的**调试**配置，要调试在 Azure 云服务，选择要部署的远程调试服务的**所有角色启用远程调试器**复选框。

2. 分析应用程序，选中**启用分析**复选框，然后选择**设置**链接来显示性能分析选项。 


    >[AZURE.NOTE] 必须使用 Visual Studio 最终以启用层交互分析 （提示） 或 IntelliTrace 并不能在同一时间同时启用。

    有关详细信息，请参阅[调试 IntelliTrace 与 Visual Studio 发布云服务](https://msdn.microsoft.com/library/azure/ff683671.aspx)和[云服务的性能测试](https://msdn.microsoft.com/library/azure/hh369930.aspx)。

1. 选择**下一步**查看该应用程序的摘要页。

## 发布应用程序

1. 您可以选择创建发布配置文件从您选择的设置。 例如，您可以创建一个测试环境的配置文件，另一个用于生产。 若要保存此配置文件，选择**保存**图标。 该向导将创建配置文件并将其保存在 Visual Studio 项目中。 若要修改该配置文件名称，打开**目标配置文件**列表中，然后选择**<...管理 >**。

    ![在发布向导的摘要屏幕](./media/vs-azure-tools-publish-azure-application-wizard/IC749015.png)

    >[AZURE.NOTE] 发布配置文件显示在解决方案资源管理器在 Visual Studio 中，并且配置文件设置写入到带有.azurePubxml 扩展名的文件。 设置保存为 XML 标记的属性。

1. 选择**发布**发布您的应用程序。 您可以监视进程状态在 Visual Studio 中的**输出**窗口中。

## 请参见

[设置发布从 Visual Studio 的云服务所需的服务](https://msdn.microsoft.com/library/azure/ff683668.aspx)

[如何︰ 迁移和发布 Web 应用程序到 Azure 的云服务从 Visual Studio](https://msdn.microsoft.com/library/azure/hh420322.aspx)

[云服务使用 Azure 工具发布](https://msdn.microsoft.com/library/azure/ff683672.aspx)

[调试发布的云服务使用 IntelliTrace 和 Visual Studio](https://msdn.microsoft.com/library/azure/ff683671.aspx)

[云服务的性能测试](https://msdn.microsoft.com/library/azure/hh369930.aspx)


测试
