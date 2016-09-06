---
ms.openlocfilehash: 18d8744415e82c9906288100bff029e8c1b7e8c7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用移动服务为 Windows 应用商店应用程序 |Microsoft Azure"
    description="按照本教程中使用 Azure 移动服务为 Windows 应用商店开发 C# 或 JavaScript 中的开始。"
    services="mobile-services"
    documentationCenter="windows"
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="glenga"/>

# <a name="getting-started"> </a>开始使用移动服务

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

本教程演示了如何将基于云的后端服务添加到通用 Windows 应用程序使用 Azure 移动服务。

在本教程中，您将创建新的移动服务和一个简单*的待办事项列表*的应用程序将应用程序数据存储在新的移动服务。 手机信息服务，您将创建用于服务器端业务逻辑使用 JavaScript。 若要创建允许您在使用 Visual Studio 中受支持的.NET 语言编写服务器端业务逻辑的移动服务，请参阅本主题的.NET 后端版本。

[AZURE.INCLUDE [mobile-services-windows-universal-get-started](../../includes/mobile-services-windows-universal-get-started.md)]

若要完成本教程，您需要以下各项︰

* 活动的 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdocumentation%2Farticles%2Fmobile-services-javascript-backend-windows-store-javascript-get-started%2F)。
* [Windows 的 Visual Studio 2013年速成版]

## 创建新的移动服务

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## 创建一个新的通用 Windows 应用程序

一旦您已经创建了您的移动服务，则可以按照管理门户创建一个新的通用 Windows 应用程序或修改现有 Windows 应用商店或 Windows Phone 应用程序项目来连接到您的移动服务中易于快速入门。

在本节中，您将创建新的通用 Windows 应用程序连接到您的移动服务。

1.  在管理门户中，单击**移动服务**，然后单击您刚刚创建的移动服务。


2. 在快速启动选项卡，单击**窗口**下**选择平台**和展开**创建新的 Windows 应用商店应用程序**。

    ![](./media/mobile-services-javascript-backend-windows-store-javascript-get-started/mobile-portal-quickstart.png)

    这将显示三个简单的步骤来创建 Windows 应用商店应用程序连接到您的移动服务。

    ![](./media/mobile-services-javascript-backend-windows-store-javascript-get-started/mobile-quickstart-steps.png)

3. 如果还没有这么做，下载并在您的本地计算机或虚拟机上安装[Visual Studio 2013][Visual Studio 2013年速成版的 Windows] 。

4. 单击**创建 TodoItem 表**创建一个表来存储应用程序数据。

5. 在**下载并运行您的应用程序**，为您的应用程序，选择语言，然后单击**下载**。

    这会将下载的示例*的待办事项列表*应用程序连接到您的移动服务的项目。 压缩的项目文件保存到本地计算机，并记下其保存的位置。

## 运行您的 Windows 应用程序

[AZURE.INCLUDE [mobile-services-javascript-backend-run-app](../../includes/mobile-services-javascript-backend-run-app.md)]

>[AZURE.NOTE]您可以查看您的移动服务来查询、 插入 default.js 文件中找到的数据访问的代码。

## 下一步行动
现在，您已完成快速入门，学会如何在移动服务中执行其他重要的任务︰

* [添加到您的应用程序的身份验证][开始使用身份验证]
  <br/>了解如何标识提供程序与应用程序的用户进行身份验证。

* [添加到您的应用程序的推式通知][开始使用推式通知]
  <br/>了解如何将非常基本的推式通知发送到您的应用程序。

有关通用的 Windows 应用程序的详细信息，请参阅[支持多个设备的平台，从单一的移动服务](mobile-services-how-to-use-multiple-clients-single-service.md#shared-vs)。

<!-- Anchors. -->
[移动服务入门]:#getting-started
[创建新的移动服务]:#create-new-service
[定义移动服务实例]:#define-mobile-service-instance
[下一步行动]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[有关数据入门]: ../mobile-services-javascript-backend-windows-universal-javascript-get-started-data.md
[开始使用身份验证]: mobile-services-windows-store-javascript-get-started-users.md
[开始使用推式通知]: mobile-services-javascript-backend-windows-store-javascript-get-started-push.md
[Windows 的 Visual Studio 2013年速成版]: http://go.microsoft.com/fwlink/?LinkId=257546
[移动服务 SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[管理门户]: https://manage.windowsazure.com/

测试
