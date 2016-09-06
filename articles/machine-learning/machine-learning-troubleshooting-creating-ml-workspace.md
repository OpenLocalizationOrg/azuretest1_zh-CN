---
ms.openlocfilehash: 8a9adbc9b462f4e75f3350c9464a54a8a3c2ab61
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="疑难解答︰ 创建和连接到机器学习区 |Microsoft Azure" 
    description="创建和连接到 Azure 机器学习工作区中的常见问题的解决方案" 
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
    ms.date="07/01/2015" 
    ms.author="garye"/>


#故障排除指南︰ 创建并连接到一个机器学习工作区

本指南提供了一些解决方案设置 Azure 机器学习工作区时经常遇到难题。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##工作区所有者

当您创建一个新的机器学习工作区时，工作区所有者字段中输入的 ID 必须是有效 Microsoft 帐户 (以前称为 Windows Live™ ID)，例如，john-contoso@live.com 或 john-contoso@hotmail.com。 它不能为非 Microsoft 帐户，如您的公司电子邮件帐户。 若要创建一个免费的 Microsoft 帐户，请转到[www.live.com](http://www.live.com)。 

请注意，登录到 Azure 门户创建工作区所用的帐户不自动具有权限*打开*到该工作区，除非该帐户指定为所有者。 机器学习 Studio 中打开工作区，您必须登录到定义的工作区中，所有者为 Microsoft 客户或您需要收到加入工作区的所有者发出邀请。 从 Azure 的门户，但是，可以*管理*工作区中，其中包括将所有者更改和配置访问权限的能力。

管理工作区的详细信息，请参阅[管理 Azure 机器学习区]。

[管理 Azure 机器学习区]: machine-learning-manage-workspace.md

##允许的区域

机器学习是目前南中心美国地区。 如果您的订阅不包括南中心美国，您可能会看到错误消息，"您有没有预订允许地区。 允许区域︰ 南中心美国。" 

若要解决此问题，如下面的屏幕快照中所示，从 Azure 管理门户网站，选择**联系 Microsoft 技术支持**，选择作为要请求将此区域添加到您的订阅的**支持类型**的**帐单**。 

![与 Microsoft 支持部门联系][screen1]

##存储帐户
 
机器学习服务需要存储的数据的存储帐户。 在南部中央美国的区域中，可以使用现有的存储帐户或创建新的机器学习区 （如果您具有创建新的存储帐户的配额） 时，您可以创建新的存储帐户。 若要查看是否您可以创建新的存储帐户，管理门户中，转到**设置**，然后单击**使用情况**。

![创建工作区][screen2]

创建新的机器学习工作区后，您可以登录到计算机学习 Studio 使用 Microsoft 帐户指定为工作区的所有者。 如果您遇到的错误信息，"区找不到"（类似于下面的屏幕快照），请使用以下步骤删除浏览器 cookie。

![找不到工作区][screen3]

**若要删除浏览器 cookie**

    If you use Internet Explorer, click the **Tools** button in the upper-right corner and select **Internet options**.  

    ![Internet options][screen4]

    Under the **General** tab, click **Delete…**

    ![General tab][screen5]

    In the **Delete Browsing History** dialog box, make sure **Cookies and website data** is selected, and click **Delete**.

    ![Delete cookies][screen6]

删除 cookie 后，请重新启动浏览器，然后转到[Microsoft Azure 机器学习](https://studio.azureml.net)网页。 当系统提示您输入用户名和密码时，请输入相同的 Microsoft 帐户指定为工作区的所有者。

我们的目标是让您的机器学习体验尽可能平稳顺畅。 请在[Azure 机器学习论坛](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning)以帮助我们更好地为您服务，张贴任何评论和问题。 

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
 

测试
