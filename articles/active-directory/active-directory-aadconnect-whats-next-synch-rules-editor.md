---
ms.openlocfilehash: fffaeaa70c436d980309b61456ebe179295e2b9d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
    pageTitle="使用 Azure AD 连接同步规则编辑器" 
    description="了解如何使用 Azure AD 连接同步规则编辑器。" 
    services="active-directory" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2015" 
    ms.author="billmath"/>

# Azure AD 连接同步规则编辑器


## 使用同步规则编辑器

在 Azure AD 连接，您可以配置和调整之间 Azure 广告的对象和属性流和您的内部目录通过配置同步规则。  

同步规则是一组满足某个条件时流动特性的配置对象。 它还用于描述连接器空间中的对象与中元节，称为联接或匹配的对象。 同步规则具有优先权，指示它们之间的相互。 具有较低的数字值优先级中的同步规则具有较高的优先级，如果属性流冲突，更高的优先级将赢得的冲突解决方法。可以使用同步规则编辑器来配置同步规则。  

作为示例，我们将介绍在同步规则"中从广告 – 用户 AccountEnabled"。 我们会将标记为 SRE 中的这行并选择 Edit.A 同步规则有四个配置部分︰ 说明，范围筛选、 联接规则和转换。

### 说明
第一节提供了基本信息，如名称和说明。

<center>![加入规则](./media/active-directory-aadconnect-whats-next-synch-rules-editor/sync1.png)
</center>

我们还发现此规则，关于哪些连接的系统的相关的信息的对象类型将应用到所连接的系统中，元节对象类型。 如果源对象类型是用户、 iNetOrgPerson 或联系，元节对象类型始终是无关的人。 元节对象类型应该永远不会更改，因此它创建作为泛型类型。 链接类型可以将联接、 StickyJoin 或规定。 与明加入规则结合使用此设置，我们将在后面讲述这。

### 作用域筛选器

作用域筛选器部分用于配置时应该应用同步规则。 因为我们正在探索同步规则的名称指示已启用用户应该只应用，因此 AD 属性 userAccountControl 不能有 2 位配置作用域设置。 当我们发现用户中广告，我们将应用此规则，如果 userAccountControl 设置为十进制值 512 （启用普通用户），但不是会应用如果我们发现的用户 userAccountControl 设置为 514 （禁用普通用户）。

<center>![加入规则](./media/active-directory-aadconnect-whats-next-synch-rules-editor/sync2.png)
</center>

作用域筛选器，并且在组子句可以嵌套。 要应用的同步规则必须满足组内的所有条款。 当定义了多个组至少一个组必须满足有关要应用的规则。 亦即 逻辑 OR 组和逻辑之间计算和评估组内。 在出站同步规则出到 AAD – 加入的组，如下所示找不到此操作的示例。 有两个同步筛选器组、 安全组 (securityEnabled 等于 True) 和通讯组 (securityEnabled 等于 False)。

<center>![加入规则](./media/active-directory-aadconnect-whats-next-synch-rules-editor/sync3.png)
</center>

此规则用于定义哪些组应调配给 AAD。 通讯组必须启用要与 AAD，同步邮件但安全组，这不是必需。 正如您可以看到，以及计算大量的附加属性。

###加入规则
第三部分用来配置中元节对象中的连接器空间对象的关联方式。 我们已经在前面的规则没有任何配置对于加入规则，相反我们要从 AD – 用户加入看。 

<center>![加入规则](./media/active-directory-aadconnect-whats-next-synch-rules-editor/sync4.png)
</center>

连接规则的内容将取决于在安装向导中选择匹配选项。 对于入站规则评估开始源连接器空间中的对象，每个组中联接规则计算序列中。 如果源对象计算匹配元节使用一种连接规则中的一个对象，对象被连接在一起。 如果所有规则中已经过都评估和没有匹配项，则使用描述页上的链接类型。 如果此设置被设置为提供一个新的对象被创建在目标中，元节。 若要提供一个新的对象，到了元节是也称为项目到了元节对象。 联接规则只计算一次。 连接器空间对象和元节对象连接在一起，它们将保持联接只要同步规则的范围仍然满意。 当计算同步规则定义的联接规则只能有一个同步规则必须在范围内。 如果联接规则与多个同步规则找到一个对象，则会引发错误。 因此最好是能够与多个同步规则时对象作用域中定义的联接一个同步规则。 在 Azure AD 连接的现成的配置中这些规则可以通过查看名称找到并查找这些词结尾的名称的连接。 如果另一个同步规则结合在一起的对象或设置目标中的一个新的对象，而不需要定义任何连接规则的同步规则将应用属性流。

###转换
转换部分定义联接对象和作用域筛选器，则满足时将应用于目标对象的所有属性流。 回到我们在从广告 – 用户 AccountEnabled 同步规则，我们会发现下列转换︰

<center>![加入规则](./media/active-directory-aadconnect-whats-next-synch-rules-editor/sync5.png)
</center>

若要将此放在上下文中，帐户资源目录林部署中我们希望启用的帐户在帐户目录林和已禁用的帐户中找到资源目录林 Exchange 和 Lync 设置。 我们所看的同步规则包含登录所需的属性，我们希望这些流动从林，我们发现此站点启用的帐户。 所有这些属性流的合成一个同步 Rule.A 转换中可具有不同的类型︰ 常数、 直和表达式。 常流始终将流中的特定值，在上面的例子中，我们始终将设置元节中的值为 True 名为 accountEnabled 的特性。 直接流量会流动到目标属性的源中的属性的值。 第三种流类型是表达式，它允许使用更高级的配置。 表达式语言是 VBA (Visual Basic for Applications) 使用户体验的 Microsoft Office 或 VBScript 将识别的格式。 属性括在方括号内，[属性名称]。 属性名和函数名区分大小写，但同步规则编辑器将对表达式求值，并提供警告，如果表达式是无效。在单独的一行，包含嵌套函数表示的所有表达式。 若要显示的配置语言的强大功能，下面是流为 pwdLastSet，但插入的其他意见︰

        // If-then-else
        IIF(
        // (The evaluation for IIF) Is the attribute pwdLastSet present in AD? 
        IsPresent([pwdLastSet]),
        // (The True part of IIF) If it is, then from right to left, convert the AD time format to a .Net datetime, change it to the time format used by AAD, and finally convert it to a string.
        CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
        // (The False part of IIF) Nothing to contribute
        NULL
        )

大主题的转换，它为 Azure AD 连接提供了自定义配置可能很大一部分。 自定义配置将不包含在该概述文档，但我们将看一下在本文档后面某些附加属性流。
 
测试
