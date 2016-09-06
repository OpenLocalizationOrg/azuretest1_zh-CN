---
ms.openlocfilehash: b39e46debb97c29ed29babd3266e86c53ecf8e09
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="比较︰ Apache 风暴与 Azure 流分析 |Microsoft Azure"
    description="了解如何使用 Twitter 主观意图进行实时分析流分析。 从事件生成活动仪表板上的数据的分步指导。"
    keywords="实时的 twitter，观点分析、 社交媒体分析，社交媒体分析工具"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="08/19/2015"
    ms.author="jeffstok"/>

# Apache 的暴风雨和 Azure 流分析的比较 #

## 简介 ##

本文档说明了 Azure 流分析和 Apache 风暴的定位以 HDInsight 托管服务。 目标是帮助客户了解这两种服务的价值主张并做出的决定的是其业务用例的正确选择。

虽然两者都提供 PaaS 解决方案带来的好处，有区分这些服务的几个主要的显著功能。 我们相信出功能，以及限制，该列表的这些服务将帮助客户解决方案能够实现其目标所需的土地。

## 常规 ##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure 流分析</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>在 HDInsight 上的 Apache 风暴</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>打开源</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
不对，Azure 流分析是一种 Microsoft 专用产品。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
是的 Apache 风暴是 Apache 许可的技术。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Microsoft 支持</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
是 </p>
            </td>
            <td width="246" valign="top">
                <p>
是 </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>硬件要求</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
没有硬件要求。 Azure 流分析是 Azure 服务。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
没有硬件要求。 Apache 风暴是 Azure 服务。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>最上层级单位</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
与 Azure 流分析客户部署和监视流作业。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
在 HDInsight 上的 Apache 风暴与客户部署和监视整个群集，可以承载多个风暴作业，以及其他工作负荷 （包括批处理）。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Price</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
流分析价位被处理的数据卷和流式处理单位 （每小时作业正在运行） 的数目。
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/stream-analytics/">更多价格信息可在此处找到。</a>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Apache HDInsight 上的风暴，为采购单位是基于群集的并且基于群集运行时，独立于部署作业的时间计费。
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/hdinsight/">更多价格信息可在此处找到。</a>
                </p>
            </td>
        </tr>
    </tbody>
</table>
## 创作 ##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure 流分析</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>在 HDInsight 上的 Apache 风暴</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>功能︰ SQL DSL</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
是的提供易于使用的 SQL 语言支持。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
否，用户必须编写的代码在 Java C# 或使用 Trident Api。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>功能︰ 临时运算符</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
开箱即用支持窗口的聚合，并临时联接。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
临时的运算符必须由用户来实现。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>开发经验</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
交互式编写和调试示例数据通过 Azure 的门户体验。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
开发、 调试和监视体验是通过 Visual Studio.NET 的用户，而对于 Java 和其他语言的开发人员的体验可能会使用他们选择的 IDE。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>调试支持</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
流分析提供了基本的作业状态和操作日志作为一种调试，但当前不提供怎样/如何大部分包含在日志中 ie 的灵活性︰ 详细模式。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
详细的日志都可用于调试目的。 有两种方法对表面的日志传输到用户，通过 visual Studio 或用户可以将 RDP 到群集访问日志。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>支持 UDF （用户定义的函数）</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
目前，并没有支持 Udf。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Udf 可以用 C#、 Java 或您选择的语言编写。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>可扩展的自定义代码 </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
没有支持可扩展的代码流分析中。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
是的在风暴中 C#、 Java 或其他受支持的语言编写自定义代码的可行性。
                </p>
            </td>
        </tr>
    </tbody>
