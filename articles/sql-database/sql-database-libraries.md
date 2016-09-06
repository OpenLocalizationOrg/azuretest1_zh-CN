---
ms.openlocfilehash: a479077bf7cc2534434684bc7b4b907a10039207
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="对于 SQL 数据库和 SQL Server 连接库"
    description="列出客户端程序可以用来连接到 SQL Azure 数据库或 Microsoft SQL Server 的每个驱动程序的最小版本号。 有关社区，而不是由 Microsoft 发布的驱动程序的版本信息的情况下提供的链接。"
    services="sql-database"
    documentationCenter=""
    authors="pehteh"
    manager="jeffreyg"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management" 
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/24/2015"
    ms.author="pehteh"/>


# 对于 SQL 数据库和 SQL Server 连接库


本主题列出每个库/驱动程序连接到 SQL Azure 数据库或 Microsoft SQL Server 时，客户端程序可以使用的最小版本号。


本主题被分为两个部分︰


- *由 Microsoft 发布的驱动程序库*-介绍 Microsoft 已发布了的库。 Microsoft 保留在这一节中的信息。
- *第三方库*-列出进行发布和维护由第三方，而不是由 Microsoft 的库。 **只有公共社区的开发人员维护这一节。 Microsoft 将不保留此节。**


## Microsoft 发布的驱动程序库的表


下表显示由 Microsoft 发布的库。 **库**栏提供了可用于每个库的下载的链接。 **版本**列列出了建议与 SQL Azure 数据库和 Microsoft SQL Server 进行交互的最低版本。


| 平台 | 工序系统 | 库<br/>下载 | 版本<br/>驱动程序 | 说明<br/>驱动程序 | 更多<br/>信息 |
| :--- | :--- | :--- | :--- | :--- | :-- |
| .NET | 跨平台 (.NET) | [ADO.NET，系统。数据。SqlClient](http://www.microsoft.com/download/details.aspx?id=30653) | 4.5 + | 对于.NET Framework 的 SQL Server 提供程序 | . |
| PHP | Windows | [SQL Server 的 PHP](http://www.microsoft.com/download/details.aspx?id=20098) | 2.0 + | SQL Server 的 PHP 驱动程序 | [链接](http://msdn.microsoft.com/library/dn865013.aspx) |
| Java | Windows | [SQL Server 的 JDBC](https://www.microsoft.com/download/details.aspx?id=11774) | 2.0 + |  类型 4 JDBC 驱动程序提供标准的 JDBC API 通过数据库连接 | [链接](http://msdn.microsoft.com/library/dn425070.aspx) |
| ODBC | Windows | [对于 SQL Server ODBC](http://www.microsoft.com/download/details.aspx?id=36434) | 11.0 + | SQL Server 的 Microsoft ODBC 驱动程序 | [链接](http://msdn.microsoft.com/library/jj730308.aspx) |
| ODBC | Suse Linux | [对于 SQL Server ODBC](http://www.microsoft.com/download/details.aspx?id=34687) | 11.0 + | SQL Server 的 Microsoft ODBC 驱动程序 | . |
| ODBC | Redhat Linux | [对于 SQL Server ODBC](http://www.microsoft.com/download/details.aspx?id=34687) | 11.0 + | SQL Server 的 Microsoft ODBC 驱动程序 | . |


### OLEDB 用于 DB2 和 SQL Server，DRDA 设计


Microsoft OLE DB 提供程序对于 DB2 版本 5.0 （数据提供程序），可以创建面向 IBM DB2 数据库的分布式应用程序。 该数据提供器利用 Microsoft SQL Server 数据访问以及作为分布式关系数据库结构 (DRDA) 应用程序请求者的 DB2 的 Microsoft 网络客户端体系结构。 该数据提供器将 Microsoft 组件对象模型 (COM) OLE DB 命令和数据类型转换为 DRDA 协议代码数据点和数据格式。


有关详细信息，请参阅︰


- [对于 DB2 版本 5.0 的 Microsoft OLE DB 提供程序](http://msdn.microsoft.com/library/dn745875.aspx)
- [对于 DB2 v4.0 为 Microsoft SQL Server 2012年的 Microsoft OLEDB 提供程序](http://www.microsoft.com/download/details.aspx?id=29100)


## 第三方函数库


> [AZURE.IMPORTANT] 下表显示发布的第三方许可条款的第三方的库。 您负责检验并遵守相关的第三方许可证才能使用这些库。 要使用这些库的风险。 Microsoft 不作任何担保，明示或暗示的担保，此处提供的信息，仅仅有至于便捷的信息提供给用户。 此处没有任何暗示任何一种由 Microsoft 的认可。
<br/><br/>由公共社区的开发人员可以更新和维护是在"第三方库"部分中，使用所拥有的**Azure**上 GitHub.com [azure 内容](http://github.com/Azure/azure-content/)存储库的信息。 Microsoft 鼓励开发人员可以更新此部分。 Microsoft 员工不要*不*打算维护的信息，在此节中，部分原因是因为其他人都更专业与每个特定第三方库不是我们。  谢谢。


下表显示由第三方，如其它公司或社区发布的库。 Microsoft 发布的库只限于在本主题中前面的一节。


| 平台 | 库 |
| :-- | :-- |
| Python | [pymssql *（组织结构图，稳定）*](http://pymssql.org/en/stable/)<br/><br/>[pymssql *（组织）*](http://pymssql.org/) |
| Node.js | [单调乏味的*(npmjs)*](http://www.npmjs.com/package/tedious) |
| Node.js | [节点-MSSQL *（github，patriksimek）*](https://github.com/patriksimek/node-mssql)<br/><br/>[节点-MSSQL *(npmjs)*](https://www.npmjs.com/package/node-mssql)<br/><br/>[节点-MSSQL 连接器*(npmjs)*](https://www.npmjs.com/package/node-mssql-connector) |
| Node.js | [Edge.js *(github com，tjanczuk)*](https://github.com/tjanczuk/edge)<br/><br/>[Edge.js *(tjanczuk，github io)*](http://tjanczuk.github.io/edge/) |
| . | [FreeTDS *（组织）*](http://www.freetds.org/) |


<!--
https://en.wikipedia.org/wiki/Draft:Microsoft_SQL_Server_Libraries/Drivers
-->
 