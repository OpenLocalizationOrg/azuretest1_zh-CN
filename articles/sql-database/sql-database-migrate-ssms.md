---
ms.openlocfilehash: e30123488a9bc02cf9fb12baf4a0e82c8faf0f58
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="迁移到 SQL 数据库使用 SSMS"
   description="Microsoft Azure SQL 数据库，sql 数据库迁移，迁移使用 ssms"
   services="sql-database"
   documentationCenter=""
   authors="carlrabeler"
   manager="jeffreyg"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="08/24/2015"
   ms.author="carlrab"/>

#迁移数据库使用 SSMS 兼容

![SSMS 迁移图](./media/sql-database-migrate-ssms/01SSMSDiagram.png)

如果数据库架构与 SQL Azure 数据库兼容，迁移只需要数据库被导入到 Azure。 这可以是单步通过 SSMS 首先导出数据库 BACPAC，然后将该 BACPAC 导入新数据库作为 Azure SQL Server 将数据库部署到 SQL Azure 数据库或作为一个两步过程。

当您导出 BACPAC 时，可以导出它到本地文件或直接到 Azure 的 blob。 如果本地导出，您可以将导出的 BACPAC 上载到 Azure 的 blob。 一旦在 Azure blob 存储 BACPAC 文件，然后可以作为数据库使用 Azure 门户导 BACPAC。 在 Azure 门户运行导入将减少延迟导入步骤，这将提高性能和可靠性与大型数据库的迁移。

直接通过 SSMS 来部署将始终部署整个架构和所有数据，而导出使用 BACPAC 允许您选择要导出到 BACPAC 的对象子集。 导出的 BACPAC 总是包括完整的数据库架构并且默认情况下，所有表中的数据。 是否您部署从 SSMS 或导出，然后导入 SSMS （或 Azure 门户） 从相同的 DAC 技术用在汽车发动机罩下，而且结果是一样的。

此选项还用作选项 #2 中的最后一步，以将数据库迁移后已将其更新以使其与 SQL Azure 数据库兼容。

## 获取最新版本的 SQL Server 管理工作室

使用最新版本的 Microsoft SQL Server 管理的 SQL Server 的 Studio 以确保您具有最新的更新 SSMS 中的工具和互动 Azure 的门户。 若要获得 SQL Server，[下载](https://msdn.microsoft.com/library/mt238290.aspx)最新版本的 Microsoft SQL Server 管理工作室，与您计划对迁移和互联网的数据库连接的客户端计算机上安装它。

##使用 SSMS 将部署到 SQL Azure 数据库
1.  设置使用 Azure 管理门户的逻辑服务器。
2.  SSMS 对象资源管理器中找到源数据库并执行该任务，**导出数据层应用程序...**

    ![从任务菜单将部署到 Azure](./media/sql-database-migrate-ssms/02MigrateusingSSMS.png)

3.  在部署向导中，配置到您在步骤 1 中设置的目标 SQL Azure 数据库逻辑服务器的连接。
4.  提供数据库**名称**并设置**版**（服务层） 和**服务目标**（性能标准）。 配置这些设置的详细信息，请参阅[SQL Azure 数据库服务层](sql-database-service-tiers.md)。

    ![导出设置](./media/sql-database-migrate-ssms/03MigrateusingSSMS.png)

5.  完成该向导以将数据库迁移。  
根据大小和数据库的复杂性，部署可能需要几分钟到几小时。 如果发现不兼容情况，架构验证例程将快速检测错误任何部署到 Azure 实际发生之前。 如果错误表示数据库架构不兼容的 SQL 数据库，可以使用迁移选项 #2。 请参阅[更新数据库中的位置，然后将部署到 SQL Azure 数据库。](sql-database-migrate-visualstudio-ssdt.md)

##使用 SSMS 来导出 BACPAC，然后将其导入到 SQL 数据库
部署过程可以分为两个步骤︰ 导出和导入。 在第一步，然后使用作为第二步中输入创建一个 BACPAC 文件。

1.  设置使用 Azure 管理门户的逻辑服务器。
2.  SSMS 对象资源管理器中找到源数据库，然后选择该任务，请**将数据库部署到 Azure SQL 数据库...**

    ![从任务菜单中导出数据层应用程序](./media/sql-database-migrate-ssms/04MigrateusingSSMS.png)

3. 在导出向导中，配置保存到本地磁盘位置或 BACPAC 文件的导出 Azure 的 blob。 导出的 BACPAC 总是包括完整的数据库架构并且默认情况下，所有表中的数据。 如果您希望排除某些或所有表的数据，请使用高级选项卡。 例如，可以选择若要仅导出数据的引用表。

    ![导出设置](./media/sql-database-migrate-ssms/05MigrateusingSSMS.png)

4.  一旦创建 BACPAC，连接到您在步骤 1 中创建的服务器，右键单击**数据库**文件夹，单击**导入数据层应用程序...**
    
    >[AZURE.NOTE] 注意︰ 您可能还导入存储在 Azure 中的 BACPAC 文件从 Azure 管理门户中的 blob。

    ![导入数据层应用程序的菜单项](./media/sql-database-migrate-ssms/06MigrateusingSSMS.png)

5.  在导入向导中，选择您刚导出 SQL Azure 数据库中创建新数据库的 BACPAC 文件。

    ![导入设置](./media/sql-database-migrate-ssms/07MigrateusingSSMS.png)

6.  提供数据库名称并设置版 （服务层） 和服务目标 （性能标准）。

7.  完成向导，将 BACPAC 文件导入，并在 SQL Azure 数据库中创建该数据库。

    ![数据库设置](./media/sql-database-migrate-ssms/08MigrateusingSSMS.png)

##备选方案
您还可以使用命令行实用程序 sqlpackage.exe 来部署数据库导出和导入或 BACPAC。 Sqlpackage.exe 使用相同的 DAC 技术作为 SSMS，因此结果是相同。 有关详细信息，请参阅[MSDN 上的 SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx)。
