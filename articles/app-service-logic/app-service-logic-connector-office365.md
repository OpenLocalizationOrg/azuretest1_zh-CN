---
ms.openlocfilehash: 1947e1186e3f52973235f39a9d9160b2edb7a210
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="逻辑应用程序在使用 Office 365 连接器 |Microsoft Azure 应用程序服务"
   description="如何创建和配置 Office 365 接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
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


# 开始使用 Office 365 连接器并将其添加到您的逻辑应用程序
连接到您的 Office 365 帐户来发送和接收电子邮件，并管理您的日历和联系人。 您可以执行各种操作，如发送、 接收和获取电子邮件、 创建和删除日历中的事件、 更新、 获取和创建删除联系人。

可以触发逻辑应用程序基于各种数据源和提供接口来获取和处理数据作为流程的一部分。 对业务工作流程和过程数据作为逻辑应用程序在此工作流的一部分，您可以添加 Office 365 连接器。 

**基本操作**

- 新邮件 （触发器）
- 发送邮件
- 答复邮件
- 发送事件
- 添加联系人

## 创建 O365 连接器 API 应用程序
连接器可以创建逻辑应用程序中，或直接从 Azure 市场创建。 若要从市场创建连接器︰  

1. 在 Azure 的 startboard 中，选择**市场**。
2. "Office 365 连接器"搜索、 选择它，然后选择**创建**。
3.  通过提供托管计划，该资源组的详细信息 API 的应用程序的名称来配置 Office 365 连接器︰  
![][21]


## 创建一个逻辑应用程序
让我们创建一个简单的逻辑应用程序获取 （在您销售查询电子邮件 id-说 sales@contoso.com) 收到一封电子邮件时触发。 而且，它创建一个事件、 添加发件人详细信息的联系人，将电子邮件发送到您的个人帐户和最后发送确认回复。

1.  登录到 Azure 门户，在新-> Web + 手机-> 逻辑应用程序上单击︰  
![][1]

2.  在创建逻辑应用程序页中，提供了所需的详细信息，例如名称、 应用程序服务计划和位置︰  
![][2]

3.  单击触发器和操作和逻辑应用程序编辑器中打开︰  
![][3]

4.  从库以将其添加到流中的此资源组中的 API 应用程序部分中选择 Office 365 触发器︰  
![][4]

6.  连接到 Office 365 要求授权逻辑应用程序能够访问您的帐户。 单击授权提供 Office 365 提供登录凭据︰  
![][5]

7.  您将被重定向到 Office 365 提供登录页面，可以使用您的 Office 365 提供帐户凭据进行身份验证︰  
![][6]  
![][7]

8.  完成授权后，将显示 Office 365 触发器︰  
![][8]

9.  将显示选择新建电子邮件触发器和输入的参数。


10. 更改触发器频率为分钟，然后单击 ✓:  
![][9]

11. Office 365 新电子邮件触发器配置，您可以看到也显示的输出参数︰  
![][10]

12. 选择 Office 365 连接器库和新的"Office 365"中的最近使用一节中添加操作。

13. 从操作和发送事件显示操作的输入的参数列表中选择发送事件:  
![][11]

14. 指定事件的详细信息，请单击 ✓︰  
![][12]

15. 选择 Office 365 连接器库和新的"Office 365"中的最近使用一节中添加操作。

16. 从操作和添加联系人显示操作的输入的参数列表中选择添加联系人:  
![][13]

17. 单击电子邮件地址字段旁边的 + 并选择从输出字段值从触发器︰  
![][14]

18. 单击 ✓ 并操作配置已完成。  
![][15]

19. 选择 Office 365 连接器库和新的"Office 365"中的最近使用一节中添加操作。


20. 选择发送电子邮件操作和发送电子邮件操作会显示输入的参数的列表中︰  
![][19]

21. 提供发送电子邮件所需的详细信息。 您可以通过键入某些内容构建一条消息像下面。 发送电子邮件操作配置后，单击 ✓︰

        Body - @concat('You got a new sales enquiry from',triggers().output.body.From)

    ![][20]
22. 选择 Office 365 连接器库和新的"Office 365"中的最近使用一节中添加操作。


23. 从操作和答复到显示的操作的输入的参数列表中选择答复到:  
![][16]

24. 单击 + 旁边发件人字段，然后选择从触发器的输出值邮件 id 并单击 ✓︰  
![][17]

25. 逻辑应用程序编辑器屏幕上单击确定，然后单击创建。 需要大约 30 秒钟创建完成。

26. 向配置与触发器的帐户发送电子邮件，您应该看到在您的个人邮件帐户、 日历事件和联系人的电子邮件在您办公室的邮件帐户。 此外，应得到重新确认销售查询做出响应很快的答复。

## 用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。

<!--Image references-->
[1]: ./media/app-service-logic-connector-office365/1_New_Logic_App.png
[2]: ./media/app-service-logic-connector-office365/2_Logic_App_Settings.png
[3]: ./media/app-service-logic-connector-office365/3_Logic_App_Editor.png
[4]: ./media/app-service-logic-connector-office365/4_Select_Office365_Gallery.png
[5]: ./media/app-service-logic-connector-office365/5_Office365_Authorize.png
[6]: ./media/app-service-logic-connector-office365/6_Office365_Login.png
[7]: ./media/app-service-logic-connector-office365/7_Office365_User_Consent.png
[8]: ./media/app-service-logic-connector-office365/8_Office365_Trigger.png
[9]: ./media/app-service-logic-connector-office365/9_Office365_Trigger_Settings.png
[10]: ./media/app-service-logic-connector-office365/10_Office365_Trigger_Configured.png
[11]: ./media/app-service-logic-connector-office365/11_Office365_Actions_List.png
[12]: ./media/app-service-logic-connector-office365/12_Office365_Create_Event_Inputs.png
[13]: ./media/app-service-logic-connector-office365/13_Office365_Add_Contact_Inputs.png
[14]: ./media/app-service-logic-connector-office365/14_Office365_Add_Contact_Email_FromTrigger.png
[15]: ./media/app-service-logic-connector-office365/15_Office365_Add_Contacts_Configured.png
[16]: ./media/app-service-logic-connector-office365/16_Office365_Reply_To_Inputs.png
[17]: ./media/app-service-logic-connector-office365/17_Office365_Reply_To_MessageId.png
[18]: ./media/app-service-logic-connector-office365/18_Office365_Reply_To_Configured.png
[19]: ./media/app-service-logic-connector-office365/19_Office365_Send_Inputs.png
[20]: ./media/app-service-logic-connector-office365/20_Office365_Send_Configured.png
[21]: ./media/app-service-logic-connector-office365/21-create-new-o365-api-app.png

测试
