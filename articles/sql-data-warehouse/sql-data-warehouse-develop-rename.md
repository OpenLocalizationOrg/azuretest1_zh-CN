---
ms.openlocfilehash: 97ebb2ba62eecc1cb2f299097cfb1bc186ff4635
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="重命名 SQL 数据仓库中 |Microsoft Azure"
   description="为开发解决方案重新命名对象和 Azure SQL 数据仓库的数据库的提示。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/26/2015"
   ms.author="JRJ@BigBangData.co.uk;barbkess"/>

# 重命名 SQL 数据仓库中
SQL Server 支持对象，并分别通过存储的过程 sp_rename 和 sp_renamedb 重命名的数据库。

SQL 数据仓库使用 DDL 语法来达到同样目的。 DDL 命令会重命名对象和重命名数据库。

## 重命名对象

请务必了解名称仅影响重命名的对象-该名称更改不会传播。 因此在创建具有旧名称的对象之前，任何使用旧名称的对象的视图都将无效。 因此您通常会看到`RENAME OBJECT`成对出现。

```
RENAME OBJECT product.item to item_old;
RENAME OBJECT product.item_new to item;
```

## 重命名数据库

重命名数据库是对象的非常相似。 

```
RENAME DATABASE AdventureWorks TO Contoso;
```

请务必记住，您不能重命名某个对象或数据库是否其他用户连接到它，或者使用它。 您可能需要首先终止打开的会话。 要终止会话，您需要使用[KILL][]命令。 请小心使用 kill 命令时。 一次执行目标的会话将被终止，并将回滚任何未提交的工作。

> [AZURE.NOTE] SID 将需要包括该前缀在 SQL 数据仓库中的会话，会话编号时调用 KILL 命令。 例如 KILL SID1234 将终止会话 1234-假设您有适当的权限来执行它。

## 终止会话
若要重命名数据库可能需要断开会话连接到您的 SQL 数据仓库。 下面的查询将生成不同的 KILL 命令以清除连接 （保存当前会话的） 列表。

```
SELECT 'KILL '''+session_id+''''
FROM    sys.dm_pdw_exec_requests r
WHERE r.session_id <> SESSION_ID()
AND EXISTS
(   SELECT  *
    FROM    sys.dm_pdw_lock_waits w
    WHERE   r.session_id = w.session_id
)
GROUP BY session_id
;
```

## 交换架构
是否要更改架构对象所属的然后就是通过`ALTER SCHEMA`:

```
ALTER SCHEMA dbo TRANSFER OBJECT::product.item;
```


## 下一步行动
开发的更多提示，请参阅[开发概述][]。

<!--Image references-->

<!--Article references-->
[开发概述]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[终止]: https://msdn.microsoft.com/en-us/library/ms173730.aspx

<!--Other Web references-->
[Azure 的管理门户]: http://portal.azure.com/


