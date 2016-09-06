---
ms.openlocfilehash: 4acca8cda5fb2604a40ce83bb46470e054d4a2bc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="与 SQL 数据仓库使用 Azure 流分析 |Microsoft Azure"
   description="开发解决方案与 Azure SQL 数据仓库使用 Azure 流分析的技巧。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sahaj08"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/22/2015"
   ms.author="sahajs"/>

# 与 SQL 数据仓库使用 Azure 流分析

Azure 流分析是一种完全托管的服务，通过流式处理数据在云中提供低延迟、 高可用性、 可扩展的复杂事件处理。 您可以通过阅读[简介 Azure 流分析][]学习基础知识。 然后，您可以学习如何使用流分析使用[入门教程][]创建一个端到端的解决方案。

在本文中，您将学习如何使用 Azure SQL 数据仓库数据库作为输出接收器蒸汽分析作业。

## 先决条件

首先，通过以下步骤在中运行[入门教程][]教程。  

1. 创建事件中心输入
2. 配置和启动事件生成器应用程序
3. 设置流分析作业
4. 指定作业输入和查询

然后，创建一个 Azure SQL 数据仓库数据库

## 指定作业输出︰ Azure SQL 数据仓库数据库

### 第 1 步
在流分析作业中单击顶部的页的**输出**，然后单击**添加输出**。

### 第 2 步
选择 SQL 数据库，然后单击下一步。
![][Add Output]

### 第 3 步
在下一个页面中输入以下值
- 输出别名︰ 输入此作业输出的友好名称。
- 订阅︰ 
    - 如果 SQL 数据仓库数据库在同一预订作为流分析作业中，选择使用 SQL 数据库从当前的订阅。
    - 如果您的数据库位于不同的订阅，从另一个订阅选择使用 SQL 数据库。
- 数据库︰ 指定目标数据库的名称。
- 服务器名称︰ 指定只是指定的数据库服务器名称。 您可以使用 Azure 门户发现这。

![][Server Name]

- 用户名︰ 指定帐户的用户名写入数据库的权限。
- 密码︰ 为指定的用户帐户提供密码。
- 表︰ 在数据库中指定目标表的名称。

![][Add Database] 

### 第 4 步
单击检查按钮以添加该作业输出并验证流分析可以成功地连接到数据库。

![][Test Connection]

在与数据库连接成功后，您将看到底部的门户的通知。 在底部来测试与数据库的连接，可单击 [测试连接。




## 下一步行动
集成的概述，请参阅[SQL 数据仓库集成概述][]。
开发的更多提示，请参阅[SQL 数据仓库开发概述][]。

<!--Image references-->
[添加输出]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[服务器名称]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[添加数据库]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[测试连接]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->
[Azure 流分析简介]: ./stream-analytics-introduction/
[获取已启动教程]: ./articles/stream-analytics-get-started/
[SQL 数据仓库开发概述]:  ./sql-data-warehouse-overview-develop/
[SQL 数据仓库集成概述]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
[Azure 流分析文档]: http://azure.microsoft.com/documentation/services/stream-analytics/

