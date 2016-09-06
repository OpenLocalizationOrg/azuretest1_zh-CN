---
ms.openlocfilehash: 9cf67745e2f3368ce366cd2a2dd68f1ae400be35
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将机器学习发布 web 服务到 Azure 市场 |Microsoft Azure" 
    description="如何将 Azure 机器学习 Web 服务发布到 Azure 市场" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="bharaths"/>

# 将 Azure 机器学习 Web 服务发布到 Azure 的市场 

Azure 市场提供发布 Azure 机器学习的 web 服务，如支付或释放外部客户使用的服务的能力。 本文概述了该进程的准则，以帮助您入门的链接。 通过使用此过程，可以使可供其他开发人员在其应用程序中使用您的 web 服务。


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## 发布流程的概览 

Azure 市场发布 Azure 机器学习 web 服务的步骤如下︰

1. 创建和发布一个机器学习请求-响应服务 (RR)
2. 将服务部署到生产环境，并获得 API 键和 OData 端点信息。
3. 使用已发布的 web 服务的 URL 将发布到[Azure 市场 （数据市场）](https://publish.windowsazure.com/workspace/) 
4. 一旦提交，检查您的服务，需要待批准之前您的客户可以开始购买它。 发布过程可能需要几个工作日。 

## 遍历
###步骤 1︰ 创建并发布机器学习请求-响应服务 (RR)###
 如果您没有完成此操作，请看一看此[遍历](machine-learning-walkthrough-5-publish-web-service.md)。

###步骤 2︰ 将服务部署到生产环境，并获得 API 密钥和 OData 端点信息###
1. 从[Azure 的管理门户](http://manage.windowsazure.com)，在左侧的导航栏中，选择**机器学习**选项并选择您的工作区。 

2. 单击**WEB 服务**选项卡上，选择您想要发布到市场上的 web 服务。

    ![Azure 的市场][workspace]

3. 选择要在市场上使用的终结点。 如果尚未创建任何其他终结点，您可以选择**默认**终结点。

4. 一旦您单击了该终结点上，您将能够查看的**API 密钥**。 您将需要这段以后在步骤 3 中的信息，因此请它的一个副本。

    ![Azure 的市场][apikey]

5. 单击**请求/响应**方法，我们不支持向市场发布的批处理执行服务这一时刻。 这样，就会对请求/响应方法的 API 帮助页。

6. 复制**OData 端点地址**，您将需要此信息以后在第 3 步。

    ![Azure 的市场][odata]




将服务部署到生产环境。



###步骤 3︰ 使用已发布的 web 服务的 URL 发布到 Azure 市场 (DataMarket)###

1.  导航到[Azure 市场 （数据市场）](http://datamarket.azure.com/home) 
2.  单击**发布**链接在页面的顶部。 这会将您带到[Microsoft Azure 发布门户](https://publish.windowsazure.com)
3.  单击**发布服务器**部分注册为发布服务器。
4.  在创建新报价时，选择**数据服务**，然后单击**创建新的数据服务**。 
 
    ![Azure 的市场][image1]

    <br />


5.  在**计划**下提供了有关您提供的服务，包括定价计划信息。 如果您将提供免费或付费服务的决定。 若要获得报酬，提供支付信息，如您的银行和税务信息。

6.  在**市场营销**下提供了有关您的优惠，例如标题和说明提供的信息。

7.  在**定价**下，可以将价格设置为特定国家/地区，您计划或您的优惠，让系统"autoprice"。

8. **数据服务**选项卡上，单击**Web 服务**作为**数据源**。

    ![Azure 的市场][image2]

9.  上述步骤 2 中所述，从 Azure 的管理门户，获取 web 服务 URL 和 API 密钥。

10. 在市场数据服务安装程序对话框中，粘贴的**服务 URL**文本框中的 OData 端点地址。

11. 对于**身份验证**，选择作为**身份验证方案**的**标头**。

    - 输入**标题名**为"授权"。
    - **标头值**，输入"载体"（不带引号），单击**空格键，**然后粘贴 API 密钥。
    - 选择**此服务是 OData**复选框。
    - 单击**测试连接**以测试连接。

12. 在**类别**，请确保选中了**机器学习**。

13. 当您完成输入您提供的所有元数据**发布**，然后**转移到推**上单击。 在这种情况下，您将需要修复的所有其他问题的通知。

14. 确保完成所有有待解决的问题后，请单击**请求批准将推送到生产**上。 发布过程可能需要几个工作日。 


[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[工作区]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png
 

测试
