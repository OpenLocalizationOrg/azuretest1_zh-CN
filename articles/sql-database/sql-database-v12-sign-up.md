---
ms.openlocfilehash: 7382d447a5c26360dc19fa02e99529b8bd2220d7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="演练︰ 激活新的 SQL 数据库更新 V12"
    description="描述尝试 V12 Azure SQL 数据库版本，使用新的 Microsoft Azure 门户 UI 的步骤。"
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jeffreyg"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management" 
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/28/2015"
    ms.author="genemi"/>


# 演练︰ 激活新的 SQL 数据库更新 V12

本主题描述激活 Azure SQL 数据库 V12 选项中，您可以执行的步骤在 2014 年 12 月，首次推出由 Microsoft。

尝试最新的 V12 首先需要订阅 Microsoft Azure 或至少一个[免费试用](http://azure.microsoft.com/pricing/free-trial/)订阅。

您可以通过在[http://portal.azure.com/](http://portal.azure.com/)，而不是[旧的 Azure 门户](http://manage.windowsazure.com/)使用新的 Azure 预览门户激活 V12。 V12 激活您的订阅后，V12 创建和升级选项将被解锁 Azure 预览门户中。 然后用户可以启动从刀片式服务器或数据库刀片式服务器[创建](sql-database-create.md)的工作流。

> [AZURE.NOTE]
> 测试数据库、 数据库副本或新的数据库升级到预览是很好的候选。 在预览期间后才应等待取决于您的业务的生产数据库。

有关为 V12 升级的详细信息，请参阅[计划和准备升级到 Azure SQL 数据库 V12](sql-database-v12-plan-prepare-upgrade.md)。


## 答︰ 安全身份验证

第一步是确保您有足够的授权，以激活您的订阅的 V12。 当您尝试激活 V12 选项时，执行授权检查以验证您具有足够的权限订阅中。

 若要尝试 V12 必须之一的下列授权︰

- 预订所有者
- 订阅是联管理员

Azure 帐户的详细信息，请参阅[管理帐户、 预订和管理角色](http://msdn.microsoft.com/library/hh531793.aspx)。

## B。 在 Azure 预览门户 UI 的步骤

本节介绍了您可以按照在 Azure 预览门户用户界面激活 V12 选项中的一次单击序列。 激活此选项后，它仍可此后。

激活的所有方案都使用相同的基本思想。 当您第一次尝试[创建新的 SQL 数据库服务器](sql-database-create.md)时，显示提供一个复选框，您可以选择要激活您的权限，以使用 V12 版本标记为**最新的更新 （预览）**的刀片。 一旦激活权限，永远不会再次看到该复选框。 相反，您看到是**|没有**可用于指定是否要使用 V12 您新的服务器控件。 如果选择**否**，您将创建 V11 服务器，（所报告的选择 @@VERSION;）。

### B.1 是 |无控制的 V12 版本

已激活的 V12 选项权限后，您将看到**是 |没有**圆圈下面的 Azure 预览门户屏幕快照中的控件。

![YesNoOptionForTheV12Preview][Image1]


## C。 接下来是什么

下一步，下面的主题介绍您可以使用 SQL 数据库 V12 的方法。

- [在 SQL 数据库更新 V12 创建数据库](sql-database-create.md)


<!-- References, Images. -->
[Image1]: ./media/sql-database-v12-sign-up/V12Preview-YesNo-Option-New-SQLDatabase-Server-Newserver-Screenshot-e23.png

 