---
ms.openlocfilehash: c49e1c22d8ff6c4b6c7fc086b839c0fb23fda918
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在应用程序逻辑中使用文件连接器 |Microsoft Azure 应用程序服务"
    description="如何创建和配置文件连接器或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
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
    ms.author="andalmia"/>

# 开始使用文件连接器并将其添加到您的逻辑应用程序
连接到要上载、 下载、 文件系统和您的主机上的文件的详细信息。 可以触发逻辑应用程序基于各种数据源和提供接口来获取和处理数据。 对业务工作流程和过程数据作为逻辑应用程序在此工作流的一部分，您可以添加文件连接器。 

文件连接器用于混合连接到主机的文件系统使用混合连接管理器。

## 为您的逻辑应用程序创建文件连接器 ##
若要使用文件连接器，您需要首先创建文件连接器 API 的应用程序的实例。 进行这种，如下所示︰

1.  打开使用 + 新 Azure 市场 Azure 入口的左侧的选项。
2.  浏览到"市场 > API 应用程序"，然后搜索"文件连接器"。
3.  按以下方法配置文件连接器︰  
![][1]

    - **名称**-一个文件连接器的名称为
    - **程序包设置**
        - **根文件夹**-指定在主机计算机上的根文件夹路径。 如。 D:\FileConnectorTest
        - **服务总线连接字符串**-提供的服务总线连接字符串。 确保服务总线命名空间类型标准的和没有基本以允许使用服务总线继电器。  服务总线中继用于连接到混合连接管理器。
    - **应用程序服务计划**-选择或创建的应用程序服务计划
    - **定价层**-选择连接器定价层
    - **资源组**的选择或创建资源组连接器所在的位置
    - **预订**-选择您想要在中创建此连接器的订阅
    - **位置**-选择您想要进行部署的连接器的地理位置

4. 在上的单击创建。 将创建新的文件连接器

## 配置混合连接管理器 ##
一旦创建 API 的应用程序实例时，浏览到它的仪表板。  这可以通过单击浏览按钮 > API 应用程序 > 选择您文件连接器 API 的应用程序。  在这里需要配置混合连接管理器。
配置和故障排除混合连接管理器的详细信息，请参阅[使用混合连接管理器]。

## 在您的应用程序逻辑中使用文件连接器 ##
一旦创建 API 的应用程序，为您的逻辑应用程序现在为一个操作使用文件连接器。 要执行此操作，您需要︰

1.  创建新的逻辑应用程序并选择同一个资源组具有文件连接器的。 按照说明[创建新的逻辑应用程序]。

2.  打开"触发器和操作"创建逻辑应用程序来打开逻辑应用程序设计器和配置您的流中。

3.  文件连接器会出现在上右手边的库中的"API 此资源组中的应用程序"一节。

4.  您可以通过单击"文件"连接器上的文件连接器 API 的应用程序放到编辑器中。 文件接口公开一个触发器和 4 的操作︰  
![][5]

6.  其中的每一个公开的某些属性。 下图列出了触发和获取文件操作的属性︰  
![][6]

7. 一旦这些配置，可以在您的流中使用触发器和操作。 同样，其他操作可以同时配置。

> [AZURE.NOTE] 之后它被成功地从文件夹中读取文件触发器将删除该文件。

## 文件连接器 REST Api ##
若要使用外部应用程序的逻辑接口，可以利用公开连接器的 REST Api。 您可以使用浏览此 API 定义-> Api 的应用程序的视图-> 文件连接器。 现在，请单击摘要部分以查看所有通过此接口公开的 Api 在 API 定义镜头上︰  
![][7]

找在[文件连接器 API 定义]的 Api 的详细信息。

## 用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。

<!-- Image reference -->
[1]: ./media/app-service-logic-connector-file/img1.PNG
[5]: ./media/app-service-logic-connector-file/img5.PNG
[6]: ./media/app-service-logic-connector-file/img6.PNG
[7]: ./media/app-service-logic-connector-file/img7.PNG

<!-- Links -->
[创建新的逻辑应用程序]: app-service-logic-create-a-logic-app.md
[文件连接器 API 定义]: https://msdn.microsoft.com/library/dn936296.aspx
[使用混合连接管理器]: app-service-logic-hybrid-connection-manager.md

测试
