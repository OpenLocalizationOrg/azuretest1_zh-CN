---
ms.openlocfilehash: b7874ccb1bdc56a59821a57eccd8dd40acfd4c5e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="管理机器学习区 |Microsoft Azure" 
    description="管理对 Azure 机器学习的工作区的访问和部署和管理 ML API web 服务" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/10/2015" 
    ms.author="garye"/>


# 管理 Azure 机器学习区 
使用 Azure 的管理门户，您可以管理您机器学习工作区︰

- 监视该工作区的使用方式
- 配置工作区来允许或拒绝访问
- 管理工作区中创建 web 服务
- 删除工作区

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

此外，仪表板选项卡提供了您的工作区使用情况的概述和快速浏览您的工作区信息。  

> [AZURE.TIP] 在 Azure 机器学习 Studio 中，在**WEB 服务**选项卡上，您可以添加、 更新或删除机器学习 web 服务。

管理工作区︰

1.  登录到您的 Microsoft Azure 帐户-使用 Azure 的订阅与相关联的帐户。
2.  在[Azure 的管理门户](https://manage.windowsazure.com/)，在 Microsoft Azure 服务面板中，单击**机器学习**。
3.  单击您想要管理的工作区。

工作区页面有三个选项卡︰

- **仪表板**-允许您查看工作区的使用情况和信息
- **配置**-使您能够管理访问工作区
- **WEB 服务**-允许您管理此工作区中已发布的 web 服务

  
## 若要监视该工作区的使用方式

单击**仪表板**选项卡。

从仪表板可以查看您的工作区的总体使用情况并得到快速的工作区信息。

- **计算**图表显示正在使用的工作区的计算资源。 您可以更改视图以显示相对或绝对数值，可以更改图表中显示的时间范围。
- **使用概述**显示 Azure 存储所使用的工作区。
- **快速浏览**提供了工作区信息和有用的链接的摘要。

> [AZURE.NOTE] **登录到 ML Studio**链接打开使用 Microsoft 帐户您当前登录到的计算机学习 Studio。 您用于登录到 Azure 门户创建工作区的 Microsoft 帐户自动没有打开该工作区的权限。 要打开工作区，您必须登录到定义的工作区中，所有者为 Microsoft 客户或您需要加入工作区的所有者从收到的邀请。 


## 若要授予或挂起用户对的访问 ##

单击**配置**选项卡。

您可以从配置选项卡
 
- 通过单击拒绝暂停访问机器学习区。 用户将不再能够在机器学习 Studio 中打开工作区。 为恢复访问，请单击允许。
- 通过指定不同的 Microsoft 帐户更改工作区所有者。 

若要管理谁有权访问计算机学习 Studio 中的工作区，单击**登录到 ML Studio**中的**仪表板**选项卡 （请参见上面有关**登录到 ML Studio**的注释）。 在机器学习 Studio 中打开工作区。 在此，请单击**设置**选项卡，然后**用户**。 您可以单击**邀请更多的用户**来授予用户访问工作区中，或选择用户并单击**删除**。


## 此工作区中的 web 服务进行管理

单击**WEB 服务**选项卡。

这将显示从该工作区发布 web 服务的列表。
若要管理 web 服务，请单击打开 web 服务页的列表中的名称。

Web 服务可能有一个或多个定义的终结点。 

- 您可以定义"默认"终结点以及其他终结点。 若要添加终结点，请单击页面底部**添加终结点**。

- 删除终结点 （您不能删除"默认"终结点），终结点名称，除行上的任意位置单击，然后单击页面底部**删除终结点**。 这从 web 服务中删除终结点。
 
    > [AZURE.NOTE] 如果应用程序使用的 web 服务终结点中删除终结点时，该应用程序会收到错误的下次它尝试访问该服务。

单击以将其打开的 web 服务终结点的名称。 使用图表显示正在使用的 web 服务终结点的计算和预测的资源。 您可以更改视图以显示相对或绝对数值，可以更改图表中显示的时间范围。

此页还提供了您需要能够访问使用 REST API，web 服务终结点的信息。 有关详细信息，请参阅[如何使用 Azure 机器学习的 web 服务][使用]。 

您可以从此页到 Azure 数据市场发布 web 服务。 有关详细信息，请参阅[发布 Azure 机器学习 Web 服务到 Azure 市场][市场]。

[使用]: machine-learning-consume-web-services.md
[市场]: machine-learning-publish-web-service-to-azure-marketplace.md

 
测试
