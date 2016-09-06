---
ms.openlocfilehash: 873113fc659367f4f4b156414807f9ba4c5136ab
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Azure RemoteApp 发布应用程序"
    description="了解如何在 Azure RemoteApp 发布应用程序和资源。"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2015"
    ms.author="elizapo" />


# 如何在远程应用程序中发布应用程序

创建远程应用程序集合后，您需要发布的应用程序或您想要为您的用户可用的资源。 与您的订阅中提供的模板图像只能有几个默认的共享其他应用程序发布的应用程序，您需要将其发布。

> [AZURE.NOTE] 您是否需要更新应用程序？ 您将首先需要更新[图像](remoteapp-update.md)。

在门户网站**发布**选项卡上，单击**发布**。 可以从模板图像的**开始**菜单添加一个应用程序，也可以提供应用程序模板图像上的安装位置的路径。 如果您选择从 「 开始 」 菜单中添加，选择列表中发布该应用程序。 如果您选择提供应用程序的路径，输入应用程序的应用程序和路径的名称。 使用路径-例如，"%系统驱动器 %"中的变量，而不是"c:\"。

> [AZURE.NOTE] 如果您想要从**开始**菜单中添加您的应用程序，您需要有*添加到该应用程序**启动**菜单模板图像。* 否则为远程应用程序只能看到*是*什么在**开始**菜单，您将感到困惑。 如果您忘记将 app 添加到**「 开始 」**菜单，创建模板时，选择添加到应用程序的路径。 （或重新创建模板图像，但这是相当多的工作。
 