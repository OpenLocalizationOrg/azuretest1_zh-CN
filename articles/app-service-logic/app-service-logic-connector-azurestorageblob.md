---
ms.openlocfilehash: c7d392f3aff74f561785c244a281bb5bd43fedf3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="在应用程序逻辑中使用 Azure 存储 Blob 连接器 |Microsoft Azure 应用程序服务" 
   description="如何创建和配置 Azure 存储 Blob 接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它" 
   services="app-service\logic" 
   documentationCenter=".net,nodejs,java" 
   authors="anuragdalmia" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="08/23/2015"
   ms.author="rajram"/>
   
# 开始使用 Azure 存储 Blob 连接器并将其添加到您的逻辑应用程序 
连接到您的 Azure 存储 Blob 可以上载、 下载和删除的 blob 容器中的 blob。 逻辑应用程序中使用连接器作为"工作流"的一部分。 

## 触发器和操作
*触发器*是发生的事件。 例如，在更新订单时或当新客户添加。 *操作*是触发器的结果。 例如，在更新订单时，将通知发送给销售人员。 或者，当添加一个新客户，新客户发送欢迎电子邮件。 

存储 Blob 连接器可以用作 JSON 和 XML 格式的逻辑应用程序，并支持数据中的操作。 目前，没有触发器存储 Blob 连接器。 

存储 Blob 连接器具有以下触发器和操作可用︰ 

触发器 | 操作
--- | ---
无 | <ul><li>获取 Blob︰ 从容器中获取特定的斑点</li><li>上载的 Blob︰ 上载新的斑点或更新现有的斑点</li><li>删除 Blob︰ 从容器中删除特定的斑点</li><li>列表中的 Blob︰ 列出目录中的所有 blob</li><li>快照的 Blob︰ 创建特定的 Blob 的只读快照</li><li>复制 Blob︰ 通过从另一个 Blob 复制创建新的斑点。  Blob 可能以相同的帐户或其他帐户中的源。</li></ul>


## 创建 Azure 存储 Blob 连接器

连接器可以创建逻辑应用程序中，或直接从 Azure 市场创建。 若要从市场创建连接器︰  

1. 在 Azure 的 startboard 中，选择**市场**。
2. "Blob"搜索︰  
    ![选择 Azure 存储 Blob 连接器][2]

3. 选择它，然后选择**创建**。
4. 输入的名称、 应用程序服务计划，以及其他属性。
5. 输入以下程序包设置︰

    名称 | 是否必需 |  说明
--- | --- | ---
容器/SA URI | 是 | 输入的 Blob 容器的 URI。 URI 可能还包括 SAS 令牌。 例如，输入 http://*storageaccountname*.blob.core.windows.net/containername 或 http://*storageaccountname*.blob.core.windows.net/containername?sr=c & si mypolicy & sig = = signatureblah
访问键 | 否 | 请输入一个有效的主要或辅助存储帐户访问键。 如果您使用 SAS 令牌进行身份验证，则将此字段留空。

    ![创建 Azure 存储 Blob 连接器][3]

6. 单击**创建**。

## 在应用程序逻辑中使用 Azure 存储 Blob 连接器
一旦创建了 Azure 存储 Blob 连接器，现在向工作流添加。

1. 创建新的逻辑应用程序︰ 新增-> Web + 手机-> LogicApp。 输入您的逻辑应用程序的属性︰  
    ![创建逻辑应用程序][4]

2. 单击**触发器和操作**。 Workfow 设计器将打开︰  
    ![逻辑应用程序空流设计器][5]

3. 右窗格中，选择您 Azure 存储 Blob 的连接器。 该连接器将列出可用的操作︰  
    ![Azure 存储 Blob 操作列表][10]

4. 在这种情况下，我们将使用**上载 Blob**操作︰  
    ![上载的 Blob 操作的输入][11]

5. 输入的输入的值并选择复选标记来完成配置︰

    输入 | 说明
--- | ---
Blob 路径 | 确定要上载的 Blob 的路径。 该路径相对于配置的容器路径解释。
Blob 写入内容 | 输入要上载的内容和属性的 Blob。
内容传输编码 | 输入无或 Base64。
Overwrite | 当设置为 true，将现有的 Blob 将被覆盖。 如果一个 Blob 已存在相同的路径，则设置为 false 时，返回错误。

请注意，配置的 Azure 存储 Blob 上载 Blob 动作显示两个输入参数为输出参数。

#### 使用上一个操作的输出作为输入到 Azure 存储 Blob 操作
在前面的屏幕快照中，**内容**的值可以是表达式︰

    @triggers().outputs.body.Content

您可以将其设置为所需的任何值。 表达式将逻辑应用触发器的输出，并使用它作为该文件的内容上载。 例如，您要使用的转换输出。 在这种情况下，该表达式将是︰

    @actions('transformservice').outputs.body.OutputXML

## 用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。

<!-- Image reference -->
[2]: ./media/app-service-logic-connector-azurestorageblob/SelectAzureStorageBlobConnector.PNG
[3]: ./media/app-service-logic-connector-azurestorageblob/CreateAzureStorageBlobConnector.PNG
[4]: ./media/app-service-logic-connector-azurestorageblob/CreateLogicApp.PNG
[5]: ./media/app-service-logic-connector-azurestorageblob/LogicAppEmptyFlowDesigner.PNG
[6]: ./media/app-service-logic-connector-azurestorageblob/ChooseBlobAvailableTrigger.PNG
[7]: ./media/app-service-logic-connector-azurestorageblob/BasicInputsBlobAvailableTrigger.PNG
[8]: ./media/app-service-logic-connector-azurestorageblob/AdvancedInputsBlobAvailableTrigger.PNG
[9]: ./media/app-service-logic-connector-azurestorageblob/ConfiguredBlobAvailableTrigger.PNG
[10]: ./media/app-service-logic-connector-azurestorageblob/ListOfAzureStorageBlobActions.PNG
[11]: ./media/app-service-logic-connector-azurestorageblob/BasicInputsUploadBlob.PNG
 
测试
