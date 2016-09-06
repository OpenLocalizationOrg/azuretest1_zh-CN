---
ms.openlocfilehash: 6f231e295236feed4df3583eaf5c13c731966483
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 的 Android 应用程序的移动服务"
    description="按照本教程中若要开始使用 Azure 开发 Android 的移动服务。"
    services="mobile-services"
    documentationCenter="android"
    authors="RickSaling"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="ricksal"/>

# 开始使用移动服务

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

本教程展示如何使用 Azure 移动服务的 Android 应用程序添加一个基于云的后端服务。 在本教程中，您将创建新的移动服务和一个简单**的待办事项列表**的应用程序将应用程序数据存储在新的移动服务。

> [AZURE.VIDEO android-support-in-windows-azure-mobile-services]

下面是从已完成的应用程序的一个屏幕快照︰
![](./media/mobile-services-android-get-started/mobile-quickstart-completed-android.png)

## 先决条件

学完本教程需要[Android 开发人员工具][Android Studio]，其中包括 Android Studio 集成的开发环境，以及最新的 Android 平台。 Android 的 4.2 或更高版本是必需的。

下载快速入门项目包含 Android Azure 移动服务 SDK。

> [AZURE.IMPORTANT] 若要完成本教程，您需要一个 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28)。


## 创建新的移动服务

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## 创建新的 Android 应用程序

一旦您已经创建了您的移动服务，则可以按照在管理门户中创建新的应用程序或修改现有应用程序连接到您的移动服务方便快速入门。

在本节中，您将创建新的 Android 应用程序连接到您的移动服务。

1.  在管理门户中，单击**移动服务**，然后单击您刚刚创建的移动服务。

2. 在快速启动选项卡，单击**选择平台**下的**Android**并展开**创建新的 Android 应用程序**。

    ![](./media/mobile-services-android-get-started/mobile-portal-quickstart-android1.png)

    这将显示三个简单的步骤来创建一个 Android 的应用程序连接到您的移动服务。

    ![](./media/mobile-services-android-get-started/mobile-quickstart-steps-android-AS.png)

3. 如果还没有这么做，下载并在您的本地计算机或虚拟机上安装[Android 开发人员工具][Android SDK] 。

4. 单击**创建 TodoItem 表**创建一个表来存储应用程序数据。


5. 立即下载您的应用程序︰
    - 最新的应用程序版本使用移动服务 Android SDK 2.0。 您可以下载该版本从<a href="https://github.com/RickSaling/mobile-services-samples/tree/futures">在此处</a>。 单击**下载 Zip**，将其解压缩，该项目是在 GettingStarted 文件夹下的 Android。

    - 早期版本使用先前版本的 SDK。 要使用它，在**下载并运行您的应用程序**下，单击**下载**。 这会将下载的示例_的待办事项列表_应用程序连接到您的移动服务的项目。 项目文件被压缩，因此浏览到其位置，展开您的计算机上的文件。


## 运行 Android 的应用程序

[AZURE.INCLUDE [mobile-services-run-your-app](../../includes/mobile-services-android-get-started.md)]

### 查看代码 （可选）

如果您想要查看已完成的应用程序的源代码，请[在此处](https://github.com/RickSaling/mobile-services-samples/tree/androidStudio/GettingStarted/AndroidStudio)。


如果您想要查看本教程的 Eclipse 版本，请转到︰[获得启动 (Eclipse)](mobile-services-android-get-started-ec.md)。

## <a name="next-steps"> </a>下一步行动
现在，您已完成快速入门，学会如何在移动服务中执行其他重要的任务︰

* [有关数据入门]
  <br/>了解有关存储和查询数据使用移动服务。

* [开始使用身份验证]
  <br/>了解如何标识提供程序与应用程序的用户进行身份验证。

* [开始使用推式通知]
  <br/>了解如何将非常基本的推式通知发送到您的应用程序。




<!-- Anchors. -->
[移动服务入门]:#getting-started
[创建新的移动服务]:#create-new-service
[定义移动服务实例]:#define-mobile-service-instance
[下一步行动]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/mobile-services-android-get-started/mobile-portal-quickstart-android.png
[7]: ./media/mobile-services-android-get-started/mobile-quickstart-steps-android-AS.png
[8]: ./media/mobile-services-android-get-started/mobile-eclipse-quickstart.png
[10]: ./media/mobile-services-android-get-started/mobile-quickstart-startup-android.png
[11]: ./media/mobile-services-android-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-android-get-started/mobile-data-browse.png
[14]: ./media/mobile-services-android-get-started/mobile-services-import-android-workspace.png
[15]: ./media/mobile-services-android-get-started/mobile-services-import-android-project.png

<!-- URLs. -->
[获取启动 (Eclipse)]: mobile-services-android-get-started-ec.md
[有关数据入门]: mobile-services-android-get-started-data.md
[开始使用身份验证]: mobile-services-android-get-started-users.md
[开始使用推式通知]: mobile-services-javascript-backend-android-get-started-push.md
[Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=280125
[Android 的 Studio]: https://developer.android.com/sdk/index.html
[移动服务 Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533

[管理门户]: https://manage.windowsazure.com/

测试
