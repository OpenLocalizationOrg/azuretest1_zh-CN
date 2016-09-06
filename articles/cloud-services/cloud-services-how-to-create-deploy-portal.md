---
ms.openlocfilehash: 60cfc4e7bd9bbb7eb7632b7d7223478c5d07e4dc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何创建和部署云服务 |Microsoft Azure"
    description="了解如何创建和部署云服务使用 Azure 中的快速创建方法。"
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2015"
    ms.author="adegeo"/>




# 如何创建和部署云服务

> [AZURE.SELECTOR]
- [Azure 门户](cloud-services-how-to-create-deploy.md)
- [Azure 预览门户](cloud-services-how-to-create-deploy-portal.md)

Azure 的门户提供了两种方法来创建和部署云服务︰*快速创建*和*自定义创建*。

本文介绍如何使用快速创建方法来创建一个新的云服务，然后使用**上载**来上载和部署中 Azure 的云服务包。 使用此方法时，Azure 门户系统使得可用方便链接，当您完成所有要求。 如果您已经准备好部署云服务在创建它时，可以不要同时使用自定义创建的同一时间。

> [AZURE.NOTE] 如果您计划发布云服务从 Visual Studio 联机 (VSO)，使用快速创建，并再从 Azure 快速入门或仪表板设置 VSO 发布。 有关详细信息，请参阅[连续传递到 Azure 通过使用 Visual Studio 在线][TFSTutorialForCloudService]，请参阅帮助**快速启动**页。

## 概念
三个组件需要部署为在 Azure 的云服务应用程序︰

- **服务定义**  
  云服务定义文件 (.csdef) 定义的服务模型，包括角色的数目。

- **服务配置**  
  云服务配置文件 (.cscfg) 提供了配置设置的云服务和个人的角色，包括角色实例数。

- **服务包**  
  服务包 (.cspkg) 中包含的应用程序代码和配置的服务定义文件。

您可以了解更多有关这些以及如何创建包[下面](cloud-services-model-and-package.md)。

## 准备您的应用程序
您可以部署云服务之前，必须从应用程序代码和云服务配置文件 (.cscfg) 创建的云服务包 (.cspkg)。 Azure SDK 提供了准备这些所需的部署文件的工具。 从[Azure 下载](http://azure.microsoft.com/downloads/)页中的在语言中喜欢用来开发应用程序代码中，可以安装 SDK。

三种云服务功能要求特殊配置导出服务包之前︰

- 如果您要部署安全套接字层 (SSL) 用于数据加密的云服务，配置 SSL 所需的应用程序。 有关详细信息，请参阅[如何配置 HTTPS 终结点上的 SSL 证书](http://msdn.microsoft.com/library/azure/ff795779.aspx)。

- 如果您想要配置的角色实例的远程桌面连接，远程桌面配置角色。 有关准备用于远程访问的服务定义文件的详细信息，请参阅[设置远程桌面连接在 Azure 中的角色](http://msdn.microsoft.com/library/hh124107.aspx)。

- 如果您想要配置详细的云服务监视，云服务启用 Azure 诊断。 *最少的监控*（默认的监视级别） 使用性能计数器收集从主机操作系统的角色实例 （虚拟机）。 *详细监视*收集基于角色实例，以便更仔细地分析应用程序的处理过程中出现的问题中的性能数据的其他指标。 若要了解如何启用 Azure 诊断程序，请参阅[启用诊断 Azure 中](cloud-services-dotnet-diagnostics.md)。

要创建部署 web 角色或辅助角色的云服务，必须创建的服务包。 有关与此包相关的文件的详细信息，请参阅[设置了云服务的 Azure](http://msdn.microsoft.com/library/hh124108.aspx)。 若要创建此包文件，请参阅[包 Azure 应用程序](http://msdn.microsoft.com/library/hh403979.aspx)。 如果使用的 Visual Studio 开发应用程序，请参阅[发布云服务使用 Azure 工具](http://msdn.microsoft.com/library/ff683672.aspx)。

## 在开始之前

- 如果您还没有安装 Azure SDK，单击打开[Azure 下载页](http://azure.microsoft.com/downloads/)上，**安装 Azure SDK** ，然后下载 SDK 以查找您想开发代码的语言。 （您必须有机会这样做以后。）

- 如果任何角色实例需要证书，请创建证书。 云服务的专用密钥需要一个.pfx 文件。 创建和部署云服务时，您可以上载证书到 Azure。 有关证书的信息，请参阅[管理证书](http://msdn.microsoft.com/library/gg981929.aspx)。

- 如果您打算部署到关系组的云服务，创建关联组。 可以使用关系组将云服务和其他 Azure 服务部署到一个区域中的同一位置。 在 Azure 的门户，在**关系组**页上的**网络**区域中，您可以创建关联组。 有关详细信息，请参阅[创建管理门户中关联组](http://msdn.microsoft.com/library/jj156209.aspx)。


## 步骤 3︰ 创建一个云服务并上载该部署包

1. 登录到 [Azure 预览门户] []。
2. 单击**新建 > 计算**，然后向下滚动并单击**云服务**。

    ![发布您的云服务](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)

3. 在新的**云服务**刀片，输入**DNS 名称**的值。
4. 创建新的**资源组**，或选择一个现有。
5. 选择一个**位置**。
6. 选择**软件包**，并**上载包**刀片上, 填写必填字段。  

     如果您的角色的任何包含的单个实例，确保**部署，即使一个或多个角色中包含的单个实例**处于选中状态。

7. 请确保已选中**开始部署**。
8. 单击**确定**。

    ![发布您的云服务](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## 将上载证书

如果您部署程序包[配置为使用证书](cloud-services-configure-ssl-certificate-portal.md#modify)，您可以立即上载证书。

9. 选择**证书**和**添加证书**刀片式服务器，选择 SSL 证书.pfx 文件，然后提供该证书的**密码**
10. 单击**附加证书**，然后单击**确定****添加证书**刀片式服务器。
11. **云服务**刀片式服务器，请单击**创建**。 如果部署达到**就绪**状态，就可以继续下面的步骤。

    ![发布您的云服务](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)


## 验证已成功完成部署

1. 单击云服务实例。

    状态应显示该服务正在**运行**。

2. 下**精要**要在 web 浏览器中打开您的云服务的**网站 URL** 。

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)


[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796&clcid=0x409

测试
