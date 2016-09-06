---
ms.openlocfilehash: 743ddf736667df3374ac3f101c467c7d0db9d285
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="安全运营的见解"
    description="了解有关如何操作的见解可以保护您的隐私并保护您的数据。"
    services="operational-insights"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="operational-insights"
    ms.workload="dev-center-name"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2015"
    ms.author="banders"/>

# 理解安全运营

[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

Microsoft 致力于保护您的隐私并保护您的数据，同时提供软件和服务帮助您管理您的组织的 IT 基础结构。 我们已经认识到，当委托给其他用户数据，这种信任需要严格的安全。 Microsoft 遵守严格的法规遵从性和安全准则 — — 从编码到操作服务。

加强安全和保护的数据是在 Microsoft 的头等大事。 请有什么问题、 建议或关于任何以下信息，其中包括我们在[Azure 支持选项](http://azure.microsoft.com/support/options/)的安全策略的问题，与我们联系。

本文介绍了数据收集、 处理和安全操作管理套件 (OMS) 中。 它以前被调用过 Microsoft Azure 运营的见解。 您可以使用任一代理直接连接到 web 服务，也可以使用系统中心操作管理器收集了 OMS 服务运营数据。 所收集的数据通过互联网发送到承载 Microsoft Azure 中的 OMS 服务。

OMS 服务管理您的数据安全地使用以下方法︰

**数据隔离︰**逻辑上独立客户数据保存在整个 OMS 服务的每个组件。 所有数据是每个组织的都标记。 此标记的数据生命周期中，仍然存在，且每一层的服务实施。

每个客户都有专用的 Azure blob 存储长期数据的。 使用唯一的每个客户键，每 90 天更改加密 blob。

**数据保留︰**为部分解决方案 （以前称为智能包），如容量管理、 聚合度量值存储在 SQL 数据库由 Microsoft Azure 托管。 该数据存储为 390 天。 索引的日志搜索数据存储和保留根据的定价计划。 详细信息查看[定价页](http://azure.microsoft.com/pricing/details/operational-insights/)

**的物理安全︰**OMS 服务可以通过 Microsoft 专业人员，所有活动都记录和可审核。 OMS 服务在 Azure 完全运行，并符合 Azure 的通用工程标准。 [Microsoft 安全概述 Azure](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf)的第 18 页，可以查看有关 Azure 资产的物理安全性的详细信息。

**的法规遵从性和认证︰**与 Microsoft 法律和法规遵从性团队和其他业界伙伴，获得认证，其中包括 ISO 的各种积极工作 OMS 软件开发和服务团队。

我们当前符合以下安全标准︰

- 通用工程标准的 Windows
- Microsoft 可信赖计算认证


## 数据流量安全
下图显示从您的公司的信息流动，如何保护原样移动到最终由您示 OMS 的 OMS 服务。 有关每个步骤的详细信息如下图。

![图像的 OMS 的数据收集和安全](./media/operational-insights-security/security.png)

### 1.注册 OMS 和收集数据

为您的组织将数据发送到了 OMS 服务必须直接连接到 web 服务时配置 Microsoft 监视代理或运营经理在操作控制台中使用配置向导。 （这可能是其他单个用户或一组人） 的用户必须创建一个或多个 OMS 帐户并注册每个直接连接的代理或其运营经理环境使用下列帐户之一︰


- [组织 ID](../sign-up-organization.md)

- [Microsoft 帐户-Outlook，Office Live MSN](../sign-up-organization.md)

其中收集、 聚合、 分析和数据显示未 OMS 帐户。 OMS 帐户主要用于作为一种手段对数据进行分区，并且每个 OMS 帐户是唯一。 例如，您可能想要使用一个 OMS 帐户管理生产数据并使用另一个帐户管理测试数据。 帐户还可以帮助管理员控制用户访问数据。 OMS 的每个帐户可以有多个与其关联的用户帐户和每个用户帐户可以有多个 OMS 帐户。

配置向导完成后，每个运营经理管理组建立与 OMS 服务的连接。 然后使用添加计算机向导可供选择的管理组中的哪些计算机可以将数据发送到该服务。

这两种类型的代理的 OMS 收集数据。 收集的数据的类型将取决于使用的解决方案的类型。 一个解决方案是一系列预定义的视图、 日志搜索查询、 数据集合规则，和处理逻辑。 只有 OMS 管理员可以使用 OMS 要导入的解决方案。 导入解决方案后，将移动到操作管理器管理服务器 （如果使用），再到您已选择的操作管理器代理。 此后，代理收集的数据。

下表列出了 OMS 和他们收集的数据的类型中可用的解决方案。


|**解决方案**|**数据类型**|
|---|---|
|配置评估|配置数据、 元数据和状态数据|
|容量规划|性能数据、 元数据和状态数据|
|反恶意软件|配置数据、 元数据和状态数据|
|系统更新评估|元数据和状态数据|
|日志管理|用户定义的事件日志|
|更改跟踪|软件清单和 Windows 服务元数据|
|SQL 和 Active Directory 评估|WMI 数据、 注册表数据、 性能数据和 SQL Server 动态管理查看结果|



下表显示的数据类型的示例︰

|**数据类型**|**字段**|
|---|---|
|Alert|警报名称、 警报描述、 BaseManagedEntityId、 问题 ID、 IsMonitorAlert、 规则 Id、 ResolutionState、 优先级、 严重程度、 类别、 所有者、 ResolvedBy、 TimeRaised、 TimeAdded、 上次更改时间、 LastModifiedBy、 LastModifiedExceptRepeatCount、 TimeResolved、 TimeResolutionStateLastModified、 TimeResolutionStateLastModifiedInDB、 RepeatCount|
|配置|客户 Id、 AgentID、 EntityID、 ManagedTypeID、 ManagedTypePropertyID、 CurrentValue，ChangeDate|
|事件|EventId、 EventOriginalID、 BaseManagedEntityInternalId、 规则 Id、 PublisherId、 PublisherName、 FullNumber、 数目、 类别、 ChannelLevel、 LoggingComputer、 EventData、 EventParameters、 TimeGenerated、 TimeAdded**注︰** *当您登录到 Windows 事件日志中使用自定义字段事件时，OMS 收集它们。*|
|元数据|BaseManagedEntityId、 ObjectStatus、 OrganizationalUnit、 ActiveDirectoryObjectSid、 PhysicalProcessors、 NetworkName、 ip 地址、 ForestDNSName、 NetbiosComputerName、 VirtualMachineName、 LastInventoryDate、 HostServerNameIsVirtualMachine、 IP 地址、 NetbiosDomainName、 LogicalProcessors、 DNSName、 显示名称、 DomainDnsName、 ActiveDirectorySite、 PrincipalName、 OffsetInMinuteFromGreenwichTime|
|表现|对象名称、 取代、 PerfmonInstanceName、 PerformanceDataId、 PerformanceSourceInternalID、 SampleValue、 TimeSampled、 TimeAdded|
|省/市/自治区|StateChangeEventId、 状态标识、 NewHealthState、 OldHealthState、 上下文、 TimeGenerated、 TimeAdded、 StateId2、 BaseManagedEntityId、 MonitorId、 HealthState、 上次更改时间、 LastGreenAlertGenerated、 DatabaseTimeModified|


### 2.从代理发送的数据

通过直接连接到 web 服务的代理，使用密钥注册并使用端口 443 代理和 OMS 服务之间建立安全连接。

与运营经理，OMS 服务注册一个帐户，并使用端口 443 OMS 服务运营经理管理服务器之间建立安全的 HTTPS 连接。 如果操作管理器不能进行通信的服务由于任何原因，所收集的数据存储在临时缓存和管理服务器尝试重新发送数据每 8 分钟为 2 个小时。 收集的数据被压缩并发送到了 OMS 服务，绕过内部的数据库，因此它不会给他们添加任何负载。 所收集的数据被发送后，它将从缓存中移除。

### 3.了 OMS 服务接收并处理数据

OMS 服务确保传入数据是来自受信任源，通过验证证书和数据完整性。 未经处理的原始数据被保存为[Microsoft Azure 存储](http://azure.microsoft.com/documentation/services/storage/)中的 blob。 OMS 的每个用户具有专用的 Azure blob，仅该用户可以访问。 存储的数据的类型将取决于已导入并用于收集数据的解决方案的类型。

OMS 服务处理原始数据，并已处理的聚合的数据存储在 SQL 数据库中。 在 SQL 数据库身份验证依赖于 OMS 服务和 SQL 数据库之间的通信。

### 4.使用 OMS 访问数据

您可以使用以前设置的帐户登录到 OMS。 通过安全的 HTTPS 通道发送 OMS OMS 服务之间的所有通信。
