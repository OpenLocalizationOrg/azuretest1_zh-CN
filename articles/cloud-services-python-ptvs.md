---
ms.openlocfilehash: 20d192d0da419ac14d5905b1e0750d337dbb1b84
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="使用 Visual Studio 的 Python 工具 2.2 Python 站点和辅助角色 |Microsoft Azure"
    description="使用 Visual Studio 的 Python 工具创建 Azure 的云服务，其中包括 web 角色和辅助角色的概述。"
    services=""
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="08/30/2015"
    ms.author="huvalo"/>




# 使用 Visual Studio 的 Python 工具 2.2 Python 站点和辅助角色

这篇文章概述了使用 Python 站点和辅助角色使用[Visual Studio 的 Python 工具][]。

## 先决条件

 - Visual Studio 2013年或 2015
 - [Python 工具 Visual Studio 的 2.2][](PTVS)
 - [VS 2013 的 azure SDK 工具][]或[VS 2015 的 Azure SDK 工具][]
 - [Python 2.7 32 位][]或者[Python 3.4 32 位][]

[AZURE.INCLUDE [create-account-and-websites-note](../includes/create-account-and-websites-note.md)]

## Python 的 web 和工作人员的角色是什么？

Azure 提供了三个计算模型，用于运行应用程序︰[在 Azure 应用程序服务 Web 应用程序功能]的[执行模型 web 站点]、 [Azure 虚拟机][执行虚拟机模型]和[Azure 云服务][执行模型云服务]。 所有三个型号都支持 Python。 云服务，包括网站和辅助的角色，提供*平台即服务 (PaaS)*。 在云服务中，web 角色提供专用的 Internet Information Services (IIS) web 服务器主机前端 web 应用程序，而工作者角色可以运行异步、 长期或永久任务独立于用户交互或输入。

有关详细信息，请参阅[云服务是什么？]。

> [AZURE.NOTE] *希望构建一个简单的网站吗？*
如果您的方案涉及的只是简单网站前端，请考虑使用 Azure 应用程序服务中的轻量的 Web 应用程序功能。 您可以轻松地升级到云服务，为您的网站的发展和需求的变化。 请参阅<a href="/develop/python/">Python 开发中心</a>的文章涵盖了 Azure 应用程序服务中的 Web 应用程序功能的开发。
<br />


## 创建项目

在 Visual Studio 中，您可以选择在**新建项目**对话框中的**Python**， **Azure 云服务**。

![新建项目对话框](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

在 Azure 云服务向导中，您可以创建新的 web 和辅助角色。

![Azure 的云服务对话框](./media/cloud-services-python-ptvs/new-service-wizard.png)

辅助角色模板附带样板化的代码连接到 Azure 存储帐户或 Azure 服务总线。

![云服务解决方案](./media/cloud-services-python-ptvs/worker.png)

可以在任何时候添加到现有的云服务的 web 或辅助角色。  您可以选择在您的解决方案中添加现有项目或创建新模板。

![添加角色命令](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

云服务可以包含用不同的语言实现的角色。  例如，您可以使用 Python，或 C# 辅助角色使用 Django，实现了 Python web 角色。  您可以轻松地使用服务总线队列存储您角色之间进行通信。

## 在本地运行

如果将云服务项目设置为启动项目并按 f5 键，云服务将运行在本地 Azure 仿真程序。

虽然 PTVS 支持在仿真器中，调试 （例如，断点） 启动不起作用。

若要调试 web 和辅助角色，可以角色项目设置为启动项目并调试，相反。  您还可以设置多个启动项目。  用鼠标右键单击该解决方案，然后选择**设置启动项目**。

![解决方案启动项目属性](./media/cloud-services-python-ptvs/startup.png)

## 发布到 Azure

若要发布，请右击解决方案中的云服务项目，然后选择**发布**。

![Microsoft Azure 发布登录](./media/cloud-services-python-ptvs/publish-sign-in.png)

在设置页上选择想要发布到的云服务。

![Microsoft Azure 发布设置](./media/cloud-services-python-ptvs/publish-settings.png)

您可以创建新的云服务如果您还没有提供。

![创建云服务对话框](./media/cloud-services-python-ptvs/publish-create-cloud-service.png)

也是用于启用调试故障的计算机到远程桌面连接。

![远程桌面配置对话框](./media/cloud-services-python-ptvs/publish-remote-desktop-configuration.png)

配置设置完成后，单击**发布**。

一些进度将显示在输出窗口中，然后您将看到 Microsoft Azure 活动日志窗口。

![Microsoft Azure 活动日志窗口](./media/cloud-services-python-ptvs/publish-activity-log.png)

部署将需要几分钟才能完成，然后将对 Azure 上运行 web 和/或辅助角色 ！

## 下一步行动

有关使用 Visual Studio 的 Python 工具中的 web 和辅助角色的详细信息，请参阅 PTVS 文档︰

- [云服务项目][]

有关使用 web 和工作人员角色，例如使用 Azure 存储或服务总线从 Azure 服务的更多详细信息请参阅以下文章。

- [Blob 服务][]
- [表服务][]
- [队列的服务][]
- [服务总线队列][]
- [服务总线主题][]


<!--Link references-->

[云服务是什么？]: /manage/services/cloud-services/what-is-a-cloud-service/
[执行模型 web 站点]: fundamentals-application-models.md#WebSites
[执行虚拟机模型]: fundamentals-application-models.md#VMachine
[执行模型云服务]: fundamentals-application-models.md#CloudServices
[Python 开发人员中心]: /develop/python/

[Blob 服务]: storage-python-how-to-use-blob-storage.md
[队列的服务]: storage-python-how-to-use-queue-storage.md
[表服务]: storage-python-how-to-use-table-storage.md
[服务总线队列]: service-bus-python-how-to-use-queues.md
[服务总线主题]: service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Visual Studio 的 Python 工具]: http://aka.ms/ptvs
[Python 工具 Visual Studio 文档]: http://aka.ms/ptvsdocs
[云服务项目]: http://go.microsoft.com/fwlink/?LinkId=624028
[Python 的 Visual Studio 工具 2.2]: http://go.microsoft.com/fwlink/?LinkID=624025
[VS 2013 的 azure SDK 工具]: http://go.microsoft.com/fwlink/?LinkId=323510
[VS 2015 的 azure SDK 工具]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32 位]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python 3.4 32 位]: http://go.microsoft.com/fwlink/?LinkId=517191

测试
