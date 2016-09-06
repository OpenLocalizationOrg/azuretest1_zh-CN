---
ms.openlocfilehash: f2a2a480136fefbe10452b634fea8b7004efd107
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="了解如何运营洞察力的安全性和审核数据"
   description="了解有关如何使用安全和审核解决方案来全面了解到您组织的 IT 安全状况与需要您注意的显著问题的内置搜索查询"
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
   ms.date="07/21/2015"
   ms.author="banders" />

# 了解如何运营洞察力的安全性和审核数据

[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

见解的运营安全和审核解决方案提供的全面视图到您的组织的 IT 安全状况与需要您注意的显著问题的内置的搜索查询。

在本文中，您将能够︰

- 进行简单调查可疑的可执行文件
- 了解取证分析的最佳做法
- 了解最佳做法安全破坏模式调查
- 了解审计方案的最佳做法

## 进行简单调查可疑的可执行文件

1. 登录到运行的见解。
2. 在**概述**页查看**安全和审核**的图块中显示的信息，然后单击它。
 ![概述页的图像](./media/operational-insights-security-audit/sec-audit-dash02.png)
3. 在**安全和审核**页上查看**显而易见的问题**刀片式服务器中的信息。 示例图像中，您将看到今天从昨天的 2 6 显而易见的问题。 在此示例中，没有 1 可疑的可执行文件。 **值得注意的问题**刀片式服务器中，请单击**可疑的可执行文件**。
 ![安全和审核页面的图像](./media/operational-insights-security-audit/sec-audit-dash03.png)
4. 搜索结果显示的查询和可疑的可执行文件，则您单击的结果。 在示例中，1 个结果并且显示其文件哈希。 请单击**FILEHASH** id。
 ![搜索结果 filehash 的图像](./media/operational-insights-security-audit/sec-audit-search01.png)
5. 搜索结果显示的可执行文件，包括文件路径和进程有关的其他信息。 单击**进程&lt;文件名&gt;**。 在示例中，这是 HEXEDIT。EXE。
![搜索结果过程的图像](./media/operational-insights-security-audit/sec-audit-search02.png)
6. 搜索将引号中的过程的名称追加到查询中。 "**HEXEDIT。EXE"**，在此示例中。
 ![搜索查询的图像](./media/operational-insights-security-audit/sec-audit-search03.png)
7. 搜索查询框中，删除所有内容，但该进程名称和引号，然后单击搜索图标。
 ![详细的搜索信息的图像](./media/operational-insights-security-audit/sec-audit-search04.png)
8. 搜索结果显示详细的信息的过程，包括在进程运行时，用户帐户下运行的进程的计算机的日期和事件的创建过程的时间。
9. 使用您所查找的信息，您可以根据需要采取纠正措施。 例如，如果您确定可执行文件是恶意软件，然后您需要采取措施来删除所有计算机系统的影响。 可执行文件被移除，运营洞察力接收有关您计算机系统的更新的日志和审核事件以后，显而易见的问题刀片式服务器上的值将更改以下一天。

[AZURE.INCLUDE [operational-insights-export](../../includes/operational-insights-export.md)]

## 取证分析的最佳做法

**寻找证据**

当您进行鉴证分析使用的安全性和审核解决方案时，您要寻找潜在的恶意用户留下的证据。 无论用户在做什么在他们的 IT 环境中，很多参与它们的活动生成安全项目。 您应注意，日志通常攻击者清除，通常是在隐藏其路径时采取的第一步。

但是，是否用户要访问他们自己的计算机或远程访问它们，它们的使用有关的证据存储在事件日志中。 运营的见解收集这些项目*一旦他们出现*之前的任何人都可篡改，, 并使您可以通过在多台计算机关联的数据执行不同类型的分析。

**了解如何配置并收集审核事件**

若要获得最大的安全和审核解决方案，应配置由您的 Windows 环境最适合您的需要，收集的审核事件的级别使用下列资源。

- [如何配置安全策略设置](https://technet.microsoft.com/library/dn135243(v=ws.10).aspx)

- [高级的审计策略配置](https://technet.microsoft.com/library/jj852202(v=ws.10).aspx)

- [审核策略建议](https://technet.microsoft.com/library/dn487457.aspx)

**启用 AppLocker**

您还应该启用 AppLocker 事件，获取丰富信息在您的 IT 环境中发生的过程执行。 有关详细信息，请参阅[配置审核仅 AppLocker 策略](https://technet.microsoft.com/library/hh994622.aspx)

**配置 Windows 事件的审核级别**

Windows 计算环境使您能够配置捕获级别的与安全相关的记录。 例如，可以配置您的环境，以便每当有人访问文件、 读取文件或打开一个文件，生成一个事件。 根据您的需要，想要从中收集的详细程度将有所不同。 但是，启用每个选项带有某种形式的成本因为您需要将您收集的所有信息都存储。 为此原因，在很多组织的 IT 环境中，人们决定不启用对象读取或写入对象的数据收集。 每次有人访问任何文件，成千上万的事件可能产生的其中一些无用的噪音。 您决定集合级别取决于您的最佳判断。

>[AZURE.NOTE] 如果您使用的直接代理，并且组织中有一个代理服务器，您应将其配置为允许代理访问操作的见解。 有关详细信息，请参阅[配置代理服务器和防火墙设置](operational-insights-proxy-firewall.md)。

## 安全性的最佳操作破坏模式调查

**调查不正常的活动模式**

安全漏洞通常源自合法凭据，并要求恶意用户尝试通过攻击获取提升的权限。 安全和审核解决方案与入侵检测不关注 — — 相反，但有助于您进行研究和发现模式的异常活动突出的问题。 例如，您应调查以下异常活动以及在**显而易见的问题**出现任何其他。

- 在正常情况下不能使用它的用户的计算机上不寻常的登录

- 从典型的用户或计算机的不寻常的网络枚举

- 具有管理权限创建新用户帐户

- 日志记录或安全策略的更改

- 可疑的可执行文件

## 审计方案的最佳做法

您的组织可能具有计算机网络法规遵从性策略和管理法规，您必须遵守，需要大量的审核记录。 例如，如果您的组织可能需要时间证明的任何给定点的记录财务公司，哪个用户执行基于网络的特定操作。 您可能还需要生成报告详细说明活动的特定用户或选定的服务器上的请求，并返回在许多个月的时间内--有时甚至后年。

您的计算机是否位于本地或在云环境中，您可以使用安全和审核解决方案可在整个 IT 环境，收集审核数据。 所有审核数据存储、 索引，和每个订阅保留。 使用高级订阅到运营的见解，不限制的数量的数据存储为一年。 然后可以查看审核数据、 执行搜索，并跨不同的数据类型和计算机以任何时间间隔获取综合结果，由于收集数据关联。

**使用组策略来收集审核数据**

您想要收集并发送到操作的见解的所有审核数据完全是使用组策略管理的。 使用它来作为一部分的组策略对象 (GPO)，链接到站点、 域和组织单位如 Active Directory 容器，其定义的安全配置，它们可以帮助您管理安全设置。 策略数据记录和稍后发送到操作的见解服务。

**使用 AppLocker 来收集审核数据**

除了本地策略设置，如果您使用 AppLocker 来收集审核数据，运营洞察力将收集的数据，然后您可以查看它。
