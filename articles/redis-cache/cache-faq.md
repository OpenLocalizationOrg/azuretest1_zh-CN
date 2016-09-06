---
ms.openlocfilehash: 66f1d9c29b9c60019fdb18a1c82c92d03e3c098e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure Redis 缓存常见问题解答" 
    description="学习 Redis Azure 的高速缓存的解答常见问题、 模式和最佳实践" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2015" 
    ms.author="sdanie"/>

# Azure Redis 缓存常见问题解答

学习 Redis Azure 的高速缓存的解答常见问题、 模式和最佳实践。

<a name="cache-size"></a>
## 我应该使用什么 Redis 缓存产品和大小？

每个 Azure Redis 缓存服务提供了不同级别的**大小**、**带宽**、**高可用性**和**SLA**选项。

-   基本的 SKU-单个节点、 没有复制或 SLA，从 250 MB 的高速缓存大小为 53 GB。
-   标准的 SKU-53 GB 到 99.9 %sla 的自动复制缓存大小从 250 MB 的主要/辅助节点。

如果需要高可用性，请选择标准缓存提供哪有 99.9 %sla。 对于开发和原型设计，或不需要 SLA 的方案，基本产品可能比较合适。

高速缓存大小和带宽映射大致大小和带宽的虚拟机的主机缓存。 基本和标准产品的 250 MB 大小的额外小 (A0) 虚拟机大小，使用专用的核心承载其他大小的同时使用共享的核心，被承载承载。 小 (A1) 虚拟机大小 1 用于操作系统和 redis 缓存服务专用虚拟内核上承载的 1 GB 缓存大小。 具有多个专用的虚拟内核的大 VM 实例上承载更大高速缓存大小。

如果缓存中有较高的吞吐量，选择 1GB 大小或更大，以便缓存正在使用专用的核心。 1 核心虚拟机上承载的 1 GB 缓存大小。 这种核心用于操作系统和缓存服务。 在具有多个内核，虚拟机上运行大于 1 GB 的缓存和 Redis 缓存使用专用的核心技术，不会与操作系统共享。

**Redis 是单线程**因此有两个以上的内核不提供上有两个核心，但**较大的 VM 大小通常有更多带宽要高于较小**的另一个好处。 如果缓存服务器或客户端达到了带宽限制，您将收到在客户端超时。

下表显示了测试各种尺寸的 Azure Redis 缓存使用时观察到的最大带宽值`redis-benchmark.exe`从 Azure Redis 缓存端点根据 Iaas VM。 请注意，这些值不能保证没有没有 SLA 的这些数字，但应为典型。 您应该加载测试您自己的应用程序，以确定您的应用程序的适当的缓存大小。

高速缓存名称|高速缓存大小|Get/秒 （1 KB 值的简单获取调用）|带宽 （Mb/秒）
---|---|---|---
C0|250 MB|610|5
C1|1 GB|12,200|100
C2|2.5 GB|24,300|200
C3|6 GB|48,875|400
C4|13 GB|61,350|500
C5|26 GB|112,275|1000
C6|53 GB|153,219|超过 1000年个

