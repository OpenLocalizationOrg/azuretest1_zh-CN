---
ms.openlocfilehash: 0c57332c38e9782412b70d6baf9c391163a303d7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="管理 Azure 的云资源管理器的资源"
   description="了解如何使用云资源管理器中浏览并管理 Azure 在 Visual Studio 中的资源。"
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

# 管理 Azure 的云资源管理器的资源

##概述

云资源管理器中旨在使您可以更轻松、 快捷地浏览和管理 Azure Visual Studio IDE 内的资源。 可以例如，使用它在 Azure 门户或在浏览器中打开 Web 应用程序或附加一个调试程序，或者您可以查看 blob 容器的属性和 Blob 容器编辑器中打开它。

云资源管理器是基于 Azure 的资源管理器堆栈上，就像 Microsoft Azure 预览门户。 它了解如 Azure 的资源组的资源和 Azure 服务逻辑应用程序和 API 的应用程序，例如，它支持[基于角色的访问控制](../role-based-access-control-configure/)(RBAC)。 若要查看已添加或更改的 Azure 资源，选择云资源管理器工具栏上的**刷新**按钮。

Azure SDK 2.7 云资源管理器中安装 Visual Studio 工具的一部分。 

## 先决条件

- Visual Studio 2015 RTM。

- Visual Studio Azure sdk 工具。 
- 您还必须 Azure 帐户和登录到其云资源管理器中查看 Azure 的资源。 如果您没有，您可以在几分钟创建一个帐户。 如果您订阅了 MSDN，请参阅[MSDN 订户的 Azure 福利](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)。 否则，请参阅[创建免费的试用帐户](http://azure.microsoft.com/pricing/free-trial/)。

- 如果看不到云资源管理器中，可以通过选择**视图**，在菜单栏上的**其他窗口，** **云资源管理器中**查看它。

## 管理 Azure 帐户和订阅

若要查看资源 Azure 云资源管理器中的，您需要登录到 Azure 帐户的一个或多个活动订阅。 如果您有多个 Azure 帐户，可以在云中资源管理器中添加，然后选择想要包括在资源视图中云资源管理器中的订阅。

如果您未使用 Azure 之前，或没有必要的帐户添加到 Visual Studio，将提示您这样做。

## 若要添加 Azure 帐户添加到云资源管理器

1. 选择云资源管理器工具栏上的设置图标。

1. 选择**添加帐户**链接。 登录到 Azure 帐户要浏览其的资源。 应在帐户选择器下拉列表中选择您刚才添加的帐户。 该帐户订阅的帐户项下出现。

    ![添加 Azure 订阅](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819514.png)

    ![选择 Azure 订阅](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819515.png)

1. 选择您想要浏览，然后选择**应用**按钮的帐户订阅所对应的复选框。

    云资源管理器中显示选定订阅的 Azure 资源。

## 若要删除 Azure 帐户

1. 请选择**文件**，在菜单栏上的**帐户设置**。

1. 在**帐户设置**对话框中的**所有帐户**部分中，选择**删除**命令的帐户旁要从中删除。 注意此命令从 Visual Studio--它只删除该帐户并不影响 Azure 帐户本身。

## 查看资源类型或组

若要查看您 Azure 的资源，您可以选择**资源类型**或**资源组**视图。

![资源视图下拉列表](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819516.png)

- **资源类型**视图，它也是 Azure 门户上使用的通用视图，显示您 Azure 的资源按其类型，例如 web 应用程序、 存储帐户和虚拟机进行分类。 这是类似于如何 Azure 资源出现在服务器资源管理器中。

- 资源组视图分类 Azure Azure 资源组所涉及的资源。

 
    资源组是 Azure 的资源，通常由特定应用程序的包。 若要了解有关 Azure 的资源组的详细信息，请参阅[Azure 资源管理器的概述](https://azure.microsoft.com/documentation/articles/resource-group-overview/)。

## 查看和导航资源

若要定位到 Azure 的资源和云资源管理器中查看其信息，展开项目的类型或相关联的资源组，然后选择资源。 当您选择某个资源时，在两个选项卡底部的云资源管理器中显示信息。

![选择一个资源视图](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819517.png)

- **操作**选项卡为所选资源云资源管理器中显示您可以采取的操作。 您还可以查看资源的快捷菜单上的可用操作。

- **属性**选项卡显示的资源，例如与之关联的类型、 区域设置和资源组的属性。

每个资源都有**开放门户中**的操作。 选择此操作时，云资源管理器中将显示所选的资源 Azure 门户中。 此功能是用于导航到深层嵌套资源尤为方便。

标记和属性值也可能基于 Azure 的资源。 例如，web 应用程序和逻辑应用程序还具有**用浏览器打开**和**附加调试器**，除了**打开门户中**的操作。 您选择存储帐户 blob、 队列或表时，将显示操作以打开编辑器。 Azure 应用程序有**URL**和**状态**属性中，而存储资源有键和连接字符串属性。

## 搜索资源

在 Azure 帐户订阅中查找具有特定名称的资源，请在云资源管理器中的搜索框中输入名称。

![云资源管理器中查找资源](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC820394.png)

在搜索框中输入字符，这些字符匹配的资源显示在资源树中。


测试
