---
ms.openlocfilehash: 819fc0e15e4f03883f266a1268ce339abb85431d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在应用程序逻辑中使用 QuickBooks 连接器 |Microsoft Azure 应用程序服务"
   description="如何创建和配置 QuickBooks 接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
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
   ms.author="sameerch"/>


# 开始使用 QuickBooks 连接器并将其添加到您的逻辑应用程序
使用 QuickBooks 连接器来创建和修改不同 QuickBooks 实体。 下表列出了支持的实体︰

Entities|说明
---|---
Account|帐户的帐户图表的一个组成部分，是会计的一部分。 用于记录总金额分配对特定的用途
CreditMemo|CreditMemo 是代表退款或信用的付款或付款的商品或服务的已售出的部分金融业务。
客户|客户是您的业务提供的产品或服务的使用者。
估计|估计值表示从业务给客户的货物或服务销售财务交易的方案，包括建议定价。
发票|发票表示，客户购买产品或服务以后销售形式。 可以在此处找到有关使用发票数据模型的附加信息。
项目|项是一件事情，您的公司购买、 销售，或重新销售，产品，如发货和处理费用、 折扣和销售税 （如果适用）。  项都显示为一条线上发票或其他销售形式。
SalesReceipt|此实体表示销售给客户的收据。

可以触发逻辑应用程序基于各种数据源和提供接口来获取和处理数据作为流程的一部分。 对业务工作流程和过程数据作为逻辑应用程序在此工作流的一部分，您可以添加 QuickBooks 连接器。 

##QuickBooks 操作 ##
以下是可用 QuickBooks 连接器中的不同操作。

操作|说明
---|---
读实体|读实体对象
创建或更新实体|创建或更新实体对象。 如果它不存在其他新更新对象创建对象
删除|该操作会删除所选实体的指定的对象
查询|查询操作是创建针对实体的指导的查询的方法。

##创建 QuickBooks 连接器 API 的应用程序##
1.  打开使用 + 新 Azure 市场上右下角的 Azure 门户选项。
2.  浏览到"Web 和移动 > API 的应用程序"，然后搜索"QuickBooks"。
3.  QuickBooks 连接器配置为主持规划资源组提供详细信息并选择 API 的应用程序的名称。

    ![][13]
4. 您有兴趣读/写在程序包设置的 QuickBooks 实体进行配置。

通过此功能，您现在可以创建 QuickBooks Conenctor API 的应用程序。


##创建一个逻辑应用程序##
让我们创建一个简单的逻辑应用程序 QuickBooks 中创建一个帐户并更新的同一个帐户的类别类型。

1.  登录到 Azure 门户网站，请单击新建-> Web + 手机-> 逻辑应用程序

    ![][1]

2.  在创建逻辑应用程序页中，提供所需的详细信息，例如名称、 应用程序服务计划和位置。

    ![][2]

3.  单击触发器和操作，逻辑应用程序编辑器屏幕出现。

    ![][3]

4.  选择手动运行此逻辑意味着此逻辑应用程序可以在仅手动调用。


5.  在库以查看所有可用 API 应用程序中展开 API 应用程序中此资源组。 从库中选择 QuickBooks 连接器和 QuickBooks 连接器添加到流。


6.  必须进行身份验证和授权逻辑应用程序执行操作以您的名义，如果 QuickBooks 联机。 启动授权，请单击授权 QuickBooks 连接器上。

    ![][4]

7.  单击授权将打开 QuickBook 的身份验证对话框。 提供您要在其执行操作的 QuickBooks 帐户的登录详细信息。

    ![][5]

8. 授权逻辑应用程序访问您的帐户以同意对话框上单击授权来执行操作以您的名义。

    ![][6]

9.  完成授权后，将显示所有操作。

    ![][7]

10. 将显示选择创建或更新帐户操作和输入的参数。

    ![][8]

11. 提供名称和帐户类型，然后单击 ✓。

    ![][9]

12. 从库中添加操作获取新 QuickBooks 最近使用部分选择 QuickBooks 连接器。

13. 选择创建或更新帐户列表中的操作和输入参数的操作的显示。

    ![][10]

14. 单击 Id，可以选择创建帐户操作的输出中的 id 值旁边的 +。

    ![][11]

15. 提供科目类型为的新值，单击 ✓。

    ![][12]

16. 逻辑应用程序编辑器屏幕上单击确定，然后单击创建。 需要大约 30 秒钟创建完成。

17. 浏览新创建的逻辑应用程序，然后单击运行以启动运行。

18. 您可以检查获取 QuickBooks 帐户中创建新帐户的名称 Contoso。

## 用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。

<!--Image references-->
[1]: ./media/app-service-logic-connector-quickbooks/1_New_Logic_App.png
[2]: ./media/app-service-logic-connector-quickbooks/2_Logic_App_Settings.png
[3]: ./media/app-service-logic-connector-quickbooks/3_Logic_App_Editor.png
[4]: ./media/app-service-logic-connector-quickbooks/4_QuickBooks_Authorize.png
[5]: ./media/app-service-logic-connector-quickbooks/5_QuickBooks_Login.png
[6]: ./media/app-service-logic-connector-quickbooks/6_QuickBooks_User_Consent.png
[7]: ./media/app-service-logic-connector-quickbooks/7_QuickBooks_Actions.png
[8]: ./media/app-service-logic-connector-quickbooks/8_QuickBooks_Create_Account.png
[9]: ./media/app-service-logic-connector-quickbooks/9_Create_Account_OK.png
[10]: ./media/app-service-logic-connector-quickbooks/10_QuickBooks_Update_Account.png
[11]: ./media/app-service-logic-connector-quickbooks/11_Record_ID_from_Create.png
[12]: ./media/app-service-logic-connector-quickbooks/12_Update_Account_Address.png
[13]: ./media/app-service-logic-connector-quickbooks/13_Create_new_quickbooks_connector.png

测试
