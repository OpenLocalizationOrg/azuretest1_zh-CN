---
ms.openlocfilehash: 3d0987d840b00387b3cc4e8a979e66d2b49fe49e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 Azure API 管理策略" 
    description="了解如何创建、 编辑和配置 API 管理策略。" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/24/2015" 
    ms.author="sdanie"/>


#在 Azure API 管理策略

在 Azure API 管理中，策略是允许发布者更改通过配置 API 的行为系统具有强大的功能。 策略是在请求按顺序执行的语句的集合或一个 API 的响应。 常用语句包含 XML 格式转换为 JSON 并调用速率限制来限制来电从开发人员的数量。 许多更多的策略是提供开箱即用。

请参阅[策略引用][]的策略声明和其设置的完整列表。

策略应用在其位于 API 使用者和托管的 API 之间的代理。 该代理接收所有请求，并通常将信息转发出去与基础 API 不变。 但是策略可以将更改应用到的请求入站和出站响应。

策略表达式可作为特性值或文本值放到任何 API 管理策略，除非该策略指定，否则。 某些策略，如[控制流][]和[将变量设置][]策略基于策略表达式。 有关详细信息，请参阅[高级策略][]和[策略表达式][]。

## <a name="scopes"> </a>如何配置策略
全局范围内或在[产品][]、 [API][]或[操作][]的作用域，则可以配置策略。 要配置策略，请导航到策略编辑器中在发布服务器上的门户。

![策略菜单][policies-menu]

策略编辑器包含三个主要部分︰ 策略作用域 （上图）、 策略定义的策略进行编辑 （左） 和语句列表 （右图）︰

![策略编辑器][policies-editor]

要开始配置策略必须先选择该策略应该应用的范围。 在初学者下面的屏幕快照中选择产品。 请注意，策略名称旁边的方形符号表示已经这个级别应用策略。

![Scope][policies-scope]

由于已应用策略，配置所示定义视图。

![配置][policies-configure]

该策略将显示只读起初。 若要编辑定义，请单击**配置策略**操作。

![编辑][policies-edit]

策略定义为简单的 XML 文档描述的入站和出站的语句序列。 可以在定义窗口中直接编辑 XML。 向右提供的语句列表和语句适用于当前作用域已启用并且突出显示;在上面的抓图中限制调用速率语句所示。

单击启用的语句将定义视图中的光标位置处添加相应的 XML。 

策略语句的完整列表以及它们的设置位于[策略参考][]。

例如，要添加新的语句将传入的请求限制为指定的 IP 地址，请将光标置于"入站"XML 元素的内容只是，然后单击限制调用方 IPs 语句。

![限制策略][policies-restrict]

这将指导您如何配置语句的"入站"元素添加 XML 代码段。

    <ip-filter action="allow | forbid">
        <address>address</address>
        <address-range from="address" to="address"/>
    </ip-filter>

若要限制入站的请求并接受不仅仅来自 1.2.3.4 的 IP 地址，如下所示修改 XML:

    <ip-filter action="allow">
        <address>1.2.3.4</address>
    </ip-filter>

![保存][policies-save]

完成配置策略语句后单击保存，所做的更改将传播到 API 管理代理立即。

##<a name="sections"> </a>了解策略配置

策略是一系列的请求和响应的顺序执行的语句。 配置是适当地划分为入站 （请求） 和出站 （策略） 如下所示的配置中。

    <policies>
        <inbound>
            <!-- statements to be applied to the request go here -->
        </inbound>
        <outbound>
            <!-- statements to be applied to the response go here -->
        </outbound>
    </policies>

由于可以在不同级别 （全球，产品、 api 和操作） 指定策略配置提供了有助于您指定此定义的语句执行相对于父策略的顺序。 

例如，如果您在全局级别和策略配置 API 有一个策略，然后只要使用该特定 API-这两个策略将应用。 API 管理可以通过基本元素的组合的策略语句的确定性订购。 

    <policies>
        <inbound>
            <cross-domain />
            <base />
            <find-and-replace from="xyz" to="abc" />
        </inbound>
    </policies>

在上面的示例策略定义，跨域语句将执行之前任何更高的策略，这反过来会跟查找和替换策略。

注︰ 全球政策有没有父策略并使用`<base>`在它的元素不起作用。 

## 下一步行动

签出以下策略表达式上的视频。

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[策略引用]: api-management-policy-reference.md
[Product]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis 
[操作]: api-management-howto-add-operations.md

[高级的策略]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[控制流]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[设置变量]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[策略表达式]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[策略菜单]: ./media/api-management-howto-policies/api-management-policies-menu.png
[策略编辑器]: ./media/api-management-howto-policies/api-management-policies-editor.png
[策略的作用域]: ./media/api-management-howto-policies/api-management-policies-scope.png
[策略配置]: ./media/api-management-howto-policies/api-management-policies-configure.png
[编辑策略]: ./media/api-management-howto-policies/api-management-policies-edit.png
[策略限制]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[策略存储]: ./media/api-management-howto-policies/api-management-policies-save.png

测试
