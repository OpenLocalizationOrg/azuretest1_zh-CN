---
ms.openlocfilehash: 6a8a87595cf5af68b19253649b72ce485f938015
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties
    pageTitle="验证要使用 Azure RemoteApp Azure VNET"
    description="了解如何确保 Azure VNET 已准备好使用 Azure 远程应用程序"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2015"
    ms.author="elizapo" />



# 验证要使用 Azure RemoteApp Azure VNET

Azure VNET 使用 Azure 远程应用程序之前，您可能需要验证 VNET。 这有助于防止连接问题。

若要验证 Azure VNET，执行以下操作︰

1. 创建您想要使用 Azure RemoteApp Azure VNET 的子网内 Azure 的虚拟机。

2. 连接到该虚拟机通过管理门户中的**连接**选项。
3. 将虚拟机加入到您想要使用 Azure RemoteApp 的相同域。

如果此操作成功，就可以使用 RemoteApp Azure VNET。

端到端混合收集工作流的详细信息，请参阅下列文章︰

- [创建混合集合](remoteapp-create-hybrid-deployment.md)
- [将 Azure 远程应用程序集部署到 Azure 虚拟网络 （与 ExpressRoute 支持）](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)
 