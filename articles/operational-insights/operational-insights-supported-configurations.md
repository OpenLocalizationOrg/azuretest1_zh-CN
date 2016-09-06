---
ms.openlocfilehash: ef0aeca49985b52538dcbfd9e53f40a8c1df160a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="支持的配置操作的见解"
   description="了解有关配置所需的运营洞察力"
   services="operational-insights"
   documentationCenter=""
   authors="bandersmsft"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operational-insights"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/02/2015"
   ms.author="banders" />

# 支持的配置操作的见解

[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

您需要使用运营的见解？ 检查出下面的信息，准备运营的见解。


## System Center 2012 操作管理器的配置

操作的见解可以用作操作管理器在 System Center 2012 R2 或 System Center 2012 SP1 R2 中的附加服务。 这使您可以在操作管理器使用操作控制台查看运营洞察力警报和配置信息。 作为运营经理在附加的服务使用运营的见解时，代理通讯直接与管理服务器，又与运营经验服务进行通信。

使用附加的服务操作的见解具有以下先决条件︰


- System Center 2012 SP1 运营经理和运营见解之间的集成需要更新的管理包包含在[运营经理的运营洞察力连接器](https://www.microsoft.com/en-us/download/details.aspx?id=38199)。 您可以从下载并安装管理包[运营经理的运营洞察力连接器](https://www.microsoft.com/en-us/download/details.aspx?id=38199)。

- System Center 2012 SP1︰ 操作管理器更新汇总 6，虽然更新汇总 7 是首选。 此更新需要为作为一个附加的服务方案运营的见解应用于管理服务器、 代理和操作控制台。

- System Center 2012 R2︰ 操作管理器更新汇总 2，尽管更新汇总 3 是首选。 此更新需要为作为一个附加的服务方案运营的见解应用于管理服务器、 代理和操作控制台。

- 若要查看容量管理数据，您必须启用运营经理连接使用 Virtual Machine Manager (VMM)。 有关将系统连接的其他信息，请参阅[如何连接 VMM 与运营经理](https://technet.microsoft.com/en-us/library/hh882396.aspx)。

安装和配置说明，请参阅[查看操作的见解警报](http://go.microsoft.com/fwlink/?LinkID=293793)。

如果您想要查看操作的见解通知有关 SharePoint Server 2010、 Lync Server 2013，Lync Server 2010 中或 System Center 2012 SP1 Virtual Machine Manager，您需要配置这些工作负载运行方式帐户。 有关如何设置运行方式帐户的信息，请参阅[与运营经验的运营经理注意事项](operational-insights-operations-manager.md)。


### 操作管理器操作系统

操作管理器代理支持各种不同的计算机上。 请参阅[系统要求︰ System Center 2012 R2 运营经理](https://technet.microsoft.com/library/dn249696.aspx)有关代理支持的详细信息。

### 操作管理器所需的软件

若要查看容量管理数据，您必须启用与 VMM 操作管理器连接。 有关将系统连接的其他信息，请参阅[如何连接 VMM 与运营经理](https://technet.microsoft.com/en-us/library/hh882396.aspx)。

## 代理直接连接到操作的见解

使用直接连接到服务代理是 Microsoft 监视代理。 在[Microsoft 下载中心获取](https://www.microsoft.com/en-us/download/details.aspx?id=40316&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)页上列出了其系统的要求。

## 浏览器

您可以使用任何以下浏览器之一与运营的见解︰

- Windows Internet Explorer 11、 Internet Explorer 10、 Internet Explorer 9、 Internet Explorer 8 或 Internet Explorer 7

- Mozilla Firefox 3.5 版或更高版本

无论您使用的浏览器，您还必须安装 Microsoft Silverlight 4。

## 您可以分析技术

操作的见解配置评估分析以下工作负荷︰

- Windows Server 2012 和 Microsoft Hyper-V 服务器 2012

- Windows Server 2008 和 Windows Server 2008 R2，包括︰
    - 活动目录
    - Hyper-V 主机
    - 常规的操作系统

- SQL Server 2012，SQL Server 2008 R2 中，SQL Server 2008
    - SQL Server 数据库引擎

- Microsoft SharePoint 2010

- Microsoft Exchange Server 2010年和 Microsoft Exchange Server 2013

- Microsoft Lync Server 2013 和 Lync Server 2010

- System Center 2012 SP1 Virtual Machine Manager

为 SQL Server 以下 32 位和 64 位版本支持如下分析︰

- SQL Server 2008年和 2008 R2 企业

- SQL Server 2008年和 2008 R2 标准

- SQL Server 2008年和 2008 R2 工作组

- SQL Server 2008年和 2008 R2 Web

- SQL Server 2008年和 2008 R2 速成版

此外，在 WOW64 实现运行时支持的 32 位版本的 SQL Server。
