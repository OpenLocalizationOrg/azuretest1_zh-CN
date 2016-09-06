---
ms.openlocfilehash: bc56c2b97464f94e21050d54f49b5714afebb1fe
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 Azure 数据工厂的常见方案" 
    description="了解有关使用 Azure 数据工厂服务了几种常见方案" 
    services="data-factory"     
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/07/2015" 
    ms.author="spelluru"/>

# 使用 Azure 数据工厂的常见方案
本部分介绍了几种示例方案 Azure 数据工厂可如今，支持并将继续以发展为中心的方案。

> [AZURE.NOTE] 通过这一阅读之前通读[Azure 数据工厂简介][datafactory 介绍]文章。   

##方案 1︰ 数据中心的数据源
![源数据中心][image-data-factory-introduction-secenario1-source-datahub]

企业有了完全不同的源中的不同类型的数据。  构建信息生产系统的第一步是连接到所有必需的源数据和处理，如 SaaS 服务的文件共享，FTP、 web 服务，并将根据需要进行后续的处理数据移动。

没有数据工厂，企业必须构建自定义数据移动组件或编写自定义的服务来集成这些数据源和处理。  这是昂贵，且难以集成和维护这样的系统，并且它通常缺少企业级监控和警报和完全托管的服务可以提供的控件。
  
使用 Azure 数据工厂数据存储和处理服务将收集到一个促进和优化计算和存储活动、 支持统一的资源消耗管理，以及为数据移动，根据需要提供服务的中心容器中。

##方案 2︰ 实施信息生产
Operationalization 方案是采购情况的数据之后的下一个逻辑步骤。 一旦数据中心里，要编写和实施数据管道，以可靠地产生转换的数据源具有受信任的数据的生产环境的可维护性、 可控制计划上。  在 Azure 数据工厂数据转换是通过配置单元、 小猪和自定义 C# 处理在 Hadoop (Azure HDInsight) 上运行。  这些变换可用于清除掩码关键数据字段的数据和其他数据执行操作中各种各样的复杂方式。  一般情况下，operationalization 实现具有复杂且难以维护基础结构和自定义服务，并提出了实施、 管理、 扩展、 故障排除和版本控制此类解决方案的挑战。
  
数据工厂作为完全托管的服务，用户可以通过定义它们的一次性或复杂的定期计划，实施这些管道，业务流程处理直接由数据工厂服务。  与集线器，群集管理的所有数据及处理集线器内部都处理代表用户，以便用户可以将精力上革命性分析基础结构管理。  Azure 数据工厂中移除使用脆弱的自定义服务的挑战，并使企业能够可靠和重复产生可靠的信息。


##方案 3︰ 将信息生产集成与数据发现
传统双方法和技术，同时提供"权威来源真实的"，几乎总是有严重的副作用︰ 由于小心控制的变更请求进程的请求不断积压。  为了适应快速变化的业务问题，提供了企业能够使用其信息消耗系统连接信息生产系统需要更大的灵活性。  Azure 数据工厂可帮助简化数据连接这些系统的困难管线信息生产和信息消耗挑战，从而能够轻松使用的窗体中提供最新的受信任的数据的地址。
  
Azure 数据工厂支持启用生成的数据的简单消耗的以下功能︰

- 轻松地移动 （一次或按预定时间） 到关系数据集市的消耗使用现有的 BI 工具生成的数据资产 (Excel、 Tableau 等。..)。
- 使用直接在 Excel 中使用电源查询的数据工厂所产生的数据资产。

请参阅使用数据使用电源查询以下主题︰ 

- [电源查询︰ 连接到 Microsoft Azure 表存储][电源查询 Azure 表]
- [电源查询︰ 连接到 Microsoft Azure Blob 存储][电源查询 Azure Blob]
- [电源查询︰ 连接到 Microsoft Azure SQL 数据库][电源查询 Azure SQL]
- [电源查询︰ 连接到 Microsoft SQL Server 内部][电源查询 OnPrem SQL] 


[电源查询 Azure 表]: http://office.microsoft.com/en-001/excel-help/connect-to-microsoft-azuretable-storage-HA104122607.aspx
[电源查询 Azure Blob]: http://office.microsoft.com/en-001/excel-help/connect-to-microsoft-azure-blob-storage-HA104113447.aspx
[电源查询 Azure SQL]: http://office.microsoft.com/en-001/excel-help/connect-to-a-microsoft-azure-sql-database-HA104019809.aspx
[电源查询 OnPrem SQL]: http://office.microsoft.com/en-001/excel-help/connect-to-a-sql-server-database-HA104019808.aspx

[复制的数据与 adf]: http://azure.microsoft.com/documentation/articles/data-factory-copy-activity/
[使用-猪-配置单元]: http://azure.microsoft.com/documentation/articles/data-factory-pig-hive-activities/
[运行图减少]: http://azure.microsoft.com/documentation/articles/data-factory-map-reduce/
[azure 的 ml-adf]: http://azure.microsoft.com/documentation/articles/data-factory-create-predictive-pipelines/

[msdn-存储-过程的活动]: https://msdn.microsoft.com/library/dn912649.aspx

[adf 教程]: data-factory-tutorial.md
[datafactory getstarted]: data-factory-get-started.md
[datafactory-简介]: data-factory-introduction.md

[image-data-factory-introduction-secenario1-source-datahub]:./media/data-factory-common-scenarios/Scenario1SourceDataHub.png

[image-data-factory-introduction-secenario2-operationalize-infoproduction]:./media/data-factory-common-scenarios/Scenario2-OperationalizeInformationProduction.png



 

测试
