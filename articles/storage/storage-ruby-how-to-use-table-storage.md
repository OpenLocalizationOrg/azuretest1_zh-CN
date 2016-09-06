---
ms.openlocfilehash: 2ea14d816c6a19d7ea808d274f39d9f2025e3ea9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用 Ruby 从表存储 |Microsoft Azure" 
    description="了解如何使用在 Azure 表存储服务。 使用 Ruby API 编写代码示例。" 
    services="storage" 
    documentationCenter="ruby" 
    authors="tfitzmac" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="07/29/2015" 
    ms.author="tomfitz"/>


# 如何使用表格存储从拼音

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]

## 概述

本指南介绍了如何执行常见使用 Microsoft Azure 表服务的情况。 这些示例被写入书面使用 Ruby API。 所包含的方案包括**创建和删除表即可删除、 插入和查询表中的实体**。

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## 创建 Ruby 应用程序

创建 Ruby 应用程序。 有关说明，请参阅[创建 Azure 中的 Ruby 应用程序](/develop/ruby/tutorials/web-app-with-linux-vm/)。

## 配置应用程序访问存储

若要使用 Azure 存储，您需要下载并使用 Ruby azure 程序包，其中包含一组与存储其他服务进行通信的便利库。

### 使用 RubyGems 获取包

1. 使用命令行界面**PowerShell** (Windows)、**终端**(Mac) 或**狂欢**(Unix) 等。

2. **Gem 安装 azure**命令窗口中键入要安装的 gem 和依赖项。

### 导入包

使用您喜爱的文本编辑器，将以下内容添加到想要使用存储的 Ruby 文件的顶部︰

    require "azure"

## 设置 Azure 存储连接

Azure 模块将读取环境变量**AZURE\_存储\_帐户**和**AZURE\_存储\_访问\_键**连接到 Azure 存储帐户所需的信息。 如果没有设置这些环境变量，则必须指定帐户信息，然后用下面的代码使用**Azure::TableService** :

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"

若要获取这些值︰

1. 登录到[Azure 的管理门户](https://manage.windowsazure.com/)。

2. 导航到要使用的存储帐户。

3. 单击导航窗格底部的**管理密钥**。

4. 在弹出对话框中，您将看到存储帐户名称、 主访问键和访问键。 访问键，则可以主或辅助的一个。

## 如何创建表

**Azure::TableService**对象使您可以使用 tabls 和实体。 若要创建表，请使用**创建\_table()**方法。 下面的示例创建表或打印出错误，则为。

    azure_table_service = Azure::TableService.new
    begin
      azure_table_service.create_table("testtable")
    rescue
      puts $!
    end

## 如何向表中添加实体

若要添加某个实体，首先创建一个定义实体属性的哈希对象。 请注意，对于每个实体 mustspecify **PartitionKey**和**RowKey**。 这些是您的实体的唯一标识符，可以查询速度快于其他属性的值。 Azure 存储服务使用**PartitionKey**自动分布多个存储节点的表的实体。 具有相同的**PartitionKey**的实体存储在同一个节点上。 **RowKey**是实体的它所属的分区中的唯一 ID。 

    entity = { "content" => "test entity", 
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.insert_entity("testtable", entity)

## 如何︰ 更新实体

有多种方法可用于更新现有实体︰

* **更新\_entity():**更新替换的现有实体。
* **合并\_entity():**通过将新的属性值合并到现有实体更新现有实体。
* **插入\_或\_合并\_entity():**更新替换的现有实体。 如果没有实体存在，则将插入一个新︰
* **插入\_或\_替换\_entity():**通过将新的属性值合并到现有实体更新现有实体。 如果没有实体存在，则将插入一个新。

下面的示例演示如何更新实体使用**更新\_entity()**:

    entity = { "content" => "test entity with updated content", 
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.update_entity("testtable", entity)

与**更新\_entity()**和**合并\_entity()**，如果正在更新的实体不存在更新操作将会失败。 因此如果您想要存储而不考虑是否已存在的实体，则应改为使用**插入\_或\_替换\_entity()**或**插入\_或\_合并\_entity()**。

## 如何︰ 处理的实体组

有时它意义提交以确保原子由服务器处理批处理中的放在一起的多个操作。 为实现这一点，您可以首先创建一个**批处理**，然后使用**执行\_batch()** **TableService**上的方法。 下面的示例演示与 RowKey 2 的两个实体和 3 批次中的提交。 请注意，它仅适用于具有相同的 PartitionKey 的实体。

    azure_table_service = Azure::TableService.new
    batch = Azure::Storage::Table::Batch.new("testtable", 
      "test-partition-key") do
      insert "2", { "content" => "new content 2" }
      insert "3", { "content" => "new content 3" }
    end
    results = azure_table_service.execute_batch(batch)

## 如何︰ 查询实体

要查询一个表中的一个实体，请使用**获得\_entity()**方法，通过**PartitionKey**和**RowKey**的表名称。

    result = azure_table_service.get_entity("testtable", "test-partition-key", 
      "1")

## 如何︰ 查询一组实体

要查询一组表中的实体，请创建哈希对象查询，并使用**查询\_entities()**方法。 下面的示例演示如何获取具有相同的**PartitionKey**的所有实体︰

    query = { :filter => "PartitionKey eq 'test-partition-key'" }
    result, token = azure_table_service.query_entities("testtable", query)

**请注意**，如果太大，一个查询以返回结果集，延续将返回标记可用于检索后续页。

## 如何︰ 查询实体属性的子集

对一个表的查询可以检索几个属性从一个实体。 这种技术，称为"投影"，减少了带宽，并可以提高查询性能，尤其是对于大型实体。 使用 select 子句并传递您想要将到客户端的属性的名称。

    query = { :filter => "PartitionKey eq 'test-partition-key'", 
      :select => ["content"] }
    result, token = azure_table_service.query_entities("testtable", query)

## 如何︰ 删除实体

若要删除某个实体，请使用**删除\_entity()**方法。 需要名称的表包含的实体，pas 的 PartitionKey 和 RowKey 的实体。

        azure_table_service.delete_entity("testtable", "test-partition-key", "1")

## 如何︰ 删除表

若要删除某个表，请使用**删除\_table()**方法，并传入要删除的表的名称。

        azure_table_service.delete_table("testtable")

## 下一步行动

现在，您已经了解了表存储的基本知识，按照这些链接以了解更复杂的存储任务。

- 请参阅 MSDN 参考︰ [Azure 存储](http://msdn.microsoft.com/library/azure/gg433040.aspx)
- 访问[Azure 存储团队博客](http://blogs.msdn.com/b/windowsazurestorage/)
- 在 GitHub 访问[Ruby 的 Azure SDK](http://github.com/WindowsAzure/azure-sdk-for-ruby)存储库
 
