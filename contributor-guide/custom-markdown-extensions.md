---
ms.openlocfilehash: 970012e69163af6f2ac9eba95d5c15953f473fa0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    title="required"
    pageTitle="在我们的技术文章中使用的自定义减价扩展"
    description="列出启用嵌入式的视频、 说明和提示、 可重用的内容和 azure.microsoft.com 的技术文章中的其他项的自定义减价扩展。"
    services=""
    solutions=""
    documentationCenter=""
    authors="tysonn"
    manager="carolz"
    editor=""/>

<tags
    ms.service="contributor-guide"
    ms.devlang=""
    ms.topic="article"
    ms.tgt_pltfrm=""
    ms.workload=""
    ms.date="01/22/2015"
    ms.author="tysonn"/>

## Azure.microsoft.com 的减价

一般减价的提示，请参阅[减价的基本知识](https://help.github.com/articles/markdown-basics/)和我们[减价速查表](./media/documents/markdown-cheatsheet.pdf?raw=true)。 如果您需要在减价中创建文章的交叉链接，请参见 [链接指南] (。 / create-links-markdown.md#markdown-syntax-for-acom-relative-links.md/)。

Azure.microsoft.com 支持[隔离的代码块](https://help.github.com/articles/github-flavored-markdown/#fenced-code-blocks)和[语法突出显示](https://help.github.com/articles/github-flavored-markdown/#syntax-highlighting)。 但是，ACOM 支持只有一个语法突出显示配色方案，而不考虑在代码块中指定的语言。

## 在我们的技术文章中使用的自定义减价扩展

我们所有项目都使用 GitHub flavored 的减价的大多数文章格式的段落、 链接、 列表，标题，等等。但是，我们使用自定义的减价扩展我们需要在 azure.microsoft.com 上生成的页面格式。 这里就是我们当前正在使用的扩展︰

+ [说明和提示]
+ [包括]
+ [嵌入的视频]
+ [技术和平台选择器]

## 说明和提示

您可以选择从 4 种类型的说明和提示︰

- AZURE。请注意
- AZURE。警告
- AZURE。提示
- AZURE。重要

###用法
一般情况下，使用说明和提示，尽量少在整个文章。 当您使用它们时，选择适当的注意或提示类型︰

- 使用 AZURE。注意若要突出显示中立或正面信息，强调或补充主要文本的关键点。 注意提供只在特殊情况下适用的信息。

  ![](./media/custom-markdown-extensions/Notes-note.PNG)

- 使用 AZURE。警告可能将来会导致问题的条件向用户发出警报。 例如，选择某些选项或做出某些选择可能永久地将您限制在特定方案。

  ![](./media/custom-markdown-extensions/Notes-warning.PNG)

- 使用 AZURE。提示若要帮助用户应用的技术和他们的特定需要对文本中所述的过程。 提示可能还建议可能不太明显的替代方法。 提示和技巧，但是，不是不可或缺的基本文本的理解。

  ![](./media/custom-markdown-extensions/Notes-tip.PNG)

- 使用 AZURE。要点若要提供对完成任务至关重要的信息。

  ![](./media/custom-markdown-extensions/Notes-important.PNG)

虽然这些说明和提示支持代码块、 图像、 列表和链接，请保持您的说明和提示，简单明了。 如果您发现自己有很多的格式创建复杂的笔记，可能只需要正文文章的另一个部分的一个符号。 而且，在一篇文章中的太多说明可以分散注意力和硬盘扫描或读取。

###示例减价

所有的示例演示 AZURE。请注意。 若要使用提示、 警告或重要，替换减价中"的说明:

    > [AZURE.TIP]

    > [AZURE.WARNING]

    > [AZURE.IMPORTANT]

单个的段落︰

    > [AZURE.NOTE] 若要完成本教程，您必须具有活动的 Microsoft Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。

Multiparagraph:

    > [AZURE.NOTE] 若要完成本教程，您必须具有活动的 Microsoft Azure 帐户。
    >
    > 如果您没有帐户，则可以[创建免费的试用帐户](http://www.windowsazure.com/pricing/free-trial/)中只需几分钟。

## 包括

我们 GitHub 存储库中的可重复使用的文字片断称为"包含"。 当您需要在多个项目中使用的文本，则减价文件中包含的文本片段的引用。 文本片段 （包含） 本身是一个简单的减价 (.md) 文件。 它可以包含任何有效的减价，包括文本、 链接和图像。 所有包括的减价的文件必须在[/ 包括目录](https://github.com/Azure/azure-content/tree/master/includes)存储库的根目录中的。 文章发布时，包括文本是无缝地集成到已发布的主题。

- 我们使用一种特定的语法来引用包括。

- 置于包含的媒体文件必须在创建介质文件夹包含所特有。 媒体文件夹包括所属[azure 的内容/包括/介质文件夹](https://github.com/Azure/azure-content/tree/master/includes/media)中。 媒体目录不应包含任何其根目录中的图像。 如果包括没有图像，则不需要对应的媒体目录。

###用法

- 用包括需要相同的文本显示在多个项目中的任何地方。
- 包括打算用于大量内容的段落或两个、 共享的过程或共享的部分。 不使用它们的任何小于一个句子;它们不是产品的名称或不完整的句子。
- 不嵌入在其他包括包括。 发布系统在发生糟糕的事情 ！
- 不共享媒体文件之间。 对于每个包含和文章的唯一名称使用一个单独的文件。 将媒体文件存储在与包括相关的媒体文件夹。
- 不要使用包括作为仅有的一篇文章的内容。  包括应为在本文的其余部分内容的补充资料。
- 因为所有包含必须在 / 目录，包含文章中包含的路径始终是

    ..包括 /

- 不进行重复链接或图像文件名中所引用的文章，包括。 添加"的包括"链接引用或媒体文件名，以避免重复的引用︰

 **链接引用**

 更改︰ 为 odata.org: odata.org 包括

 **图像引用**

 更改︰ 为 table.png︰ 表 include.png

###示例减价
将包含添加到文档文章的语法是︰

    [AZURE.INCLUDE [include-short-name](../includes/include-file-name.md)]

示例

    [AZURE.INCLUDE [howto-blob-storage](../includes/howto-blob-storage.md)]

包括的第一部分是不带路径和扩展名为.md 的包括名称。 第二部分是包含在中的相对路径 / 包括目录，使用.md 扩展名。

###呈现

在 GitHub 呈现页中，包括将呈现，如下所示︰

 [AZURE。包括如何 blob 存储]

在 azure.microsoft.com，从 HTML 呈现的 HTML 中包含合并到文档的 HTML 的其余部分。 但是，HTML 将包含 HTML 注释与原始包括减价文件名和 GitHub 提交哈希。 这个意见是为了排除故障，以便可以轻松地标识和在 GitHub 中找到源内容包括︰

  ![](./media/custom-markdown-extensions/include.png)


## 嵌入的视频

我们的技术文章支持技术文章中 embeddeded 视频，只要这些视频是在 Microsoft 的[频道 9](http://channel9.msdn.com/)网站上。 必须与[azure.microsoft.com 视频中心](http://azure.microsoft.com/documentation/videos/home/)集成通道 9 视频。 我们当前不支持嵌入的 YouTube 视频;如果你的社区参与者，您将欢迎有过帐所需功能的视频到 YouTube 链接。 频道 9 和视频中心，则应使用 Microsoft contributors （参与者）。

### 用法

- 请确保视频视频中心上。

- 从第 9 频道上视频的友好的 URL 或 Azure 视频中心复制视频的 ID。 例如，在[http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/](http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/)的视频的视频 ID 是**azure 计划程序的特殊计划**。

### 语法

    > [AZURE.VIDEO video-id-string]

### 呈现

在 GitHub: [https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md](https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md)

已发布的文章︰ [http://azure.microsoft.com/documentation/articles/web-sites-backup/](http://azure.microsoft.com/documentation/articles/web-sites-backup/)


## 技术和平台选择器

用 switchers 技术和平台技术的文章中，在跨平台或技术创作多口味的地址不同，在实现相同文章时。 这是通常最适用于我们的移动平台内容的开发。 目前有两种不同类型的选择器、[简单选择器](#simple-selectors)和[双向选择器](#two-way-selectors)。

由于相同的选择器减价转选定内容中的每个主题中，我们建议放置您的主题的选择器中将包括，然后引用，包括在所有您的主题中使用的相同的选择器。

###<a id="simple-selectors"></a>简单选择器

简单 （单向） 的选择器将呈现为一组选项按钮标题的正下方。 当客户需要可供选择的单个平台或技术集，例如.NET，Node.js 和 Java 中的主题时，请使用这些按钮。  为所有选择器使用自定义减价扩展-对于选择器，请使用 HTML。  

请参阅[入门通知集线器](http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started/)才能看到作者创建 8 版本的同一篇文章中，但使用的选择器，以实现跨所有的导航方式。

![简单选择器的示例](./media/custom-markdown-extensions/selectors.PNG)

####语法

    > [AZURE.SELECTOR]
    - [链接标签 #1](link #1 url)
    - [#2 链接标签](link #2 url)

示例：

    > [AZURE.SELECTOR]
    - [通用的 Windows](../articles/notification-hubs-windows-store-dotnet-get-started/)
    - [Windows Phone](../articles/notification-hubs-windows-phone-get-started/)
    - [iOS](../articles/notification-hubs-ios-get-started/)
    - [Android](../articles/notification-hubs-android-get-started/)
    - [Kindle](../articles/notification-hubs-kindle-get-started/)
    - [Baidu](../articles/notification-hubs-baidu-get-started/)
    - [Xamarin.iOS](../articles/partner-xamarin-notification-hubs-ios-get-started/)
    - [Xamarin.Android](../articles/partner-xamarin-notification-hubs-android-get-started/)

#### 呈现

上面的图像显示在 azure.microsoft.com 上呈现。 在 GitHub 呈现页面中，选择器将呈现为链接的项目符号列表。

###<a id="two-way-selectors"></a>双向选择器

双向选择器允许用户从两种矩阵中选择主题。 在 Azure 的技术，如移动服务，支持多个后端平台以及多个客户端时，这是必不可少的。 请记住以下原则︰

- 虽然它被设计为`(Platform | Backend)`，dropwdown 文本现在进行自定义。
- 不需要您矩阵中，每个点的列表项，但只能有一个主题的 URL 位置存在并且不是重复项。
- 链接可以是任何 URL，虽然通常另一个 GitHub 主题。

请参阅[开始使用移动服务](http://azure.microsoft.com/en-us/documentation/articles/mobile-services-ios-get-started/)请参阅作者创建 15 版本的同一篇文章 （9 移动客户端平台和 2 后端平台），但使用的选择器，以实现跨所有的导航方式。 请注意，没有两个后端版本 3 的文章。

![双向选择器示例](./media/custom-markdown-extensions/selector-list.png)

####语法

    > [AZURE。选择器列表 (Dropdown1 |Dropdown2)]     -  [(Dropdown1Text1 |Dropdown2Text1)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text1 |Dropdown2Text2)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text2 |Dropdown2Text3)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text3 |Dropdown2Text4)](../articles/dropdown1-text1-dropdown2-text1.md)

示例：

    > [AZURE。选择器列表 (平台 |后端）]     -  [(iOS |.NET)](./mobile-services-dotnet-backend-ios-get-started-push.md)
    - [(iOS |JavaScript)](./mobile-services-javascript-backend-ios-get-started-push.md)
    - [(Windows 通用 C# |.NET)](./mobile-services-dotnet-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows 通用 C# |Javascript)](./mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows Phone |.NET)](./mobile-services-dotnet-backend-windows-phone-get-started-push.md)
    - [(Windows Phone |Javascript)](./mobile-services-javascript-backend-windows-phone-get-started-push.md)
    - [(Android |.NET)](./mobile-services-dotnet-backend-android-get-started-push.md)
    - [(Android |Javascript)](./mobile-services-javascript-backend-android-get-started-push.md)
    - [(Xamarin iOS |Javascript)](./partner-xamarin-mobile-services-ios-get-started-push.md)
    - [(Xamarin Android |Javascript)](./partner-xamarin-mobile-services-android-get-started-push.md)

#### 呈现

上面的图像显示在 azure.microsoft.com 上呈现。 在 GitHub 呈现页面中，选择器将呈现为链接的项目符号列表。

<!--Anchors-->
[说明和提示]: #notes-and-tips
[包括]: #includes
[嵌入的视频]: #embedded-videos
[技术和平台选择器]: #technology-and-platform-selectors

###贡献者指南链接

- [概述文章](./../README.md)
- [指南文章索引](./contributor-guide-index.md)
