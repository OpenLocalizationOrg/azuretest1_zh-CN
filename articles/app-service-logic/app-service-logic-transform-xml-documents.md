---
ms.openlocfilehash: a7fbbb2014916a13fe6b7a114678f7bd9247ec45
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="BizTalk 转换" 
    description="了解如何将转换到另一个架构的 XML 文档。" 
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
    ms.date="06/30/2015"
    ms.author="anuragdalmia"/>

#BizTalk 转换


## 概述
简单地说，BizTalk 转变 API 的应用程序将数据从一种格式到另一种格式。 例如，可能需要送货地址和帐单地址从采购订单并将它们插入到发票单据。 或者您可以使用传入消息所包含的*YearMonthDay*格式的当前日期。 您要重新格式化为*MonthDayYear*格式的日期。 

您可以在 Microsoft Azure 应用程序服务使用转变 API 应用程序这样做。 转换或映射的源 XML 架构 （输入） 和包含目标 XML 架构 （输出）。 不同的内置功能可用于帮助操纵或控制的数据，包括字符串操作、 条件分配、 算术表达式、 日期时间格式化程序，和甚至循环构造。 

在使用[Microsoft Azure BizTalk 服务 SDK](http://www.microsoft.com/download/details.aspx?id=39087)的 Visual Studio 中创建映射。 创建和测试映射完成后，您上传到 BizTalk 转换 API 应用程序映射 (.trfm)。

其他功能包括︰

- 在图中创建的变换可以很简单，如姓名和地址从一个文档复制到另一个。 或者，您可以创建更复杂的转换使用的框映射操作。
- 多个映射运算或函数是易于使用，包括字符串、 日期时间函数，等等。
- 可以执行架构之间的直接数据副本。 在 BizTalk 映射器，这是简单，只需绘制一条线连接与目标架构中的对应项的源架构中的元素。
- 在创建映射时，您可以查看地图，包括查看所有的关系和您创建的链接的图形化表示。
- 使用**测试映射**功能来添加示例 XML 消息。 通过简单的单击，可以测试您创建的该图并查看生成的输出。
- 将上载现有 Azure BizTalk 服务映射 (.trfm)，使用转变 API 应用程序的所有好处。
- 在创建映射时，您不需要添加架构。 准备好您的映射时，将其添加到转换 API 应用程序，您就可以转。 
- XML 格式的支持。


## 从连接器 API 的应用程序下载架构
您可以从 API 应用程序摘要页面下载接口，例如︰ SQL、 SAP 和 SharePoint 的 XML 架构。 例如，如果您想要为特定的 SAP 接口 API 应用程序下载的 XML 架构，API 的应用程序浏览，并打开摘要页。 选择**下载架构**和具有对应的 SAP 操作的所有架构的 zip 文件下载到您的计算机。 您可以使用架构来创作在 Visual Studio 中的映射 (.trfm)。


## 创建和添加映射
使用[Microsoft Azure BizTalk 服务 SDK](http://www.microsoft.com/download/details.aspx?id=39087)，这可免费下载的 Visual Studio 中创建的转换或映射。 

创建地图的帮助信息，请参阅[创建 Visual Studio 中的映射](http://aka.ms/createamapinvs)。 地图已创建，并可以投入生产后，可以添加到 BizTalk 转变 API 的应用程序在 Azure 管理门户中创建映射 （.trfm 文件）。 

如果映射更改或修改其上载后，您可以上载已更新的映射和它将替换已有的映射转换 API 应用程序中。

1.  选择在 Azure 管理门户 （在屏幕的左侧） 上的**浏览**，然后选择**API 的应用程序**。 如果**API 的应用程序**没有显示，选择**所有内容**，并从可用列表中选择**API 的应用程序**︰

    ![][7]

2.  显示列表中所有**API 的应用程序**创建的 Azure 订购︰

    ![][8]

3.  选择 BizTalk 转变 API 的应用程序在上一节中创建。

4.  配置刀片式服务器 API 应用程序打开。 您可以查看组件部分中的**映射**︰

    ![][9]

5.  选择**映射**的映射列表中打开新的刀片。

6.  **添加映射**上选择图标以打开**添加映射**刀片的顶部︰

    ![][10]

7.  选择本地计算机中的文件图标，通过浏览查找映射文件 (.trfm)。

8.  创建新的映射并选择**确定**。 它所示的映射的列表。


## 在逻辑应用程序中使用 BizTalk 转换 API 的应用程序
一旦地图已编写并测试，就可供消耗。

1. 逻辑应用程序中，在 BizTalk 转换位于右侧库。 从库中选择**BizTalk 转换服务**。 该转换将被添加到流︰

    ![][11]

2. 选择的**转换**操作。 显示输入的参数︰

    ![][12]

3. 输入完成的**转换**操作配置以下参数︰
         
    - 输入 XML
        - 输入转换 API 应用程序中映射的源架构一致的有效 XML 内容。 这可能是前一操作，例如调用 RFC — SAP 或插入所至表 – SQL 逻辑应用程序中的输出。
        
    - 映射名称 （可选）
        - 在您转换的 API 应用程序中输入已上载一个有效的映射名称。 如果输入没有地图，地图会自动选择基础的输入的 XML 符合源架构。

    ![][13]

4. 可以在后续操作中的逻辑应用程序使用输出 XML 操作的输出。

<!--Image references-->
[1]: ./media/app-service-logic-transform-xml-documents/Create_Everything.png
[2]: ./media/app-service-logic-transform-xml-documents/Create_Marketplace.png
[4]: ./media/app-service-logic-transform-xml-documents/Search_TransformAPIApp.png
[5]: ./media/app-service-logic-transform-xml-documents/Transform_APIApp_Landing_Page.png
[6]: ./media/app-service-logic-transform-xml-documents/New_TransformAPIApp_Blade.png
[7]: ./media/app-service-logic-transform-xml-documents/Browse_APIApps.png
[8]: ./media/app-service-logic-transform-xml-documents/Select_APIApp_List.png
[9]: ./media/app-service-logic-transform-xml-documents/Configure_Transform_APIApp.png
[10]: ./media/app-service-logic-transform-xml-documents/Add_Map.png
[11]: ./media/app-service-logic-transform-xml-documents/Transform_action_flow.png
[12]: ./media/app-service-logic-transform-xml-documents/Transform_Inputs.png
[13]: ./media/app-service-logic-transform-xml-documents/Transform_configured.png
[14]: ./media/app-service-logic-transform-xml-documents/Download_Schemas.png



 
测试
