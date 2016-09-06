---
ms.openlocfilehash: ea20f5022971b40623e5cf5cfe6d37fab1371878
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 的移动服务，html 5 应用程序"
    description="按照本教程中若要开始使用 Azure 的 HTML 开发的移动服务。"
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html5"
    ms.devlang="javascript"
    ms.topic="article" 
    ms.date="07/25/2015"
    ms.author="glenga"/>


# <a name="getting-started"> </a>开始使用移动服务

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

##概述 

本教程展示了如何将基于云的后端服务添加到 HTML 应用程序使用 Azure 移动服务。 在本教程中，您将创建新的移动服务和一个简单*的待办事项列表*的应用程序将应用程序数据存储在新的移动服务。 您可以查看以下本教程的视频版本。 

> [AZURE.VIDEO mobile-get-started-html]
 
下面是从已完成的应用程序的一个屏幕快照︰

![][0]

学完本教程是为 HTML 应用程序的所有其他移动服务教程的先决条件。 PhoneGap/Cordova 的应用程序，请参阅本教程[PhoneGap/Cordova 版本](mobile-services-javascript-backend-phonegap-get-started.md)。

##先决条件

完成本教程需要以下︰

+ 您必须具有本地计算机上运行下列 web 服务器之一︰

    +  **在 Windows 上**︰ IIS Express。 由[Microsoft Web 平台安装程序]安装了 IIS Express。
    +  **在 MacOS X**: Python，应已安装。
    +  **在 Linux 上**︰ Python。 您必须安装[最新版本的 Python]。

    您可以使用任何 web 服务器来承载该应用程序，但它们是 web 服务器所支持的下载脚本。  

+ Web 浏览器支持 HTML5。
+ Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started-html%2F"%20target="_blank)。 


## <a name="create-new-service"> </a>创建新的移动服务

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## 创建新的 HTML 应用程序

一旦您已经创建了您的移动服务，则可以按照在管理门户中创建新的应用程序或修改现有应用程序连接到您的移动服务方便快速入门。

在本节中，您将创建新的 HTML 应用程序连接到您的移动服务。

1.  在管理门户中，单击**移动服务**，然后单击您刚刚创建的移动服务。


2. 在快速启动选项卡，单击**窗口**下**选择平台**和展开**创建新的 HTML 应用程序**。

    ![][6]

    这将显示三个简单的步骤来创建和 HTML 应用程序连接到您的移动服务的主机。

    ![][7]

3. 单击**创建 TodoItems 表**创建一个表来存储应用程序数据。

4. 在**下载并运行您的应用程序**，请单击**下载**。

    这会将下载的示例_的待办事项列表_应用程序连接到您的移动服务的网站文件。 将压缩的文件保存到本地计算机，并记下其保存的位置。

5. 在**配置**选项卡，验证`localhost`还**跨原始资源共享 (CORS)**下的**允许请求的主机名**列表中列出。 如果不是，请键入`localhost`**主机名称**的字段，然后单击**保存**。

    ![][9]

    > [AZURE.IMPORTANT] 快速入门应用程序部署到本地主机以外的 web 服务器时，如果您必须将 web 服务器的主机名添加到**允许请求的主机名的**列表。 有关详细信息，请参阅[跨原始资源共享](http://msdn.microsoft.com/library/windowsazure/dn155871.aspx"%20target="_blank)。

## 承载并运行您的 HTML 应用程序

本教程的最后阶段是承载和在您的本地计算机上运行新应用程序。

1. 浏览到保存压缩的项目文件的位置、 展开您计算机上的文件和启动**服务器**子文件夹中的以下命令文件之一。

    + **启动 windows**（Windows 计算机）
    + **发布 mac.command** （Mac OS X 计算机）
    + **发布 linux.sh** （Linux 计算机）

    > [AZURE.NOTE] 在 Windows 计算机上，键入`R`PowerShell 当要求您确认您想要运行的脚本。 Web 浏览器可能会警告您不运行脚本，因为它从互联网下载。 这种情况下，您必须请求浏览器继续加载该脚本。

    这将启动 web 服务器上本地计算机新的应用程序的宿主。

2. 打开 URL <a href="http://localhost:8000/" target="_blank">http://localhost:8000 /</a> web 浏览器中启动该应用程序。

3. 在应用程序中，在**输入新任务**中，键入有意义的文本，例如，_完成本教程_，，然后单击**添加**。

    ![][10]

    将 POST 请求发送到 Azure 中承载新的移动服务。 从请求的数据插入到 TodoItem 表中。 表中存储的项目由手机信息服务，并将数据显示在应用程序中的第二列中。

    > [AZURE.NOTE] 您可以查看您的移动服务来查询、 插入 app.js 文件中找到的数据访问的代码。

4. 回在管理门户中，单击**数据**选项卡，然后单击**TodoItems**表。

    ![][11]

    这允许您浏览该应用程序插入表的数据。

    ![][12]

## <a name="next-steps"> </a>下一步行动
现在，您已完成快速入门，学会如何在移动服务中执行其他重要的任务︰

* **[有关数据入门]**
  <br/>了解有关存储和查询数据使用移动服务。

* **[从一个 HTML 应用程序调用一个自定义的 API]**
  <br/>将 HTML 应用程序连接与移动服务上承载自定义 API。

* **[开始使用身份验证]**
  <br/>了解如何标识提供程序与应用程序的用户进行身份验证。

* **[移动服务帮助 HTML/JavaScript 的概念引用]**
  <br/>了解更多关于如何使用 HTML/JavaScript 的移动服务

<!-- Anchors. -->
[移动服务入门]:#getting-started
[创建新的移动服务]:#create-new-service
[定义移动服务实例]:#define-mobile-service-instance
[下一步行动]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-html-get-started/mobile-quickstart-completed-html.png

[6]: ./media/mobile-services-html-get-started/mobile-portal-quickstart-html.png
[7]: ./media/mobile-services-html-get-started/mobile-quickstart-steps-html.png

[9]: ./media/mobile-services-html-get-started/mobile-services-set-cors-localhost.png
[10]: ./media/mobile-services-html-get-started/mobile-quickstart-startup-html.png
[11]: ./media/mobile-services-html-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-html-get-started/mobile-data-browse.png


<!-- URLs. -->
[有关数据入门]: mobile-services-html-get-started-data.md
[开始使用身份验证]: mobile-services-html-get-started-users.md
[从一个 HTML 应用程序调用一个自定义的 API]: mobile-services-html-call-custom-api.md

[管理门户]: https://manage.windowsazure.com/
[Microsoft Web 平台安装程序]:  http://go.microsoft.com/fwlink/p/?LinkId=286333
[Python 的最新版本]: http://go.microsoft.com/fwlink/p/?LinkId=286342
[移动服务帮助 HTML/JavaScript 的概念引用]: mobile-services-html-how-to-use-client-library.md
[跨原始资源共享]: http://msdn.microsoft.com/library/azure/dn155871.aspx
 

测试
