---
ms.openlocfilehash: 5e03f4084862a3a2bae0584e420be339529e4b05
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="创建数据集" 
    description="了解 Azure 数据工厂数据集，并了解如何创建它们。" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/28/2015" 
    ms.author="spelluru"/>

# 数据集

## 说明
数据集是数据的逻辑说明。 所描述的数据可以从简单的字节，如 CSV 文件一直到关系表或甚至模型的半结构化数据各不相同。 在链接的服务定义和数据集定义中引用 （地址、 协议、 身份验证方案） 可以访问的数据的机制。

## 语法

    {
        "name": "<name of dataset>",
        "properties":
        {
           "structure": [ ],
           "type": "<type of dataset>",
            "external": <boolean flag to indicate external data>,
            "typeProperties":
            {
            },
            "availability":
            {
    
            },
           "policy": 
            {      
    
            }
        }
    }

**语法的详细信息**

| 属性 | 说明 | 是否必需 | 默认值 |
| -------- | ----------- | -------- | ------- |
| 名称 | 数据集名称 | 是 | NA |
| 结构 | <p>数据集架构</p><p>有关详细信息，请参见[数据集结构](#Structure)部分</p> | No. | NA |
| 类型 | 数据集的类型 | 是 | NA |
| typeProperties | <p>对应于所选类型的属性</p><p>有关受支持的类型和它们的属性的详细信息请参见[数据集类型](#Type)一节。</p> | 是 | NA |
| 外部 | Boolean 标志，用于指定是否数据集显式生成数据工厂管道或不  | 否 | false | 
| 可用性 | <p>定义在处理窗口或数据集生产的切片模型。 </p><p>[数据集的可用性](#Availability)主题的更多详细信息，请参阅</p><p>对数据集进行切片模型上的更多详细信息请参阅[计划和执行](data-factory-scheduling-and-execution.md)文章</p> | 是 | NA
| 策略 | 定义条件或数据集切片必须满足的条件。 <p>[数据集策略](#Policy)主题的更多详细信息，请参阅</p> | 否 | NA |

### 示例

下面是示例表示名为**MyTable** **SQL Azure 数据库**中的表的数据集。 **AzureSqlLinkedService**在此数据集中引用中定义的 SQL Azure 数据库连接字符串。 此数据集切片每日。  

    {
        "name": "DatasetSample",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": 
            {
                "tableName": "MyTable"
            },
            "availability": 
            {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

## <a name="Structure"></a>数据集结构

**结构**部分断言数据集的架构。 它包含的列，这些列的类型的集合。 每列均包含以下属性︰

属性 | 说明 | 是否必需 | 默认值  
-------- | ----------- | -------- | -------
name | 列的名称 | 否 | NA 
类型 | 列的数据类型 | 否 | NA 

### 示例

在以下示例中，数据集有三列 slicetimestamp、 项目名称，以及 pageviews。

    structure:  
    [ 
        { "name": "slicetimestamp", "type": "String"},
        { "name": "projectname", "type": "String"},
        { "name": "pageviews", "type": "Decimal"}
    ]

## <a name="Type"></a> 数据集类型

支持的数据源和数据集类型进行对齐。 请参阅连接器主题有关的信息的类型和配置数据集的[数据移动活动](data-factory-data-movement-activities.md)文章中提到。 

## <a name="Availability"></a> 数据集可用性

在数据集中可用性部分定义处理窗口或数据集生产的切片模型。 请参阅数据集进行切片和相关性模型数据集切片的更多详细信息的主题。 

| 属性 | 说明 | 是否必需 | 默认值 |
| -------- | ----------- | -------- | ------- |
| 频率 | 指定数据集切片生产的时间单位。<p>**支持频率**︰ 分钟、 小时、 天、 周、 月</p> | 是 | NA |
| interval | 指定频率的倍频<p>"X 频率间隔"决定切片生成频率。</p><p>如果您需要的数据集进行切片在每小时的基础上，您将设置**时间间隔**为**1****小时**，与**频率**。</p><p>**注意︰**如果您指定频率为分钟，建议将间隔设置为不小于 15</p> | 是 | NA |
| style | 指定是否应在间隔的起点产生切片。<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><p>如果将频率设置为月和样式设置为 EndOfInterval，切片上个月的最后一天产生。 如果样式设置为 StartOfInterval，切片生成的蛾子。 <.p > 的第一天<p>如果频率设置为天和样式设置为 EndOfInterval，在过去的一天中的小时生成切片。</p>如果频率设置为小时和样式设置为 EndOfInterval，切片生成末尾的小时。 例如，下午 1 点--下午 2 点期间的切片，切片在下午 2 点产生。</p> | 否 | EndOfInterval |
| anchorDateTime | 定义了时间计划程序用于计算数据集切片边界中的绝对位置。 <p>**注意︰**如果 AnchorDateTime 有频率比粒度更细的日期部分的更精细的部分将被忽略。 例如，如果**时间间隔**为**每小时**(频率︰ 小时和间隔︰ 1) 和**AnchorDateTime**包含，**分和秒**，则 AnchorDateTime 的**分钟数和秒**的部分将被忽略。 </p>| 否 | 0001 年 01 月 01 日 |
| 偏移量 | 依据移动的开始和结尾的所有数据集切片的时间跨度。 <p>**注意︰**如果指定了 anchorDateTime 和偏移量，则结果为组合 shift 键。</p> | 否 | NA |

### anchorDateTime 示例

**示例︰** 23 小时数据集切片开始的 2007年-04-19T08:00:00

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": "23",   
        "anchorDataTime":"2007-04-19T08:00:00"  
    }


### 对方的示例

在而不是默认午夜早上 6 点开始的每日快讯。 

    "availability":
    {
        "frequency": "Daily",
        "interval": "1",
        "offset": "06:00:00"
    }

在这种情况下，SliceStart 6 小时移动，并且将 6 点。

为 12 个月 (频率 = 月; 间隔 = 12) 安排、 对方︰ 60.00:00:00 意味着每 3 年月第二个或第三 (从该年度开始的 60 天内如果样式 = StartOfInterval) 根据一年是否是闰年。



## <a name="Policy"></a>数据集策略

在数据集中的策略一节定义条件或数据集切片必须满足的条件。

### 验证策略

| 策略名称 | 说明 | 应用于 | 是否必需 | 默认值 |
| ----------- | ----------- | ---------- | -------- | ------- |
| minimumSizeMB | 验证，Azure 的 blob 中的数据满足最小大小 （以兆字节为单位）。 | Azure 的斑点 | 否 | NA |
|minimumRows | 验证 SQL Azure 数据库或 Azure 表中的数据包含的最小行数。 | <ul><li>SQL azure 数据库</li><li>Azure 表</li></ul> | 否 | NA

#### 示例

**minimumSizeMB:**

    "policy":
    
    {
        "validation":
        {
            "minimumSizeMB": 10.0
        }
    }

**minimumRows**

    "policy":
    {
        "validation":
        {
            "minimumRows": 100
        }
    }

### ExternalData 策略

外部数据集是那些不由运行在数据工厂管线产生。 如果数据集被标记为外部，可以将 ExternalData 策略定义为影响行为的数据集切片的可用性。 

| 名称 | 说明 | 是否必需 | 默认值  |
| ---- | ----------- | -------- | -------------- |
| dataDelay | <p>对给定的切片的外部数据的可用性检查的延迟时间。 例如，如果数据应能每小时，可以通过 dataDelay 延迟检查有实际可用的外部数据和相应扇区为就绪。</p><p>仅适用于当前时间;例如，如果是现在下午 1:00，此值为 10 分钟，验证将在下午 1:10 开始。</p><p>此设置不影响在过去切片 (片与切片结束时间 + dataDelay < 现在) 将没有任何延迟处理。</p> <p>大于 23:59 小时需要指定使用的 day.hours:minutes:seconds 格式的时间。 例如，要指定 24 小时内，请不要使用 24:00:00;相反，使用 1.00:00:00。 如果您使用 24:00:00 时，它将被视为 24 天 (24.00:00:00)。 1 一天 4 小时，指定 1:04:00:00。 </p>| 否 | 0 |
| retryInterval | 请重试尝试之间失败下, 一次的等待时间。 适用于当前时间;如果以前尝试失败，我们等待这最后一次尝试很久以后。 <p>如果是现在下午 1:00，我们将开始第一次尝试。 下次重试如果完成第一项验证检查的持续时间为 1 分钟和操作失败，将会在 1:00 + 1 分钟 （持续时间） + 1 分钟 （重试间隔） = 1:02 pm。 </p><p>对于过去的切片，将不会延迟。 重试将会立即发生。</p> | 否 | 00:01:00 （1 分钟） | 
| retryTimeout | 每次重试尝试超时。<p>如果该选项设置为 10 分钟，验证需要在 10 分钟内完成。 如果花费超过 10 分钟来执行验证，重试将超时。</p><p>如果所有尝试验证都超时，则切片将标记为超时。</p> | 否 | 00:10:00 （10 分钟） |
| maximumRetry | 对外部数据的可用性检查次数。 允许的最大值是 10。 | 否 | 3 | 

#### 更多示例

如果您需要在特定的日期和时间 （上午 8:00 在每月第三个上假设） 在每月的基础上运行管道，则可以使用**偏移**标记来设置日期和时间运行。 

    {
      "name": "MyDataset",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "availability": {
          "frequency": "Month",
          "interval": 1,
          "offset": "3.08:10:00",
          "style": "StartOfInterval"
        }
      }
    }

## 发送反馈
我们真诚欢迎您的反馈意见，在这篇文章。 请花几分钟的时间来提交您的反馈意见通过[电子邮件](mailto:adfdocfeedback@microsoft.com?subject=data-factory-create-datasets.md)。 





  










测试
