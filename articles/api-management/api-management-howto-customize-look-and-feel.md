---
ms.openlocfilehash: 1798ebd6b00baf00e5279b828893165ceff5cd35
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何自定义在 Azure API 管理开发人员门户的外观和感觉" 
    description="如何自定义在 Azure API 管理开发人员门户的外观和感觉。" 
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
    ms.date="06/16/2015" 
    ms.author="sdanie"/>

# 如何自定义在 Azure API 管理开发人员门户的外观和感觉

由样式规则定义的颜色、 字体、 大小、 间距和其他方面的开发人员门户的外观和感觉。 每个结构元素的页面的页眉、 菜单、 内容主体、 页面标题等存在的这些规则集。本帮助主题中，您将学习如何修改样式规则。

要编辑样式规则，请在**外观**上从菜单中单击**开发人员门户**发布门户中。 然后单击**开始自定义**来启用样式编辑器中。

您的浏览器将切换到包含示例的内容，并提供在网站上的任何位置使用的所有样式规则的示例开发人员门户中隐藏的页面中。 若要打开样式编辑器中，您将光标移动到细灰的垂直线在页面最左边的部分。 编辑器工具栏应该显示如下︰ 

![自定义工具栏][api-management-customization-toolbar]

有两种主要模式，用于编辑样式规则-**编辑的所有规则**显示列表中的任意位置; 所使用的所有样式规则同时**选取元素**允许您从页上并将显示仅对该特定元素的样式中选择元素。

在本节中，我们想要更改仅特定元素-例如，页标头的样式。 单击样式编辑器工具栏中的**选择元素**选项，然后单击**选择要自定义的元素**上。 元素将现在变得突出显示悬停以表示该元素的样式，您将看到是否您单击它。 

将鼠标移到表示标头中的网页标题的文本，单击它。 样式编辑器中将显示一组命名和分类的样式规则。

每个规则表示所选元素的样式属性。 例如，对于上面所选的标头文本，文本的大小是 @font-size-h1 在 @headings-font-family 的替代字体的名称时。

> 如果您不熟悉[引导](http://getbootstrap.com/)，这些规则实际上是[较少的变量](http://getbootstrap.com/css/)中所使用的开发人员门户引导主题。

现在让我们来更改标题文本的颜色 ！ 在**@headings 颜色**字段中选择的项，然后键入 #000000。 这是黑颜色的十六进制代码。 执行此操作时，您将看到一个正方形颜色指示器将显示在文本框的末尾。 如果您单击此标记，拾色器将允许您选择一种颜色。

![颜色选取器][api-management-customization-toolbar-color-picker]

当使用完更改为所选元素的样式单击**预览更改**在屏幕上查看结果。 这一次它们只是对管理员可见。 若要使这些更改对所有人可见，单击样式编辑器中的**发布**按钮，确认所做的更改。

![发布窗体][api-management-customization-toolbar-publish-form]

> 若要更改样式设置编辑器中将应用于页按照相同的过程，就像头-单击**选取的元素**上的任何其他元素的样式规则，请选择您感兴趣，该元素并开始修改屏幕上显示的样式规则的值。


[下一步行动]: #next-steps

[管理门户]: https://manage.windowsazure.com/

[api 管理自定义工具栏]: ./media/api-management-howto-customize-look-and-feel/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-howto-customize-look-and-feel/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-howto-customize-look-and-feel/api-management-customization-toolbar-publish-form.png

测试
