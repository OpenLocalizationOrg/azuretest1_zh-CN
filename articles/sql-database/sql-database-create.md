---
ms.openlocfilehash: f453dea0b0d8783e8123af20880588e3da919e9b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 SQL 数据库更新 V12 创建数据库" 
    description="演示如何在 Azure SQL 数据库更新 V12 创建数据库" 
    services="sql-database" 
    documentationCenter="" 
    authors="sonalmm" 
    manager="jeffreyg" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-management" 
    ms.date="04/28/2015" 
    ms.author="sonalm"/>


# 在 SQL 数据库 V12 创建数据库


<!--
True author is: authors="sonalmm" , ms.author="sonalm".
-->


为 SQL 数据库 V12 [（预览某些地区）](sql-database-v12-whats-new.md#V12AzureSqlDbPreviewGaTable)以利用 Microsoft Azure 上的 SQL 数据库的下一代的[注册](https://portal.azure.com)。 首先，需要 Microsoft Azure 订阅。 注册[免费试用的 Azure](http://azure.microsoft.com/pricing/free-trial)并查看[定价](http://azure.microsoft.com/pricing/details/sql-database)信息。 


| 创建数据库 | 屏幕抓图 |
| :--- | :--- |
| 1.登录到[http://portal.azure.com/](http://portal.azure.com/)。 | ![新的 Azure 门户][1] |
| 2.底部的页上，在左边，单击**新建**。 | ![启动新服务][2]|
| 3.单击**SQL 数据库**。| ![不同的服务，从选择][3] |
| 4.一个**SQL 数据库**叶片打开。 在**名称**字段中，指定一个数据库名称。 | ![命名数据库][4] |
| 5.在**SQL 数据库刀片式服务器**，单击**服务器**。 **服务器**刀片式服务器将打开一个提供两个选择︰ 既可以创建新的服务器，也可以使用现有的服务器。| ![选择的服务器的类型][4] |
|5a。 如果您选择**使用现有的服务器**选项，选择所选的服务器，单击**选择**。 然后，完成步骤 6 在病房中的所有操作。| ![从列表中选择一台服务器][5]| 
|5b。   如果您选择**创建一个新服务器**，请打开**新服务器**刀片式服务器。 指定服务器名称、 服务器管理登录和密码。 单击要选择的服务器位置的**位置**。 | ![完成创建新的服务器选项][9]| 
|5 c。**新服务器**刀片式服务器将为您提供创建使用 V12 更新新的服务器。 若要了解更多有关 V12 服务器的功能，查看[SQL 数据库 V12 中的新增功能是什么](sql-database-v12-whats-new.md)。| ![选择 V12 服务器][6]|
|5 天。 在**新的服务器**刀片式服务器中作出选择，并单击**确定**。 这样，就会返回到**SQL 数据库**刀片式服务器无法完成操作，以创建一个数据库的其余部分。 | ![完成新建服务器刀片式服务器操作][8]|
|6.单击**选择源**。 可以从选择要创建的数据库源的不同类型的︰ 一个空数据库，示例数据库或从数据库的备份。| ![选择源数据库][10]|
|7.下一步，在**SQL 数据库**刀片式服务器，请单击**定价层**。 您可以选择一种推荐的定价层或**查看所有**可用的定价层。 做出选择后，单击**选择**。 <p> 有关定价层的详细信息，请参阅[升级到新的服务层 SQL 数据库 Web/业务数据库](./sql-database-upgrade-new-service-tiers/)和[Azure SQL 数据库服务层和性能级别](http://msdn.microsoft.com/library/azure/dn741336.aspx)。 |![选择定价层][7]
| 8.接下来， **SQL 数据库**刀片中单击**可选配置**，进行选择，单击**确定**。 
| 9.当您选择现有的服务器，**资源组**和**预订**已选择为您。 在**SQL 数据库**刀片式服务器，您将看到旁边**的资源组**和**订阅**一个锁定的图标。 如果您创建一个新的服务器，然后您可以选择或创建资源组。 详细信息，请参阅[使用资源组来管理 Azure 资源。](resource-group-overview.md)|![指定资源组][11]
| 10.单击**创建**。 创建新的数据库与 SQL 数据库 V12 功能。 |![创建新的数据库][12]

## 相关的链接

- [在 SQL 数据库 V12 中的新增功能](sql-database-v12-whats-new.md)
- [计划和准备升级到 SQL Azure 数据库 V12](sql-database-v12-plan-prepare-upgrade.md)

<!--Image references-->
[1]: ./media/sql-database-create/firstscreenportal.png
[2]: ./media/sql-database-create/new.png
[3]: ./media/sql-database-create/sqldatabase.png
[4]: ./media/sql-database-create/databasename.png
[5]: ./media/sql-database-create/useexistingserver.PNG
[6]: ./media/sql-database-create/v12server.PNG
[7]: ./media/sql-database-create/pricingtierdetails.png
[8]: ./media/sql-database-create/finishnewserverblade.png
[9]: ./media/sql-database-create/createnewserver.png
[10]: ./media/sql-database-create/selectsource.png
[11]: ./media/sql-database-create/resourcegroup.png
[12]: ./media/sql-database-create/create.png

 