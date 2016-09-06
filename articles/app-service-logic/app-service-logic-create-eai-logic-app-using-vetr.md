---
ms.openlocfilehash: ac7005b31d256aef8276945e61519b03dd32439a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="创建 EAI 的逻辑应用程序使用 VETR |Microsoft Azure"
   description="本主题介绍 BizTalk XML 服务验证、 编码和转换的功能。"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="rajeshramabathiran"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="06/24/2015"
   ms.author="rajram"/>


# 创建 EAI 的逻辑应用程序使用 VETR

大多数企业应用程序集成 (EAI) 情况下的媒介在源和目标之间的数据。 这类方案通常有一组常见的要求︰

- 确保来自不同系统的数据格式正确
- "查找"对传入的数据来做出决定
- 将数据从一种格式转换为另一个 （例如，从 ERP 系统的数据格式与 CRM 系统的数据格式）
- 将数据路由到所需的应用程序或系统

本文介绍一个通用的集成模式:"单向消息中介"或验证、 Enrich、 变换 (路由） 的 VETR VETR 模式调节临近范围源实体与目标实体之间的数据。 通常的源和目标是数据源。

考虑接受订单的网站。 用户投递到系统使用 HTTP 的订单。 在后台系统验证传入的数据的正确性、 标准化，并将其保持在服务总线队列中进行进一步处理。 系统将从队列，它应为某一特定格式的订单。 因此，端到端流程为︰

**HTTP** > **验证** > **变换** > **服务总线**

![基本的 VETR 流][1]

以下的 BizTalk API 应用程序可帮助生成此模式︰

* **HTTP 触发器**的触发器消息事件源
* **验证**-验证传入的数据的正确性
* **转换**的转换数据来自下游系统所需的传入格式的格式
* **服务总线连接器**的目标实体发送数据的位置


## 建立基本的 VETR 模式
### 基础知识

在 Azure 管理门户中，单击屏幕左下方的**+ 新建**按钮，然后单击**逻辑应用程序**。 选择名称、 位置、 订阅、 资源组和起作用的位置。 资源组作为容器的应用程序和所有您的应用程序的资源转到同一个资源组。

接下来，让我们添加触发器和操作。


## 添加 HTTP 触发器

1. 从库来创建一个新的侦听器选择**HTTP 侦听程序**。 **HTTP1**调用它。
2. 将**自动发送响应？**设置为 false。 配置设置开机_自检_的_HTTP 方法_和_/OneWayPipeline__相对 URL_设置触发器操作。

![HTTP 触发器][2]


## 添加验证操作

现在，让我们输入运行时触发触发器 — 即 HTTP 端点上接收时调用的操作。

1. 从库中添加**BizTalk XML 验证器**并将其命名为_(Validate1)_来创建的实例。
2. 配置 XSD 架构验证传入的 XML 消息。 选择_验证_操作并选择_triggers('httplistener').outputs。内容_作为_inputXml_参数的值。

现在，验证操作是 HTTP 侦听程序后的第一个操作。 同样，让我们添加操作的其余部分。

![BizTalk XML 验证器][3]


## 添加转换操作
让我们来配置传入数据进行正态化转换。

1. 从库中添加**转换**。
2. 若要配置转换来转换传入的 XML 消息，选择**转换**操作作为携带出何时调用此 API 和选择的操作```triggers(‘httplistener’).outputs.Content```作为_inputXml_的值。 地图是一个可选参数，因为传入的数据相匹配的所有已配置的转换，并将应用仅显示那些与架构相匹配。
3. 最后，验证成功时才会运行转换。 要配置此条件，请单击右上角的齿轮图标并选择_添加条件得以满足_。 将条件设置为 ```equals(actions('xmlvalidator').status,'Succeeded')```


![BizTalk 转换][4]


## 添加服务总线连接器
接下来，让我们添加目标-服务总线 Queu-数据写入。

1. 从库中添加**服务总线连接器**。 将**名称**设置为_Servicebus1_，设置**连接字符串*保存到您的服务总线实例的连接字符串设置**实体名称**的_队列_中，并跳过**订阅名称**。
2. 选择**发送消息**的操作，并将该动作的**邮件**字段设置为_actions('transformservice').outputs。OutputXml_

![服务总线][5]


## 发送的 HTTP 响应
完成管道处理后，重新发送 HTTP 响应的成功和失败与以下步骤︰

1. 从库中添加**HTTP 侦听程序**并选择**发送 HTTP 响应**操作。
2. **响应内容**设*管道处理完成*，**响应状态代码**为*200*来指示 HTTP 200 确定，并为表达式的**条件** ```@equals(actions('servicebusconnector').status,'Succeeded')```

重复上述步骤失败以及发送的 HTTP 响应。 更改**条件** ```@not(equals(actions('servicebusconnector').status,'Succeeded')).```


## 完成
每次有人向 HTTP 端点发送消息，它将触发应用程序并执行您刚刚创建的操作。 管理此类逻辑的应用程序创建、 单击 Azure 管理门户中**浏览**并单击**逻辑应用程序**。 单击您的应用程序的详细信息，请参阅。


<!--image references -->
[1]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BasicVETR.PNG
[2]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/HTTPListener.PNG
[3]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkXMLValidator.PNG
[4]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkTransforms.PNG
[5]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/AzureServiceBus.PNG

测试
