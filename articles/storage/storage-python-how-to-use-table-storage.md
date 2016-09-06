---
ms.openlocfilehash: 9fc3ba259dbd66aca947dda8425fc24f874bfda6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用 Python 中的表存储 |Microsoft Azure"
    description="了解如何使用 Python 中的表服务，可以创建和删除表，并可插入和查询的表。"
    services="storage"
    documentationCenter="python"
    authors="emgerner-msft"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="08/25/2015"
    ms.author="emgerner"/>


# 如何使用 Python 中的表存储

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]

## 概述

本指南介绍了如何使用 Azure 表存储服务来执行常见的方案。 这些示例用 Python 编写和使用[Python Azure 存储软件包][]。 已覆盖的方案包括创建和删除表，以及插入和查询表中的实体。

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.NOTE] 如果您需要安装 Python 或者[Python Azure 包][]，请参阅[Python 安装指南 》](../python-how-to-install.md)。


## 创建表

**TableService**对象使您可以使用表格服务。 下面的代码创建一个**TableService**对象。 添加您希望以编程方式访问 Azure 存储在其中任何 Python 文件的顶部附近的代码︰

    from azure.storage.table import TableService, Entity

下面的代码通过使用存储帐户的用户名和帐户密码创建一个**TableService**对象。  真正的帐户和密钥替换我的帐户和 mykey 派生而来。

    table_service = TableService(account_name='myaccount', account_key='mykey')

    table_service.create_table('tasktable')

## 向表中添加实体

若要添加实体，首先创建定义您的实体属性名称和值的字典。 请注意，对于每个实体，您必须指定**PartitionKey**和**RowKey**。 这些都是您的实体的唯一标识符。 您可以查询这些值可以查询您的其他属性比快得多。 系统使用**PartitionKey**表实体自动分布多个存储节点。
实体具有相同的**PartitionKey**存储在同一个节点上。 **RowKey**是实体的它所属的分区中的唯一 ID。

若要将实体添加到表中，词典将对象传递给**插入\_实体**方法。

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the trash', 'priority' : 200}
    table_service.insert_entity('tasktable', task)

您还可以传递到**实体**类的一个实例**插入\_实体**方法。

    task = Entity()
    task.PartitionKey = 'tasksSeattle'
    task.RowKey = '2'
    task.description = 'Wash the car'
    task.priority = 100
    table_service.insert_entity('tasktable', task)

## 更新实体

此代码演示如何用一个更新版本替换旧版本的现有实体。

    task = {'description' : 'Take out the garbage', 'priority' : 250}
    table_service.update_entity('tasktable', 'tasksSeattle', '1', task)

如果不存在要更新的实体，然后更新操作将失败。 如果您想要存储的实体，而不管其是否存在之前，请使用**插入\_或\_replace_entity**。
在以下示例中，第一次调用将替换现有的实体。 第二次调用将插入新的实体，由于没有实体与指定**PartitionKey**和**RowKey**表中存在。

    task = {'description' : 'Take out the garbage again', 'priority' : 250}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task)

    task = {'description' : 'Buy detergent', 'priority' : 300}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '3', task)

## 更改一组实体

有时它意义提交以确保原子由服务器处理批处理中的放在一起的多个操作。 若要实现这一点，请使用**开始\_批次** **TableService** ，然后调用的系列照常操作方法。 如果要提交的批处理，您可以调用**提交\_批次**。 注意要更改作为批处理所有实体必须都是在同一分区中。 下面的示例将两个实体中一批相加。

    task10 = {'PartitionKey': 'tasksSeattle', 'RowKey': '10', 'description' : 'Go grocery shopping', 'priority' : 400}
    task11 = {'PartitionKey': 'tasksSeattle', 'RowKey': '11', 'description' : 'Clean the bathroom', 'priority' : 100}
    table_service.begin_batch()
    table_service.insert_entity('tasktable', task10)
    table_service.insert_entity('tasktable', task11)
    table_service.commit_batch()

## 查询实体

要查询一个表中的一个实体，请使用**获得\_实体**通过**PartitionKey**和**RowKey**的方法。

    task = table_service.get_entity('tasktable', 'tasksSeattle', '1')
    print(task.description)
    print(task.priority)

## 查询的实体集

下面的示例查找所有任务在西雅图都基于**PartitionKey**。

    tasks = table_service.query_entities('tasktable', "PartitionKey eq 'tasksSeattle'")
    for task in tasks:
        print(task.description)
        print(task.priority)

## 查询实体属性的子集

对一个表的查询可以检索几个属性从一个实体。
这种技术，称为*投影*，减少了带宽，并可以提高查询性能，尤其是对于大型实体。 使用**选择**的参数，并传递要将到客户端的属性的名称。

下面的代码中的查询将返回表中仅实体的说明。

[AZURE.NOTE] 下面的代码段只有不利于云存储服务。 这不是支持存储仿真器。

    tasks = table_service.query_entities('tasktable', "PartitionKey eq 'tasksSeattle'", 'description')
    for task in tasks:
        print(task.description)

## 删除实体

您可以通过分区和行键来删除某个实体。

    table_service.delete_entity('tasktable', 'tasksSeattle', '1')

## 删除表

下面的代码从存储帐户中删除表。

    table_service.delete_table('tasktable')

## 下一步行动

现在，您已经学习了表存储的基本知识，按照这些链接以了解更复杂的存储任务︰

-   请参阅 MSDN 参考[Azure 存储][]。
-   访问[Azure 存储团队博客][]。

[Azure 存储]: http://msdn.microsoft.com/library/azure/gg433040.aspx
[Azure 存储团队博客]: http://blogs.msdn.com/b/windowsazurestorage/
[Python Azure 包]: https://pypi.python.org/pypi/azure
[Python Azure 存储软件包]: https://pypi.python.org/pypi/azure-storage
