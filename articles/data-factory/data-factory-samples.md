---
ms.openlocfilehash: 7803a2c917948cbd74ebbf2eab6ea200076a8038
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties     
    pageTitle="Azure 数据工厂-示例" 
    description="在 Azure 数据工厂服务提供有关装运的示例的详细信息。" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/21/2015" 
    ms.author="spelluru"/>

# Azure 数据工厂-示例

## 在 Azure 门户中的示例
可快速部署、 检查，并测试在 Azure 门户中使用**示例管道**刀片 Azure 数据工厂示例。 

1. 创建新的数据工厂或打开现有的数据工厂。 请参阅[使用 Azure 数据工厂入门][数据-工厂--入门]步骤创建的数据工厂。
2. 在数据工厂**数据工厂**刀片式服务器，单击**样本管道**铺。

    ![示例的管线图块](./media/data-factory-samples/SamplePipelinesTile.png)

2. 在**样本管道**刀片式服务器，请单击要部署**示例**。 
    
    ![样品的管线刀片式服务器](./media/data-factory-samples/SampleTile.png)

3. 指定配置设置的示例。 例如，您 Azure 存储帐户用户名和帐户密码、 SQL Azure 服务器名称、 数据库、 用户 ID 和密码等... 

    ![刀片式服务器示例](./media/data-factory-samples/SampleBlade.png)

4. 使用指定的配置设置完之后，请单击**创建**创建/部署示例管道和管道使用的链接的服务/表。
5. 单击**示例管线**刀片式服务器的前面的示例图块上，您将看到部署的状态。

    ![部署状态](./media/data-factory-samples/DeploymentStatus.png)

6. 当看到**部署已成功**消息示例图块上时，关闭**示例管道**刀片式服务器。  
5. **数据工厂**刀片式服务器，您将看到链接的服务、 数据集和管线被添加到您的数据工厂。  

    ![数据工厂刀片式服务器](./media/data-factory-samples/DataFactoryBladeAfter.png)
   

下表提供可用**示例的管线**图块中的示例的简要说明。 

示例名称 | description
----------- | -----------
游戏的客户概要分析 | Contoso 是一家游戏公司创建多个平台的游戏︰ 游戏控制台、 手持式设备和个人电脑 (Pc)。 这些游戏的每个生成日志的吨。 Contoso 的目标是收集和分析这些游戏来获取使用信息，确定向上销售和交叉销售的机会，制定引人注目的新功能等所产生的日志...改进的业务并为客户提供更好的体验。 此示例收集的示例日志、 进程和与引用数据，使其更加丰富和转换的数据，以评估最近已启动了 Contoso 在市场营销活动的有效性。
 
## 在 GitHub 上的示例
[GitHub Azure DataFactory 存储库](https://github.com/azure/azure-datafactory)包含一些示例，帮助您快速量产 Azure 数据工厂服务与 （或） 修改脚本，并在自己的应用程序中使用它。 Samples\JSON 文件夹包含 JSON 段为常见方案。

## 发送反馈
我们真诚欢迎您的反馈意见，在这篇文章。 请花几分钟的时间来提交您的反馈意见通过[电子邮件](mailto:adfdocfeedback@microsoft.com?subject=data-factory-samples.md)。    

[数据的工厂--入门]: data-factory-get-started.md#CreateDataFactory 
测试t
