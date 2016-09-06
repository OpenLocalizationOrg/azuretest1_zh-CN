---
ms.openlocfilehash: b9176381f069da52aefe0d1d87bdceef0e95741c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="除了 ADO.NET 4.5 和 SQL 的 1433年端口数据库 V12 |Microsoft Azure"
    description="客户端连接到 Azure SQL 数据库 V12 有时绕过代理，直接与数据库进行交互。 除 1433年端口变得重要。"
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jeffreyg"
    editor="" />


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="genemi"/>


# 除了 ADO.NET 4.5 和 SQL 数据库 V12 1433 端口


本主题描述 Azure SQL 数据库 V12 带给使用 ADO.NET 4.5 或更高版本的客户端的连接行为的更改。


## SQL 数据库的 V11︰ 端口 1433年


当客户端程序使用 ADO.NET 4.5 连接和查询的 SQL 数据库 V11 时，内部顺序如下所示︰


1. ADO.NET 将尝试连接到 SQL 数据库。

2. ADO.NET 使用端口 1433年调用中间件模块，并连接到 SQL 数据库的中间件。

3. SQL 数据库发送回中间件将转发到端口 1433 ADO.NET 的响应及其响应。


**的术语︰**我们说，ADO.NET 使用*代理服务器路由*与 SQL 数据库交互描述上面的序列。 如果没有中间件涉及我们会说用*直接路由*。


## SQL 数据库的 V12︰ 内部与外部


连接到 V12 我们必须询问是否客户端程序运行*外部*或*内部*的 Azure 云边界。 各小节讨论了两种常见情况。


#### *外部︰*在桌面计算机上运行的客户端


端口 1433年是必须在承载您的 SQL 数据库客户端应用程序的桌面计算机上打开的唯一端口。


#### *内︰*在 Azure 上运行的客户端


当您的客户端运行在 Azure 云边界内时，它使用我们可以称之为*直接路由*与 SQL 数据库服务器进行交互。 建立连接后，进一步在客户机和数据库之间的交互涉及任何中间件代理。


顺序如下所示︰


1. 4.5 （或更高版本） 的 ADO.NET 启动交互的简要使用 Azure 的云，并接收动态标识的端口号。
 - 动态确定的端口号范围内的 11000-11999 是。

2. ADO.NET 再到 SQL 数据库服务器直接连接，没有中间件与其间。

3. 查询直接发送到数据库，并直接向客户机返回结果。


请确保处于可用的客户端交互，ADO.NET 4.5 与 SQL 数据库 V12 11000-11999 Azure 的客户端计算机上的端口范围。

- 特别是，在范围内的端口必须可用的任何其他出站拦截器。
- Azure VM 上的 Windows 防火墙控制的端口设置。


## 包含在代理服务器路由的隐式重试逻辑


在生产环境中，客户端连接到 Azure SQL 数据库 V11 或 V12 的建议在其代码中实现重试逻辑。 这可以是自定义代码，也可以利用 API 如企业库的代码。


本主题中前面所述的代理服务器路由是重试逻辑的问题相关的︰


- 在 V11，还充当代理的中间件模块提供了适度程度的重试逻辑，要妥善处理某些瞬时故障。

- V12，代理不提供任何重试逻辑。


在这两种情况我们建议客户端在其自己的代码中实现重试逻辑。 可以说在客户端的重试逻辑需要增加与提供任何重试逻辑的最新代理服务器路由。


有关代码示例来演示重试逻辑，请参阅︰[客户端代码示例 SQL 数据库的快速启动](sql-database-develop-quick-start-client-code-samples.md)。


## 版本说明


这一节阐明了指向产品版本的名字对象。 它还列出了一些版本产品之间的配对。


#### ADO.NET


- ADO.NET 4.0 支持的 TDS 7.3 协议，但不是 7.4。
- ADO.NET 4.5 及更高版本支持的 TDS 7.4 协议。


#### SQL 数据库 V11 和 V12


本主题中突出显示 SQL 数据库 V11 和 V12 之间的客户端连接差异。


*注意︰*事务处理 SQL 语句`SELECT @@version;`返回的值开头的数字如 '11'。 或者"12。"以及那些 V11 和 V12 我们版本名称匹配的 SQL 数据库。


## 相关的链接


- [在 SQL 数据库 V12 中的新增功能](sql-database-v12-whats-new.md)


- [连接到 SQL 数据库︰ 链接、 最佳做法和设计指南](sql-database-connect-central-recommendations.md)


- ADO.NET 4.6 发布于 2015 年 7 月 20 日。 从.NET 团队博客发布位于[此处](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx)。


- ADO.NET 4.5 发布于 2012 年 8 月 15 日。 从.NET 团队博客发布位于[此处](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx)。
 - 有关 ADO.NET 4.5.1 博客张贴内容可[在此处](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx)。


- [TDS 协议版本列表](http://www.freetds.org/userguide/tdshistory.htm)

