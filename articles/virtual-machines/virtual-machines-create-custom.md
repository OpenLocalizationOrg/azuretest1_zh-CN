---
ms.openlocfilehash: 994246dd3f80b152570df07557b66c371e66d69f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Azure 中创建自定义的虚拟机"
    description="了解如何在 Azure 中创建自定义的虚拟机。"
    services="virtual-machines"
    documentationCenter=""
    authors="KBDAzure"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2015"
    ms.author="kathydav"/>

#如何创建自定义的虚拟机

*自定义*虚拟机只是意味着使用**剪辑库中的**选项，因为它会为您提供更多的配置选项，比**快速创建**选项创建一个虚拟机。 这些选项包括︰

- 连接到虚拟网络的虚拟机。
- 安装的 Azure 虚拟机代理和 Azure 虚拟机的扩展，如反恶意软件。
- 将虚拟机添加到现有的云服务。
- 将虚拟机添加到现有的存储帐户。
- 将虚拟机添加到的可用性设置。

> [AZURE.IMPORTANT] 如果您希望您的虚拟机可以使用虚拟网络，这样可以直接通过主机名连接到它，也可以设置跨内部连接，请确保您在创建虚拟机时指定虚拟网络。 可以将虚拟机配置为仅当创建虚拟机加入虚拟网络。 虚拟网络的详细信息，请参阅[Azure 虚拟网络概述](virtual-networks-overview.md)。

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]
