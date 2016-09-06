---
ms.openlocfilehash: bb01d8be1c5bf31ed0894cc7ce860c38bb48acc6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="云服务部署 (Node.js) 阶段，以 |Microsoft Azure" 
    description="了解如何部署到临时环境，Azure 应用程序，然后将部署到生产环境中使用虚拟 IP (VIP) 交换。" 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="MikeWasson" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="02/25/2015" 
    ms.author="mwasson"/>



# 临时在 Azure 中的应用

打包的应用程序可以部署到临时环境进行测试之前将其移动到生产环境中的应用程序可以在 Internet 上访问的 Azure 中。 暂存环境与生产环境中，完全只是分阶段经过模糊处理的 URL 生成的 Azure 应用程序只能访问。 验证应用程序正常工作后，它可以部署到生产环境中执行虚拟 IP (VIP) 交换。

> [AZURE.NOTE] 本文中的步骤仅适用于节点作为 Azure 云服务承载的应用程序。

此任务包括以下步骤︰

-   [步骤 1︰ 阶段化安装应用程序]
-   [步骤 2: 将应用部署到生产通过交换 Vip]

## 步骤 1︰ 阶段化安装应用程序

此任务包括如何通过使用**Windows Azure PowerShell**阶段化安装应用程序。

1.  当发布服务，只需将传递**-插槽**参数与**发布 AzureServiceProject** cmdlet。

    **将发布 AzureServiceProject-插槽临时**

2.  登录到[Azure 管理门户]并选择**云服务**。 创建云服务和**临时**列的状态已更新到**运行**之后，请单击服务名称。

    ![门户显示正在运行的服务][cloud-service]

3.  选择**仪表板**，然后选择**转移**。

    ![云服务仪表板][cloud-service-dashboard]

4. 请注意右边的**网站 URL**项中的值。 DNS 名称是 Azure 生成经过模糊处理的内部 ID。

    ![站点的 url][cloud-service-staging-url]

现在，您可以验证的应用程序是否正常工作在临时环境中使用临时的网站 URL。

对于升级方案，分阶段应用程序处于升级的版的一个已部署到生产环境，您可以[升级应用程序在生产环境中通过交换 Vip][第 2 步︰ 应用程序部署到生产通过交换 Vip]。

## 步骤 2︰ 升级通过交换 Vip 生产中的应用

验证在暂存环境中应用程序的升级的版本后，可以快速进行它可用在生产中通过交换虚拟 IPs (VIPs) 的临时和生产环境。

> [AZURE.NOTE] 此步骤假定您已经部署到生产环境的应用程序并转移升级的应用程序版本。

1.  登录到[Azure 管理门户]，单击**云服务**，然后选择服务名称。

2.  从**仪表板**中，选择**转移**，，然后单击页面底部**换**。 这将打开对话框中 VIP 交换。

    ![vip 交换对话框][vip-swap-dialog]

3.  查看信息，，然后单击**确定**。 两种部署开始更新临时部署切换到生产以及生产部署到临时切换。

您成功地分阶段的部署，通过换用在临时部署的 Vip 升级生产部署。

## 其他资源

- [如何通过换用 Azure 中的 Vip 将服务升级部署到生产]
- [管理部署在 Azure 中的概述]

  [步骤 1︰ 阶段化安装应用程序]: #step1
  [步骤 2: 将应用部署到生产通过交换 Vip]: #step2
  [Azure 的管理门户]: http://manage.windowsazure.com
[云服务]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[云服务仪表板]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
  [云服务临时 url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
  [vip 交换对话框]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
  [如何通过换用 Azure 中的 Vip 将服务升级部署到生产]: http://msdn.microsoft.com/library/windowsazure/ee517253.aspx
  [管理部署在 Azure 中的概述]: http://msdn.microsoft.com/library/windowsazure/hh386336.aspx
 

测试
