---
ms.openlocfilehash: da46e39b20570708814b3a8404985957b0fda7d2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="启用监视和诊断" 
    description="了解如何设置您的资源在 Azure 中的诊断程序。" 
    authors="stepsic-microsoft-com" 
    manager="ronmart" 
    editor="" 
    services="azure-portal" 
    documentationCenter="na"/>

<tags 
    ms.service="azure-portal" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/28/2015" 
    ms.author="stepsic"/>

# 启用监视和诊断

在[Azure 门户网站](http://portal.azure.com)中，可以将配置有关资源丰富、 频繁、 监控和诊断数据。 您可以使用的[REST API](https://msdn.microsoft.com/library/azure/dn931932.aspx)或[.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/)通过编程方式配置诊断。

在 Azure 中的诊断程序、 监控和度量数据将保存在您选择的存储帐户。 这使您可以使用所需的任何工具读取数据、 存储资源管理器中，从电源给第三方工具的业务智能。

## 当您创建一个资源

多数服务都允许您启用诊断程序，第一次在[Azure 门户网站](http://portal.azure.com)中创建了它们。

1. 转到**新建**并选择您感兴趣的资源。 

2. 选择**可选的配置**。
    ![诊断程序刀片式服务器](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)

3. 选择**诊断**，然后**单击**。 您需要选择您想要保存到的诊断的存储帐户。 诊断发送到存储帐户时，您将需要支付正常的数据速率为存储和事务。

4. 单击**确定**并创建资源。 

## 更改现有的资源设置

如果您已创建了一个资源，您想要更改的诊断设置 （例如更改数据集的级别），您可以做到这一点在 Azure 门户。

1. 转到该资源，然后单击**设置**命令。

2. 选择**诊断**。

3. **诊断程序**刀片式服务器有可能诊断和监视集合数据对该资源的所有。 对于某些资源还可以选择**保留**策略的数据，从您的存储帐户进行清理。 
    ![存储诊断](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)

4. 一旦选择了您的设置，请单击**保存**命令。 可能需要一段时的监控数据，如果要启用它第一次露面。 

### 虚拟机的数据收集的类别
为虚拟机的所有指标和日志将都记录每次间隔一分钟，使有关您的计算机可以始终有最新的信息。

- **基本指标**︰ 有关您如处理器和内存的虚拟机的运行状况指标 
- **网络和 web 标准**︰ 有关您的网络连接和 web 服务的指标
- **.NET 的指标**︰ 在虚拟机上运行的.NET 和 ASP.NET 应用程序有关的指标
- **SQL 标准**︰ 如果您运行的 Microsoft SQL 服务，其性能指标
- **Windows 应用程序日志事件**︰ Windows 事件发送到应用程序的通道
- **Windows 事件系统日志**︰ 将被发送到系统通道的 Windows 事件。 这还包括从[Microsoft 反恶意软件](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409)的所有事件。 
- **Windows 事件安全日志**︰ 安全通道发送的 Windows 事件
- **诊断程序基础结构日志**︰ 记录有关诊断收集基础结构
- **IIS 日志**︰ 关于您的 IIS 服务器的日志记录

请注意，此时不支持特定的 Linux 分发，并且必须在虚拟机上安装来宾代理。

## 下一步行动

* [接收通知](insights-receive-alert-notifications.md)每当发生的操作事件或指标跨越阈值。
* [监视服务标准](insights-how-to-customize-monitoring.md)，以确保您的服务是可用的并且能够作出响应。
* [自动缩放实例计数](insights-how-to-scale.md)，以确保按需服务扩展。
* [监视应用程序性能](insights-perf-analytics.md)如果想要了解完全代码性能如何在云中。
* [查看事件并审核日志](insights-debugging-with-events.md)以了解一切服务中发生了什么。
* [跟踪服务运行状况](insights-service-health.md)，以查明 Azure 出现性能下降或服务中断。 
 
测试
