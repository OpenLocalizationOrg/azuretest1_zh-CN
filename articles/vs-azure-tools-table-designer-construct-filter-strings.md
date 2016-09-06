---
ms.openlocfilehash: d7b089877b8cccaf6abaeb913e0bc52aa588b50b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="表设计器中构建筛选器字符串"
   description="表设计器中构建筛选器字符串"
   services="visual-studio-online"
   documentationCenter="na"
   authors="kempb"
   manager="douge"
   editor="tlee" />
<tags 
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2015"
   ms.author="kempb" />

# 表设计器中构建筛选器字符串

##概述

若要筛选显示在 Visual Studio**设计器表**Azure 表中的数据，您构造一个筛选器字符串和筛选器字段中输入它。 筛选器字符串语法定义的 WCF 数据服务和类似于 SQL WHERE 语句，但被发送到通过 HTTP 请求表服务。 **表设计器**处理为您，所以要作为筛选依据的所需的属性值的正确编码，您只需要输入属性名称、 比较运算符、 标准值和 （可选） 布尔运算符在筛选字段中的。 您不需要包括 $filter 查询选项，如果您构建这样的 URL 查询通过[存储服务 REST API 参考](http://go.microsoft.com/fwlink/p/?LinkId=400447)表一样。

WCF 数据服务基于[开放的数据协议](http://go.microsoft.com/fwlink/p/?LinkId=214805)(OData)。 筛选系统查询选项 (**$filter**) 的详细信息，请参阅[OData URI 约定规范](http://go.microsoft.com/fwlink/p/?LinkId=214806)。

## 比较运算符

使用下面的逻辑运算符支持的所有属性类型︰

|逻辑运算符|说明|筛选器字符串的示例|
|---|---|---|
|eq|等于|城市 eq 雷德蒙|
|gt|大于|价格 gt 20|
|ge|大于或等于|价格 ge 10|
|浅色|小于|价格长期 20|
|le|小于或等于|价格 le 100|
|ne|不等于|城市 ne 伦敦|
| 和 |And|价格 le 200 和价格 gt 3.5|
|或|或|价格 le 3.5 或价格 gt 200|
|不|不|不 isAvailable|

在构造一个筛选器字符串，则下面的规则非常重要︰

- 逻辑运算符用于将属性的值进行比较。 请注意，不可能为一个动态值; 如果属性进行比较一侧必须是表达式的常量。

- 筛选器字符串的所有部件都都区分大小写。

- 常量的值必须是同一数据类型作为筛选器的顺序属性返回的有效结果。 有关受支持的属性类型的详细信息，请参阅[了解表服务数据模型](http://go.microsoft.com/fwlink/p/?LinkId=400448)。

## 筛选字符串属性

筛选字符串属性时，将字符串常量用单引号引起来。

下面的示例筛选的**PartitionKey**和**RowKey**的属性;其他非键属性也可以添加到筛选器字符串︰

    PartitionKey eq 'Partition1' and RowKey eq '00001'


可将每个筛选器表达式放在括号中，尽管不是必需的︰

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')


请注意，表服务不支持通配符查询，不支持在表设计器中或者。 但是，您可以执行上所需的前缀使用比较运算符匹配的前缀。 下面的示例返回以字母 A 开头的姓氏属性的实体︰

    LastName ge 'A' and LastName lt 'B'

## 在数值属性过滤

若要筛选在整数或浮点数，指定不使用引号。

本示例返回与 Age 属性，其值大于 30 的所有实体︰

    Age gt 30


本示例返回与它的值是小于或等于 100.25 数量属性的所有实体︰

    AmountDue le 100.25

## 在布尔属性过滤

若要筛选在一个布尔值，指定**true**或**false**不使用引号。

下面的示例返回所有实体位置的 IsActive 属性设置为**true**:

    IsActive eq true

您还可以编写逻辑运算符没有此筛选器表达式。 在以下示例中，表服务属性也将返回所有实体在其中 IsActive 为**true**:

[复制](javascript:if (window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_3d6a191e-f389-447a-bbbb-ef8b163bc645');)

若要返回 false IsActive 的所有实体，可以使用 not 运算符︰

    IsActive

## 在日期时间属性过滤

要作为筛选依据的日期时间值，请指定**日期时间**关键字后, 跟日期/时间常量用单引号引起来。 日期/时间常数必须以组合 UTC 格式，[格式日期时间属性值](http://go.microsoft.com/fwlink/p/?LinkId=400449)中所述。

下面的示例返回实体的 CustomerSince 属性等于 2008 年 7 月 10 日︰

    CustomerSince eq datetime'2008-07-10T00:00:00Z'

测试
