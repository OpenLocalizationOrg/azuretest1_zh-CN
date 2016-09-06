---
ms.openlocfilehash: 93ac5d5dde2ec25a409f4551f3cfb9c4b68ba926
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="SQL azure 数据库资源限制"
    description="此页为 SQL Azure 数据库介绍一些常见的资源限制。"
    services="sql-database"
    documentationCenter="na"
    authors="rothja"
    manager="jeffreyg"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/28/2015"
    ms.author="jroth" />


# Azure 的 SQL 数据库资源限制

## 概述

Azure 的 SQL 数据库管理的资源提供给数据库使用两个不同的机制︰**资源管理**和**实施的限制**。 本主题说明资源管理这两个主要区域。

## 资源管理
基本、 标准和特优服务层的设计目标之一是 Azure 才能表现就像在自己的机器，完全独立于其它数据库上运行数据库的 SQL 数据库。 资源进行监管，以模拟这种行为。 如果总的资源利用率达到最大的可用 CPU、 内存、 日志 I/O 和分配给数据库的数据 I/O 资源，资源治理将队列中执行查询并将资源分配给排队的查询，如他们释放。

为在专用计算机上，利用所有可用的资源将导致更长的时间执行的当前正在执行查询，这可能会导致在客户端的命令超时。 试图执行新查询时已达到限制的并发请求时，雄心勃勃的重试逻辑的应用程序，并对具有高频率的数据库执行查询的应用程序可能会遇到错误消息。

### 建议︰
在接近一个数据库的最大使用率时监视的资源利用率，以及查询的平均响应时间。 遇到较长的查询延迟时通常会有三个选项︰

1.  将传入的请求的数量减少到数据库，以避免超时和向上的请求之中。

2.  为数据库指派的更高的性能级别。

3.  优化查询，以减少每个查询的资源利用率。 有关详细信息，请参阅在 Azure SQL 数据库性能的指南文章中的查询优化/Hinting 部分。

## 强制执行的限制
资源而不是 CPU、 内存、 日志 I/O 和数据 I/O 会强制执行达到限制时拒绝新请求。 客户端将收到[错误消息](sql-database-develop-error-messages.md)根据已达到限制。

例如，SQL 数据库的连接数，以及可以处理的并发请求数将受到限制。 SQL 数据库允许连接到数据库的并发请求支持连接池数超过数。 可用连接的数量可以容易地控制应用程序，而并行请求的金额通常是很难估计和控制的时间。 特别是在峰值负载期间或者发送请求太多或数据库达到其资源的应用程序限制和成堆更长时间运行的查询，由于工作线程启动时可以遇到错误。 

## 服务层和性能级别

由数据库的性能级别定义数据库的实际限制。 有关详细说明，请参阅[Azure SQL 数据库服务层和性能级别](https://msdn.microsoft.com/library/azure/dn741336.aspx)。

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

## 数据库设置配额

Azure SQL 数据库都有每个逻辑服务器的当前 2000 DTUs DTU 配额。 此配额表示逻辑服务器可以承载，DTUs 基于 DTUs 的总和如果在服务器上的每个数据库的性能级别。 例如，有 5 个基本数据库 (最大 5 X 5 DTUs)、 2 标准 S1 数据库 (2 X 20 DTUs 最大) 和 3 特优 P1 数据库 （3 X 100 DTUs 最多） 的服务器占用了 365 DTUs 的 2000 DTU 配额。

>[AZURE.NOTE] 您可以通过[联系支持人员](http://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)请求此配额的增加。

## 资源

[Azure 订阅和服务限制、 配额和约束](../azure-subscription-service-limits.md)

[SQL azure 数据库服务层和性能级别](https://msdn.microsoft.com/library/azure/dn741336.aspx)

[对于 SQL 数据库的客户端程序的错误消息](sql-database-develop-error-messages.md)