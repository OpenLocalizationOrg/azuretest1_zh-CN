---
ms.openlocfilehash: 3011474f768b9b2412110855b224df24a53204f5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure AD 连接设计概念 |Microsoft Azure"
   description="本主题详细介绍特定实现设计区域"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="09/03/2015"
   ms.author="andkjell"/>

# Azure AD 连接的设计概念
本主题的目的是为了说明必须 Azure AD 连接的实现设计过程中考虑的区域。 这是深入查看某些区域上的，这些概念进行了简要描述以及其他主题。

## sourceAnchor
SourceAnchor 属性被定义为*不可变的对象的生存期内的特性*。 它唯一地标识对象为同一对象内部和 Azure 的广告中。 属性也被称为**immutableId** ，可互换使用的两个名称。

单词的不可变的即不能更改，对该主题很重要。 由于设置了后不能更改此属性的值很重要，要选择一种设计，这将支持您的方案。

在下列情况下使用该特性︰

- 当新的同步引擎服务器生成，或重建后灾难恢复方案中时，此特性将在对象内部使用 Azure AD 链接现有对象。
- 如果您移动到同步的标识模型从云专用标识此特性将 Azure AD 与内部对象中允许的"硬匹配"现有对象的对象。
- 如果使用联合身份验证时，此特性以及**范围内**使用在声明中来唯一地标识用户。

本主题将只谈 sourceAnchor 与用户相关。 相同的规则应用于所有对象类型，但它仅对用户是这通常是一个问题。

### 选择一个好的 sourceAnchor 特性
属性值必须遵循下列规则︰

- 将长度少于 60 个字符
- 不能包含特殊字符︰ & #92; ！ # $ % & * + / = ? ^ & #96;{ } |~ < > （);: , [ ] " @ _
- 必须是全局唯一的
- 必须是字符串、 整数或二进制文件
- 不基于用户的名称，这些更改
- 应该不区分大小写并避免可能因大小写的值
- 应在创建对象时分配。


如果所选的 sourceAnchor 不是字符串类型，Azure AD 连接将 Base64Encode 属性值，可以确保没有特殊字符将出现。 如果您使用 ADFS 比另一个联合服务器，请确保您的服务器还具有 Base64Encode 的功能特性。

SourceAnchor 属性是区分大小写。 值为"JohnDoe"不是"johndoe"相同。

如果您采用单目录林内部则应使用该属性为**objectGUID**。 这也是在 Azure AD 连接中使用快速设置时使用的属性以及使用的目录同步的特性。

如果您有多个林，不要在目录林之间和同一个林中的域之间移动用户**objectGUID**则使用甚至在这种情况下的良好特性。

如果目录林和域之间移动用户，然后必须找到特性，它不会改变，也可以移动用户与移动的过程。 建议的方法是引入综合特性。 无法保存东西它看起来像一个 GUID 特性会比较适合。 在对象创建过程中一个新的 GUID 创建和用户标记。 在同步引擎服务器中，可以创建基于**objectGUID**此值和更新中添加选定的属性，可以创建自定义规则。 当您移动对象时，请确保复制此值的内容。

另一个解决方案是挑选您知道不会更改现有属性。 常用的属性包括**雇员 id**。 如果您考虑属性将包含字母，请确保没有该特性的值可以更改大小写 （大写和小写） 没有机会。 错误不应使用的属性包括用户的名称。 在结婚或离婚需要更改名称，它不允许为此属性。 这也是为什么等**范围内**、**邮件**和**targetAddress**属性不是甚至可以在 Azure AD 连接安装向导中选择的一个原因。 这些属性还包含 @ 字符，这不允许在 sourceAnchor 中。


### 更改 sourceAnchor 属性
在 Azure AD 中创建了对象并标识同步之后，sourceAnchor 属性值不能更改。

鉴于此，以下限制适用于 Azure AD 连接︰

- 只能在初始安装过程中设置 sourceAnchor 属性。 如果您重新运行安装向导，此选项是只读的。 如果您需要更改此设置，您必须卸载并重新安装。
- 如果您安装了另一个 Azure AD 连接服务器，则必须选择以前使用相同的 sourceAnchor 属性。 如果您之前已使用目录同步，并移动到 Azure AD 连接，则必须使用**objectGUID** ，因为它是由目录同步的特性。
- 如果 sourceAnchor 的值更改后到 Azure AD，然后 Azure AD 连接同步将引发错误，不允许任何更改回源目录中更改对象之前，此问题已得到解决和 sourceAnchor 已导出对象。

测试
