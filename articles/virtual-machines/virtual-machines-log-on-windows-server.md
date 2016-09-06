---
ms.openlocfilehash: 8078f1ea38e8e64b9f4a0e131b22b31c89d3793b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="登录到运行 Windows 服务器的虚拟机"
    description="了解如何使用 Azure 门户登录到运行 Windows 服务器的虚拟机。"
    services="virtual-machines"
    documentationCenter=""
    authors="KBDAzure"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/12/2015"
    ms.author="kathydav"/>


# 如何登录到运行 Windows 服务器的虚拟机#

您将使用 Azure 门户中的**连接**按钮以启动远程桌面会话。 （Linux 虚拟机，请参阅[如何登录到运行 Linux 的虚拟机](virtual-machines-linux-how-to-log-on.md)。

## 如何登录

[AZURE.VIDEO logging-on-to-vm-running-windows-server-on-azure]

[AZURE.INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## 故障排除提示

下面是尝试快速的几个措施︰

远程桌面连接的问题，请尝试重置从门户配置。 从虚拟机面板下**快速粗略地看**，单击**重置远程配置**。

您的密码的问题，请尝试门户中重置。 虚拟机面板中，从下**快速粗略地看**，单击**重设密码**。

如果这些不起作用，您需要执行更多疑难解答。 有关说明，请参阅[解决远程桌面连接到基于 Windows Azure 虚拟机](virtual-machines-troubleshoot-remote-desktop-connections.md)。
