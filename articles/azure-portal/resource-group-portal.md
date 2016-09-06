---
ms.openlocfilehash: 6d452f094bd36bad5b9a5adb35347fdc780403b0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 Azure 预览门户管理 Azure 资源" 
    description="将成为它所包含的资源的生命周期边界的逻辑组为多个资源进行分组。" 
    services="azure-portal" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-portal" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/22/2015" 
    ms.author="tomfitz"/>


# 使用 Azure 预览门户管理 Azure 资源

## 简介

从历史上看，管理 Microsoft Azure 中的资源 （用户管理实体如数据库服务器、 数据库或网站） 要求您执行一次对一个资源的操作。 如果您有多个资源的组成复杂的应用程序，该应用程序的管理变得复杂的任务。 在 Azure 预览门户，您可以创建资源组来管理您的应用程序中的所有资源放在一起。 资源组是作为它所包含的每个资源的生命周期边界的 Azure 中的一个新概念。 

资源组使您能够管理您的应用程序中的所有资源放在一起。 启用的新的管理功能，Azure 资源管理器的资源组。 Azure 的资源管理器可以作为一个逻辑组作为它所包含的每个资源的生命周期边界组的多个资源。 通常一个组将包含在特定应用程序相关的资源。 例如，一个组可以包含承载公共网站、 存储关系数据使用的网站上的 SQL 数据库和存储非关系资产存储帐户的网站资源。 

下面是如何使用 Azure 门户中的资源组的简要概述。 

## 创建资源组

只要在门户中创建资源时，它总是在资源组中创建。 您可以选择创建新的资源组，或创建流程中使用现有资源组。 <br><br />

![创建资源组](./media/resource-group-portal/1_createWebsite.png)

当您创建的应用程序包含少数资源一起工作 （例如网站 + 数据库） 始终在其中创建其自己的资源组，以便您可以管理使用资源组的所有相关资产的生命周期。 您可以添加或从资源组中移除额外的资源，随着您的应用程序的发展。 

![创建资源组](./media/resource-group-portal/2_createWSandDB.png)

## 浏览资源组

您可以通过单击屏幕左侧 Jumpbar 浏览所有资源组。 资源组有刀片，为您提供特定资源组上的所有信息。 资源组刀片还提供统一的视图，您的付费和监视信息的所有资源的资源组中。

摘要部分显示可视资源映射的所有资源的资源组中，它显示链接到的资源组中其他资源组的资源。 资源映射还显示每个资源的状态。 
![资源组摘要](./media/resource-group-portal/3_1BrowseRGs.png)

可自定义的资源映射部分以显示较大的尺寸，这将显示包含在资源组内的所有资源和资源链接其他资源组中。 这一部分可以固定到 Starboard，它会将部件复制到 Startboard 中。

![插针](./media/resource-group-portal/3_2BrowseRGs.png)

资源在地图上单击启动资源图上的所有资源的列表视图。 此视图将列出的资源组中的所有资源，或链接到它。 单击这些资源将启动其刀片。 

![请参阅资源](./media/resource-group-portal/3_3BrowseRGs.png)

## 将资源添加到资源组

向资源组使用资源组刀片式服务器上**添加**命令，可以添加资源。 流中的步骤操作将允许您将其他资源添加到资源组。

![添加资源](./media/resource-group-portal/4_AddResource.png)

注意︰ 不建议置于其他 Azure 资源所在的资源组的团队项目。 如果您创建的团队项目中使用新的帐户和组，然后创建一个网站，该网站用户组将默认到最后一组使用 （VSO） 和最终将与同组中的运行时/开发人员资源。 

## 删除用户组资源

由于资源组使您可以管理所包含的所有资源的生命周期，则删除资源组将删除它所包含的所有资源。 您还可以删除资源组内的单个资源。 您想要删除某一资源组有可能会被链接到其他资源时请务必小心。 您可以查看的资源映射中的链接的资源和采取必要的措施以避免任何意外的后果，当您删除资源组。 

## 下一步行动
入门教程  

- 资源管理器中的概念的介绍，请参见[Azure 资源管理器的概述](../resource-group-overview.md)。  
- 部署资源时使用 Azure PowerShell 的简介，请参见[使用 Azure PowerShell 使用 Azure 资源管理器中](../powershell-azure-resource-manager.md)。
- 部署资源时使用 Azure CLI 的简介，请参见[使用 Azure CLI 的 Mac、 Linux、 Windows Azure 资源管理](../xplat-cli-azure-resource-manager.md)。 
- 若要了解如何以逻辑方式组织您的资源，请参阅[使用标签来组织您的 Azure 资源](../resource-group-using-tags.md)。
  


 

测试
