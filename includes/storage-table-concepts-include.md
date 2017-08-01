---
ms.openlocfilehash: 3b0435cb7b6128efbba4a69d083249b1f4f56f7e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 表服务是什么

Azure 表存储服务存储大量的结构化数据。 该服务是 NoSQL 数据存储区，它接受经过身份验证的调用从内部和外部 Azure 的云。 Azure 表非常适合于结构化、 非关系型数据存储。 常用的表服务包括︰

-   能够提供 web 应用程序的结构化数据的 Tb 存储
-   存储数据集不需要复杂的联接、 外键或存储的过程，可以用于快速访问非正常化
-   快速查询使用聚集的索引的数据
-   使用 WCF 数据服务的.NET 库使用 OData 协议和 LINQ 查询来访问数据

您可以使用表服务来存储和查询结构化、 非关系型数据和表格的大集将随需求的增加。

## 表服务概念

表服务包含以下组件︰

![表 1][Table1]

-   **的 URL 格式︰**代码地址表中使用此地址格式的帐户︰   
    http://`<storage account>`.table.core.windows.net/`<table>`  
      
    可以直接使用 OData 协议使用此地址的 Azure 表来解决。 有关详细信息，请参见[OData.org][]

-   **存储帐户︰**对 Azure 存储的所有访问都是通过存储帐户。 有关帐户的存储容量的详细信息，请参阅[Azure 存储可扩展性和性能目标](storage-scalability-targets.md)。

-   **表**︰ 表是实体的集合。 表不强制执行架构上的实体，这意味着单个表可以包含具有不同的属性集的实体。 存储帐户可以包含的表的数目仅受存储帐户容量限制。

-   **实体**︰ 一个实体是一组属性，类似于数据库行。 实体可以是大小为 1 MB。

-   **属性**︰ 属性是名称 / 值对。 每个实体可以包含多达 252 属性来存储数据。 每个实体也有 3 个系统属性指定分区键、 行键和一个时间戳。 可以查询速度越快，并在原子操作中插入/更新相同的分区键的实体。 实体的行键是在一个分区中的唯一标识符。


  
  [表 1]: ./media/storage-table-concepts-include/table1.png
  [OData.org]: http://www.odata.org/