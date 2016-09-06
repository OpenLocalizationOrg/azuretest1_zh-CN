---
ms.openlocfilehash: 04c6ee11df6f1df77542a68b58ba1d2d0b7b90eb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure RemoteApp 授权" 
    description="学习许可的方式在 Azure 远程应用程序。" 
    services="remoteapp" 
    solutions="" documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/12/2015" 
    ms.author="elizapo" />


# 如何授权工作在 Azure 远程应用程序？


因此您设置了您的 Azure 远程应用程序服务，创建自己的模板，并准备将应用程序发布到您的用户。 但还有最后一点想出︰ 授权。 只是如何执行远程应用程序和通过远程应用程序共享的应用程序的授权工作？

远程应用程序不需要任何 Windows 许可证或远程桌面 Cal。 您的订购负责本身的 RemoteApp 侧。 （签出的[定价计划](../../../pricing/details/remoteapp/)的详细信息）。 

如果您使用一个包含在您的订阅中的图像，您可以共享任何安装该映像中，而不需要单独的许可证的应用程序。 例如，如果您使用 Windows Server 2012 R2 模板图像来建立您的集合，可以与您的用户共享系统中心端点保护。 此规则的唯一例外是 Office 365 ProPlus，这就需要单独的订阅，并且 Office 2013，生产集合中不能共享。

如果您想要使用 Office 365 模板图像，配有远程应用程序，您必须将*现有*的 Office 365 ProPlus 计划。 同样适用于您使用的自定义模板发布任何 Office 365 提供应用程序。 您需要激活自己订阅的应用程序。 这是试用版和付费订阅，则返回 true。 如果您想要试用，*并还没有预订*，期间使用 Office 365 模板图像转到 Office 365 页到[注册](https://go.microsoft.com/fwlink/p/?LinkID=403802)试用订阅。 请参阅[如何 RemoteApp 和办公室工作在一起？](remoteapp-o365.md)的详细信息。

如果在试用期内，您不想获取 Office 365 试用订阅，请使用 Office 2013 专业加上模板图像，配有远程应用程序。 此模板图像仅用于 30 天，而且不能转换为付费的集合。 

对于其他应用程序，您需要确保您具有要共享应用程序的许可证。

这是可以理解，正确吗？ 您可以发布任何不合法权共享的应用程序。 然后，您需要确保您确实有权共享您的程序。

请注意，您不能使用 CAL 或批量许可协议，云集合中。 您*可以*使用批量许可证协议激活应用程序 （除了办公室） 混合集合中。 您只需安装这些模板图像的批量许可证媒体上。 按照应用程序软件供应商可以在远程桌面环境中安装许可证信息。
 