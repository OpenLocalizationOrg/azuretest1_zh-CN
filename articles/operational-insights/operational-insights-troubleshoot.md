---
ms.openlocfilehash: 3d29e38e740667967a91f376dc1e0e5bfb89be21
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="解决运营洞察力的问题"
   description="了解有关运营的经验与问题进行故障排除"
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

#解决运营洞察力的问题

[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

可以使用以下各节中的信息来帮助您解决问题。 如果您遇到的问题不在本文中，您可以尝试[运营洞察力团队博客](http://blogs.technet.com/b/momteam/archive/2014/05/29/advisor-error-3000-unable-to-register-to-the-advisor-service-amp-onboarding-troubleshooting-steps.aspx)。

## 诊断连接问题的运营洞察力

由于 Microsoft Azure 运营经验依赖于云来回移动的数据，可以削弱的连接问题。 使用以下信息来理解并解决连接问题。


**错误消息︰**互联网连接已经过验证，但无法建立连接到的服务操作的见解。 请稍后再试。

**可能的原因︰**
- 在维护是运营经验服务。 等待，直到执行操作的见解维护工作。
- 您的网络已阻止操作的见解。 请与网络管理员联系并请求的访问权限操作的见解，或另一台服务器用作网关。

**错误消息︰**无法建立 Internet 连接。 请检查您的代理设置。

**可能的原因︰**
- 此服务器没有连接到互联网。 检查 Internet 连接状态，并将服务器连接到 Internet。
- 代理设置不正确。 有关如何设置或更改您的代理服务器设置的信息，请参阅[配置代理服务器和防火墙设置](operational-insights-proxy-firewall.md)。
- 代理服务器要求身份验证。 请参阅以了解如何操作管理器配置为使用代理服务器[配置代理服务器和防火墙设置](operational-insights-proxy-firewall.md)。


## SQL Server 查询的疑难解答

如果您运行的 Microsoft SQL Server 2008 R2 和部署操作管理器代理，尽管看不到此服务器的警报，则可能必须发现问题。

要确认是否这是问题的来源，请检查下面两个问题︰

- 在操作管理器事件日志中，您会看到事件 ID 4001。 此事件表示无效的类。

- 在 SQL Server 配置管理器中，查看 SQL Server 服务，当您看到错误消息"远程过程调用失败。 []"0x0800706be

如果这两个问题，您需要安装 SQL Server 2008 R2 Service Pack 2。 若要下载此服务包，请参阅[SQL Server 2008 R2 Service Pack 2](http://go.microsoft.com/fwlink/?LinkId=271310)中 Microsoft 下载中心。

安装服务包后，您应该在 24 小时内看到服务器的运营经验数据。

## 疑难解答代理或运营经理数据流向运营的见解

下面一组步骤是为了作为一个指南，帮助解决直接连接代理或运营经理部署配置到 Azure 运营洞察力的报告数据。

### 步骤 1︰ 验证是否适当的管理包获取下载到您的操作管理器环境
>[AZURE.NOTE] 如果您只使用直接代理，您可以跳到下一个过程。

根据从 OpInsights 门户网站已启用 （以前称为智能包） 的解决方案将会看到更多或更少的这些 MPs。 搜索关键字顾问或者智力在它们的名称。
您可以检查这些使用 OpsMgr PowerShell 的 Mp:

```Powershell
Get-SCOMManagementPack | where {$_.DisplayName -match 'Advisor'} | Select Name,Sealed,Version
Get-SCOMManagementPack | where {$_.Name -match 'IntelligencePacks'} | Select Name,Sealed,Version
```

>[AZURE.NOTE] 如果您在排查容量解决方案，检查*多少*管理包名称包含产能有︰ 有两个具有相同的显示名称的管理包 (但不同内部 ID)，采用相同的 MP 束;如果其中一个不会输入 （通常由于缺少 VMM 依赖项） 不会输入其他 MP 和不重试该操作。

您应该可以看到与产能的以下三个管理包
- 微软系统中心顾问容量智能包
- 微软系统中心顾问容量智能包
- 微软系统中心顾问容量存储数据

如果您只看到一个或其中两个，但不是所有三个，将其删除，然后等待操作管理器来下载并重新导入 — — 在此期间检查事件日志中有错误 5/10 分钟。

### 步骤 2︰ 验证是否正确的解决方案获取下载到您的直接代理
>[AZURE.NOTE] 如果您只使用操作管理器，您可以忽略此过程。

在直接代理，您应该看到在**C:\Program 该监视 Agent\Agent\Health 服务 State\Management 包**下缓存解决方案集策略


### 步骤 3︰ 验证是否正在发送数据到顾问服务 （或在最后一次尝试）
根据您使用的直接连接的代理或运营经理，您可以执行以下过程，直接代理计算机上或运营经理管理服务器上︰

1. - 打开性能监视器
2. - 选择运行状况服务管理组
3. - 添加 HTTP 开头的所有计数器

如果正确配置的事情您应该看到计数器，这些计数器的活动如上载事件和其他数据项 （基于在门户，并配置的日志收集策略解决方案 onboarded）。 这些计数器不一定必须不断 '忙'-如果看它可能是您不是在许多解决方案 onboarded 或有非常轻便的收集策略不活动性较低。

### 步骤 4︰ 检查上的管理服务器或直接代理事件日志的错误
作为最后一步如果上述所有方法均失败，但您仍然看不到数据服务接收到的检查发现在**事件查看器**的任何错误。

打开**事件查看器**–>**应用程序和服务**–>**运营经理**和由事件源的筛选器︰**顾问**、**健康服务模块**、 **HealthService**和**服务连接器**（此最后一个只应用于直接代理）。

这些事件大部分是类似在任一运营经理和直接代理和疑难解答的步骤将是类似的。
运营经理和直接代理不同的唯一部分是注册过程 （GUI 操作管理器中的，工作区中直接代理的 Id/键组合），但是，初始注册后证书交换，并且使用而且一切有关与该服务通信是一样的。

因此，其中许多事件适用于这两种类型的报告基础结构。 下面是常见的您可能会看到一个列表。

### 事件源健康服务模块
##### EventID 2138
这意味着您的代理服务器要求身份验证。 请按照下列步骤[配置代理服务器](https://msdn.microsoft.com/library/azure/dn884643.aspx)

##### EventID 2137
操作管理器无法读取身份验证证书。 重新运行审查注册向导将修复证书/runas 帐户

##### EventID 2132
指**未被授权**。 可能是有问题的证书和/或登记服务;请尝试重新运行注册向导，它可以修复证书和 RunAs 帐户。 此外，请验证代理服务器已设置为允许排除。 有关详细信息，请参阅[配置代理服务器](https://msdn.microsoft.com/library/azure/dn884643.aspx)

##### EventID 2129
这是由于失败的 SSL 协商失败的连接。 请验证您的系统被配置为使用 TLS 和不 SSL。 可能有一些奇怪的 SSL 设置与 chipers，在 Internet Explorer**高级**选项，或在 Windows 注册表项下有关此服务器上

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

##### EventID 2127
无法发送数据接收到错误代码。 如果这仅偶尔发生，它可能只是一个小故障。 关注它，理解它发生的频率。 如果经常出现相同 (每隔 10 分钟左右的时间较长)，则它可能是真正的问题 – 检查您的网络配置，上面描述的代理服务器设置并重新运行注册向导。 但如果只出现少 （亦即几次每天） 然后一切应该都适用，如排队和重新传输数据。 如果这就是它发生的频率，它可能是只是网络超时。

一些的 HTTP 错误代码即具有一些特殊的含义︰

- 内容直接代理或管理服务器会尝试将数据发送到我们的服务，第一次获得了 500 错误与内部 404 错误代码-404 意味着找不到;这表明，我们将使用此新的工作区，您的存储区域尚未完全准备好 — 正在配置。 下次重试，但是会准备就绪，流将开始 （正常情况） 下工作。
- 403 可能表示权限中的凭据问题，等等。

##### EventID 2128
DNS 名称解析失败。 您的服务器不能解析打算将数据发送到我们的互联网地址。 这可能是 DNS 解析器设置在您的计算机、 不正确的代理服务器设置或在您的提供商的 （临时） DNS 问题。 像以前的事件，具体取决于它在不断地发生，或偶尔可能表示真正的问题 — 或不。

##### EventID 2130
超时值。 像以前的事件，具体取决于它在不断地发生，或偶尔可能是一个问题 — 或不。

### 来自源 HealthService 事件
##### EventID 4511
无法加载模块"System.PublishDataToEndPoint"– 找不到文件。 "System.PublishDataToEndPoint"("{D407D659-65E4-4476-BF40-924E56841465}"CLSID) 类型的模块的初始化失败，错误代码系统无法找到指定的文件。  
此错误指示您已在您计算机上的旧 Dll 不包含所需的模块。 解决方法是更新到最新的更新汇总的管理服务器。

##### EventID 4502
出现故障的模块。 如果您看到此工作流的名称，如**CollectInstanceSpace**或**CollectTypeSpace**它可能意味着服务器时遇到问题，发送一些配置数据。 根据频率发生-经常或偶尔-它可能是一个问题，或没有。 如果出现更，每隔一小时肯定会出现问题。 如果只将失败，此操作每日一次或两次，它将会能够恢复正常。 这取决于该模块的实际故障 （说明将有更多详细信息） 这可能是内部问题--也就是收集到 DB — 或发送到云的问题。 请验证您的网络和代理服务器设置和最糟糕的案例尝试重新启动 HealthService。

##### EventID 4501
出现故障的模块"System.PublishDataToEndPoint"。 模块类型的"System.PublishDataToEndPoint"报告错误 87 L 的运行规则运行实例"操作管理器管理组"id"Microsoft.SystemCenter.CollectAlertChangeDataToCloud"的一部分:"{6B1D1BE8-EBB4-B425-08DC-2385C5930B04}"管理组"SCOMTEST"。
应该*未*看到此与此工作流程、 模块和错误的确切了，所以用来是[*现在修复*bug](http://feedback.azure.com/forums/267889-azure-operational-insights/suggestions/6714689-alert-management-intelligence-pack-not-sending-ale)。 如果您看到此再次将其请报告到您首选的 Microsoft 支持渠道。


### 来自源服务连接器事件
##### EventID 4002
服务在查询响应中返回的 HTTP 状态代码 403。  请与服务的运行状况的服务管理员。 稍后将重试查询。 您可以在代理的初始注册阶段获得 403，您将看到 URL https://<YourWorkspaceID>.oms.opinsights.azure.com/ AgentService.svc/AgentTopologyRequest 错误代码 403 意味着 fordbidden--这通常是错误地复制 WorkspaceId 或键，或时钟不同步的计算机上。 尝试与可靠的时间源同步，使用连接检查控制面板小程序的 Microsoft 监控代理验证具有右区 Id 和密钥。


### 步骤 5︰ 寻找代理发送他们的数据，并让其在门户中的索引
检查在 OpInsights 门户中，从概述页导航到小图块**服务器和使用**--这将显示如果管理组 （和其代理） 和直接代理日志搜索到报告数据。 从数据派生的图块上的代理的数量 – 如果机器不报告的两周内他们将放置雷达。

钻取的列表将您记录搜索并显示每台计算机的最后一个索引的数据的时间戳。 从这里您可以浏览它是何种数据。 配置数据收集的数量和其解决方案、 数据上载时间表和速度可以改变。

此页还提供了计量信息 （此操作不使用日志搜索索引但计费系统，它具有刷新每隔几个小时） 发送到服务解决方案按细分的数据量有关。
