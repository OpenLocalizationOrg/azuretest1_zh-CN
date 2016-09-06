---
ms.openlocfilehash: 26215008641022afd0ba4130ed408025ca8ae988
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="创建自定义运行 Linux 在 Azure 中的虚拟机"
    description="了解如何创建自定义运行 Linux 在 Azure 中的虚拟机。"
    services="virtual-machines"
    documentationCenter=""
    authors="dsk-2015"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2015"
    ms.author="dkshir"/>

# 如何创建自定义运行 Linux 在 Azure 中的虚拟机

本主题介绍如何使用 Azure CLI 使用传统部署模型创建*自定义*的虚拟机。 我们将使用 Azure 的 Linux 映像中可用的**图像**。 以下配置选项以及其他 Azure CLI 命令为︰

- 将虚拟机连接到虚拟网络
- 将虚拟机添加到现有的云服务
- 将虚拟机添加到现有的存储帐户
- 将虚拟机添加到可用性组或位置

> [AZURE.IMPORTANT] 如果您想要使用的虚拟网络，因此可以通过主机名或跨内部连接设置直接连接到该虚拟机，请确保您在创建虚拟机时指定虚拟网络。 可以将虚拟机配置为仅当创建虚拟机加入虚拟网络。 虚拟网络的详细信息，请参阅[Azure 虚拟网络概述](http://go.microsoft.com/fwlink/p/?LinkID=294063)。

<p/>
[AZURE.INCLUDE [service-management-pointer-to-resource-manager](../../includes/service-management-pointer-to-resource-manager.md)]

- [创建一个运行 Linux 虚拟机](virtual-machines-linux-tutorial.md)


## 如何创建 Linux 虚拟机使用传统部署模型

[AZURE.INCLUDE [virtual-machines-create-LinuxVM](../../includes/virtual-machines-create-linuxvm.md)]
