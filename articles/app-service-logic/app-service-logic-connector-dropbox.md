---
ms.openlocfilehash: 5d0af68c0ff4282562392e3a839e8db09705157c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="收存箱接头用逻辑应用程序 |Microsoft Azure 应用程序服务"
    description="如何创建和配置的收存箱接头或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
    authors="anuragdalmia"
    manager="dwrede"
    editor=""
    services="app-service\logic"
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2015"
    ms.author="sameerch"/>

# 入门收存箱连接器并将其添加到您的逻辑应用程序
连接到要上载或下载文件的收存箱帐户。 可以触发逻辑应用程序基于各种数据源和提供接口来获取和处理数据。 业务工作流程和过程数据作为逻辑应用程序在此工作流的一部分，您可以添加的收存箱接头。

## 触发器和操作

触发器启动基于特定事件类似的一个新的邮件到达一个新实例。 对策是结果，像后收到一封新邮件，然后将该文件上载到收存箱。

收存箱接头可以用作 JSON 和 XML 格式的逻辑应用程序，并支持数据中的操作。 收存箱接头有以下触发器和操作可用︰

触发器 | 操作
--- | ---
无 | <ul><li>删除文件</li><li>获取文件</li><li>上载文件</li><li>文件列表</li></ul>


## 为您的逻辑应用程序创建一个连接器，收存箱
连接器可以创建逻辑应用程序中，或直接从 Azure 市场创建。 若要从市场创建连接器︰  

1. 在 Azure 的 startboard 中，选择**市场**。
2. "收存箱接头"搜索、 选择它，然后选择**创建**。
3. 输入的名称、 应用程序服务计划，以及其他属性︰  
    ![][1]
    - **位置**-选择您想要进行部署的连接器的地理位置
    - **预订**-选择您想要在中创建此连接器的订阅
    - **资源组**的选择或创建资源组连接器所在的位置
    - **应用程序服务计划**的选择，或创建 web 宿主计划
    - **定价层**-选择连接器定价层
    - **名称**-授予的收存箱连接器名称  
4. 选择**创建**。


## 在您的应用程序逻辑中使用收存箱接头
一旦创建 API 的应用程序，为您的逻辑应用程序现在为一个操作使用收存箱接头。 若要此操作︰

1.  逻辑应用程序中打开**触发器和操作**来打开逻辑应用程序设计器和配置您的流︰  
    ![][3]
2.  在库列出收存箱接头︰  
    ![][4]
3.  选择要在设计器中自动添加的收存箱连接器。 选择**授权**、 输入您的凭据，并选择**允许**:  
    ![][5]
    ![][6]
    ![][7]

现在，您可以使用收存箱接头在流中。 您可以使用收存箱操作"上载文件"将文件上载到您的收存箱帐户︰  
    ![][8]
    ![][9]

"上载文件"操作的输入的属性按如下配置︰  

- **文件路径**-指定要上载的文件的目标收存箱文件路径。 示例︰ Photos/image.png
- **内容**-指定要上载的文件的内容。 通常情况下，这会从前面的步骤中您的逻辑的应用程序。
- **内容传输编码**-指定 none 或 base64。
- **覆盖**的指定"true"，如果已经存在，则覆盖该文件。

## 用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。

<!-- Image reference -->
[1]: ./media/app-service-logic-connector-dropbox/img1.PNG
[2]: ./media/app-service-logic-connector-dropbox/img2.PNG
[3]: ./media/app-service-logic-connector-dropbox/img3.png
[4]: ./media/app-service-logic-connector-dropbox/img4.png
[5]: ./media/app-service-logic-connector-dropbox/img5.PNG
[6]: ./media/app-service-logic-connector-dropbox/img6.PNG
[7]: ./media/app-service-logic-connector-dropbox/img7.PNG
[8]: ./media/app-service-logic-connector-dropbox/img8.PNG
[9]: ./media/app-service-logic-connector-dropbox/img9.PNG

测试
