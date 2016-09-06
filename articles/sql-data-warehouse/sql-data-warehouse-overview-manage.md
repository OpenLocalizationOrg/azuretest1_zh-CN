---
ms.openlocfilehash: 4f4cceaae0a1b89f6eea5d97fb9f9588d6580da7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="SQL 数据仓库的管理工具 |Microsoft Azure"
   description="简介了 SQL 数据仓库的管理工具。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="HappyNicolle"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/24/2015"
   ms.author="mausher;nicw;barbkess;JRJ@BigBangData.co.uk;"/>

# SQL 数据仓库的管理工具
本主题介绍并比较工具和选项可用于管理 SQL 数据仓库，以便您可以针对您的需要选择合适的工具。 选择合适的工具取决于多少个数据库管理，任务，以及执行任务的频率。

## Azure 门户
[Azure 门户][]是一个基于 web 的管理门户中创建、 更新和删除数据库和监控数据库资源。 此工具非常重要如果您刚开始使用 Azure，管理少量的数据仓库数据库，或者需要快速执行一些。 

门户网站包括指标涵盖了当前和历史性能 DWU 设置、 正在使用的存储量、 成功和失败的 SQL 连接和一套可视化效果和使您了解您的实例和这些查询的详细信息上运行的查询的数据。  

## 在 Visual Studio 中的 SQL Server 数据工具   
[SQL Server 数据工具][](SSDT) 在 Visual Studio 中是一种客户端工具，在您的计算机上运行，并允许您连接、 管理和开发中云数据库。 如果您是应用程序开发人员熟悉 Visual Studio 或其他集成的开发环境 (Ide)，请在 Visual Studio 中使用 SSDT。 

SSDT 包括 SQL Server 浏览器，它使您能够可视化、 连接和执行针对 SQL 数据仓库数据库的脚本。 快速连接到 SQL 数据仓库，可以在 Azure 门户中查看数据库详细信息时只需单击命令栏中的**在 Visual Studio 中打开**按钮。  

您可以下载最新版本的[SQL Server 数据工具][](SSDT) 包括支持 SQL 数据仓库。

## 命令行工具
一个选项是使用 PowerShell 或 sqlcmd 命令行工具来管理 SQL 数据仓库并自动 Azure 的资源部署。 我们建议这些工具用于管理大量的逻辑服务器和部署在生产环境中的资源更改，可以编写脚本和然后自动化所需的任务。

## 下一步行动
若要启动[连接][]主题通过使用这些工具，头部。

<!--Image references-->

<!--Article references-->
[连接]: sql-data-warehouse-develop-connections.md

<!--MSDN references-->
[SQL Server 数据工具]: https://msdn.microsoft.com/en-us/library/mt204009.aspx

<!--Other web references-->
[Azure 门户]: http://portal.azure.com/