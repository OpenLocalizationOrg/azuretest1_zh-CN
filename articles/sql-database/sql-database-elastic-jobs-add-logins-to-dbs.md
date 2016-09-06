---
ms.openlocfilehash: 92b8d20c3e6cf8ef3227b1f00f84e4f6b16c09a6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    title="How to add a users to an elastic database pool" 
    pageTitle="如何将用户添加到一个弹性的数据库池" 
    description="必须将具有权限的用户添加到池中的每个数据库" 
    metaKeywords="azure sql database elastic databases credentials" 
    services="sql-database" documentationCenter=""  
    manager="jeffreyg" 
    authors="sidneyh"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/27/2015" 
    ms.author="ddove; sidneyh" />

# 如何将用户添加到一个弹性的数据库池

（预览） 的**弹性数据库作业**功能可以跨一组包括**弹性数据库池**或**弹性数据库 shard 设置**Azure SQL 数据库中的数据库的自定义集合的数据库运行的事务处理 SQL 脚本。 若要运行该脚本，具有适当权限的用户必须添加到将在其中执行作业的每个数据库。 有关详细信息，请参阅[管理数据库和 SQL Azure 数据库中的登录名](https://msdn.microsoft.com/library/azure/ee336235.aspx)或[将用户添加到您的 SQL Azure 数据库](http://azure.microsoft.com/blog/2010/06/21/adding-users-to-your-sql-azure-database/)

## 先决条件
* 将[弹性作业组件](sql-database-elastic-jobs-service-installation.md)安装。 

## 如何将用户添加到数据库

1.  首次连接到**主机**的 SQL Azure 数据库服务器中弹性数据库池的数据库驻留在其中并创建新登录使用相同的凭据提供安装**弹性数据库作业**时。

        CREATE LOGIN login1 WITH password='<ProvidePassword>';

2. 登录到池中的每个数据库，并创建用户使用相同的名称和密码。 用户必须具有足够的权限来执行该作业。 必须在每个数据库运行此代码。

        CREATE USER admin1 FROM LOGIN login1;
        
3. 用户必须具有权限，也不足以执行指定的作业的脚本。 使用**sp_addrolemember**过程中为用户提供要执行成功的脚本所需的最低权限。 

## 下一步行动

创建和管理作业，请参阅[创建和管理弹性数据库作业](sql-database-elastic-jobs-create-and-manage.md)。

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-overview/elastic-jobs.png
<!--anchors-->

 