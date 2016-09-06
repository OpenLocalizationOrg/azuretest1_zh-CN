---
ms.openlocfilehash: 9a775fb152731f2617fc58b73a4fa361312c0de1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="从系统中心操作管理器连接到操作的见解"
   description="了解如何连接到操作器的操作见解。"
   services="operational-insights"
   documentationCenter=""
   authors="lauracr"
   manager="jwhit"
   editor=""/>

<tags
   ms.service="operational-insights"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/06/2015"
   ms.author="lauracr"/>

# 从系统中心操作管理器连接到操作的见解


[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

您可以连接到现有的系统中心操作管理器环境的运营经验。 这将允许您使用现有的运营经理代理，以便进行数据采集。 关于使用的运营洞察力的操作管理器的其他信息，请参阅[与运营经验的运营经理注意事项](operational-insights-operations-manager.md)。

如果操作管理器用于监视任何以下工作负载，您需要为其设置操作管理器运行方式帐户。 有关设置帐户的详细信息，请参阅[与运营经验的运营经理注意事项](operational-insights-operations-manager.md)。

- SQL 评估
- Virtual Machine Manager
- Lync 服务器
- SharePoint 

 >[AZURE.NOTE] 操作管理器 2012 SP1 UR6 和操作管理器 2012 R2 UR2 提供了运营经验的支持。 系统中心操作管理器 2012 SP1 UR7 和系统中心操作管理器 2012 R2 UR3 中添加了代理支持。

## 连接操作的见解操作管理器并添加代理

1. 在操作管理器控制台中，单击**管理**。

2. 展开**操作见解**节点并单击**操作的见解连接**。

3. 单击**注册运营的见解到**链接，按照屏幕上的说明。

4. 完成登记向导后，请单击**添加计算机/组**。
![操作管理器添加计算机/组](./media/operational-insights-connect-scom/om01.png)
5. 在**计算机搜索**对话框中您可以搜索计算机或组通过操作管理器监视。 选择计算机或组到板载到运营的见解，单击**添加**，然后单击**确定**。
![操作管理器添加计算机](./media/operational-insights-connect-scom/om02.png)
## 下一步行动

[配置代理服务器和防火墙设置 （可选）](operational-insights-proxy-firewall.md)
