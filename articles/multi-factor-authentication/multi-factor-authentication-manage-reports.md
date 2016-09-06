---
ms.openlocfilehash: 583e720c17757aef3418755814859650d8303b7e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 的多因素身份验证报告" 
    description="描述了如何使用 Azure 多因素身份验证功能的报告。" 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2015" 
    ms.author="billmath"/>

# 在 Azure 的多因素身份验证中的报告

Azure 的多因素身份验证提供了可供您和您的组织的多个报表。 可以通过多因素身份验证管理门户访问这些报表。 以下是可用报表的列表。

您可以通过 Azure 管理门户访问报告。

名称| 说明
:------------- | :------------- | 
用法 | 使用率报告的总体使用情况、 汇总用户和用户的详细信息的显示信息。
服务器状态|此报表显示与您的帐户相关联的多因素身份验证服务器的状态。
阻止的用户历史记录|这些报告显示请求，以阻止或允许用户的历史的记录。
跳过的用户历史记录|显示历史记录的请求以避开多因素身份验证的用户的电话号码。
欺诈警报|显示您指定的日期范围内提交的欺诈警报历史记录。
排队|列表报告排队等待处理和它们的状态。 报告完成后提供的链接来下载或查看报表。

## 若要查看报表

1. 登录到[http://azure.microsoft.com](http://azure.microsoft.com)
2. 在左边，选择活动目录。
3. 在顶部选择多因素身份验证提供程序。 这将弹出了多因素身份验证提供程序的列表。
4. 如果您有多个多因素身份验证提供程序，请选择您想要查看欺诈警报报告并单击管理页面底部的一个。 如果您只能有一个，只需单击管理。 这将打开 Azure 多因素身份验证管理门户。
5. 在 Azure 多因素身份验证管理门户网站，在左侧，您会看到查看报告。  从这里您可以选择以上所述的报告。


 
<center>![云](./media/multi-factor-authentication-manage-reports/report.png)</center>


**其他资源**

* [对于用户](multi-factor-authentication-end-user.md)
* [在 MSDN 上的 azure 多因素身份验证](https://msdn.microsoft.com/library/azure/dn249471.aspx)
 