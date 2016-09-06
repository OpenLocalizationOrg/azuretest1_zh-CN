---
ms.openlocfilehash: 5d032a764e5e1cd55925407942dea4168936818c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在减价中创建图像"
    description="解释如何在减价根据准则设置 Azure 存储库中创建图像。"
    services=""
    solutions=""
    documentationCenter=""
    authors="kenhoff"
    manager="ilanas"
    editor="tysonn"/>

<tags
    ms.service="contributor-guide"
    ms.devlang=""
    ms.topic="article"
    ms.tgt_pltfrm=""
    ms.workload=""
    ms.date="06/25/2015"
    ms.author="kenhoff" />

# 在减价中创建图像

## 映像文件夹创建和链接语法

新的文章中，您将需要在以下位置创建一个文件夹︰

    /articles/<service-directory>/media/<article-name>/

例如︰

    /articles/app-service/media/app-service-enterprise-multichannel-apps/

创建文件夹并添加到图像后，使用以下语法来创建在您的文章的图像︰

```
![Alt image text](./media/article-name/your-image-filename.png)
```
示例：

[减价模板](../markdown%20templates/markdown-template-for-new-articles.md)的示例，请参阅。  这个减价模板中的图像引用链接被设计模板的底部。

## 针对 azure.microsoft.com 的准则

如果不能包括重现步骤当前鼓励屏幕抓图。 不要编写您的内容，以使内容能经受得住没有屏幕抓图如果需要。

使用下列准则创建和包括画的文件时︰
- 不在文档之间共享图片文件。 将复制的文件需要并将其添加到介质文件夹中，为您的特定主题。 因为它是易于删除过时内容和图像保持 repo 干净，不鼓励文件之间共享。

- .png 文件高度优于是其他格式。

- 使用画图中提供的默认宽度的红色正方形 (5 像素) 引起对特定元素中的屏幕快照。  

    示例：
    
    ![这是一种用作标注一个红色正方形。](./media/create-images-markdown/gs13noauth.png)

- 避免屏幕抓图的边缘上的空白。 如果裁剪边缘留下白色背景的方式的一个屏幕快照，请添加单个像素灰度图像边框。  如果使用画图程序，默认调色板 (0xC3C3C3) 中使用浅灰色。 如果使用某些其他图形的应用程序，RGB 颜色是 R195，G195，195。 可以轻松添加图像周围的灰色边框 Visio，这样，选择该图像，选择行，并确保在正确的颜色设置，并且然后更改为 1 1/2 磅的线条粗细。  屏幕抓图应该具有 1 个像素宽的灰色边框，以便屏幕截图的白色区域执行不模糊到网页。

    示例：

    ![这是一种灰色边框的空白。](./media/create-images-markdown/agent.png)

- 允许使用空白的概念性表示图没有灰色 boder。  
    
    例如︰  ![这是一个概念与空白和没有灰色边框图像的示例。](./media/create-images-markdown/ic727360.png)

- 尽量不要使太宽的图像。  将自动调整图像，如果太宽。 不过，调整有时会导致颜色容差，所以我们建议您限制到 780 图像的宽度像素和手动调整图像，如有必要在提交之前。

- 屏幕截图中显示命令输出。  如果您的文章中包含用户工作在一个外壳内的位置的步骤，它可在屏幕截图中显示命令的输出。 在这种情况下，通常大约 72 个字符限制外壳宽度可以确保您的图像将保持 780 像素宽度准则中。 输出的屏幕抓图前，调整窗口大小以便只需相关命令和输出显示 （还可以使用任意一侧空行）。

- 将整个屏幕抓图的 windows 在可能的情况。 采取一个浏览器窗口的屏幕快照，当调整浏览器窗口到 780 像素宽或更小，并保持尽可能的为浏览器窗口高度的短如适合窗口的应用程序。

    示例：

    ![这是一个浏览器窗口屏幕快照示例。](./media/create-images-markdown/helloworldlocal.png)

- 请谨慎使用哪些信息会显示在屏幕抓图。  不要泄露公司内部信息或个人信息。

- 概念性图片或图表，使用云和企业符号和图标集的官方图标。 Http://aka.ms/CnESymbols 提供了公用组。

###贡献者指南链接

- [概述文章](./../README.md)
- [指南文章索引](./contributor-guide-index.md)
