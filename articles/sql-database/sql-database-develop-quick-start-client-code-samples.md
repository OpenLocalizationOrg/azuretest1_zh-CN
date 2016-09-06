---
ms.openlocfilehash: 6d277ec3d4c6c037803b062e3e85feb3a6d2bc58
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="客户端快速启动代码示例 SQL 数据库 |Microsoft Azure" 
    description="在 Linux 上的 Node.js，Python Mac OS、 Java 和 Windows、 企业库和许多其他所有的 SQL Azure 数据库客户端提供代码示例和驱动程序。"
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jeffreyg" 
    editor=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="genemi"/>


# 对 SQL 数据库的客户端快速启动代码示例


本主题提供链接，以快速启动代码示例可用于连接到 SQL Azure 数据库︰


- 简短的示例连接和查询。
- 重试样本连接和查询，但如果遇到的错误归为[*瞬时性故障*](sql-database-develop-error-messages.md#bkmk_connection_errors)（如连接超时） 自动重试。


这些示例包括︰


- 各种不同的编程语言。
- Windows、 Linux、 并为您的客户端程序可以运行在操作系统 Mac OS。
- 下载所有必需的连接的驱动程序的链接。
- 短的快速入门代码示例。
- 包含处理自动重试逻辑的形式的瞬态故障的更长的时间样本。
- 代码示例的关系结果集转换为对象面向格式。


> [AZURE.NOTE] 准备的更多语言的代码示例，并对它们的链接将添加到该主题。


## 在 Linux 上的客户端


本节提供指向代码示例的主题的链接，在 Linux 上运行的客户端程序。


| 语言 | 简短的示例 | 重试的示例 | 对象关系 |
| :-- | :-- | :-- | :-- |
| Node.js | [单调乏味](sql-database-develop-nodejs-simple-linux.md) | . | . |
| Python | [FreeTDS pymssql](sql-database-develop-python-simple-unbutu-linux.md) | . | . |
| 拼音 | [FreeTDS TinyTDS](sql-database-develop-ruby-simple-linux.md) | . | . |


## 在 Mac OS 上的客户端


本章节提供在 Mac OS 运行的客户端程序代码示例的主题的链接。


| 语言 | 简短的示例 | 重试的示例 | 对象关系 |
| :-- | :-- | :-- | :-- |
| Python | [pymssql](sql-database-develop-python-simple-mac-osx.md) | . | . |
| 拼音 | [Homebrew<br/>FreeTDS TinyTDS](sql-database-develop-ruby-simple-mac-osx.md) | . | . |


## 在 Windows 上的客户端


本节提供有关在 Windows 运行的客户端程序代码示例的主题的链接。


| 语言 | 简短的示例 | 重试的示例 | 对象关系 |
| :-- | :-- | :-- | :-- |
| C# | [ADO.NET](sql-database-develop-dotnet-simple.md) | [ADO.NET 的自定义](sql-database-develop-csharp-retry-windows.md)<br/><br/>[企业库](sql-database-develop-entlib-csharp-retry-windows.md) | （实体框架） |
| C + + | [ODBC 驱动程序](http://msdn.microsoft.com/library/azure/hh974312.aspx) | . | . |
| Java | [Java。 JDBC，JDK。 插入、 交易记录，选择。](sql-database-develop-java-simple-windows.md)<br/><br/>[Java。 Eclipse](sql-data-java-how-to-use-sql-database.md)<br/><br/>[Java。 JDBC](http://msdn.microsoft.com/library/azure/gg715284.aspx) | . | . |
| Node.js | [msnodesql](sql-database-develop-nodejs-simple-windows.md) | . | . |
| PHP | [ODBC](sql-database-develop-php-simple-windows.md) | [自定义 ODBC](sql-database-develop-php-retry-windows.md) | . |
| Python | [pymssql](sql-database-develop-python-simple-windows.md) | . | . |


## 请参见


- [下载 Sdk 和工具，为各种语言和平台](http://azure.microsoft.com/downloads/#cmd-line-tools)
- [对于 SQL 数据库和 SQL Server 连接库](sql-database-libraries.md)
- [瞬态错误的数字代码的列表](sql-database-develop-error-messages.md#bkmk_connection_errors)<br/>&nbsp;
- [SQL azure 数据库开发︰ 帮助主题](http://msdn.microsoft.com/library/azure/ee621787.aspx)
- [连接到 SQL 数据库︰ 链接、 最佳做法和设计指南](sql-database-connect-central-recommendations.md)
- [创建第一个 SQL Azure 数据库](sql-database-get-started.md)
- [实体框架在这里，6 EF 7 日在 GitHub 上](http://entityframework.codeplex.com/)