有关下载的 Redis 工具如说明`redis-benchmark.exe`，请参阅[如何运行 Redis 命令？](#cache-commands)部分。

<a name="cache-region"></a>
## 在哪些地区应找到我缓存？

为获得最佳性能和最低的延迟，作为缓存客户端应用程序的相同区域中找到 Azure Redis 高速缓存。

<a name="cache-billing"></a>
## 我如何在结算为 Azure Redis 缓存？

Azure Redis 缓存定价是[这里](http://azure.microsoft.com/pricing/details/cache/)。 定价页列出了定价为小时工资率。 从高速缓存将被删除之前创建的时间按每分钟计费缓存。 没有任何用于停止或暂停的帐单的缓存选项。

<a name="cache-timeouts"></a>
## 为什么会看到超时？

在客户端用来与 Redis 发生超时。 在大多数情况下 Redis 服务器会不会超时。 将命令发送到 Redis 服务器，该命令排队等候和 Redis 服务器最终挑选命令并执行它。 但是客户端可以在此过程超时时间和是否异常引发上的调用方。 超时问题进行故障排除的详细信息，请参阅[Investigating StackExchange.Redis Redis Azure 的高速缓存超时异常](http://azure.microsoft.com/blog/2015/02/10/investigating-timeout-exceptions-in-stackexchange-redis-for-azure-redis-cache/)。

<a name="cache-monitor"></a>
## 如何监视运行状况和我缓存的性能？

可以在[Azure 预览门户](https://portal.azure.com)监视 Microsoft Azure Redis 缓存实例。 可以查看指标、 固定到 Startboard 的标准图表、 自定义监视图表的日期和时间范围，添加和从图表中删除指标和满足某些条件时设置警报。 这些工具使您能够监视 Azure Redis 缓存实例的运行状况，并帮助您管理应用程序缓存。 有关监视高速缓存的详细信息，请参阅[显示器 Azure Redis 高速缓存](https://msdn.microsoft.com/library/azure/dn763945.aspx)。

<a name="cache-disconnect"></a>
## 我的客户端被断开从缓存

以下是一些常见缓存断开连接的原因。

-   客户端的原因
    -   客户端应用程序被重新部署。
    -   客户端应用程序执行缩放操作。
        -   云服务或 Web 应用程序，这可能是由于自动缩放。
    -   更改网络上的客户端层。
    -   瞬态错误出现在客户端或客户端和服务器之间的网络节点中。
    -   已达到带宽的阈值限制。
    -   CPU 绑定的操作时间太长而无法完成。
-   服务器端的原因
    -   该产品标准的高速缓存，Azure Redis 缓存服务启动故障转移从主节点到辅助节点。
    -   Azure 已修补的实例部署缓存的位置
        -   这可以是 Redis 服务器更新或 VM 的常规维护。

<a name="cache-configuration"></a>
## StackExchange.Redis 的配置选项是什么？

StackExchange.Redis 有很多选项。 这一节谈到的一些常见设置。 StackExchange.Redis 选项的详细信息，请参阅[StackExchange.Redis 配置](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md)。

ConfigurationOptions|说明|建议
---|---|---
AbortOnConnectFail|当设置为 true，则该连接将不重新连接网络出现故障后。|设置为 false，让 StackExchange.Redis 自动重新连接。
ConnectRetry|连接在初始过程中重复的连接尝试的次数。||
ConnectTimeout|以毫秒为单位的超时连接操作。|

在大多数情况下客户端的默认值是足够的。 您可以微调选项根据您的工作负载。

-   重试次数
    -   ConnectRetry 和 ConnectTimeout 的一般性指导原则是快速失败，然后重试。 这取决于您的工作负载和上花费的时间平均则采用为您的客户端发出 Redis 命令和接收响应。
    -   让 StackExchange.Redis 而不是检查连接状态，然后重新连接自己时自动重新连接。 **避免使用 ConnectionMultiplexer.IsConnected 属性**。
    -   Snowballing-有时您可能会遇到的问题，其中将重试，这 snowballs 并永远不会恢复。 在这种情况下，您应该考虑使用指数退避算法重试算法，如[重试的一般指南](https://github.com/mspnp/azure-guidance/blob/master/Retry-General.md)中所述由 Microsoft 模式与实践小组发布。
-   超时值
    -   请考虑您的工作负载，并相应地设置值。 如果您存储较大的值，设置为较高的值超时。
        -   将 ABortOnConnectFail 设置为 false，让您重新连接 StackExchange.Redis。
-   应用程序使用一个 ConnectionMultiplexer 实例。 可以使用 LazyConnection 来创建单个实例返回的连接属性，如下所示[连接到使用 ConnectionMultiplexer 类缓存](https://msdn.microsoft.com/library/azure/dn690521.aspx#Connect)中。
-   设置`ConnectionMultiplexer.ClientName`为诊断目的的应用程序实例唯一名称的属性。
-   使用多个`ConnectionMultiplexer`的自定义工作负载的实例。
    -   如果您的应用程序中有不同的负载，您可以按照这种模型。 例如︰
        -   有一个大密钥处理的多路复用器。 
        -   有一个小键处理多路复用器。 
        -   可以设置不同的值，连接超时和重试逻辑，对于您使用的每个 ConnectionMultiplexer。
        -   设置`ClientName`属性对每个多路复用器有助于诊断。 
        -   这将导致对更多优化每个延迟`ConnectionMultiplexer`。

<a name="cache-redis-commands"></a>
## 什么是一些注意事项使用常用的 Redis 命令时？

-   您不应该运行某些 Redis 命令这花费很长时间才能完成，而无需了解这些命令的影响。
    -   例如，不要运行[键](http://redis.io/commands/keys)命令在生产中根据它可能需要很长时间才能返回根据键的数目。 Redis 是一个单线程服务器，并命令处理一次。 如果您有其他键后发布的命令，他们将不会处理 Redis 处理键命令之前。
-   小的密钥值或较大的密钥值应使用的密钥大小的？ 一般情况下它取决于该方案。 如果您的情况需要较长的密钥可以调整 ConnectionTimeout 和重试值然后调整重试逻辑。 Redis 服务器的角度来看，从较小的值会观察到有更好的性能。
    -   这并不意味着不能将较大的值存储在 Redis;必须注意下列事项。 滞后时间将会更高。 如果您有一个大的数据集和一个小的您可以使用多个 ConnectionMultiplexer 实例，以前的[该怎么办的 StackExchange.Redis 配置选项](#cache-configuration)节中所述配置有一组不同的超时和重试值。


<a name="cache-ssl"></a>
## 时应使连接到 Redis 的非 SSL 端口？

Redis 服务器不支持 SSL 开箱即用，但 Redis Azure 的高速缓存。 如果您要连接到 Azure Redis 缓存和您的客户端支持 SSL，如 StackExchange.Redis，您应使用 SSL。

注意默认情况下，新 Azure Redis 缓存实例的情况下禁用的非 SSL 端口。 如果您的客户端不支持 SSL，然后您必须启用非 SSL 端口按照[配置 Redis Azure 的高速缓存中的缓存](https://msdn.microsoft.com/library/azure/dn793612.aspx)项目的[访问端口](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts)部分中的说明进行操作。

如 redis 工具`redis-cli`不使用的 SSL 端口，但您可以使用一个实用程序，如`stunnel`安全地使用 SSL 端口连接工具，按照[Redis 预览版宣布 ASP.NET 会话状态提供程序](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)撰写的博客文章中的说明进行操作。

下载 Redis 工具的说明，请参阅[如何运行 Redis 命令？](#cache-commands)部分。

<a name="cache-benchmarking"></a>
## 如何进行基准和测试我缓存的性能？

-   [启用缓存诊断程序](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics)使您可以[监视](https://msdn.microsoft.com/library/azure/dn763945.aspx)您的缓存的运行状况。 您可以预览门户中查看规格，您还可以[下载并查看](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring)其使用您选择的工具。
-   您可以使用 redis benchmark.exe 加载测试 Redis 服务器。
    -   确保负载测试客户机和 Redis 缓存在同一区域。
-   使用 redis cli.exe 和监视缓存使用 INFO 命令。
    -   如果您的负载导致较高的内存碎片然后您应扩大到更大的缓存容量。
-   下载 Redis 工具的说明，请参阅[如何运行 Redis 命令？](#cache-commands)部分。

<a name="cache-commands"></a>
## 如何运行 Redis 命令？

您可以使用任何除了列出在[Redis Redis Azure 的高速缓存中不支持的命令](cache-configure.md#redis-commands-not-supported-in-azure-redis-cache)的命令在[Redis 命令](http://redis.io/commands#)列出的命令。 若要运行 Redis 命令可以有多种选项。

-   如果您有一个标准的缓存，您可以运行 Redis 命令使用[Redis 控制台](cache-configure.md#redis-console)。 这提供了一种安全的方法，在预览门户中运行 Redis 命令。
-   您还可以使用 Redis 命令行工具。 若要使用它们，请执行以下步骤。
    -   下载[Redis 命令行工具](https://github.com/MSOpenTech/redis/releases/download/win-2.8.19.1/redis-2.8.19.zip)。
    -   连接至高速缓存可使用`redis-cli.exe`。 通过在缓存端点使用-h 开关，按键使用-a 在下面的示例所示。
        -   `redis-cli -h <your cache name>.redis.cache.windows.net -a <key>`
    -   请注意，Redis 命令行工具不起作用的 SSL 端口，但您可以使用一个实用程序，如`stunnel`安全地使用 SSL 端口连接工具，按照[Redis 预览版宣布 ASP.NET 会话状态提供程序](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)撰写的博客文章中的说明进行操作。

<a name="cache-common-patterns"></a>
## 一些常见的缓存模式和注意事项是什么？

-   Microsoft 模式与做法有以下指南。
    -   [缓存的指导](https://github.com/mspnp/azure-guidance/blob/master/Caching.md)。
    -   [Azure 的云应用程序设计和实施指南](https://github.com/mspnp/azure-guidance)
-   [常见的缓存模式使用 Azure Redis 高速缓存](cache-howto-common-cache-patterns.md)

<a name="cache-reference"></a>
## Azure Redis 缓存为什么没有像某些其他 Azure 服务 MSDN 类库参考？

Microsoft Azure Redis 缓存基于 Redis 的高速缓存，这样就可以访问到一个安全的、 专用 Redis 高速缓存，由 Microsoft 管理常用开源。 大量的[Redis 客户端](http://redis.io/clients)都可用于许多编程语言。 每个客户端都有它自己对 Redis 缓存实例使用[Redis 命令](http://redis.io/commands)进行调用的 API。

在 MSDN; 由于每个客户端不同，还有没有一个集中的类引用而每个客户端维护自己引用的文档。 除了参考文档中，Azure.com 显示如何开始使用 Redis [Redis 缓存文档](http://azure.microsoft.com/documentatgion/services/redis-cache/)页面上使用不同的语言和缓存客户端缓存的 Azure 上还有几个教程。
