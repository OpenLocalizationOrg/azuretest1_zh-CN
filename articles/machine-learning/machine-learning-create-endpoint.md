---
ms.openlocfilehash: d1d43e7792dc127cb632077e8f1c711f1bdd19fe
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在机器学习中创建 web 服务终结点 |Microsoft Azure" 
    description="在 Azure 机器学习中创建 web 服务终结点" 
    services="machine-learning" 
    documentationCenter="" 
    authors="hiteshmadan" 
    manager="padou" 
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd" 
    ms.date="04/21/2015"
    ms.author="himad"/>


# 创建终结点

Azure 机器学习可以创建多个已发布的 web 服务的终结点。 每个终结点是单独发送、 调节和管理，独立于其他 web 服务的终结点。 没有每个终结点的唯一 URL 和授权密钥。

这允许 Azure 机器学习用户创建 web 服务，通过再出售前向其客户。 每个终结点可以分别自定义与自己培训模型时仍然链接到创建此 web 服务的实验。 此外，对实验的任何更新可以有选择地应用到的终结点而不会覆盖这些自定义项。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## 终结点创建步骤
- 打开[http://manage.windowsazure.com](http://manage.windowsazure.com)，单击左列中的**机器学习**。 单击工作区包含您感兴趣的 web 服务。
![导航到工作区](./media/machine-learning-create-endpoint/figure-1.png)


- 单击**Web 服务**选项卡。
![导航到 web 服务](./media/machine-learning-create-endpoint/figure-2.png)


- 单击您感兴趣查看列表中的可用终结点的 web 服务。
![导航到的终结点](./media/machine-learning-create-endpoint/figure-3.png)


- 单击底部的**添加终结点**按钮。 填写的名称和说明，请确保没有其他的终结点具有相同的名称，此 web 服务中。 限制级留下它的默认值，除非您有特殊要求。
若要了解有关限制的详细信息，请参阅[扩展 API 端点](machine-learning-scaling-endpoints.md)。
![创建终结点](./media/machine-learning-create-endpoint/figure-4.png)


一旦创建终结点，可以使用同步 Api，批处理 Api，通过和 excel 工作表。
有关使用机器学习的 web 服务的详细信息，请参阅[如何使用已发布的 Azure 机器学习 web 服务](machine-learning-consume-web-services.md)。
 
测试
