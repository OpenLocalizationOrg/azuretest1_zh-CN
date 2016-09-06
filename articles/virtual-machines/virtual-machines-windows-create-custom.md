---
ms.openlocfilehash: d62a8e2501cbc67c073dc69eac870b5a43bc265d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="创建自定义运行 Windows Azure 中的虚拟机"
    description="了解如何创建自定义运行 Windows Azure 中的虚拟机。"
    services="virtual-machines"
    documentationCenter=""
    authors="KBDAzure"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>


<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2015"
    ms.author="kathydav"/>

#创建自定义运行 Windows Azure 中的虚拟机

*自定义*虚拟机只是意味着使用**剪辑库中的**选项，因为它会为您提供更多的配置选项，比**快速创建**选项创建一个虚拟机。 这些选项包括︰

- 连接到虚拟网络的虚拟机。
- 安装虚拟机代理和扩展，如反恶意软件。
- 将虚拟机添加到现有的云服务。
- 将虚拟机添加到现有的存储帐户。
- 将虚拟机添加到的可用性设置。

> [AZURE.IMPORTANT] 如果您想要使用的虚拟网络，因此可以通过主机名或跨内部连接设置直接连接到该虚拟机，请确保您在创建虚拟机时指定虚拟网络。 可以将虚拟机配置为仅当创建虚拟机加入虚拟网络。 虚拟网络的详细信息，请参阅[Azure 虚拟网络概述](virtual-networks-overview.md)。

##若要创建虚拟机

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]
