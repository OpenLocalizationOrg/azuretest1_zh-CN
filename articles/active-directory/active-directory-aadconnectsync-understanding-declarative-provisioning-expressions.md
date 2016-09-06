---
ms.openlocfilehash: 8b6a500f3fd3c3bb886c1e3ff2fcf622397df0a6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure AD 连接同步︰ 了解资源调配的声明性表达式"
    description="介绍了资源调配的声明性表达式。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2015"
    ms.author="markusvi"/>


# Azure AD 连接同步︰ 了解资源调配的声明性表达式

第一次引入前沿标识管理器 2010，若要使您可以实现完整标识集成的业务逻辑，而无需编写代码的声明性调配基于 Azure 活动目录连接同步服务 （Azure AD 连接同步）。

声明性资源调配的核心部分是属性流中使用的表达式语言。 所用的语言是 Microsoft® Visual Basic® 的应用程序 (VBA) 的一个子集。 在 Microsoft Office 中使用这种语言并具有经验的 VBScript 的用户也将识别它。 提供声明性表达式语言只使用函数并不是结构化的语言;没有任何方法或语句。 而是将快速程序流程嵌套函数。

有关更多详细信息，请参阅[Office 2013 的语言参考 Visual Basic for Applications 欢迎](https://msdn.microsoft.com/library/gg264383(v=office.15).aspx)。

特性是强类型。 它需要一个单值字符串属性将不会接受多值函数或不同类型的属性。 这也是区分大小写的。 函数名称和属性名称必须具有正确的大小写，或将引发错误





## 语言的定义和标识符

- 函数具有跟括号中的参数的名称︰ FunctionName (<< 参数 1 >>，<<argument N>>)。
- 属性由方括号: [属性名称]
- 参数由百分号: %参数名称 %
- 字符串常量用引号引起来︰ 如 ""Contoso
- 数字值是表示不带引号，并应为十进制。 为前缀的十六进制值 & h。 例如 98052，& h f F
- 布尔值表示具有常量︰ 真、 假。
- 它们的名称与内置常量表示︰ 空，CRLF，IgnoreThisFlow


## 运算符

可以使用下列运算符︰

- **比较**︰ <，< =、 <>、 =、 >、 > =
- **数学**: +，-，*，-
- **字符串**︰ & （连接）
- **逻辑**︰ & & （和），| |（或者）
- **求值顺序**: （)



从左到右计算运算符。 2*(5 + 3) 并不相同，则为 2*5 + 3。<br> 用括号 （） 来更改求值顺序。





## 参数

参数定义连接器或管理员使用 PowerShell。 参数通常包含将不同系统之间的值，例如位于用户的域的名称。 可以使用这些属性流中。

活动目录连接器为入站同步规则提供以下参数︰

 
|Domain.Netbios |Domain.FQDN |Domain.LDAP | |Forest.Netbios |Forest.FQDN |Forest.LDAP |
 

系统提供以下参数︰

Connector.ID

将填充元节属性域与用户所在的域的 netbios 名称示例。

域 <-%Domain.Netbios%

## 常见方案

### 属性的长度

字符串属性的默认设置为可索引和最大长度为 448 个字符。 如果您正在使用字符串属性，其中可能包含更多，请确保为属性流中包括以下内容︰

`attributeName <- Left([attributeName],448)`

### 更改 userPrincipalSuffix

在 Active Directory 中的范围内特性并非始终是已知用户和可能不适合作为登录 id。 AAD 同步安装指南 》，选取不同的特性，例如邮件。 但在某些情况下必须计算该属性。 例如公司 Contoso 具有两个 AAD 目录、 一个用于生产，另一个用于测试。 他们希望在其测试租户仅用户更改登录 ID.userPrincipalName 中的后缀 <-Word([userPrincipalName],1,"@") 和"@contosotest.com"

在此表达式中，我们采取一切左侧的第一个 @ 符号 (Word) 和固定的字符串串联。





### 将多值转换为单值

Active Directory 中的某些属性是多值架构中，即使它们看上去单个值在 Active Directory 用户和计算机。 例如，说明属性。

在此表达式中的特性具有值的情况下我们采取的第一项 （项） 的属性中、 移除前导空格和尾随空格 (Trim)，然后保持首先 448 个字符 （左） 在字符串中。



## 高级的概念

### 空值与 IgnoreThisFlow

为入站同步规则，应始终使用该常量**为 NULL** 。 这表明，流动作出贡献没有值，另一个规则可以提供一个值。 如果没有规则提供一个值，则将移除该元属性。

对于出站同步规则有两个不同的常数，使用︰ 空和 IgnoreThisFlow。 同时表示，没有任何属性流能够有所贡献，但区别在于没有其它规则有什么贡献或者会发生什么情况。 如果在连接目录中的现有值，空值将阶段将其删除，而 IgnoreThisFlow 将保留现有的值的属性上的删除。



#### ImportedValue

由于属性名称必须括在引号，而不是方括号，是不同于所有其他函数函数 ImportedValues: ImportedValue("proxyAddresses")。

通常在同步期间属性将所需的值，即使还没有已导出或导出 （"顶部的塔"） 过程中接收到的错误。 入站的同步将假定其还尚未达到连接的目录属性最终到达它。 在某些情况下很重要，只同步一个已确认的连接目录的值和函数 ImportedValue 是在这种情况下使用 （"全息图和增量导入塔"）。

举例说明可以从广告 – 用户通用从交换位置混合交换中通过 Exchange 联机添加的值应只同步如果已确认值导出成功找到在现成的同步规则中︰


`proxyAddresses <- RemoveDuplicates(Trim(ImportedValues(“proxyAddresses”)))`

有关函数的完整列表，请参阅[Azure AD 连接同步︰ 函数引用](active-directory-aadconnectsync-functions-reference.md)


## 其他资源

* [Azure AD 连接的同步︰ 自定义同步选项](active-directory-aadconnectsync-whatis.md)
* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
 
<!--Image references-->

测试
