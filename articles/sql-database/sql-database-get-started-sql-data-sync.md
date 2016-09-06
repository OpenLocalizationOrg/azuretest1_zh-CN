---
ms.openlocfilehash: a23af831638322005a0522436152056b9f9e6064
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="要开始使用 SQL 数据库的数据同步"
    description="本教程将帮助您开始使用 Azure SQL 数据同步 （预览）。"
    services="sql-database"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/04/2015"
    ms.author="spelluru"/>


#要开始使用 SQL Azure 数据同步 （预览）
在本教程中，您了解了最基本的 SQL Azure 的数据同步使用 Azure （预览） 门户。


本教程假定您使用 SQL Server 和 SQL Azure 数据库最小经验。 在本教程中，您可以创建完全配置并根据您设置的计划同步的混合 （SQL Server 和 SQL 数据库实例） 同步组。


## 步骤 1︰ 连接到 SQL Azure 数据库

1. 登录到[管理门户](http://manage.windowsazure.com)。

2. 在左窗格中，单击**SQL 数据库**。

3. 单击页面底部的**同步**。 单击同步，将显示一个列表显示您可以添加的内容**新的同步组**和**新的同步代理**。

4. 若要启动新的 SQL 数据同步代理程序向导，请单击**新同步代理**。

5. 如果您没有添加代理程序之前，**请单击此处下载**。

    ![Image1](./media/sql-database-get-started-sql-data-sync/SQLDatabaseScreen-Figure1.PNG)


## 步骤 2︰ 添加客户端代理
仅当您要将内部的 SQL Server 数据库包含在同步组中，则需要此步骤。 您可以跳到第 4 步︰ 创建一个同步组，如果同步组有仅 SQL 数据库实例。

<a id="InstallRequiredSoftware"></a>
### 步骤 2a︰ 安装必需的软件
请确保您已安装了以下产品的计算机上安装了客户端代理。

- **.NET Framework 4.0**

 您可以从[这里](http://go.microsoft.com/fwlink/?linkid=205836)安装.NET Framework 4.0。

- **Microsoft SQL Server 2008 R2 SP1 系统 CLR 类型 (x86)**

 您可以从[这里](http://www.microsoft.com/download/en/details.aspx?id=26728)安装的 Microsoft SQL Server 2008 R2 SP1 系统 CLR 类型 (x86)

- **Microsoft SQL Server 2008 R2 SP1 共享管理对象 (x86)**

 您可以从[这里](http://www.microsoft.com/download/en/details.aspx?id=26728)安装 Microsoft SQL Server 2008 R2 SP1 共享管理对象 (x86)



<a id="InstallClient"></a>
### 第 2b 步︰ 安装新的客户端代理

按照说明[安装客户端代理 （SQL 数据同步）](http://msdn.microsoft.com/library/jj823137.aspx)在安装代理。



<a id="RegisterSSDb"></a>
### 步骤 2 c︰ 完成新 SQL 数据同步代理向导

1.  返回新的 SQL 数据同步代理向导。
2.  为代理提供有意义的名称。
3.  从下拉列表中，选择的**地区**（数据中心） 来承载此代理。
4.  从下拉列表中，选择**订阅**此代理程序的宿主。
5.  单击向右箭头。



## 步骤 3︰ 使用客户端代理注册的 SQL Server 数据库

安装了客户端代理后，注册您要包括在同步组与代理的每个内部部署 SQL Server 数据库。
若要向代理注册数据库，按照在[注册客户端代理与 SQL Server 数据库](http://msdn.microsoft.com/library/jj823138.aspx)的说明。



## 步骤 4︰ 创建一个同步组


<a id="StartNewSGWizard"></a>
### 在步骤 4a︰ 启动新的同步组向导

1.  返回[管理门户](http://manage.windowsazure.com)。
2.  单击**SQL 数据库**。
3.  单击页面底部的**添加同步**，然后从抽屉中选择新的同步组。

    ![Image2](./media/sql-database-get-started-sql-data-sync/NewSyncGroup-Figure2.png)



<a id=""></a>
### 步骤 4b︰ 输入的基本设置


1.  输入同步组有意义的名称。
2.  从下拉列表中，选择的**地区**（数据中心） 来承载此同步组。
3. 单击向右箭头。

    ![Image3](./media/sql-database-get-started-sql-data-sync/NewSyncGroupName-Figure3.PNG)


<a id="DefineHubDB"></a>
### 步骤 4 c︰ 定义同步中心

1. 从下拉列表中，选择要用作同步组中心的 SQL 数据库实例。
2. 此 SQL 数据库实例的**集线器用户名**和**集线器密码**中输入的凭据。
3. 等待确认用户名和密码的 SQL 数据同步。 凭据被确认时，则将出现一个绿色的复选标记右侧的密码。
4. 从下拉列表中选择的**冲突解决**策略。

 **集线器获胜**的向中心数据库中写入任何更改写入到参考数据库，覆盖相同的引用数据库记录中的更改。 就功能而言，这意味着写入到该中心的第一个更改将传播到其他数据库。


 **Wins 客户端**的更改写入到该中心将引用数据库中的更改所覆盖。 就功能而言，这意味着上次更改写入到该中心是一个保存并传播到其他数据库。

5.  单击向右箭头。

    ![Image4](./media/sql-database-get-started-sql-data-sync/NewSyncGroupHub-Figure4.PNG)

<a id="AddRefDB"></a>
### 步骤 4 d︰ 添加参考数据库


为您想要添加到同步组中每个其他数据库重复此步骤。

1. 从下拉列表中，选择要添加的数据库。

    在下拉列表中的数据库包含两个已注册的代理和 SQL 数据库实例的 SQL Server 数据库。
2.  请输入此数据库的**用户名**和**密码**凭据。
3.  从下拉列表中选择该数据库的**同步方向**。

    **双向语言**的引用数据库中的更改写入到中心数据库中，并对中心数据库的更改写入到参考数据库。

    **从集线器同步**的数据库从该集线器接收更新。 不，它不会将更改发送到中心。

    **同步到中心**的数据库将更新发送到该中心。 中心不会写入到此数据库。

4.  要完成创建的同步组中，单击右下角的向导中的复选标记。 等待在 SQL 数据同步，以确认凭据。 绿色复选标记表示凭据被确认。

5.  第二次单击该复选标记。 此操作将返回到 SQL 数据库下**同步**页面。 与其他同步组和代理现在列出此同步组。

    ![Image5](./media/sql-database-get-started-sql-data-sync/NewSyncGroupReference-Figure5.PNG)


## 第 5 步︰ 定义要同步的数据

Azure SQL 数据同步允许您选择要同步表和列。 如果您还想要筛选列，以便各仅行与特定的值 (如︰ 年龄 > = 65) 是同步，在 Azure 使用 SQL 数据同步门户和在文档中选择的表、 列和行同步定义的数据进行同步。

1.  返回[管理门户](http://manage.windowsazure.com)。
2.  单击**SQL 数据库**。
3.  单击**同步**选项卡。
4.  单击此同步组的名称。
5.  单击**同步规则**选项卡。
6.  选择您想要提供的同步组架构的数据库。
7.  单击向右箭头。
8.  单击**刷新架构**。
9.  对于每个数据库表中，选择要包括在同步中的列。
    - 不支持的数据类型的列不能选择。
    - 如果未不选择任何表中的列，表不包含在同步组中。
    - 选择/取消选择所有表，请都单击屏幕底部的选择。
10. 单击**保存**，然后等待同步组完成资源调配。
11. 若要返回到数据同步登陆页面，请单击 （高于同步组的名称） 在屏幕的左上角的后退箭头。

    ![Image6](./media/sql-database-get-started-sql-data-sync/NewSyncGroupSyncRules-Figure6.PNG)

## 第 6 步︰ 配置同步组

通过单击同步数据同步登陆页面的底部，您可以始终同步同步组。
如果您希望按计划同步的同步组，您可以配置同步组。

1.  返回[管理门户](http://manage.windowsazure.com)。
2.  单击**SQL 数据库**。
3.  单击**同步**选项卡。
4.  单击此同步组的名称。
5.  单击**配置**选项卡。
6.  **自动同步**
    - 若要配置上设置的频率同步的同步组，单击**开**。 您仍然可以通过单击同步同步点播。
    - 单击**关闭**以同步组同步仅当您单击同步配置。
7.  **同步频率**
    - 如果在自动同步，则设置同步的频率。 频率必须介于 5 分钟到 1 个月。
8.  单击**保存**。

![Image7](./media/sql-database-get-started-sql-data-sync/NewSyncGroupConfigure-Figure7.PNG)

祝贺您。 您已创建一个包含 SQL 数据库实例和一个 SQL Server 数据库的同步组。

## 下一步行动
有关 SQL 数据库和 SQL 数据同步请参阅详细信息︰

* [在 MSDN Library 的 SQL 数据同步内容](https://msdn.microsoft.com/library/azure/hh456371.aspx)
* [SQL 数据库概述](sql-database-technical-overview.md)
* [数据库生命周期管理](https://msdn.microsoft.com/library/jj907294.aspx)
 

 