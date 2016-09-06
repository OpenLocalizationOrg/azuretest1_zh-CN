---
ms.openlocfilehash: 4f904b763ccd008634a0a06df630e1086614cd23
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="使用 SQL Azure 迁移向导 |Microsoft Azure" 
   description="Microsoft Azure SQL 数据库，数据库迁移、 导入数据库，导出数据库迁移向导" 
   services="sql-database" 
   documentationCenter="" 
   authors="pehteh" 
   manager="jeffreyg" 
   editor="monicar"/>


<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="07/01/2015"
   ms.author="pehteh"/>


# 如何︰ 使用 SQL Azure 迁移向导


![alt 文本](./media/sql-database-migration-wizard/01SAMWDiagram.png)


此选项使用 SQL Azure 迁移向导来生成 T SQL 脚本然后转换向导，请与 SQL 数据库，然后连接到 Azure SQL 数据库以针对空 SQL Azure 数据库执行脚本的源数据库中。 该脚本可以使用仅限架构生成或可以在 BCP 格式中包含的数据。 该向导还允许您以包括或排除特定的对象从脚本中的数据库。


请注意并不是所有向导可检测到不兼容的架构都可以通过其内置的转换。 不能解决的不兼容脚本将报告为错误，带有注释插入到生成的脚本中。 如果发生这种情况必须保存该脚本，并将其手动编辑，然后它才能提交到 SQL Azure 数据库中。 如果更改不需要脚本都可以保存和再编辑 SSMS 或刀具 VS，在 SQL Server 和一次兼容、 执行的带外针对 SQL Azure 数据库。 


> **注意︰**作为对此选项的扩展，如果检测到了很多错误，并纠正这些不是简单可以导入生成的脚本文件到 Visual Studio 中的数据库项目。 如果您将项目的目标设置为 SQL 数据库 V12 可以然后生成项目并逐渐解决使用 SQL Server 工具 VS 中的错误。 一旦架构中有任何错误，您可以发布架构的源数据库的一个副本，然后使用 SSMS 部署或导出/导入数据库与 SQL Azure 数据库选项 #1 中所述。


## 下载 SQLAzureMW.exe


您可以从 CodePlex 下载 SQL Azure 迁移向导︰


[下载 SQLAzureMW.exe](http://sqlazuremw.codeplex.com/)


## 迁移步骤


&nbsp;1.设置一个新的数据库，或者在 SQL 数据库中的新服务器或现有服务器升级如前面所述。 您将执行针对此数据库作为最后一步，此选项在创建的迁移脚本。 


&nbsp;2.打开迁移向导并选择****分析/迁移数据库选项将**目标服务器**设置为 Azure SQL 数据库 V12，然后单击**下一步**。


![alt 文本](./media/sql-database-migration-wizard/02MigrationWizard.png)


&nbsp;3.在下一个页面中，单击**连接到服务器**并提供源服务器的连接信息。 可以在 [连接] 对话框中指定的源数据库或连接到服务器，然后从数据库的列表中选择源数据库。


![alt 文本](./media/sql-database-migration-wizard/03MigrationWizard.png)


&nbsp;4.在下一页上，选择的对象，您可以选择是否要编写脚本的所有数据库对象 （默认值），或选择要为其编写脚本的特定数据库对象。 您可能会发现最适合用来启动脚本的第一个过程中的所有对象，然后可能回来到这一步并排除的对象，如果该向导将报告都脚本或转换错误。 该向导的工作原理是第一个脚本 （使用 SMO） 数据库中的对象，然后后过程所生成的脚本将应用一系列基于正则表达式的验证和转换规则。


![alt 文本](./media/sql-database-migration-wizard/04MigrationWizard.png)


&nbsp;5.选择高级并查看向导使用的配置选项。 对于第一次，尤其对于大的数据库，您应该的设置**脚本的表数据**复制到表架构，确保**目标服务器**设置为 Microsoft Azure SQL 数据库 V12 和**兼容性检查**设置重写为︰ 执行所有的兼容性检查。


![alt 文本](./media/sql-database-migration-wizard/05MigrationWizard.png)


&nbsp;6.单击**下一个**要检查的选项，然后**下一次**重新生成和转换脚本。 您可以查看 SQL 脚本选项卡上的脚本。


&nbsp;7.脚本生成将报告错误，如果架构与 SQL 数据库不兼容，无法进行转换向导。 在屏幕上拍摄下转换过程发现了一个存储过程的问题。 在这种情况下，已写入过程中要使用全文搜索不但是 V12 支持 （但它将在以后的版本中支持）。 


![alt 文本](./media/sql-database-migration-wizard/06MigrationWizard.png)


- 根据错误的评估，您可以返回向导并排除问题的根源，并重新生成脚本的对象。 当您考虑如何应对计划错误考虑对数据库使用数据库的应用程序中其他对象的影响。
- 如果该脚本具有需要更正的错误可以保存 SQL 脚本选项卡仅限于架构的脚本文件和打开并编辑该脚本以自己喜爱的编辑器来执行它在 SAMW 之外根据您在步骤 1 中创建新数据库之前更正错误。 如果该脚本很大或有大量错误，则应使用选项 3。 请注意，虽然可以将生成的脚本文件导入到 Visual Studio SQL 从导入文件可能会花相当多时间长于选项 3 所述数据库中导入。 


&nbsp;8.如果没有错误或去掉了源的错误从而可以单击**下一步**代，然后在下面的页上单击**连接到服务器**以连接到您在步骤 1 中创建的 SQL Azure 数据库服务器。


![alt 文本](./media/sql-database-migration-wizard/07MigrationWizard.png)


&nbsp;9.下一步是选择要对其执行该脚本的数据库。 将列出所有服务器上的现有数据库。 选择您在步骤 1 中创建的数据库。 数据库应该是空的并且有层此操作的相应定价。 
- 如果您愿意，您可以使用创建数据库此时向导。 为这样单击创建数据库配置和创建它。 选择要在迁移过程中使用的性能级别，请参阅指南的入门部分。
- 一旦创建了数据库选择并继续。 


&nbsp;10.建立一个空的数据库，选择单击**下一步**，确认您想要执行脚本以完成迁移。

 
