---
ms.openlocfilehash: 56ee95d0331ecdaa89915de8073b01c3d86314ec
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
为了支持移动服务的脱机功能，我们使用`IMobileServiceSyncTable`接口，并初始化`MobileServiceClient.SyncContext`与本地存储。 在这种情况下，本地存储时 SQLite 数据库。

移动服务的普通的 CRUD 操作工作应用程序仍处于连接状态，但对本地存储区中发生的所有操作。

当我们想要与服务器同步本地存储时，我们使用`IMobileServiceSyncTable.PullAsync`和`MobileServiceClient.SyncContext.PushAsync`的方法。

*  若要将更改推送到服务器，我们称为`IMobileServiceSyncContext.PushAsync()`。 此方法是的成员`IMobileServicesSyncContext`而不是在同步表，因为它将会使跨所有表的更改。

    将会以某种方式 （通过 CUD 操作） 本地已修改的记录发送到服务器。
   
* 要拉到应用程序服务器上的表中的数据，我们称为`IMobileServiceSyncTable.PullAsync`。

    拉总是首先发出推。 这是为了确保关系以及本地存储区中的所有表都保持一致。

    也有的重载`PullAsync()`，允许要筛选的数据存储在客户端上指定的查询。 如果不传递查询，`PullAsync()`将拉在相应的表 （或查询） 中的所有行。 可以通过查询来筛选只更改您的应用程序需要与同步。

* 若要启用增量同步，将传递查询 ID 为`PullAsync()`。 查询 ID 用于存储从最后一次提取操作的结果的最后一次更新时间戳。 查询 ID 应该是唯一的应用程序中每个逻辑查询的描述性字符串。 如果查询中有一个参数，然后相同的参数值必须是一部分的查询 id。

    例如，如果您正在筛选的用户 id，它需要是查询 ID 的一部分︰

        await PullAsync("todoItems" + userid, syncTable.Where(u => u.UserId = userid));

    如果您想要抛弃增量同步，通过`null`作为查询 id。 在这种情况下，将检索所有记录在每次调用`PullAsync`，这是有可能效率低下。

* 手机信息服务数据库中删除后，请从设备的本地存储区删除记录，则应启用软删除。 否则，您的应用程序应定期调用`IMobileServiceSyncTable.PurgeAsync()`清除本地存储区。
