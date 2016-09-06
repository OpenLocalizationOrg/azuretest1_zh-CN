---
ms.openlocfilehash: a3f89dc8baa6a515656a85f2febe59bb77d4b742
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="自定义的开发人员门户中 Azure API 管理"
    description="自定义的开发人员门户中 Azure API 管理。"
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
    ms.topic="get-started-article" 
    ms.date="08/05/2015"
    ms.author="sdanie"/>

# 自定义的开发人员门户中 Azure API 管理

本指南介绍了如何修改与您的品牌的一致性在 API 管理的开发人员门户的外观和感觉。

## <a name="change-page-headers"> </a>更改页标题中的文本/徽标

门户的自定义项的主要方面之一与您的公司名称或徽标替换所有页面的顶部的文本。

开发人员门户中的内容被修改通过出版商门户中，通过 Azure 管理门户进行访问。 要到达 API 发布者门户，单击**管理**API 管理服务 Azure 门户中。

![出版商门户][api-management-management-console]

开发人员门户基于内容管理系统或 CMS。 在每一页显示的标题是内容的一种特殊类型称为一个构件。 若要编辑该构件中的内容从**开发人员门户**菜单的左侧，单击**小部件**，然后从列表中选择该**标题**构件。

![小部件标题][api-management-widgets-header]

标头的内容是从**正文**字段中可编辑的。 将文本更改为"Fabrikam 开发人员门户"，单击页面底部的**保存**。

现在您应该能够看到开发人员门户中每一页上的新的头 ！

> 出版商门户中的开发人员门户，单击打开**开发人员门户**上顶栏中。

## <a name="change-headers-styling"> </a>更改标题的样式

由样式规则定义的颜色、 字体、 大小、 间距和门户网站上所有页面的其他样式相关的元素。 若要编辑样式**外观**从菜单中单击**开发人员门户**发布门户中。 然后单击**开始自定义**来启用样式编辑器中。

您的浏览器将切换到包含示例的内容，并提供在网站上的任何位置使用的所有样式规则的示例开发人员门户中隐藏的页面中。 若要打开样式编辑器中，您将光标移动到细灰的垂直线在页面最左边的部分。 编辑器工具栏应该显示。

![自定义工具栏][api-management-customization-toolbar]

有两种主要模式的编辑样式规则-**编辑的所有规则**显示列表中的任意位置; 所使用的所有样式规则同时**选取元素**允许您从页上并将显示样式仅该元素选择的元素。

在本节中我们想要更改的样式的仅是邮件头。 单击样式编辑器工具栏中的**选择元素**选项，然后单击**选择要自定义的元素**上。 元素将现在变得被突出显示以表示哪些元素的样式，您可以开始编辑如果您单击鼠标悬停在它们上方。 将鼠标移到文本标题 （"Fabrikam 开发人员门户"如果您遵循上一节中的说明进行操作） 中的单位名称，然后单击它。 样式编辑器中将显示一组命名和分类的样式规则。

每个规则表示所选元素的样式属性。 例如，对于上面所选的标头文本，文本的大小是 @font-size-h1 在 @headings-font-family 的替代字体的名称时。

> 如果您不熟悉[引导][]，这些规则实际上是[较少的变量][]中所使用的开发人员门户引导主题。

让我们来更改标题文本的颜色。 在**@headings 颜色**字段中选择的项，然后键入 #000000。 这是黑颜色的十六进制代码。 执行此操作，您将看到一个正方形颜色指示器将显示在文本框的末尾。 如果您单击该指示器，拾色器将允许您选择一种颜色。

![颜色选取器][api-management-customization-toolbar-color-picker]

当使用完更改为所选元素的样式单击**预览更改**在屏幕上查看结果。 这一次它们只是对管理员可见。 若要使这些更改对所有人可见，单击样式编辑器中的**发布**按钮，确认所做的更改。

![发布菜单][api-management-customization-toolbar-publish-form]

> 若要更改样式设置编辑器中将应用于页按照相同的过程，就像头-单击**选取的元素**上的任何其他元素的样式规则，请选择您感兴趣，该元素并开始修改屏幕上显示的样式规则的值。

## <a name="edit-page-contents"> </a>编辑页面的内容

开发人员门户包含类似的 Api、 产品、 应用程序、 问题和手动编写的内容自动生成的页面。 由于它基于一个内容管理系统可以创建所需的此类内容。

若要查看所有现有内容的列表页**内容**从菜单中单击**开发人员门户**发布门户中。

![管理内容][api-management-customization-manage-content]

单击"欢迎"页后，可以编辑开发人员门户的主页上显示的内容。 进行的更改、 预览它们如有必要，然后再单击**立即发布**以使其对所有人可见。

> 在主页上使用特殊的布局，这使它可以在顶部显示的标题。 此标题从内容部分不可编辑。 若要编辑此标题，请单击**小部件**上的从**开发人员门户**菜单，然后从**当前图层**下拉列表中选择**主页**，然后打开的**横幅**项目下的主要部分。 此构件中的内容进行编辑就像其他任何页一样。

## <a name="next-steps"> </a>下一步行动

-   查看[高级 API 配置入门][]教程中的其他主题。

[更改页面页眉中的文本/徽标]: #change-page-headers
[更改标题的样式]: #change-headers-styling
[编辑页面的内容]: #edit-page-contents
[下一步行动]: #next-steps

[管理门户]: https://manage.windowsazure.com/

[api 管理管理控制台]: ./media/api-management-customize-portal/api-management-management-console.png
[api 管理小部件标题]: ./media/api-management-customize-portal/api-management-widgets-header.png
[api 管理自定义工具栏]: ./media/api-management-customize-portal/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-portal/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-portal/api-management-customization-toolbar-publish-form.png
[api 管理-自定义的管理的内容]: ./media/api-management-customize-portal/api-management-customization-manage-content.png


[开始使用高级 API 配置]: api-management-get-started-advanced.md
[引导]: http://getbootstrap.com/
[较少的变量]: http://getbootstrap.com/css/

测试
