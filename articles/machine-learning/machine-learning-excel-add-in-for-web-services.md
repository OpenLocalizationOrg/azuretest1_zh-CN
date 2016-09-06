---
ms.openlocfilehash: f9fc1f3c86db0c8d9e55b43719059ca658096f51
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Excel 加载项为机器学习 web 服务 |Microsoft Azure"
    description="如何在 Excel 中直接使用 Azure 机器学习的 web 服务，而无需编写任何代码。"
    services="machine-learning"
    documentationCenter=""
    authors="tedway"
    manager="paulettm"
    editor="cgronlun"
    tags=""/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="08/25/2015"
    ms.author="tedway;garye" />

# Azure 机器学习的 web 服务，excel 外接程序

Excel 可以方便地调用 web 服务，而无需编写任何代码直接。

## 步骤以使用现有的 Web 服务在工作簿中

1. 打开[Excel 文件的示例](http://aka.ms/amlexcel-sample-2)，其中包含 Excel 外接程序和 Titanic 上乘客提供有关数据。
2. 通过单击来选择该 web 服务的"Titanic Survivor 预测值 （Excel 外接程序示例） [分数]"在此示例中。

    ![选择 web 服务][01]

3. 这将为**Predict**部分。  此工作簿中已包含示例数据，但可能还在 Excel 中选择一个单元格，单击**使用示例数据**的空白工作簿。
4. 选择的数据具有标题，然后单击输入的数据范围图标。  请确保选中"我的数据带有标题"复选框。
5. 在**输出**，输入输出为，如"H1"在此处放置单元格号。
6. 单击**预测**。

    ![预测一节][02]

## 若要添加新的 Web 服务的步骤

1. 发布 web 服务 （[此页面](machine-learning-walkthrough-5-publish-web-service.md)说明了如何执行此操作），或者使用现有的 web 服务。
2. 在 Excel 中，转到**Web 服务**部分 （如果您是在**Predict**部分中，单击后退箭头定位到列表中的 web 服务）。

    ![转到 web 服务选择][03]

3. 单击**添加 Web 服务**。
4. 在机器学习 Studio 中，单击左侧窗格中， **WEB 服务**部分，然后选择 web 服务。

    ![Studio 选择 web 服务][04]

5. 将复制的 web 服务的 API 密钥。

    ![Studio API 键][05]

6. 在 Excel 中添加文本框中的粘贴 API 键标有**API 密钥**。
7. 为 web 服务**面板**选项卡上，单击**请求/响应**链接。
8. **OData 端点地址**部分的外观。  将 URL 复制并粘贴到文字框的复选标记**的 URL**。
9. 单击**添加**。

    ![URL 和 API 键。][06]

10. 要使用 web 服务，请按照上面，"步骤为使用现有 Web 服务"。

## 共享工作簿

如果您已经添加的 web 服务的 API 键也将被保存，然后保存您的工作簿。  这意味着仅应与您信任的人共享工作簿。

要求下或在我们的[论坛](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409)上的任何问题。

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png

测试
