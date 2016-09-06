---
ms.openlocfilehash: 145f396f3a6e8bdcc5bf0b6c448c6f94c66f09a1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在五分钟后开始使用 Azure 存储 |Microsoft Azure" 
    description="快速斜向提刀在 Microsoft Azure Blob、 表和队列使用 Azure 快速入门、 Visual Studio 和 Azure 存储仿真器。 运行在五分钟内首个 Azure 存储应用程序。" 
    services="storage" 
    documentationCenter=".net" 
    authors="tamram" 
    manager="adinah" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="05/28/2015" 
    ms.author="tamram;selcint"/>

# 在五分钟后开始使用 Azure 存储 

很容易地开发针对 Azure 存储入门。 本教程演示了如何获取 Azure 存储应用程序设置并快速运行。 我们将演示很容易在 Azure 存储上斜向上的两种方案︰

- [对 Azure 存储仿真器本地运行首个 Azure 存储应用程序](#run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator)
- [对 Azure 存储在云中运行首个 Azure 存储应用程序](#run-your-first-azure-storage-application-against-azure-storage-in-the-cloud)

如果您想要了解更多关于 Azure 存储到代码之前，请参阅[下一个步骤](#next-steps)。

## 先决条件

请确保在开始之前具有以下先决条件︰

1. 若要编译并生成应用程序时，您需要在您的计算机上安装[Visual Studio](https://www.visualstudio.com/)的版本。 
2. 安装最新版本的[.NET 的 Azure SDK](http://azure.microsoft.com/downloads/)。 SDK 包含 Azure 快速入门示例项目，Azure 存储仿真器， [Azure 存储.NET 客户端库](https://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)。
3. 请确认您有在您的计算机上安装[.NET Framework 4.5](http://www.microsoft.com/download/details.aspx?id=30653) ，Azure 快速入门示例项目，我们将使用在本教程中所要求。 如果您不确定哪个版本的.NET Framework 安装在您的计算机，请参阅[方法︰ 确定哪些.NET Framework 版本都已安装](https://msdn.microsoft.com/vstudio/hh925568.aspx)。 或者，按**开始**按钮或 Windows 键，键入**控制面板**。 然后，单击**程序** > **程序和功能**，并确定是否已安装的程序中列出了.NET Framework 4.5。

Azure 存储客户端库的二进制文件的最新版本是[NuGet](https://www.nuget.org/packages/WindowsAzure.Storage/)上可用。


## 对 Azure 存储仿真器本地运行首个 Azure 存储应用程序

在开发时使用 Azure 存储的应用程序，您可以运行对[Azure 存储仿真器](storage-use-emulator.md)。 存储仿真器提供了模拟动向的 Azure Blob、 队列和表服务的本地环境。 可以使用存储仿真器本地测试存储应用程序，而无需创建 Azure 订阅向导或存储帐户，而不会带来任何成本。

来试一试，让我们创建一个简单的 Azure 存储应用程序在 Visual Studio 中使用 Azure 快速入门示例项目之一。 本教程重点介绍**Azure Blob 存储**、 **Azure 表存储**和**Azure 队列存储**示例项目︰

1. 启动 Visual Studio。
2. 从**文件**菜单上，单击**新建项目**。
3. 在**新建项目**对话框中，单击**已安装** > **模板** > **C#** > **云** > **快速启动** > **数据服务**。
    - 3.a。  选择下列模板之一︰ Azure Blob 存储、 Azure 表存储或 Azure 存储队列。 
    - 3.b。 请确保选择**.NET Framework 4.5**作为目标的框架。   
    - 3.c。 指定您的项目的名称和创建新的 Visual Studio 解决方案中，如下所示︰
    
    ![Azure 快速入门][Image1]

您可以在运行应用程序前查看源代码。 若要查看该代码，请在 Visual Studio 中的**视图**菜单上选择**解决方案资源管理器**。 然后，双击 Program.cs 文件。 

接下来，在 Azure 存储仿真器中运行示例应用程序︰

1.  按**开始**按钮或 Windows 键， *Azure 存储仿真器*，搜索和启动应用程序。
2.  在 Visual Studio 中，单击**生成**菜单上的**生成解决方案**。 
3.  在**调试**菜单上，请按**F11**可运行该解决方案的步骤，或按**f5 键**以运行该解决方案，从开始到结束。

## 对 Azure 存储在云中运行首个 Azure 存储应用程序

对 Azure 存储在云环境中运行，您需要订阅了 Azure 和存储帐户，如果您还没有︰ 

- 若要获取 Azure 的订阅，请参见[免费试用](http://azure.microsoft.com/pricing/free-trial/)、[购买选项](http://azure.microsoft.com/pricing/purchase-options/)，并[提供了成员](http://azure.microsoft.com/pricing/member-offers/)（MSDN，Microsoft 合作伙伴网络，并 BizSpark，其他 Microsoft 程序中的成员）
- 在 Azure 创建存储帐户，请参阅[如何创建、 管理或删除存储帐户](storage-create-storage-account.md)。

一旦您有一个帐户，您可以创建一个简单的 Azure 存储应用程序在 Visual Studio 中使用 Azure 快速入门示例项目之一。 本教程重点介绍**Azure Blob 存储**、 **Azure 表存储**和**Azure 队列存储**示例项目︰

1. 启动 Visual Studio。
2. 从**文件**菜单上，单击**新建项目**。
3. 在**新建项目**对话框中，单击**已安装** > **模板** > **C#** > **云** > **快速启动** > **数据服务**。
    - 3.a。 选择下列模板之一︰ Azure Blob 存储、 Azure 表存储或 Azure 存储队列。
    - 3.b。 请确保选择**.NET Framework 4.5**作为目标的框架。
    - 3.c。 指定您的项目的名称和创建新的 Visual Studio 解决方案。 

您可以在运行应用程序前查看源代码。 若要查看该代码，请在 Visual Studio 中的**视图**菜单上选择**解决方案资源管理器**。 然后，双击 Program.cs 文件。 

接下来，运行示例应用程序︰

1.  在 Visual Studio 中，在**视图**菜单上选择**解决方案资源管理器**。 双击的 App.config 文件和注释掉连接字符串 Azure SDK 存储仿真程序︰

    `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2.  取消注释为 Azure 存储服务的连接字符串，并提供在 App.config 文件中的存储帐户名称和访问键︰`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

    若要查找存储帐户名称和访问键，请参阅[什么是存储帐户](storage-whatis-account.md)。

3.  之后您提供存储帐户名称和访问键中 App.config 文件，请在**文件**菜单上，单击**全部保存**来保存所有的项目文件。
4.  单击**生成**菜单上的**生成解决方案**。
5.  在**调试**菜单上，请按**F11**可运行该解决方案的步骤或按**f5 键**以运行该解决方案。


## 下一步行动

这些资源以了解有关 Azure 存储的详细信息，请参阅︰

* [介绍 Microsoft Azure 存储](storage-introduction.md)
* [如何使用.NET 中的 Blob 存储](storage-dotnet-how-to-use-blobs.md)
* [如何使用.NET 中的表存储](storage-dotnet-how-to-use-tables.md)
* [如何使用队列存储从.NET](storage-dotnet-how-to-use-queues.md)
* [Azure 存储文档](http://azure.microsoft.com/documentation/services/storage/)
* [Azure 存储客户端库](https://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)
* [REST API，azure 存储](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png
 