</table>
## 输入和输出 ##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure 流分析</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>在 HDInsight 上的 Apache 风暴</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                 <strong>输入的数据源</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>支持的输入的源是 Azure 事件集线器和 Azure Blob。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
对事件集线器、 服务总线，Kafka 等有可用的连接器。不受支持的连接器可以通过自定义代码来实现。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>输入数据格式</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
受支持的输入的格式是 Avro，JSON，CSV。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
任何格式可以通过自定义代码来实现。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>输出</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
一个流作业可以具有多个输出。 支持输出︰ Azure 事件集线器、 Azure Blob 存储，Azure 表、 SQL Azure 数据库和 PowerBI。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
支持在一个拓扑中的多个输出，对于每个输出可能有下游处理的自定义逻辑。 开箱即用冲击包括 PowerBI、 Azure 事件集线器，Azure Blob 存储，Azure DocumentDB、 SQL 和 HBase 连接器。 不受支持的连接器可以通过自定义代码来实现。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>数据编码格式</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
流分析要求 utf-8 数据格式使用。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
通过自定义代码，可以实现任何数据编码格式。
                </p>
            </td>
        </tr>
    </tbody>
</table>
## 管理和操作 ##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure 流分析</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>在 HDInsight 上的 Apache 风暴</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>作业部署模型</strong>
                </p>
                <p>
                    - <strong>Azure 门户</strong>
                </p>
                <p>
                    - <strong>Visual Studio</strong>
                </p>
                <p>
                    - <strong>PowerShell</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
部署是通过 Azure 门户、 PowerShell 和 REST Api 实现的。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Depolyment 是通过 Azure 门户、 PowerShell，Visual Studio 和 REST Api 实现的。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>监视 （统计、 计数器等）</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
监控是通过 Azure 门户和 REST Api 实现的。
                </p>
                <p>
用户还可以配置 Azure 的警报。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
监控是通过风暴 UI 和 REST Api 实现的。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>可扩展性</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
每个作业流的单位数。 每个流单元处理高达 1 MB/s。 默认情况下的 50 个单位的最大值。 调用以增加限制。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
在 HDI 风暴群集中的节点数。 节点 （顶部限制由您 Azure 的配额） 的数量没有限制。 调用以增加限制。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>数据处理限制</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
用户可以放大或缩小流要增加数据处理或优化成本的单位数。
                </p>
                <p>
缩放到 1 GB/s </p>
            </td>
            <td width="246" valign="top">
                <p>
用户可以放大或缩小簇大小，以满足需求。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>停止/继续</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
停止并从上次停止的地方重新开始。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
停止并重新开始从上一次放置已停止根据水印。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>服务和框架的更新</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
自动修补程序而不需要停机。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
自动修补程序而不需要停机。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>通过具有保证 SLA 的高可用服务业务连续性</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
SLA 为 99.9%正常运行时间 </p>
                <p>
从故障自动恢复 </p>
                <p>
恢复状态的时间性运算符是内置的。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
SLA 为风暴群集的 99.9%正常运行时间。 Apache 风暴是容错流平台，但是它是客户的责任，以确保不间断地运行工作流。
                </p>
            </td>
        </tr>
    </tbody>
</table>
## 高级的功能 ##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure 流分析</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>在 HDInsight 上的 Apache 风暴</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>最晚到达和无序的事件处理</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
内置的配置策略，以重新排序，请拖放事件或事件时间调整。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
用户必须实现逻辑来处理这种情况。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>引用数据</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
从 Azure Blob 最大大小为 100 MB 的高速缓存内存中查找可用的参考数据。 由服务管理是引用数据的刷新。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
数据大小没有限制。 可用于 HBase、 DocumentDB、 SQL Server 和 Azure 的连接器。 不受支持的连接器可以通过自定义代码来实现。 
                </p>
                <p>
由自定义代码，必须处理引用数据的刷新。
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>机器学习与集成</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
是的通过配置发布 Azure 机器学习模型作为函数 ASA 作业创建过程。
                </p>
            </td>
            <td width="246" valign="top">
                <p>
可通过风暴螺栓。
                </p>
            </td>
        </tr>
    </tbody>
</table>