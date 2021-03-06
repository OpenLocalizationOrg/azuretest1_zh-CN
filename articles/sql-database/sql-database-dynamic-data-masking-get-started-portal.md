---
ms.openlocfilehash: d303c420365df536f3ff69310e48b19d38849a53
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="开始使用 SQL 数据库动态数据屏蔽 （Azure 门户）" 
   description="如何开始使用 SQL 数据库动态数据屏蔽在 Azure 门户" 
   services="sql-database" 
   documentationCenter="" 
   authors="nadavhelfman" 
   manager="jeffreyg" 
   editor="v-romcal"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services" 
   ms.date="07/30/2015"
   ms.author="nadavh; ronmat; v-romcal; sstein; ronitr"/>

# 开始使用 SQL 数据库动态数据屏蔽 （Azure 门户）

> [AZURE.SELECTOR]
- [动态数据屏蔽-Azure 预览门户](sql-database-dynamic-data-masking-get-started.md)

## 概述

SQL 数据库动态数据屏蔽的掩蔽它给非特权用户，从而限制敏感数据泄露。 在预览基本、 标准和特优服务层，SQL Azure 数据库的 V12 版本中是动态数据屏蔽。

动态数据屏蔽有助于防止对敏感数据的未经授权的访问，通过使客户能够指定要显示在应用程序层的影响最小的敏感数据。 它是一种基于策略的安全功能，在指定的数据库域的上方隐藏查询的结果集中的敏感数据，而不更改数据库中的数据。

例如，电话中心支持人员可能由几个数字的社会安全号码或信用卡号码标识调用方，但这些数据项目不应完全公开向支持者。 开发人员可以定义屏蔽规则将应用于每个掩码所有但任何社会安全号码或信用卡号码中结果集的最后四位数的查询结果。 通过使用适当的数据掩码来保护个人可识别信息 (PII) 的数据，另一个示例，开发人员可以查询进行故障排除，同时又不违反法规遵从性要求的生产环境。

## SQL 数据库动态数据屏蔽基础知识

您将动态数据屏蔽 Azure 门户下数据库审核和安全选项卡中的策略设置。
设置动态数据如果屏蔽检查之前使用["下层客户端"](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)。


> [AZURE.NOTE] 若要设置屏蔽 Azure 预览门户中的动态数据，请参阅[开始使用 SQL 数据库动态数据屏蔽 （Azure 预览门户）](sql-database-dynamic-data-masking-get-started.md)。 


### 动态数据屏蔽权限

可以通过 Azure 数据库管理、 服务器管理或安全官员角色配置动态数据屏蔽。

### 动态数据屏蔽策略

* **特权的登录**的登录将在 SQL 查询结果中获取未被蒙版遮盖的数据集。
  
* **屏蔽规则**的一套规则，用于定义要遮罩的指定的字段和将使用的屏蔽功能。 可以使用数据库表名称和列名称来定义指定的字段。

* **屏蔽功能**的一组控件公开的不同应用场景的数据的方法。

| 屏蔽功能 | 掩蔽逻辑 |
|----------|---------------|
| **默认值**  |**根据指定的字段的数据类型的完全屏蔽**<br/><br/>• 使用 XXXXXXXX 或少 Xs 字段的大小是否少于 8 个字符的字符串数据类型 （nchar、 ntext 或 nvarchar）。<br/>• 为数值数据类型 （bigint、 位、 小数、 int、 货币、 数字、 smallint、 smallmoney、 tinyint、 浮动、 实） 使用零值。<br/>• 使用日期/时间数据类型 （日期、 datetime2、 日期时间、 方法、 短格式、 时间） 01-01-1900。<br/>• 针对 SQL 变量，当前类型的默认值使用。<br/>• 针对 XML 文档<masked/>使用。<br/>• 为特殊的数据类型使用空值 (时间戳表，hierarchyid，GUID、 二进制文件、 图像、 低级空间类型)。
| **信用卡** |**掩蔽的方法将指定的字段的最后四位数**并将常数字符串添加作为信用卡的形式的前缀。<br/><br/>XXXX XXXX XXXX 1234|
| **社会安全号** |**掩蔽的方法将指定的字段的最后两位数**，并将常数字符串添加为在窗体中，美国的社会安全号码的前缀。<br/><br/>XXX 的 XX-XX12 |
| **Email** | **掩蔽的方法其公开的第一个字母，替换 XXX.com 的域**使用的电子邮件地址的窗体中的常量字符串前缀。<br/><br/>aXX@XXXX.com |
| **随机数字** | 根据实际数据类型与所选的边界的**屏蔽的方法以生成一个随机数**。 如果指定的边界相等，掩蔽功能将固定数目。<br/><br/>![导航窗格](./media/sql-database-dynamic-data-masking-get-started-portal/1_DDM_Random_number.png) |
| **自定义文本** | **屏蔽其公开的第一个和最后一个字符的方法**，并在中间添加自定义填充字符串。<br/>[空白] 前缀后缀<br/><br/>![导航窗格](./media/sql-database-dynamic-data-masking-get-started-portal/2_DDM_Custom_text.png) |

  
<a name="Anchor1"></a>
### 启用安全的连接字符串

如果您使用的["下层客户端"](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)，则您必须更新现有的客户端 (示例︰ 应用程序) 使用修改后的连接字符串格式。 请单击[此处](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)以了解详细信息。


## 设置动态掩蔽使用 Azure 门户数据库的数据

1. 启动在[https://manage.windowsazure.com](https://manage.windowsazure.com)Azure 的门户。

2. 单击您想要屏蔽的数据库，然后单击**审核和安全**选项卡。

3. 下**屏蔽的动态数据**，请单击**启用**掩盖功能的动态数据。  

4. 键入应具有对未被蒙版遮盖的敏感数据的访问权限的登录。

    >[AZURE.TIP] 若要使应用程序层可以显示应用程序的特权用户的敏感数据，添加用于查询数据库的 SQL 登录名。 强烈建议在此列表中包含的登录尽量减少暴露敏感数据的最少数量。

    ![导航窗格](./media/sql-database-dynamic-data-masking-get-started-portal/4_ddm_policy_classic_portal.png)

5. 在菜单栏中的网页底部，单击**添加蒙版**打开屏蔽规则配置窗口。

6. 从定义将屏蔽指定的字段的下拉列表中选择**表**和**列**。

7. 从敏感数据屏蔽类别列表中选择一个**屏蔽功能**。

    ![导航窗格](./media/sql-database-dynamic-data-masking-get-started-portal/5_DDM_Add_Masking_Rule_Classic_Portal.png) 
    
8. 数据屏蔽更新屏蔽策略的动态数据中的规则集的规则窗口中单击**确定**。

9. 单击**保存**以保存新的或更新的屏蔽策略。


## 设置屏蔽数据库使用 Powershell cmdlet 的动态数据

请参阅[SQL Azure 数据库 Cmdlet](https://msdn.microsoft.com/library/azure/mt163521.aspx)。

## 设置动态数据屏蔽数据库使用 REST API，

请参阅[SQL Azure 数据库的操作](https://msdn.microsoft.com/library/dn505719.aspx)。
