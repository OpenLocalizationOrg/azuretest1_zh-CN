---
ms.openlocfilehash: 798623c51a2e241f6e8a4efbee1229fd6b17eb91
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties title="" pageTitle="文件的名称和位置的 Azure 的技术文章" description="说明文章和创建新文章时应遵循的命名约定的文件结构。" metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="12/16/2014" ms.author="tysonn" />

#文件的名称和位置的 Azure 的技术文章

在我们技术的内容存储库，我们使用单个文件夹 （**文章**文件夹） 的所有项目。 没有文件夹层次结构-所有项目都居住在平面文件结构。 如果您用其中的文章创建文件夹，不能发布文章。

而不是作为一个组织原则中使用的文件结构，我们可以使用严格的文件命名约定清楚地标识的主题并且向 web 上的可发现性的贡献。

以下是您需要了解︰

+ [规则]
+ [模式]
+ [标准的示例]
+ [Azure 预览门户的特殊文件命名约定]
+ [市场上的内容]
+ [文件名称审批]

##规则

- 不包含空格或标点字符。 使用连字符分隔的文件名中的单词。
- 使用所有小写字母
- 不能超过 80 个字符-这是发布的系统限制
- 使用操作谓词，例如特定开发、 购买、 构建、 解决。 没有-ing 的单词。
- 不包含任何小的词 — a 和，，或，etc。
- 所有文件必须在减价并使用.md 文件扩展名。

##模式

以下是一般模式︰

 **service-platform-language-content-product-version.md**

使用模式的应用，并查看以了解现有名称的存储库中的项目的列表部分。 不要开始开发平台的名称或服务名称都可能怀疑有问题，并通过进度落后。

##标准的示例

以下是几个遵循模式的有效名称。 :

- 云-服务-最低-连续-delivery.md
- 移动-服务-ios 的 get-started.md
- documentdb 管理 account.md
- mobile-services-dotnet-backend-get-started-settings-sync.md
- active-directory-java-authenticate-users-access-control-eclipse.md
- virtual-machines-install-windows-server-2008r2.md


##Azure 预览门户的特殊文件命名约定

现在，我们有两个门户网站运行的[一般可用性门户](https://manage.windowsazure.com)和[Azure 预览门户](https://portal.azure.com)。 要清楚地标识没有在元数据中隐藏该已写入的预览门户网站的内容，我们需要遵循一些稍有自定义的文件命名指南︰

- 如果该服务仅在 Azure 预览门户中可用，可以很容易。 只需按照标准命名指南。

- 如果服务也可在两个门户网站，正在编写一篇文章中预览门户的服务有关之前.md 扩展名, 的文件名称的末尾添加**预览门户**。 这将有助于我们在旧的门户，服务内容分开新门户网站在该服务的内容。 （不要混合门户内容 ！）

- 如果文章是关于预览门户本身并不特定于任何服务或平台，开始文件命名为与**azure 预览门户**。

下面是一些示例︰

- azure-preview-portal-supported-browsers-devices.md
- 存储-高级-存储-预览-portal.md

##市场上的内容

若要区分重点合作伙伴 Azure 市场贡献的内容，启动文件的名称与"市场"。 此内容不应太常见，因为大多数合作伙伴内容应在合作伙伴的网站上创建。

- marketplace-mongodb-virtual-machines-install-windows-server-2008r2.md

##文件名称审批

它是拉的请求审阅者时第一次新的文件提交到资源库文件的名称我们组的工作。 拉请求审阅者应该查看的文件的名称并提供通过拉请求注释流的反馈，如果需要进行更改。 需要进行更正，然后在拉请求被接受的文件的名称。 参与者可以很容易地将更新推送到挂起请求的请求。

###贡献者指南链接

- [概述文章](./../README.md)
- [指南文章索引](./contributor-guide-index.md)


<!--Anchors-->
[规则]: #rules
[模式]: #pattern
[标准的示例]: #standard-examples
[Azure 预览门户的特殊文件命名约定]: #special-file-naming-convention-for-the-azure-preview-portal
[市场上的内容]: #marketplace-content
[文件名称审批]: #file-name-approval
