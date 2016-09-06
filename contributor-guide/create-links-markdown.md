---
ms.openlocfilehash: b68b5fd4adb230d7d5df49ba8b130cd65a391f9c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在减价文章中创建链接" description="解释如何在减价的交叉链接的代码。" metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="02/03/2015" ms.author="tysonn" />

# 链接的 Azure 的技术内容的指南
## 在 azure.microsoft.com 上的技术文章的准则

| 链接方案 | 指南  |
|---------------|-----------|
|从 ACOM 文章链接到另一个 ACOM 文章|使用相对链接。 不包括 en-我们相对链接中的语言环境。|
|链接到 MSDN 库主题、 TechNet 库主题或知识文库文章|使用的实际链接到文章或主题，但删除 en-我们的链接中的语言环境。|
|从 ACOM 文章链接到其他网页|使用直接链接|

### ACOM 相对链接的减价语法

若要创建到另一个 ACOM 技术文章 ACOM 技术文章的内联链接，使用该链接的格式。   如果您在目录中创建任何新的链接或文章，您将需要按照新链接语法。

若要从一个 ACOM 技术文档链接到另一个的旧链接语法︰

    [link text](filename.md)

**新链接语法** 

文章链接从一个子目录的根目录中的文章︰

    [link text](../article-name.md)

向服务子目录中的项目根目录链接中的文章︰ 

    [link text](service-directory/article-name.md)

服务子目录链接到另一个服务子目录中一篇文章中的文章︰

    [link text](../service-directory/article-name.md)
 
目录链接到同一个目录中的另一篇文章中的文章︰

    [link text](article-name.md)


不需要再创建定位标记-自动生成它们时的所有 H2 标题发布时间。 您所要做的唯一事情就是创建 H2 部分的链接︰

    [link](#the-text-of-the-H2-section-separated-by-hyphens)
    [Create cache](#create-cache)

若要链接到同一个子目录中的另一篇文章中的定位点︰

    [link text](article-name.md#anchor-name)
    [Configure your profile](media-services-create-account.md#configure-your-profile)

若要链接到另一个服务子目录中的定位点︰

    [link text](service-directory/article-name.md#anchor-name)
    [Configure your profile](service-directory/media-services-create-account.md#configure-your-profile)


## 自定义减价链接语法

由于包括文件位于其他目录中，您将需要使用相对路径如下所示。 从单篇文章的链接包括文件，请使用以下格式︰

    [link text](../articles/service-folder/article-name.md)
    
了解更多关于如何使用[自定义减价扩展准则](custom-markdown-extensions.md#includes)中包括文件。

如果您有包含嵌入的选择器，请使用这种类型的链接︰ 

    > [AZURE。选择器列表 (Dropdown1 |Dropdown2)]     -  [(文本 1 |Example1)](../articles/service-folder/article-name1.md)
    - [(文本 1 |Example2)](../articles/service-folder/article-name2.md)
    - [(文本 2 |Example3)](../articles/service-folder/article-name3.md)
    - [(文本 2 |Example4)](../articles/service-folder/article-name4.md)

若要链接到页上 ACOM （例如一个定价页面，SLA 页面或以外的任何其他文档文章），使用绝对 URL，但请省略区域设置。 这里的目标是在 GitHub 和呈现网站的链接工作︰

    [link text](http://azure.microsoft.com/pricing/details/virtual-machines/)

若要测试链接，将您的页面推送到您分叉在呈现的视图中进行查看和发布到沙盒。 在 GitHub 版本的页面上的交叉链接应工作只要存在于您分叉的 Url 目标。

我们的[技术文章的减价模板](../markdown templates/markdown-template-for-new-articles.md/)显示另一种方法创建的交叉链接在减价，这样所有交叉链接被编码在一起在文章的结尾部分，即使它们显示内联。

## 引用样式的链接

引用样式链接可用于增强您的源内容的可读性。 引用样式链接替换内嵌链接语法简化语法，您可以将长 Url 移到文章的末尾。 下面是 Daring 火球的示例︰

内联文本︰

    I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].

在文章的末尾的链接引用︰

    <!--Reference links in article-->
    [1]: http://google.com/
    [2]: http://search.yahoo.com/  
    [3]: http://search.msn.com/

## 请记住 Azure 库镶边 ！
如果您想要链接到一个居住在[该节点](https://msdn.microsoft.com/library/azure)下的 Azure 库主题，请务必指定 Azure 链接 （/azure/） 中的铬。 Azure 镶边共享 ACOM 导航选项，并只显示的 Azure 内容的 MSDN 库。 正确地指定了作用域的链接如下所示︰

    http://msdn.microsoft.com/library/azure/dd163896.aspx

否则，将使用显示的整个 MSDN 树 MSDN 标准型呈现页面。

## FWLinks

Azure.microsoft.com 内容中避免 FWLinks （我们重定向系统）。 它们应使用仅作为最后的手段，当您需要创建页面还不知道其 URL 的链接。 他们几乎从不实际需要。 对于 ACOM，您定义的文件名称，以便您可以知道什么它会提前。 尚未发布库主题中，您可以创建使用 GUID 的主题，因此不需要使用 FWLink 的链接。

如果您必须在 web 页上使用 FWLink，包括 P 参数，以使其永久重定向︰

    http://go.microsoft.com/fwlink/p/?LinkId=389595

当目标 URL 粘贴到 FWLink 工具时，请记住如果您目标的链接是 ACOM，MSDN 或 TechNet 库中删除区域设置。

### 贡献者指南链接

- [概述文章](./../README.md)
- [指南文章索引](./contributor-guide-index.md)

<!--image references-->
[1]: ./media/create-tables-markdown/table-markdown.png
[2]: ./media/create-tables-markdown/break-tables.png
